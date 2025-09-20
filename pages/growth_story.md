---
title: "Growth in all its forms"
---
# Executive summary

To be completed later

# 1. Most countries are stuck in their development paths - can they escape?

The development gap between countries is stark. Can governments overcome it? Latin Americans often see the disparity between developed and developing countries as a structural irreversible process. 

If they limit the comparison to Latin America and Western countries, this is true. 

```sql europe_latam_gdp
SELECT
  year,
  SUM(rGDP_USD) / SUM(pop) as rgdp_pc_usd,
  CASE
    WHEN iso3 IN ('GBR', 'FRA', 'ITA', 'ESP', 'USA') THEN 'Western Europe+USA'
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
  END as region
FROM gmd
WHERE year BETWEEN 1900 AND 2025
  AND iso3 IN ('GBR', 'FRA', 'ARG', 'BRA', 'ITA', 'ESP', 'USA')
GROUP BY ALL
ORDER BY year
```

<LineChart
    data={europe_latam_gdp}
    x=year
    y=rgdp_pc_usd
    series=region
    title='125 years later, still a persistent gap in GDP per capita'
    subtitle="GDP per Capita: Europe vs. Latin America (1900-Present)"
    yFmt="$#,##0"
    downloadableData=false
    downloadableImage=false
    >
    <ReferenceArea 
      xMin='1900' 
      xMax='1910' 
      label='GDP per capita gap: 5:1' 
      labelPosition='center'
      color=info/>
</LineChart>

maybe a summary table here??? 

```sql europe_latam_ea_gdp
SELECT
  year,
  --countryname,
  SUM(rGDP_USD) / SUM(pop) as rgdp_pc_usd,
  CASE
    WHEN iso3 IN ('GBR', 'FRA', 'ITA', 'ESP') THEN 'Western Europe'
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region
FROM gmd
WHERE year >= 1900
  AND iso3 IN ('GBR', 'FRA', 'ARG', 'BRA', 'ITA', 'ESP', 'JPN', 'KOR')
GROUP BY ALL
ORDER BY year
```

<LineChart
    data={europe_latam_ea_gdp}
    x=year
    y=rgdp_pc_usd
    series=region
    title='Korea and Japan buck the trend, catching up with the West in 3-4 decades'
    subtitle="GDP per Capita: East Asia vs Western Europe vs. Latin America (1900-Present)"
    yFmt="$#,##0"
    colorPalette={['#afb1b5ff','#afb1b5ff','#2563eb']}
    downloadableData=false
    downloadableImage=false
    >
  <ReferenceArea xMin='1950' xMax='1990' color=info/>

</LineChart>

---

# 2. How did South Korea and Japan succeed while Argentina and Brazil did not? (1950s–1990s)

List hypotheses here and then we validate 

1. **Openness to trade**: South Korea and Japan embraced open trade policies, while Argentina and Brazil remained protectionist. The divergence accelerated in the following decades. South Korea pursued an aggressive export-oriented strategy, coupled with high rates of investment. In contrast, the Latin American economies generally focused more on domestic consumption, often punctuated by cycles of import substitution, and were hit harder by external shocks like the 1970s oil crisis and the 1980s debt crisis.
2. **Sustainable allocation of state resources**: Japan and South Korea effectively mobilized state resources to support industrial policy and infrastructure development, while Argentina and Brazil often misallocated resources, leading to inefficiencies and corruption.
3. **Political stability and governance**: South Korea and Japan had more stable political environments and effective governance, while Argentina and Brazil faced political turmoil and corruption.

## Openness to trade

```sql regional_comparison_external_sector
SELECT
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(exports / USDfx) / SUM(nGDP / USDfx) as exports_GDP,
  SUM(imports / USDfx) / SUM(nGDP / USDfx) as imports_GDP,
  SUM(CA / USDfx) / SUM(nGDP / USDfx) as CA_GDP,
  (SUM(exports / USDfx) + SUM(imports / USDfx)) / SUM(nGDP / USDfx) as trade_openness
FROM gmd
WHERE iso3 IN ('JPN', 'KOR', 'BRA', 'ARG') AND year BETWEEN 1950 AND 1990
GROUP BY ALL
ORDER BY year, region
```

<LineChart 
    data={regional_comparison_external_sector} 
    x=year 
    y=trade_openness 
    series=region 
    title='Japan and Korea embraced open trade, while Argentina and Brazil remained stuck in protectionism'
    subtitle="Trade Openness (Exports + Imports) / GDP)" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

