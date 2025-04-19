 #  Data Exploration of COVID-19 Deaths and Vaccinations Using SQL
 ![Logo](https://github.com/vikassaraswatiitg26/Covid19_sql_project/blob/main/covid.logo1.jpg)

## Objective :

   This project uses SQL to explore and analyze global COVID-19 data — focusing on infections, deaths, and 
   vaccinations — to derive meaningful insights and trends.
   
# 📌 Project Overview

## The dataset consists of two tables:

   1. Covid_Death – Includes data on COVID-19 cases, deaths, hospitalizations, and other related statistics.

   2. Covid_Vaccination – Contains vaccination data, testing metrics, and socio-economic indicators.

## The project is built to:

• Analyze the spread and fatality of COVID-19 across countries and continents.

• Compare total cases vs total deaths to understand mortality rates.

• Explore infection penetration by comparing total cases with population.

• Track vaccination progress by calculating cumulative vaccinations and % population vaccinated.

• Create views and temporary tables to aid in visualization and reporting.


# 🛠️ Tools & Technologies:

• SQL (Subqueries, Joins, Views, CTEs, Window Functions)

• PostgreSQL / MySQL / SQL Server (any RDBMS)

• Data Source: Our World in Data - COVID-19 Dataset

# 🔍 Key SQL Operations

1. Total Cases vs Total Deaths
-- Determining death likelihood per infection by country:

``` SELECT location1, date, total_cases, total_deaths,
       ROUND((total_deaths/total_cases)*100, 2) AS Death_Percentage
FROM Covid_Death
WHERE location1 LIKE '%India%' AND total_cases IS NOT NULL;
```
2. Infection Rate per Population

-- Shows what percentage of a country's population has been infected:

```SELECT location1, date, total_cases, population,
       ROUND((total_cases/population)*100, 2) AS Infection_Rate
FROM Covid_Death
WHERE continent IS NOT NULL AND total_cases IS NOT NULL;
```
3. Vaccination Progress

-- Using window functions to calculate rolling vaccination numbers and vaccination percentages:

```SELECT cd.continent, cd.location1, cd.date, cd.population, cv.new_vaccinations,
       SUM(cv.new_vaccinations) OVER (PARTITION BY cd.location1 ORDER BY cd.date) AS rolling_people_vaccinated,
       ROUND((SUM(cv.new_vaccinations) OVER (PARTITION BY cd.location1 ORDER BY cd.date) / cd.population)*100, 3) AS Vaccinated_Percentage
FROM Covid_death cd
JOIN Covid_vaccination cv ON cd.location1 = cv.location1 AND cd.date = cv.date
WHERE cd.continent IS NOT NULL;
```

4. View for Visualization

--  Creating a view to assist in later data visualizations:

```CREATE VIEW pop_vs_vacc AS
SELECT cd.continent, cd.location1, cd.date, cd.population,
       cv.new_vaccinations,
       SUM(cv.new_vaccinations) OVER (PARTITION BY cd.location1 ORDER BY cd.date) AS rolling_people_vaccinated
FROM Covid_death cd
JOIN Covid_vaccination cv ON cd.location1 = cv.location1 AND cd.date = cv.date
WHERE cd.continent IS NOT NULL;
```

# 📈 Insights & Observations

• Countries show vastly different death percentages relative to reported cases.

• Vaccination rates grew rapidly after 2021, with rolling vaccination giving a clearer picture than daily numbers.

• Some countries have disproportionately low testing or vaccination despite high population density.

# ✅ Future Enhancements

• Connect the SQL backend to Power BI or Tableau for dashboards.

• Clean and format data using Python or Pandas before loading into SQL.

• Automate daily data updates using ETL pipelines.






