---
title: "What we can learn from East Asia's industrialisation after WWII"
---
# 1. Since 1945, only a few developing countries have successfully closed the development gap with the West

The development gap between countries is stark. Can we overcome it? Latin Americans often see the disparity between developed and developing countries as a structural irreversible process. 

In this analysis I compare two major Latin American economies, <strong>Argentina</strong> and <strong>Brazil</strong>, with two East Asian economies that have successfully transitioned to developed status: <strong>Japan</strong> and <strong>South Korea</strong>.

I use <strong>GDP per capita</strong> to gauge development. While GDP per capita does not account for social factors (eg., access to education, life expectancy, income inequality), it is still a good proxy metric for overall economic well-being. 

<div style="padding: 15px; border: 1px solid #ddd; border-left: 5px solid #f0ad4e; background-color: #fcf8e3;">
  For simplicity, whenever I refer to "Latin America" in this analysis, I mean Argentina and Brazil. Whenever I refer to "East Asia", I mean Japan and South Korea.
</div>

### Over a century later, there is still a persistent development gap between Latin America and Western Europe

<LineChart
    data={europe_latam_gdp}
    x=year
    y=rgdp_pc_usd
    series=region
    subtitle="GDP per Capita: Western Europe vs. Latin America (1900-Present)"
    yFmt="$#,##0"
    downloadableData=false
    downloadableImage=false
  >
      <ReferenceArea 
      xMin='2020' 
      xMax='2025' 
      color=info/>
      <Callout x=2023 y=36500 labelPosition=center symbol=none align=left>
      Latin America has barely reduced its GDP per capita gap with Western Europe
      </Callout>
</LineChart>

<br/>

### However, it took Korea and Japan less than 4 decades to catch up with the West

<LineChart
    data={europe_latam_ea_gdp}
    x=year
    y=rgdp_pc_usd
    series=region
    subtitle="GDP per Capita: East Asia vs Western Europe vs. Latin America (1900-Present)"
    yFmt="$#,##0"
    colorPalette={['#7c828dff','#cccfd5ff','#103ea1ff']}
    downloadableData=false
    downloadableImage=false
    >
  <ReferenceArea xMin='1950' xMax='1990' color=info label='Korea and Japan become developed countries'/>

</LineChart>

---

# 2. Three major factors explain why South Korea and Japan succeeded while Argentina and Brazil did not (1950s–1990s)

In this analysis, I delve into the reasons behind the divergent development paths of South Korea and Japan compared to Argentina and Brazil between 1950 and 1990. 

I will explore three main hypotheses:
1. **Openness to trade**: Integrating into the global economy forces domestic industries to become more competitive, generates currency reserves, and allows countries to specialise based on their comparative advantages.
2. **State policies to spur long-term capital formation**: Deliberate state intervention makes sense if the goal is long-term globally competitive industrialisation rather than short-term consumption.
3. **Monetary discipline**: Maintaining strict, disciplined control over money supply growth lays the foundations for economic stability and, consequently, creates the necessary conditions for sustainable growth.

### Openness to trade

Both imports and exports as a share of GDP have been significantly higher in East Asia than Latin America. 

This reflects not only a more open trade policy, but also a more competitive economy that has to innovate constantly to perform well and stay ahead. This results in more long-term value creation and productivity growth.

<LineChart 
    data={regional_comparison_external_sector} 
    x=year 
    y=trade_openness 
    series=region 
    title="Japan and Korea embraced open trade, while Argentina and Brazil remained stuck in protectionism"
    subtitle="Trade openness (Exports + Imports as % of GDP)" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false>
</LineChart>

As a result, Japan and Korea consistently run current account surpluses. This has probably generated a virtuous cycle: higher savings and investment, leading to greater productivity and competitiveness.

<LineChart 
    data={regional_comparison_external_sector} 
    x=year 
    y=CA_GDP 
    series=region 
    title="Japan and Korea's export-led growth model pays off"
    subtitle="Current account balance (% GDP) (1950–1990)" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
    >
    <ReferenceArea xMin=1968 xMax=1990 yMin=0 yMax=0.04 label="Consistent current account surpluses" color=base-content-muted border=true/>
</LineChart>

<br/>

### State policies to spur long-term capital formation

<LineChart 
    data={regional_comparison_gov_expenditure} 
    x=year 
    y=govexp_GDP 
    series=region 
    title='At first glance, both regions have had similar levels of government expenditure'
    subtitle="Government Expenditure (% of GDP) (1950–1990)" 
    yFmt="0%"
    downloadableData=false
    downloadableImage=false
