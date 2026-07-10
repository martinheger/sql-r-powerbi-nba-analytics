# 🏀 End-to-end analýza dat NBA: SQL → R → Power BI

Kompletní datový pipeline od relační databáze až po interaktivní dashboard: vlastní relační databázový model v MS SQL Serveru, zpracování a analýza dat v R, a finální vizualizace v Power BI.

## Přehled pipeline

```
1. SQL (MS SQL Server)  →  2. R (zpracování a analýza)  →  3. Power BI (dashboard)
```

Databáze obsahuje data ze sezóny 2023/24 NBA (National Basketball Association) — výkony hráčů, týmy, draft, divize a konference, ve vlastním relačním modelu (3. normální forma).

## 1️⃣ SQL — databázový model

Vlastní relační databáze (`mahe_projekt_NBA`) s tabulkami pro hráče, týmy, sezónní statistiky, draft, divize a konference. Návrh a příprava datového modelu zdokumentována v [docs/521087-Heger_Martin-M8DBR_projekt___Heger.pdf](./docs/521087-Heger_Martin-M8DBR_projekt___Heger.pdf).

## 2️⃣ R — připojení a analýza

R skript se připojuje přímo na MS SQL Server (přes `DBI`/`odbc`) a načítá data pomocí SQL dotazů (`dbGetQuery`):

```r
nba_zapasy <- dbGetQuery(con, "SELECT * FROM nba_zapasy")
nba_hraci  <- dbGetQuery(con, "SELECT * FROM nba_hraci")
...
```

K dispozici jsou dvě varianty přístupu k datům:
- [`DatSysyProjekt.Rmd`](./2-r-analysis/DatSysyProjekt.Rmd) — přímé SQL dotazy (`dbGetQuery`)
- [`DatSysyProjekt_dbplyr.Rmd`](./2-r-analysis/DatSysyProjekt_dbplyr.Rmd) — alternativní přístup přes `dbplyr`, kde se SQL generuje automaticky z `dplyr` syntaxe a dotazy běží přímo na serveru

📄 [Zobrazit vyrenderovanou analýzu (HTML)](./2-r-analysis/DatSysyProjekt.html)

> Skripty vyžadují vlastní připojení k MS SQL Server databázi — bez přístupu k databázi je nelze spustit, výsledky jsou ale k prohlédnutí v přiloženém HTML.

## 3️⃣ Power BI — dashboard

Transformace relačního modelu do analytického schématu vhodného pro Power BI, s řešením cyklických závislostí při modelování sportovních utkání. Dokumentace přístupu v [docs/power_bi_nba_dokumentace.pdf](./docs/power_bi_nba_dokumentace.pdf) a [docs/project_plan_powerbi.pdf](./docs/project_plan_powerbi.pdf).

```
3-power-bi/
├── nba_powerbi_dashboard.pbix       # hlavní verze dashboardu
└── nba_powerbi_dashboard_alt.pbix   # alternativní verze (stejná data, jiné rozvržení reportu)
```

## Struktura repa

```
├── 2-r-analysis/    # R skripty pro připojení k DB a analýzu (2 přístupy: přímé SQL a dbplyr)
├── 3-power-bi/       # Power BI dashboardy (.pbix)
└── docs/              # dokumentace datového modelu a Power BI implementace
```

## Tech stack
MS SQL Server · R (DBI, odbc, dbplyr) · Power BI (DAX) · relační databázové modelování

---
*Školní projekt (Databázové systémy a R v datové vědě, M8DBR).*
