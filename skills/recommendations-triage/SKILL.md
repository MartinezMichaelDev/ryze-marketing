---
name: recommendations-triage
description: Sorts Google Ads recommendations into apply, dismiss and discuss with a plain reason for each, then applies or dismisses only the ones the user approves. Use when asked to "go through my Google recommendations", "should I apply this recommendation", "raise my optimization score", or "clean up recommendations".
---

# Recommendations Triage

Google's recommendations mix useful fixes with suggestions that mainly raise spend. Sort them for the user and act only on what they approve.

## Safety rules

- Nothing is applied or dismissed without a clear yes from the user for that item.
- Never apply a recommendation that raises budgets or loosens targeting as part of a batch. Those always get their own question.
- After each change, report exactly what was changed.

## Workflow

1. **Pick the account.** Call `google_ads__listAccessibleCustomers`. Ask which account to use if there is more than one.
2. **List the recommendations.** Call `google_ads__listRecommendations`. Record each one's type, the campaign it applies to, and the impact Google states.
3. **Get context.** Call `google_ads__getAccountSummary` for the last 30 days, so each recommendation can be judged against real spend and cost per conversion.
4. **Sort into three groups.**
   - **Apply:** fixes with little downside, such as adding missing assets, fixing disapproved ads, fixing conversion tracking, or removing conflicting negative keywords.
   - **Discuss:** changes that can help but cost money or control, such as raising budgets, switching bid strategy, adding broad match keywords, or turning on automatic assets.
   - **Dismiss:** suggestions that do not fit the account's goal, such as expanding to networks or audiences the user has chosen to avoid.
5. **Explain each one.** Give one or two sentences on what would change and what it could cost. Avoid jargon.
6. **Ask, then act.** Go through the Apply group first. For each approved item, call `google_ads__applyRecommendation`. For items the user rejects for good, call `google_ads__dismissRecommendation`. Leave undecided items alone.

## Output

Three tables, one per group, with these columns: recommendation, campaign, what changes, stated impact, my advice. After the user has decided, add a short log with each item that was applied or dismissed.

## If something is missing

- If there are no recommendations, say so. That is a normal result for a well kept account.
- If applying fails, show the error and leave the recommendation in place.
- Optimization score is a Google measure of how many recommendations were followed. Tell the user it is not a measure of profit.
