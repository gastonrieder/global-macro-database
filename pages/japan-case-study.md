---
title: "Crisis, Recovery, and Divergence: South Korea's Post-1997 Surge vs. Japan's Lost Decades"
---

This page explores the divergent economic paths of Japan and South Korea following the Asian Financial Crisis of the late 1990s. While both countries were hit by significant economic shocks, their recoveries and subsequent growth trajectories have been remarkably different. We will use the Global Macro Database to analyze why South Korea staged a robust recovery and continued to grow, while Japan entered a prolonged period of stagnation often referred to as its "lost decades."

---

## 1. A Shared Shock: The Late 1990s Crisis

Both Japan and South Korea faced severe economic crises in the 1990s. Japan's asset bubble burst in the early 1990s, leading to a prolonged banking crisis. South Korea was hit hard by the Asian Financial Crisis in 1997. The chart below uses crisis dummy variables from the database to show that both countries experienced systemic banking and currency crises during this period, setting the stage for their divergent paths.

```sql crisis_comparison
SELECT
  year,
  countryname,
  iso3,
  BankingCrisis,
  CurrencyCrisis
FROM gmd
WHERE iso3 IN ('JPN', 'KOR') AND year BETWEEN 1990 AND 2005
ORDER BY year, countryname
```

<BarChart
    data={crisis_comparison}
    x=year
    y=BankingCrisis
    series=countryname
    type=grouped
    title="Banking Crisis Years"
    downloadableData=false
    downloadableImage=false
/>

<BarChart
    data={crisis_comparison}
    x=year
    y=CurrencyCrisis
    series=countryname
    type=grouped
    title="Currency Crisis Years"
    downloadableData=false
    downloadableImage=false
/>

---

## 2. The Great Divergence: Growth and Exports

In the aftermath of the crises, the two economies diverged sharply. South Korea experienced a painful but rapid V-shaped recovery, returning to a strong growth trajectory. Japan, in contrast, languished in a low-growth trap. The charts below illustrate this divergence in both overall economic growth and export performance.

```sql growth_export_comparison
SELECT
  year,
  countryname,
  iso3,
  (rGDP_USD / nullif(pop, 0)) / lag(rGDP_USD / nullif(pop, 0)) over (partition by iso3 order by year) - 1 as rgdp_pc_growth,
  exports_GDP / 100 as exports_GDP
FROM gmd
WHERE iso3 IN ('JPN', 'KOR') AND year BETWEEN 1990 AND 2025
ORDER BY year, countryname
```

### Real GDP Per Capita Growth

South Korea's growth, while volatile, clearly rebounded and outpaced Japan's anemic performance in the 2000s.

<LineChart
    data={growth_export_comparison}
    x=year
    y=rgdp_pc_growth
    series=countryname
    title="Real GDP per Capita Growth"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
>
    <Callout x=1998 y=-0.06 labelPosition=right>
    Korea was hit harder than Japan during its crisis.
    </Callout>
    <Callout x=2011 y=0.09 labelPosition=right symbol=none>
    Since then, it has had consistently higher growth than Japan.
    </Callout>
</LineChart>

### Export Competitiveness

South Korea's export engine continued to fire, adapting to new global markets, while Japan's share of exports in its economy stagnated, indicating a loss of competitive dynamism.

<LineChart
    data={growth_export_comparison}
    x=year
    y=exports_GDP
    series=countryname
    title="Exports as a Share of GDP"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

---

## 3. Explaining the Divergence: Fiscal Discipline

One of the most striking differences in the post-crisis policy response was in the realm of fiscal policy. Japan embarked on years of massive fiscal stimulus, leading to an explosion in public debt, with little discernible impact on growth. South Korea, having received an IMF bailout, pursued a more disciplined fiscal path.

```sql fiscal_comparison
SELECT
  year,
  countryname,
  iso3,
  govdebt_GDP / 100 as govdebt_GDP,
  govdef_GDP / 100 as govdef_GDP,
  govexp_GDP / 100 as govexp_GDP,
  inv_GDP / 100 as inv_GDP
FROM gmd
WHERE iso3 IN ('JPN', 'KOR') AND year >= 1990
ORDER BY year, countryname
```

### Government Debt & Deficits

The charts below show the dramatic and sustained rise in Japan's government debt-to-GDP ratio, driven by persistent fiscal deficits. South Korea, in contrast, has maintained a much more conservative fiscal position.

<Grid cols=2>
<LineChart
    data={fiscal_comparison}
    x=year
    y=govdebt_GDP
    series=countryname
    title="Government Debt as a Share of GDP"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

<BarChart
    data={fiscal_comparison}
    x=year
    y=govdef_GDP
    series=countryname
    type=grouped
    title="Fiscal Deficit as a Share of GDP"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>
</Grid>

### Government Spending and investment 

Despite rising government expenditure, Japan's growth remained weak, suggesting fiscal policy was not effective. South Korea's spending has been more modest.

A key difference in policy response can also be seen in investment. South Korea's investment levels remained robust, supporting its recovery, while Japan's investment share of the economy declined.

<Grid cols=2>
<LineChart
    data={fiscal_comparison}
    x=year
    y=govexp_GDP
    series=countryname
    title="Government Expenditure (% GDP)"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>

<LineChart
    data={fiscal_comparison}
    x=year
    y=inv_GDP
    series=countryname
    title="Investment as a Share of GDP"
    yFmt="0.0%"
    downloadableData=false
    downloadableImage=false
/>
</Grid>