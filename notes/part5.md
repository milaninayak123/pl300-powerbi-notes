---
layout: default
title: "Part 5 — Optimizing Performance & Fixing Import Errors"
parent: Notes
nav_order: 5
---

# Part 5 — Optimizing Performance & Fixing Import Errors

Getting GreenCart's data in is one thing; getting it in *fast*, and knowing what to do when something breaks, is the other half of the job.

## Query Folding — Still the Most Important Lever

We introduced this in Part 4, but it's worth returning to since it's the single biggest performance factor. Instead of pulling all of GreenCart's Orders table and filtering for this year inside Power BI, Query Folding pushes that filter back to SQL Server itself:

```sql
SELECT * FROM Orders WHERE Year = 2026;
```

**Check if it's happening:** right-click the last step in Power Query's Applied Steps → **View Native Query**. If that option is available, folding is occurring.

**When it breaks:** some steps can't fold — adding an Index Column, merging/appending data from different sources, and certain data type conversions all stop folding.

## Query Diagnostics

When a refresh is slow and it's not obvious why, **Query Diagnostics** (Power Query → Tools → Start Diagnostics) shows exactly which transformation step is eating the most time — useful for actually investigating a bottleneck in, say, GreenCart's delivery-time calculation, rather than guessing.

## Other Performance Habits

- Push as much processing as possible to the source, not Power BI
- Use native SQL queries where you can
- Keep Date and Time as separate columns
- Import only the columns and rows you actually need

## Fixing Common Import Errors

**"Query Timeout Expired"** — too much data requested at once. Fix: filter or aggregate in the SQL query itself, or import fewer rows/columns. If GreenCart's full 10-million-row Orders table times out, filter to just this year before importing.

**"We Couldn't Find Any Data Formatted as a Table"** — happens with Excel imports if the range isn't a real Excel table. Fix: select the data in Excel, press **Ctrl + T**, save, and re-import.

**"Couldn't Find File"** — the source file moved or access was lost. Fix: Power Query → Query Settings → Source → update the file path.

**Data Type Errors** — Power BI guessed the wrong type, causing blanks or failures. Fix by explicitly casting before import:

```sql
SELECT CAST(CustomerPostalCode AS VARCHAR(10))
FROM Customers;
```

## Quick Revision

**Key Takeaways**
- Query Folding = performance's biggest lever; verify with "View Native Query"
- Query Diagnostics pinpoints exactly which step is slow
- Most import errors trace back to too much data, wrong formatting, or a moved file

**Interview Tip:** Be ready to name at least two transformations that break Query Folding (merging/appending across sources and adding an Index Column are the two most commonly cited).

**Common Mistake to Avoid:** Trying to fix a slow refresh by adding more Power BI-side transformations, when the real fix is almost always pushing the work back to the source via folding.
