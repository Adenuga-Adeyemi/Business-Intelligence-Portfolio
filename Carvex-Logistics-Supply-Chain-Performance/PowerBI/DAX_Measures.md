# DAX Measures

These measures are transcribed from the [source article](https://medium.com/@adeyemi.da/carvex-logistics-supply-chain-performance-dashboard-2610ac31b8fd). The referenced table is `Supply Chain`. Field names and formulas are retained as published; they have not been validated against the unavailable source workbook.

```DAX
// Core Volume & Revenue
Total Units Sold = SUM('Supply Chain'[Number of products sold])

Total Revenue = SUM('Supply Chain'[Revenue generated])

Total SKUs = DISTINCTCOUNT('Supply Chain'[SKU])

Average Selling Price = AVERAGE('Supply Chain'[Price])

// Quality & Compliance
Pass Count = CALCULATE(COUNTROWS('Supply Chain'), 'Supply Chain'[Inspection results] = "Pass")
Fail Count = CALCULATE(COUNTROWS('Supply Chain'), 'Supply Chain'[Inspection results] = "Fail")
Pending Count = CALCULATE(COUNTROWS('Supply Chain'), 'Supply Chain'[Inspection results] = "Pending")

Pass Rate % = DIVIDE([Pass Count], COUNTROWS('Supply Chain'), 0)

Average Defect Rate = AVERAGE('Supply Chain'[Defect rates])

// Inventory & Risk
Average Stock Level = AVERAGE('Supply Chain'[Stock levels])
Low Stock SKUs = CALCULATE(DISTINCTCOUNT('Supply Chain'[SKU]), 'Supply Chain'[Stock levels] < 20)

// Shipping & Cost
Total Shipping Cost = SUM('Supply Chain'[Shipping costs])
Average Shipping Cost = AVERAGE('Supply Chain'[Shipping costs])
Total Logistics Cost = SUM('Supply Chain'[Costs])

// Location / Carrier Performance
Revenue by Location = [Total Revenue]   // use in visuals with Location on axis
Units by Carrier = [Total Units Sold]   // use with Shipping carriers

// Comparison helpers (useful for cards)
Previous Period Units = 0   // placeholder – no date dimension exists
MoM Growth % = BLANK()      // can be replaced once a date table is added

Revenue per Unit = DIVIDE([Total Revenue], [Total Units Sold], 0)

Defect Cost Exposure = [Total Revenue] * [Average Defect Rate]   // rough proxy

High Risk SKUs =
CALCULATE(
    DISTINCTCOUNT('Supply Chain'[SKU]),
    'Supply Chain'[Defect rates] > 3 || 'Supply Chain'[Inspection results] = "Fail"
)

// Number of Active Suppliers
Number of Active Suppliers =
DISTINCTCOUNT('Supply Chain'[Supplier name])

// Average Lead Time
Average Lead Time =
AVERAGE('Supply Chain'[Lead time])

// Total Manufacturing Cost
Total Manufacturing Cost =
SUM('Supply Chain'[Manufacturing costs])
```

`Revenue by Location` and `Units by Carrier` are the base measures used with a location or shipping-carrier visual axis. `Previous Period Units` and `MoM Growth %` are expressly placeholders in the article. `Defect Cost Exposure` is called a rough proxy in the source; its output should not be interpreted as audited or directly incurred cost.
