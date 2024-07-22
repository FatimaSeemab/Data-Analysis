# Layoff Data Analysis Project

This project involves the analysis of companies' layoff data from 2020 to 2024 to understand the impact of the COVID-19 pandemic. The data was cleaned first and then a detailed exploratory data analysis was performed on dataset. 

## Time Range of Data:
To extract data belongs to which years and timeline.

~~~sql
select min(`date`),max(`date`) from layoff_stage2;
~~~

## Some Useful Insights:
Furthurmore,we extarcted useful insights such as which companies, industries and countries have are most affected and have maximum layoffs during this time periods.Some of the sql commands are follows

~~~sql

Select industry, Sum(total_laid_off)  from layoff_stage2
group by industry order by 2 DESC;
~~~

~~~sql
Select country, Sum(total_laid_off)  from layoff_stage2
group by country order by 2 DESC;
~~~

~~~sql
Select Year(`date`), Sum(total_laid_off)  from layoff_stage2
group by Year(`date`) order by 2 DESC;
~~~
## Top 5 Rankings Based on Layoffs
The top 5 rankings for layoffs per year were generated using the following command:
~~~sql

with rank_company_on_layoffs as (
select company, year(`date`) as year_name,SUM(total_laid_off) as total from layoff_stage2 
where year(`date`) is not null
group by company,year(`date`) 
having SUM(total_laid_off) is not null
order by 2 desc
), 

company_ranking as(
select *, DENSE_Rank() 
over(partition by year_name order by total desc) as ranking 
from rank_company_on_layoffs)
select * from company_ranking where ranking<=5;
~~~