>
    <ReferenceArea xMin=1950 xMax=1980 yMin=0.07 yMax=0.25 label="Similar levels of gov. expenditure" color=base-content-muted border=true/>
</LineChart>

<Grid cols=2>
<Group>
However, from the outset Japan and Korea invested heavily in their economies
<LineChart 
    data={regional_comparison_investment} 
    x=year 
    y=inv_GDP 
    series=region 
    subtitle="Investment (% GDP) (1950–1990)" 
    yFmt="0%"
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
    subtitle="Fixed investment (% GDP) (1950–1990)" 
    yFmt="0%"
    yMax=0.5
    downloadableData=false
    downloadableImage=false
/>    </Group>

</Grid>

Instead, Argentina and Brazil focused on consumption, which only serves the short-term and is unsustainable in the long run.

<LineChart 
    data={regional_comparison_consumption} 
    x=year 
    y=cons_GDP 
    series=region 
    subtitle="Consumption (% GDP) (1960-1990; data only available from 1960)¹" 
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

<br/>

### Monetary discipline
<div style="padding: 15px; border: 1px solid #ddd; border-left: 5px solid #f0ad4e; background-color: #fcf8e3;">
  Notice the difference in the Y-axis in the charts below. It highlights how Latin America enacted fundementally unsustainable policies, which culminated in extremely high volatility and crisis at the end of the 1980s.
</div>
<br/>
The persistent expansion of M2² supply in Latin America led to an erosion of confidence in the currency and banking system. 
<Grid cols=2>
<Group>
<LineChart 
    data={fiscal_comparison_1950_1982} 
    x=year 
    y=m2_growth 
    series=region 
    subtitle="M2 Growth (1950–1982)" 
    yFmt="0%"
    downloadableData=false
    downloadableImage=false
/> </Group>
<Group>
<LineChart 
    data={fiscal_comparison_1983_1990} 
    x=year 
    y=m2_growth 
    series=region 
    subtitle="M2 Growth (1983–1990)" 
    yFmt="0%"
    downloadableData=false
    downloadableImage=false
/> </Group>
</Grid>

<Grid cols=2>
<Group>
Korea and Japan's policy was geared towards maintaining a low, stable inflation rate...
<LineChart 
    data={fiscal_comparison_1950_1982} 
    x=year 
    y=infl 
    series=region 
    subtitle="Inflation (CPI) (1950–1982)" 
    yFmt="0%"
    downloadableData=false
    downloadableImage=false
/> </Group>
<Group>
... by the end of the 80s, Argentina and Brazil's volatility culminated in a hyperinflation crisis. 
<LineChart 
    data={fiscal_comparison_1983_1990} 
    x=year 
    y=infl 
    series=region 
    subtitle="Inflation (CPI) (1983–1990)" 
    yFmt="0%"
    downloadableData=false
    downloadableImage=false
/> </Group>
</Grid>

<Grid cols=2>
<Group>
East Asia's predictable low short-term interest rate environment encouraged the private sector to invest in long-term industrialisation...
<LineChart 
    data={fiscal_comparison_1950_1982} 
    x=year 
    y=strate 
    series=region 
    subtitle="Short-term interest rates (1950–1982)³" 
    yFmt="0%"
    downloadableData=false
    downloadableImage=false
/> </Group>
<Group>
... while monetary authoritities in Latin America resorted to short-term interest rate hikes to fight the crisis, thus paralysing the economy.
<LineChart 
    data={fiscal_comparison_1983_1990} 
    x=year 
    y=strate 
    series=region 
    subtitle="Short-term interest rates (1983–1990)³" 
    yFmt="0%"
    downloadableData=false
    downloadableImage=false
/> </Group>
</Grid>

---
### Put together, these policies bridged the gap with the West

<LineChart
    data={europe_latam_ea_gdp}
    x=year
    y=rgdp_pc_usd
    series=region
    subtitle="GDP per Capita: East Asia vs Western Europe vs. Latin America (1900-Present)"
    yFmt="$#,##0"
    colorPalette={['#7c828dff','#cccfd5ff','#103ea1ff']}
    downloadableData=false
    downloadableImage=false
    >
  <ReferenceArea xMin='1950' xMax='1990' color=info label='Korea and Japan become developed countries'/>

</LineChart>

<Grid cols=2>
<BarChart
    data={crisis_timeline_latam}
    x=year
    y=crisis_count
    series=crisis_type
    type=stacked
    title="LatAm's short-term thinking made it crisis-prone..."
    subtitle="Timeline of crises by type in Latin America (1950-1990)"
    downloadableData=false
    downloadableImage=false
>
</BarChart>

