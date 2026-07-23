---
layout: default
title: "Part 20 — Choosing a Content Distribution Method"
parent: Notes
nav_order: 20
---

# Part 20 — Choosing a Content Distribution Method

The GreenCart model and reports are built and trustworthy. The last question: how do different people across the company actually get access to them?

## Three Ways to Share Content

**1. Workspace Roles** — access to everything in the workspace, all-or-nothing. Fine for GreenCart's own analytics team, wrong for a regional manager who should only see their region's report.

> ⚠️ **Interview trap:** Workspace roles cannot share just one report — it's the entire workspace or nothing. This is the reason Item-Level Sharing exists.

Note: **Row-Level Security (RLS) only applies to Viewers** — someone with Contributor/Member/Admin access can see unfiltered data regardless of RLS rules.

**2. Item-Level Sharing** — shares one specific report or dashboard without exposing the rest of the workspace. Recipients can interact but not edit.

| Workspace Sharing | Item-Level Sharing |
|---|---|
| All content | Selected report/dashboard only |
| Built for collaboration | Built for targeted access |
| Less control | More control |

**3. Power BI Apps** — packages multiple reports/dashboards/models into one distributable unit, better for reaching a large audience with controlled, centralized access. One Workspace App exists per workspace, moving through Create → Add Content → Publish → Update → Republish.

## Audiences — One App, Different Views

**Audiences** let different user groups see different content inside the *same* App. GreenCart's Sales team sees sales reports, Finance sees financial reports, HR sees HR reports — one app, three different experiences, without maintaining three separate apps.

## Org Apps — the Newer Fabric Alternative

| Workspace App | Org App |
|---|---|
| One per workspace | Multiple per workspace |
| Requires publish/republish | Saves = live immediately |
| Users install the app | No installation needed |
| Good for staged releases | Good for fast-moving updates |

## Governance: Making Sure Content Is Trusted and Safe

**Promotion / Certification** — same concept from Part 19, applied to reports and dashboards too, not just semantic models.

**Sensitivity Labels** — classify and protect content (Public, Confidential, Highly Confidential). These stick with the file even after export — a Confidential GreenCart financial report stays protected even if someone exports it to PDF or PowerPoint.

## Keeping Reports Reaching People

**Report Subscriptions** — auto-email a report on a schedule (hourly to monthly), as a link, PDF, or PowerPoint (Premium). Useful for a regional manager who wants Monday morning's numbers without opening Power BI at all.

**Usage Metrics** — tracks report views, unique users, load performance, and engagement, so GreenCart's analytics team knows which reports are actually being used (and which ones to retire).

## Quick Revision

**Key Takeaways**
- Workspace Roles = all-or-nothing; Item-Level Sharing = one report; Apps = packaged, large-audience distribution
- RLS only enforces for Viewers — higher roles bypass it
- Audiences let one App serve multiple teams differently; Org Apps update instantly with no install step
- Sensitivity Labels persist even after export

**Interview Tip:** "How would you share a single confidential report with just the finance team?" is a good scenario question — the expected chain of reasoning is Item-Level Sharing (not workspace-wide) + a Sensitivity Label (protection survives export).
