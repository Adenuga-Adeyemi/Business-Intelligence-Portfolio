# DAX Measures and Calculation Logic

This file records the DAX and calculated-field logic explicitly documented in the Retail Performance Market Intelligence Report.

## Core Measures

```DAX
Total Revenue = SUM(Sales[Revenue])

Total Profit = SUM(Sales[Profit])

Profit Margin % =
DIVIDE([Total Profit], [Total Revenue])

Return Rate % =
DIVIDE(
    CALCULATE(
        COUNTROWS(Sales),
        Sales[Return_Flag] = "Yes"
    ),
    COUNTROWS(Sales)
)

Revenue (excl Returns) =
CALCULATE(
    [Total Revenue],
    Sales[Return_Flag] = "No"
)

Profit (excl Returns) =
CALCULATE(
    [Total Profit],
    Sales[Return_Flag] = "No"
)
```

## Calculated Fields

### Actual Revenue

```text
Units × Unit_Price_NGN × (1 − Discount_Pct)
```

### Total Cost

```text
Units × Cost_Per_Unit_Filled
```

### Profit

```text
Actual Revenue − Total Cost
```

### Is Return

```text
1 if Return_Flag = "Yes"
0 otherwise
```

## Date Table

The documented date table uses a calendar between the minimum and maximum sales dates and adds year, month, quarter, week, weekday and weekend attributes.

```DAX
DateTable =
ADDCOLUMNS(
    CALENDAR(
        MIN(Sales[Date]),
        MAX(Sales[Date])
    ),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "Month Short", FORMAT([Date], "MMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Quarter Number", QUARTER([Date]),
    "Year-Month", FORMAT([Date], "YYYY-MM"),
    "Week Number", WEEKNUM([Date], 2),
    "Day", DAY([Date]),
    "Day Name", FORMAT([Date], "dddd"),
    "Day Short", FORMAT([Date], "ddd"),
    "Day of Week", WEEKDAY([Date], 2),
    "Is Weekend", IF(WEEKDAY([Date], 2) >= 6, "Yes", "No")
)
```

## Return Handling

For performance analysis, transactions with `Return_Flag = "Yes"` were excluded from profitability, market/category rankings and monthly performance measures. Returns were retained for return-rate analysis.

## Dynamic KPI Logic

The source documentation also contains month-over-month text measures and conditional-formatting measures for:

- Profit
- Profit Margin
- Revenue by State
- Revenue by Product
- Revenue by Sales Representative
- Revenue by Channel
- Return Rate by Product Category
- Revenue by Month

The complete expressions are preserved in the source Medium article referenced in the project README.
