---
title: Data Exploration & Quality Checks
---

# Data Exploration & Quality Checks
<LastRefreshed prefix="Data last updated"/>

This page checks basic integrity, coverage, missingness, and outliers in the GMD source.

---

## 1 Coverage and Basic Facts

```sql table_overview
select
  count(*) as rows,
  count(distinct ISO3) as countries,
  min(year) as min_year,
  max(year) as max_year
from gmd
```

- Rows: <Value data={table_overview} column=rows/>
- Countries: <Value data={table_overview} column=countries/>
- Year range: <Value data={table_overview} column=min_year/>–<Value data={table_overview} column=max_year/>

```sql coverage_by_country
select
  countryname,
  ISO3,
  min(year) as start_year,
  max(year) as end_year,
  count(*) as obs,
  max(year) - min(year) + 1 as span,
  (max(year) - min(year) + 1) - count(*) as gaps
from gmd
group by countryname, ISO3
order by gaps desc, obs asc
```

<DataTable data={coverage_by_country} rows=20>
  <Column id=countryname title="Country"/>
  <Column id=ISO3 title="ISO3"/>
  <Column id=start_year title="Start"/>
  <Column id=end_year title="End"/>
  <Column id=obs title="Obs"/>
  <Column id=span title="Span (yrs)"/>
  <Column id=gaps title="Gaps (yrs)"/>
</DataTable>

```sql top_gap_countries
select countryname, gaps
from ${coverage_by_country}
order by gaps desc
limit 20
```

<BarChart
  data={top_gap_countries}
  x=countryname
  y=gaps
  title="Top 20 Countries by Year Gaps"
/>

---

## 2 Duplicates and Key Integrity

Assuming (ISO3, year) should be unique:

```sql dup_iso3_year
select ISO3, year, count(*) as n
from gmd
group by ISO3, year
having count(*) > 1
order by n desc, ISO3, year
```

<DataTable data={dup_iso3_year} rows=20>
  <Column id=ISO3/>
  <Column id=year/>
  <Column id=n title="Count"/>
</DataTable>

Also flag ISO3 anomalies (not uppercase or length != 3):

```sql iso3_anomalies
select ISO3, count(*) as rows
from gmd
group by ISO3
having length(trim(ISO3)) <> 3 or ISO3 <> upper(ISO3)
order by rows desc, ISO3
```

<DataTable data={iso3_anomalies} rows=20/>

---

## 3 Missingness (Key Fields)

```sql missingness
select 'rGDP_pc' as column, sum(case when rGDP_pc is null then 1 else 0 end) as missing from gmd
union all
select 'nGDP_USD' as column, sum(case when nGDP_USD is null then 1 else 0 end) from gmd
union all
select 'rGDP' as column, sum(case when rGDP is null then 1 else 0 end) from gmd
union all
select 'pop' as column, sum(case when pop is null then 1 else 0 end) from gmd
```

<DataTable data={missingness} rows=10>
  <Column id=column title="Column"/>
  <Column id=missing title="Missing Count" fmt="#,##0"/>
</DataTable>

Missing by year:

```sql missing_over_time
select
  year,
  sum(case when rGDP_pc is null then 1 else 0 end) as missing_rGDP_pc,
  sum(case when pop is null then 1 else 0 end) as missing_pop,
  count(*) as total_rows
from gmd
group by year
order by year
```

<LineChart
  data={missing_over_time}
  x=year
  y=missing_rGDP_pc
  title="Missingness by Year"
/>

<LineChart
  data={missing_over_time}
  x=year
  y=missing_pop
  title="Missingness by Year"
/>

---

## 4 Invalid/Impossible Values

```sql invalid_values
select
  sum(case when rGDP_pc < 0 then 1 else 0 end) as neg_rGDP_pc,
  sum(case when nGDP_USD < 0 then 1 else 0 end) as neg_nGDP_USD,
  sum(case when pop <= 0 or pop is null then 1 else 0 end) as nonpos_or_missing_pop
from gmd
```

- Negative rGDP_pc: <Value data={invalid_values} column=neg_rGDP_pc/>
- Negative nGDP_USD: <Value data={invalid_values} column=neg_nGDP_USD/>
- Non-positive or missing pop: <Value data={invalid_values} column=nonpos_or_missing_pop/>

Key ranges:

```sql key_ranges
select
  min(rGDP_pc) as min_rGDP_pc, max(rGDP_pc) as max_rGDP_pc,
  min(nGDP_USD) as min_nGDP_USD, max(nGDP_USD) as max_nGDP_USD,
  min(pop) as min_pop, max(pop) as max_pop
from gmd
```

<DataTable data={key_ranges}/>

---

## 5 Outlier Scan (within-country z-scores for rGDP_pc)

```sql rgdp_pc_outliers
with stats as (
  select
    ISO3, countryname, year, rGDP_pc,
    avg(rGDP_pc) over (partition by ISO3) as mean_pc,
    stddev_pop(rGDP_pc) over (partition by ISO3) as sd_pc
  from gmd
)
select
  ISO3,
  countryname,
  year,
  rGDP_pc,
  (rGDP_pc - mean_pc) / nullif(sd_pc, 0) as z
from stats
where sd_pc > 0 and abs((rGDP_pc - mean_pc) / nullif(sd_pc, 0)) >= 4
order by abs(z) desc
limit 50
```

