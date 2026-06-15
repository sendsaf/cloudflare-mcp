---
name: "cloudflare-mcp"
displayName: "Cloudflare MCP"
description: "Official Cloudflare skills and MCP servers for building on Workers, Pages, D1, R2, KV, AI, Agents SDK, Durable Objects, and the wider Cloudflare Developer Platform. Mirrors the cloudflare/skills plugin from Cloudflare."
keywords: ["cloudflare", "workers", "mcp", "agents", "d1", "r2", "kv", "wrangler", "durable-objects", "ai-gateway"]
author: "Cloudflare"
---

# Cloudflare MCP

Official Cloudflare skills and MCP servers, sourced directly from https://github.com/cloudflare/skills.

## MCP Servers

This power connects 5 official Cloudflare remote MCP servers (OAuth on first connect):

| Server | URL | Purpose |
|--------|-----|---------|
| `cloudflare-api` | `https://mcp.cloudflare.com/mcp` | Full Cloudflare API — 2,500+ endpoints via `search()` + `execute()` |
| `cloudflare-docs` | `https://docs.mcp.cloudflare.com/mcp` | Up-to-date Cloudflare reference documentation |
| `cloudflare-bindings` | `https://bindings.mcp.cloudflare.com/mcp` | Build Workers apps with storage, AI, and compute primitives |
| `cloudflare-builds` | `https://builds.mcp.cloudflare.com/mcp` | Manage and inspect Workers builds |
| `cloudflare-observability` | `https://observability.mcp.cloudflare.com/mcp` | Debug logs and analytics |

For CI/CD or automation, pass a Cloudflare API token as a bearer header instead of OAuth:
```json
"headers": { "Authorization": "Bearer YOUR_CLOUDFLARE_API_TOKEN" }
```
Create tokens at https://dash.cloudflare.com/profile/api-tokens.

## Skills

Skills are contextual — loaded based on what you're building. Each one biases toward **retrieving current Cloudflare docs** over pre-trained knowledge.

### Platform skills

| Skill | Useful for |
|-------|------------|
| `cloudflare` | Comprehensive platform: Workers, Pages, storage (KV, D1, R2), AI (Workers AI, Vectorize, Agents SDK), networking (Tunnel, Spectrum), security (WAF, DDoS), IaC (Terraform, Pulumi) |
| `agents-sdk` | Building stateful AI agents with state, scheduling, RPC, MCP servers, email, and streaming chat |
| `durable-objects` | Stateful coordination (chat rooms, games, booking), RPC, SQLite, alarms, WebSockets |
| `sandbox-sdk` | Secure code execution for AI code execution, code interpreters, CI/CD systems, and interactive dev environments |
| `wrangler` | Deploying and managing Workers, KV, R2, D1, Vectorize, Queues, Workflows |
| `web-perf` | Auditing Core Web Vitals (FCP, LCP, TBT, CLS), render-blocking resources, network chains |
| `workers-best-practices` | Reviewing and authoring Worker code against production standards |
| `cloudflare-email-service` | Sending and receiving transactional emails with Cloudflare Email Routing + Workers |
| `turnstile-spin` | Integrating Cloudflare Turnstile CAPTCHA across Next.js, Astro, SvelteKit, Hugo, and vanilla HTML |

### Cloudflare One skills

| Skill | Useful for |
|-------|------------|
| `cloudflare-one` | Designing, configuring, troubleshooting Cloudflare One (Access, Gateway, WARP, Tunnel, Magic WAN, DLP, CASB) |
| `cloudflare-one-migrations` | Migration assessments and policy mapping from Zscaler, Palo Alto, legacy VPN/SWG to Cloudflare One |

## Commands

Commands are explicit workflows you invoke. In Kiro, run these by asking the agent directly — the command files live at `commands/` inside this power.

| Command | Description |
|---------|-------------|
| `build-agent` | Build an AI agent on Cloudflare using the Agents SDK |
| `build-mcp` | Build a remote MCP server on Cloudflare using McpAgent |

**To invoke build-agent:** ask the agent to "build an AI agent on Cloudflare" and describe what you want. The agent will read `skills/agents-sdk/SKILL.md` and the relevant references under `skills/agents-sdk/references/`, fetch latest API details from developers.cloudflare.com, scaffold the project, and wire the MCP servers.

**To invoke build-mcp:** ask the agent to "build an MCP server on Cloudflare". The agent reads `skills/agents-sdk/references/mcp.md` for McpAgent APIs, transport options, and OAuth setup.

## Available Steering Files

- **servers** — Full catalog of all 16 Cloudflare MCP servers (including the product-specific ones beyond the 5 bundled here)
- **usage-guidance** — Task → skill → MCP server mapping

## Skills Structure

Skills live under `skills/` in this power, verbatim from `cloudflare/skills`:

```
skills/
├── cloudflare/           ← comprehensive platform skill + ~50 product references
│   ├── SKILL.md
│   └── references/
│       ├── workers/, pages/, d1/, r2/, kv/, queues/, vectorize/
│       ├── durable-objects/, workflows/, containers/
│       ├── workers-ai/, ai-gateway/, ai-search/, agents-sdk/
│       ├── wrangler/, observability/, analytics-engine/
│       ├── waf/, ddos/, tunnel/, spectrum/ ...
├── agents-sdk/           ← stateful agents, chat, RPC, MCP, email, voice
│   ├── SKILL.md
│   └── references/
│       ├── streaming-chat.md, state-scheduling.md, callable.md
│       ├── mcp.md, workflows.md, durable-execution.md
│       ├── email.md, voice.md, browse-the-web.md ...
├── durable-objects/      ← DO patterns, RPC, SQLite, testing
├── sandbox-sdk/          ← secure code execution, examples
├── workers-best-practices/ ← production review rules
├── wrangler/             ← CLI usage
├── web-perf/             ← Core Web Vitals auditing
├── cloudflare-email-service/ ← email sending, routing, deliverability
├── cloudflare-one/       ← Zero Trust platform
├── cloudflare-one-migrations/
└── turnstile-spin/       ← Turnstile integration + templates
```

## Best Practices

- Always read the relevant `SKILL.md` before starting a task — skills instruct the agent to retrieve current docs rather than rely on baked-in knowledge.
- Use `cloudflare-docs` MCP server alongside the `cloudflare` skill to get current limits, pricing, and API signatures.
- Never commit API tokens to source. Use `.env.local` for local dev, Cloudflare secrets/bindings for deployed Workers.
- Scope API token permissions tightly to what the task needs.
- Review any write, deploy, billing, auth, or destructive MCP operation before confirming.

## Troubleshooting

**OAuth flow not completing:** disconnect and reconnect the server in the MCP client; ensure cookies/redirects to cloudflare.com aren't blocked.

**"Unauthorized" with API token:** verify the token has required permissions and the `Authorization: Bearer <token>` header format is correct.

**Server not responding:** confirm URLs use `https://` and end in `/mcp` (not `/sse` which is deprecated).

**Resources:**
- Skills repo: https://github.com/cloudflare/skills
- MCP servers repo: https://github.com/cloudflare/mcp-server-cloudflare
- Agents docs: https://developers.cloudflare.com/agents/
- API tokens: https://dash.cloudflare.com/profile/api-tokens