<LineChart 
    data={regional_comparison_external_sector} 
    x=year 
    y=CA_GDP 
    series=region 
    title="As a result, Japan and Korea developed more competitive economies, leading to recurrent account surpluses"
    subtitle="Current account balance as a share of GDP, 1950–1990" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

## Equally expanding states, but with different goals in mind 

```sql regional_comparison_gov_finance
SELECT
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(govdebt / USDfx) / SUM(nGDP / USDfx) as govdebt_GDP,
  SUM(govrev / USDfx) / SUM(nGDP / USDfx) as govrev_GDP,
  SUM(govtax / USDfx) / SUM(nGDP / USDfx) as govtax_GDP,
  SUM(govdef / USDfx) / SUM(nGDP / USDfx) as govdef_GDP,
  SUM(govexp / USDfx) / SUM(nGDP / USDfx) as govexp_GDP
FROM gmd
WHERE iso3 IN ('JPN', 'KOR', 'BRA', 'ARG') AND year BETWEEN 1950 AND 1990
GROUP BY ALL
ORDER BY year, region
```

<LineChart 
    data={regional_comparison_gov_finance} 
    x=year 
    y=govexp_GDP 
    series=region 
    title='At first glance, both regions have had similar levels of government expenditure'
    subtitle="Government Expenditure as a Share of GDP, 1950–1990" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>


```sql regional_comparison_investment
select
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(inv / USDfx) / SUM(nGDP / USDfx) as inv_GDP,
  SUM(finv / USDfx) / SUM(nGDP / USDfx) as finv_GDP
from gmd
where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year between 1950 and 1990
GROUP BY ALL
order by year, region
```

<Grid cols=2>
<Group>
However, from the outset Japan and Korea invested heavily in their economies
<LineChart 
    data={regional_comparison_investment} 
    x=year 
    y=inv_GDP 
    series=region 
    subtitle="Investment as a Share of GDP, 1950–1990" 
    yFmt="0.0%"
    yMax=0.5
    downloadableData=false
    downloadableImage=false
/>
</Group>
<Group>
  The gap is significant for fixed investments, which reflect long-term capital formation
<LineChart 
    data={regional_comparison_investment} 
    x=year 
    y=finv_GDP 
    series=region 
    subtitle="Fixed Direct Investment as a Share of GDP, 1950–2025" 
    yFmt="0.0%"
    yMax=0.5
    downloadableData=false
    downloadableImage=false
/>    </Group>

</Grid>


```sql regional_comparison_consumption
select
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(cons_GDP * rGDP_USD) / SUM(rGDP_USD) / 100 as cons_GDP
from gmd
where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year between 1960 and 1990
GROUP BY ALL
order by year, region
```

<LineChart 
    data={regional_comparison_consumption} 
    x=year 
    y=cons_GDP 
    series=region 
    title="Instead, Argentina and Brazil focused on consumption, which is unsustainable in the long run"
    subtitle="Consumption as a Share of GDP, 1960-1990 (data only available from 1960)" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

```sql regional_comparison_govdebt
select
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(govdebt_GDP * rGDP_USD) / SUM(rGDP_USD) / 100 as govdebt_GDP
from gmd
where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year between 1950 and 1990
GROUP BY ALL
order by year, region
```

<LineChart 
    data={regional_comparison_govdebt} 
    x=year 
    y=govdebt_GDP 
    series=region 
    title="Government Debt as a Share of GDP, 1950–2000" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

```sql regional_comparison_m2
with country_growth as (
    select
      year,
      iso3,
      rGDP_USD,
      M2 / lag(M2) over (partition by iso3 order by year) - 1 as m2_growth
    from gmd
    where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year between 1950 and 1990
)
select
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(m2_growth * rGDP_USD) / SUM(rGDP_USD) / 100as m2_growth
from country_growth
where m2_growth is not null
group by year, region
order by year, region
```

<LineChart 
    data={regional_comparison_m2} 
    x=year 
    y=m2_growth 
    series=region 
    title="M2 Money Supply Growth (YoY), 1950–2025" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

```sql regional_comparison_reer
select
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(REER * rGDP_USD) / SUM(rGDP_USD) as REER
from gmd
where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year between 1950 and 1990
group by year, region
order by year, region
```

