---
layout: default
title: "Part 11 — Filter Context & CALCULATE"
parent: Notes
nav_order: 11
---

# Part 11 — Filter Context & CALCULATE

*A deep dive into the single most important DAX topic: filter context, CALCULATE, and Context Transition.*

Part 10 introduced `CALCULATE()` and filter context briefly. This part goes deep on both — and honestly, if you only fully understand one topic in all of DAX, make it this one. Filter Context, `CALCULATE()`, and Context Transition together cover most of what real-world Power BI development actually looks like.

## 1. What Is Filter Context?

**Filter Context** is simply the set of filters active when a measure gets calculated. Every measure is evaluated inside whatever filters happen to be applied at that moment — from slicers, visuals, report/page filters, relationships, or cross-filtering between visuals.

Say GreenCart's Orders table looks like this:

| Year | Region | Sales |
|---|---|---|
| 2024 | North | 100 |
| 2025 | North | 150 |
| 2025 | South | 200 |

A report slicer set to `Year = 2025` automatically filters the Orders table. So the measure:

```dax
Revenue = SUM(Orders[Sales])
```

returns **350** (150 + 200) — not because the formula changed, but because the Filter Context is now "Year = 2025." Same measure, different context, different answer.

## 2. Where Filter Context Comes From

Filters build up automatically from several places at once: report filters, page filters, visual filters, slicers, relationships, and cross-filtering between visuals. A common chain looks like:

**Date Slicer → Date Table → Orders Table → Revenue Measure**

The relationship between the Date table and Orders table is what propagates the filter down automatically — this is the same relationship concept from Part 8's star schema.

## 3. Row Context vs. Filter Context

One of the most frequently asked Power BI interview questions, and worth having a rock-solid mental model for.

| | Row Context | Filter Context |
|---|---|---|
| Operates on | The current row | The current set of filters |
| Used by | Calculated Columns | Measures |
| Scope | One row at a time | The entire filtered dataset |

**The easy trick:** Calculated Column thinks *"this row."* Measure thinks *"these filtered rows."*

> ⚠️ **Interview trap:** people often assume both contexts behave the same way just because both "filter" the data. They don't — a calculated column's `Profit = Orders[Sales] - Orders[Cost]` only ever knows about its own row, with zero awareness of slicers or filters. A measure has the opposite problem: it has no concept of "current row" at all until something (like `CALCULATE`) creates one.

## 4. Why CALCULATE() Matters

`CALCULATE()` is the single most powerful function in DAX because it changes the Filter Context *before* a measure gets evaluated.

```dax
CALCULATE(
  Expression,
  Filter1,
  Filter2
)
```

Read it as: *"calculate this expression, but under my filter conditions instead of whatever's already applied."*

## 5. A Simple CALCULATE Example

```dax
Revenue = SUM(Orders[Sales])

Revenue Dairy =
CALCULATE(
  [Revenue],
  Products[Category] = "Dairy"
)
```

Power BI temporarily filters the Products table to only Dairy before calculating Revenue — regardless of what a report-level slicer says.

## 6. Boolean Filters

The simplest kind of filter — an expression that returns TRUE or FALSE, like `Products[Category] = "Dairy"` or `Products[ListPrice] > 100`.

## 7. FILTER() — For When Simple Isn't Enough

```dax
Revenue High Margin =
CALCULATE(
  [Revenue],
  FILTER(
    Products,
    Products[ListPrice] > Products[Cost] * 2
  )
)
```

This includes only products whose selling price is more than double their cost — a condition too complex for a plain Boolean filter.

> ⚠️ **Interview trap:** using `FILTER()` for a condition that's actually simple (`FILTER(Products, Products[Category] = "Dairy")` instead of just `Products[Category] = "Dairy"`) works, but it's slower — `FILTER()` scans row by row. Use a Boolean filter whenever the condition allows it.

## 8. What Happens When CALCULATE Adds a Filter?

There are only two possible outcomes:

**No existing filter** — Power BI simply adds it. If no Region filter exists yet, `CALCULATE([Revenue], Orders[Region] = "South")` sets Region = South.

**A filter already exists** — Power BI *replaces* it, not combines with it. If the report already filters to `Region = North` and your measure says `Region = South`, the measure ignores North entirely and uses South.

> ⚠️ **This is the single most misunderstood CALCULATE behavior.** People instinctively expect filters to "stack" (North AND South), but CALCULATE overwrites by default — it doesn't add on top. If you actually want to preserve the existing filter, that's what `KEEPFILTERS()` is for (below).

## 9. REMOVEFILTERS()

Removes filters entirely.

```dax
Revenue Total =
CALCULATE(
  [Revenue],
  REMOVEFILTERS(Products)
)
```

Now Revenue calculates across all products, ignoring any slicer or filter — commonly used for grand totals.

## 10. Percentage of Total — A Very Common Interview Question

```dax
Revenue % =
DIVIDE(
  [Revenue],
  CALCULATE([Revenue], REMOVEFILTERS(Products))
)
```

