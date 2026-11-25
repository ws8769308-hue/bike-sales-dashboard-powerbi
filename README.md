# Sales Dashboard (Power BI)

![Dashboard](docs/dashboard.jpg)

A training project: a one-page Power BI dashboard of bike-shop sales across seven Egyptian cities, 2016 to 2018. The data is practice data, and the PUMA name and logo in the report are part of the exercise. This is not client work and not affiliated with PUMA.

## What is in the report
- KPI cards: Gross Sales, Discount Value, Net Sales, Quantity, Count of Category
- Slicers: Category, Date (year range)
- Map and ribbon chart: Net Sales by city and year
- Funnel: Net Sales by category
- Line chart: Net Sales by month

## Data model
Star schema: `SalesT` (fact) joined many-to-one to `Product` (on ProductID) and `Location` (on CityCode).

| Table | Rows | Columns |
|---|---|---|
| SalesT | 3,145,725 | ProductID, CityCode, Date, Quantity, Discount, Gross Sales, Disc_Value, Net Sales |
| Product | 25 | ProductID, ProductName, Category, Price |
| Location | 7 | City, CityCode |

Data was loaded from an Access database and cleaned in Power Query (trim, clean, proper-case, remove duplicate keys, typed columns). See `powerbi/power_query.m.txt` and `powerbi/dax_measures.md`.

## Headline numbers
| Metric | Value |
|---|---|
| Gross Sales | $217.6M |
| Discounts | $27.2M (12.5% of gross) |
| Net Sales | $190.4M |
| Units | 6.29M |

Net Sales by category: Bikes $148.1M (77.8%), Clothing $22.6M, Accessories $12.1M, Components $7.5M.
Four bike models make up all Bikes revenue. Cargo Bike ($54.1M) and Mountain Bikes ($49.0M) lead.

Net Sales by year: 2016 $63,381,894, 2017 $63,554,799, 2018 $63,468,233. These sum to $190,404,926 ($190.4M). Rounded to one decimal the three years show as $63.4M, $63.6M and $63.5M, which add up to $190.5M because of rounding.

## Checks done on the data
- Every SalesT row matches a Product and a Location (no orphan keys), no nulls.
- Report totals match a recomputation from the raw rows (Gross, Net, Quantity).
- Gross Sales equals Quantity x Price on every row.

## Limitations
- Net Sales is almost flat across cities (each city $27.1M to $27.3M), years and months, and Quantity (1 to 3) and Discount (10% to 15%) are evenly spread, as is common in training datasets. The project is about modelling and dashboard design, not business findings.
- SalesT has no transaction ID. About 1.08M rows (34%) are exact copies of another row, so true duplicates cannot be told apart from repeat sales. Totals include them, as the dashboard does.
- Prices are fixed per product, with no price history.
- `data/` holds only the aggregates and reference tables. The full 3.1M-row fact table is embedded in `powerbi/Sales_Dashboard.pbix`.
- The .pbix is the original file. Its Power Query still points to the author's local Access file (`SalesDB.accdb`) and it carries Power BI report IDs. Open it in Power BI Desktop and refresh will fail unless you repoint the source. The data already loaded in the model is kept.

## Interactive demo
`docs/dashboard_interactive_demo.mp4` is a recreation of the dashboard built from the full dataset with Python. Each slicer or map click recomputes the numbers from the real rows. It is not a recording of Power BI, and the map has no basemap.

The video has a Quran recitation (Surah Fatir 35:34-35) as audio. The audio was taken from a third-party clip supplied by the author. The reciter and rights holder are not identified, and the author makes no claim of ownership or licence over it. It will be removed on request.

## Files
- `powerbi/Sales_Dashboard.pbix`: the original report with the embedded data
- `data/reference/`: Product and Location tables
- `data/aggregates/`: CSV summaries by year, city, category, month, product
- `docs/dashboard.jpg`: screenshot
- `docs/dashboard_interactive_demo.mp4`: recreation demo (34s, with audio)
