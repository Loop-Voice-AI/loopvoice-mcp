# LoopVoice MCP server

LoopVoice runs AI voice calls and texts for online stores: abandoned-checkout recovery, post-purchase
follow-ups and support lines. This hosted MCP server lets Claude, ChatGPT, Gemini, Cursor or any MCP
client answer questions about your store's LoopVoice results.

| | |
|---|---|
| Server URL | `https://mcp.loopvoice.ai/mcp` |
| Transport | Streamable HTTP |
| Sign-in | OAuth 2.1 with PKCE. Your assistant registers itself, so there is no API key to copy |
| Registry | [`ai.loopvoice/mcp`](https://registry.modelcontextprotocol.io/v0/servers?search=ai.loopvoice) |
| Docs | [loopvoice.ai/mcp](https://loopvoice.ai/mcp) |

There is nothing to install: LoopVoice hosts the server. This repository only holds its listing
metadata: `server.json` (MCP Registry), `glama.json`, `plugin.json` + `mcp.json` (Agent Plugins
format) and `.mcp.json` (Cursor).

## What you can ask

- How did LoopVoice do last week? What drove the recovered revenue?
- Which calls went to voicemail or failed yesterday, and why?
- What are the top objections shoppers raise before deciding not to buy?
- Is my abandoned-cart campaign healthy? Is anything blocking it?
- What is our plan, how many credits are left, and is anything stopping calls?

## Connect

You need a LoopVoice account. Every client ends on the same LoopVoice sign-in: pick the workspace
(store) and click **Allow**. A connection sees only the workspace you picked.

| Client | How |
|---|---|
| Claude (web and desktop) | Settings → Connectors → **Add custom connector**, name `LoopVoice`, URL `https://mcp.loopvoice.ai/mcp`, then **Connect** |
| ChatGPT | [chatgpt.com/plugins](https://chatgpt.com/plugins) → **+** → **Add custom MCP server**, paste the URL, choose **OAuth** |
| Claude Code | `claude mcp add --transport http loopvoice https://mcp.loopvoice.ai/mcp`, then `/mcp` → **Authenticate** |
| Gemini CLI | `gemini mcp add --transport http loopvoice https://mcp.loopvoice.ai/mcp`, then `/mcp auth loopvoice` |
| Cursor | Settings → Tools & Integrations → MCP → add `https://mcp.loopvoice.ai/mcp` |
| Anything else | Add a remote MCP server with the URL above; it discovers OAuth on its own |

Inside LoopVoice, **Settings → Connected AI apps** has one-click buttons for Claude and Cursor and lists
every connection, with when it was last used and a button to disconnect it.

## Tools

Every read tool is annotated `readOnlyHint: true`, `destructiveHint: false`, `openWorldHint: false`.

| Tool | What it returns |
|---|---|
| `get_performance` | Recovered revenue and orders, calls, pickup rate and conversion for a date range, against the previous period |
| `search_calls` | Calls by date, outcome, campaign, automation or agent, or by what was said |
| `get_call` | One call: agent, outcome, attribution, summary and the redacted transcript |
| `get_customer_insights` | Support topics, objections and pickup patterns |
| `get_customer_timeline` | Calls, texts and orders for one shopper |
| `get_campaigns` | Campaigns with their status, results and anything blocking them |
| `get_automations` | Automations (flows) with their status and results |
| `get_agents` | Voice agents and how they are set up |
| `get_recommendations` | The latest Loops review: what to change, and why |
| `get_account_status` | Plan, credits, store connection and anything that would stop calls |
| `search_knowledge_base` | The store's knowledge base, as the agents use it |
| `search` / `fetch` | Search across calls, campaigns and automations, then open one result |
| `list_capabilities` / `read_capability` | Find and run any other read in the LoopVoice app |

Workspace admins can also allow changes (pausing a campaign or an automation, adding a number to Do Not
Call, editing settings). Those tools appear only for admins after they allow it, are annotated as
writes, and show a preview and wait for a yes before a risky change. **No tool places a call.**
Launching or resuming a campaign, test calls and enrolling shoppers are done inside LoopVoice.

Starter prompts: `weekly_recovery_report`, `why_did_calls_fail_yesterday`, `top_objections`,
`campaign_health_check`, `account_status`.

## Privacy

- One workspace per connection, checked on every call. A member removed from the workspace loses access at once.
- Shopper names, numbers and emails are masked unless an admin grants contact details.
- Call recordings never leave LoopVoice. Answers link back with "Open in LoopVoice".
- Transcripts and texts are returned inside untrusted-content markers, so the assistant treats them as
  what a shopper said, not as instructions.
- Admins can disconnect any app in Settings → Connected AI apps. That revokes its tokens.

## Support

support@loopvoice.ai · [loopvoice.ai](https://loopvoice.ai)