<DataTable data={rgdp_pc_outliers} rows=50>
  <Column id=ISO3/>
  <Column id=countryname title="Country"/>
  <Column id=year/>
  <Column id=rGDP_pc title="rGDP_pc" fmt="#,##0"/>
  <Column id=z title="z-score" fmt="#,##0.00"/>
</DataTable>

---

## 6 Percentiles (Distribution Snapshot)

```sql rgdp_pc_percentiles
select
  quantile_cont(rGDP_pc, 0.01) as p01,
  quantile_cont(rGDP_pc, 0.05) as p05,
  quantile_cont(rGDP_pc, 0.50) as p50,
  quantile_cont(rGDP_pc, 0.95) as p95,
  quantile_cont(rGDP_pc, 0.99) as p99
from gmd
where rGDP_pc is not null
```

<DataTable data={rgdp_pc_percentiles}/>

---

## 7 Random Sample (Spot Check)

```sql sample_rows
select *
from gmd
order by random()
limit 20
```

<DataTable data={sample_rows} rows=20/>

---

## 8 Total GDP since 1950 (Selected Countries)

```sql gdp_total_major
select
  year,
  countryname,
  iso3,
  nGDP_USD / 1e12 as gdp_trillions_usd
from gmd
where iso3 in ('USA','CHN','JPN','DEU','IND','GBR','FRA')
  and year >= 1950
order by year, countryname
```

<LineChart
  data={gdp_total_major}
  x=year
  y=gdp_trillions_usd
  series=countryname
  title="Total GDP (Nominal USD, trillions) — Selected Countries since 1950"
  xAxisTitle="Year"
  yAxisTitle="GDP (USD trillions)"
  yFmt="#,##0.0"
/>

```sql gdp_share_major
select
  year,
  countryname,
  iso3,
  nGDP_USD / nullif(sum(nGDP_USD) over (partition by year), 0) as share_world
from gmd
where iso3 in ('USA','CHN','JPN','DEU','IND','GBR','FRA')
  and year >= 1950
order by year, countryname
```

<LineChart
  data={gdp_share_major}
  x=year
  y=share_world
  series=countryname
  title="Share of World GDP (Nominal) — Selected Countries since 1950"
  xAxisTitle="Year"
  yAxisTitle="Share of World GDP"
  yFmt="#,##0.0%"
/>

---

## 9 GDP per Capita since 1950 (Selected Countries)

```sql rgdp_pc_major
select
  year,
  countryname,
  iso3,
  rGDP_pc
from gmd
where iso3 in ('USA','CHN','JPN','DEU','IND','GBR','FRA')
  and year >= 1950
order by year, countryname
```

<LineChart
  data={rgdp_pc_major}
  x=year
  y=rGDP_pc
  series=countryname
  title="GDP per Capita (Real, constant) — Selected Countries since 1950"
  xAxisTitle="Year"
  yAxisTitle="GDP per Capita (USD, constant)"
  yFmt="#,##0"
/>

---

Interpretation Guide
- Duplicates: Any rows in dup_iso3_year indicate key conflicts; upstream deduping may be required.
- Missingness: Non-trivial missingness in rGDP_pc or pop will bias aggregates; consider imputation rules.
- Invalids: Negative GDP per capita or non-positive population are red flags; filter or correct upstream.
- Gaps: Large gaps reduce time-series comparability; treat cautiously in trend analysis.
- Outliers: z ≥ 4 likely warrant manual inspection or winsorization when aggregating.


## 4. Outcomes Today — Divergence Realized

By the 21st century, the cumulative effect of these different paths was a stark divergence. South Korea had become a wealthy, industrialized nation integrated into global high-tech value chains, while the Latin American economies remained middle-income countries, often reliant on commodity exports.

```sql latest_outcomes_table
select
  countryname,
  iso3,
  year,
  rGDP_USD / nullif(pop, 0) as rgdp_pc_usd,
  exports_GDP / 100 as exports_GDP,
  inv_GDP / 100 as inv_GDP,
  infl
from gmd
where iso3 in ('JPN', 'KOR', 'BRA', 'ARG')
  and year = (select max(year) from gmd where iso3 in ('JPN', 'KOR', 'BRA', 'ARG') and rGDP_USD is not null)
order by rgdp_pc_usd DESC
```

<DataTable
    data={latest_outcomes_table}
>
    <Column id=countryname title="Country" />
    <Column id=rgdp_pc_usd title="GDP per Capita (USD)" fmt="$#,##0" />
    <Column id=exports_GDP title="Exports (% GDP)" fmt="0.1%" />
    <Column id=inv_GDP title="Investment (% GDP)" fmt="0.1%" />
    <Column id=infl title="Inflation" fmt="0.1%" />
</DataTable>

