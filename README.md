<img src="logo.png" alt="Hooklistener" width="96" height="96">

# Hooklistener MCP Server

Hosted MCP server that lets AI coding agents test webhooks end to end: create a public webhook URL, wait for the webhook to arrive, verify its signature, and replay it to localhost. The same server also covers email inboxes, WebSocket/Socket.IO/MQTT/SSE endpoints, localhost tunnels and uptime monitors.

- **Endpoint:** `https://app.hooklistener.com/api/mcp`
- **Transport:** Streamable HTTP. Nothing to install or run locally.
- **Auth:** OAuth 2.1 with PKCE and dynamic client registration. Clients request `full_access` or `read_only`. A Hooklistener API key (`hklst_…`) also works as a Bearer token for clients without OAuth.
- **Tools:** 69, across 11 categories.
- **Plans:** works on every plan, including Free.

This repository holds the setup instructions and the [MCP Registry](https://registry.modelcontextprotocol.io) entry (`server.json`). The server itself is operated by Hooklistener.

## Connect your client

### Claude Code

```bash
claude mcp add --transport http hooklistener https://app.hooklistener.com/api/mcp
```

Then run `/mcp` in Claude Code, select `hooklistener` and sign in with your browser. [Full guide](https://www.hooklistener.com/mcp/claude-code)

### Codex

```bash
codex mcp add hooklistener \
  --url https://app.hooklistener.com/api/mcp \
  --oauth-resource https://app.hooklistener.com/api/mcp
codex mcp login hooklistener --scopes full_access   # or read_only
```

[Full guide](https://www.hooklistener.com/mcp/codex)

### Cursor

`.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "hooklistener": { "url": "https://app.hooklistener.com/api/mcp" }
  }
}
```

[Full guide](https://www.hooklistener.com/mcp/cursor)

### VS Code (GitHub Copilot)

`.vscode/mcp.json`:

```json
{
  "servers": {
    "hooklistener": { "type": "http", "url": "https://app.hooklistener.com/api/mcp" }
  }
}
```

[Full guide](https://www.hooklistener.com/mcp/vs-code)

### Claude.ai and Claude Desktop

Customize → Connectors → **+** → Add custom connector → paste `https://app.hooklistener.com/api/mcp` → Add → Connect. [Full guide](https://www.hooklistener.com/mcp/claude)

### Other clients

Step-by-step guides: [ChatGPT](https://www.hooklistener.com/mcp/chatgpt) · [Gemini CLI](https://www.hooklistener.com/mcp/gemini-cli) · [Windsurf](https://www.hooklistener.com/mcp/windsurf) · [Zed](https://www.hooklistener.com/mcp/zed) · [OpenCode](https://www.hooklistener.com/mcp/opencode) · [Grok (xAI API)](https://www.hooklistener.com/mcp/grok)

Any MCP client that supports Streamable HTTP can connect with the endpoint above.

## What your agent can do

| Category | Examples |
| --- | --- |
| Debug endpoints | `create_endpoint`, `get_endpoint`, `list_endpoint_anomalies`, `set_endpoint_alerts` |
| Captured requests | `wait_for_request`, `search_requests`, `get_request`, `diagnose_request`, `investigate_request_retries` |
| Verify, compare and replay | `verify_request_signature` (Stripe, GitHub, Slack), `validate_request` (JSON Schema), `diff_requests`, `replay_request` (edit the body and re-sign) |
| Mock responses | `create_response_rule`, `test_response_rules` |
| Request threads | `set_thread_rule`, `list_request_threads` |
| Replay cases and suites | `save_request_case`, `run_endpoint_cases`, `wait_for_case_run` |
| Real-time endpoints | `create_realtime_endpoint` (WebSocket, Socket.IO, MQTT, SSE), `wait_for_realtime_message`, `send_realtime_message` |
| Email inboxes | `create_inbox`, `wait_for_email`, `get_email` |
| Localhost tunnels | `plan_tunnel_action`, `replay_tunnel_capture` |
| Uptime monitors | `create_monitor`, `get_monitor_status` |
| Secrets and tasks | `create_secret`, `cancel_task` |

Every tool has a title and read-only/destructive annotations. Full reference: [docs.hooklistener.com/mcp/available-tools](https://docs.hooklistener.com/mcp/available-tools)

### Example prompts

- "Create a Hooklistener endpoint for Stripe, trigger a test checkout, and wait for `checkout.session.completed`."
- "Verify that webhook's signature with my Stripe secret, then replay it to `http://localhost:3000/webhooks`, re-signed."
- "Create an inbox, sign up with it on staging, and read me the verification code from the email."
- "Save the last three webhooks as a replay suite and run it against localhost after my fix."

## Safety

- **Read-only access:** request the `read_only` scope and the agent can search, wait for, diff, validate and diagnose traffic, but can't create, edit, delete, replay, forward or send anything.
- **Deletes are two-step:** preview with `dry_run`, then confirm.
- **Replays and forwards** need an idempotency key, so a retried call never sends a webhook twice.
- **Credentials are masked** in tool results: common secrets in headers, cookies, query strings and JSON bodies.
- Data is scoped to your organization and hosted in Europe.

## No account yet?

Agents can create a temporary webhook URL without an account, which the user can claim later. See [app.hooklistener.com/llms.txt](https://app.hooklistener.com/llms.txt).

## Links

- Website: https://www.hooklistener.com/mcp
- Docs: https://docs.hooklistener.com/mcp/overview
- Webhook MCP servers compared: https://www.hooklistener.com/compare/webhook-mcp-servers
- Privacy: https://www.hooklistener.com/privacy
- Support: support@hooklistener.com
