---
layout: default
title: "Part 16 — Designing Power BI Reports: Layout, Visuals & Filters"
parent: Notes
nav_order: 16
---

# Part 16 — Designing Power BI Reports: Layout, Visuals & Filters

*Layout principles and a full guide to choosing the right visual for the job.*

With the groundwork from Part 15 in place, here's how a GreenCart report actually gets built and laid out.

## Report Structure

**Semantic Model → Report → Pages → Visuals + Elements**

A report can have multiple **pages** (add, rename, duplicate, hide, reorder). Best practice: first page = summary/dashboard, later pages = detailed analysis. Keep unfinished pages hidden, and lean on tooltips/drillthrough pages instead of endlessly adding new ones.

## Good Dashboard Design — Five Principles

The overall rule: **less is more.**

- **Placement** — most important visual top-left (for left-to-right readers), arranged left→right, top→bottom
- **Balance** — distribute visuals evenly; don't stack everything on one side
- **Proximity** — keep related visuals together (GreenCart's Sales KPI card, Sales Trend line, and Sales by Region chart should sit as a cluster, not scattered)
- **Contrast** — use color to highlight what matters (red = bad, green = good, blue = highlight) — don't highlight everything, or nothing stands out
- **Repetition/Consistency** — same fonts, colors, card styles, and spacing throughout; use Power BI **Report Themes** to enforce this automatically

Also worth keeping in mind: leave real white space, keep margins even, size visuals by importance (bigger = more important), and use Power BI's alignment tools rather than eyeballing placement.

## Report Objects: Visuals vs. Elements

**Visuals** show data — bar charts, line charts, tables, maps, KPIs. **Elements** don't — text boxes, buttons, shapes, images, used for titles, navigation, logos, and bookmark triggers.

## Choosing the Right Visual — The Question That Always Comes Up

This is one of the most common practical interview topics, so it's worth having a clean mental map.

**Bar / Column Chart** — comparing categories. GreenCart: Sales by Product, Sales by City. Sort by value, not alphabetically — a chart sorted A-Z instead of by size hides the story.

**Line Chart** — time series, trends. GreenCart: Revenue by Month. Don't use a line chart for categories — a line implies continuity that categories don't have.

**Stacked Bar/Column** — comparing proportions within categories. GreenCart: how much each product category contributes to total sales, month by month.

**Pie / Doughnut Chart** — part-to-whole, but only with a *few* categories. GreenCart: share of orders by payment method (UPI, Card, COD) — fine with 3-4 slices, unreadable with 15.

**Card** — one important KPI, standalone. GreenCart: Total Orders Today.

**Multi-row Card** — several KPIs shown together (Orders, Revenue, Active Riders in one tile group).

**Table** — detailed, row-level records. GreenCart: a raw list of today's orders.

**Matrix** — data with a hierarchy, supports drill-down and conditional formatting. GreenCart: Year → Quarter → Month sales, expandable in place.

**KPI Visual** — actual vs. target over time. GreenCart: this month's orders vs. the monthly target.

**Gauge** — progress toward a single target. GreenCart: % of the monthly delivery target reached so far.

**Map** — only when geography is actually meaningful. GreenCart: Orders by City. Don't force a map onto data where location isn't the point — it looks impressive but adds no insight.

| Need | Best Visual |
|---|---|
| Compare categories | Bar / Column Chart |
| Trend over time | Line Chart |
| Part of a whole | Pie, Doughnut, Stacked Bar |
| Single KPI | Card |
| Multiple KPIs | Multi-row Card |
| Detailed records | Table |
| Hierarchical data | Matrix |
| Target vs. Actual | KPI / Gauge |
| Geographic data | Map |

> ⚠️ **Interview trap:** "when would you use a Table vs. a Matrix?" — Table is flat, row-level data; Matrix supports hierarchy and drill-down (Year → Quarter → Month). If the data has natural levels, Matrix is the right call, not Table.

## Filters — Five Levels

Power BI filters at five levels: **Row-Level Security (RLS)**, **Report**, **Page**, **Visual**, and **Measure** (via DAX, using `CALCULATE` from Part 11). The Filters Pane splits into Report/Page/Visual filters, and each can be hidden or locked from editing.

## Slicers — the User-Facing Version of a Filter

A **slicer** is an interactive filter placed directly on the report canvas — dropdown, list, date range, or numeric range — that viewers use themselves, and can be synced across pages.

| Filters | Slicers |
|---|---|
| Faster, more advanced logic | More user-friendly |
| Hidden in the Filters Pane | Visible on the report |
| Built for report logic | Built for interactive exploration |

**Simple rule: Slicers for users, Filters for report logic.**

## Other Interactivity Worth Knowing

- **Drillthrough** — right-click a data point to jump into a detailed report filtered to that context
- **Tooltips** — extra info shown on hover
- **Bookmarks** — save a report's current state, useful for reset-filter buttons, guided navigation, or storytelling walkthroughs
- **Visual interactions** — selecting one visual can filter others on the same page (and this can be disabled per pair of visuals if needed)

## Quick Revision

**Key Takeaways**
- Structure: Semantic Model → Report → Pages → Visuals/Elements
- Layout follows five principles: Placement, Balance, Proximity, Contrast, Consistency
- Visual choice maps directly to the question being asked — categories, trends, proportions, hierarchy, or geography
- Slicers are for users; Filters (Report/Page/Visual/Measure) are for report logic

**Interview Tip:** If asked "how would you choose a visual for X," always answer by naming *what question the data answers* first (comparison, trend, proportion, hierarchy, location), then map that to the chart — it shows reasoning, not memorization.

**Common Mistake to Avoid:** Defaulting to a pie chart for anything part-to-whole. With more than a handful of categories, a pie chart becomes unreadable — a stacked bar or a simple table usually communicates the same thing more clearly.
