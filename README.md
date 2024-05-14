# World_Population_Analysis_2023
## Table of Contents
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning, Verification, and Preparation](#data-cleaning-verification-and-preparation)


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
/* Selecting the two tables*/
select * from [dbo].[world_country_stats]
select * from [dbo].[world_population_by_country_2023]
```


For the verification of the data types of the columns
``` SQL
/* verify data types of columns*/
select COLUMN_NAME, DATA_TYPE, CHARACTER_MAXIMUM_LENGTH, NUMERIC_PRECISION, NUMERIC_SCALE
from INFORMATION_SCHEMA.COLUMNS
where TABLE_NAME = 'world_population_by_country_2023';
```

Check the presence of Duplicates in the Country column
``` SQL
/* Check if there are duplicates in the country column*/
select country, COUNT(*) AS Country_count
from [dbo].[world_population_by_country_2023]
group by [country]
HAVING COUNT(*) > 1
```

Handling the missing values
``` SQL
/* Handling missing values of population_urban column*/
update [dbo].[world_population_by_country_2023]
set population_urban = 0
where population_urban IS NULL

/* Handling missing values of fertility_rate column*/
update [dbo].[world_population_by_country_2023]
set fertility_rate = 0
where fertility_rate IS NULL

/* Handling missing values of median_age column*/
update [dbo].[world_population_by_country_2023]
set median_age = 0
where median_age IS NULL
```

### Data Manipulation and Analysis
Joining the two tables on the basis of the country column
``` SQL
/* Join the two tables world_country_stat and World_pop_by_country_2023 on country basis*/
SELECT [dbo].[world_population_by_country_2023].*, [dbo].[world_country_stats].[region]
from [dbo].[world_population_by_country_2023] inner join 
[dbo].[world_country_stats]
on [dbo].[world_population_by_country_2023].[country] = [dbo].[world_country_stats].[country]
```

Creating the region column in table 1 and copying data from table 2 by making an inner join
``` SQL
/* create a region column in World_pop_by_country_2023*/
ALTER TABLE [dbo].[world_population_by_country_2023]
ADD region varchar(50) null

/* Copy the column region from world_pop_stat table*/
UPDATE [dbo].[world_population_by_country_2023]
SET [dbo].[world_population_by_country_2023].[region] = [dbo].[world_country_stats].[region]
FROM [dbo].[world_population_by_country_2023]
inner join [dbo].[world_country_stats]
on [dbo].[world_population_by_country_2023].[country] = [dbo].[world_country_stats].[country]
```

Total Population by Region
``` SQL
/* Aggregate by region in the total population in the world pop 2023 */
select [region], SUM([population]) AS Total_Population_by_region
from [dbo].[world_population_by_country_2023]
group by [region]
order by Total_Population_by_region desc
```

Land area by Region
``` SQL
/* Aggregate by region in the land area in the world pop by country */
select [region], SUM([land_area]) as Total_land_region
from [dbo].[world_population_by_country_2023] 
group by region
order by Total_land_region desc
```

Average Fertility by Region
``` SQL
/* Aggregate Average fertility by region in the world pop 2023 */
select [region], AVG([fertility_rate]) AS AvG_Fertility_by_region
from [dbo].[world_population_by_country_2023]
group by [region]
order by [AvG_Fertility_by_region] desc
```

Average Age by Region
``` SQL
/* Aggregate Average age by region in the world pop 2023 */
select [region], AVG([median_age]) AS median_age_by_region
from [dbo].[world_population_by_country_2023]
group by [region]
order by median_age_by_region desc

Total Net Migrants by Region
``` SQL
/* Total net migrants by region in the world pop 2023 */
select [region], SUM([net_migrants]) AS total_net_migrants_byregion
from [dbo].[world_population_by_country_2023]
group by [region]
order by total_net_migrants_byregion asc
```

### Results/Findings
Summary
1. The global population is approximately 8 billion, with Asia having the highest population and Australia the lowest.
2. Africa has the highest fertility rate (3.7), while North America and Europe have the lowest (1.5 each).
3. The average age in Europe and North America is relatively high (41), whereas Africa has a younger average age (22).
4. North America and Europe are experiencing significant influxes of migrants (1.2 million and 0.8 million respectively), while Africa and Asia are witnessing the highest departures (0.5 million and 1.5 million respectively).
