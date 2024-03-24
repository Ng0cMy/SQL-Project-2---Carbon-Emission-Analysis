# SQL-Project-2-Carbon-Emission-Analysis
Finish SQL module

## Wind turbines contribute the most to carbon emissions.
SQL Code description:
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


## The industries producing the TOP 10 products contributing the most to carbon emissions are “Electrical Equipment and Machinery”, “Automobiles & Components”, “Materials” and “Capital Goods”.

SQL Code description:
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


## The industry that contributes the most to carbon emissions is "Electrical Equipment and Machinery". This is because the production of electrical equipment and machinery consumes a lot of energy, including the use of furnaces, welding, machining, etc., which increases electricity consumption and carbon dioxide emissions.

SQL Code description:
```
SELECT ig.industry_group, sum(carbon_footprint_pcf) as "total_pcf"
FROM carbon_emissions.product_emissions pe 
	INNER JOIN industry_groups ig 
	ON pe.industry_group_id =ig.id
GROUP BY ig.industry_group
ORDER BY total_pcf desc
LIMIT 10;
```

| industry_group                                   | total_pcf | 
| -----------------------------------------------: | --------: | 
| Electrical Equipment and Machinery               | 9801558   | 
| Automobiles & Components                         | 2582264   | 
| Materials                                        | 577595    | 
| Technology Hardware & Equipment                  | 363776    | 
| Capital Goods                                    | 258712    | 
| "Food, Beverage & Tobacco"                       | 111131    | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486     | 
| Chemicals                                        | 62369     | 
| Software & Services                              | 46544     | 
| Media                                            | 23017     |                

## "Gamesa Corporación Tecnológica, S.A." is the largest contributor to carbon emissions in the "Electrical Equipment and Machinery" industry.

SQL Code description:
```
SELECT  ig.industry_group,c.company_name, sum(carbon_footprint_pcf) as "total_pcf"
FROM carbon_emissions.product_emissions pe 
	LEFT JOIN carbon_emissions.companies c 
	ON pe.company_id =c.id
	INNER JOIN industry_groups ig 
	ON pe.industry_group_id =ig.id
GROUP BY c.company_name
ORDER BY total_pcf desc
LIMIT 10;
```

| industry_group                     | company_name                            | total_pcf | 
| ---------------------------------: | --------------------------------------: | --------: | 
| Electrical Equipment and Machinery | "Gamesa Corporación Tecnológica, S.A."  | 9778464   | 
| Automobiles & Components           | Daimler AG                              | 1594300   | 
| Automobiles & Components           | Volkswagen AG                           | 655960    | 
| Materials                          | "Mitsubishi Gas Chemical Company, Inc." | 212016    | 
| Automobiles & Components           | "Hino Motors, Ltd."                     | 191687    | 
| Materials                          | Arcelor Mittal                          | 167007    | 
| Capital Goods                      | Weg S/A                                 | 160655    | 
| Automobiles & Components           | General Motors Company                  | 137007    | 
| Technology Hardware & Equipment    | "Lexmark International, Inc."           | 132012    | 
| Capital Goods                      | "Daikin Industries, Ltd."               | 105600    |                 

## The TOP 10 countries contributing the most to carbon emissions are listed in the table below. Among them, Spain is the country with the highest carbon emissions.

SQL Code description:
```
SELECT c2.country_name , sum(carbon_footprint_pcf) as "total_pcf"
FROM carbon_emissions.product_emissions pe 
	LEFT JOIN carbon_emissions.countries c2 
	ON pe.country_id = c2.id 
GROUP BY c2.country_name
ORDER BY total_pcf DESC 
LIMIT 10;
```

| country_name | total_pcf | 
| -----------: | --------: | 
| Spain        | 9786130   | 
| Germany      | 2251225   | 
| Japan        | 653237    | 
| USA          | 518381    | 
| South Korea  | 186965    | 
| Brazil       | 169337    | 
| Luxembourg   | 167007    | 
| Netherlands  | 70417     | 
| Taiwan       | 62875     | 
| India        | 24574     |                 

