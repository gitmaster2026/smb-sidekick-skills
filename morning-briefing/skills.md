# Morning Business Briefing — SMB Sidekick

## Purpose
Generate a structured daily business briefing for a small business owner 
using their SMB Sidekick call data, sentiment trends, and calendar.

## When to use this skill
Use this skill when the user asks for a morning briefing, daily summary, 
business update, or "what happened yesterday" type questions. Also 
trigger when user says "start my day" or "catch me up."

## Required connector
SMB Sidekick (https://mcp.smbsidekick.ai/mcp) must be connected.

## Workflow

Execute these steps in order:

### Step 1 — Gather data (run all simultaneously)
- Call `get_call_summaries` with days=1 (yesterday's calls)
- Call `get_sentiment_trend` with days=7 (weekly context)
- Call `get_appointments` with days_ahead=1 (today's schedule)
- Call `get_business_metrics` (current usage and plan status)

### Step 2 — Analyze
Identify:
- Total calls yesterday and how that compares to the 7-day average
- Any calls flagged as action_required — list these first
- Overall sentiment yesterday vs 7-day trend (improving/declining/stable)
- Most common topics callers asked about
- Any callers with negative sentiment who may need follow-up
- Upcoming appointments today

### Step 3 — Generate briefing

Format the output as:

**Good morning. Here's your SMB Sidekick briefing for [date].**

📞 **Yesterday's Calls**
- [N] calls total ([comparison to 7-day average])
- [List any action_required calls by area code and topic]
- Top topics: [list]

📊 **Sentiment**
- Yesterday: [score] ([positive/neutral/negative])
- 7-day trend: [improving/stable/declining]
- [Flag if any callers need follow-up based on negative sentiment]

📅 **Today's Schedule**
- [List appointments or "No appointments scheduled"]

⚡ **Recommended actions**
- [1-3 specific action items based on the data]
- [Offer to send follow-up SMS to action_required callers]
- [Offer to save briefing as a report to the dashboard]

### Step 4 — Offer next steps
After delivering the briefing, ask:
"Would you like me to send follow-up texts to any of these callers, 
or save this briefing to your dashboard?"

If yes to saving: call `save_business_report` with category="custom",
title="Morning Briefing [date]", content=[the briefing text].

## Tone
Professional but concise. Business owners are busy — lead with what 
needs attention, not with what went well. Flag problems first.

## Error handling
If get_call_summaries returns empty (no calls yesterday), say so 
clearly and offer the 7-day trend instead. Do not fabricate data.
