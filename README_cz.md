# End-to-end analýza dat NBA: SQL → R → Power BI

**[English](README.md) | [Česky](README_cz.md)**

Kompletní datový pipeline od relační databáze až po interaktivní dashboard: vlastní relační databázový model v MS SQL Serveru, zpracování a analýza dat v R, a finální vizualizace v Power BI.

## Přehled pipeline

```text
1. SQL (MS SQL Server) → 2. R (zpracování a analýza) → 3. Power BI (dashboard)
```

Databáze obsahuje data ze sezóny 2023/24 NBA (National Basketball Association) — výkony hráčů, týmy, draft, divize a konference, ve vlastním relačním modelu (3. normální forma).

## 1. SQL — databázový model

Vlastní relační databáze (mahe_projekt_NBA) s tabulkami pro hráče, týmy, sezónní statistiky, draft, divize a konference. Návrh a příprava datového modelu zdokumentována v [docs/521087-Heger_Martin-M8DBR_projekt___Heger.pdf](./docs/521087-Heger_Martin-M8DBR_projekt___Heger.pdf).

## 2. R — připojení a analýza

R skript se připojuje přímo na MS SQL Server (přes DBI/odbc) a načítá data pomocí SQL dotazů:

```r
nba_zapasy <- dbGetQuery(con, "SELECT * FROM nba_zapasy")
nba_hraci  <- dbGetQuery(con, "SELECT * FROM nba_hraci")
```

K dispozici jsou dvě varianty přístupu k datům:

- [DatSysyProjekt.Rmd](./2-r-analysis/DatSysyProjekt.Rmd) — přímé SQL dotazy (dbGetQuery)
- [DatSysyProjekt_dbplyr.Rmd](./2-r-analysis/DatSysyProjekt_dbplyr.Rmd) — alternativní přístup přes dbplyr, kde se SQL generuje automaticky z dplyr syntaxe a dotazy běží přímo na serveru

Zobrazit report: [https://htmlpreview.github.io/?https://github.com/martinheger/sql-r-powerbi-nba-analytics/blob/main/2-r-analysis/DatSysyProjekt_static.html](https://htmlpreview.github.io/?https://github.com/martinheger/sql-r-powerbi-nba-analytics/blob/main/2-r-analysis/DatSysyProjekt_static.html)

Report výše je statická verze bez interaktivního Shiny widgetu. Původní zdrojový kód ([DatSysyProjekt.Rmd](./2-r-analysis/DatSysyProjekt.Rmd)) obsahuje i interaktivní widget pro výběr hráče (`runtime: shiny`) — jeho spuštění vyžaduje běžící R session s připojením na databázi.

## 3. Power BI — dashboard

Transformace relačního modelu do analytického schématu vhodného pro Power BI, s řešením cyklických závislostí při modelování sportovních utkání. Dokumentace přístupu v [docs/power_bi_nba_dokumentace.pdf](./docs/power_bi_nba_dokumentace.pdf).

Dashboard: [3-power-bi/nba_powerbi_dashboard.pbix](./3-power-bi/nba_powerbi_dashboard.pbix)

## Struktura repa

```text
├── 2-r-analysis/   # R skripty pro připojení k DB a analýzu (2 přístupy: přímé SQL a dbplyr)
├── 3-power-bi/     # Power BI dashboardy (.pbix)
└── docs/           # dokumentace datového modelu a Power BI implementace
```

## Tech stack

MS SQL Server, R (DBI, odbc, dbplyr), Power BI (DAX), relační databázové modelování

## Autorství a kontext

Projekt vznikl na Masarykově univerzitě ve dvou navazujících kurzech:

- SQL a R část — kurz M8DBR Databázové systémy a R v datové vědě, Přírodovědecká fakulta (jaro 2025). Individuální práce.
- Power BI část — kurz MPM_DPBI Zpracování dat s PowerBI, Ekonomicko-správní fakulta (podzim 2025).

## Licence

MIT — viz [LICENSE](./LICENSE)
