# Superstore – Profitabilitätsanalyse nach Verkaufsregionen

**IHK Data Analyst · Abschlussprojekt · Oktober 2026**  
**Teamprojekt mit KNIME und Microsoft Power BI**

## Projektüberblick

Wie kann ein Einzelhandelsunternehmen seinen Gewinn verbessern, obwohl der Umsatz bereits wächst?

In diesem gemeinsamen Abschlussprojekt haben wir den öffentlich verfügbaren, **fiktiven Beispieldatensatz „Sample Superstore“ von Tableau** untersucht. Im Mittelpunkt standen die Profitabilität der vier Verkaufsregionen **Central, East, South und West**, der Einfluss von Rabatten und die Retourenentwicklung.

Unser Ziel war es, aus Rohdaten einen nachvollziehbaren Analyseprozess aufzubauen: **Datenaufbereitung und explorative Analyse mit KNIME, Datenmodellierung und Visualisierung mit Power BI sowie konkrete, datenbasierte Handlungsempfehlungen.**

## Unsere Leitfragen

1. Wie entwickeln sich Umsatz, Gewinn und Bestellungen nach Region im Jahres-, Monats- und Wochenverlauf?
2. Wie unterscheiden sich Gewinn und Retouren nach Produktkategorie, Kundensegment, Region und Versandart?
3. Welche Zusammenhänge bestehen zwischen Rabatt, Umsatz je Stück, Absatzmenge und Profitabilität?
4. Welche regionalen Maßnahmen lassen sich aus den Analyseergebnissen ableiten?

## Datenbasis

Die Excel-Quelldatei enthält die Tabellen **Orders**, **Returns** und **People**.

| Kennzahl | Umfang |
|---|---:|
| Verkaufspositionen vor der Bereinigung | 10.194 |
| Verkaufspositionen nach der Bereinigung | 10.192 |
| Eindeutige Bestellungen | 5.111 |
| Kunden | 804 |
| Produkte | 1.862 |
| Verkaufsregionen | 4 |
| Analysezeitraum | 2023–2026 |

Zwei tatsächliche Duplikate wurden bei der Datenbereinigung entfernt. Retouren wurden auf Basis der Bestellkennungen zugeordnet; die Kennzahl **Retourenquote** bezieht sich auf *eindeutige Bestellungen*, nicht auf Verkaufspositionen.

**Datenquelle:** Tableau, „Sample Superstore“ (fiktive Beispieldaten). Die Originalquelle und die Nutzungsbedingungen sind bei einer Weiterverwendung zu beachten. Die Währung wird in diesem Projekt als **US-Dollar angenommen**, da sie in der Quelldatei nicht eindeutig ausgewiesen ist.

## Technologien und Vorgehensweise

| Werkzeug | Einsatz |
|---|---|
| **Microsoft Excel** | Rohdaten und Übergabedatei |
| **KNIME Analytics Platform** | Datenprüfung, ETL, Joins, Berechnungen, explorative Datenanalyse und Export |
| **Microsoft Power BI** | Sternschema, DAX-Kennzahlen, interaktive Berichtsseiten und Visualisierungen |

```mermaid
flowchart LR
    A[Excel: Orders, Returns, People] --> B[KNIME: Bereinigung und Joins]
    B --> C[KNIME: Kennzahlen und EDA]
    C --> D[Excel: Fakt- und Dimensionstabellen]
    D --> E[Power BI: Datenmodell und DAX]
    E --> F[Dashboard und Empfehlungen]
```

### Datenaufbereitung und Analyse in KNIME

- Einlesen, Prüfen und Verknüpfen der drei Quelltabellen
- Prüfung und Entfernung von Duplikaten sowie Behandlung leerer Retourenkennzeichen
- Berechnung zusätzlicher Kennzahlen, unter anderem Marge, Umsatz je Stück, Versanddauer und Verlustkennzeichen
- Analysen mit **GroupBy**, Rabattklassen, **IQR-Ausreißererkennung** und Korrelationsanalyse
- Kennzeichnung auffälliger Werte, ohne plausible Geschäftsvorfälle pauschal zu löschen
- Export der aufbereiteten Tabellen nach `data/processed/superstore_clean.xlsx`

### Datenmodell in Power BI

Die KNIME-Übergabedatei enthält:

- `Fakt_Verkauf` – 10.192 Verkaufspositionen
- `Dim_Produkt` – 1.862 Produkte
- `Dim_Kunde` – 804 Kunden
- `Dim_Region` – 4 Regionen

In Power BI wurde zusätzlich eine Datumstabelle (`Dim_Date`) erstellt. Das Datenmodell nutzt ein **Sternschema** und DAX-Measures zur Berechnung und Filterung der Kennzahlen.

## Power-BI-Dashboard

Der Bericht besteht aus vier Seiten:

| 1. Übersicht | 2. Gewinn & Retouren |
|---|---|
| ![Dashboard: Übersicht](images/01_overview.png) | ![Dashboard: Gewinn und Retouren](images/02_profit_returns.png) |
| **3. Rabatt & Profitabilität** | **4. Empfehlungen** |
| ![Dashboard: Rabatt und Profitabilität](images/03_discount_analysis.png) | ![Dashboard: Handlungsempfehlungen](images/04_recommendations.png) |

Die Originaldatei ist unter [`powerbi/Superstore_8.pbix`](powerbi/Superstore_8.pbix) abgelegt.

## Zentrale Ergebnisse

