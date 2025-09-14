# SQL Project: Analyzing the Link Between Global Deforestation and CO₂ Emissions (1990-2020)

## Project Objective
This project uses SQL to analyze global datasets on forest area and CO₂ emissions. The objective is to quantify forest loss in key countries and investigate the correlation between deforestation and the rise in carbon emissions over a 30-year period.

---

## Key SQL Queries
The analysis was conducted using a series of SQL queries to progress from a broad overview to specific, joined insights.

### Query 1: Calculating Absolute Forest Loss
This query identifies the total square kilometers of forest area lost by each country between 1990 and 2020.
```sql
SELECT
    country_name,
    (SUM(CASE WHEN year = 1990 THEN forest_sq_km ELSE 0 END) - SUM(CASE WHEN year = 2020 THEN forest_sq_km ELSE 0 END)) AS loss_sq_km
FROM
    forest_area
GROUP BY
    country_name
ORDER BY
    loss_sq_km DESC;
```

### Query 2: Calculating Percentage Forest Loss
To provide a more robust analytical view, this query calculates the forest loss as a percentage of each country's 1990 forest area.
```sql
SELECT
    country_name,
    ((SUM(CASE WHEN year = 1990 THEN forest_sq_km ELSE 0 END) - SUM(CASE WHEN year = 2020 THEN forest_sq_km ELSE 0 END)) / SUM(CASE WHEN year = 1990 THEN forest_sq_km ELSE 0 END)) * 100 AS percentage_loss
FROM
    forest_area
GROUP BY
    country_name
ORDER BY
    percentage_loss DESC;
```

### Query 3: Joining Forest & Emissions Data for a Comprehensive View
The final query joins the forest area and CO₂ emissions tables to directly compare the change in forest area with the change in emissions for each country.
```sql
SELECT
    fa.country_name,
    (SUM(CASE WHEN fa.year = 2020 THEN fa.forest_sq_km ELSE 0 END) - SUM(CASE WHEN fa.year = 1990 THEN fa.forest_sq_km ELSE 0 END)) AS forest_change_sq_km,
    (SUM(CASE WHEN ce.year = 2020 THEN ce.emissions_tonnes ELSE 0 END) - SUM(CASE WHEN ce.year = 1990 THEN ce.emissions_tonnes ELSE 0 END)) AS emissions_change_tonnes
FROM
    forest_area AS fa
JOIN
    co2_emissions AS ce ON fa.country_name = ce.country_name AND fa.year = ce.year
GROUP BY
    fa.country_name
ORDER BY
    forest_change_sq_km ASC;
```
---

## Results & Insights
The final query produced the following table, directly linking deforestation to changes in CO₂ emissions:

| Country Name | Forest Change (sq km) | Emissions Change (tonnes) |
| :--- | :--- | :--- |
| Brazil | -500,854 | +253,000,000 |
| DR Congo | -357,180 | +3,000,000 |
| Indonesia | -264,118 | +433,000,000 |
| Nigeria | -95,130 | +63,000,000 |
| Canada | -13,449 | +115,000,000 |
| Russia | +3,116 | -705,000,000 |
| Pakistan | +3,690 | +147,000,000 |
| India | +82,210 | +1,836,000,000 |

### Key Insights:
1.  **Strong Correlation Identified:** The analysis reveals a strong trend where countries experiencing significant forest loss (e.g., **Brazil, Indonesia, DR Congo**) also saw substantial increases in their CO₂ emissions between 1990 and 2020.
2.  **Deforestation as an Emission Driver:** Indonesia, for example, lost over 264,000 sq km of forest while its emissions grew by over 433 million tonnes, highlighting the critical role of deforestation in climate change.
3.  **Complex Factors:** The data also shows that forest area is not the only factor. Countries like India and Pakistan experienced an increase in forest area but also saw large emissions growth, likely due to industrialization and economic development. This demonstrates the complexity of climate data and the need for nuanced, multi-faceted analysis.

---
This project demonstrates a complete analytical workflow using SQL to derive meaningful insights from raw environmental data.
