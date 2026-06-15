# Cloudflare MCP Servers — Full Catalog

Complete reference for all Cloudflare managed remote MCP servers. Each entry includes the server URL, what it does, when to use it, and a ready-to-paste `mcp.json` snippet.

---

## Cloudflare API MCP Server

**URL:** `https://mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp

The primary server. Covers the entire Cloudflare API — 2,500+ endpoints — using only two tools:

- `search(query)` — finds relevant API endpoints for a task
- `execute(code)` — runs JavaScript against a typed Cloudflare API client inside an isolated Dynamic Worker sandbox

**Token cost:** ~1,000 tokens (vs ~1,170,000 for full native MCP). This is the [Codemode](https://developers.cloudflare.com/agents/model-context-protocol/protocol/codemode/) technique.

**Covers:** DNS, Workers, R2, KV, D1, Pages, Zero Trust, WAF, Cache, Images, Stream, Email Routing, Queues, Vectorize, AI Gateway, Radar, Analytics — everything.

**When to use:** Any Cloudflare task not served by a product-specific server, or when you want a single server for all CF work.

```json
{
  "mcpServers": {
    "cloudflare-api": {
      "url": "https://mcp.cloudflare.com/mcp"
    }
  }
}
```

**With API token (CI/CD):**
```json
{
  "mcpServers": {
    "cloudflare-api": {
      "url": "https://mcp.cloudflare.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_CLOUDFLARE_API_TOKEN"
      }
    }
  }
}
```

---

## Documentation Server

**URL:** `https://docs.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/docs-vectorize

Semantic search across all Cloudflare documentation. Returns up-to-date reference content, configuration guides, and API specs.

**When to use:** When you need current Cloudflare docs — config options, API reference, limits, pricing, compatibility flags. Prefer this over relying on training-data knowledge for CF-specific details.

```json
{
  "mcpServers": {
    "cloudflare-docs": {
      "url": "https://docs.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Workers Bindings Server

**URL:** `https://bindings.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/workers-bindings

Helps build Workers applications with access to Cloudflare's storage, AI, and compute primitives (KV, D1, R2, Queues, Vectorize, Workers AI, Durable Objects, etc.).

**When to use:** Scaffolding, configuring, or debugging Workers with bindings. Good companion to local `wrangler dev` when you need to inspect or modify binding configurations via the API.

```json
{
  "mcpServers": {
    "cloudflare-bindings": {
      "url": "https://bindings.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Workers Builds Server

**URL:** `https://builds.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/workers-builds

Get insights into and manage Cloudflare Workers Builds — build history, status, logs, and triggering new builds.

**When to use:** CI/CD debugging, checking why a Worker build failed, reviewing build logs, or triggering deployments.

```json
{
  "mcpServers": {
    "cloudflare-builds": {
      "url": "https://builds.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Observability Server

**URL:** `https://observability.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/workers-observability

Debug and inspect your applications via Workers logs and analytics. Query logs, filter by time or request ID, and get aggregated analytics.

**When to use:** Debugging live Worker errors, tailing logs for a specific route or status code, getting traffic analytics, or investigating latency spikes.

```json
{
  "mcpServers": {
    "cloudflare-observability": {
      "url": "https://observability.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Radar Server

**URL:** `https://radar.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/radar

Global Internet traffic insights, trends, URL scanning, and network utilities using Cloudflare Radar data.

**When to use:** Researching global traffic patterns, scanning URLs for threats, checking BGP routing data, or getting ASN/IP intelligence. Public data — no account auth required for most operations.

```json
{
  "mcpServers": {
    "cloudflare-radar": {
      "url": "https://radar.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Container Server

**URL:** `https://containers.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/sandbox-container

Spin up ephemeral sandbox development environments (Cloudflare Containers / sandbox workers). Run untrusted code, test scripts, or isolated dev workflows.

**When to use:** Running code in a safe sandbox, testing scripts before deploying, building AI code-execution features, or any isolated compute need.

```json
{
  "mcpServers": {
    "cloudflare-containers": {
      "url": "https://containers.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Browser Run Server

**URL:** `https://browser.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/browser-rendering

Fetch web pages using Cloudflare Browser Rendering, convert them to clean Markdown, and take screenshots.

**When to use:** Scraping pages for content extraction, converting docs/articles to Markdown for analysis, taking screenshots of URLs, or checking how a page renders.

```json
{
  "mcpServers": {
    "cloudflare-browser": {
      "url": "https://browser.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Logpush Server

**URL:** `https://logs.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/logpush

Get quick health summaries for Cloudflare Logpush jobs — job status, delivery failures, and configuration details.

