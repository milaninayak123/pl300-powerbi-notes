---
layout: default
title: "Part 17 — Enhancing Report UX"
parent: Notes
nav_order: 17
---

# Part 17 — Enhancing Report UX

*Drillthrough, tooltips, buttons, bookmarks, and building a proper mobile layout.*

A report shouldn't dump everything on one page. The right flow is **High-Level KPIs → Supporting Charts → Detailed Data** — let the viewer choose how deep to go.

## Getting to Detail Without Clutter

**Drillable Visuals** — a **Matrix** with a Year → Quarter → Month hierarchy lets users expand deeper in place, instead of building a separate chart per level.

**Tooltips** — extra info on hover. A basic tooltip shows a few extra fields; a **Page Tooltip** shows a small mini-report page instead.

**Drillthrough** — right-click a point on GreenCart's Sales by Region chart to jump into a filtered "Region Transaction Details" page — the same summary-to-detail move behind the West-region investigation from Part 1.

> ⚠️ **Interview trap:** Tooltip vs. Drillthrough — a tooltip is a quick popup, no navigation; drillthrough actually opens a new, filtered page for real analysis.

## Highlighting What Matters

**Conditional Formatting** — on Tables/Matrices: background color, font color, data bars, or icons to flag values that need attention (e.g., regions below target in red).

**Analytics Pane** — adds reference lines to charts: trend line, average line, min/max line, forecast.

**Anomaly Detection** — Power BI's AI flags unusual points automatically on time-series charts (e.g., a sudden delivery-time spike).

## Making Reports Feel Interactive

**Buttons** turn a static report into something app-like: Back, Bookmark, Drillthrough, Page Navigation, Q&A, or open a Web URL.

**Bookmarks** save a report's current state — filters, slicers, active page, visual visibility — commonly used to reset slicers, swap between visuals, or guide a viewer through a story step by step.

> ⚠️ **Interview trap:** these two get bundled together, but they're different tools — a Button is the clickable trigger; a Bookmark is the saved state it applies. A "Reset Filters" button is really a Button triggering a Bookmark.

## Performance and Mobile

Same **Performance Analyzer** from Part 14 applies here too — if a report's slow, reduce visuals/fields, simplify DAX, or optimize the model.

**Mobile Layout** — Power BI supports a separate phone-optimized view: fewer visuals, arranged vertically, mobile-friendly slicers — since a warehouse manager checking GreenCart's delivery queue from a phone needs a very different layout than the analyst's desktop report.

## Quick Revision

**Key Takeaways**
- Structure reports summary-first: KPIs → charts → detail, using drillthrough/drillable visuals rather than crowding one page
- Buttons trigger actions; Bookmarks are the saved states those actions apply
- Build a dedicated Mobile Layout — don't rely on the desktop layout auto-shrinking

**Interview Tip:** If asked how you'd improve a "wall of charts" report, the expected answer is restructuring it around guided analytics (summary → detail) rather than just reformatting what's already there.
