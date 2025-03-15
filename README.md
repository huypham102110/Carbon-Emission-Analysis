# Carbon Emission Analysis
## 1. Explore date
Table 'product_emissions'
Query
```sql
Select * from product_emissions limit 5;
```
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 

table 'industry_groups'
Query
```sql
Select * from industry_groups limit 5;
```
| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 

Table 'companies'
Query
```sql
Select * from companies limit 5;
```
| id | company_name                  | 
| -: | ----------------------------: | 
| 1  | "Autodesk, Inc."              | 
| 2  | "Casio Computer Co., Ltd."    | 
| 3  | "Cisco Systems, Inc."         | 
| 4  | "CNX Coal Resources, LP"      | 
| 5  | "Coca-Cola Enterprises, Inc." | 

Table 'countries'
Query
```sql
Select * from countries limit 5;
```
| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 

## 2. Carbon Emission Data Analysis
### 1. Which products contribute the most to carbon emissions?
```sql
SELECT product_name, ROUND(AVG(carbon_footprint_pcf),2) AS 'Average PCF'
FROM product_emissions
GROUP BY product_name
ORDER BY AVG(carbon_footprint_pcf) DESC
LIMIT 10;
```
| product_name                                                                                                                       | Average PCF | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ----------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00  | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00  | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00  | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00  | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00   | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00   | 
| TCDE                                                                                                                               | 99075.00    | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00    | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00    | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00    | 

Conclusion: 
Wind turbines (especially large-scale models) have the highest carbon footprint, likely due to manufacturing and material processing.
Automobiles and heavy infrastructure (e.g., retaining walls) also contribute significantly.


### 2. What are the industry groups of these products?
```sql
SELECT product_emissions.product_name, industry_groups.industry_group, ROUND(AVG(product_emissions.carbon_footprint_pcf),2) AS average_PCF
FROM product_emissions
JOIN industry_groups ON product_emissions.industry_group_id = industry_groups.id
GROUP BY product_emissions.product_name, industry_groups.industry_group
ORDER BY average_PCF DESC
LIMIT 10;
```
| product_name                                                                                                                       | industry_group                     | average_PCF | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | ----------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00  | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00  | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00  | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00  | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00   | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00   | 
| TCDE                                                                                                                               | Materials                          | 99075.00    | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00    | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00    | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00    | 

### 3. What are the industries with the highest contribution to carbon emissions?
```sql
SELECT industry_groups.industry_group, ROUND(AVG(product_emissions.carbon_footprint_pcf),2) AS average_PCF
FROM product_emissions
JOIN industry_groups ON product_emissions.industry_group_id = industry_groups.id
GROUP BY industry_groups.industry_group
ORDER BY average_PCF DESC
LIMIT 5;
```
| industry_group                                   | average_PCF | 
| -----------------------------------------------: | ----------: | 
| Electrical Equipment and Machinery               | 891050.73   | 
| Automobiles & Components                         | 35373.48    | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 24162.00    | 
| Capital Goods                                    | 7391.77     | 
| Materials                                        | 3208.86     | 

Conclusion:
Electrical Equipment and Machinery leads in emissions, primarily due to large-scale industrial production.
Automobiles & Components follow, showing how transportation remains a major emitter.


### 4. What are the companies with the highest contribution to carbon emissions?
```sql
SELECT companies.company_name, SUM(product_emissions.carbon_footprint_pcf) AS total_PCF
FROM product_emissions
JOIN companies ON product_emissions.company_id = companies.id
GROUP BY companies.company_name
ORDER BY total_PCF DESC
LIMIT 5;
```
| company_name                            | total_PCF | 
| --------------------------------------: | --------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464   | 
| Daimler AG                              | 1594300   | 
| Volkswagen AG                           | 655960    | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016    | 
| "Hino Motors, Ltd."                     | 191687    | 

Conclusion:
Gamesa Corporación Tecnológica, S.A. (a major wind turbine manufacturer) has the highest emissions.
Daimler AG and Volkswagen AG highlight the impact of the automotive sector.



### 5. What are the countries with the highest contribution to carbon emissions?
```sql
SELECT countries.country_name, SUM(product_emissions.carbon_footprint_pcf) AS total_PCF
FROM product_emissions
JOIN countries ON product_emissions.country_id = countries.id
GROUP BY countries.country_name
ORDER BY total_PCF DESC
LIMIT 5;
```
| country_name | total_PCF | 
| -----------: | --------: | 
| Spain        | 9786130   | 
| Germany      | 2251225   | 
| Japan        | 653237    | 
| USA          | 518381    | 
| South Korea  | 186965    | 

Conclusion:
Spain leads, likely due to large-scale renewable energy projects.
Germany and Japan are significant contributors, given their industrial sectors.


### 6. What is the trend of carbon footprints (PCFs) over the years?
```sql
SELECT year, SUM(carbon_footprint_pcf) AS total_PCF
FROM product_emissions
GROUP BY year
ORDER BY year;
```
| year | total_PCF | 
| ---: | --------: | 
| 2013 | 503857    | 
| 2014 | 624226    | 
| 2015 | 10840415  | 
| 2016 | 1640182   | 
| 2017 | 340271    | 