**When to use:** Monitoring whether Logpush jobs are delivering logs to your destination (S3, R2, Splunk, etc.), debugging delivery failures, or auditing Logpush configuration.

```json
{
  "mcpServers": {
    "cloudflare-logpush": {
      "url": "https://logs.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## AI Gateway Server

**URL:** `https://ai-gateway.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/ai-gateway

Search and inspect AI Gateway logs — view prompts, responses, model usage, costs, and latencies across your AI Gateway gateways.

**When to use:** Debugging AI requests routed through AI Gateway, reviewing prompt/response history, analysing model usage and cost, or investigating latency.

```json
{
  "mcpServers": {
    "cloudflare-ai-gateway": {
      "url": "https://ai-gateway.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## AI Search (AutoRAG) Server

**URL:** `https://autorag.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/autorag

List and search documents stored in your Cloudflare AI Search (AutoRAG) indexes.

**When to use:** Inspecting what's in your AutoRAG document store, running semantic searches against your indexed content, or debugging retrieval quality.

```json
{
  "mcpServers": {
    "cloudflare-autorag": {
      "url": "https://autorag.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Audit Logs Server

**URL:** `https://auditlogs.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/auditlogs

Query Cloudflare account audit logs and generate reports for compliance or security review.

**When to use:** Investigating who changed what in your Cloudflare account, generating audit reports for compliance, or tracking configuration changes over time.

```json
{
  "mcpServers": {
    "cloudflare-auditlogs": {
      "url": "https://auditlogs.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## DNS Analytics Server

**URL:** `https://dns-analytics.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/dns-analytics

Optimize DNS performance and debug issues based on your current DNS configuration and query analytics.

**When to use:** Investigating DNS resolution failures, reviewing query volume and latency, optimizing TTLs, or troubleshooting DNS propagation.

```json
{
  "mcpServers": {
    "cloudflare-dns-analytics": {
      "url": "https://dns-analytics.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Digital Experience Monitoring (DEX) Server

**URL:** `https://dex.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/dex-analysis

Get quick insight into the performance and availability of critical applications for your organization via Cloudflare DEX (part of Zero Trust).

**When to use:** Monitoring application experience for remote employees, checking synthetic test results, diagnosing connectivity issues reported by users.

```json
{
  "mcpServers": {
    "cloudflare-dex": {
      "url": "https://dex.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Cloudflare One CASB Server

**URL:** `https://casb.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/cloudflare-one-casb

Quickly identify security misconfigurations for SaaS applications to safeguard users and data (Cloudflare One CASB).

**When to use:** Security audits of connected SaaS apps (Google Workspace, Microsoft 365, Salesforce, etc.), finding misconfigured sharing settings, reviewing access control issues.

```json
{
  "mcpServers": {
    "cloudflare-casb": {
      "url": "https://casb.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## GraphQL Server

**URL:** `https://graphql.mcp.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/graphql/

Get analytics data using Cloudflare's GraphQL Analytics API — HTTP traffic, firewall events, Workers metrics, Cache performance, and more.

**When to use:** Deep analytics queries, custom dashboards, or when the standard API endpoints don't surface the aggregation or filtering you need.

```json
{
  "mcpServers": {
    "cloudflare-graphql": {
      "url": "https://graphql.mcp.cloudflare.com/mcp"
    }
  }
}
```

---

## Agents SDK Documentation Server

**URL:** `https://agents.cloudflare.com/mcp`
**Repo:** https://github.com/cloudflare/agents/tree/main/site/agents

Token-efficient semantic search of the Cloudflare Agents SDK documentation.

**When to use:** Building agents with the Cloudflare Agents SDK — look up API shapes, patterns, Durable Objects integration, WebSocket handling, and Workers AI tooling.

```json
{
  "mcpServers": {
    "cloudflare-agents-sdk": {
      "url": "https://agents.cloudflare.com/mcp"
    }
  }
}
```

---

## Using Multiple Servers Together

You can mix and match. Example — full development + monitoring setup:

```json
{
  "mcpServers": {
    "cloudflare-api": {
      "url": "https://mcp.cloudflare.com/mcp"
    },
    "cloudflare-docs": {
      "url": "https://docs.mcp.cloudflare.com/mcp"
    },
    "cloudflare-observability": {
      "url": "https://observability.mcp.cloudflare.com/mcp"
    },
    "cloudflare-agents-sdk": {
      "url": "https://agents.cloudflare.com/mcp"
    }
  }
}
```

Each server authenticates independently via OAuth — you'll go through the flow once per server.
