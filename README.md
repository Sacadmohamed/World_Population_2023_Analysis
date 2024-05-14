# World_Population_Analysis_2023
### Project Overview
This project analyzes the world population in 2023, focusing on regions and countries. It examines total populations, fertility rate, net migration, land area, average age, and world share. By uncovering trends and patterns, it provides valuable insights for policymakers and researchers. 

### Data Source
The primary data sources for this project are "world_country_stats.csv" and  "world_population_by_country_2023.csv" from Kaggle under this [link](https://www.kaggle.com/datasets/chandanchoudhury/world-population-dataset).

### Tools
- Excel for downloading the files and uploading them to the database
- SQL Server for Data cleaning, verification, and Analysis
- Power BI for the visualization and building of the report

### Data Cleaning, Verification, and Preparation
1. Verification of the data types of the columns
2. Checking the presence of Duplicates
3. Handling of the missing values

For the selection of the two uploaded tables
``` SQL

```


For the verification of the data types of the columns
``` SQL
select COLUMN_NAME, DATA_TYPE, CHARACTER_MAXIMUM_LENGTH, NUMERIC_PRECISION, NUMERIC_SCALE
from INFORMATION_SCHEMA.COLUMNS
where TABLE_NAME = 'world_population_by_country_2023';
```
