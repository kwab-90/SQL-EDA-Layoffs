# SQL-EDA-Layoffs
This project applies exploratory data analysis (EDA) techniques using SQL to investigate a dataset of tech industry layoffs. After data cleaning and preparation, this analysis identifies patterns in layoffs by company, country, year, industry, and funding stage, offering a data-driven lens on workforce reduction trends.

## 📦 Dataset Context

The dataset originates from a cleaned and processed version of raw layoff records (**see Data Cleaning Project**), featuring:

- Company, location, country
- Industry, stage, and funding raised
- Layoff figures: total laid off, percentage laid off
- Timestamps and event dates

## 🔍 EDA Objectives

- Identify companies with the highest total layoffs
- Track layoffs across years and industries
- Analyse temporal trends using rolling monthly totals
- Surface top 5 companies per year by layoff volume
- Assess layoff severity by funding stage and country

## 🛠️ SQL Techniques Used
Aggregations: **SUM(), AVG(), MAX(), MIN()**

- Time Series Analysis: using YEAR() and SUBSTRING() for monthly breakdowns
- Window Functions: DENSE_RANK() and SUM() OVER() for rolling totals and rankings
- CTEs (Common Table Expressions): layered queries for clarity and modularity
- Filtering and Sorting: to extract high-impact trends

## 📈 Key Analyses & Insights

- Top Companies by Layoffs: Ranked by total laid off each year
- Layoffs by Industry: Revealed most-affected sectors (e.g., Crypto, Retail Tech)
- Country-Level Trends: Identified the most impacted regions
- Stage Analysis: Compared average layoff percentages across startup funding stages
- Rolling Totals: Built cumulative monthly layoff views to visualise acceleration

## 🧠 Example Queries

**Top 5 companies with highest layoffs per year**

with company_year (company, years, total_laid_off) as -- 1st CTE

(select company, year(`date`), sum(total_laid_off)
from layoffs_staging2
group by company, year(`date`)
order by company desc
), 
Company_Year_Rank as -- 2nd CTE
(
select *, dense_rank() over(partition by years order by total_laid_off desc) as ranking
from company_year
where years is not null
)
select *  
from Company_Year_Rank
where ranking <= 5;
