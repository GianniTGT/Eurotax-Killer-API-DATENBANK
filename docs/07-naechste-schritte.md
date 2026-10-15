# 07 – Nächste Schritte

## Sofort (Oktober / November 2026)

- [ ] AutoScout24: Testzugang aktivieren, offene Frage klären → Lead-API CHF 480 pro Account oder pauschal?
- [ ] ASTRA: TAS-Datensatz herunterladen und Struktur prüfen (`4000-Typengenehmigungen_TAS`)
- [ ] ASTRA: Newsletter Fahrzeugdaten abonnieren
- [ ] ASTRA: schriftlich anfragen, ob kommerzielle Nutzung erlaubt ist und wie die Quellenangabe lauten muss
- [ ] Eurotax / JD Power Schweiz: Angebot für VIN-API + AutovistaSPEC anfragen (als DMS-Anbieter, Mengenkonditionen) → nur zum Vergleich

## Im DMS (Tiff Cardealer)

- [ ] Feld Typengenehmigungsnummer + automatisches Füllen aus ASTRA
- [ ] VIN-Prüfung und -Dekodierung
- [ ] Ausstattungskatalog (erste ~100 Codes) statt Freitext
- [ ] Pflichtfelder beim Verkaufsabschluss: Verkaufspreis, Datum, Inzahlungnahme
- [ ] `export_queue` mit Positivliste
- [ ] Onboarding-Checkbox + Infotexte (siehe 05)

## Rechtliches

- [ ] AGB-Klausel und Datenschutzerklärung von Fachperson prüfen lassen
- [ ] Auftragsbearbeitungsvertrag (AVV) für Kundendaten im DMS
- [ ] Seite «Datenschutz & Marktdaten» auf der Homepage

## Daten-Pool (dieses Repo)

- [ ] `/schema`: Start-Schema aus 04 als Migration
- [ ] `/import/astra`: TAS-Import-Script
- [ ] `/ingest`: Eingangs-API mit Pseudonymisierung
- [ ] Server in der Schweiz aufsetzen (PostgreSQL, Backups, Secrets)

## Später

- [ ] Auswertungsjobs + Schutzregeln
- [ ] DMS-Feature «Marktpreis» (ab genug Volumen)
- [ ] Bewertungs-API für Dritte
