# 02 – Datenquellen: Wie Eurotax zu seinen Daten kommt

Eurotax kauft keine Autos. Die Daten kommen von Herstellern, Importeuren und Händlern.

## Beispiel: neuer VW Golf in der Schweiz

1. **Preisliste vom Importeur.** AMAG (Importeur VW) schickt vor Markteinführung alle Varianten, Ausstattungslinien, Listenpreise und Optionspreise. Importeure machen das freiwillig, weil Leasing und Versicherung mit Eurotax arbeiten.
2. **Erfassung durch Spezialisten.** Jede Variante bekommt einen Eurotax-Code, verknüpft mit der Schweizer **Typengenehmigungsnummer** (steht im Fahrzeugausweis).
3. **VIN-Daten vom Hersteller.** Über Datenverträge mit Herstellern bekommt Eurotax pro VIN die ab Werk verbaute Ausstattung.
4. **Gebrauchtpreise.** Händler melden aus ihrem DMS echte Verkaufspreise. Dazu werden Inserate (z. B. AutoScout24) ausgewertet → Marktwert.

## Was liefert welche Quelle?

| Quelle | Liefert | Liefert NICHT | Zugang für TIFF |
|---|---|---|---|
| **VIN** (17 Stellen) | Hersteller, Modell, Werk, Modelljahr (10. Stelle), Seriennummer | Ausstattung, Preise | gratis (Dekodierung nach Norm) |
| **ASTRA Typengenehmigung** | Motor, Leistung, Hubraum, Treibstoff, Getriebe, Gewichte, Masse | Optionen (Navi, Panoramadach …), Preise | gratis, Open Data |
| **ASTRA eDatenblatt (eCoC)** | genauere technische Daten pro Fahrzeug (CoC) | Preise | gratis, Open Data |
| **Hersteller-Systeme** | Ausstattung pro VIN | – | nur mit Vertrag, faktisch nicht für Start |
| **Importeur-Preislisten** | Neupreise, Optionspreise | – | nur für etablierte Anbieter |
| **Händler-DMS (Tiff Cardealer)** | Ausstattung, Angebotspreis, **echter Verkaufspreis**, Standzeit, Region | – | **eigener Vorteil** |

## ASTRA Open Data – Details (Stand Okt. 2026)

- Portal: https://opendata.astra.admin.ch/ivzod/
- **Neues Format TAS verwenden:** Ordner `4000-Typengenehmigungen_TAS`, JSON/Rohdaten, monatlich.
- **Altes Format TARGA nicht mehr verwenden:** letzte Aktualisierung Ende Juli 2026, Entfernung Ende 2026 (Ordner 2200 und 2500).
- Zusätzlich: `3000-eDatenblatt` (CoC-Daten, monatlich).
- Weitere nützliche Datensätze: Neuzulassungen (monatlich), Fahrzeugbestände, Gebrauchtimporte, Halterwechsel.
- Keine Personendaten.
- Kundenspezifische Datensätze möglich, kostenpflichtig nach Aufwand (GebV-ASTRA).
- Newsletter Fahrzeugdaten abonnieren, um Formatänderungen mitzubekommen.

Quellen:
- https://www.astra.admin.ch/de/fahrzeugdaten
- https://www.astra.admin.ch/dam/de/sd-web/YZ5OtKJr5yXO/Newsletter_Fahrzeugdaten_2_2026_DE.pdf
- https://www.astra.admin.ch/dam/en/sd-web/BakiDqfaJMx8/01%20Overview%20vehicle%20data%20information%20products-20260701.pdf

## Folgerung

- Technische Grunddaten: **ASTRA** (gratis).
- Ausstattung: **selbst aufbauen** über einheitliche Erfassung im DMS + Lernen pro Variante.
- Preise / Marktwert: **selbst aufbauen** über echte Verkaufspreise aus dem DMS.
