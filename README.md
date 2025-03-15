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
### Which products contribute the most to carbon emissions?
```sql
SELECT product_name, ROUND(AVG(carbon_footprint_pcf),2) AS 'Average PCF'
FROM product_emissions
GROUP BY product_name
ORDER BY AVG(carbon_footprint_pcf) DESC
LIMIT 5;
```
| product_name                                                       | Average PCF | 
| -----------------------------------------------------------------: | ----------: | 
| Wind Turbine G128 5 Megawats                                       | 3718044.00  | 
| Wind Turbine G132 5 Megawats                                       | 3276187.00  | 
| Wind Turbine G114 2 Megawats                                       | 1532608.00  | 
| Wind Turbine G90 2 Megawats                                        | 1251625.00  | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit. | 191687.00   | 
