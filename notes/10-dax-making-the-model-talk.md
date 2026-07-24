---
layout: default
title: "Part 10 — DAX: Making the Model Talk"
parent: Notes
nav_order: 10
---

# Part 10 — DAX: Making the Model Talk

GreenCart's model is clean, structured, and named sensibly. Now it needs to actually answer questions — total sales, profit margin, year-over-year growth. That's what **DAX (Data Analysis Expressions)** is for.

> **Power Query prepares the data. DAX analyzes it.** Power Query's work happens before data loads into the model; DAX works after — turning stored data into calculations, KPIs, and business logic.

## The Three Things DAX Can Create

| Type | Purpose | Stored in memory? |
|---|---|---|
| **Calculated Table** | Builds an entirely new table | ✅ Yes |
| **Calculated Column** | Adds a value to every row | ✅ Yes |
| **Measure** | Calculates dynamically, on demand | ❌ No |

**Calculated Table** — builds a new table from existing model data. GreenCart might use this to create a second copy of the Date table for a different relationship (e.g., `Delivery Date` alongside `Order Date`). ⚠️ These increase model size, since they're stored just like any other table.

**Calculated Column** — computes one value *per row*. `Profit = Orders[OrderAmount] - Orders[Cost]` adds a stored Profit value to every row in GreenCart's Orders table. Use this when each row genuinely needs its own stored value — for instance, something you'll filter or group by.

**Measure ⭐** — the one you'll use constantly. `Total Sales = SUM(Orders[OrderAmount])` only calculates when a visual actually asks for it. If a regional manager filters to `Region = West`, Total Sales recalculates for West on the fly — nothing pre-stored, nothing wasted.

| Calculated Column | Measure |
|---|---|
| Calculated for every row | Calculated only when needed |
| Stored in memory | Not stored |
| Increases model size | Doesn't increase model size |
| Row-level calculations | KPIs, totals, averages, YTD |

> ⚠️ **Interview trap — near-guaranteed question:** "why use a Measure instead of a Calculated Column?" The core answer is that measures are dynamic and don't bloat the model — use calculated columns only when a value must exist at the row level (like something you'll slice by), and measures for everything else.

## Basic Syntax and Referencing

Every DAX formula: `Calculation Name = Formula`. Columns are written as `TableName[Column]` (e.g., `Orders[OrderAmount]`); measures are written as `[Measure Name]` with no table prefix (e.g., `[Total Sales]`).

## The Functions You'll Use Constantly

**SUM()** — `Total Sales = SUM(Orders[OrderAmount])`

**AVERAGE()** — `Average Order Value = AVERAGE(Orders[OrderAmount])`

**COUNT()** — counts rows/values: `COUNT(Customers[CustomerID])`

**DISTINCTCOUNT()** — counts only unique values. If the same GreenCart customer placed 3 orders, `COUNT` counts 3, `DISTINCTCOUNT` counts 1 customer. Extremely common for "unique customer" KPIs.

**COUNTROWS()** — counts rows in a table or filtered table expression, often used inside CALCULATE for things like "number of orders this month."

**IF()** — `IF(Orders[OrderAmount] > 1000, "High Value", "Regular")`

**DIVIDE()** — safe division, automatically handling zero/BLANK denominators: `Profit Margin = DIVIDE([Profit], [Revenue])`. ⚠️ **Best practice:** always use `DIVIDE()` over `/` whenever the denominator could be zero — a common "what would you improve" interview follow-up.

**RELATED()** — pulls a value across a relationship, from the "one" side of a one-to-many relationship. If Orders links to Products by `ProductID`, `RELATED(Products[Category])` lets you pull a product's category into the Orders table.

## CALCULATE() — The Function That Actually Runs Power BI

If there's one function to be fluent in, it's this one. `CALCULATE()` changes the **filter context** a calculation runs in — it lets you override or add filters on top of whatever the report is already filtering by (slicers, visuals, etc.).