<BarChart
    data={crisis_timeline_ea}
    x=year
    y=crisis_count
    series=crisis_type
    type=stacked
    title="... while East Asia built more stable foundations"
    subtitle="Timeline of crises by type in East Asia (1950-1990)"
    downloadableData=false
    downloadableImage=false
> 
</BarChart>
</Grid>

# 3. Growth models don't last forever: how South Korea adapted after the 1990s and outgrew Japan

While these policies worked well for Japan and South Korea until the early 1990s, they eventually reached their limits. Growth models do not last forever, and it is important to adapt to changing circumstances.

South Korea has arguably been able to adapt much better - it's managed to maintain an explosive growth trajectory over time. In contrast, Japan plateaued for most of the 1990s and early 21<sup>st</sup> Century.

<div style="padding: 15px; border: 1px solid #ddd; border-left: 5px solid #f0ad4e; background-color: #fcf8e3;">
  Click<a href="korea_japan_1990_crisis" style="background-color: #fffbe6; padding: 2px 6px; border-radius: 4px; font-weight: bold;">here</a>to see a detailed analysis of this divergence. 
</div>

<LineChart
    data={development_paths_korea_japan}
    x=year
    y=rgdp_pc_usd
    series=countryname
    title='South Korea has managed to maintain an explosive growth trajectory over time'
    subtitle="GDP per Capita, 1950-Present"
    yFmt="$#,##0"
    downloadableData=false
    downloadableImage=false
>
    <ReferenceArea xMin=1990 xMax=2025 yMin=25000 yMax=40000 label="Japan's growth has been much less spectacular" color=base-content-muted border=true/>
</LineChart>

---

## SQL queries used in this page

If the queries are hidden, click on the hamburger menu in the top right corner and then click on "Show Queries". 

```sql europe_latam_gdp
SELECT
  year,
  SUM(rGDP_USD) / SUM(pop) as rgdp_pc_usd,
  CASE
    WHEN iso3 IN ('GBR', 'FRA', 'ITA', 'ESP') THEN 'Western Europe'
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
  END as region
FROM gmd
WHERE year BETWEEN 1900 AND 2025
  AND iso3 IN ('GBR', 'FRA', 'ARG', 'BRA', 'ITA', 'ESP')
GROUP BY ALL
ORDER BY year
```

```sql europe_latam_ea_gdp
SELECT
  year,
  --countryname,
  SUM(rGDP_USD) / SUM(pop) as rgdp_pc_usd,
  CASE
    WHEN iso3 IN ('GBR', 'FRA', 'ITA', 'ESP',) THEN 'Western Europe'
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region
FROM gmd
WHERE year >= 1900
  AND iso3 IN ('GBR', 'FRA', 'ARG', 'BRA', 'ITA', 'ESP', 'JPN', 'KOR')
GROUP BY ALL
ORDER BY year
```

```sql regional_comparison_gov_expenditure
SELECT
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(govexp / USDfx) / SUM(nGDP / USDfx) as govexp_GDP
FROM gmd
WHERE iso3 IN ('JPN', 'KOR', 'BRA', 'ARG') AND year BETWEEN 1950 AND 1990
GROUP BY ALL
ORDER BY year, region
```

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

```sql crisis_timeline_ea
WITH crisis_by_year AS (
    SELECT
      year,
      CASE 
        WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
      END as region,
      SUM(CurrencyCrisis) as currency, 
      SUM(SovDebtCrisis) as sovereign_debt, 
      SUM(BankingCrisis) as banking
    FROM gmd
    WHERE iso3 IN ('JPN', 'KOR') AND year BETWEEN 1950 AND 1990
    GROUP BY ALL
)
SELECT year, region, 'Currency' as crisis_type, currency as crisis_count FROM crisis_by_year
UNION ALL
SELECT year, region, 'Sovereign debt' as crisis_type, sovereign_debt as crisis_count FROM crisis_by_year
UNION ALL
SELECT year, region, 'Banking' as crisis_type, banking as crisis_count FROM crisis_by_year
```

```sql crisis_timeline_latam
WITH crisis_by_year AS (
    SELECT
      year,
      CASE 
        WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
      END as region,
      SUM(CurrencyCrisis) as currency, 
      SUM(SovDebtCrisis) as sovereign_debt, 
      SUM(BankingCrisis) as banking
    FROM gmd
    WHERE iso3 IN ('BRA', 'ARG') AND year BETWEEN 1950 AND 1990
    GROUP BY ALL
)
SELECT year, region, 'Currency' as crisis_type, currency as crisis_count FROM crisis_by_year
UNION ALL
SELECT year, region, 'Sovereign debt' as crisis_type, sovereign_debt as crisis_count FROM crisis_by_year
UNION ALL
SELECT year, region, 'Banking' as crisis_type, banking as crisis_count FROM crisis_by_year
```

