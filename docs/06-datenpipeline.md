# 06 – Datenpipeline: Übernahme, Speicherung, Verarbeitung

## Überblick

```
 Tiff Cardealer (DMS, pro Händler)
        │  1. Ereignis: Fahrzeug erfasst / geändert / verkauft
        ▼
 Export-Warteschlange (im DMS)       ← nur erlaubte Felder, Personendaten raus
        │  2. Nächtlicher Job, HTTPS, signiert
        ▼
 Eingangszone (Staging)              ← Rohdaten, 30 Tage aufbewahrt
        │  3. Prüfen, bereinigen, pseudonymisieren
        ▼
 Daten-Pool (PostgreSQL)             ← pool_fahrzeug, pool_transaktion …
        │  4. Auswertungsjobs (wöchentlich)
        ▼
 Kennzahlen-Tabellen (aggregiert)    ← nur Werte, die Schutzregeln erfüllen
        │  5. Bewertungs-API
        ▼
 DMS-Feature «Marktpreis»  ·  später: externe API-Kunden
```

Parallel: **ASTRA-Import** monatlich → `astra_typ`.

## 1. Erfassen im DMS

- Beim Speichern eines Fahrzeugs und beim Verkaufsabschluss schreibt das DMS einen Eintrag in eine Export-Tabelle (`export_queue`).
- Nur Felder aus einer **Positivliste** (Whitelist) werden übernommen. Neue Felder müssen bewusst freigeschaltet werden.
- Personendaten (Käufer, Lead, Mitarbeiter) sind technisch gar nicht in der Positivliste.
- Händler, die per Vertrag ausgeschlossen sind (z. B. Testkunden), werden markiert und nicht exportiert.

## 2. Übertragen

- Nächtlicher Job pro DMS-Instanz sendet neue Einträge per HTTPS an die Pool-Eingangs-API.
- Authentifizierung mit API-Key pro Instanz, Payload signiert.
- Idempotent: jeder Eintrag hat eine eindeutige ID, doppelte Lieferungen werden ignoriert.
- Bei Fehler: Eintrag bleibt in der Queue, nächster Lauf versucht es erneut.

Beispiel-Payload:

```json
{
  "event_id": "c7b1…",
  "event": "verkauft",
  "haendler_id": "H-0042",
  "vin": "WVWZZZCDZRW000000",
  "tg_nummer": "1VB123",
  "erstzulassung": "2021-03",
  "km_stand": 48200,
  "ausstattung": ["NAV", "AHK", "LED"],
  "angebotspreis": 21900,
  "verkaufspreis": 20800,
  "inseriert_am": "2026-08-14",
  "verkauft_am": "2026-10-02",
  "inzahlungnahme": true,
  "dms_version": "1.4.0"
}
```

## 3. Bereinigen und pseudonymisieren (Eingangszone → Pool)

| Schritt | Was passiert |
|---|---|
| Plausibilität | Preis > 0, km plausibel, Verkaufspreis nicht > 2× Angebotspreis, Erstzulassung nicht in der Zukunft |
| ASTRA-Abgleich | TG-Nummer muss in `astra_typ` existieren, sonst Status «unvollständig» |
| VIN | Prüfziffer/Format prüfen, dann `SHA-256(VIN + geheimer Salt)` → `vin_hash`, Klartext verworfen |
| Händler | `haendler_id` → `haendler_hash`, nur Kanton und PLZ-Region bleiben |
| Datum | Verkaufsdatum auf Monat gekürzt, Standzeit vorher berechnet |
| Ausstattung | nur Codes aus `ausstattung_katalog`, Rest verworfen |
| Ausreisser | stark abweichende Preise markieren, nicht löschen (später manuell prüfen) |

Der Salt liegt **nur** im Pool-Server (Secret), nie im DMS und nie im Git-Repo.

## 4. Auswerten

- Wöchentlicher Job berechnet Kennzahlen (Median, Quantile, Standzeit, Ausstattungsquoten).
- Gruppen: TG-Nummer → Modell → Marke; Kanton → Grossregion → Schweiz; Kilometer- und Altersbänder.
- **Schutzregeln** aus [05-recht-datenschutz.md](05-recht-datenschutz.md) werden hier erzwungen. Nur Gruppen, die sie erfüllen, landen in den Kennzahlen-Tabellen.
- Die Bewertungs-API liest **nur** Kennzahlen-Tabellen, nie die Pool-Rohdaten.

## Wo speichern

| Komponente | Empfehlung |
|---|---|
| Server-Standort | **Schweiz** (Verkaufsargument + Datenschutz) – z. B. Schweizer Hoster mit VPS/Managed DB |
| Datenbank | PostgreSQL (JSONB für ASTRA-Rohdaten, gute Auswertungsfunktionen) |
| Trennung | Pool-DB getrennt von allen DMS-Datenbanken, eigener Server oder mindestens eigene DB + eigener User |
| Backups | täglich, verschlüsselt, ebenfalls in der Schweiz, 30 Tage |
| Zugriff | nur Pipeline-Jobs + Admin (2FA), Zugriffe protokolliert |
| Secrets | Salt, API-Keys in Umgebungsvariablen / Secret-Store, nie im Repo |

## Aufbewahrung

- Eingangszone (Rohdaten mit VIN-Klartext): **30 Tage**, dann automatisch löschen.
- Pool (pseudonymisiert): unbefristet.
- Kennzahlen: Versionen pro Monat behalten (Preisentwicklung über Zeit = späteres Produkt).

## Geplante Repo-Struktur

```
/docs                Planung (diese Dateien)
/schema              SQL-Migrationen Daten-Pool
/import/astra        Monatlicher TAS-Import (JSON → astra_typ)
/ingest              Eingangs-API + Bereinigung/Pseudonymisierung
/jobs                Auswertungsjobs (Kennzahlen)
/api                 Bewertungs-API (liest nur Kennzahlen)
/katalog             Ausstattungskatalog (CSV, mehrsprachig)
```
