---
layout: default
title: "Part 22 — Secure Data Access in Power BI"
parent: Notes
nav_order: 22
---

# Part 22 — Secure Data Access in Power BI

*Row-Level Security, Object-Level Security, and the limitation almost everyone misses.*

One GreenCart sales report, many regional managers — but a West-region manager shouldn't see East-region numbers. That's what this part covers.

## Row-Level Security (RLS)

**RLS** restricts which *rows* a user sees, letting one report safely serve everyone.

**Static RLS** — a fixed DAX filter, best for a small number of roles that rarely change: `[Region] = "West"`.

**Dynamic RLS** — filters based on the logged-in user's identity, best for many users/roles: `[Email] = USERPRINCIPALNAME()`. `USERPRINCIPALNAME()` is the preferred function; `USERNAME()` and `CUSTOMDATA()` (Embedded scenarios only) also exist.

| Static | Dynamic |
|---|---|
| Fixed values | Based on logged-in user |
| More manual maintenance | Automatic |
| Small teams | Large organizations |

**Setup:** Modeling → Manage Roles → create role + DAX filter → **View As** (test it) → Publish → assign users/groups in Power BI Service.

> ⚠️ **Interview trap — the limitation people miss:** RLS only applies to **Workspace Viewers**. Admins, Members, and Contributors bypass it entirely and see all data regardless of role assignment. Also: if no role is assigned, users see *everything*, not nothing.

Best practice: assign **Security Groups**, not individual users, to roles — easier to maintain as GreenCart's team grows.

## Single Sign-On (SSO)

Used with **DirectQuery**: Power BI passes the user's identity straight to the source database, and the database enforces security itself instead of Power BI doing it. One catch: **View As doesn't work to test this** when DirectQuery + SSO are combined.

## Object-Level Security (OLS)

Where RLS hides rows, **OLS** hides entire tables or columns — e.g., hiding a Salary column from most GreenCart staff viewing an HR report.

- Secures tables/columns, not rows
- Measures can't be secured directly, but a measure referencing a secured column becomes restricted automatically
- Same limitation as RLS: applies only to Workspace Viewers
- Can't combine RLS and OLS from different roles
- Not supported with Quick Insights, Smart Narrative, or Excel Data Types Gallery

## Quick Revision

**Key Takeaways**
- RLS = row-level; OLS = table/column-level; both apply only to Workspace Viewers
- Dynamic RLS with `USERPRINCIPALNAME()` scales to large teams; Static RLS is fine for a handful of fixed roles
- No role assigned = full access, not restricted access

**Interview Tip:** "Does RLS apply to a workspace Admin?" is a near-guaranteed trick question — the answer is no, RLS/OLS only ever restrict Viewers.
