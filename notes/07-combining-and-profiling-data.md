---
layout: default
title: "Part 7 — Combining & Profiling Data"
parent: Notes
nav_order: 7
---

# Part 7 — Combining & Profiling Data

*Append vs. Merge, and reading Column Quality, Distinct vs. Unique, and Column Profile before trusting your data.*

Two things in this part: stitching multiple GreenCart tables into one, and actually inspecting the data before trusting it.

## Append vs. Merge — the Pair People Mix Up

**Append** stacks rows from one table onto another. If GreenCart wants one master contact list combining `HR.Employees`, `Warehouse.Staff`, and `Sales.Customers`, appending stacks all their rows into a single table — 300 + 100 + 200 rows becomes 600. For this to work cleanly, the columns you care about (ID, name, phone) need identical names across all three source tables first.

**Merge** joins *columns* from one table into another using a shared key — like a SQL JOIN. Merging GreenCart's `Orders` and `OrderDetails` tables on their shared `OrderID` adds relevant columns from one into the other.

| Join Type | Returns |
|---|---|
| Left Outer | All rows from table 1 + matches from table 2 |
| Full Outer | All rows from both tables |
| Inner | Only rows that match in both |

> ⚠️ **Interview trap — near-guaranteed question:** Append = stacking rows (SQL `UNION`). Merge = joining columns on a key (SQL `JOIN`). If you remember only one line, remember that pairing.

## Profiling Data — Trust, But Verify

Before building a single visual, it's worth checking GreenCart's Orders data for anomalies. Turn this on via **View → Data Preview**:

**Column Quality** — the % of values that are Valid, Error, or Empty. Ideally close to 100% valid; anything less is worth investigating.

**Column Distribution** — shows **distinct** vs. **unique** counts, which sound similar but aren't:
- **Distinct** = every different value, duplicates included
- **Unique** = only values appearing exactly once

A `CustomerID` column of `[101, 101, 102, 103]` has 3 distinct values but only 2 unique ones (102, 103).

**Column Profile** — deeper stats on a column: row count, error/empty counts, min/max, and for numbers, average and standard deviation. The **Value Distribution** graph here is genuinely useful — if one rider's name shows up far more often than everyone else's in a `DeliveredBy` column, that's worth checking. It might be a real top performer, or it might be duplicated data.

> ⚠️ **Interview trap:** "why profile data before visualizing?" — the expected answer is that catching bad data here is far cheaper than catching it after a regional manager notices a wrong number on a live dashboard.

## Quick Revision

**Key Takeaways**
- Append stacks rows; Merge joins columns on a key
- Distinct counts every different value; Unique counts only values with zero repeats
- Column Profile's Value Distribution graph is where outliers usually surface

**Interview Tip:** Have the `[101, 101, 102, 103]` (or an equivalent) example ready to demonstrate Distinct vs. Unique on the spot — a live example lands better than a definition.

**Common Mistake to Avoid:** Appending tables whose columns aren't identically named first — this quietly produces a messy table full of near-duplicate columns instead of one clean one.