<LineChart 
    data={regional_comparison_reer} 
    x=year 
    y=REER 
    series=region 
    title="Real Effective Exchange Rate (REER), 1950–2025" 
    yFmt="0.0"
    downloadableData=false
    downloadableImage=false
/>

### Monetary Stability Hypothesis

Expectation: LatAm had high, volatile infl and erratic interest rates. East Asia had low, stable inflation.

```sql inflation_comparison_countries
SELECT
    year,
    countryname,
    (CASE
        WHEN infl <= 0 THEN 0.001
        ELSE infl
    END) / 100 as infl
FROM gmd
WHERE iso3 IN ('ARG', 'BRA', 'JPN', 'KOR') 
AND year BETWEEN 1950 AND 1990
ORDER BY year, countryname
```

<LineChart
    data={inflation_comparison_countries}
    x=year
    y=infl
    series=countryname
    title="Inflation (CPI, YoY)"
    yFmt="0.0%"
    ylog=true
    downloadableData=false
    downloadableImage=false
/>

```sql inflation_volatility
SELECT
    countryname,
    STDDEV_SAMP(infl) as inflation_volatility
FROM gmd
WHERE iso3 IN ('ARG', 'BRA', 'JPN', 'KOR') AND year BETWEEN 1960 AND 1990
GROUP BY countryname
ORDER BY inflation_volatility DESC
```

<BarChart
    data={inflation_volatility}
    x=countryname
    y=inflation_volatility
    title="Inflation Volatility (Std. Dev. of YoY Inflation, 1960-1990)"
/>

```sql interest_rate_comparison
SELECT
    year,
    countryname,
    strate / 100 as strate
FROM gmd
WHERE iso3 IN ('ARG', 'BRA', 'JPN', 'KOR') AND year >= 1960
ORDER BY year, countryname
```

<LineChart
    data={interest_rate_comparison}
    x=year
    y=strate
    series=countryname
    title="Short-term Interest Rates"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

---

## 3. Productivity & Macro Stability (1990s–2000s)

The consequences of these strategies became evident in productivity and macroeconomic stability. South Korea successfully transitioned into high-tech manufacturing, fostering productivity growth. The Latin American economies, in contrast, often battled high inflation and macroeconomic volatility, which hampered sustained productivity gains and structural transformation.

```sql productivity_proxy
with base as (
  select
    year,
    CASE WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
         WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
    END as region,
    SUM(rGDP_USD) / nullif(SUM(pop), 0) as rgdp_pc_usd
  from gmd
  where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year between 1950 and 2025
  GROUP BY ALL
),
growth as (
  select
    *,
    (rgdp_pc_usd / lag(rgdp_pc_usd) over (partition by region order by year)) - 1 as yoy_growth
  from base
)
select 
    *,
    avg(yoy_growth) over (partition by region order by year rows between 4 preceding and current row) as yoy_growth_5yr_avg
from growth 
where yoy_growth is not null 
order by year
```

<LineChart 
    data={productivity_proxy} 
    x=year 
    y=yoy_growth 
    series=region 
    title="Real GDP per Capita Growth (YoY), 1986-2010" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

```sql inflation_comparison
select
  year,
  CASE 
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(infl * rGDP_USD) / SUM(rGDP_USD) / 100 AS infl
from gmd
where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year between 1950 and 1990
AND infl > 0
GROUP BY ALL
order by year, region
```

<LineChart 
    data={inflation_comparison} 
    x=year 
    y=infl 
    series=region 
    title="Inflation (CPI, YoY), 1990–2010" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

```sql currency_crises_comparison
SELECT
  year,
  CASE 
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(CurrencyCrisis) as currency_crises, 
  SUM(SovDebtCrisis) as sovereign_debt_crises, 
  SUM(BankingCrisis) as banking_crises
FROM gmd
WHERE iso3 IN ('JPN', 'KOR', 'BRA', 'ARG') AND year BETWEEN 1950 AND 2025
GROUP BY ALL
ORDER BY year, region
```

<BarChart
    data={currency_crises_comparison}
    x=year
    y=currency_crises
    series=region
    type=stacked
    title="Number of Currency Crises"
    downloadableData=false
    downloadableImage=false
/>

<BarChart
    data={currency_crises_comparison}
    x=year
    y=banking_crises
    series=region
    type=stacked
    title="Number of Currency Crises"
    downloadableData=false
    downloadableImage=false
/>

<BarChart
    data={currency_crises_comparison}
    x=year
    y=sovereign_debt_crises
    series=region
    type=stacked
    title="Number of Currency Crises"
    downloadableData=false
    downloadableImage=false
