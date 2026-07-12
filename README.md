# End-to-End NBA Data Analytics: SQL → R → Power BI

**[English](README.md) | [Česky](README_cz.md)**

A complete data pipeline from a relational database to an interactive dashboard: custom relational database model in MS SQL Server, data processing and analysis in R, and final visualization in Power BI.

## Pipeline Overview

```text
1. SQL (MS SQL Server) → 2. R (processing & analysis) → 3. Power BI (dashboard)
```

The database contains data from the 2023/24 NBA season — player performances, teams, drafts, divisions, and conferences, structured in a custom relational model (3rd Normal Form).

## 1. SQL — Database Model

Custom relational database (`mahe_projekt_NBA`) with tables for players, teams, seasonal stats, drafts, divisions, and conferences. Design and preparation of the data model are documented in [docs/521087-Heger_Martin-M8DBR_projekt___Heger.pdf](./docs/521087-Heger_Martin-M8DBR_projekt___Heger.pdf).

## 2. R — Connection and Analysis

The R script connects directly to MS SQL Server (via DBI/odbc) and loads data using SQL queries:

```r
nba_zapasy <- dbGetQuery(con, "SELECT * FROM nba_zapasy")
nba_hraci  <- dbGetQuery(con, "SELECT * FROM nba_hraci")
```

Two data access approaches are available:

- [DatSysyProjekt.Rmd](./2-r-analysis/DatSysyProjekt.Rmd) — direct SQL queries (`dbGetQuery`)
- [DatSysyProjekt_dbplyr.Rmd](./2-r-analysis/DatSysyProjekt_dbplyr.Rmd) — alternative approach via `dbplyr`, where SQL is generated automatically from `dplyr` syntax and queries run directly on the server

View the report: [https://htmlpreview.github.io/?https://github.com/martinheger/sql-r-powerbi-nba-analytics/blob/main/2-r-analysis/DatSysyProjekt_static.html](https://htmlpreview.github.io/?https://github.com/martinheger/sql-r-powerbi-nba-analytics/blob/main/2-r-analysis/DatSysyProjekt_static.html)

The report above is a static version without the interactive Shiny widget. The original source code ([DatSysyProjekt.Rmd](./2-r-analysis/DatSysyProjekt.Rmd)) includes an interactive widget for player selection (`runtime: shiny`) — running it requires an active R session with a database connection.

## 3. Power BI — Dashboard

Transformation of the relational model into an analytical schema suitable for Power BI, including solving cyclic dependencies when modeling sports matches. The approach is documented in [docs/power_bi_nba_dokumentace.pdf](./docs/power_bi_nba_dokumentace.pdf).

Dashboard: [3-power-bi/nba_powerbi_dashboard.pbix](./3-power-bi/nba_powerbi_dashboard.pbix)

## Repo Structure

```text
├── 2-r-analysis/   # R scripts for DB connection and analysis (direct SQL & dbplyr)
├── 3-power-bi/     # Power BI dashboards (.pbix)
└── docs/           # data model and Power BI implementation documentation
```

## Tech Stack

MS SQL Server, R (DBI, odbc, dbplyr), Power BI (DAX), Relational Database Modeling

## Authorship and Context

This project was developed across two consecutive courses at Masaryk University:

- SQL and R part — course M8DBR Database Systems and R in Data Science, Faculty of Science (Spring 2025). Individual work.
- Power BI part — course MPM_DPBI Data Processing with Power BI, Faculty of Economics and Administration (Autumn 2025).

## License

MIT — see [LICENSE](./LICENSE)
