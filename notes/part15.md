---
layout: default
title: "Part 15 — Report Design Requirements"
parent: Notes
nav_order: 15
---

# Part 15 — Report Design Requirements

Before building anything, a good report starts with a simple question: who's actually going to look at this, and what do they need from it? This part covers that groundwork; Part 16 gets into actually building the report.

## The Three Audiences

| Audience | What they need | Easy trick |
|---|---|---|
| **Executive** | High-level business decisions | Executive → **Monitor** |
| **Analyst** | Deep exploration, finding insights | Analyst → **Analyze** |
| **Information Worker** | Daily operational work | Info Worker → **Act** |

For GreenCart, that's the CEO checking company-wide KPIs, an analyst digging into why West-region orders dropped, and a warehouse manager checking today's delivery queue.

## The Four Report Types

- **Dashboard** — single-page, minimal interaction, answers "how are we doing?" Built for Executives. GreenCart's CEO dashboard: total orders, revenue, active riders, at a glance.
- **Analytical Report** — filters, drill-down, drillthrough, answers "why did this happen?" Built for Analysts. The West-region investigation from Part 1 lives here.
- **Operational Report** — real-time, action-oriented, simple interface, for fast daily decisions. GreenCart's live inventory or delivery-queue report, used by warehouse staff.
- **Educational Report** — storytelling, simple visuals, for people unfamiliar with the data — think a public sustainability report GreenCart publishes for customers.

> ⚠️ **Interview trap:** Dashboard and Analytical Report get confused constantly. The test: a Dashboard has minimal interaction and answers *"how are we doing"*; an Analytical Report is built for digging in and answers *"why did this happen."* Where it's built matters too — Dashboards live in the Service; Analytical Reports are built in Desktop.

## UI vs. UX — Not the Same Thing

**UI (User Interface)** = how the report *looks and behaves*: form factor (desktop = more visuals, landscape; mobile = fewer visuals, portrait, bigger touch targets), input method (mouse vs. touch), theming/branding, and **accessibility** — large fonts, strong color contrast, alt text on visuals, keyboard navigation.

**UX (User Experience)** = how *easy* the report is to actually use. This covers interactivity (drill-down, drillthrough, slicers), flexible access (export to Excel, natural-language Q&A, alerts), scenario testing (What-If parameters — move a slider, see profit recalculate), and collaboration (subscriptions, comments, scheduled email delivery).

> ⚠️ **Interview trap:** "What's the difference between UI and UX?" — UI is the *look*, UX is the *ease of use*. A report can look polished (good UI) but still be confusing to navigate (poor UX).

## Quick Revision

**Key Takeaways**
- Know the audience first — Executive/Analyst/Information Worker maps directly to Dashboard/Analytical/Operational report types
- UI = look and behavior; UX = ease of use
- Accessibility (contrast, alt text, keyboard nav) isn't optional polish — it's a design requirement

**Interview Tip:** If asked to design a report from scratch, always start your answer with "first, I'd identify the audience" — it signals a structured approach before jumping to chart choices.