## Carbon footprints (PCFs) have gradually increased since 2013 and peaked in 2015 at 43188.90, then gradually decreased in the following years.
![image](https://github.com/Ng0cMy/SQL-Project-2---Carbon-Emission-Analysis/assets/162866097/2bc6e9af-b4aa-41f7-97d8-1dcf52cddb7d)

SQL Code description:
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

Copy this table into Excel to create a chart, then copy the chart to GitHub.

## The industry groups with low or almost no PCF volume such as "Textiles, Apparel",  "Footwear and Luxury Goods", "Gas Utilities", "Household & Personal Products", "Retailing",  "Semiconductors & Semiconductor Equipment", "Tobacco", and "Utilities".

## Industry groups have shown a clear decrease in PCF over the past 5 years, notably in media. Additionally, there has been a sharp decrease in the last 2 years in industries like ‘Food, Beverage & Tobacco’, ‘Commercial & Professional Services’, and ‘Software & Services’.

SQL Code description:
```
SELECT  ig.industry_group, pe.year , SUM(pe.carbon_footprint_pcf) as total_pcf  
FROM product_emissions pe 
INNER JOIN industry_groups ig 
ON pe.industry_group_id =ig.id 
GROUP BY ig.industry_group, pe.year 
ORDER BY ig.industry_group, pe.year, pe.carbon_footprint_pcf ;
```

| industry_group                                                         | year | total_pcf | 
| ---------------------------------------------------------------------: | ---: | --------: | 
| "Consumer Durables, Household and Personal Products"                   | 2015 | 931       | 
| "Food, Beverage & Tobacco"                                             | 2013 | 4995      | 
| "Food, Beverage & Tobacco"                                             | 2014 | 2685      | 
| "Food, Beverage & Tobacco"                                             | 2015 | 0         | 
| "Food, Beverage & Tobacco"                                             | 2016 | 100289    | 
| "Food, Beverage & Tobacco"                                             | 2017 | 3162      | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 2015 | 8909      | 
| "Mining - Iron, Aluminum, Other Metals"                                | 2015 | 8181      | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2013 | 32271     | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2014 | 40215     | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 2015 | 387       | 
| Automobiles & Components                                               | 2013 | 130189    | 
| Automobiles & Components                                               | 2014 | 230015    | 
| Automobiles & Components                                               | 2015 | 817227    | 
| Automobiles & Components                                               | 2016 | 1404833   | 
| Capital Goods                                                          | 2013 | 60190     | 
| Capital Goods                                                          | 2014 | 93699     | 
| Capital Goods                                                          | 2015 | 3505      | 
| Capital Goods                                                          | 2016 | 6369      | 
| Capital Goods                                                          | 2017 | 94949     | 
| Chemicals                                                              | 2015 | 62369     | 
| Commercial & Professional Services                                     | 2013 | 1157      | 
| Commercial & Professional Services                                     | 2014 | 477       | 
| Commercial & Professional Services                                     | 2016 | 2890      | 
| Commercial & Professional Services                                     | 2017 | 741       | 
| Consumer Durables & Apparel                                            | 2013 | 2867      | 
| Consumer Durables & Apparel                                            | 2014 | 3280      | 
| Consumer Durables & Apparel                                            | 2016 | 1162      | 
| Containers & Packaging                                                 | 2015 | 2988      | 
| Electrical Equipment and Machinery                                     | 2015 | 9801558   | 
| Energy                                                                 | 2013 | 750       | 
| Energy                                                                 | 2016 | 10024     | 
| Food & Beverage Processing                                             | 2015 | 141       | 
| Food & Staples Retailing                                               | 2014 | 773       | 
| Food & Staples Retailing                                               | 2015 | 706       | 
| Food & Staples Retailing                                               | 2016 | 2         | 
| Gas Utilities                                                          | 2015 | 122       | 
| Household & Personal Products                                          | 2013 | 0         | 
| Materials                                                              | 2013 | 200513    | 
| Materials                                                              | 2014 | 75678     | 
| Materials                                                              | 2016 | 88267     | 
| Materials                                                              | 2017 | 213137    | 
| Media                                                                  | 2013 | 9645      | 
| Media                                                                  | 2014 | 9645      | 
| Media                                                                  | 2015 | 1919      | 
| Media                                                                  | 2016 | 1808      | 
| Retailing                                                              | 2014 | 19        | 
| Retailing                                                              | 2015 | 11        | 
| Semiconductors & Semiconductor Equipment                               | 2014 | 50        | 
| Semiconductors & Semiconductor Equipment                               | 2016 | 4         | 
| Semiconductors & Semiconductors Equipment                              | 2015 | 3         | 
| Software & Services                                                    | 2013 | 6         | 
| Software & Services                                                    | 2014 | 146       | 
| Software & Services                                                    | 2015 | 22856     | 
| Software & Services                                                    | 2016 | 22846     | 
| Software & Services                                                    | 2017 | 690       | 
| Technology Hardware & Equipment                                        | 2013 | 61100     | 
| Technology Hardware & Equipment                                        | 2014 | 167361    | 
| Technology Hardware & Equipment                                        | 2015 | 106157    | 
| Technology Hardware & Equipment                                        | 2016 | 1566      | 
| Technology Hardware & Equipment                                        | 2017 | 27592     | 
| Telecommunication Services                                             | 2013 | 52        | 
| Telecommunication Services                                             | 2014 | 183       | 
| Telecommunication Services                                             | 2015 | 183       | 
| Tires                                                                  | 2015 | 2022      | 
| Tobacco                                                                | 2015 | 1         | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 2015 | 239       | 
| Utilities                                                              | 2013 | 122       | 
| Utilities                                                              | 2016 | 122       |         

Convert this table into Excel to analyze which industry groups have demonstrated the most notable decrease in carbon footprints (PCFs) over time.
Follow this sample:
| industry_group                                        | 2013 | 2014 | 2015 | 2016 | 2017 | 
|-----------------------------------------------------: | ---: | ---: | ---: | ---: | ---: |
| "Consumer Durables, Household and Personal Products": |      |      |  931 |      |      |
|"Food, Beverage & Tobacco"                             | 4995 | 2685 |      |100289| 3162 |
|.....                                                  | .... | .... | .... | .... | .... |
