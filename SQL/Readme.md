# Layoff Data Analysis Project

This project involves the analysis of companies' layoff data from 2020 to 2024 to understand the impact of the COVID-19 pandemic. The data was initially in raw format and required cleaning. The data cleaning process involved the following steps:

## Data Cleaning Steps

1. **Data Duplication**
2. **Data Standardization**
3. **Removing Null Values**
4. **Removing Unnecessary Columns**

### Step 1: Data Duplication

First, we staged the data in a new table called `layoff_stage2` with an added field `row_number` to track back to our data in case of any mistakes. Data duplicates were removed using the following logic:
~~~sql
INSERT INTO layoff_stage2
SELECT *, ROW_NUMBER() OVER (
    PARTITION BY location, industry, total_laid_off, percentage_laid_off, date, stage, country, funds_raised_millions
) AS row_num
FROM layoff_stage; 
SELECT * FROM layoff_stage2 WHERE row_num > 1;
~~~

### Step 2: Data Standardization
To standardize the data, we removed spaces using TRIM and replaced different variants of the same instance, like "crypto" and "cryptocurrency", with one standard value.

~~~sql
UPDATE layoff_stage2 SET company = TRIM(company);

SELECT industry FROM layoff_stage2 WHERE industry LIKE 'Crypto%';

UPDATE layoff_stage2 SET industry = 'Crypto' WHERE industry LIKE 'Crypto%';

~~~
### Step 3: Removing Null Values
In our data, some entries had null values for the industry. To replace these null values with the actual values, an inner join was used:


~~~sql
SELECT * FROM layoff_stage2 AS l1
JOIN layoff_stage2 AS l2
ON l2.company = l1.company AND l1.location = l2.location
WHERE (l1.industry IS NULL OR l1.industry = '') AND l2.industry IS NOT NULL;

UPDATE layoff_stage2 AS l1
JOIN layoff_stage2 AS l2
ON l2.company = l1.company AND l1.location = l2.location
SET l1.industry = l2.industry
WHERE (l1.industry IS NULL OR l1.industry = '') AND l2.industry IS NOT NULL;

~~~
### Step 4: Removing Unnecessary Columns

To delete unnecessary rows and columns, the following code was used:

~~~sql
DELETE FROM layoff_stage2 WHERE total_laid_off IS NULL AND percentage_laid_off IS NULL;
~~~

The row_num column was added to assist in finding duplicate values. After its use, we can delete it using the command:
~~~sql
ALTER TABLE layoff_stage2 DROP COLUMN row_num;~~~

## Exploratory Data Analysis:

1. **D**
2. **Data Standardization**
3. **Removing Null Values**
4. **Removing Unnecessary Columns**