# 🦠 COVID-19 Data Analysis — SQL Portfolio Project

An exploratory data analysis of global COVID-19 trends using SQL. This project examines infection rates, death counts, and vaccination progress across countries and continents using real-world data.

---

## 📁 Project Structure

covid_analysis/
│
├── CovidDeaths.csv           
├── CovidVaccinations.csv     
├── covid_analysis.sql          # Main SQL script with all queries
└── README.md

---

## 🗄️ Database & Tables

**Database:** `PortfolioProject`

| Table | Description |
|---|---|
| `CovidDeaths` | Country-level daily data on cases, deaths, and population |
| `CovidVaccinations` | Country-level daily data on vaccination counts |

---

## 🔍 What This Project Covers

### 1. Death Rate Analysis
Calculates the percentage of confirmed cases that resulted in death — giving a sense of COVID fatality rates by country over time.

### 2. Infection Rate vs Population
Shows what share of each country's population was infected with COVID-19 at various points in time.

### 3. Highest Infection Rates by Country
Ranks countries by their peak infection count relative to population size.

### 4. Death Count by Country & Continent
Identifies which countries and continents suffered the highest total death tolls.

### 5. Global Totals
Aggregates worldwide case and death numbers to compute an overall global death percentage.

### 6. Vaccination Rollout Tracking
Uses a **rolling SUM window function** to track cumulative vaccinations per country over time.

---

## 🛠️ SQL Concepts Used

- `JOIN` — Merging death and vaccination tables on location and date
- `Window Functions` — Rolling vaccination totals with `SUM() OVER (PARTITION BY ... ORDER BY ...)`
- `CTE (Common Table Expressions)` — Cleaner calculation of vaccination percentages
- `Temporary Tables` — Storing intermediate results for reuse
- `Views` — Creating a reusable `PercentPopVaccinatedView` for downstream visualization
- `Aggregate Functions` — `MAX()`, `SUM()`, `CAST()`, `COALESCE()`
- `Filtering` — Excluding non-country rows (e.g., `WHERE continent IS NOT NULL`)

---

## 📊 Key Queries at a Glance

```sql
-- Death percentage by country
SELECT Location, date, total_cases, total_deaths,
       (total_deaths/total_cases)*100 AS DeathPercentage
FROM CovidDeaths WHERE continent IS NOT NULL;

-- Rolling vaccination count using window function
SELECT dea.location, dea.date, vac.new_vaccinations,
       SUM(CAST(COALESCE(vac.new_vaccinations, 0) AS SIGNED))
       OVER (PARTITION BY dea.Location ORDER BY dea.date) AS RollingPeopleVaccinated
FROM CovidDeaths dea JOIN CovidVaccinations vac
ON dea.location = vac.location AND dea.date = vac.date;
```

---

## 🚀 How to Run

1. Import the `CovidDeaths` and `CovidVaccinations` CSV datasets into your MySQL instance.
2. Create and select the `PortfolioProject` database.
3. Run `covid_analysis.sql` section by section in MySQL Workbench or any SQL client.

> **Note:** This project is written for **MySQL**. Some syntax (e.g., `CAST AS SIGNED`, `STR_TO_DATE`) may need adjustment for SQL Server or PostgreSQL.

---

## 📂 Data Source

Data sourced from [Our World in Data – COVID-19 Dataset](https://ourworldindata.org/covid-deaths)

---

## 👤 Author

**Arya Tilekar**
[GitHub](https://github.com/AryaTilekar) • [LinkedIn](https://www.linkedin.com/in/arya-tilekar/)
