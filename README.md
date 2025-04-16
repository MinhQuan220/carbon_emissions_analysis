# carbon_emissions_analysis

## 1. Introduction
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
![image](https://github.com/user-attachments/assets/e0913708-3a95-4944-88e2-8c61af2a8c1d)

### 1.1. Data Model
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
![image](https://github.com/user-attachments/assets/1c7a88a2-0844-4935-9392-3440b2dc1d66)

### 1.2. Data Structure
Table 'product_emissions'
```SQL
SELECT * FROM product_emissions LIMIT 10;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2014|85|28|11|2014|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2015|85|28|6|2015|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|

Table 'companies'
```SQL
SELECT * FROM companies LIMIT 10;
```
|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|
|6|"Compañía Española de Petróleos, S.A.U. CEPSA"|
|7|"Daikin Industries, Ltd."|
|8|"Elitegroup computer systems co., Ltd."|
|9|"Fuji Xerox Co., Ltd."|
|10|"Gamesa Corporación Tecnológica, S.A."|

Table 'countries'
```SQL
SELECT * FROM countries LIMIT 10;
```
|id|country_name|
|--|------------|
|1|Australia|
|2|Belgium|
|3|Brazil|
|4|Canada|
|5|Chile|
|6|China|
|7|Colombia|
|8|Finland|
|9|France|
|10|Germany|

Table 'industry_groups'
```SQL
SELECT * FROM industry_groups  LIMIT 10;
```
|id|industry_group|
|--|--------------|
|1|"Consumer Durables, Household and Personal Products"|
|2|"Food, Beverage & Tobacco"|
|3|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|
|4|"Mining - Iron, Aluminum, Other Metals"|
|5|"Pharmaceuticals, Biotechnology & Life Sciences"|
|6|"Textiles, Apparel, Footwear and Luxury Goods"|
|7|Automobiles & Components|
|8|Capital Goods|
|9|Chemicals|
|10|Commercial & Professional Services|

## 2. Data Explore
Data Duplicate
```SQL
SELECT *,
	COUNT(*) AS duplicate_count
FROM product_emissions pe
GROUP BY 
	id,
	company_id,
	country_id,
	industry_group_id,
	year,
	product_name,
	weight_kg,
	carbon_footprint_pcf,
	upstream_percent_total_pcf,
	operations_percent_total_pcf,
	downstream_percent_total_pcf 
HAVING COUNT(*) > 1
LIMIT 10;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|duplicate_count|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|---------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|2|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|2|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|2|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|2|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|2|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|2|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2014|85|28|11|2014|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2015|85|28|6|2015|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|

Duplicate Result 
```SQL
SELECT COUNT(product_name) AS 'Total number of products',
	COUNT(DISTINCT product_name) AS 'Number of unique products'
FROM product_emissions;
```
|Total number of products|Number of unique products|
|------------------------|-------------------------|
|1037|661|

## 3. Data Analysis
### 3.1. Which products contribute the most to carbon emissions?
```SQL
SELECT  product_name,
	ROUND(AVG(carbon_footprint_pcf),2) AS 'Avarage PCF'
FROM product_emissions
GROUP BY product_name
ORDER BY carbon_footprint_pcf DESC
LIMIT 10;
```

|product_name|Avarage PCF|
|------------|-----------|
|Wind Turbine G128 5 Megawats|3718044.00|
|Wind Turbine G132 5 Megawats|3276187.00|
|Wind Turbine G114 2 Megawats|1532608.00|
|Wind Turbine G90 2 Megawats|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|

Conclusion: The products that contribute the most to carbon emissions are primarily Wind Turbines, with models such as the Wind Turbine G128 5 Megawatts and Wind Turbine G132 5 Megawatts leading the list due to their exceptionally high PCF values. Despite their vital role in renewable energy, these findings emphasize the environmental impact during their production and lifecycle.

### 3.2. What are the industry groups of these products?
```SQL
SELECT  ig.industry_group,
	p.product_name,
	ROUND(AVG(carbon_footprint_pcf),2) AS 'Avarage PCF'
FROM product_emissions AS p
JOIN industry_groups AS ig ON  p.industry_group_id = ig.id
GROUP BY product_name
ORDER BY carbon_footprint_pcf DESC
LIMIT 5;

```
|industry_group|product_name|Avarage PCF|
|--------------|------------|-----------|
|Electrical Equipment and Machinery|Wind Turbine G128 5 Megawats|3718044.00|
|Electrical Equipment and Machinery|Wind Turbine G132 5 Megawats|3276187.00|
|Electrical Equipment and Machinery|Wind Turbine G114 2 Megawats|1532608.00|
|Electrical Equipment and Machinery|Wind Turbine G90 2 Megawats|1251625.00|
|Automobiles & Components|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|

Conclusion: The industry groups of the listed products highlight their diverse sectors. The Wind Turbines (G128, G132, G114, G90) belong to the Electrical Equipment and Machinery industry group, reflecting their role in energy production and machinery. Meanwhile, the Land Cruiser Prado, FJ Cruiser, Dyna Trucks, Toyoace.IMV Def Unit falls under the Automobiles & Components group, emphasizing its alignment with the automotive sector. 

### 3.3. What are the industries with the highest contribution to carbon emissions?
```SQL
SELECT
	ig.industry_group, 
	ROUND(SUM(p.carbon_footprint_pcf),2) AS total_emissions
FROM product_emissions AS p
JOIN industry_groups AS ig ON p.industry_group_id = ig.id
GROUP BY ig.industry_group
ORDER BY total_emissions DESC
LIMIT 1;
```
|industry_group|total_emissions|
|--------------|---------------|
|Electrical Equipment and Machinery|9801558.00|

Conclusion: The Electrical Equipment and Machinery industry stands out as the largest contributor to carbon emissions, with a total of 9,801,558. This highlights its significant environmental impact, emphasizing the need for innovation and sustainable practices within this sector to mitigate its carbon footprint.

### 3.4. What are the companies with the highest contribution to carbon emissions?
```SQL
SELECT 	
	cp.company_name, 
	ROUND(SUM(p.carbon_footprint_pcf),2) AS total_emissions
FROM product_emissions AS p
JOIN companies AS cp ON p.company_id  = cp.id
GROUP BY cp.company_name
ORDER BY total_emissions DESC
LIMIT 1;
```
|company_name|total_emissions|
|------------|---------------|
|"Gamesa Corporación Tecnológica, S.A."|9778464.00|

Conclusion: The company with the highest contribution to carbon emissions is "Gamesa Corporación Tecnológica, S.A.", with a total emission of 9,778,464. This significant figure highlights the environmental impact of its operations and underscores the importance of adopting sustainable practices to mitigate its carbon footprint.

### 3.5. What are the countries with the highest contribution to carbon emissions?
```SQL
SELECT 	
	ct.country_name, 
	ROUND(SUM(p.carbon_footprint_pcf),2) AS total_emissions
FROM product_emissions AS p
JOIN countries AS ct ON p.country_id  = ct.id
GROUP BY ct.country_name
ORDER BY total_emissions DESC
LIMIT 1;
```
|country_name|total_emissions|
|------------|---------------|
|Spain|9786130.00|

Conclusion: The country with the highest contribution to carbon emissions is Spain, with a total emission of 9,778,464. This highlights the significant impact of activities in Spain on the global carbon footprint, emphasizing the importance of exploring sustainable practices in industries operating within the country.

### 3.6. What is the trend of carbon footprints (PCFs) over the years?
```SQL
SELECT 
    p.year, 
    SUM(p.carbon_footprint_pcf) AS total_emissions
FROM product_emissions p
GROUP BY p.year
ORDER BY p.year;
```
|year|total_emissions|
|----|---------------|
|2013|503857|
|2014|624226|
|2015|10840415|
|2016|1640182|
|2017|340271|

Conclusion: Based on data from 2013 to 2017 regarding the total carbon footprints (PCFs), carbon emissions show a fluctuating trend over the years. Notably, 2015 recorded the highest emissions level, reaching 10,840,415, which may be attributed to specific factors or events in production or consumption. After 2015, emissions significantly decreased, especially by 2017 when it dropped to 340,271, reflecting improvements or changes in production processes, consumption, or environmental mitigation measures. This trend highlights the importance of studying the influencing factors to adjust activities and minimize negative impacts on the environment.

### 3.7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
