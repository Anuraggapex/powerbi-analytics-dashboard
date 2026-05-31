# 📊 Tech Stock Performance Dashboard (2010–2024)

A Power BI dashboard that visualizes stock performance trends for five major tech companies — **Apple (AAPL)**, **Amazon (AMZN)**, **Google (GOOGL)**, **Microsoft (MSFT)**, and **NVIDIA (NVDA)** — across a 14-year period from 2010 to 2024.

---

## 📸 Dashboard Screenshots

### Overall View (All Companies, All Years)
![Overall Dashboard](Screenshot_2026-05-31_162744.png)
> Full dashboard showing aggregated metrics across all five companies and all years (2010–2024).

---

### AAPL – Apple Inc. (2024)
![AAPL Dashboard](Screenshot_2026-05-31_162849.png)
> Filtered view for Apple. Sum of Volume: **1.37bn** | Max High: **259.81** | Avg Close: **243.72**

---

### GOOGL – Alphabet Inc. (2024)
![GOOGL Dashboard](Screenshot_2026-05-31_162909.png)
> Filtered view for Google. Sum of Volume: **893.73M** | Max High: **201.19** | Avg Close: **181.67**

---

### MSFT – Microsoft Corporation (2024)
![MSFT Dashboard](Screenshot_2026-05-31_162928.png)
> Filtered view for Microsoft. Sum of Volume: **633.32M** | Max High: **455.25** | Avg Close: **432.37**

---

### AMZN – Amazon.com Inc. (2024)
![AMZN Dashboard](Screenshot_2026-05-31_162948.png)
> Filtered view for Amazon. Sum of Volume: **1.09bn** | Max High: **233.00** | Avg Close: **217.79**

---

### NVDA – NVIDIA Corporation (2024)
![NVDA Dashboard](Screenshot_2026-05-31_163004.png)
> Filtered view for NVIDIA. Sum of Volume: **6.39bn** | Max High: **152.87** | Avg Close: **138.31**

---

## 📈 Key Metrics (All Companies, All Years)

| Metric | Value |
|---|---|
| 💰 Total Volume | 3.41 Trillion |
| 📈 Max of High | 465.64 |
| 📉 Average of Close | 68.73 |
| 🏆 Top Company (by volume) | MSFT |

---

## 🗂️ Dashboard Components

### KPI Cards (Top Row)
Four summary cards displayed at the top of the dashboard:
- **Sum of Volume** — Total shares traded across the selected period
- **Max of High** — Highest price reached by the top company
- **Average of Close** — Mean closing price across selected companies/years
- **Top Company** — Company with the highest average closing price in the selected period

---

### 📊 Charts & Visuals

| Visual | Description |
|---|---|
| **Bar Chart** – Avg of Close & Max of High by Company | Side-by-side comparison of average close price and maximum high for each company |
| **Donut Chart** – Sum of Volume by Company | Proportional share of total trading volume per company |
| **Line Chart** – Sum of Volume by Company & Year | Volume trends across years for each company (2010–2024) |
| **Bar Chart** – % Change by Company (Last 30 Days) | Recent 30-day percentage price change per company |
| **Area Chart** – Variance of Close by Month & Company | Month-over-month variance in closing prices, filtered per company |

---

## 🔍 Filters & Interactivity

- **Year Slicer** — Select one or multiple years (2010–2024) to filter all visuals simultaneously
- **Company Logo Buttons** — Click on AAPL, AMZN, NVDA, MSFT, or GOOGL logos to cross-filter the dashboard to a single company
- **Cross-filtering** — Clicking any chart element updates all other visuals dynamically

---

## 🏢 Companies Covered

| Ticker | Company | Sector |
|---|---|---|
| AAPL | Apple Inc. | Consumer Electronics |
| AMZN | Amazon.com Inc. | E-Commerce / Cloud |
| GOOGL | Alphabet Inc. | Internet / Advertising |
| MSFT | Microsoft Corporation | Software / Cloud |
| NVDA | NVIDIA Corporation | Semiconductors / AI |

---

## 📌 Notable Insights

- **MSFT** leads in average closing price across all years (~$130 avg), with a maximum high of **$466**
- **NVDA** dominates in trading volume in 2024 (**6.39bn shares**), reflecting its AI-driven growth
- **AAPL** shows the highest variance in closing price month-over-month in 2024
- **MSFT** held the top position by volume historically, accounting for **55.07%** of total volume across all years
- **GOOGL** reached an average close of **$182** in 2024, with a max high of **$201**

---

## 🧹 Data Preprocessing (Power Query M)

The raw CSV data goes through a multi-step preprocessing pipeline in Power Query before being loaded into the data model.

### Preprocessing Steps

| Step | Transform | Purpose |
|---|---|---|
| 1 | **Remove Duplicates** | Drops exact duplicate rows that could skew aggregations |
| 2 | **Remove Null Dates** | Removes corrupt records with no date value |
| 3 | **Sort by Date** | Ensures chronological order before transformations |
| 4 | **Replace Null Prices** | Fills missing price cells with `0` instead of nulls |
| 5 | **Remove Zero Rows** | Drops rows where every price column is `0` (empty trading days) |
| 6 | **Filter Date Range** | Enforces the 2010–2024 scope, removing out-of-range records |
| 7 | **Add Quarter Column** | Adds `Q1/Q2/Q3/Q4` for quarterly trend analysis in visuals |

