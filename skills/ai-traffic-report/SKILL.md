---
name: ai-traffic-report
description: Reports how much website traffic comes from AI assistants such as ChatGPT, Perplexity, Gemini and Claude, which pages those visitors land on, and how the trend is moving, using Google Analytics 4 data. Use when asked "how much traffic do I get from ChatGPT", "is AI search sending me visitors", "which pages do AI assistants cite", or "track my AI visibility".
---

# AI Traffic Report

Show the user how much of their traffic comes from AI assistants, where it lands, and whether it is growing.

## Before you start

- This skill reads data only.
- It needs Google Analytics 4 connected in the Ryze workspace.
- Default to the last 90 days. Use another window if the user asks.

## Workflow

1. **Traffic by assistant.** Call `google_analytics__getAiTrafficByEngine`. Record sessions, engaged sessions and conversions for each assistant.
2. **Trend over time.** Call `google_analytics__getAiTrafficDaily`. Group the days into weeks and compare the most recent four weeks with the four weeks before.
3. **Landing pages.** Call `google_analytics__getAiLandingPages`. List the pages that receive the most AI traffic.
4. **Referral detail.** Call `google_analytics__getAIReferrals` to see the referring sources behind the totals.
5. **Read the pattern.** Work out:
   - AI traffic as a share of all sessions, if the total is available.
   - Which assistant sends the most visitors, and which sends the most engaged visitors.
   - What the top landing pages have in common, such as format, topic or page type.
   - Pages that rank well in the list but convert poorly.
6. **Suggest next steps.** Base each suggestion on the data. Examples: write more of the page type that already gets cited, add a clear next step to AI landing pages that do not convert, or refresh a cited page that is out of date.

## Output

Open with two sentences: how much AI traffic the site gets and whether it is rising or falling. Then three tables:

1. Assistants: assistant, sessions, engaged sessions, conversions, change versus prior period.
2. Landing pages: page, sessions from AI, conversions.
3. Weekly trend: week, sessions from AI.

Close with three suggested actions.

## If something is missing

- Small numbers are normal. If there are fewer than 50 AI sessions in the window, say the sample is too small to draw conclusions, and show the raw numbers.
- This report counts visits that arrive from AI assistants. It cannot count times an assistant mentioned the brand without the user clicking through. Say this when the user asks about mentions.
- If Google Analytics 4 is not connected, point the user to https://www.get-ryze.ai/how-to-connect-claude-to-google-meta-ads-mcp
