---
layout: default
title: "Part 8 — Data Modeling Foundations: Star Schema & Model Frameworks"
parent: Notes
nav_order: 8
---

# Part 8 — Data Modeling Foundations: Star Schema & Model Frameworks

GreenCart's data is now connected and clean. The next question is how to structure it so Power BI can actually reason about it — this is **data modeling**, and it's arguably the most important skill in the whole PL-300 syllabus.

## Semantic Model, Dataset — Same Thing, Different Stage

You build a **model** in Power BI Desktop. Once published to the Service, that same thing is called a **dataset**. "Semantic model" is just the more formal term for it. All three describe the same underlying object — knowing that keeps you from getting tripped up if an interviewer uses them interchangeably.

## How a Visual Actually Gets Its Data

Every time a GreenCart report visual displays something, it sends an **analytic query** to the model, and that query always runs in the same three-step order:

**Filter → Group → Summarize**

Say a report has a slicer for `Year = 2026`, a chart broken out by `Region`, and Total Orders as the value:

1. **Filter** — keep only 2026 orders, discard the rest. (The filter value itself never appears *in* the result — it just narrows things down.)
2. **Group** — split the filtered rows by region: North, South, East, West.
3. **Summarize** — sum total orders per region — these become the bar heights.

The same logic works outside Power BI entirely: filtering GreenCart's staff to "Delivery Riders," grouping them by city, and summarizing their average delivery time follows the identical three steps.

> ⚠️ **Interview trap:** this exact order — Filter, then Group, then Summarize, never any other sequence — is a very common question. Memorize the order, not just the names.

## Star Schema — Structuring the Model

Power BI models are **tabular models**: tables plus relationships, hierarchies, and calculations. The recommended structure is **star schema**, where every table is one of two things:

- **Dimension table** — describes entities: Products, Customers, Dates, Delivery Riders
- **Fact table** — records events: GreenCart's `Orders` table, with a row per transaction and numeric values (amount, quantity) plus keys linking back to the dimensions

Visually, the fact table sits in the center, dimension tables radiate outward like a star's points. In an analytic query: dimension columns filter/group, fact columns get summarized — which is exactly the Filter → Group → Summarize sequence above, just named differently.

| | Dimension Table | Fact Table |
|---|---|---|
| Stores | Descriptive attributes | Measurable events |
| GreenCart example | Products, Customers, Dates | Orders |
| Used for | Filtering, grouping | Summarizing (Sum, Count, etc.) |

> ⚠️ **Interview trap — one of the most-asked modeling questions:** be ready to define both table types, give a concrete example of each, and explain *why* star schema is preferred: simpler relationships, faster queries, and it's the industry-standard approach used well beyond just Power BI.

## Storage Modes, Revisited

Every table carries a storage mode, and it's what actually determines the model framework:

| Mode | How it works |
|---|---|
| **Import** | Data cached inside the model |
| **DirectQuery** | No cache — every query passes through live |
| **Dual** | Can do either; Power BI picks whichever's faster |

## The Three Model Frameworks

**Import Model** — the default, and what GreenCart's analysts build most often. Supports every data source and all DAX/Power Query features, and gives the fastest performance since everything's cached in memory. The tradeoff: models have a size limit, and data is only as fresh as the last scheduled refresh.

**DirectQuery Model** — every table queries the source live. Right for GreenCart's Orders table if it's too large to import outright, or if regional managers need truly real-time numbers. The tradeoff: fewer supported sources, some transformations (pivot/unpivot) can't translate into a native query, and performance depends on how well GreenCart's own database is optimized.

**Composite Model** — mixes storage modes: DirectQuery for the massive Orders fact table, Import (or Dual) for small dimension tables like Products or Regions. This is the standard approach for large enterprise models that need both scale and speed. The tradeoff: imported portions still need refreshing and can drift slightly out of sync with the live DirectQuery portions.

> ⚠️ **Interview trap:** "Import vs. DirectQuery" in one line — Import is fast and cached but size-limited and needs refreshing; DirectQuery is live with no Power BI-side size limit, but slower and dependent on the source. Have one concrete GreenCart-style scenario ready for each.

## Quick Revision

**Key Takeaways**
- Model in Desktop → Dataset once published → same thing, different names
- Every visual runs Filter → Group → Summarize, always in that order
- Star schema = one fact table (summarized) surrounded by dimension tables (filter/group)
- Import = fast + cached; DirectQuery = live + size-unlimited but slower; Composite = both, mixed per table

**Interview Tip:** If asked to design a model for a new scenario, default to describing a star schema first — naming the fact table and its dimensions — before discussing storage mode. Structure comes before performance tuning.

**Common Mistake to Avoid:** Describing "filter context" and the Filter→Group→Summarize sequence as unrelated ideas — they're the same mechanism, just discussed at different levels of detail. We'll dig further into filter context specifically when we get to DAX.
