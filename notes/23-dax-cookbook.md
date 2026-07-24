---
layout: default
title: "Part 23 — DAX Cookbook: Calculated Columns & Measures"
parent: Notes
nav_order: 23
---

# Part 23 — DAX Cookbook: Calculated Columns & Measures

*Parts 10 and 11 covered how DAX works. This part is the actual formulas, real DAX for the calculated columns and measures you'll write constantly, organized by scenario so you can find the pattern you need and adapt it.*

All examples use GreenCart's `Orders`, `Products`, `Customers`, and `Date` tables.

## Calculated Columns

A calculated column belongs on the table itself, computed row by row, stored, and usable as a filter/group field.

```dax
Profit = Orders[OrderAmount] - Orders[Cost]

Order Size = IF(Orders[OrderAmount] > 1000, "Large", "Regular")

Full Product Name = Products[Category] & " - " & Products[ProductName]

Order Month = FORMAT(Orders[OrderDate], "MMMM YYYY")
```

## The Everyday Measures

```dax
Total Revenue = SUM(Orders[OrderAmount])

Total Orders = COUNTROWS(Orders)

Unique Customers = DISTINCTCOUNT(Orders[CustomerID])

Average Order Value = DIVIDE([Total Revenue], [Total Orders])

Total Profit = SUM(Orders[OrderAmount]) - SUM(Orders[Cost])

Profit Margin % = DIVIDE([Total Profit], [Total Revenue])
```

## CALCULATE Patterns You'll Reuse Constantly

**Filtered to one value:**

```dax
West Region Revenue = CALCULATE([Total Revenue], Orders[Region] = "West")
```

**% of Total (ignoring the current filter for the denominator):**

```dax
Revenue % of Total =
DIVIDE(
    [Total Revenue],
    CALCULATE([Total Revenue], REMOVEFILTERS(Products))
)
```

**Year-to-Date:**

```dax
Revenue YTD = CALCULATE([Total Revenue], DATESYTD('Date'[Date]))
```

**Same Period Last Year:**

```dax
Revenue LY = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Date'[Date]))

Revenue Growth % = DIVIDE([Total Revenue] - [Revenue LY], [Revenue LY])
```

**New Customers This Month** (a genuinely common interview scenario, from Part 12):

```dax
New Customers =
VAR CustomersTillToday = CALCULATE(DISTINCTCOUNT(Orders[CustomerID]), Orders[OrderDate] <= MAX('Date'[Date]))
VAR CustomersBeforeThisMonth = CALCULATE(DISTINCTCOUNT(Orders[CustomerID]), Orders[OrderDate] < STARTOFMONTH('Date'[Date]))
RETURN CustomersTillToday - CustomersBeforeThisMonth
```

## Iterator (X) Functions

Use these when the calculation needs to happen row by row before it's summed:

```dax
Total Revenue (SUMX version) = SUMX(Orders, Orders[Quantity] * Orders[UnitPrice])

Average Basket Size = AVERAGEX(Orders, Orders[Quantity])
```

## Ranking

```dax
Product Rank by Revenue =
RANKX(ALL(Products), CALCULATE([Total Revenue]), , DESC)
```

Ranks every product by revenue, `ALL(Products)` makes sure the ranking ignores whatever's currently filtered, so a product's rank stays consistent regardless of what slicer is active elsewhere on the report.

## A Full Variable-Based Measure

```dax
Profit Summary =
VAR CurrentRevenue = [Total Revenue]
VAR CurrentCost = SUM(Orders[Cost])
VAR CurrentProfit = CurrentRevenue - CurrentCost
RETURN
    IF(
        ISBLANK(CurrentRevenue),
        BLANK(),
        CurrentProfit
    )
```

## Quick Revision

| Scenario | Pattern |
|---|---|
| Basic aggregation | `SUM`, `COUNTROWS`, `DISTINCTCOUNT`, `DIVIDE` |
| Filtered to one value | `CALCULATE([Measure], Condition)` |
| % of total | `CALCULATE` + `REMOVEFILTERS` in the denominator |
| YTD | `CALCULATE([Measure], DATESYTD('Date'[Date]))` |
| Year-over-year | `SAMEPERIODLASTYEAR` + a growth % formula |
| Row-by-row math before summing | `SUMX`, `AVERAGEX` |
| Ranking | `RANKX` with `ALL()` |

**Interview Tip:** if you're asked to write DAX live in an interview, narrate your thinking out loud as you build it, "I need the revenue total, but filtered to just this region, so I'll wrap it in CALCULATE" shows the same reasoning process this whole guide has been building toward, even if the syntax isn't perfect on the first try.
