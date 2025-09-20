---
title: "Growth Miracles: The Great Convergence"
---

# Growth Miracles: The Great Convergence

Which countries have had the most spectacular rise from poverty to prosperity in the modern era? This page uses the Global Macro Database to identify the "growth miracles"—nations that achieved the most rapid and sustained economic development since 1960.

## Identifying the Top Performers

The table below analyzes the long-term growth of all countries that started with a GDP per capita below $5,000 in 1960. It calculates the factor by which their economies have grown ("Growth Multiple") and the compound annual growth rate (CAGR) over the period.

Sort the table to see the top performers.

```sql growth_miracles_data
WITH
  start_year AS (
    SELECT iso3, countryname, rGDP_USD / nullif(pop, 0) as start_gdp_pc
    FROM gmd
    WHERE year = 1960 AND rGDP_USD IS NOT NULL AND pop IS NOT NULL
  ),
  latest_years AS (
    SELECT iso3, max(year) as max_yr
    FROM gmd
    WHERE rGDP_USD IS NOT NULL AND pop IS NOT NULL
    GROUP BY iso3
  ),
  end_year AS (
    SELECT
        g.iso3,
        g.countryname,
        g.year as end_year,
        g.rGDP_USD / nullif(g.pop, 0) as end_gdp_pc
    FROM gmd g
    JOIN latest_years ly ON g.iso3 = ly.iso3 AND g.year = ly.max_yr
  )
SELECT
  s.iso3,
  s.countryname,
  s.start_gdp_pc,
  e.end_gdp_pc,
  e.end_gdp_pc / s.start_gdp_pc AS growth_multiple,
  pow(e.end_gdp_pc / s.start_gdp_pc, 1.0 / (e.end_year - 1960)) - 1 AS cagr
FROM start_year s
JOIN end_year e ON s.iso3 = e.iso3
WHERE s.start_gdp_pc < 5000
ORDER BY growth_multiple DESC
```

<DataTable
    data={growth_miracles_data}
    rows=15
>
    <Column id=countryname title="Country" />
    <Column id=start_gdp_pc title="GDP per Capita (1960)" fmt="$#,##0" />
    <Column id=end_gdp_pc title="GDP per Capita (Latest)" fmt="$#,##0" />
    <Column id=growth_multiple title="Growth Multiple" fmt="0.1x" />
    <Column id=cagr title="Annual Growth (CAGR)" fmt="0.1%" />
</DataTable>

---

## Trajectories of the Top Performers

The chart below visualizes the growth paths of the top 8 countries identified in the table. It dramatically illustrates the "hockey stick" trajectory of rapid, sustained development that defines an economic miracle.

```sql top_performers_paths
WITH
  top_performers AS (
    SELECT iso3 FROM (${growth_miracles_data}) ORDER BY growth_multiple DESC LIMIT 8
  )
SELECT
  g.year,
  g.countryname,
  g.iso3,
  g.rGDP_USD / nullif(g.pop, 0) as rgdp_pc_usd
FROM gmd g
JOIN top_performers tp ON g.iso3 = tp.iso3
WHERE g.year >= 1960
ORDER BY g.year, g.countryname
```

<LineChart
    data={top_performers_paths}
    x=year
    y=rgdp_pc_usd
    series=countryname
    title="The Growth Miracles: GDP per Capita Trajectories of the Top 8 Performers"
    yFmt="$#,##0"
/>
