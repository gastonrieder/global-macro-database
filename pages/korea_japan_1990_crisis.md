---
title: "Two different ways of responding to a crisis: how South Korea outgrew Japan"
---

# 1. Asia as a whole was struck by crisis in the second half of the 1990s

<BigValue
    data={asian_crisis_comparison}
    value=total_crises 
    comparison=crisis_growth
    comparisonFmt="+0"
    comparisonTitle="vs. 1990-1995"
    downIsGood=true
    title='Total number of crises in major Asian economies (1996-2000)¹'
    />

### Korea and Japan were both negatively impacted, but their post-crisis paths suggest different policy responses

<Grid cols=2>
<LineChart
    data={growth_export_comparison_0}
    x=year
    y=rgdp_pc_growth
    series=countryname
    title="Korea was hit harder but quickly rebounded..."
    subtitle="Real GDP per capita growth (1990-2000)"
    yFmt="0.0%"
    >
    <ReferenceArea 
    xMin='1997' 
    xMax='1999' 
    label="Asian financial crisis" 
    color=negative
    />
</LineChart>
<LineChart
    data={growth_export_comparison_1}
    x=year
    y=rgdp_pc_growth
    series=countryname
    title="... and since then, it's consistently outgrown Japan"
    subtitle="Real GDP per capita growth (2000-2025)"
    yFmt="0.0%"
    >
</LineChart>
</Grid>

---

# 2. Fiscal discipline and continued investment in long-term industrialisation explain South Korea's stronger growth vs. Japan

In response to the crisis, Japan redirected its fiscal policy towards short-term stimulus.

This led to a vicious cycle of rising fiscal deficits and debt, which constrained Japan's ability to invest in the long-term. 

Indeed, we've seen that Japan's growth has been weak after the crisis. 

<Grid cols=2>
<BarChart
    data={fiscal_comparison}
    x=year
    y=govdef_GDP
    series=countryname
    type=grouped
    title='Japan tried to spend its way out of crisis...'
    subtitle="Fiscal Deficit (% GDP)"
    yFmt="0.0%"
/>
<LineChart
    data={fiscal_comparison}
    x=year
    y=govdebt_GDP
    series=countryname
    title="... eventually accruing unsustainable levels of debt"
    subtitle="Government Debt (% GDP)"
    yFmt="0.0%"
/>
</Grid>

On the other hand, South Korea embraced fiscal discipline, reorientating its economy back on track with a focus on long-term investment.


<Grid cols=2>
<LineChart
    data={fiscal_comparison}
    x=year
    y=govexp_GDP
    series=countryname
    title="Korea kept government spending in check..."
    subtitle="Government Expenditure (% GDP)"
    yFmt="0.0%"
/>
<LineChart
    data={fiscal_comparison}
    x=year
    y=finv_GDP
    series=countryname
    title="... while still investing heavily in the long-term"
    subtitle="Fixed investment (% GDP)"
    yFmt="0.0%"
/>
</Grid>


# 3. This helped South Korea remain competitive in global markets and sustain its export-led growth model

South Korea's continued high investment rate allowed it to stay abreast of technological advancements, just as the world stepped into the digital age. 

While still a major global player, Japan failed to adapt as quickly because of a lack of fiscal discipline and a focus on short-term stimulus. This has led to a gradual loss in competitiveness. 

<Grid cols=2>
<LineChart
    data={growth_export_comparison}
    x=year
    y=exports_GDP
    series=countryname
    subtitle="Exports (% GDP)"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
    >
</LineChart>
    <br> <br> <br> 
South Korea's export-to-GDP ratio continued to rise after the crisis. 
    <br> <br>
Japan's exports dwindled and took longer to pick up again.
</Grid>

---

## SQL queries used in this page

If the queries are hidden, click on the hamburger menu in the top right corner and then click on "Show Queries". 


```sql growth_export_comparison
SELECT
  year,
  countryname,
  iso3,
  (rGDP_USD / nullif(pop, 0)) / lag(rGDP_USD / nullif(pop, 0)) over (partition by iso3 order by year) - 1 as rgdp_pc_growth,
  exports_GDP / 100 as exports_GDP
FROM gmd
WHERE iso3 IN ('JPN', 'KOR') AND year BETWEEN 1989 AND 2025
ORDER BY year, countryname
```

```sql growth_export_comparison_0
SELECT
  year,
  countryname,
  iso3,
  (rGDP_USD / nullif(pop, 0)) / lag(rGDP_USD / nullif(pop, 0)) over (partition by iso3 order by year) - 1 as rgdp_pc_growth
FROM gmd
WHERE iso3 IN ('JPN', 'KOR') AND year BETWEEN 1989 AND 2000
ORDER BY year, countryname
```

```sql growth_export_comparison_0
SELECT
  year,
  countryname,
  iso3,
  (rGDP_USD / nullif(pop, 0)) / lag(rGDP_USD / nullif(pop, 0)) over (partition by iso3 order by year) - 1 as rgdp_pc_growth,
  exports_GDP / 100 as exports_GDP
FROM gmd
WHERE iso3 IN ('JPN', 'KOR') AND year BETWEEN 1989 AND 2000
ORDER BY year, countryname
```

```sql growth_export_comparison_1
SELECT
  year,
  countryname,
  iso3,
  (rGDP_USD / nullif(pop, 0)) / lag(rGDP_USD / nullif(pop, 0)) over (partition by iso3 order by year) - 1 as rgdp_pc_growth,
  exports_GDP / 100 as exports_GDP
FROM gmd
WHERE iso3 IN ('JPN', 'KOR') AND year BETWEEN 2000 AND 2025
ORDER BY year, countryname
```

```sql fiscal_comparison
SELECT
  year,
  countryname,
  iso3,
  govdebt_GDP / 100 as govdebt_GDP,
  govdef_GDP / 100 as govdef_GDP,
  govexp_GDP / 100 as govexp_GDP,
  finv_GDP / 100 as finv_GDP
FROM gmd
WHERE iso3 IN ('JPN', 'KOR') AND year BETWEEN 1989 AND 2025
ORDER BY year, countryname
```

```sql asian_crisis_comparison
WITH crisis_counts as (
    SELECT
        CASE 
            WHEN year BETWEEN 1989 AND 1995 THEN '1990-1995'
            WHEN year BETWEEN 1996 AND 2000 THEN '1996-2000'
        END as period,
        SUM(CurrencyCrisis + BankingCrisis + SovDebtCrisis) as total_crises
    FROM gmd
    WHERE iso3 IN ('KOR', 'IDN', 'THA', 'MYS', 'PHL', 'SGP', 'HKG', 'JPN', 'CHN', 'TWN')
      AND year BETWEEN 1989 AND 2000
    GROUP BY period
)
SELECT
    curr.total_crises as total_crises,
    (curr.total_crises - prev.total_crises) as crisis_growth
FROM crisis_counts curr
JOIN crisis_counts prev ON curr.period = '1996-2000' AND prev.period = '1990-1995'
```

<small><small><i>¹ Major Asian economies included in this calculation are: South Korea, Indonesia, Thailand, Malaysia, Philippines, Singapore, Hong Kong, Japan, China, and Taiwan.</i></small></small>


