# SQL Layoffs Dataset - Data Cleaning

This project focuses on **Data Cleaning** of the global tech layoffs dataset.  
The cleaned and standardized table `layoffs_staging2` serves as the foundation for later **Exploratory Data Analysis (EDA)**.

## Objectives
- Import raw dataset (`layoffs.csv`) into SQL
- Standardize column names and formats
- Handle missing values and duplicates
- Ensure consistent date formats
- Prepare dataset for analysis and visualization

## Dataset
- **Source:** Public layoffs data (Kaggle/online sources)  
- **Raw Table:** `layoffs_raw`  
- **Cleaned Table:** `layoffs_staging2`  
- **Key Columns:** `company`, `industry`, `country`, `total_laid_off`, `percentage_laid_off`, `funds_raised_millions`, `date`, `stage`

## SQL Techniques Used
- Importing and transforming CSV data
- Removing duplicates and handling NULLs
- Standardizing text formats (e.g., company names, country names)
- Converting string dates to SQL `DATE` type
- Using conditional statements for consistent stage and industry values

## Example Queries
```sql
-- Remove duplicates
DELETE FROM layoffs_raw
WHERE id NOT IN (
    SELECT MIN(id)
    FROM layoffs_raw
    GROUP BY company, date
);

-- Standardize date format
UPDATE layoffs_raw
SET date = STR_TO_DATE(date, '%Y-%m-%d');

-- Handle missing values
UPDATE layoffs_raw
SET total_laid_off = 0
WHERE total_laid_off IS NULL;

-- Create cleaned table
CREATE TABLE layoffs_staging2 AS
SELECT *
FROM layoffs_raw
WHERE company IS NOT NULL AND date IS NOT NULL;
