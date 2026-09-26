# Installing the Hooklistener MCP server (for AI agents)

Hooklistener is a hosted MCP server. There is nothing to download, build or
run locally: you add one URL to the MCP settings and authenticate.

- **URL:** `https://app.hooklistener.com/api/mcp`
- **Transport:** Streamable HTTP
- **Auth:** OAuth (browser sign-in) where the client supports it for remote
  servers; otherwise a Hooklistener API key sent as `Authorization: Bearer hklst_…`

## Cline

Cline documents bearer-token headers for remote servers, so use an API key.

1. Ask the user for a Hooklistener API key. They create one at
   https://app.hooklistener.com/organization/settings/api-keys (API keys are
   available on paid plans; keys start with `hklst_`). Never invent a key.
2. Add this entry to `cline_mcp_settings.json`, keeping any existing servers:

   ```json
   {
     "mcpServers": {
       "hooklistener": {
         "type": "streamableHttp",
         "url": "https://app.hooklistener.com/api/mcp",
         "headers": {
           "Authorization": "Bearer hklst_REPLACE_WITH_USER_KEY"
         },
         "disabled": false,
         "autoApprove": []
       }
     }
   }
   ```

   `type` must be exactly `streamableHttp`. Without it Cline falls back to
   SSE and the connection fails.

   Alternatively, in Cline's MCP Servers panel open **Remote Servers**, enter
   `hooklistener` as the name and the URL above, choose **Streamable HTTP**,
   and add the `Authorization` header in the JSON afterwards.
3. Verify: call the `list_endpoints` tool. An empty list is a success for a new
   account. Then try `create_endpoint` with a name such as `cline-test` and
   show the user the returned webhook URL.

Leave `autoApprove` empty unless the user asks otherwise. Delete tools
(`delete_*`) require a dry run and an explicit confirmation anyway.

## Which tools appear

A new workspace lists 26 of the 67 tools: every webhook tool plus the tool
that starts each other product (`create_inbox`, `create_monitor`,
`create_realtime_endpoint`, `save_request_case`, `create_response_rule`). After
the first inbox, monitor or real-time endpoint, that toolset appears when Cline
reconnects. To list everything up front, add the header
`"x-hooklistener-toolsets": "all"` next to `Authorization`.

## No account yet

To receive a webhook without an account, `POST https://app.hooklistener.com/api/v1/anon/endpoints`
returns a temporary webhook URL (up to 24 hours, 500 requests) that the user
can claim later. See https://app.hooklistener.com/llms.txt.

## Other clients

Step-by-step setup for Claude Code, Codex, Cursor, VS Code, Claude.ai,
ChatGPT, Gemini CLI, Windsurf, Zed, OpenCode and Grok:
https://www.hooklistener.com/mcp
