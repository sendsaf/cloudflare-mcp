# cloudflare-mcp

> A Kiro Power that connects the Kiro AI IDE to the full Cloudflare developer platform — live API access, up-to-date docs, built-in Cloudflare skills, and guided workflows for Workers, AI Gateway, D1, R2, KV, Agents SDK, Durable Objects, and more.

[![Cloudflare](https://img.shields.io/badge/Cloudflare-MCP-F38020?logo=cloudflare&logoColor=white)](https://developers.cloudflare.com)
[![Kiro Power](https://img.shields.io/badge/Kiro-Power-6C47FF?logo=amazonaws&logoColor=white)](https://kiro.dev)
[![MCP](https://img.shields.io/badge/Model_Context_Protocol-compatible-blue)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## What is this?

This is a **[Kiro Power](https://kiro.dev)** — a plugin for the Kiro AI IDE that bundles everything you need to work with Cloudflare from inside your editor:

- **5 live Cloudflare MCP servers** — direct API access to your Cloudflare account (Workers, Pages, AI Gateway, D1, R2, KV, DNS, WAF, and 2,500+ more endpoints)
- **Cloudflare skills** — contextual knowledge guides that load based on what you're building, biased toward fetching current Cloudflare docs rather than relying on stale AI training data
- **Guided workflows** — `build-agent` and `build-mcp` commands that scaffold and deploy full Cloudflare projects step by step
- **Steering files** — task-to-skill-to-server mappings so Kiro always picks the right tool for the job

### What makes it more than just MCP URLs

The MCP server URLs themselves are public — Cloudflare publishes them. What this Power adds on top is the **Kiro-native layer**: skills that give the agent deep, current product knowledge before it touches the API; commands that orchestrate multi-step build workflows; and steering that routes the right context to the right task automatically. That's the part that only works in Kiro.

---

## Install in Kiro

1. Open the **Kiro Powers panel** in the sidebar
2. Search for `cloudflare-mcp`
3. Click **Install**
4. On first use, the `cloudflare-api` server will prompt OAuth — authorize with your Cloudflare account in the browser tab that opens
5. Done — ask Kiro anything about your Cloudflare account

### Or add manually to `.kiro/settings/mcp.json`

```json
{
  "mcpServers": {
    "cloudflare-api": {
      "type": "http",
      "url": "https://mcp.cloudflare.com/mcp"
    }
  }
}
```

For API token auth instead of OAuth:
```json
{
  "mcpServers": {
    "cloudflare-api": {
      "type": "http",
      "url": "https://mcp.cloudflare.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_CLOUDFLARE_API_TOKEN"
      }
    }
  }
}
```
Create tokens at https://dash.cloudflare.com/profile/api-tokens.

---

## What you can do once connected

### Talk to your Cloudflare account
```
"List all my Workers"
"Show the last 100 requests to my AI Gateway"
"What are the spend limits on my luvloot-beta gateway?"
"Create a new KV namespace called sessions"
"Show DNS records for my domain"
"Deploy this Worker script to production"
```

### Build with guided workflows
```
"Build an AI agent on Cloudflare that handles customer support"
"Build a remote MCP server on Cloudflare with OAuth"
```
Kiro reads the relevant skill guides, fetches current Cloudflare docs, scaffolds the project, and wires the bindings — step by step.

### Get current Cloudflare knowledge
```
"What are the D1 row size limits?"
"How does Durable Object hibernation work?"
"What's the difference between Workers AI and AI Gateway?"
```
Skills bias toward retrieving live Cloudflare documentation rather than using potentially outdated training data.

---

## MCP Servers

| Server | URL | Purpose |
|--------|-----|---------|
| `cloudflare-api` | `https://mcp.cloudflare.com/mcp` | Full Cloudflare REST API — `search()` the OpenAPI spec, `execute()` against live endpoints, `docs()` for reference |
| `cloudflare-docs` | `https://docs.mcp.cloudflare.com/mcp` | Semantic search over current Cloudflare developer documentation |
| `cloudflare-bindings` | `https://bindings.mcp.cloudflare.com/mcp` | Scaffold Workers apps with KV, D1, R2, AI, Queues, Durable Objects bindings |
| `cloudflare-builds` | `https://builds.mcp.cloudflare.com/mcp` | Trigger, inspect, and manage Workers builds |
| `cloudflare-observability` | `https://observability.mcp.cloudflare.com/mcp` | Query logs, analytics, and metrics from your account |

> All servers are maintained by Cloudflare at [github.com/cloudflare/mcp-server-cloudflare](https://github.com/cloudflare/mcp-server-cloudflare).

---

## Included Skills

Skills are loaded contextually — Kiro pulls in the right one based on your task.

| Skill | What it covers |
|-------|---------------|
| `cloudflare` | Comprehensive platform: Workers, Pages, D1, R2, KV, AI Gateway, Workers AI, Vectorize, Queues, Tunnels, WAF, DNS, and more |
| `agents-sdk` | Stateful AI agents — state management, scheduling, RPC, streaming chat, MCP, email, voice |
| `durable-objects` | Coordination patterns, SQLite storage, alarms, WebSockets, testing |
| `wrangler` | CLI usage for local dev and deployment |
| `workers-best-practices` | Production code review — streaming, floating promises, global state, secrets, observability |
| `sandbox-sdk` | Secure code execution, AI code interpreters, sandboxed environments |
| `web-perf` | Core Web Vitals auditing — LCP, CLS, TBT, render-blocking resources |
| `cloudflare-email-service` | Email sending and routing via Cloudflare Email Workers |
| `cloudflare-one` | Zero Trust — Access, Gateway, WARP, Tunnel, DLP, CASB |
| `cloudflare-one-migrations` | Migration from Zscaler, Palo Alto, legacy VPN/SWG to Cloudflare One |
| `turnstile-spin` | Turnstile CAPTCHA integration for Next.js, Astro, SvelteKit, Hugo, vanilla HTML |

---

## Commands

Invoke these by describing the task to Kiro:

**`build-agent`** — Full scaffold-and-deploy workflow for a stateful AI agent on Cloudflare Workers using the Agents SDK. Reads the agents-sdk skill, fetches current API docs, scaffolds the project, wires MCP servers.

**`build-mcp`** — Full scaffold-and-deploy workflow for a remote MCP server on Cloudflare using McpAgent. Covers tools, OAuth setup, and deployment.

---

## Repo Structure

```
cloudflare-mcp/
├── mcp.json                  # MCP server config (all 5 servers)
├── POWER.md                  # Kiro Power manifest
├── README.md
├── logo.svg
├── commands/
│   ├── build-agent.md        # Guided workflow: build an AI agent
│   └── build-mcp.md          # Guided workflow: build an MCP server
├── steering/
│   ├── servers.md            # Full catalog of all 16 Cloudflare MCP servers
│   └── usage-guidance.md     # Task → skill → MCP server mapping
└── skills/
    ├── cloudflare/           # Comprehensive platform skill + ~50 product references
    ├── agents-sdk/
    ├── durable-objects/
    ├── sandbox-sdk/
    ├── workers-best-practices/
    ├── wrangler/
    ├── web-perf/
    ├── cloudflare-email-service/
    ├── cloudflare-one/
    ├── cloudflare-one-migrations/
    └── turnstile-spin/
```

---

## Using the MCP servers without Kiro

The MCP server URLs work in any MCP-compatible client (Claude Desktop, Cursor, Windsurf, Zed) — but you get only the raw API access, not the skills, commands, or steering that make this Power useful in Kiro.

```json
{
  "mcpServers": {
    "cloudflare-api": {
      "type": "http",
      "url": "https://mcp.cloudflare.com/mcp"
    }
  }
}
```

For clients that only support stdio (older versions of some editors), use mcp-remote as a proxy:
```bash
npx -y mcp-remote https://mcp.cloudflare.com/mcp
```

---

## Troubleshooting

**OAuth not completing** — disconnect and reconnect the server in the Kiro MCP panel. Make sure your browser isn't blocking redirects to cloudflare.com.

**"Unauthorized" with API token** — verify the token is valid and has the right permissions. Check the `Authorization: Bearer <token>` header format.

**Server URL errors** — URLs must use `https://` and end in `/mcp`. The old `/sse` suffix is deprecated.

---

## Related

| Resource | URL |
|----------|-----|
| Kiro IDE | https://kiro.dev |
| Cloudflare MCP servers (official) | https://github.com/cloudflare/mcp-server-cloudflare |
| Cloudflare Skills plugin | https://github.com/cloudflare/skills |
| Cloudflare Agents SDK | https://developers.cloudflare.com/agents/ |
| Workers docs | https://developers.cloudflare.com/workers/ |
| AI Gateway docs | https://developers.cloudflare.com/ai-gateway/ |
| API token management | https://dash.cloudflare.com/profile/api-tokens |
| Model Context Protocol | https://modelcontextprotocol.io |

---

## License

MIT — see [LICENSE](LICENSE).

Skills content under `skills/` is sourced from [github.com/cloudflare/skills](https://github.com/cloudflare/skills) and subject to its license.

---

*Not officially affiliated with Cloudflare or Kiro (AWS). Packages official Cloudflare MCP servers as a Kiro Power for community use.*
