---
layout: default
title: "Part 14 — Performance Optimization"
parent: Notes
nav_order: 14
---

# Part 14 — Performance Optimization

Every idea in this part serves one goal: **a smaller model performs better** — faster loads, faster refreshes, less memory.

## What Slows a Model Down

Poor model design, inefficient DAX, too many visuals on one page, large datasets, high-cardinality columns, and DirectQuery limitations (Part 4/8) are the usual culprits.

## Finding the Bottleneck: Performance Analyzer

Power BI's **Performance Analyzer** shows exactly how long each visual takes — split into DAX query time, visual rendering time, and other processing. Always start here before guessing what to optimize.

## The Core Fixes

**Use Variables (VAR).** Beyond the readability benefit from Part 10, storing a repeated calculation in a variable instead of recalculating it each time is a genuine performance win, not just a style preference.

**Watch Cardinality.** Cardinality = the number of unique values in a column. GreenCart's `Gender` or `City` columns have low cardinality; `OrderID` or `CustomerID` have high cardinality.

> ⚠️ **Remember:** high cardinality = more memory used, worse compression. Lower cardinality columns compress far better — this is a direct, concrete reason "remove unnecessary columns" is repeated so often as best practice.

**Get relationships right.** One-to-Many is the most common and generally the healthiest relationship type. Always match data types on both sides of a relationship key — `ProductID` as Integer on one side and Text on the other silently breaks things.

**Reduce model size.** Remove unused columns/rows, summarize detail where possible, avoid duplicated values, hide fields not meant for direct use.

**Use Aggregations** — store summarized data (Daily or Monthly Sales) instead of every raw transaction. GreenCart doesn't need 1 million individual order rows in memory if Daily Sales totals answer the same business questions faster. Trade-off: less row-level detail available.

**Import vs. DirectQuery, revisited.** Import is generally faster since everything's cached; DirectQuery trades speed for real-time data and no local storage, but performance then depends on the source database (full comparison in Part 4/8).

**Disable Auto Date/Time** if you're already using a proper custom Date table (Part 9) — otherwise Power BI silently builds a hidden date table per date column, bloating the model for no reason.

## Quick Revision

| Concept | Remember |
|---|---|
| Performance Analyzer | Finds exactly which visual/query is slow |
| VAR | Faster execution + more readable DAX |
| Cardinality | Fewer unique values = better performance |
| Aggregations | Pre-summarized data trades detail for speed |
| Auto Date/Time | Disable it if you already have a real Date table |

## 🎯 Final Takeaway — The 7 Golden Rules

1. Smaller models perform better
2. Use Performance Analyzer to find bottlenecks, don't guess
3. Use VAR for both speed and readability
4. Reduce cardinality where possible
5. Remove unnecessary columns/rows
6. Use aggregations for large datasets
7. Prefer Import over DirectQuery unless real-time data is genuinely required

Being able to explain these seven points covers most of what PL-300 and Data Analyst interviews ask about performance.
