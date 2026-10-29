# Installing the flayr MCP server

flayr is a **remote** MCP server. There is nothing to clone, build or run locally.

- Server URL: `https://flayr.social/api/mcp`
- Transport: Streamable HTTP (`type: "streamable-http"` or `"http"`)
- Auth: a flayr API key sent as `Authorization: Bearer flayr_sk_…`, or OAuth sign-in (the client opens a browser and the person signs in to flayr and picks a brand)
- Plan: the person's flayr account needs the Pro plan or higher

## Steps

1. Add the server to the MCP settings file (for Cline: `cline_mcp_settings.json`):

   ```json
   {
     "mcpServers": {
       "flayr": {
         "type": "streamableHttp",
         "url": "https://flayr.social/api/mcp"
       }
     }
   }
   ```

2. If the client supports MCP OAuth, it will open flayr's sign-in page on first use. The person signs in, picks the brand the agent may use, and approves.

3. If the client does not support OAuth, ask the person for a flayr API key (created at https://flayr.social/developers) and add it as a header:

   ```json
   {
     "mcpServers": {
       "flayr": {
         "type": "streamableHttp",
         "url": "https://flayr.social/api/mcp",
         "headers": { "Authorization": "Bearer flayr_sk_YOUR_KEY" }
       }
     }
   }
   ```

   Never print or commit the key.

4. Check the connection by calling the `whoami` tool. It returns the account, plan and brand the connection can reach.

## Notes for the agent

- `publish` posts to real social accounts. Confirm with the person first and pass an `idempotencyKey`.
- Full reference for every tool: https://flayr.social/llms.txt
