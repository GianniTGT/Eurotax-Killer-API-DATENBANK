# Eurotax-Killer – API & Fahrzeugdatenbank Schweiz

Eigene Schweizer Fahrzeugdaten- und Bewertungsdatenbank, aufgebaut über das Händler-DMS **Tiff Cardealer** (TIFF Software Solutions).

**Kernidee:** Das DMS ist nicht das Ziel, es ist die Datenpumpe. Eurotax verkauft Daten, hat aber keinen direkten Draht zum Händler-Arbeitsplatz. Tiff Cardealer fängt genau dort an.

## Dokumentation

| Datei | Inhalt |
|---|---|
| [docs/01-ausgangslage-kosten.md](docs/01-ausgangslage-kosten.md) | AutoScout24-Konditionen, Eurotax, wer was bezahlt |
| [docs/02-datenquellen.md](docs/02-datenquellen.md) | Wie Eurotax zu seinen Daten kommt, VIN, ASTRA, Ausstattung |
| [docs/03-stufenplan.md](docs/03-stufenplan.md) | Vorgehen in 5 Stufen, vom DMS zum Datenprodukt |
| [docs/04-datenmodell.md](docs/04-datenmodell.md) | Erfassung im DMS, Ausstattungskatalog, Start-Schema Daten-Pool |
| [docs/05-recht-datenschutz.md](docs/05-recht-datenschutz.md) | AGB-Klausel, Texte für Homepage und App, Schutzregeln |
| [docs/06-datenpipeline.md](docs/06-datenpipeline.md) | Übernahme aus Tiff Cardealer, Speicherung, Verarbeitung |
| [docs/07-naechste-schritte.md](docs/07-naechste-schritte.md) | Konkrete To-dos |

## Kurzfassung

1. **Grundlage gratis:** Technische Fahrzeugdaten aus den ASTRA-Typengenehmigungsdaten (Open Data, Format TAS, JSON).
2. **Eigene Daten sammeln:** Jeder Händler erfasst im DMS Fahrzeug, Ausstattung (feste Liste), Angebotspreis und echten Verkaufspreis.
3. **Rechtlich absichern:** AGB-Klausel zur anonymisierten, aggregierten Datennutzung ab Tag 1.
4. **Masse aufbauen:** Erst Händlerbasis, dann Datenprodukt.
5. **Verwerten:** Feature im DMS → Bewertungs-API für Dritte → evtl. Verkauf des Datenbestands.

## Status

Planungsphase – Stand 15.10.2026.
