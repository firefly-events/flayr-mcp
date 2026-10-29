# flayr MCP server

flayr is a one stop shop for a business's social content. Its remote MCP server lets AI agents and assistants work with your brands: list brands and connected accounts, draft posts, attach media, schedule or publish to 13 platforms, manage posting queues and approvals, and read analytics.

- **Server URL:** `https://flayr.social/api/mcp` (Streamable HTTP, stateless)
- **Auth:** OAuth sign-in or a flayr API key. With OAuth your client opens flayr's sign-in page, you sign in, pick the brand the agent may use and approve; you can revoke the connection at any time. Clients without MCP OAuth use an API key as `Authorization: Bearer flayr_sk_…` (create one at https://flayr.social/developers).
- **Plan:** the MCP server needs the flayr **Pro plan or higher**. See [Plans](#plans).
- **Tools:** see [`tools.json`](tools.json). Read-only tools are annotated read-only; anything that publishes or deletes is annotated destructive, so your assistant asks first.
- **Reference:** https://flayr.social/llms.txt · API docs https://flayr.social/docs
- **Privacy:** https://flayr.social/privacy · **Terms:** https://flayr.social/terms · **Help:** https://flayr.social/help

There is nothing to install or run locally. This repository holds the listing metadata the MCP directories read (`server.json` for the official MCP Registry, `gemini-extension.json` for Gemini CLI, `llms-install.md` for Cline, `docker/` for the Docker MCP Catalog).

## Store-specific endpoints

| URL | Tool set |
| --- | --- |
| `https://flayr.social/api/mcp` | All tools (57) |
| `https://flayr.social/api/mcp/claude` | The Claude directory build (55 tools; leaves out the clip-combining tools) |
| `https://flayr.social/api/mcp/chatgpt` | The ChatGPT directory build (55 tools) |

## Connect

Menu names in each app change often. The URL and auth are what matter. The examples without a header use OAuth sign-in. If your client doesn't support MCP OAuth, add the `Authorization: Bearer flayr_sk_YOUR_KEY` header as in [Any other MCP client](#any-other-mcp-client).

### Claude Code

```bash
claude mcp add --transport http flayr https://flayr.social/api/mcp
# then run /mcp in Claude Code and choose "Authenticate"
```

With an API key instead:

```bash
claude mcp add --transport http flayr https://flayr.social/api/mcp \
  --header "Authorization: Bearer flayr_sk_YOUR_KEY"
```

### Claude (claude.ai and Claude Desktop)

Settings → Connectors → Add custom connector → URL `https://flayr.social/api/mcp/claude` → Connect, then sign in to flayr and pick a brand.

### ChatGPT (developer mode)

Settings → Apps & Connectors → Advanced settings → enable Developer mode → Create. URL `https://flayr.social/api/mcp/chatgpt`, authentication OAuth.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "flayr": { "url": "https://flayr.social/api/mcp" }
  }
}
```

### VS Code (Copilot agent mode)

`.vscode/mcp.json`:

```json
{
  "servers": {
    "flayr": { "type": "http", "url": "https://flayr.social/api/mcp" }
  }
}
```

### Gemini CLI

```bash
gemini extensions install https://github.com/firefly-events/flayr-mcp
```

Or in `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "flayr": { "httpUrl": "https://flayr.social/api/mcp" }
  }
}
```

### Perplexity and Mistral Le Chat

Add a custom MCP connector with URL `https://flayr.social/api/mcp`. If the app asks for a header or bearer token instead of OAuth, use a flayr API key.

### Any other MCP client

```json
{
  "type": "http",
  "url": "https://flayr.social/api/mcp",
  "headers": { "Authorization": "Bearer flayr_sk_YOUR_KEY" }
}
```

## A first conversation

1. "Which flayr brands can you reach?" → `whoami`, `list_brands`
2. "Draft a LinkedIn post about Friday's event for <brand>." → `create_content` (a draft; nothing is posted)
3. "Schedule it for 9 a.m. tomorrow." → the assistant confirms, then `publish` with `scheduledAt`
4. "What's scheduled this week?" → `list_content`

## Plans

Over MCP, connecting, reading, drafting and publishing need the flayr **Pro plan or higher**. Each publish counts against your plan's direct posts for the month. Posting queues, reviews and approvals need the Agency plan or higher.

You can still connect on a lower plan: the assistant can tell you which plan you have, and its tools say your plan doesn't include them. Once your plan includes them, the same connection works without reconnecting. See https://flayr.social/pricing and https://flayr.social/docs/ai-assistants.

## License

MIT, see [LICENSE](LICENSE). The flayr service itself is governed by https://flayr.social/terms.