/>

# partial conclusions here
---

## 4. Deeper Dive: Monetary Stability Hypothesis

### A. Descriptive / Visual Tests

#### Time Series Analysis
Expectation: LatAm had high, volatile inflation and erratic interest rates. East Asia had low, stable inflation.

```sql monetary_stability_timeseries
WITH country_data as (
    SELECT
      year,
      iso3,
      nGDP / USDfx as nGDP_USD,
      infl,
      strate,
      ltrate,
      M2 / lag(M2) over (partition by iso3 order by year) - 1 as m2_growth
    FROM gmd
    WHERE iso3 IN ('JPN', 'KOR', 'BRA', 'ARG') AND year BETWEEN 1950 AND 1999
)
SELECT
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(m2_growth * nGDP_USD) / SUM(nGDP_USD) as m2_growth,
  SUM(infl * nGDP_USD) / SUM(nGDP_USD) / 100 as infl,
  SUM(strate * nGDP_USD) / SUM(nGDP_USD) / 100 as strate,
  SUM(ltrate * nGDP_USD) / SUM(nGDP_USD) / 100 as ltrate
FROM country_data
WHERE nGDP_USD IS NOT NULL
GROUP BY ALL
ORDER BY year, region
```

<LineChart
    data={monetary_stability_timeseries}
    x=year
    y=m2_growth
    series=region
    title="M2 Money Supply Growth (YoY), 1950-1999"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

<LineChart
    data={monetary_stability_timeseries}
    x=year
    y=infl
    series=region
    title="Inflation (CPI, YoY), 1950-1999"
    yFmt="0.0%"
    yLog=true
    downloadableData=false
    downloadableImage=false
/>

<LineChart
    data={monetary_stability_timeseries}
    x=year
    y=strate
    series=region
    title="Short-Term Interest Rates, 1950-1999"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

<LineChart
    data={monetary_stability_timeseries}
    x=year
    y=ltrate
    series=region
    title="Long-Term Interest Rates, 1950-1999"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

#### Volatility Analysis

```sql monetary_stability_volatility
WITH regional_data as (
    SELECT
      year,
      CASE
        WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
        WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
      END as region,
      nGDP / USDfx as nGDP_USD,
      infl,
      strate,
      M2 / lag(M2) over (partition by iso3 order by year) - 1 as m2_growth
    FROM gmd
    WHERE iso3 IN ('JPN', 'KOR', 'BRA', 'ARG') AND year BETWEEN 1950 AND 1999
),
weighted_data as (
    SELECT
      year,
      region,
      SUM(m2_growth * nGDP_USD) / SUM(nGDP_USD) as m2_growth,
      SUM(infl * nGDP_USD) / SUM(nGDP_USD) as infl,
      SUM(strate * nGDP_USD) / SUM(nGDP_USD) as strate
    FROM regional_data
    WHERE nGDP_USD IS NOT NULL
    GROUP BY year, region
)
SELECT
  floor(year / 10) * 10 as decade,
  region,
  STDDEV_SAMP(m2_growth) as m2_growth_volatility,
  STDDEV_SAMP(infl) as infl_volatility,
  STDDEV_SAMP(strate) as strate_volatility
FROM weighted_data
GROUP BY ALL
ORDER BY decade, region
```

<BarChart
    data={monetary_stability_volatility}
    x=decade
    y=m2_growth_volatility
    series=region
    type=grouped
    title="Volatility of M2 Growth by Decade"
    downloadableData=false
    downloadableImage=false
/>

<BarChart
    data={monetary_stability_volatility}
    x=decade
    y=infl_volatility
    series=region
    type=grouped
    title="Volatility of Inflation by Decade"
    downloadableData=false
    downloadableImage=false
/>

<BarChart
    data={monetary_stability_volatility}
    x=decade
    y=strate_volatility
    series=region
    type=grouped
    title="Volatility of Short-Term Interest Rates by Decade"
    downloadableData=false
    downloadableImage=false
/>

### B. Simple Correlation / Cross-Country Scatter

