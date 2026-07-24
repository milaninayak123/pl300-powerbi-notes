---
layout: default
title: "Part 18 — Managing Workspaces in Power BI Service"
parent: Notes
nav_order: 18
---

# Part 18 — Managing Workspaces in Power BI Service

Everything so far has happened in Power BI Desktop. This part covers what happens after: publishing GreenCart's reports to the cloud so the wider team can actually use them.

## Power BI Service — the Web Version

**Power BI Service** (app.powerbi.com) is where GreenCart's team publishes, shares, collaborates on, and manages reports once they leave Desktop.

**The workflow:** Power BI Desktop → Publish → Power BI Service → Share & Collaborate

## Workspace — the Collaboration Container

A **Workspace** stores reports, dashboards, semantic models, and apps for a team. GreenCart's analytics team works out of a **Shared Workspace**; **My Workspace** is personal-only and never used for real team projects.

**Workspace types** (by license, not by function): **Pro Workspace** for small teams, **Premium Per User (PPU)** for AI features and larger models with up to 48 refreshes/day, and **Fabric Capacity** for enterprise-scale needs (dedicated compute, dataflows, warehouses).

## Workspace Roles

| Role | Permissions |
|---|---|
| **Viewer** | View only |
| **Contributor** | Create, edit, delete content |
| **Member** | Contributor + publish/share apps |
| **Admin** | Full control |

**Remember the order:** Viewer < Contributor < Member < Admin.

## Dashboard vs. Report — Revisited in the Service

A **Report** is built in Desktop, has multiple pages, and runs off one semantic model. A **Dashboard** is single-page, built only in the Service, and combines pinned visuals from *multiple* reports — GreenCart's CEO dashboard might pull KPI tiles from the Sales report, the Delivery report, and the Inventory report all at once.

| Dashboard | Report |
|---|---|
| Single page | Multiple pages |
| Pulls from multiple reports/models | One semantic model |
| KPI summary | Detailed analysis |
| Service only | Desktop & Service |

## Apps

An **App** packages multiple reports, dashboards, and semantic models into one distributable unit — instead of sharing five separate GreenCart reports with regional managers, the team ships one App with everything bundled and access managed centrally.

## Publishing

From Desktop: **Home → Publish → Select Workspace.** This uploads both the report and its semantic model. Publishing again after edits updates the existing report in place (republishing), rather than creating a duplicate.

## Quick Revision

**Key Takeaways**
- Workflow: Desktop builds → Publish → Service hosts, shares, collaborates
- Workspace roles are hierarchical: Viewer < Contributor < Member < Admin
- Dashboard = single-page, Service-only, pulls from multiple reports; Report = multi-page, one semantic model

**Interview Tip:** "Difference between Dashboard and Report" is nearly guaranteed — always lead with *where each is built* (Report: Desktop & Service; Dashboard: Service only), then the structural difference.
