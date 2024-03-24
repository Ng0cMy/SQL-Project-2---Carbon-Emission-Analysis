# SQL-Project-2-Carbon-Emission-Analysis
Finish SQL module

## 1. Which products contribute the most to carbon emissions?

### Wind turbines contribute the most to carbon emissions.
```
SELECT pe.product_name, pe.carbon_footprint_pcf
FROM carbon_emissions.product_emissions pe 
ORDER BY pe.carbon_footprint_pcf desc
LIMIT 10;
```

| product_name                                                                                                                       | carbon_footprint_pcf | 
| ---------------------------------------------------------------------------------------------------------------------------------: | -------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044              | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187              | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608              | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625              | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687               | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000               | 
| TCDE                                                                                                                               | 99075                | 
| TCDE                                                                                                                               | 99075                | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000                | 
| Electric Motor                                                                                                                     | 87589                |                                             |         


## 2. What are the industry groups of these products?

### The industries producing the TOP 10 products contributing the most to carbon emissions are “Electrical Equipment and Machinery”, “Automobiles & Components”, “Materials” and “Capital Goods”.

Describe what did I do...
```
SELECT distinct ab.industry_group
FROM (
SELECT  ig.industry_group,pe.product_name, pe.carbon_footprint_pcf
FROM carbon_emissions.product_emissions pe 
INNER JOIN industry_groups ig 
	ON pe.industry_group_id =ig.id
ORDER BY pe.carbon_footprint_pcf desc
LIMIT 10) as ab;
```

| industry_group                     | 
| ---------------------------------: | 
| Electrical Equipment and Machinery | 
| Automobiles & Components           | 
| Materials                          | 
| Capital Goods                      |   


## 3. What are the industries with the highest contribution to carbon emissions?

### 
Describe what did you do...
```
SELECT ig.industry_group, sum(carbon_footprint_pcf) as "total_carbon_footprint_pcf"
FROM carbon_emissions.product_emissions pe 
	LEFT JOIN industry_groups ig 
	ON pe.industry_group_id =ig.id
GROUP BY ig.industry_group
ORDER BY total_carbon_footprint_pcf desc
LIMIT 10;
```

| industry_group                                   | total_carbon_footprint_pcf | 
| -----------------------------------------------: | -------------------------: | 
| Electrical Equipment and Machinery               | 9801558                    | 
| Automobiles & Components                         | 2582264                    | 
| Materials                                        | 577595                     | 
| Technology Hardware & Equipment                  | 363776                     | 
| Capital Goods                                    | 258712                     | 
| "Food, Beverage & Tobacco"                       | 111131                     | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486                      | 
| Chemicals                                        | 62369                      | 
| Software & Services                              | 46544                      | 
| Media                                            | 23017                      |                       


## 4. What are the companies with the highest contribution to carbon emissions?

```
SELECT c.company_name, sum(carbon_footprint_pcf) as "total_carbon_footprint_pcf"
FROM carbon_emissions.product_emissions pe 
	LEFT JOIN carbon_emissions.companies c 
	ON pe.company_id =c.id 
WHERE pe.operations_percent_total_pcf NOT LIKE "%N/a%"
GROUP BY c.company_name
ORDER BY total_carbon_footprint_pcf desc
LIMIT 5;
```

| company_name                            | total_carbon_footprint_pcf | 
| --------------------------------------: | -------------------------: | 
| "Mitsubishi Gas Chemical Company, Inc." | 199078                     | 
| "Hino Motors, Ltd."                     | 191687                     | 
| Weg S/A                                 | 160655                     | 
| "Daikin Industries, Ltd."               | 105600                     | 
| Volkswagen AG                           | 102601                     |      

## 5. What is the trend of carbon footprints (PCFs) over the years?

Describe what did you do...
```
SELECT c2.country_name , sum(carbon_footprint_pcf) as "total_carbon_footprint_pcf"
FROM carbon_emissions.product_emissions pe 
	LEFT JOIN carbon_emissions.countries c2 
	ON pe.country_id = c2.id 
GROUP BY c2.country_name
ORDER BY total_carbon_footprint_pcf DESC 
LIMIT 5;

```

| country_name | total_carbon_footprint_pcf | 
| -----------: | -------------------------: | 
| Spain        | 9786130                    | 
| Germany      | 2251225                    | 
| Japan        | 653237                     | 
| USA          | 518381                     | 
| South Korea  | 186965                     |         

## 6. What is the trend of carbon footprints (PCFs) over the years?

```
SELECT pe.year , ROUND(AVG(carbon_footprint_pcf),2) as "avg_carbon_footprint_pcf" 
FROM product_emissions pe
GROUP BY pe.year;
```

| year | avg_carbon_footprint_pcf | 
| ---: | -----------------------: | 
| 2013 | 2399.32                  | 
| 2014 | 2457.58                  | 
| 2015 | 43188.90                 | 
| 2016 | 6891.52                  | 
| 2017 | 4050.85                  |   

![image](https://github.com/Ng0cMy/SQL-Project-2---Carbon-Emission-Analysis/assets/162866097/2bc6e9af-b4aa-41f7-97d8-1dcf52cddb7d)


### Carbon footprints (PCFs) have gradually increased since 2013 and peaked in 2015 at 43188.90, then gradually decreased in the following years.

## 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
    

