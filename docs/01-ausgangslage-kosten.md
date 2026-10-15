# 01 – Ausgangslage und Kosten

## AutoScout24 – Angebot an TIFF (Mail Support, 14.10.2026)

| Schnittstelle | Zweck | Kosten |
|---|---|---|
| **DMS-API** | Fahrzeuge aus dem DMS automatisch als Inserat auf AutoScout24 | CHF 1'200 / Jahr pro Händler-Account |
| | Deckel ab 5 Händlern | max. CHF 6'000 / Jahr total |
| **Lead-API** | Kundenanfragen von AutoScout24 automatisch ins DMS | CHF 480 / Jahr (offen: pro Account oder pauschal?) |

Testzugang wurde angeboten.

### Rechenbeispiel DMS-API

| Anzahl Händler | Total / Jahr | Pro Händler |
|---|---|---|
| 1 | 1'200 | 1'200 |
| 3 | 3'600 | 1'200 |
| 5 | 6'000 | 1'200 |
| 10 | 6'000 | 600 |
| 20 | 6'000 | 300 |

Ab Händler Nr. 6 kostet jeder weitere nichts mehr.

### Zum Vergleich: Händler-Inserate direkt bei AutoScout24

- Händler zahlen monatliche Abos (Basic / Professional / Professional Plus) plus Inserateplätze.
- Beispiel: Basic mit 10 Plätzen ca. CHF 359 / Monat Listenpreis, multipliziert mit dem «Car Value Factor» (0.8–1.2).
- Bis 25 Fahrzeuge Richtung CHF 560 / Monat.

## Wer bezahlt was?

**Grundsatz: TIFF schluckt keine Marktplatzkosten.**

- **Modell A (empfohlen für Start):** Händler hat eigenen AutoScout24-Vertrag. Tiff Cardealer schiebt nur die Daten über die API. TIFF ist nicht im Geldfluss der Inserate.
- **Modell B:** TIFF bündelt und verrechnet die Kosten weiter (Software-Abo + Durchlaufposten). Nur sinnvoll mit besseren Reseller-Konditionen.
- Die API-Gebühren (DMS-API, Lead-API) werden im Abopreis von Tiff Cardealer eingerechnet.

## Eurotax / JD Power (Autovista)

- Eurotax Schweiz läuft heute unter **JD Power / Autovista** (zusammen mit Glass's und Schwacke).
- Angebot: VIN-/Zulassungscode-Abfrage (99 % der Fahrzeuge), AutovistaSPEC (technische Daten), Bewertung, Restwerte.
- Eigenes **DMS Integration Portal** → Softwareanbieter wie TIFF sind Zielkunden.
- **Preise nicht öffentlich**, nur auf Anfrage. Testzugang möglich.
- Erwartung: Enterprise-Vertrag, eher fünfstellig pro Jahr.

### Strategischer Mittelweg

- Stammdaten (Technik): ASTRA gratis, später evtl. Eurotax.
- AutoScout24 nur für das, was nur sie haben: Reichweite, Inserat, Leads.
- Bewertung/Preise: langfristig eigene Daten aus dem DMS.
