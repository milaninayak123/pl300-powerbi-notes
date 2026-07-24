---
layout: default
title: "Part 19 — Managing Semantic Models"
parent: Notes
nav_order: 19
---

# Part 19 — Managing Semantic Models

*Gateways, scheduled vs. incremental refresh, and endorsing trusted models.*

A semantic model — GreenCart's Orders + Products + Customers structure from Parts 8–9 — shouldn't be rebuilt for every new report. This part covers keeping that one model current, trusted, and reusable.

## Why Manage Semantic Models Centrally

One shared model means one source of truth: no duplicate work, easier collaboration, and everyone's reports agree with each other because they're built on the same data.

## Gateway — Bridging On-Premises Data

If GreenCart's Orders database lives on an internal SQL Server (not the cloud), Power BI Service needs a **Gateway** to reach it securely. Cloud sources (SharePoint, OneDrive) don't need one.

- **Standard Gateway** — multi-user, multi-source, for organization-wide use
- **Personal Gateway** — single user, Power BI only, can't be shared, and the PC must stay on

## Keeping Data Fresh

**Scheduled Refresh** — automatic, at set times. Limits: **8 refreshes/day on Shared Capacity, 48/day on Premium/PPU.**

**Refresh Now (on-demand)** — manual, anytime, doesn't disturb the schedule.

**Refresh History** — shows last/next refresh time and success/failure, essential for troubleshooting a stale GreenCart dashboard.

## Incremental Refresh

Refreshes only *new or changed* data instead of reloading everything. GreenCart might store 5 years of order history but refresh only the last 10 days — much faster, far less memory.

**Requires** two parameters, `RangeStart` and `RangeEnd`, and **requires Query Folding** (Part 5) to actually work.

> ⚠️ **Interview trap:** without Query Folding, Incremental Refresh doesn't deliver its speed benefit — Power Query still has to pull more data than necessary before filtering it locally.

## Endorsing Trusted Models

- **Promotion** — flags a model as recommended/valuable, encourages reuse
- **Certification** — officially approved by admins as a trusted source
- **Master Data** (Fabric) — the designated single source of truth

## Query Caching

Available only on Fabric/Premium/Embedded — stores query results so reports load faster without hitting the model every time. Cache is user-specific and still respects security/filters, so it doesn't leak data across users.

## Lineage View & Impact Analysis

**Lineage View** shows the data flow across objects: Data Source → Gateway → Semantic Model → Report → Dashboard — useful for understanding what feeds what.

**Impact Analysis** shows what would break *before* you make a change — which reports depend on a semantic model, or which reports/models use a particular data source. Always worth checking before deleting or modifying a GreenCart data source.

## Quick Revision

**Key Takeaways**
- Gateway only needed for on-premises data, not cloud sources
- Incremental Refresh needs both RangeStart/RangeEnd parameters AND Query Folding
- Certification = officially trusted; Promotion = recommended, not officially vetted
- Impact Analysis should be checked before touching a shared semantic model or data source

**Interview Tip:** If asked to compare Promotion and Certification, the distinguishing detail is *who* approves it — Promotion is self-flagged by any user as valuable; Certification requires an authorized admin.