Conclusion:
A sharp spike in 2015 (due to electrical machinery and automobiles) followed by a decline.


### 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
```sql
SELECT
	industry_groups.industry_group AS 'Industry Group',
	ROUND(AVG(CASE WHEN product_emissions.year = 2013 then product_emissions.carbon_footprint_pcf ELSE 0 END),2) AS '2013 Emission',
	ROUND(AVG(CASE WHEN product_emissions.year = 2014 then product_emissions.carbon_footprint_pcf ELSE 0 END),2) AS '2014 Emission',
	ROUND(AVG(CASE WHEN product_emissions.year = 2015 then product_emissions.carbon_footprint_pcf ELSE 0 END),2) AS '2015 Emission',
	ROUND(AVG(CASE WHEN product_emissions.year = 2016 then product_emissions.carbon_footprint_pcf ELSE 0 END),2) AS '2016 Emission',
	ROUND(AVG(CASE WHEN product_emissions.year = 2017 then product_emissions.carbon_footprint_pcf ELSE 0 END),2) AS '2017 Emission'
FROM
	product_emissions
LEFT JOIN
	industry_groups ON product_emissions.industry_group_id = industry_groups.id
GROUP BY
	industry_groups.industry_group
ORDER BY
	industry_groups.industry_group;
```
| Industry Group                                                         | 2013 Emission | 2014 Emission | 2015 Emission | 2016 Emission | 2017 Emission | 
| ---------------------------------------------------------------------: | ------------: | ------------: | ------------: | ------------: | ------------: | 
| "Consumer Durables, Household and Personal Products"                   | 0.00          | 0.00          | 116.38        | 0.00          | 0.00          | 
| "Food, Beverage & Tobacco"                                             | 39.02         | 20.98         | 0.00          | 783.51        | 24.70         | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 0.00          | 0.00          | 685.31        | 0.00          | 0.00          | 
| "Mining - Iron, Aluminum, Other Metals"                                | 0.00          | 0.00          | 2727.00       | 0.00          | 0.00          | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 10757.00      | 13405.00      | 0.00          | 0.00          | 0.00          | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 0.00          | 0.00          | 14.33         | 0.00          | 0.00          | 
| Automobiles & Components                                               | 1783.41       | 3150.89       | 11194.89      | 19244.29      | 0.00          | 
| Capital Goods                                                          | 1719.71       | 2677.11       | 100.14        | 181.97        | 2712.83       | 
| Chemicals                                                              | 0.00          | 0.00          | 1949.03       | 0.00          | 0.00          | 
| Commercial & Professional Services                                     | 26.30         | 10.84         | 0.00          | 65.68         | 16.84         | 
| Consumer Durables & Apparel                                            | 42.16         | 48.24         | 0.00          | 17.09         | 0.00          | 
| Containers & Packaging                                                 | 0.00          | 0.00          | 373.50        | 0.00          | 0.00          | 
| Electrical Equipment and Machinery                                     | 0.00          | 0.00          | 891050.73     | 0.00          | 0.00          | 
| Energy                                                                 | 150.00        | 0.00          | 0.00          | 2004.80       | 0.00          | 
| Food & Beverage Processing                                             | 0.00          | 0.00          | 7.05          | 0.00          | 0.00          | 
| Food & Staples Retailing                                               | 0.00          | 32.21         | 29.42         | 0.08          | 0.00          | 
| Gas Utilities                                                          | 0.00          | 0.00          | 61.00         | 0.00          | 0.00          | 
| Household & Personal Products                                          | 0.00          | 0.00          | 0.00          | 0.00          | 0.00          | 
| Materials                                                              | 1113.96       | 420.43        | 0.00          | 490.37        | 1184.09       | 
| Media                                                                  | 643.00        | 643.00        | 127.93        | 120.53        | 0.00          | 
| Retailing                                                              | 0.00          | 3.80          | 2.20          | 0.00          | 0.00          | 
| Semiconductors & Semiconductor Equipment                               | 0.00          | 10.00         | 0.00          | 0.80          | 0.00          | 
| Semiconductors & Semiconductors Equipment                              | 0.00          | 0.00          | 1.00          | 0.00          | 0.00          | 
| Software & Services                                                    | 0.18          | 4.29          | 672.24        | 671.94        | 20.29         | 
| Technology Hardware & Equipment                                        | 228.84        | 626.82        | 397.59        | 5.87          | 103.34        | 
| Telecommunication Services                                             | 5.78          | 20.33         | 20.33         | 0.00          | 0.00          | 
| Tires                                                                  | 0.00          | 0.00          | 1011.00       | 0.00          | 0.00          | 
| Tobacco                                                                | 0.00          | 0.00          | 1.00          | 0.00          | 0.00          | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 0.00          | 0.00          | 39.83         | 0.00          | 0.00          | 
| Utilities                                                              | 30.50         | 0.00          | 0.00          | 30.50         | 0.00          | 

Conclusion:
Some industries show a downward trend, possibly due to regulatory changes or improved efficiency.
