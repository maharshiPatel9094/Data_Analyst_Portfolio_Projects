## SQL_Queries used in CovidDeath analysis

# ALL data
select *
from portfolio_projects.dbo.CovidDeaths
order by 3,4


# data that we will be using
select location, date, total_cases, new_cases, total_deaths, population
from portfolio_projects.dbo.CovidDeaths
order by 1,2

# total cases vs total deaths
# At the end of covid in India there were total of 19164969 cases and the death percent was 1.10%
select location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS death_percent
from portfolio_projects.dbo.CovidDeaths
where location like '%India%'
order by 2 desc

# total cases vs population
# In India, affection percentage at the end of covid pandemic was nearly around 1.38%
select location, date, total_cases, population,  (total_cases/population)*100 AS affection_percent
from portfolio_projects.dbo.CovidDeaths
where location like '%India%'
order by 2 desc


# highest infection rate in country
# In this analysis we got to know that Andorra was having highest infection rate of 17.13%
# In this analysis we got to know that Europe was the highest infected country with 748680069 infected people during the covid pandemic
select location, population, MAX(total_cases) AS highest_infected_country,  MAX(total_cases/population)*100 AS affection_percent
from portfolio_projects.dbo.CovidDeaths
group by location, population
order by 4 desc

# countries with the highest death count per population
# In this analysis we got to know that USA was the country having highest number of death count which was 576232 
select location , MAX(cast(total_deaths as int)) as total_deaths_count
from portfolio_projects.dbo.CovidDeaths
where continent IS NOT NULL
group by location
order by 2 desc

# let's break the things with continent
select continent , MAX(cast(total_deaths as int)) as total_deaths_count
from portfolio_projects.dbo.CovidDeaths
where continent IS NOT NULL
group by continent
order by 2 desc

# with these analysis we got to know that europe was the continent with highest number of deaths 1016750, followed by north america
select location , MAX(cast(total_deaths as int)) as total_deaths_count
from portfolio_projects.dbo.CovidDeaths
where continent IS NULL
group by location
order by 2 desc