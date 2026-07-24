---
layout: default
title: "Part 1 — Understanding Power BI & Data Analysis"
parent: Notes
nav_order: 1
---

# Part 1 — Understanding Power BI & Data Analysis

*What data analysis actually is, the four types of analytics, and the five things a Data Analyst does — all through GreenCart's West-region sales drop.*

## Meet GreenCart

Throughout this guide, we'll follow one company: **GreenCart**, a fictional online grocery delivery business operating in a handful of Indian cities. GreenCart sells fruits, vegetables, dairy, snacks, and household essentials through an app, delivered by a fleet of riders from local warehouses.

Every order GreenCart processes — what was bought, by whom, when, delivered how fast — becomes data. And every week, GreenCart's leadership asks questions like: *Which products are selling best? Why did West region orders drop last month? Will the upcoming monsoon slow down deliveries? Should we open a new warehouse?*

Answering those questions, with data instead of guesswork, is what data analysis actually is.

## What Is Data Analysis?

At its core, data analysis is the process of turning raw, messy numbers into something a person can act on. Raw data — a spreadsheet of ten thousand orders — is just noise until someone shapes it into a story: *this* product is underperforming, *this* region needs attention, *this* trend is worth watching.

**Semantic model** — before we go further, one term worth knowing early: a semantic model is the organized, cleaned-up version of your data that a tool like Power BI actually builds reports from. Think of it as the difference between a pile of grocery receipts and a properly organized ledger — same information, but one is usable.

## The Four Types of Analytics — Told Through One GreenCart Problem

Say GreenCart notices something: total orders in the West region dropped 20% last month. Here's how the four types of analytics tackle that, in order.

**1. Descriptive Analytics — "What happened?"**
Just the facts: West region orders fell from 50,000 to 40,000. No explanation yet, just a clear summary — the kind of thing you'd see on a dashboard.

**2. Diagnostic Analytics — "Why did it happen?"**
Digging in: a competitor launched aggressive discounts in the same cities, and heavy rains disrupted rider availability for two weeks. Now the drop makes sense.

**3. Predictive Analytics — "What's likely to happen next?"**
Using past patterns (monsoon seasons, competitor behavior), GreenCart forecasts a further 10% dip next month if the rains continue — a forecast, not a certainty.

**4. Prescriptive Analytics — "What should we do about it?"**
Based on all of the above: increase rider incentives during rain, launch a matching discount campaign, and pre-stock the West warehouse before the season worsens.

| Type | Question | GreenCart Example |
|---|---|---|
| Descriptive | What happened? | West orders fell 20% |
| Diagnostic | Why did it happen? | Competitor discounts + rain disrupted delivery |
| Predictive | What's likely next? | Possible further 10% drop if rain continues |
| Prescriptive | What should we do? | Rider incentives + discount campaign + pre-stocking |

> ⚠️ **Common mix-up:** people often blur descriptive and diagnostic together. The test is simple — descriptive never explains *why*, it only reports *what*. If a sentence includes a reason, it's already diagnostic.
>
> Another trap: predictive analytics gives a *probability*, not a promise. An interviewer may ask "does predictive analytics guarantee the outcome?" — the answer is no, it extrapolates patterns forward, and those patterns can break.

## Who Actually Does This Work?

Data analysis at a company like GreenCart isn't one person's job — it's split across roles:

- **Business Analyst** — focuses on the business problem itself (why are West orders down?), interprets dashboards for leadership
- **Data Analyst** — turns raw order/delivery data into insights using SQL, Power BI, Excel
- **Data Engineer** — builds and maintains the pipelines that get order data from the app into a usable database
- **Analytics Engineer** — sits in between, preparing clean, ready-to-use datasets for the Data Analyst
- **Data Scientist** — builds the actual forecasting models behind the "10% further drop" prediction

## The Five Things a Data Analyst Actually Does

Whoever ends up analyzing GreenCart's West-region problem moves through five stages:

1. **Prepare** — clean the raw order data: fix duplicate orders, handle missing delivery timestamps, standardize city names
2. **Model** — connect the Orders table to the Customers and Delivery tables so they can be analyzed together
3. **Visualize** — build a dashboard showing the regional order trend over time
4. **Analyze** — actually interpret it: spot that the drop coincides exactly with the rain dates and the competitor's launch
5. **Manage** — publish the dashboard so regional managers can check it themselves, with proper access controls

## Quick Revision

**Key Takeaways**
- Data analysis = turning raw data into decisions, not just numbers into charts
- Four analytics types build on each other: what → why → what next → what to do
- A Data Analyst's real job spans five stages: Prepare, Model, Visualize, Analyze, Manage

**Interview Tip:** If asked to explain the four analytics types, always anchor each one to a question ("what/why/next/action") rather than a definition — it's easier to remember and easier to explain out loud.

**Common Mistake to Avoid:** Treating "data analysis" as synonymous with "making dashboards." Dashboards are the *visualize* stage — only one of five.
