# ClippingAlpha MCP server

The hosted [Model Context Protocol](https://modelcontextprotocol.io) server of [ClippingAlpha](https://clippingalpha.com). It lets an AI assistant find and read live paid creator campaigns (clipping, UGC, logo and music) from Content Rewards, Promote.fun, Clipster, Clipstake, Vyro and Evangelist, ranked by the Alpha Score.

- Endpoint: `https://clippingalpha.com/mcp` (remote, Streamable HTTP)
- Without 18+ campaigns: `https://clippingalpha.com/mcp/general` (the same tools, with campaigns in regulated categories such as gambling, prediction markets and crypto trading left out)
- Auth: none for the campaign tools. OAuth with dynamic client registration for the account tools.
- Server card: [clippingalpha.com/.well-known/mcp/server-card.json](https://clippingalpha.com/.well-known/mcp/server-card.json)
- Setup page: [clippingalpha.com/clip-with-ai](https://clippingalpha.com/clip-with-ai)

This repository holds the documentation and the registry metadata. The server runs at clippingalpha.com, there is nothing to install or host.

## What it does

The Alpha Score weighs pay per 1K views after fees and payout rules, approval odds, budget left and payout reliability against the live campaigns of the same type. Numbers come from each clipping platform's public pages and carry the time they were read. Join links open the campaign on its clipping platform. What a post earns depends on its views and the brand approving it. The Alpha Score is never for sale. How it works: [clippingalpha.com/how-we-rank](https://clippingalpha.com/how-we-rank).

## Tools

Campaign tools, no sign-in needed:

| Tool | What it returns |
| --- | --- |
| `top_alpha_now` | The best live campaigns now by Alpha Score, for one social app or campaign type or all, with a market line. |
| `search_campaigns` | Live campaigns that match filters: type, social app, minimum pay per 1K views, niche, clipping platform, Alpha tier, open join, new in the last hours, words in the name. |
| `get_campaign` | One campaign: Alpha Score and tier, pay per 1K views on each clipping platform that lists it, budget left, run-out forecast, checklist and where to join. |
| `board_summary` | The clipping platforms side by side: live campaigns, median pay per 1K views, budget left, new this week, platform fee, payout window and payout track record. |
| `show_campaign` | One campaign as an interactive panel in clients that render MCP Apps, with what a clip pays at 10K, 50K and 100K views and the best moments to clip. |
| `get_brief` | What the brand wants, in short items: format, must include, what to avoid, footage, social apps, account rules, deadline, and the Brief Quality grade (A to F). |
| `plan_clips` | A creative plan for one campaign: how many clips the material supports, length, format, what to cut it from, and the clips best first. |
| `get_moments` | The analysed source videos of a campaign and the moments worth clipping, with links to the source at each second. |
| `get_assets` | Every source link found in the brief: public videos and channels with their details, and, signed in, the brand asset links. |
| `get_campaign_analytics` | The verdicts behind a campaign's Alpha Score, what one clip pays at 10K, 50K and 100K views after caps and fees, and the budget runway. |
| `compare_campaigns` | Two to five campaigns side by side, ranked in the order to clip them, with a one line recommendation. |

Account tools, for a creator signed in through ClippingAlpha's OAuth:

| Tool | What it does |
| --- | --- |
| `my_saved_campaigns` | Lists the campaigns and brands the creator saved, with whether each is live. |
| `save_campaign` | Saves or unsaves a campaign, which turns on alerts for its next rounds, rate rises, top-ups and run-out. |
| `my_alert_rules` | Lists the creator's alert rules and alert settings. |
| `set_alert_rule` | Adds or changes an alert rule: minimum pay, social apps, niches, clipping platforms, pay type, lowest Alpha tier, open join. |
| `delete_alert_rule` | Deletes one alert rule. |

Signed in, members campaigns, brand file links and the footage passwords brands publish for clippers unlock. A signed-in client that supports MCP events can subscribe to `campaign.new`, `campaign.budget_low` and `campaign.ended`.

Each campaign tool takes a campaign as `brand/series` (for example `lovable/clipping`), its clippingalpha.com page URL, or the `campaignId` of an earlier answer.

## Setup

### Claude (claude.ai and Claude Desktop)

Open Connectors (Customize, Connectors) and choose Add custom connector. Name `ClippingAlpha`, URL `https://clippingalpha.com/mcp`. Or open the prefilled link on [clippingalpha.com/clip-with-ai](https://clippingalpha.com/clip-with-ai). Anthropic's guide: [custom connectors using remote MCP](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp).

### Claude Code

```bash
claude mcp add --transport http clippingalpha https://clippingalpha.com/mcp
```

Then run `/mcp` in Claude Code to sign in for the account tools. Docs: [code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp).

### ChatGPT

With developer mode on, add an app in Settings, Apps with the MCP URL `https://clippingalpha.com/mcp` and OAuth. OpenAI's docs: [developers.openai.com/api/docs/mcp](https://developers.openai.com/api/docs/mcp).

### Cursor

Use the install button on [clippingalpha.com/clip-with-ai](https://clippingalpha.com/clip-with-ai), or add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "clippingalpha": {
      "url": "https://clippingalpha.com/mcp"
    }
  }
}
```

Docs: [cursor.com/docs/mcp](https://cursor.com/docs/mcp).

### VS Code

Use the install button on [clippingalpha.com/clip-with-ai](https://clippingalpha.com/clip-with-ai), or add this to `.vscode/mcp.json`:

```json
{
  "servers": {
    "clippingalpha": {
      "type": "http",
      "url": "https://clippingalpha.com/mcp"
    }
  }
}
```

Docs: [MCP servers in VS Code](https://code.visualstudio.com/docs/agent-customization/mcp-servers).

### Windsurf

Add this to the `mcp_config.json` file the Cascade docs name:

```json
{
  "mcpServers": {
    "clippingalpha": {
      "serverUrl": "https://clippingalpha.com/mcp"
    }
  }
}
```

Docs: [Cascade MCP integration](https://docs.windsurf.com/windsurf/cascade/mcp).

### Any other client

A client that speaks only stdio can reach the server through [mcp-remote](https://github.com/punkpeye/mcp-remote):

```json
{
  "mcpServers": {
    "clippingalpha": {
      "command": "npx",
      "args": ["mcp-remote", "https://clippingalpha.com/mcp"]
    }
  }
}
```

## Example prompts

- What should I clip today on TikTok?
- Show me the top UGC campaigns right now.
- Find clipping campaigns on YouTube paying at least $2 per 1K views.
- Which clipping campaigns went live in the last 24 hours?
- What does lovable/clipping want, and what should I cut first?
- Which moments in this campaign's footage are worth clipping?
- Compare the clipping platforms by median pay and payout window.
- Compare these three campaigns and tell me which to clip first.
- Alert me to YouTube campaigns over $2 per 1K views.
- Save this campaign and tell me when its budget runs low.

## Registry metadata

[`server.json`](server.json) is a copy of the server's registry entry (schema `2025-12-11`). The source of truth is the server itself, and this copy is updated whenever the server's version changes, so the two stay in sync.

## Privacy, terms and support

- Privacy policy: [clippingalpha.com/privacy](https://clippingalpha.com/privacy)
- Terms: [clippingalpha.com/terms](https://clippingalpha.com/terms)
- Help and support: [clippingalpha.com/support](https://clippingalpha.com/support), or email privacy@clippingalpha.com

ClippingAlpha is operated by Momentist, Inc. It is independent of the clipping platforms it lists, apart from Evangelist, which is run by the same team and labeled that way on the site.

## License

The contents of this repository (documentation and configuration) are [MIT](LICENSE) licensed. The ClippingAlpha service and its data are covered by the [terms](https://clippingalpha.com/terms).
