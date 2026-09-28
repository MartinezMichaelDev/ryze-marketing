# Ryze AI Marketing Skills for Claude

Six marketing skills for Claude, built by [Ryze AI](https://www.get-ryze.ai). They turn common paid media and analytics jobs into one request: check the health of a Google Ads account, clean up wasted search terms, build a keyword plan, check budget pacing, work through Google's recommendations, and report on traffic from AI assistants.

The skills run on your own live data through the Ryze MCP connector, so Claude reads real numbers from your accounts instead of guessing.

## What is included

| Skill | What it does | Data it reads |
|---|---|---|
| `google-ads-health-check` | Reviews account structure, conversion tracking, spend and results, then ranks the problems | Google Ads |
| `search-terms-cleanup` | Finds search terms that spend money without converting and drafts negative keyword lists | Google Ads |
| `keyword-plan-builder` | Expands seed keywords into a grouped plan with volume, competition and cost per click | Google Ads keyword planning |
| `budget-pacing-check` | Compares month to date spend against budget and flags campaigns that will overspend or underspend | Google Ads |
| `recommendations-triage` | Sorts Google's recommendations into apply, dismiss and discuss, with a reason for each | Google Ads |
| `ai-traffic-report` | Shows which AI assistants send visitors to your site and which pages they land on | Google Analytics 4 |

## What you need

1. A Ryze AI account. A free trial is available at https://app.get-ryze.ai
2. At least one platform connected in your Ryze workspace, such as Google Ads or Google Analytics 4.

## How it connects

This plugin bundles one remote MCP server, the Ryze connector, at `https://connector.get-ryze.ai/mcp`. The first time a skill needs data, Claude asks you to sign in to Ryze with OAuth and choose a workspace. There are no API keys to copy.

## What this plugin does and does not do

- It contains skills written in Markdown and one connector reference. It has no hooks, no scripts, no commands that run on your machine and no telemetry.
- It sends requests only to the Ryze connector at `connector.get-ryze.ai`, which is operated by Ryze AI. The connector reads data from the ad and analytics accounts you connected in Ryze.
- Reading data is the default. Any skill that can change an account, such as adding negative keywords or applying a recommendation, first shows you the exact change and waits for your approval.
- Every action runs with the permissions of the accounts you connected. The plugin cannot reach accounts you have not connected.

## Example requests

- "Run a health check on my Google Ads account"
- "Which search terms wasted money in the last 30 days?"
- "Build a keyword plan for project management software in the United States"
- "Are my campaigns on pace for this month's budget?"
- "Go through Google's recommendations with me"
- "How much traffic do I get from ChatGPT and Perplexity?"

## Support and policies

- Setup guide: https://www.get-ryze.ai/how-to-connect-claude-to-google-meta-ads-mcp
- Help center: https://help.get-ryze.ai
- Privacy policy: https://www.get-ryze.ai/privacy

## License

MIT. See the LICENSE file.
