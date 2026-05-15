# Weekly Business Review — SMB Sidekick

## Purpose
Generate a comprehensive weekly business review and save it to the 
SMB Sidekick dashboard for the business owner's records.

## When to use this skill
Trigger when user asks for a weekly review, weekly summary, week in 
review, or weekly report. Also trigger on "how did we do this week."

## Required connector
SMB Sidekick (https://mcp.smbsidekick.ai/mcp) must be connected.

## Workflow

### Step 1 — Gather data
- Call `get_call_summaries` with days=7
- Call `get_sentiment_trend` with days=7
- Call `get_business_metrics`
- Call `get_documents` (to reference knowledge base status)

### Step 2 — Analyze
Calculate:
- Total calls this week, average per day, busiest day
- Sentiment arc across the week (did it improve or decline?)
- Top 5 topics callers asked about
- Calls that needed follow-up — were they resolved?
- Minutes and SMS used vs plan limits (flag if over 80%)
- Knowledge base coverage gaps (topics callers asked that 
  may not be covered in uploaded documents)

### Step 3 — Generate review

Format as a structured report:

**Weekly Business Review — [date range]**

**Executive Summary**
[2-3 sentence summary of the week]

**Call Volume**
- Total: [N] calls over 7 days
- Daily average: [N]
- Busiest day: [day] with [N] calls
- vs prior context: [trend if available]

**Customer Sentiment**
- Weekly average: [score]
- Trend: [improving/stable/declining]
- Notable: [any days with significant dips or spikes]

**Top Customer Topics**
1. [Topic] — [N] mentions
2. [Topic] — [N] mentions
3. [Topic] — [N] mentions

**Action Items**
- [Specific recommendations based on the data]
- [Knowledge base gaps to address]
- [Follow-up calls or SMS to send]

**Usage**
- Minutes used: [N] of [plan limit] ([%])
- SMS sent: [N] of [plan limit] ([%])
- [Flag if approaching limits]

### Step 4 — Save to dashboard
Always offer to save using `save_business_report`:
  title: "Weekly Review — [date range]"
  category: "weekly_review"
  content: [the full report text]

Confirm once saved: "Your weekly review has been saved to your 
SMB Sidekick dashboard at smbsidekick.ai/reports"

## Tone
Executive summary style. Data-first. Actionable recommendations 
at the end, not buried in the middle.
