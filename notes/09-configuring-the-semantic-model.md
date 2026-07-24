---
layout: default
title: "Part 9 — Configuring the Semantic Model"
parent: Notes
nav_order: 9
---

# Part 9 — Configuring the Semantic Model

*Renaming, hiding, marking a Date table, and the Data Type vs. Format distinction that trips people up.*

GreenCart's model now has the right structure — a star schema with Orders as the fact table and Products, Customers, Dates as dimensions. But the tables are still full of database-style names and defaults nobody wants report authors to see. This part is about polishing that.

## Tables — Make Them Make Sense

`FactOrders` becomes `Orders`. A short **description** ("Orders table containing all customer transactions") helps anyone hovering over the table in the Data pane. **Synonyms** teach Power BI's Q&A/Copilot alternate names — if `Customer` has synonyms like Client or Buyer, someone asking "show sales by client" still gets the right answer.

Some tables — like a bridge table that only exists to support a relationship — should never be dragged into a visual directly. **Hiding** them keeps report authors from accidentally using something they shouldn't.

## The Date Table — Don't Skip This

Power BI auto-generates a hidden date table by default, but that's a beginner convenience, not something to rely on. Instead, GreenCart's analyst creates or imports a proper Date table and **marks it as the Date Table** — it needs unique, continuous dates with no gaps.

Why it matters: DAX time-intelligence functions — YTD, MTD, `SAMEPERIODLASTYEAR` — only work correctly against a properly marked date table.

## Columns — Same Idea, Smaller Scale

- **Rename**: `Cust_ID` → `Customer ID`
- **Hide** technical columns (IDs, foreign keys) — Power BI still uses them for relationships behind the scenes, but users don't see clutter
- **Data Category** tells Power BI what a column represents (City, Country, Web URL) so it renders the right map or link

### Data Type vs. Format — the Classic Mix-Up

> ⚠️ **Interview trap:** this comes up constantly. **Data Type** = how the value is *stored* (Whole Number, Decimal, Date, Text). **Format** = how it's *displayed* (1000 shown as ₹1,000.00). The underlying value never changes — only its appearance.

### Sort By Column

Alphabetical sorting breaks for months — April, August, December, February isn't useful order. Fix: add a hidden `MonthNumber` column (January = 1, February = 2...) and set `Month`'s **Sort By** property to it. Now GreenCart's monthly sales chart sorts chronologically, not alphabetically.

### Default Summarization

Numeric columns auto-aggregate — but not every number should be summed. `OrderAmount` makes sense as a Sum; `CustomerID` doesn't, so it should be set to **Don't Summarize**.

## Hierarchies

A hierarchy bundles related columns into one drill-down path — Year → Quarter → Month, for GreenCart's sales trends. Report authors drag one field instead of three, and viewers drill down naturally in a visual.

## Measures — A First Look

A **measure** is a DAX calculation — like `Total Sales = SUM(Orders[OrderAmount])` — that only computes when a visual actually needs it, rather than being stored per row. That makes it dynamic and efficient. We'll cover DAX properly in the next part, but it's worth knowing measures live in this same model-configuration layer.

If you'd rather not write DAX by hand yet, **Quick Measures** let you pick a calculation template (Running Total, Average per Category) and Power BI generates the formula for you.

## Parameters — Making the Report Interactive

**Numeric Range (What-If) Parameter** — lets users test scenarios without touching underlying data. A Delivery Fee slicer where selecting "₹20 per order" instantly recalculates GreenCart's projected profit measure, without changing a single actual row.

**Field Parameter** — lets users choose which field a visual breaks down by. Instead of building four separate charts (Sales by City, by Category, by Rider, by Month), GreenCart's analyst builds one chart with a Field Parameter, and the viewer picks what to see.

## Quick Revision

**Key Takeaways**
- Rename, describe, and hide tables/columns so the model is self-explanatory to anyone using it
- A proper Date table (marked as such) is required for time-intelligence DAX to work
- Data Type controls storage; Format controls display — they're independent
- Parameters make one report do the job of several

**Interview Tip:** If asked "how do you make a model report-ready," walk through this list in order — rename, hide, date table, data types, hierarchies — it demonstrates a systematic approach rather than a random list of features.

**Common Mistake to Avoid:** Relying on Power BI's auto-generated hidden date table in a production report instead of building a proper marked Date table — it works for quick testing but breaks down for real time-intelligence needs.
