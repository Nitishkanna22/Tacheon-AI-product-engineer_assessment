# Tacheon-AI-product-engineer_assessment
Task 1:
Marketing Performance Intelligence Tool "VeloxBoard"
Submitted by: Nitishkanna M
Version: V1
Date: May 26, 2026

What is VeloxBoard ? 

Veloxboard is an internal briefing surface. With main purpose being to auto-generate summary for each client brand that answers the standard question in a consistent format.

## The Problem Being Solved

Today, when someone asks "how is our marketing performing across channels right now, and where 
should we focus?" — the answer requires one person to manually log into multiple tools, pull 
numbers, and stitch together a response. It takes too long, looks different every time depending 
on who does it, and if that person is busy, the question just goes unanswered.

VeloxBoard fixes this by making the answer automatic, consistent, and available to anyone on 
the team in under 60 seconds — without changing how the team currently works.

Primary User: The Internal Analyst / Account Manager

The primary users are not the clients but the technical staffs. Incorporating a client based model in V1 would lead to second layer design in the model that distracts from the core problem. 

Requirements of team members include:
1. Provide quick answers / insights when the client asks
2. Identify major risk even before it becomes a major problem

## How VeloxBoard works

Data Sources
The tool reads from whatever platforms the team already uses. Typical marketing tech stack connections:

1. Google Ads API — conversions, impressions
2. Meta Ads API (Facebook/Instagram) — spend, reach, CTR, CPM
3. Google Analytics 4 — sessions, source/medium, goal completions
4. Email platform  — opens, clicks, sends

![image_alt](https://github.com/Nitishkanna22/Tacheon-AI-product-engineer_assessment/blob/205efb97cc2083b056a3f6089a74c4d1fa9df515/VeloxBoard%20Flow%20Diagram.png)  

**Data Flow Explained**

1. VeloxBoard automatically pulls data from the four platforms the team already uses — Google Ads, Meta Ads, GA4, and the email platform.
2. API Health Check
Before any data is written, the pipeline checks whether each API responded successfully.
* If YES: Data moves to transformation
* If NO: source is marked stale and flagged in the UI
3. Raw API responses are flattened, normalised, and enriched with derived fields — 7-day totals, period-over-period deltas, and channel-level KPIs.
4. Rule-based logic scans the transformed data and generates signals — flagging anomalies
5. Storage
Two tables are written:
* Brand Day Snapshot:  one aggregated row per brand per day
* Signal Log: one signal record per brand per run
6. The UI assembles the channel snapshot and focus signals into a single branded view.

## How should a good working model be like ?

When an analyst open VeloxBoard, clicks into client brand and would see,
* How each marketing channel performed over the last 7 days compared to the prior 7 days
* Which channel needs attention and why
* When the data was last refreshed and which sources contributed

## How reliant is VeloxBoard
   
VeloxBoard is only useful if the team trusts what it shows. Three things make that possible:

**Timestamps on everything.** Every view shows exactly when the data was last refreshed — 
"Last refreshed: 08:47 today" — so users always know how current the numbers are.

**Source attribution.** Every snapshot lists which platforms contributed to it. Users can see 
at a glance whether Google Ads, Meta, GA4, and Email all synced successfully.

**Honest failure states.** If an API call fails, the source is marked as stale and shown 
clearly in the UI. VeloxBoard never hides a data gap behind a zero. If something is missing, 
you will know.

## Architecture Decisions

**Why nightly and not real-time?**
"Right now" in a marketing context means the last 7 days, not the last 5 minutes. Nightly fetches 
are sufficient, simpler to operate, and significantly cheaper. Real-time pipelines add complexity 
with no meaningful gain for this use case.

**Why aggregated and not raw event-level data?**
VeloxBoard answers one fixed question. Raw event data would enable more queries but massively 
increases storage costs and maintenance overhead. One aggregated row per brand per day is all this 
tool needs. Deeper analysis can always be done in the source platform directly.

**Why rule-based signals and not AI?**
Rules are auditable and immediately trustworthy. A signal that says "CTR has fallen 3 weeks in a 
row" is self-explaining. An AI narrative that says "consider reallocating budget" without showing 
its reasoning will be questioned every time. Rules ship faster too — AI-generated signals are a 
natural v2 upgrade once the baseline is validated.

**Why internal-first and not client-facing?**
Building for internal analysts and building for clients are two different products. Internal users 
tolerate rough edges; clients need white-labelling, access controls, and a polished trust model. 
Solving for internal use first lets the team validate signal quality before exposing anything externally.

  