| Product | Revenue | Revenue % |
|---|---|---|
| Fruits & Veg | 300 | 30% |
| Dairy | 700 | 70% |

The denominator ignores the product filter (via `REMOVEFILTERS`) so it always represents the grand total, while the numerator stays filtered to each row.

## 11. KEEPFILTERS()

Normally `CALCULATE` replaces existing filters — `KEEPFILTERS()` makes it combine with them instead.

```dax
Revenue Dairy =
CALCULATE(
  [Revenue],
  KEEPFILTERS(Products[Category] = "Dairy")
)
```

Now "Dairy" gets combined with whatever's already filtered, rather than overwriting it. Use this whenever you don't want `CALCULATE` to silently discard an existing filter.

## 12. USERELATIONSHIP()

GreenCart's Orders table might have both `OrderDate` and `DeliveryDate`, but only one relationship to the Date table can stay active at a time (as covered in Part 9). To use the inactive one temporarily:

```dax
Revenue by Delivery Date =
CALCULATE(
  [Revenue],
  USERELATIONSHIP(Date[Date], Orders[DeliveryDate])
)
```

## 13–17. Reading the Current Filter Context

A handful of functions exist just to *inspect* filter context rather than change it:

- **VALUES()** — returns the values currently in context. If a slicer selects "India," `VALUES(Customers[Country])` returns India.
- **HASONEVALUE()** — TRUE if exactly one value is selected, FALSE if multiple.
- **SELECTEDVALUE()** — returns the single selected value, or BLANK (or a fallback you specify) if multiple are selected.
- **ISFILTERED()** — checks whether a column is directly filtered.
- **ISINSCOPE()** — checks whether a hierarchy level (like Month, inside a Year → Quarter → Month hierarchy) is currently being displayed — useful for drill-down-aware measures.

> ⚠️ **Interview trap:** `HASONEVALUE()` and `SELECTEDVALUE()` get confused constantly. `HASONEVALUE()` returns a TRUE/FALSE check; `SELECTEDVALUE()` returns the actual value (or BLANK). In practice, `SELECTEDVALUE()` is usually the better choice since it already handles the "multiple values selected" case internally.

## 18. Context Transition — The Hardest DAX Concept

Calculated Columns normally use Row Context; Measures normally use Filter Context. **Context Transition** is what happens when you make a Calculated Column behave like a Measure — and `CALCULATE()` is what performs it.

Without `CALCULATE`, `SUM(Orders[Sales])` inside a calculated column would just return the total for the *entire* table, ignoring the current row. Wrapped in `CALCULATE`, Power BI converts the current row into a filter first:

```dax
CALCULATE(SUM(Orders[Sales]))
```

Now the calculation becomes specific to that row, because `CALCULATE` turned the row into a one-row filter before evaluating.

**The trick to remember:** `CALCULATE` can convert Row Context into Filter Context — that's Context Transition, in one sentence.

## 19. Iterator Functions (Quick Review)

Functions ending in **X** — `SUMX`, `AVERAGEX`, `COUNTX`, `RANKX` — evaluate an expression row by row, then aggregate the result:

```dax
Revenue = SUMX(Orders, Orders[Quantity] * Orders[Price])
```

Think: **calculate first, per row — summarize after.** Use these whenever the value you need doesn't already exist as a column and has to be computed on the fly for each row.

## Quick Revision

| Concept | Remember |
|---|---|
| Filter Context | The current filters affecting a measure |
| CALCULATE() | Changes Filter Context |
| FILTER() | Builds complex, non-Boolean filter conditions |
| REMOVEFILTERS() | Strips filters — used for grand totals, % of total |
| KEEPFILTERS() | Combines with existing filters instead of replacing them |
| USERELATIONSHIP() | Temporarily activates an inactive relationship |
| VALUES() / SELECTEDVALUE() | Read what's currently selected in Filter Context |
| Context Transition | CALCULATE() converting Row Context into Filter Context |
| Iterator (X) Functions | Row-by-row calculation, then aggregation |

**Interview Questions Worth Practicing Out Loud**
- What is Filter Context, in your own words?
- Row Context vs. Filter Context — what's the one-line distinction?
- What happens when `CALCULATE` adds a filter that already exists — does it stack or replace?
- Why would you use `REMOVEFILTERS()` for a percentage-of-total calculation?
- What is Context Transition, and what causes it?

## 🎯 Final Takeaway

You don't need to memorize every DAX function for an interview — but you do need to be fluent in seven ideas: **Filter Context** (measures always calculate within it), **CALCULATE()** (the function that changes it), **REMOVEFILTERS()** (for grand totals and % of total), **USERELATIONSHIP()** (activating inactive relationships), **Row Context vs. Filter Context**, **Context Transition**, and **Iterator functions**. If you can explain each of these with a simple example — not just a definition — you're genuinely ready for both PL-300 and most Data Analyst interviews.
