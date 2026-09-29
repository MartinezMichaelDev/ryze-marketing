---
name: budget-pacing-check
description: Compares month to date Google Ads spend against budget and flags campaigns on course to overspend or underspend. Use when asked "am I on pace", "how is my budget pacing", "will I hit my budget this month", or "which campaigns are limited by budget".
---

# Budget Pacing Check

Show whether spend is on track for the month and which campaigns need a budget change.

## Before you start

- Ask for the monthly budget target if the user has one. If not, use the sum of daily budgets multiplied by the days in the month.
- This skill reads data only. Recommend budget changes, and let the user decide.

## Workflow

1. **Pick the account.** Call `google_ads__listAccessibleCustomers`. Ask which account to use if there is more than one. If the account sits under a manager account, call `google_ads__getAccountHierarchy` and pass the manager's ID as the login customer ID on every later call.
2. **Pull spend and budgets.** Call `google_ads__runRawGaql` with:

   ```
   SELECT campaign.name, campaign.status, campaign_budget.amount_micros,
          campaign_budget.period, campaign_budget.has_recommended_budget,
          metrics.cost_micros, metrics.conversions
   FROM campaign
   WHERE segments.date DURING THIS_MONTH AND campaign.status = 'ENABLED'
   ORDER BY metrics.cost_micros DESC
   ```

   Divide micros by 1,000,000 to get the amount in the account currency.
3. **Work out the pace.** For the account and for each campaign:
   - Days elapsed and days left in the month. Today's data is partial, so count completed days only.
   - Average daily spend so far.
   - Projected month end spend, which is spend so far plus average daily spend times days left.
   - Pace, which is projected spend divided by the monthly target.
4. **Flag the outliers.**
   - Over pace: projected spend above 110 percent of target.
   - Under pace: projected spend below 85 percent of target.
   - Limited by budget: `has_recommended_budget` is true, or daily spend hits the daily budget on most days.
5. **Weigh it against results.** Compare each campaign's cost per conversion with the account average. Moving budget only makes sense toward campaigns that convert at or below the average.
6. **Recommend changes.** Suggest a new daily budget for each flagged campaign that brings the month back on target. Show the arithmetic.

## Output

Open with one line on the account: spend so far, projected spend, target, and pace as a percentage. Then one table with these columns: campaign, daily budget, spend so far, projected spend, pace, cost per conversion, suggested daily budget. Finish with up to three budget moves, each with the amount and the reason.

## If something is missing

- Shared budgets cover several campaigns. When a budget is shared, report pace for the budget as a whole and name the campaigns that use it.
- In the first three days of a month, say that the projection is unreliable and show last month's final numbers next to it.
