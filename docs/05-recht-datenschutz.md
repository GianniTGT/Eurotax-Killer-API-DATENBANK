# 05 – Recht, Datenschutz und Kommunikation (Homepage / App)

> Hinweis: Das ist ein Arbeitsentwurf, keine Rechtsberatung. Vor Livegang AGB und Datenschutzerklärung von einer Fachperson (Anwalt / Datenschutzberatung Schweiz) prüfen lassen.

## Rechtlicher Rahmen

- Schweizer Datenschutzgesetz (**nDSG**, in Kraft seit 1.9.2023).
- **Anonymisierte** Daten sind keine Personendaten → nDSG gilt dafür nicht mehr. Darum: im Pool konsequent anonymisieren bzw. pseudonymisieren.
- Verkaufsdaten des Händlers sind **Geschäftsgeheimnisse** des Händlers → Nutzung muss vertraglich (AGB) geregelt sein.
- VIN + Datum kann über Umwege auf einen Halter führen → VIN nur gehasht, Datum nur auf Monat.
- Wenn TIFF Kundendaten des Händlers verarbeitet, ist TIFF **Auftragsbearbeiter** → separater Auftragsbearbeitungsvertrag (AVV). Diese Daten gehen nie in den Pool.

## AGB-Klausel (Entwurf)

> **§ X Nutzung von Markt- und Fahrzeugdaten**
>
> 1. Der Kunde räumt TIFF Software Solutions das zeitlich unbefristete, nicht ausschliessliche Recht ein, Fahrzeug- und Transaktionsdaten, die bei der Nutzung von Tiff Cardealer entstehen (insbesondere technische Fahrzeugdaten, Ausstattung, Kilometerstand, Angebots- und Verkaufspreise, Standzeiten), in anonymisierter und aggregierter Form zu speichern, auszuwerten und für Marktanalysen, Bewertungsfunktionen und Datenprodukte zu nutzen und Dritten zur Verfügung zu stellen.
> 2. Personendaten (insbesondere Daten von Käufern, Interessenten und Mitarbeitenden) sind von dieser Nutzung ausgeschlossen.
> 3. TIFF stellt sicher, dass aus veröffentlichten oder an Dritte weitergegebenen Auswertungen weder der Kunde noch einzelne Geschäftsfälle des Kunden erkennbar sind.
> 4. Dieses Nutzungsrecht bleibt für bereits anonymisierte Daten auch nach Vertragsende bestehen.

## Text für die Homepage (Seite «Datenschutz & Marktdaten»)

> **Wie wir Marktdaten nutzen**
>
> Tiff Cardealer hilft Garagen, den Wert ihrer Fahrzeuge besser einzuschätzen. Dafür werten wir Fahrzeug- und Verkaufsdaten aus allen angeschlossenen Betrieben aus – **anonymisiert und zusammengefasst**.
>
> **Was wir nutzen:** Fahrzeugtyp, Ausstattung, Jahrgang, Kilometer, Angebots- und Verkaufspreis, Standzeit, Region (Kanton).
>
> **Was wir nie nutzen:** Namen, Adressen oder Kontaktdaten Ihrer Kundinnen und Kunden, Ihre Firmenbezeichnung, Kontrollschilder, Rechnungen.
>
> **Ihr Vorteil:** Sie sehen, zu welchem Preis vergleichbare Fahrzeuge in Ihrer Region tatsächlich verkauft wurden – nicht nur, was in Inseraten steht.
>
> **Ihr Schutz:** Auswertungen erscheinen erst, wenn genügend Fahrzeuge von mehreren Betrieben vorliegen. Kein Betrieb und kein einzelner Verkauf ist erkennbar. Alle Daten werden in der Schweiz gespeichert.

## Text in der App

**Beim Onboarding (Pflicht-Checkbox, mit Link zu AGB):**

> ☐ Ich habe die AGB gelesen und bin einverstanden, dass Fahrzeug- und Verkaufsdaten anonymisiert für Marktauswertungen genutzt werden. Kundendaten werden nie ausgewertet. [Mehr erfahren]

**Infobox bei der Preisbewertung im DMS:**

> ℹ️ Diese Werte stammen aus anonymisierten Verkäufen von Tiff-Garagen in Ihrer Region. Ihre eigenen Verkäufe fliessen ebenfalls anonym ein.

**In den Einstellungen (Transparenz):**

> Marktdaten: aktiv · Beigetragene Fahrzeuge: 142 · [Was wird geteilt?]

## Schutzregeln für Auswertungen (technisch umgesetzt)

| Regel | Wert (Start) |
|---|---|
| Mindestanzahl Fahrzeuge pro angezeigtem Wert | 10 |
| Mindestanzahl verschiedene Händler pro Wert | 3 |
| Kein Händler darf mehr als … des Werts ausmachen | 50 % |
| Region | Kanton bzw. PLZ-Region (2 Ziffern), nie Ort |
| Zeit | Monat, nie Tag |

Wenn eine Regel nicht erfüllt ist → Wert wird nicht angezeigt oder auf grössere Gruppe ausgeweitet (z. B. ganze Schweiz statt Kanton).

## ASTRA-Daten

- Open Data, Pflicht zu «sorgsamem und fachgerechtem Umgang».
- **Offen:** schriftlich beim ASTRA bestätigen lassen, dass die Nutzung in einem kommerziellen Produkt erlaubt ist, und Quellenangabe klären.
