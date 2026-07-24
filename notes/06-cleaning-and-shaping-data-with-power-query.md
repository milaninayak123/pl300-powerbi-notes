---
layout: default
title: "Part 6 — Cleaning & Shaping Data with Power Query"
parent: Notes
nav_order: 6
---

# Part 6 — Cleaning & Shaping Data with Power Query

*Headers, unpivot vs. pivot, replacing nulls, and why data types matter more than they look.*

GreenCart's raw Orders data is now connected — but it's rarely usable as-is. This part covers the everyday cleanup work in Power Query Editor.

## Getting Into the Editor

**Home → Transform Data** opens it: your table in the middle, all your tables listed on the left, every change you make logged as a step on the right. One thing worth knowing early: Power Query only edits a *view* — GreenCart's original database is never touched — and every step reruns automatically on each refresh.

## Fixing Headers and Names

- **Promote Headers** — if GreenCart's imported file has its real column names sitting as a data row instead of headers: Home → Use First Row as Headers
- **Rename Columns** — right-click → Rename, or double-click and retype

## Removing What You Don't Need

- **Remove Top Rows** — for junk rows above the real data
- **Remove Columns** — delete what you'll never report on, or flip it and use **Remove Other Columns** to keep only what matters

> ⚠️ **Interview trap:** "why remove columns as early as possible?" isn't really about tidiness — it's about model performance. Fewer columns means a smaller, faster model with cleaner relationships. You can always bring a column back later if needed.

## Unpivot vs. Pivot

GreenCart's finance team hands you a sheet with separate `2024` and `2025` columns for monthly sales — hard to total or trend together.

**Unpivot** collapses that wide layout into tall, analysis-friendly rows: select both year columns → Transform → Unpivot, then rename the results to `Year` and `SalesAmount`. Now you have a clean `Month / Year / SalesAmount` table, easy to build DAX on.

**Pivot** does the reverse — takes flat data and aggregates it into a summary table. Pivoting GreenCart's product-category column with a Count gives "how many products per category" as a compact table.

| | Direction | Use it when... |
|---|---|---|
| **Unpivot** | Wide → Long | Data needs to be analysis-ready |
| **Pivot** | Flat → Summarized | You want a compact aggregated view |

> ⚠️ **Common mix-up:** people remember *what* pivot/unpivot do but blank on *which is which* under pressure. Anchor it to the word itself: "un-pivot" undoes a pivot table's wide shape — turns it back into long rows.

## Cleaning Up Values

- **Rename a Query** — a table imported as `FactOrdersTable` becomes `Orders`
- **Replace Values** — fixes typos (a misspelled city name) at the column level
- **Replace Nulls** — a null delivery fee, left unhandled, will skew an average instead of counting as zero — replace it with the right default *before* aggregating
- **Remove Duplicates** — right-click a column → Remove Duplicates, useful for a clean lookup list (e.g., unique GreenCart product categories)

> ⚠️ **Interview trap:** "how would you handle nulls in a numeric column before averaging?" — the expected answer is replace with an appropriate default (often 0) *before* the aggregation runs, since an unhandled null silently distorts the result.

## Naming Conventions

Use business-friendly terms, replace underscores with spaces (`Order_Date` → `Order Date`), keep abbreviations consistent, and drop unnecessary prefixes. The test: would someone outside the analytics team understand the name without asking?

## Data Types Matter More Than They Look

Power BI guesses each column's type from the first 1,000 rows — and gets it wrong often, especially with human-entered Excel/CSV data. A GreenCart `OrderDate` column stuck as Text can't power a YTD measure:

```dax
Orders YTD = TOTALYTD(SUM(Orders[OrderQty]), Orders[OrderDate])
```

This errors out unless `OrderDate` is properly typed as Date — and you also lose the ability to build a Year → Month → Week drill-down hierarchy.

**Fix it in Power Query, before loading.** The fix gets logged as a step called **Changed Type**, which reapplies automatically every refresh — unlike a fix made later in Report view, which won't survive the next refresh.

## Quick Revision

**Key Takeaways**
- Power Query edits a view, never the source, and every step reruns on refresh
- Unpivot = wide → long; Pivot = flat → summarized
- Fix data types upstream in Power Query, not downstream in the report

**Interview Tip:** Whenever asked "why fix X in Power Query instead of later," the answer is almost always the same: Power Query steps survive refreshes; downstream fixes don't.

**Common Mistake to Avoid:** Leaving nulls unhandled before building an average or sum measure — it's one of the most common sources of a "wrong number" a stakeholder catches.
