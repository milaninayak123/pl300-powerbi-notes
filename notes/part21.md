---
layout: default
title: "Part 21 — Creating Dashboards in Power BI"
parent: Notes
nav_order: 21
---

# Part 21 — Creating Dashboards in Power BI

Parts 2 and 18 already covered the Dashboard vs. Report distinction — single-page, Service-only, built from pinned tiles. This part is about actually building one.

## Tiles and Pinning

A **Tile** is a visual pinned from a report onto a dashboard. Two ways to pin:

**Pin Visual** — pins one chart at a time. Lets you mix tiles from different GreenCart reports (Sales, Delivery, Inventory) onto a single CEO dashboard, and rearrange/resize them freely. Updates automatically whenever the source report is republished.

**Pin Live Page** — pins an entire report page at once, slicers and filters intact, and it updates live as the report changes.

> ⚠️ **Interview trap:** "when would you pin a live page vs. individual visuals?" — best practice is to prefer **individual visuals**, since pinning a whole page gives less control over dashboard layout and mixes concerns. Live Page pinning is really for cases where you want the full interactive experience preserved as-is.

**The build flow:** Create Report → Publish → Power BI Service → Pin Visuals → Dashboard.

## Data Alerts

Notify users when a KPI crosses a threshold — available only on **Card, KPI, and Gauge** tiles, with an above/below condition. GreenCart could set an alert if today's order count drops below a set number, notified via the Power BI notification center or email.

## AI Features on Dashboards

**Copilot** — natural-language exploration ("total sales by region," "top products this year"), can summarize a report, generate visuals and DAX, and pin the results straight to a dashboard. It's replacing the older Q&A dashboard feature, which is being retired.

**Quick Insights** — AI automatically scans data for trends and patterns and generates up to 32 insight cards, which can be pinned directly. **Import Mode only — not supported on DirectQuery** (Part 4/8).

> ⚠️ **Interview trap:** Quick Insights failing on a DirectQuery-based GreenCart Orders table isn't a bug — it's a hard limitation. This is a good example of a real tradeoff of choosing DirectQuery over Import.

## Appearance and Mobile

**Themes** — Light, Dark, color-blind friendly, custom, or an uploaded JSON theme, controlling background, tile colors, and fonts — for consistent branding across GreenCart's dashboards.

**Mobile View** — a separate phone-optimized layout (rearranged, resized, or hidden tiles), configurable from both Desktop and Service — same principle as the Mobile Layout for reports in Part 14/17.

## Quick Revision

**Key Takeaways**
- Pin individual visuals by default; pin a live page only when the full interactive experience needs to travel intact
- Data Alerts only work on Card/KPI/Gauge tiles
- Quick Insights requires Import Mode — DirectQuery isn't supported

**Interview Tip:** If asked to design a dashboard for an executive, mention the build flow explicitly (Report → Publish → Pin) — it shows you understand dashboards aren't built from scratch, they're assembled from existing reports.
