# MCP Registry

A minimal MCP registry allowing only the Atlassian MCP server, for use with GitHub Copilot enterprise allowlisting.

## Setup

1. Push this repo to GitHub
2. Go to **Settings → Pages** in your GitHub repo
3. Under "Source", select **Deploy from a branch**
4. Select the **main** branch and **/ (root)** folder, then click **Save**
5. After a minute or two, your registry will be live at:
   `https://<your-org>.github.io/<repo-name>`

## Connecting to GitHub Copilot

Once your Pages site is live:

1. Go to your GitHub **Organization Settings → Copilot → Policies**
2. Set **MCP servers in Copilot** to **Enabled**
3. Paste your Pages URL into the **MCP Registry URL** field and click **Save**
4. Set **Restrict MCP access to registry servers** to **Registry only**

## Adding or removing MCP servers

To add another server, add an entry to `v0.1/servers/index.json` and create the corresponding version files under `v0.1/servers/<name>/versions/`.

To remove the Atlassian server (or replace it), edit `v0.1/servers/index.json` and delete or update the relevant version folders.
