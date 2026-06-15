# cloudflare-mcp

> Connect any MCP-compatible AI assistant (Claude, Kiro, Cursor, Windsurf, Zed, etc.) to the full Cloudflare developer platform — live API access, up-to-date docs, and guided workflows for Workers, AI, storage, and more.

[![Cloudflare](https://img.shields.io/badge/Cloudflare-MCP-F38020?logo=cloudflare&logoColor=white)](https://developers.cloudflare.com)
[![MCP](https://img.shields.io/badge/Model_Context_Protocol-compatible-blue)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## What is this?

This repo packages the **official Cloudflare remote MCP servers** into a ready-to-use configuration for any MCP-compatible AI client. It gives your AI assistant:

- **Live Cloudflare API access** — read and write across your account (Workers, Pages, D1, R2, KV, AI Gateway, DNS, WAF, and 2,500+ more endpoints)
- **Up-to-date Cloudflare documentation** — fetched at query time, not baked-in stale knowledge
- **Guided workflows** — built-in skills for building AI agents, MCP servers, Durable Objects, Workers, and more

No local server to run. No npm install. These are **remote HTTP MCP servers** hosted by Cloudflare — you just point your client at the URLs.

---

## MCP Servers

| Server ID | URL | What it does |
|-----------|-----|-------------|
| `cloudflare-api` | `https://mcp.cloudflare.com/mcp` | Full Cloudflare REST API — `search()` the OpenAPI spec, `execute()` against live endpoints, `docs()` for reference. 2,500+ endpoints. |
| `cloudflare-docs` | `https://docs.mcp.cloudflare.com/mcp` | Semantic search over Cloudflare's developer documentation. Always current. |
| `cloudflare-bindings` | `https://bindings.mcp.cloudflare.com/mcp` | Scaffold and wire Workers apps with KV, D1, R2, AI, Queues, Durable Objects bindings |
| `cloudflare-builds` | `https://builds.mcp.cloudflare.com/mcp` | Trigger, inspect, and manage Cloudflare Workers builds |
| `cloudflare-observability` | `https://observability.mcp.cloudflare.com/mcp` | Query logs, analytics, and metrics from your Cloudflare account |

> **Source:** All servers are maintained by Cloudflare at [github.com/cloudflare/mcp-server-cloudflare](https://github.com/cloudflare/mcp-server-cloudflare). This repo provides the configuration layer and usage guidance.

---

## Quick Start

### 1. Choose your auth method

**OAuth (recommended for interactive use)**
On first connection, the server will open a browser tab to authorize with your Cloudflare account. No config needed.

**API Token (recommended for CI/CD or fine-grained control)**
Create a token at https://dash.cloudflare.com/profile/api-tokens, then add it as a header (see config examples below).

### 2. Add to your MCP client

Pick the config format for your client:

#### Claude Desktop / Claude.ai
Add to `claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "cloudflare-api": {
      "type": "http",
      "url": "https://mcp.cloudflare.com/mcp"
    },
    "cloudflare-docs": {
      "type": "http",
      "url": "https://docs.mcp.cloudflare.com/mcp"
    },
    "cloudflare-bindings": {
      "type": "http",
      "url": "https://bindings.mcp.cloudflare.com/mcp"
    },
    "cloudflare-builds": {
      "type": "http",
      "url": "https://builds.mcp.cloudflare.com/mcp"
    },
    "cloudflare-observability": {
      "type": "http",
      "url": "https://observability.mcp.cloudflare.com/mcp"
    }
  }
}
```

#### With API Token auth (all clients)
Replace any server entry with:
```json
"cloudflare-api": {
  "type": "http",
  "url": "https://mcp.cloudflare.com/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_CLOUDFLARE_API_TOKEN"
  }
}
```

#### Cursor
Add to `.cursor/mcp.json` in your project root (or `~/.cursor/mcp.json` globally) — same JSON format as above.

#### Windsurf
Add to `~/.codeium/windsurf/mcp_config.json` — same JSON format.

#### Kiro (AWS)
Add to `.kiro/settings/mcp.json` in your workspace:
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

#### Zed
Add to your Zed `settings.json`:
```json
{
  "context_servers": {
    "cloudflare-api": {
      "command": {
        "path": "npx",
        "args": ["-y", "mcp-remote", "https://mcp.cloudflare.com/mcp"]
      }
    }
  }
}
```

#### Generic (mcp-remote proxy)
For any client that only supports stdio-based servers:
```bash
npx -y mcp-remote https://mcp.cloudflare.com/mcp
```

---

## What you can do with it

Once connected, your AI assistant can:

### Cloudflare Workers & Pages
- List, deploy, update, and delete Workers scripts
- Manage custom domains and routes
- Configure environment variables and secrets
- Deploy Pages projects

### Storage (D1, R2, KV)
- Query D1 SQLite databases with SQL
- Read/write R2 object storage
- Get/put/delete KV namespace entries
- Create and manage databases and buckets

### AI Products
- List and manage AI Gateway gateways
- Inspect request logs and spend limits
- Configure caching, rate limiting, and authentication
- Query Workers AI model catalog

### DNS & Networking
- List and manage DNS zones and records
- Configure Cloudflare Tunnel
- Manage load balancers and health checks

### Security
- Inspect and update WAF rules and rulesets
- Manage rate limiting rules
- Configure DDoS protection settings

### Observability
- Query Workers logs and analytics
- Inspect build history and deployment logs
- Monitor error rates and request volumes

### Example prompts to try
```
"List all my Cloudflare Workers"
"Show me the last 100 requests to my AI Gateway"
"Create a new KV namespace called 'sessions'"
"What are the current rate limits on my luvloot-beta AI gateway?"
"Deploy my Worker script to production"
"Show me DNS records for my domain"
"What does the Cloudflare docs say about Durable Object storage limits?"
```

---

## Skills & Guided Workflows

This repo includes **skill guides** under `skills/` — markdown documents that give your AI assistant deep, current knowledge of each Cloudflare product before it drives the API. Skills bias toward fetching live documentation rather than relying on stale training data.

| Skill | When to load it |
|-------|----------------|
| `cloudflare` | General platform work — Workers, Pages, storage, networking, security |
| `agents-sdk` | Building stateful AI agents with the Cloudflare Agents SDK |
| `durable-objects` | Coordinating state across WebSockets, chat, games, booking systems |
| `wrangler` | Using the Wrangler CLI for local dev and deployment |
| `workers-best-practices` | Reviewing Worker code against production standards |
| `sandbox-sdk` | Building secure code execution environments |
| `web-perf` | Auditing Core Web Vitals and page performance |
| `cloudflare-email-service` | Sending and receiving email via Cloudflare Email Routing |
| `cloudflare-one` | Zero Trust — Access, Gateway, WARP, Tunnel, DLP |
| `turnstile-spin` | Integrating Cloudflare Turnstile CAPTCHA |

### Commands
- **build-agent** — Full workflow to scaffold and deploy a stateful AI agent on Cloudflare Workers using the Agents SDK
- **build-mcp** — Full workflow to scaffold and deploy a remote MCP server on Cloudflare using McpAgent

---

## Repo Structure

```
cloudflare-mcp/
├── mcp.json                  # Drop-in MCP server config (all 5 servers)
├── POWER.md                  # Kiro Power manifest (for Kiro IDE users)
├── README.md                 # This file
├── logo.svg                  # Cloudflare logo
├── commands/
│   ├── build-agent.md        # Guided workflow: build an AI agent
│   └── build-mcp.md          # Guided workflow: build an MCP server
├── steering/
│   ├── servers.md            # Full catalog of all 16 Cloudflare MCP servers
│   └── usage-guidance.md     # Task → skill → MCP server mapping
└── skills/
    ├── cloudflare/           # Comprehensive platform skill (~50 product references)
    ├── agents-sdk/           # Agents SDK: state, scheduling, RPC, streaming
    ├── durable-objects/      # Durable Objects patterns, SQLite, testing
    ├── sandbox-sdk/          # Secure code execution
    ├── workers-best-practices/
    ├── wrangler/
    ├── web-perf/
    ├── cloudflare-email-service/
    ├── cloudflare-one/
    ├── cloudflare-one-migrations/
    └── turnstile-spin/
```

---

## Authentication & Security

- **OAuth** is the easiest path for interactive use — no tokens to manage
- **API tokens** are better for teams and CI/CD — scope them tightly to only the permissions needed
- Never commit API tokens to source control — use environment variables or secret managers
- The MCP servers run on Cloudflare's infrastructure — your credentials go directly to Cloudflare, not through any third party

Token permission recommendations by server:

| Task | Minimum token permissions |
|------|--------------------------|
| Read-only inspection | `Account:Read`, `Zone:Read` |
| Workers deploy | `Workers Scripts:Edit` |
| Full platform management | `Account:Edit`, `Zone:Edit`, `Workers Scripts:Edit` |

---

## Troubleshooting

**OAuth flow not completing**
Disconnect and reconnect the server in your MCP client. Make sure your browser isn't blocking redirects to `cloudflare.com`.

**"Unauthorized" errors with API token**
Verify the token is still valid at https://dash.cloudflare.com/profile/api-tokens and has the required permissions. Check the `Authorization: Bearer <token>` header format.

**Server URL issues**
URLs must use `https://` and end in `/mcp`. The old `/sse` suffix is deprecated and will not work.

**Client doesn't support HTTP MCP servers**
Use `npx -y mcp-remote <url>` as a proxy — it converts HTTP MCP to stdio for clients that only support local servers.

---

## Related Resources

| Resource | URL |
|----------|-----|
| Cloudflare MCP servers (official) | https://github.com/cloudflare/mcp-server-cloudflare |
| Cloudflare Skills plugin | https://github.com/cloudflare/skills |
| Cloudflare Agents SDK docs | https://developers.cloudflare.com/agents/ |
| Workers docs | https://developers.cloudflare.com/workers/ |
| AI Gateway docs | https://developers.cloudflare.com/ai-gateway/ |
| API token management | https://dash.cloudflare.com/profile/api-tokens |
| Model Context Protocol spec | https://modelcontextprotocol.io |

---

## License

MIT — see [LICENSE](LICENSE).

Skills content under `skills/` is sourced from [github.com/cloudflare/skills](https://github.com/cloudflare/skills) and subject to its license.

---

## Contributing

PRs welcome — especially for:
- Additional client config examples (VS Code, JetBrains, etc.)
- Updated skill guides when Cloudflare releases new products
- Real-world prompt examples and workflow recipes

---

*Not officially affiliated with Cloudflare. Packages the official Cloudflare MCP servers for easy community use.*
