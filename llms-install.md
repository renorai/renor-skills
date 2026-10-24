# Install Renor in Cline

Renor is a hosted MCP server. There is nothing to clone, build or run locally, and no
API key to paste: you sign in once in the browser (OAuth).

Remote endpoint (Streamable HTTP):

```text
https://mcp.renor.cloud/mcp
```

## Steps for Cline (VS Code extension)

1. Open the MCP Servers icon in the Cline panel and go to the **Remote Servers** tab.
2. Name the server `renor` and enter the URL `https://mcp.renor.cloud/mcp`. Choose the
   Streamable HTTP transport.
3. The server shows an authentication error. Click **Authenticate**, sign in to Renor in
   the browser and click **Approve**. Cline stores the credentials and the server turns green.

Equivalent entry in `cline_mcp_settings.json` (open it from the **Configure** tab, then
**Configure MCP Servers**). `type` must be `streamableHttp`; leaving it out selects the
legacy SSE transport, which Renor does not serve:

```json
{
  "mcpServers": {
    "renor": {
      "type": "streamableHttp",
      "url": "https://mcp.renor.cloud/mcp",
      "disabled": false
    }
  }
}
```

Do not add an `Authorization` header or a token: authentication is the browser sign-in.

## Steps for the Cline CLI

```sh
cline mcp add renor https://mcp.renor.cloud/mcp --transport http --yes
cline mcp
```

In the `cline mcp` wizard, choose **Authorize OAuth** and the `renor` server, then approve
in the browser.

## Add the skill

The MCP server gives the agent the tools. The `renor-memory` skill tells it when to use
them. Run this in your project directory:

```sh
npx skills add renorai/renor-skills --agent '*' -y
```

## Requirements

A Renor account. Signing in happens in the browser the first time.
