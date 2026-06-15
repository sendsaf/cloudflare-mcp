# Cloudflare MCP — Usage Guidance & Skills

The MCP servers in this power are the **connection layer** — they let an agent read and change Cloudflare resources. The *knowledge* of how to build correctly on Cloudflare lives in **skills**. The official Cloudflare Skills plugin (https://github.com/cloudflare/skills) bundles both together; this power separates them because Kiro already provides the skill side natively.

## Where the guidance lives

Cloudflare's skills are already installed in this workspace as native Kiro skills under `.kiro/skills/`. When doing real Cloudflare work, load the relevant skill **before** driving the MCP tools — the skill biases toward retrieving current Cloudflare docs rather than relying on stale pre-trained knowledge.

| Task | MCP server to use | Skill to load first |
|------|-------------------|---------------------|
| Any Cloudflare platform task (Workers, Pages, KV, D1, R2, AI, networking, security) | `cloudflare-api` | `cloudflare` |
| Deploy / manage via CLI (Workers, KV, R2, D1, Queues, Workflows) | `cloudflare-api`, `cloudflare-builds` | `wrangler` |
| Author / review Worker code to production standards | `cloudflare-bindings` | `workers-best-practices` |
| Build a stateful AI agent (state, scheduling, RPC, streaming chat) | `cloudflare-bindings` | `agents-sdk` / `building-ai-agent-on-cloudflare` |
| Build a remote MCP server (tools, OAuth, deploy) | `cloudflare-bindings`, `cloudflare-builds` | `building-mcp-server-on-cloudflare` |
| Stateful coordination (chat rooms, games, booking), SQLite, alarms, WebSockets | `cloudflare-bindings` | `durable-objects` |
| Secure code execution / code interpreters / sandboxes | `cloudflare-containers` | `sandbox-sdk` |
| Debug logs, analytics, app performance | `cloudflare-observability`, `cloudflare-logpush` | `observability` / `web-perf` |
| AI Gateway request inspection | `cloudflare-ai-gateway` | `ai-gateway` |
| Audit Core Web Vitals / page performance | `cloudflare-browser` | `web-perf` |

To load a skill in Kiro, activate it by name (e.g. the `cloudflare`, `wrangler`, or `agents-sdk` skill). The skill's decision trees point to the right product reference; then use the MCP server to inspect or apply changes against the live account.

## Slash-command equivalents

The Cloudflare Skills plugin exposes user-invocable slash commands. Kiro has no slash-command mechanism, but the same workflows are covered by loading the matching skill and driving the MCP servers:

| Plugin command | Kiro equivalent |
|----------------|-----------------|
| `/cloudflare:build-agent` | Load `building-ai-agent-on-cloudflare` (or `agents-sdk`) skill, then use `cloudflare-bindings` + `cloudflare-builds` |
| `/cloudflare:build-mcp` | Load `building-mcp-server-on-cloudflare` skill, then use `cloudflare-bindings` + `cloudflare-builds` |

## Recommended working pattern

1. **Identify the task** and load the matching skill (guidance + current-docs retrieval).
2. **Read** current state with the MCP server (`search()` then `execute()` on `cloudflare-api`, or a product-specific server).
3. **Apply** changes with the MCP server — review any write, deploy, billing, auth, or destructive operation before allowing it.
4. **Verify** with the observability/analytics servers or `wrangler` where appropriate.

## Why this power doesn't embed the full skill text

The `cloudflare` skill alone spans dozens of product references and explicitly tells agents to **retrieve current docs over baked-in knowledge**. Copying it into this power would duplicate content that already exists in `.kiro/skills/`, and it would drift out of date. Pointing at the live skills keeps a single source of truth.
