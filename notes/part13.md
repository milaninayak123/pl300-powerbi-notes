---
layout: default
title: "Part 13 — Visual Calculations"
parent: Notes
nav_order: 13
---

# Part 13 — Visual Calculations

**Visual Calculations** are DAX written directly on a visual instead of in the semantic model — they only exist for that one chart or table.

> **Measure → model-level, reusable everywhere. Visual Calculation → visual-level, lives on one chart only.**

Instead of creating a `Profit` measure in the model, you can write it directly on a GreenCart sales table visual:

```dax
Profit = [Sales Amount] - [Total Product Cost]
```

It only works inside that one visual — nowhere else.

## Why Use Them

Quick, one-off calculations without cluttering the model: Running Sum, Moving Average, % of Parent, comparing a value to the previous/next row. Power BI ships ready-made **templates** for all of these, so you rarely write the DAX by hand. You can also hide the raw fields a calculation depends on (e.g., hide Sales Amount and Cost, show only Profit).

Two settings control how a Visual Calculation behaves:

- **Axis** — direction it moves through the visual: Rows (top to bottom, default) or Columns
- **Reset** — when it restarts, e.g., for a Running Sum inside a Year → Quarter → Month hierarchy: **None** (never resets), **Highest Parent** (resets every Year), **Lowest Parent** (resets every Quarter)

## Measure vs. Visual Calculation

| Measure | Visual Calculation |
|---|---|
| Stored in the model | Stored only on one visual |
| Reusable across reports | Works only in that visual |
| Can use the full data model | Limited to fields already in the visual |
| Best for business KPIs | Best for quick, one-off visual analysis |

**Limitation worth knowing:** Visual Calculations can't reference relationship-based functions like `RELATED()` or `USERELATIONSHIP()` — they only see what's already in the visual.

## Quick Revision

**Key Takeaway:** Use Measures for reusable KPIs (Revenue, Profit, YTD). Use Visual Calculations for a quick, one-chart-only need (Running Total, Moving Average, % of Parent) — they simplify DAX without adding permanent clutter to the model.

**Interview Tip:** If asked when *not* to use a Visual Calculation — any time the same logic needs to be reused across multiple visuals or reports, since it can't be shared like a Measure can.
