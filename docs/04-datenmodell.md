# 04 – Datenmodell und Erfassung in Tiff Cardealer

## Grundprinzip: zwei getrennte Welten

| | **DMS-Datenbank** (pro Händler) | **Daten-Pool** (Eurotax-Killer) |
|---|---|---|
| Zweck | Tagesgeschäft des Händlers | Marktstatistik, Bewertung |
| Inhalt | alles: Kunden, Leads, Rechnungen, Fahrzeuge | nur Fahrzeug- und Transaktionsdaten, pseudonymisiert |
| Personendaten | ja | **nein, nie** |
| Zugriff | Händler + Support | nur Auswertungsjobs, keine Händler |

Der Pool bekommt Daten **nur über die Pipeline** (siehe [06-datenpipeline.md](06-datenpipeline.md)), nie per direktem Zugriff auf die DMS-Tabellen.

## Erfassungsablauf im DMS (Beispiel VW Golf)

1. Händler gibt **Typengenehmigungsnummer** ein (oder scannt den Fahrzeugausweis) + **VIN**.
2. System füllt automatisch aus ASTRA: Marke, Modell, Motor, Leistung, Hubraum, Treibstoff, Getriebe, Gewichte.
3. System dekodiert VIN: Hersteller, Werk, Modelljahr.
4. Händler ergänzt: Erstzulassung, Kilometer, Farbe, Zustand.
5. Händler hakt **Ausstattung aus fester Liste** an (später: System schlägt typische Ausstattung vor).
6. Angebotspreis, Einkaufspreis (optional), Inseratedatum.
7. Beim Verkauf: **Verkaufspreis**, Verkaufsdatum, Inzahlungnahme ja/nein → wird automatisch gespeichert.

**Pflichtfelder für den Pool:** Typengenehmigungsnummer, Erstzulassung, Kilometer, Angebotspreis, Verkaufspreis, Verkaufsdatum. Ohne diese Felder geht ein Datensatz nicht in den Pool.

## Feste Ausstattungsliste (Katalog)

- Eigene Tabelle mit Codes, z. B. `NAV` = Navigationssystem, `AHK` = Anhängerkupplung, `PANO` = Panoramadach, `LED` = LED-Scheinwerfer, `ACC` = Abstandstempomat.
- Mehrsprachige Bezeichnung (DE/FR/IT/EN), Code bleibt gleich.
- Neue Codes nur durch TIFF, nicht durch Händler. Freitextfeld darf existieren, wird aber **nicht** ausgewertet.
- Start mit ca. 80–120 Positionen, gruppiert (Komfort, Sicherheit, Multimedia, Exterieur, Antrieb).

## Start-Schema Daten-Pool (PostgreSQL)

```sql
-- Stammdaten aus ASTRA (TAS), monatlich importiert
CREATE TABLE astra_typ (
    tg_nummer        TEXT PRIMARY KEY,   -- Typengenehmigungsnummer
    marke            TEXT,
    modell           TEXT,
    variante         TEXT,
    treibstoff       TEXT,
    leistung_kw      INTEGER,
    hubraum_ccm      INTEGER,
    getriebe         TEXT,
    leergewicht_kg   INTEGER,
    rohdaten         JSONB,               -- Original-JSON für spätere Felder
    importiert_am    DATE NOT NULL
);

-- Ausstattungskatalog
CREATE TABLE ausstattung_katalog (
    code             TEXT PRIMARY KEY,   -- z.B. 'NAV'
    gruppe           TEXT NOT NULL,
    bez_de           TEXT NOT NULL,
    bez_fr           TEXT,
    bez_it           TEXT,
    bez_en           TEXT
);

-- Pseudonymisierter Händler (nur Region + Typ, kein Name)
CREATE TABLE pool_haendler (
    haendler_hash    TEXT PRIMARY KEY,   -- Hash der Händler-ID mit geheimem Salt
    kanton           CHAR(2),
    plz_region       CHAR(2),            -- nur erste 2 Ziffern der PLZ
    haendler_typ     TEXT                -- Marken-Garage / freier Händler
);

-- Ein Fahrzeug-Lebenslauf im Pool
CREATE TABLE pool_fahrzeug (
    id               BIGSERIAL PRIMARY KEY,
    vin_hash         TEXT NOT NULL,       -- VIN nie im Klartext
    tg_nummer        TEXT REFERENCES astra_typ(tg_nummer),
    modelljahr       SMALLINT,
    erstzulassung_jm CHAR(7),            -- 'YYYY-MM', kein Tag
    farbe_gruppe     TEXT,
    UNIQUE (vin_hash)
);

CREATE TABLE pool_fahrzeug_ausstattung (
    fahrzeug_id      BIGINT REFERENCES pool_fahrzeug(id),
    code             TEXT REFERENCES ausstattung_katalog(code),
    PRIMARY KEY (fahrzeug_id, code)
);

-- Jedes Angebot / jeder Verkauf ist eine Transaktion
CREATE TABLE pool_transaktion (
    id               BIGSERIAL PRIMARY KEY,
    fahrzeug_id      BIGINT REFERENCES pool_fahrzeug(id),
    haendler_hash    TEXT REFERENCES pool_haendler(haendler_hash),
    km_stand         INTEGER NOT NULL,
    angebotspreis    NUMERIC(10,0),
    verkaufspreis    NUMERIC(10,0),
    inseriert_am     DATE,
    verkauft_monat   CHAR(7),            -- 'YYYY-MM'
    standzeit_tage   INTEGER,
    inzahlungnahme   BOOLEAN,
    quelle_version   TEXT,               -- DMS-Version für Nachvollziehbarkeit
    geladen_am       TIMESTAMPTZ DEFAULT now()
);
```

Bewusst **nicht** im Pool: Kundennamen, Adressen, Telefon, E-Mail, Leads, Rechnungsnummern, Kontrollschild, genaue Verkaufstage, Händlername.

## Kennzahlen, die daraus entstehen

- Median-Verkaufspreis pro Typengenehmigung × Alter × Kilometerband × Region
- Differenz Angebotspreis ↔ Verkaufspreis (Verhandlungsspielraum)
- Standzeit pro Modell und Preisklasse
- Ausstattungsquote pro Variante (z. B. 78 % der Golf 1.5 TSI haben Navi)
- Preiseinfluss einzelner Ausstattungen