```sql regional_comparison_govdebt
select
  year,
  CASE
    WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
    WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
  END as region,
  SUM(govdebt_GDP * rGDP_USD) / SUM(rGDP_USD) / 100 as govdebt_GDP
from gmd
where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year BETWEEN 1950 and 1990
GROUP BY ALL
order by year, region
```

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

```sql regional_comparison_m2
with country_growth as (
    select
      year,
      rGDP_USD,
      -- Assign the region to each row here, before the aggregation
      CASE
        WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
        WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
      END as region,
      -- This calculation remains the same
      (M2 / lag(M2) over (partition by iso3 order by year)) - 1 as m2_growth
    from gmd
    where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year BETWEEN 1950 and 1990
)
select
  year,
  region,
  -- Now you can group by region and calculate the weighted average
  SUM(m2_growth * rGDP_USD) / SUM(rGDP_USD) as m2_growth
from country_growth
where m2_growth is not null
group by year, region
order by year, region;
```

```sql development_paths_korea_japan
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
where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and year BETWEEN 1950 and 1990
GROUP BY ALL
order by year, region
```

```sql fiscal_comparison_1950_1982
WITH country_data AS (
    SELECT
        year,
        iso3,
        infl,
        strate,
        -- Calculate nGDP in USD for weighting
        nGDP / USDfx as nGDP_USD,
        -- Calculate M2 growth for each country before aggregation
        (M2 / lag(M2) over (partition by iso3 order by year)) - 1 as m2_growth
    FROM gmd
    WHERE iso3 IN ('JPN', 'KOR', 'BRA', 'ARG') AND year BETWEEN 1950 AND 1982
)
SELECT
    year,
    CASE 
        WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
        WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
    END as region,
    -- Inflation, now correctly weighted by nGDP_USD
    SUM(infl * nGDP_USD) / SUM(nGDP_USD) / 100 AS infl,
    -- New: Weighted average for short-term interest rates
    SUM(strate * nGDP_USD) / SUM(nGDP_USD) / 100 as strate,
    -- New: Weighted average for M2 growth
    SUM(m2_growth * nGDP_USD) / SUM(nGDP_USD) as m2_growth
FROM country_data
WHERE nGDP_USD IS NOT NULL
GROUP BY ALL
ORDER BY year, region
```

```sql fiscal_comparison_1983_1990
WITH country_data AS (
    SELECT
        year,
        iso3,
        infl,
        strate,
        -- Calculate nGDP in USD for weighting
        nGDP / USDfx as nGDP_USD,
        -- Calculate M2 growth for each country before aggregation
        (M2 / lag(M2) over (partition by iso3 order by year)) - 1 as m2_growth
    FROM gmd
    WHERE iso3 IN ('JPN', 'KOR', 'BRA', 'ARG') AND year BETWEEN 1983 AND 1990
)
SELECT
    year,
    CASE 
        WHEN iso3 IN ('ARG', 'BRA') THEN 'Latin America'
        WHEN iso3 IN ('JPN', 'KOR') THEN 'East Asia' 
    END as region,
    -- Inflation, now correctly weighted by nGDP_USD
    SUM(infl * nGDP_USD) / SUM(nGDP_USD) / 100 AS infl,
    -- New: Weighted average for short-term interest rates
    SUM(strate * nGDP_USD) / SUM(nGDP_USD) / 100 as strate,
    -- New: Weighted average for M2 growth
    SUM(m2_growth * nGDP_USD) / SUM(nGDP_USD) as m2_growth
FROM country_data
WHERE nGDP_USD IS NOT NULL
GROUP BY ALL
ORDER BY year, region

```

<small><small><i>¹ Technically this metric tracks both government and private consumption. It would be better to disaggregate these components, but as a proxy for overall state intervention patterns, it serves its purpose.</i></small></small>
<br/>
<small><small><i>² M2 was chosen over M1 because M1 only tracks cash and checking accounts, which misses the significant purchasing power held in easily accessible savings. M2 was chosen over M3 and M4 because both include large, institutional assets not closely tied to consumer spending, which can obscure the signal on inflation.</i></small></small>
<br/>
<small><small><i>³ Short-term rates were chosen due to the scarcity of both central bank and long-term interest rate data for Argentina. In any case, short-term rates are a robust metric: particularly in periods of high inflation, they reflect the real-time, extreme cost of capital plaguing a country's economy.</i></small></small>