| Analyseergebnis | Beobachtung |
|---|---|
| **Umsatzentwicklung** | Von 2023 bis 2026 stieg der Umsatz um rund **51 %**; 2024 gab es einen Rückgang gegenüber dem Vorjahr. |
| **Verlustpositionen** | **1.900 von 10.192 Verkaufspositionen (18,6 %)** weisen einen negativen Gewinn aus. |
| **Regionale Profitabilität** | Die Gewinnmarge beträgt in **Central 7,9 %**, in **West 15,0 %**. |
| **Produktkategorien** | Die Kategorie **Furniture** erreicht eine Marge von lediglich **2,6 %**. |
| **Hohe Rabatte** | Positionen mit **mehr als 20 % Rabatt** ergeben zusammen einen Gewinn von rund **−136.009 $**. |
| **Rabatt und Menge** | Zwischen Rabatt und Stückzahl besteht praktisch keine lineare Korrelation (**r ≈ 0,008**). |
| **Retouren** | Die Retourenquote liegt in **West bei 11,6 %**, in den anderen Regionen bei ungefähr **2,9–3,3 %**. |

Diese Werte beschreiben **Zusammenhänge in den vorliegenden Daten**. Sie beweisen keine kausalen Wirkungen.

## Drei Handlungsempfehlungen

| Schwerpunkt | Datenbasierter Befund | Vorgeschlagene Maßnahme |
|---|---|---|
| **Rabattfreigabe** | Positionen mit mehr als 20 % Rabatt: **−136.009 $** aggregierter Gewinn | Rabatte über 20 % nur nach Freigabe; Wirkung zunächst testen |
| **Region Central** | **27,7 %** der Positionen mit mehr als 20 % Rabatt; diese Positionen ergeben **−52.444 $** | Rabattpolitik und betroffene Produktgruppen gemeinsam mit dem Regionalmanagement prüfen |
| **Retouren in West** | **11,6 %** Retourenquote; **107.483 $** Umsatz sind mit retournierten Bestellungen verbunden | Retourengründe nach Produkt, Kunde und Versandart systematisch erfassen |

**Wichtig:** Die ausgewiesenen negativen Ergebnisse sind keine garantiert vermeidbaren Verluste. Ebenso sind **107.483 $ Umsatz aus retournierten Bestellungen nicht automatisch 107.483 $ endgültig verlorener Umsatz**. Die vorgeschlagenen Maßnahmen sollten kontrolliert erprobt und anhand von KPIs bewertet werden.

## Projektstruktur

```text
superstore-profitability-analysis/
├── README.md
├── data/
│   ├── raw/
│   │   └── sample_-_superstore.xlsx
│   └── processed/
│       └── superstore_clean.xlsx
├── knime/
│   └── Superstore_Analyse_clean.knar
├── powerbi/
│   └── Superstore_8.pbix
├── images/
│   ├── 01_overview.png
│   ├── 02_profit_returns.png
│   ├── 03_discount_analysis.png
│   └── 04_recommendations.png
├── docs/
│   └── Projektdokumentation und Projektunterlagen
└── presentation/
    └── Abschlusspräsentation
```

*Hinweis: Dateinamen im Repository und die Bildpfade gegebenenfalls an die tatsächlich hochgeladenen Dateien anpassen.*

## Projekt nachvollziehen

1. Die Quelldaten unter `data/raw/` prüfen.
2. Den KNIME-Workflow aus `knime/` in KNIME importieren und bei Bedarf Dateipfade für Excel Reader bzw. Writer anpassen.
3. Die bereinigten Tabellen unter `data/processed/` einsehen bzw. mit KNIME neu erzeugen.
4. Die `.pbix`-Datei aus `powerbi/` in Power BI Desktop öffnen. Falls erforderlich, den Pfad zur Excel-Quelldatei in den Datenquelleneinstellungen aktualisieren.
5. Die vier Berichtsseiten und die dokumentierten Analyseergebnisse vergleichen.

## Team und Zusammenarbeit

Dieses Abschlussprojekt wurde **gemeinsam als vierköpfiges Team** durchgeführt:

| Teammitglied | Schwerpunkt |
|---|---|
| **Mesut Karagöz** | Projektleitung, Koordination, visuelles Konzept und Zusammenführung der Handlungsempfehlungen |
| **Erol Bek** | KNIME – ETL, Datenbereinigung und Aufbereitung |
| **Vivien Kreitow** | KNIME – explorative Datenanalyse und Auswertung |
| **Gözde Kuyumcu** | Power BI – Datenmodell, Kennzahlen und Dashboard |

Konzeption, Diskussion der Ergebnisse, Qualitätssicherung und Abschlusspräsentation waren Teil unserer **gemeinsamen Teamarbeit**.

## Grenzen der Analyse

- Der Datensatz ist **fiktiv** und bildet keine realen Unternehmensentscheidungen ab.
- Es handelt sich um eine **beobachtende Analyse**; Korrelation bedeutet nicht Kausalität.
- Die tatsächliche Wirkung einer Rabattgrenze wurde **nicht experimentell geprüft**.
- Konkrete Retourengründe und separate detaillierte Kostendaten liegen nicht vor; die vorhandene Gewinnkennzahl wurde aus dem Datensatz übernommen.
- Die Währung wurde als **USD angenommen**.

---

**IHK Data Analyst · Gruppenprojekt · Oktober 2026**  
*Von der Datenaufbereitung bis zu nachvollziehbaren Managementempfehlungen.*
