# Tacheon-AI-product-engineer_assessment
Task 1:
Marketing Performance Intelligence Tool "VeloxBoard"
Submitted by: Nitishkanna M
Version: V1
Date: May 26, 2026

What is VeloxBoard ? 

Veloxboard is an internal briefing surface. With main purpose being to auto-generate summary for each client brand that answers the standard question in a consistent format.

Primary User: The Internal Analyst / Account Manager

The primary users are not the clients but the technical staffs. Incorporating a client based model in V1 would lead to second layer design in the model that distracts from the core problem. 

Requirements of team members include:
1. Provide quick answers / insights when the client asks
2. Identify major risk even before it becomes a major problem

What It Needs to Work 

Data Sources
The tool reads from whatever platforms the team already uses. Typical marketing tech stack connections:

1. Google Ads API — spend, ROAS, conversions, impressions
2. Meta Ads API (Facebook/Instagram) — spend, reach, CTR, CPM
3. Google Analytics 4 — sessions, source/medium, goal completions
4. Email platform (e.g. Klaviyo, Mailchimp, HubSpot) — opens, clicks, sends

![image_alt](https://github.com/Nitishkanna22/Tacheon-AI-product-engineer_assessment/blob/205efb97cc2083b056a3f6089a74c4d1fa9df515/VeloxBoard%20Flow%20Diagram.png)  

How Veloxboard works:

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
   
  


  