```sql correlation_analysis
WITH m2_growth_calc as (
    SELECT
        year,
        iso3,
        M2 / lag(M2) over (partition by iso3 order by year) - 1 as m2_growth,
        rGDP_USD / pop as gdp_pc,
        infl,
        ltrate,
        finv / nGDP as finv_gdp
    FROM gmd
    WHERE year BETWEEN 1960 AND 1990
),
growth_and_monetary as (
    SELECT
        iso3,
        AVG(gdp_pc) as avg_gdp_pc,
        AVG(m2_growth) as avg_m2_growth,
        STDDEV_SAMP(infl) as infl_volatility,
        AVG(ltrate) as avg_ltrate,
        AVG(finv_gdp) as avg_finv_gdp
    FROM m2_growth_calc
    GROUP BY iso3
)
SELECT
    g.iso3,
    g.countryname,
    gam.avg_gdp_pc,
    gam.avg_m2_growth,
    gam.infl_volatility,
    gam.avg_ltrate,
    gam.avg_finv_gdp
FROM gmd g
JOIN growth_and_monetary gam ON g.iso3 = gam.iso3
WHERE g.year = 1990
```

<ScatterPlot
    data={correlation_analysis}
    x=avg_m2_growth
    y=avg_gdp_pc
    series=countryname
    title="M2 Growth vs. GDP per Capita Growth"
    downloadableData=false
    downloadableImage=false
/>

<ScatterPlot
    data={correlation_analysis}
    x=infl_volatility
    y=avg_gdp_pc
    series=countryname
    title="Inflation Volatility vs. GDP per Capita Growth"
    downloadableData=false
    downloadableImage=false
/>

<ScatterPlot
    data={correlation_analysis}
    x=avg_ltrate
    y=avg_finv_gdp
    series=countryname
    title="Long-Term Interest Rates vs. Fixed Investment"
    downloadableData=false
    downloadableImage=false
/>

# 5. Growth models don't last forever: how South Korea adapted and outgrew Japan

Divergence between South Korea and Japan - why? [CHANGE COLOR PALETTE]

South Korea keeps growing, while Japan stagnates. 

```sql development_paths
SELECT
  year,
  countryname,
  iso3,
  rGDP_USD / nullif(pop, 0) as rgdp_pc_usd
FROM gmd
WHERE year >= 1950
  AND iso3 IN ('KOR', 'JPN')
ORDER BY year, countryname
```

<LineChart
    data={development_paths}
    x=year
    y=rgdp_pc_usd
    series=countryname
    title='South Korea in particular has had an explosive growth trajectory'
    subtitle="GDP per Capita, 1950-Present"
    yFmt="$#,##0"
    downloadableData=false
    downloadableImage=false
/>
---



<LineChart 
    data={regional_comparison_gov_finance} 
    x=year 
    y=govdebt_GDP 
    series=region 
    title="Government Debt as a Share of GDP, 1950–1990" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/> 

<LineChart 
    data={regional_comparison_gov_finance} 
    x=year 
    y=govrev_GDP 
    series=region 
    title="Government Revenue as a Share of GDP, 1950–1990" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>  

<BarChart 
    data={regional_comparison_gov_finance} 
    x=year 
    y=govtax_GDP 
    series=region 
    type=grouped
    title="Government Tax Revenue as a Share of GDP, 1950–1990" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>  

<LineChart 
    data={regional_comparison_gov_finance} 
    x=year 
    y=govdef_GDP 
    series=region 
    title="Government Deficit as a Share of GDP, 1950–1990" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>


<LineChart 
    data={regional_comparison_external_sector} 
    x=year 
    y=exports_GDP 
    series=region 
    title="Exports as a Share of GDP, 1950–1990" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

<LineChart 
    data={regional_comparison_external_sector} 
    x=year 
    y=imports_GDP 
    series=region 
    title="Imports as a Share of GDP, 1950–1990" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

The story of economic development is often told as a story of divergence, where the rich get richer and the poor get poorer. But is this always the case? The chart below plots every country's position in the global income distribution in 1900 against its position in 2025.

Countries on the diagonal line have maintained their relative rank. Most countries, as you can see, have not moved much. However, there are notable exceptions. South Korea, for example, has moved from the bottom of the distribution to the top, showing that incredible economic transformation is possible.

