# World Bank

* Meta Data - No.of Rows x Columns
* Summary Stats
* Univariate (Single Column)
  * Histogram, Density Plot - Frequency
* Bivariate Analysis (Two columns)  
  * Scatter Plot - Correlation

# the number of countries
```sql
select count(distinct country_code) from `patents-public-data.worldbank_wdi.wdi_2016`
```

# Average min max year
```sql
```

# average min and max surface area of all countries in the dataset cummulative
```sql
select min(indicator_value) as min_surface_are,
max(indicator_value) as max_surface_area,
avg(indicator_value) as avg_surface_area
from `patents-public-data.worldbank_wdi.wdi_2016`
where lower(indicator_name) like '%surface%'
limit 100
```

# average min and max surface area of all countries in the dataset individual
```sql
select country_name, min(indicator_value) as min_surface_are,
max(indicator_value) as max_surface_area,
avg(indicator_value) as avg_surface_area
from `patents-public-data.worldbank_wdi.wdi_2016`
where lower(indicator_name) like '%surface%'
group by country_name
```
# Co2 emissions of a country per year
```sql
SELECT * 
FROM `patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'CO2 emissions (kt)'
and country_name = 'Germany'
and indicator_value > 0
order by year
```
# Avg. Co2 emissions of a country over the years
```sql
select avg(indicator_value), country_name
FROM `patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'CO2 emissions (kt)'
group by country_name
```

# Which country's manufacturing sector has largest contribution to GDP?
```sql
select *
FROM `patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'Manufacturing, value added (% of GDP)'
and year =2015
order by indicator_value desc
limit 1
```
```sql
select count(indicator_value),year
FROM `patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'Manufacturing, value added (% of GDP)'
and indicator_value > 0
group by year
```

# Total population for each country?
```sql
SELECT
country_name ,avg(indicator_value) as Population
FROM `patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name="Population, total"
group by country_name
```

# Urban population of countries where gdp < global avergage
```sql
with avg_gdp as (
SELECT
avg(indicator_value) as avg_gdp_per_capita
FROM
  `patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'GDP per capita (current US$)'
and year = 2015
),

list_of_countries as 
(select 
distinct (country_name),indicator_value 
FROM
  `patents-public-data.worldbank_wdi.wdi_2016`
join avg_gdp a
on 1 = 1 
where indicator_name = 'GDP per capita (current US$)'
and year = 2015
and indicator_value > a.avg_gdp_per_capita)

select loc.country_name, world.indicator_value as urban_population
FROM list_of_countries loc 
join `patents-public-data.worldbank_wdi.wdi_2016` world
on world.country_name = loc.country_name 
where indicator_name = 'Urban population'
and year = 2015
order by urban_population desc
```