### Full Power Query M Code

```m
let
    Source = Csv.Document(File.Contents("C:\Users\Admin1234\Downloads\15 Years Stock Data of NVDA AAPL MSFT GOOGL and AMZN.csv"),[Delimiter=",", Columns=26, Encoding=1252, QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Date", type date}, {"Close_AAPL", type number}, {"Close_AMZN", type number}, {"Close_GOOGL", type number}, {"Close_MSFT", type number}, {"Close_NVDA", type number}}),

    // ── PREPROCESSING START ──────────────────────────────────────────

    // 1. Remove fully duplicate rows
    #"Removed Duplicates" = Table.Distinct(#"Changed Type"),

    // 2. Remove rows where Date is null (corrupt/missing records)
    #"Removed Null Dates" = Table.SelectRows(#"Removed Duplicates", each [Date] <> null),

    // 3. Sort by Date ascending for chronological order
    #"Sorted by Date" = Table.Sort(#"Removed Null Dates", {{"Date", Order.Ascending}}),

    // 4. Replace null price values with 0 (handles missing trading days)
    #"Replaced Null Prices" = Table.ReplaceValue(#"Sorted by Date", null, 0, Replacer.ReplaceValue,
        {"Close_AAPL", "Close_AMZN", "Close_GOOGL", "Close_MSFT", "Close_NVDA"}),

    // 5. Remove rows where all price columns are 0 (entirely empty records)
    #"Removed Zero Rows" = Table.SelectRows(#"Replaced Null Prices", each 
        not ([Close_AAPL] = 0 and [Close_AMZN] = 0 and [Close_GOOGL] = 0 and [Close_MSFT] = 0 and [Close_NVDA] = 0)),

    // 6. Filter to valid date range only (2010–2024)
    #"Filtered Date Range" = Table.SelectRows(#"Removed Zero Rows", each 
        Date.Year([Date]) >= 2010 and Date.Year([Date]) <= 2024),

    // 7. Add a Quarter column for time-based analysis
    #"Added Quarter" = Table.AddColumn(#"Filtered Date Range", "Quarter", each 
        "Q" & Text.From(Date.QuarterOfYear([Date])), type text),

    // ── PREPROCESSING END ────────────────────────────────────────────

    #"Unpivoted Other Columns" = Table.UnpivotOtherColumns(#"Added Quarter", {"Date", "Quarter"}, "Attribute", "Value"),
    #"Split Column by Delimiter" = Table.SplitColumn(#"Unpivoted Other Columns", "Attribute", Splitter.SplitTextByDelimiter("_", QuoteStyle.Csv), {"Attribute.1", "Attribute.2"}),
    #"Renamed Columns" = Table.RenameColumns(#"Split Column by Delimiter",{{"Attribute.1", "Type"}, {"Attribute.2", "Company"}, {"Value", "Price"}}),
    #"Inserted Year" = Table.AddColumn(#"Renamed Columns", "Year", each Date.Year([Date]), Int64.Type),
    #"Inserted Month" = Table.AddColumn(#"Inserted Year", "Month", each Date.Month([Date]), Int64.Type),
    #"Inserted Day Name" = Table.AddColumn(#"Inserted Month", "Day Name", each Date.DayOfWeekName([Date]), type text),
    #"Removed Blank Rows" = Table.SelectRows(#"Inserted Day Name", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    #"Changed Type1" = Table.TransformColumnTypes(#"Removed Blank Rows",{{"Date", type date}, {"Price", type number}, {"Company", type text}, {"Year", Int64.Type}}),
    #"Pivoted Column" = Table.Pivot(#"Changed Type1", List.Distinct(#"Changed Type1"[Type]), "Type", "Price", List.Sum)
in
    #"Pivoted Column"
```

---

## 🛠️ Built With

- **Microsoft Power BI Desktop**
- Data source: Historical stock price data (2010–2024) for 5 major tech companies
- Visualizations: Bar charts, donut chart, area chart, line chart, KPI cards
- Interactivity: Slicers, cross-filtering, logo-based company filters

---

## 📁 File Structure

```
tech-stock-dashboard/
│
├── README.md                          # This file
├── TechStockDashboard.pbix            # Power BI project file
└── screenshots/
    ├── Screenshot_2026-05-31_162744.png   # Overall view
    ├── Screenshot_2026-05-31_162849.png   # AAPL view
    ├── Screenshot_2026-05-31_162909.png   # GOOGL view
    ├── Screenshot_2026-05-31_162928.png   # MSFT view
    ├── Screenshot_2026-05-31_162948.png   # AMZN view
    └── Screenshot_2026-05-31_163004.png   # NVDA view
```

---

## 🚀 How to Use

1. Open `TechStockDashboard.pbix` in **Power BI Desktop**
2. Use the **Year slicer** on the left to filter by specific years
3. Click on any **company logo** at the top-left to isolate a single company's data
4. Hover over any chart element to see detailed tooltips
5. Click on any bar, slice, or data point to **cross-filter** all other visuals

---

*Dashboard created to analyze and compare tech stock performance trends from 2010 to 2024.*
