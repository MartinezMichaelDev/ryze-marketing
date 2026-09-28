---
name: google-ads-health-check
description: Reviews a Google Ads account from its live data and ranks the problems found - account structure, conversion tracking, spend versus results, and open recommendations. Use when asked to "audit my Google Ads", "check my Google Ads account", "why is my CPA high", or "is my account set up correctly".
---

# Google Ads Health Check

Review the user's Google Ads account using their own data through the Ryze connector. Report only what the data shows. Do not estimate numbers that were not returned.

## Before you start

- This skill reads data only. It never changes the account.
- If the Ryze connector is not signed in, ask the user to sign in and pick a workspace, then continue.
- Read each tool's input description before calling it, and pass the fields it asks for.

## Workflow

1. **Find the account.** Call `google_ads__listAccessibleCustomers`. If a manager account is returned, call `google_ads__getAccountHierarchy` to list the client accounts. If more than one account is possible, ask the user which one to review.
2. **Get the overview.** Call `google_ads__getAccountSummary` for the last 30 days. Note spend, clicks, conversions, cost per conversion and conversion value.
3. **Check conversion tracking.** Call `google_ads__listConversionActions`. Flag these problems:
   - No conversion action is enabled.
   - More than one action counts the same event as a primary conversion, which double counts results.
   - A primary action has recorded nothing in the last 30 days.
4. **Look at campaigns.** Call `google_ads__runRawGaql` with:

   ```
   SELECT campaign.name, campaign.status, campaign.advertising_channel_type,
          campaign.bidding_strategy_type, metrics.cost_micros, metrics.clicks,
          metrics.impressions, metrics.conversions, metrics.conversions_value
   FROM campaign
   WHERE segments.date DURING LAST_30_DAYS AND campaign.status = 'ENABLED'
   ORDER BY metrics.cost_micros DESC
   ```

   Divide `cost_micros` by 1,000,000 to get the amount in the account currency.
5. **Find the weak spots.** From the campaign rows, list:
   - Campaigns that spent money and recorded zero conversions.
   - Campaigns whose cost per conversion is more than double the account average.
   - Campaigns using a conversion based bid strategy with fewer than 15 conversions in 30 days.
6. **Check open recommendations.** Call `google_ads__listRecommendations` and count them by type. Mention the three with the largest stated impact. Do not apply any of them in this skill.

## Output

Start with a one paragraph verdict: is the account healthy, and what is the single biggest problem. Then give four short sections, each with a table: Conversion tracking, Campaigns that waste money, Bid strategy fit, Open recommendations. End with a list of five actions ranked by money at stake.

## If something is missing

- If Google Ads is not connected in the Ryze workspace, say so and point the user to https://www.get-ryze.ai/how-to-connect-claude-to-google-meta-ads-mcp
- If a query returns no rows, report that plainly. Do not fill the gap with industry averages.
