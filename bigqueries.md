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
