# MCP Registry

A minimal MCP registry compliant with the [v0.1 MCP Registry API spec](https://registry.modelcontextprotocol.io/docs), served via GitHub Pages. Currently allowlists the Atlassian MCP server.

## Setup

1. Push this repo to GitHub
2. Go to **Settings → Pages** in your GitHub repo
3. Under "Source", select **Deploy from a branch**
4. Select the **main** branch and **/ (root)** folder, then click **Save**
5. After a minute or two, your registry will be live at:
   `https://<your-org>.github.io/<repo-name>`

## API Endpoints

All endpoints return JSON (served as `text/html` — a GitHub Pages limitation). Requests without a trailing slash redirect automatically.

| Endpoint | Description |
|----------|-------------|
| `GET /v0.1/servers/` | List all servers (ServerListResponse) |
| `GET /v0.1/servers/{name}/versions/` | List versions of a server |
| `GET /v0.1/servers/{name}/versions/{version}/` | Get a specific version (ServerResponse) |
| `GET /v0.1/servers/{name}/versions/latest/` | Get the latest version |

Where `{name}` uses forward slashes as path separators (e.g. `com.atlassian/mcp`).

## Connecting to GitHub Copilot

Once your Pages site is live:

1. Go to your GitHub **Organization Settings → Copilot → Policies**
2. Set **MCP servers in Copilot** to **Enabled**
3. Paste your Pages URL into the **MCP Registry URL** field and click **Save**
4. Set **Restrict MCP access to registry servers** to **Registry only**

## File Structure

```
v0.1/servers/
  index.html                          ← GET /v0.1/servers/
  {reverse-dns}/{id}/
    versions/
      index.html                      ← GET /v0.1/servers/{name}/versions/
      {version}/
        index.html                    ← GET /v0.1/servers/{name}/versions/{version}/
      latest/
        index.html                    ← GET /v0.1/servers/{name}/versions/latest/
```

## JSON Schema

Responses follow the [MCP Registry v0.1 spec](https://registry.modelcontextprotocol.io/openapi.yaml):

**ServerListResponse** (`/v0.1/servers/`):
```json
{
  "servers": [{ "server": { ... }, "_meta": { ... } }],
  "metadata": { "count": 1 }
}
```

**ServerResponse** (`/v0.1/servers/{name}/versions/{version}/`):
```json
{
  "server": {
    "$schema": "https://static.modelcontextprotocol.io/schemas/2025-12-11/server.schema.json",
    "name": "com.example/server-id",
    "title": "Display Name",
    "description": "...",
    "version": "1.0.0",
    "remotes": [{ "type": "streamable-http", "url": "https://..." }]
  },
  "_meta": {
    "io.modelcontextprotocol.registry/official": {
      "status": "active",
      "statusChangedAt": "...",
      "publishedAt": "...",
      "updatedAt": "...",
      "isLatest": true
    }
  }
}
```

## Adding a Server

1. Create the directory `v0.1/servers/{reverse-dns}/{id}/versions/`
2. Add `index.html` (versions list), `{version}/index.html`, and `latest/index.html`
3. Add an entry to `v0.1/servers/index.html` (increment `metadata.count`)

The server `name` field must be in reverse-DNS format with one slash (e.g. `com.example/my-server`).
