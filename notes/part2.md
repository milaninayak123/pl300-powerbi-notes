---
layout: default
title: "Part 2 — Getting to Know Power BI (The Tool Itself)"
parent: Notes
nav_order: 2
---

# Part 2 — Getting to Know Power BI (The Tool Itself)

Part 1 was about data analysis as a discipline. Now let's talk about the actual tool GreenCart's analysts use to do it: **Power BI**.

Power BI is a business intelligence tool — it connects to data, lets you clean and shape it, model it, build visuals, and share the result as interactive reports and dashboards. It scales from a single analyst working off one Excel sheet, all the way up to GreenCart's entire company pulling from a live orders database.

## The Three Pieces of Power BI

**Power BI Desktop** — where the building happens. GreenCart's analyst opens Desktop, connects to the Orders database, cleans it, models it, builds charts, and then publishes the finished report to the cloud.

**Power BI Service** — the cloud platform where the published report lives. This is where GreenCart's regional managers actually go to view the West-region dashboard, where refreshes get scheduled, and where access is controlled.

**Power BI Mobile** — for viewing (and Desktop for optimizing) reports on a phone. A GreenCart warehouse manager checking today's delivery numbers from their phone is using Mobile.

The simplest way to remember the split: **Desktop is where you build, Service is where you share, Mobile is where you view.**

## The Typical Workflow

**Connect to data → Clean & transform (Power Query) → Build the semantic model → Build reports & visuals → Publish to Service → Create dashboards → Share & manage**

Every concept in this guide slots into one of these steps.

## Report vs. Dashboard — the Distinction People Get Wrong

GreenCart's analyst builds a multi-page **Sales Report**: one page for regional trends, one for product categories, one for delivery times. It's detailed and interactive — click into any chart, filter by date, drill down by city.

The regional manager, though, doesn't want to dig through five pages every morning. So the analyst pins the key numbers — total orders, average delivery time, revenue — onto a single-page **Dashboard**. One glance tells the manager if something's wrong; clicking a tile takes them into the full report for detail.

| Report | Dashboard |
|---|---|
| Multiple pages | Single page |
| Highly interactive | High-level summary |
| Built in Power BI Desktop | Built in Power BI Service |
| Used for detailed analysis | Used for quick daily monitoring |
| Contains many visuals | Contains tiles pinned from reports |

> ⚠️ **Common mix-up:** people use "report" and "dashboard" interchangeably in casual conversation, but Power BI treats them as genuinely different objects, built in different places. If an interviewer asks you to define both, lead with *where each is built* — that's the fastest way to keep them straight.

The analogy that sticks: **a dashboard is like your car's dashboard** — a glance tells you the essentials. If something looks off, you "open the hood" (the full report) for detail.

## Workspaces and Apps

A **workspace** is a shared area in the Service where GreenCart's analytics team stores and manages reports, dashboards, and semantic models together. "My Workspace" is for personal testing only — real projects live in a shared team workspace.

A **Power BI App** bundles multiple reports/dashboards into one package for a specific audience. Instead of sharing ten separate reports with regional managers, GreenCart's team ships one app containing everything they need, with access controlled centrally.

## Keeping Data Fresh

Since GreenCart's order data changes constantly, reports need **refresh** — either triggered on-demand or scheduled automatically, so the dashboard a manager checks at 9am isn't showing yesterday's numbers.

## Quick Revision

**Key Takeaways**
- Desktop builds, Service shares, Mobile views
- A report is detailed and multi-page; a dashboard is a single-page summary built from pinned report tiles
- Workspaces organize team content; Apps package it for distribution

**Interview Tip:** Expect a direct "what's the difference between a report and a dashboard" question. Answer with the build-location distinction first, then the structural one (multi-page vs. single-page).

**Common Mistake to Avoid:** Assuming a dashboard can be built in Desktop — it can't. Dashboards only exist in the Service, built by pinning visuals from an already-published report.
