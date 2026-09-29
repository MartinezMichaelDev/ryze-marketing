---
name: keyword-plan-builder
description: Expands seed keywords into a grouped keyword plan with monthly search volume, competition and cost per click from Google Ads keyword planning data. Use when asked to "build a keyword plan", "find keywords for a campaign", "how much search volume does this keyword have", or "what should I bid on".
---

# Keyword Plan Builder

Turn a few seed keywords or a landing page into a keyword plan that is ready to become ad groups.

## Before you start

Ask for anything that is missing:

- The product or service, or a landing page address.
- The target country and language.
- A monthly budget, if the user has one in mind.

This skill reads planning data only. It does not create campaigns or keywords.

## Workflow

1. **Pick the account.** Call `google_ads__listAccessibleCustomers`. Keyword planning needs an account to run under. Ask which one if there are several. If the account sits under a manager account, call `google_ads__getAccountHierarchy` and pass the manager's ID as the login customer ID on every later call.
2. **Generate ideas.** Call `google_ads__generateKeywordIdeas` with the seeds, or the page address, plus the country and language. The tool takes Google's location and language codes, not plain names. For example, the United States is `geoTargetConstants/2840` and English is `languageConstants/1000`. Set the network to Google Search. Ask for a few hundred ideas.
3. **Get the history.** Call `google_ads__generateKeywordHistoricalMetrics` for the shortlist, with the same location, language and network codes, so each keyword has average monthly searches, competition level and the low and high top of page bid.
4. **Filter.** Remove keywords that are off topic, that name other brands, or that have no measurable volume. Keep a separate short list of low volume keywords that match the product exactly.
5. **Group by intent.** Build ad group sized clusters of 5 to 15 keywords that share one meaning. Label each cluster with its intent: ready to buy, comparing options, or learning.
6. **Suggest match types and negatives.** Recommend exact or phrase match for ready to buy clusters. List obvious negatives found in the ideas, such as "free", "jobs" or "definition", when they do not fit the offer.
7. **Size the budget.** For each cluster, multiply expected clicks by the midpoint bid to show a rough monthly cost. State clearly that this is an estimate built from Google's bid ranges.

## Output

One table per cluster with these columns: keyword, average monthly searches, competition, low bid, high bid, suggested match type. Above the tables, give a summary with the number of clusters, total monthly searches and the rough monthly cost. Below the tables, list the negative keywords and the low volume exact matches.

## If something is missing

- If the idea tool returns nothing, try broader seeds and tell the user what you changed.
- If volume data is not available for the chosen country, say so. Do not substitute numbers from another country.
