# OwlStat MCP: Instagram & TikTok analytics for AI assistants

OwlStat's remote MCP server connects Claude, ChatGPT, Cursor, Codex and any other MCP client to the Instagram and TikTok accounts in your [OwlStat](https://owlstat.io) workspace. It is built for agencies and growth teams that manage many accounts: ask how a client is doing, compare accounts with competitors, find top posts, Reels and TikTok videos, get alerts and build a report, in plain English or from your own scripts and agents.

- **Endpoint:** `https://mcp.owlstat.io/mcp` (Streamable HTTP)
- **Auth:** OAuth 2.1 with dynamic client registration, or your OwlStat API key as a Bearer token
- **Tools:** 31 (Instagram, TikTok, short-form video, alerts, Create and Schedule)
- **Docs:** [owlstat.io/instagram-mcp](https://owlstat.io/instagram-mcp/)

This repository holds documentation and the registry manifest only. The server is hosted by OwlStat: there is nothing to install.

## Data

Accounts you connect are read through Meta's official API with the permissions you grant, so reach, views, saves and shares are available and no password is shared. Any other public Instagram or TikTok account you track (a competitor, a prospect) comes from the daily snapshot of public numbers OwlStat already keeps. The assistant never logs in to Instagram or TikTok.

## Get an API key

1. Create an account at [owlstat.io](https://owlstat.io) and track at least one account. The free plan covers two accounts, the last 30 days of data and 50 MCP calls a month.
2. Open your OwlStat settings and copy the key from the API section.

## Connect

### Claude (desktop and web)

Settings → Connectors → Add custom connector. Paste `https://mcp.owlstat.io/mcp` and sign in, or choose API key authentication and paste your key.

### Claude Code

```bash
claude mcp add owlstat \
  --transport http \
  --header "Authorization: Bearer YOUR_API_KEY" \
  --scope user \
  https://mcp.owlstat.io/mcp
```

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "owlstat": {
      "url": "https://mcp.owlstat.io/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

### OpenAI Codex CLI

`~/.codex/config.toml`:

```toml
[mcp_servers.owlstat]
url = "https://mcp.owlstat.io/mcp?apiKey=YOUR_API_KEY"
```

### ChatGPT

Add a custom connector pointing at `https://mcp.owlstat.io/mcp`. MCP support in ChatGPT depends on your plan.

### Any other client

Point it at `https://mcp.owlstat.io/mcp` over Streamable HTTP with `Authorization: Bearer YOUR_API_KEY`, or let it run the OAuth flow.

## Tools

### Instagram

| Tool | What it does | |
|---|---|---|
| `get_account_stats` | Get Instagram account statistics including followers, engagement rate, reach, and growth metrics for a specific account over a given time period. | read-only |
| `get_top_posts` | Get the top performing Instagram posts for an account, ranked by engagement rate, likes, comments, or reach. | read-only |
| `compare_accounts` | Compare 2-5 Instagram accounts side by side on key metrics like engagement rate, follower growth, reach, and posting frequency. | read-only |
| `analyze_hashtags` | Analyze hashtag performance for an Instagram account, showing which hashtags drive the most engagement. | read-only |
| `get_audience_insights` | Get audience and growth insights for an Instagram account including follower growth trajectory, engagement health, and projected growth. | read-only |
| `generate_report` | Generate a comprehensive Instagram analytics report for an account with executive summary, growth analysis, engagement metrics, top posts, and recommendations. | read-only |
| `list_monitored_accounts` | List all Instagram accounts currently being monitored in your OwlStat workspace. | read-only |
| `get_best_posting_times` | Find the best days and times to post on Instagram based on historical engagement data. Returns a heatmap showing when posts get the most engagement. | read-only |
| `get_content_breakdown` | Analyze content performance by media type (Image, Video, Reel, Carousel) showing which format drives the most engagement. | read-only |
| `get_reels_analytics` | Analyze Instagram Reels performance with viral velocity, engagement rates, and view metrics. | read-only |
| `get_post_insights` | Track how a specific post's reach, views, saves, and shares change over time. | read-only |
| `get_growth_summary` | Get a computed growth summary with follower projections, engagement trends, and posting frequency. | read-only |
| `manage_workspace` | Add, remove, or list Instagram accounts in your tracked workspace. | changes data, can remove |

### TikTok

| Tool | What it does | |
|---|---|---|
| `get_tiktok_account_stats` | Get TikTok account statistics including followers, total likes, posts count, and growth metrics for a specific account over a given time period. | read-only |
| `get_tiktok_top_videos` | Get the top performing TikTok videos for an account, ranked by engagement rate, plays, diggs (likes), comments, or shares. | read-only |
| `compare_tiktok_accounts` | Compare 2-5 TikTok accounts side by side on key metrics like engagement rate, follower growth, total plays, and posting frequency. | read-only |
| `get_tiktok_growth_summary` | Get a computed TikTok growth summary with follower projections, engagement trends, total likes change, and posting frequency. | read-only |
| `get_tiktok_video_analytics` | Analyze TikTok video performance with viral velocity, engagement rates, and play metrics. | read-only |
| `get_tiktok_best_posting_times` | Find the best days and times to post on TikTok based on historical play and engagement data. Returns a heatmap showing when videos get the most plays. | read-only |
| `get_tiktok_content_breakdown` | Analyze TikTok content performance broken down by format (video vs slideshow), sponsorship, and duration buckets, showing which type drives the most engagement. | read-only |
| `generate_tiktok_report` | Generate a comprehensive TikTok analytics report for an account with executive summary, growth analysis, engagement metrics, top videos, and recommendations. | read-only |
| `list_tiktok_monitored_accounts` | List all TikTok accounts currently being monitored in your OwlStat workspace. | read-only |
| `manage_tiktok_workspace` | Add, remove, or list TikTok accounts in your tracked workspace. | changes data, can remove |

### Short-form video (Reels, Shorts, TikTok)

| Tool | What it does | |
|---|---|---|
| `get_viral_studio_overview` | Get a cross-platform Viral Studio overview for short-form videos across Instagram Reels, YouTube Shorts, and TikTok. | read-only |
| `get_viral_studio_recommendations` | Get Viral Studio recommendations for short-form videos across Instagram Reels, YouTube Shorts, and TikTok. | read-only |

### Alerts

| Tool | What it does | |
|---|---|---|
| `get_alerts` | Which tracked accounts need a look right now, and why: followers falling, engagement falling (judged on posts old enough to have settled, so one viral post or a post published an hour ago does not trigger it), or no new post… | read-only |
| `manage_alerts` | Change the user's alerts: tune or switch off a rule (followers drop, engagement drop, no new posts), set how often the alert email comes (every day, every Monday, or never), or hide an account from alerts and show it again. | changes data |

### Create and Schedule (beta, per account)

| Tool | What it does | |
|---|---|---|
| `create_carousel` | Make an Instagram carousel in OwlStat Create, in the brand's own style (its colors, fonts and logo, and the look of its posts), from an article link or from facts written in the conversation. Every fact on the slides is… | changes data |
| `get_carousel` | A carousel made in OwlStat Create: whether it is still being made, and once it is done its slides (as images), headline and caption, the text of every slide (to change words with edit_carousel), plus the link to edit it in… | read-only |
| `edit_carousel` | Change words on a finished carousel from OwlStat Create: for each edit, the slide number, the old text exactly as get_carousel shows it (a whole line, or part of one) and the new text. A whole line keeps its style, and *word*… | changes data, can remove |
| `manage_schedule` | The user's OwlStat post schedule for Instagram. "list": what is planned in a period, for every account or one, with each post's id, time and state. "add": put a finished carousel (from get_carousel) on an account, at a time or… | changes data, can remove |

## Example questions

- "How did @client do this month, and does anything need a look?"
- "Compare @brand with @competitor1 and @competitor2 on engagement and reach over 90 days."
- "Which Reels from the last 30 days had the most views, and what do they have in common?"
- "When should @brand post on TikTok?"
- "Build a monthly report for @client."

## Limits

The free plan keeps 30 days of history and 50 calls a month; paid plans remove the call limit and open the full history. Create and Schedule are betas opened per account: their tools are listed only to accounts that have them. Scheduled posts never go out by themselves, they wait for approval on OwlStat's Schedule page.

## Support and privacy

- Support: [support@owlstat.io](mailto:support@owlstat.io)
- Privacy policy: [owlstat.io/privacy-policy](https://owlstat.io/privacy-policy/)
- Terms: [owlstat.io/terms](https://owlstat.io/terms/)
