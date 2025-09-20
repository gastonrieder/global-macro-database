---
title: Global Convergence
---

# 🌍 Global Convergence

This page uses the Global Macro Database (GMD) to explore the story:
- Poverty has declined worldwide
- Inequality between countries is narrowing
- Inequality within countries is rising (external data needed)

---

## GDP per Capita Over Time

```sql gdp_by_country
select
  year,
  countryname,
  sum(rGDP_USD) / sum(pop) as rGDP_pc
from gmd
where iso3 in ('USA','CHN','IND','BRA','NGA', 'DEU','JPN','RUS','GBR','FRA')
  and year BETWEEN 1960 AND 2024
GROUP BY year, countryname
order by year, countryname
```

<LineChart
  data={gdp_by_country}
  x=year
  y=rGDP_pc
  series=countryname
  title="GDP per Capita (1960-2020)"
  xAxisTitle="Year"
  yAxisTitle="GDP per Capita (USD, constant)"
  yFmt="#,##0"
/>

---
```sql gdp_total
select
  year,
  sum(rGDP_USD) / nullif(sum(pop), 0) as rGDP_pc
from gmd
where iso3 in ('USA','CHN','IND','BRA','NGA', 'DEU','JPN','RUS','GBR','FRA')
  and year >= 1960
group by year
order by year
```

<LineChart
  data={gdp_total}
  x=year
  y=rGDP_pc
  title="GDP per Capita (1960-2020)"
  xAxisTitle="Year"
  yAxisTitle="GDP per Capita (USD, constant)"
  yFmt="#,##0"
/>

---

## Weighted vs. Unweighted World Averages

```sql gdp_world
select
  year,
  sum(rGDP_USD) / nullif(sum(pop), 0) as weighted_avg,
  avg(rGDP_USD / nullif(pop, 0)) as unweighted_avg
from gmd
where year between 1960 and 2020
group by year
order by year
```

<LineChart
  data={gdp_world}
  x=year
  y=weighted_avg
  title="World GDP per Capita: Weighted vs. Unweighted"
  xAxisTitle="Year"
  yAxisTitle="GDP per Capita (USD, constant)"
  yFmt="#,##0"
/>

<LineChart
  data={gdp_world}
  x=year
  y=unweighted_avg
  title="World GDP per Capita: Weighted vs. Unweighted"
  xAxisTitle="Year"
  yAxisTitle="GDP per Capita (USD, constant)"
  yFmt="#,##0"
/>
---

## Cross-country Inequality

```sql ratio
select
  year,
  max(rGDP_USD / nullif(pop, 0)) / nullif(min(rGDP_USD / nullif(pop, 0)), 0) as richest_to_poorest
from gmd
where year between 1960 and 2020
group by year
order by year
```

<LineChart
  data={ratio}
  x=year
  y=richest_to_poorest
  title="Richest-to-Poorest Country Ratio"
  xAxisTitle="Year"
  yAxisTitle="Income Ratio"
/>

---

## ✨ Takeaway

- Incomes are rising globally.
- Cross-country inequality is falling, as emerging economies catch up.
- Within-country inequality still rising — external data (e.g., World Bank, WID) completes the story.

<!--
Notes:
- Column names confirmed from schema: countryname, ISO3, year, rGDP_pc, pop.
- DuckDB identifiers are case-insensitive unless quoted, so iso3 works with ISO3.
- Each chart references its corresponding query name.
-->