```sql gdp_percentiles_1900_2025
with base_1900 as (
    select
        iso3,
        countryname,
        rGDP_USD / pop as gdp_pc_1900,
        cume_dist() over (order by rGDP_USD / pop) as percentile_rank_1900
    from gmd
    where year = 1950 and rGDP_USD is not null and pop is not null
),
base_2025 as (
    select
        iso3,
        rGDP_USD / pop as gdp_pc_2025,
        cume_dist() over (order by rGDP_USD / pop) as percentile_rank_2025
    from gmd
    where year = 2025 and rGDP_USD is not null and pop is not null
)
select
    b1.iso3,
    b1.countryname,
    b1.percentile_rank_1900,
    b2.percentile_rank_2025,
    CASE
        WHEN b1.iso3 IN ('USA', 'CAN', 'AUS', 'NZL', 'GBR', 'FRA', 'DEU', 'ITA', 'ESP', 'NLD', 'BEL', 'CHE', 'SWE', 'NOR', 'DNK', 'FIN', 'AUT', 'PRT', 'IRL', 'LUX') THEN 'Western Europe+USA+CAN+AUS+NZ'
        WHEN b1.iso3 IN ('POL', 'CZE', 'SVK', 'HUN', 'ROU', 'BGR', 'SVN', 'HRV', 'SRB', 'BIH', 'MNE', 'MKD', 'ALB', 'EST', 'LVA', 'LTU') THEN 'CEE'
        WHEN b1.iso3 IN ('ARG', 'BOL', 'BRA', 'CHL', 'COL', 'CRI', 'CUB', 'DOM', 'ECU', 'SLV', 'GTM', 'HND', 'MEX', 'NIC', 'PAN', 'PRY', 'PER', 'URY', 'VEN') THEN 'Latin America'
        WHEN b1.iso3 IN ('NGA', 'EGY', 'ZAF', 'DZA', 'AGO', 'ETH', 'KEN', 'GHA', 'MAR', 'TZA', 'SDN', 'UGA', 'CIV', 'CMR', 'ZWE', 'ZMB') THEN 'Africa'
    WHEN b1.iso3 IN ('USA', 'CAN', 'AUS', 'NZL', 'GBR', 'FRA', 'DEU', 'ITA', 'ESP', 'NLD', 'BEL', 'CHE', 'SWE', 'NOR', 'DNK', 'FIN', 'AUT', 'PRT', 'IRL', 'LUX') THEN 'Western Europe+USA+CAN+AUS+NZ'
    WHEN b1.iso3 IN ('POL', 'CZE', 'SVK', 'HUN', 'ROU', 'BGR', 'SVN', 'HRV', 'SRB', 'BIH', 'MNE', 'MKD', 'ALB', 'EST', 'LVA', 'LTU') THEN 'CEE'
    WHEN b1.iso3 IN ('ARG', 'BOL', 'BRA', 'CHL', 'COL', 'CRI', 'CUB', 'DOM', 'ECU', 'SLV', 'GTM', 'HND', 'MEX', 'NIC', 'PAN', 'PRY', 'PER', 'URY', 'VEN') THEN 'Latin America'
    WHEN b1.iso3 IN ('NGA', 'EGY', 'ZAF', 'DZA', 'AGO', 'ETH', 'KEN', 'GHA', 'MAR', 'TZA', 'SDN', 'UGA', 'CIV', 'CMR', 'ZWE', 'ZMB') THEN 'Africa'
    WHEN b1.iso3 IN ('JPN', 'KOR') THEN 'Japan+Korea'
    WHEN b1.iso3 IN ('IDN', 'VNM', 'PHL', 'THA', 'MYS', 'SGP', 'MMR', 'KHM', 'LAO') THEN 'South East Asia'
    WHEN b1.iso3 IN ('IND', 'PAK', 'BGD', 'LKA', 'NPL') THEN 'Indian Subcontinent'
    WHEN b1.iso3 IN ('CHN', 'IRN', 'SAU', 'HKG', 'ISR', 'ARE', 'IRQ', 'QAT', 'KWT', 'OMN') THEN 'Other Asia'
    ELSE 'ROW'
END as region
from base_1900 b1
join base_2025 b2 on b1.iso3 = b2.iso3
```

<ScatterPlot
    data={gdp_percentiles_1900_2025}
    x=percentile_rank_1900
    y=percentile_rank_2025
    series=region
    title="Global Economic Mobility, 1900 vs. 2025"
    xFmt="0%"
    yFmt="0%"
    downloadableData=false
    downloadableImage=false
    >
    <ReferenceLine y=x/>
</ScatterPlot>
 
The rest of this page explores the story behind this divergence by comparing the East Asian model, represented by Japan and South Korea, with the model pursued by major Latin American economies: Brazil and Argentina.


# Wanna see how your country did vs South Korea? Pick your country from the dropdown below, and get a summary file.
