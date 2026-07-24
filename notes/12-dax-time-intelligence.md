---
layout: default
title: "Part 12 — DAX Time Intelligence"
parent: Notes
nav_order: 12
---

# Part 12 — DAX Time Intelligence

**Time Intelligence** just means date-based calculations — YTD, MTD, QTD, Year-over-Year growth, new customers this month. All of it works by automatically shifting the date filter context.

**Prerequisite:** none of this works without a proper Date table, marked as the Date Table, with continuous, unique dates — the same requirement from Part 9.

## The Building Blocks

**FIRSTDATE() / LASTDATE()** — return the first/last date in the current filter context. If the filter is "August 2025," `LASTDATE()` returns Aug 31.

**DATESBETWEEN()** — returns every date between two given dates. Useful for custom ranges like Life-to-Date totals.

**New Customers** — a common interview scenario. A GreenCart customer counts as "new" only in the month of their *first* order:

```
New Customers = Customers till today − Customers before this month
```

## Snapshot Data — The Trap Almost Everyone Falls Into

Some GreenCart data — warehouse inventory, delivery-partner count — is a **snapshot**: a value true at one point in time, not something that accumulates like sales.

> ⚠️ **The mistake:** summing snapshot values across dates. Monday's stock + Tuesday's stock is meaningless — stock isn't cumulative. The correct move is always **the latest available value**, not a sum.

**LASTNONBLANK()** solves the real-world version of this: if GreenCart's inventory data only goes up to June 15 but the report shows through June 30, `LASTNONBLANK()` correctly returns June 15 instead of a blank. **FIRSTNONBLANK()** does the same in reverse — the first date that actually has data.

> ⚠️ **Interview trap:** `LASTDATE()` returns the last date *in the filter context* — even if there's no data for it. `LASTNONBLANK()` returns the last date that actually *has* data. Mixing these up is a very common mistake with inventory-style reports.

## Quick Revision

| Concept | Remember |
|---|---|
| Time Intelligence | Date-based calculations (YTD, MTD, YoY) |
| Date Table | Mandatory, must be marked as Date Table |
| DATESBETWEEN() | Dates between two bounds |
| Snapshot Data | Never sum across dates — use the latest value |
| LASTNONBLANK() | Last date with actual data (vs. LASTDATE, which ignores that) |

**Interview Tip:** If asked about inventory/balance-style reporting, lead with the snapshot rule — "never sum, always take the latest value" — it's the single most-tested idea in this topic.