```dax
West Sales = CALCULATE([Total Sales], Orders[Region] = "West")
```

This forces `Total Sales` to always calculate for the West region, no matter what a report-level slicer is set to. It's the engine behind:

- **Year-over-year comparisons** — `CALCULATE` combined with `SAMEPERIODLASTYEAR`
- **YTD totals** — `CALCULATE([Total Sales], DATESYTD('Date'[Date]))`
- **"Ignore this filter" logic** — using `ALL()` inside `CALCULATE` to remove a filter (e.g., calculating a region's % of *total* GreenCart sales by ignoring the region filter just for the denominator)

> ⚠️ **Interview trap:** if asked "how would you calculate Year-to-Date sales" or "% of total sales by category," the expected answer almost always involves `CALCULATE`. Be ready to write a basic example from memory, not just describe it.

### Row Context vs. Filter Context

Two terms that explain *why* calculated columns and measures behave differently:

- **Row context** — DAX evaluates one row at a time. This is what calculated columns use.
- **Filter context** — the set of active filters (from slicers, visuals, or `CALCULATE`) that determines which rows a measure sees before it aggregates. This is what measures respond to.

> A measure recalculating when a manager clicks a region slicer isn't magic — it's filter context changing, and the measure re-evaluating against the newly filtered rows.

## Business Measures GreenCart Builds Constantly

```dax
Revenue = SUM(Orders[OrderAmount])
Profit = [Revenue] - [Cost]
Average Order Value = DIVIDE([Revenue], DISTINCTCOUNT(Orders[OrderID]))
Unique Customers = DISTINCTCOUNT(Customers[CustomerID])
```

Total Sales, Profit, Average Order Value, Customer Count, Profit Margin %, YTD Sales, Growth % — nearly every real dashboard is some combination of these.

## Variables — Cleaner, Faster DAX

```dax
Profit Measure =
VAR CurrentProfit = [Revenue] - [Cost]
RETURN CurrentProfit
```

`VAR` stores an intermediate result instead of recalculating it repeatedly — more readable, faster, and easier to debug once formulas get complex.

## BLANK() — Not the Same as Zero

DAX represents missing values as `BLANK()`, not `NULL`, and it isn't the same as 0 either:

```dax
IF(ISBLANK([Revenue]), 0, [Revenue])
```

⚠️ **Exam trick:** arithmetic operations often treat `BLANK` differently depending on context — `BLANK + BLANK` returns `BLANK`, but `SUM()` over a column of blanks and numbers ignores the blanks entirely rather than erroring. Don't assume BLANK behaves identically to zero in every formula.

## Quick Revision

**Key Takeaways**
- Power Query prepares; DAX analyzes
- Measures are dynamic and preferred over calculated columns for anything that isn't a genuine row-level value
- `CALCULATE()` is the mechanism behind nearly every advanced calculation — YTD, YoY, % of total
- Filter context (what measures respond to) is different from row context (what calculated columns use)

**Interview Questions Worth Practicing Out Loud**
- What is DAX, in one sentence?
- Calculated Column vs. Measure — and why does it matter for model size?
- Why `DIVIDE()` instead of `/`?
- `COUNT()` vs. `DISTINCTCOUNT()`?
- What does `CALCULATE()` actually do?
- What is `BLANK()`, and how is it different from zero?

**Common Mistake to Avoid:** Explaining measures as "dynamic" without being able to say *why* — the real answer is filter context: a measure re-evaluates against whatever the report is currently filtered to, and `CALCULATE` is how you deliberately override that.

## 🎯 Final Takeaway for This Chapter

For a Data Analyst interview, real fluency means being able to write — not just recognize — the everyday functions (`SUM`, `AVERAGE`, `DISTINCTCOUNT`, `IF`, `DIVIDE`, `CALCULATE`), explain filter context in your own words, and produce common business KPIs like Total Sales, Profit, and YTD from scratch on a whiteboard if asked.
