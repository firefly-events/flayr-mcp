# flayr

flayr makes and posts a business's social content. Its MCP server lets you work with the brands the person connected.

- Start with `whoami`, then `list_brands`.
- `create_content` makes a draft. Nothing is posted.
- `publish` posts to real social accounts. Confirm with the person before you call it, and always pass an `idempotencyKey`.
- Tools marked destructive delete data or act on live accounts. Ask first.
- Errors come back as the REST API's status and JSON body: 401 sign in again, 402 plan too low, 403 missing permission or another brand, 422 validation, 429 rate limit.

Reference: https://flayr.social/llms.txt
