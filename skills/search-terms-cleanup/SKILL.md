---
name: search-terms-cleanup
description: Finds Google Ads search terms that spend money without converting, groups them by theme, and drafts negative keyword lists. Adds the negatives only after the user approves each list. Use when asked to "find wasted spend", "clean up search terms", "build a negative keyword list", or "what am I paying for that does not convert".
---

# Search Terms Cleanup

Find the search terms that cost money and bring nothing back, then turn them into negative keywords the user can approve.

## Safety rules

- Reading comes first. Do not change anything until the user has seen the list and said yes.
- Show the exact keywords, match types and the campaign or ad group each one will be added to.
- Ask for approval one list at a time. If the user says no to a list, leave it out.

## Workflow

1. **Pick the account.** Call `google_ads__listAccessibleCustomers`. Ask which account to use if there is more than one. If the account sits under a manager account, call `google_ads__getAccountHierarchy` and pass the manager's ID as the login customer ID on every later call.
2. **Pull the search terms.** Call `google_ads__runRawGaql` with:

   ```
   SELECT search_term_view.search_term, campaign.name, ad_group.name,
          metrics.cost_micros, metrics.clicks, metrics.impressions,
          metrics.conversions
   FROM search_term_view
   WHERE segments.date DURING LAST_30_DAYS AND metrics.cost_micros > 0
   ORDER BY metrics.cost_micros DESC
   LIMIT 1000
   ```

   If the account has low volume, use a 60 or 90 day window instead. There is no preset for those, so replace the date condition with exact dates, for example `segments.date BETWEEN '2026-07-01' AND '2026-09-28'`. Tell the user which window you used.
3. **Set the waste threshold.** Work out the account's average cost per conversion from step 2. Treat a term as wasteful when it has zero conversions and its spend is at least that average. For accounts with no conversions at all, stop and tell the user that tracking must be fixed first.
4. **Group by theme.** Sort the wasteful terms into themes, such as competitor names, job seekers, free or cheap, wrong location, wrong product, and questions with no buying intent.
5. **Protect good terms.** Remove any candidate that matches a keyword the account is bidding on, or that converted in a longer window. When unsure, keep the term and mark it "review".
6. **Draft the negatives.** For each theme, propose the shortest phrase that blocks the theme without blocking real buyers. Use phrase match by default. Use exact match when a phrase would be too broad.
7. **Get approval, then apply.** Present each list with its total wasted spend. For approved lists only, add the negatives with `google_ads__runRawMutate`. Report what was added and where.

## Output

A table for each theme with these columns: search term, campaign, spend, clicks, conversions, proposed negative, match type. Put the total spend for the theme above its table. Finish with the total monthly spend that the approved negatives would save.

## If something is missing

- If the search term query returns nothing, the account may have no Search campaigns. Say so.
- If the change is refused, show the error text and the list, so the user can add the keywords by hand.
