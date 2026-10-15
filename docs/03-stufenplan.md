# 03 – Stufenplan: vom DMS zum Datenprodukt

Eurotax hat 1936 auch mit gesammelten Händlerdaten angefangen. Der Mechanismus ist heute derselbe, nur digital.

## Stufe 1 – Daten einsammeln, ohne dass es wehtut (ab sofort)

- Jeder Händler erfasst im DMS sowieso Fahrzeug, Ausstattung, Angebotspreis und Verkauf.
- Wichtig: **von Anfang an strukturiert** erfassen (Typengenehmigungsnummer, feste Ausstattungsliste, kein Freitext).
- Technische Daten automatisch aus ASTRA füllen → weniger Tipparbeit für den Händler, sauberere Daten für uns.
- Details: [04-datenmodell.md](04-datenmodell.md), [06-datenpipeline.md](06-datenpipeline.md)

## Stufe 2 – Rechtliche Grundlage (vor dem ersten Kunden)

- AGB-Klausel: anonymisierte, aggregierte Nutzung der Transaktionsdaten.
- Datenschutzerklärung auf Homepage und in der App.
- Hinweis beim Onboarding.
- Details: [05-recht-datenschutz.md](05-recht-datenschutz.md)

## Stufe 3 – Masse sammeln (Jahr 1–3)

- 10 Händler → einige hundert Verkäufe / Jahr → zu wenig für Bewertungen.
- 100 Händler → tausende Verkäufe → statistisch brauchbar für häufige Modelle.
- Fokus: **DMS verkaufen**, nicht Datenprodukt. Das DMS ist der Köder, die Daten sind der Fang.

## Stufe 4 – Daten als Feature zurückgeben (ab genug Volumen)

- Im DMS: «Ähnliche Fahrzeuge wurden in deiner Region für CHF X verkauft», «Durchschnittliche Standzeit: Y Tage».
- Ausstattungsvorschläge pro Variante (System lernt, was bei einem Golf 1.5 TSI typisch ist).
- Macht das DMS wertvoller → mehr Händler → mehr Daten. Schwungrad dreht.

## Stufe 5 – Eigenes Datenprodukt (Jahr 5–10)

- Bewertungs-API an Garagen ausserhalb von Tiff, Leasingfirmen, Versicherungen, Banken.
- Eigene Daten ersetzen für die eigenen Händler den teuren Eurotax-Zugang.
- Langfristig: Datenbestand ist für Eurotax/JD Power oder AutoScout24 als Kaufobjekt interessant.

## Was wir verkaufen (und was nicht)

| Verkaufbar (nur bei uns) | Nicht verkaufbar |
|---|---|
| Echte Verkaufspreise | ASTRA-Daten (für alle gratis) |
| Typische Ausstattung pro Variante | VIN-Dekodierung (Norm) |
| Standzeiten | Daten einzelner Händler |
| Regionale Preisunterschiede Schweiz | Personendaten von Käufern |
