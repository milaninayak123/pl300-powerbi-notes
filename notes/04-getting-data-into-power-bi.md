---
layout: default
title: "Part 4 — Getting Data into Power BI"
parent: Notes
nav_order: 4
---

# Part 4 — Getting Data into Power BI

*Connecting to SQL Server, NoSQL, and online services, plus dynamic reports with parameters and choosing a storage mode.*

With the big picture out of the way, let's get practical: how does GreenCart's data actually get *into* Power BI?

## Where Data Comes From

GreenCart's data doesn't live in one place. Orders sit in SQL Server, delivery-partner details might be in an Excel sheet HR maintains, and warehouse stock data could come from Cosmos DB. Power BI's real strength is connecting to all of these and combining them into one report.

## Importing Data

The path is: **Home → Get Data → Select Data Source → Connect.** Once connected, you choose:

- **Load** — bring the data in as-is (use when it's already clean)
- **Transform Data** — open Power Query Editor to clean it first (remove duplicates, fix types, filter rows, etc.)

**Power Query** is Power BI's ETL (Extract, Transform, Load) tool — it connects, cleans, transforms, and combines data before it reaches the semantic model.

## Connecting to SQL Server (GreenCart's Orders Database)

Two options: import the whole table, or write your own query to pull only what's needed:

```sql
SELECT OrderID, City, OrderAmount
FROM Orders;
```

Pulling only the required columns — instead of the entire table — is better for performance.

**View** — a virtual table built from a SQL query but usable like a regular table. Connecting to a view (instead of a raw table) enables **Query Folding** — see below.

**Query Folding** — when Power Query pushes transformations back to the source instead of processing them in Power BI. If GreenCart's Orders table has 10 million rows but you only need this year's data, Power Query sends the database `SELECT * FROM Orders WHERE Year = 2026` instead of pulling everything and filtering locally. Less data moved, faster refresh.

## Dynamic Reports — One Report, Many Views

Instead of building a separate sales report for every one of GreenCart's ten regional managers, you build **one** report and let a **Parameter** control what each manager sees.

**Single-value parameter:** write a query like `SELECT * FROM Orders WHERE Region = @Region`, create a `Region` parameter in Power Query (default value: "North"), and connect it to the query. Change the parameter to "West" and the whole report updates — same report, different data.

**Multiple-value parameter:** if GreenCart's leadership wants to compare several regions side by side, you build a small table listing the regions, turn the query into a reusable function, and invoke it once per region — Power Query runs it for each and combines the results.

## Data from NoSQL and Online Services

If GreenCart's warehouse tracking data lives in Cosmos DB as JSON documents rather than rows and tables, connecting works differently: Get Data → Azure Cosmos DB → select the collection → Transform Data → expand the nested JSON into rows/columns → Close & Apply. Once flattened, it behaves like any other table.

Power BI can also pull directly from services like SharePoint — say GreenCart tracks monthly sales targets in a SharePoint list, which can be combined with the Orders data in the same report.

## Storage Modes

This decision shapes performance and freshness for every table in your model:

| Mode | How it works | Best for |
|---|---|---|
| **Import** (default) | Full copy cached in the model | Small/medium data, fastest performance |
| **DirectQuery** | No copy — always queries the source live | Very large or fast-changing data |
| **Dual (Composite)** | Some tables imported, others live | Mixing large fact tables with small lookup tables |

GreenCart's Orders table (millions of rows, changing constantly) might use DirectQuery, while its small Products table uses Import.

## Azure Analysis Services

**Azure Analysis Services (AAS)** hosts pre-built enterprise semantic models — relationships, measures, and calculations already defined. Connecting with **Connect Live** reuses that model directly instead of duplicating data or measures, so it always reflects the latest AAS refresh.

## Quick Revision

**Key Takeaways**
- Power Query is the ETL layer: connect, clean, transform, combine
- Query Folding pushes work back to the source for speed — check with "View Native Query"
- Parameters turn many near-identical reports into one dynamic report
- Storage mode (Import/DirectQuery/Dual) is chosen per table, not per report

**Interview Tip:** "Why use a parameterized report instead of ten separate reports?" — the expected answer is maintenance: one report to update instead of ten, with no risk of them drifting out of sync.

**Common Mistake to Avoid:** Assuming DirectQuery is always the "better" choice because it's always live. It's slower and more limited (fewer supported transformations) — Import is usually the right default unless the data is too big or must be real-time.
