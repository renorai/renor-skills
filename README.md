# renor-memory

Agent skill for [Renor](https://renor.cloud) — durable memory for AI agents over MCP.

The skill tells an agent **when** to recall and **when** to write: it supplies the trigger
that makes a connected agent use its memory layer instead of asking the user to repeat
context. Install it, connect Renor as an MCP server, and the agent recalls what the user
already told an AI and records what should survive the session.

The skill carries no credentials. Authentication lives in your MCP client's own config;
the skill names only a placeholder token.

## Install

Run this in your project directory. It asks nothing, so your agent can run it too:

```sh
npx skills add patrickpassosb/renor-skills --agent '*' -y
```

It puts the skill in `.agents/skills/renor-memory` in that directory and links it for the agents you use there.

In your own terminal you can run the interactive form instead. It asks which agents to install to; inside an agent it
recognises, such as Claude Code, it asks nothing. In a shell with no terminal and no recognised agent it installs
nothing, so give an agent the form above.

```sh
npx skills add patrickpassosb/renor-skills@renor-memory
```

For a specific agent (for example Claude Code and OpenCode, globally):

```sh
npx skills add patrickpassosb/renor-skills@renor-memory -a claude-code -a opencode -g -y
```

### Claude Code plugin

Claude Code can install the skill and the Renor MCP server together, as one plugin. Inside
Claude Code, run these two commands:

```text
/plugin marketplace add patrickpassosb/renor-skills
/plugin install renor@renor
```

Then sign in once: run `/mcp`, select `plugin:renor:renor`, choose Authenticate, and approve
in the browser. The plugin signs in with OAuth, so no token goes in your config.

### Other agents

Each of these installs the skill and the Renor MCP server together, or says how to add the
server. Every one signs in with OAuth in the browser, so no token goes in your config.

**Codex**

```sh
codex plugin marketplace add patrickpassosb/renor-skills
codex plugin add renor@renor
codex mcp login renor
```

**GitHub Copilot CLI**

```sh
copilot plugin marketplace add patrickpassosb/renor-skills
copilot plugin install renor@renor
```

Then sign in to the `renor` MCP server, which Copilot CLI reads from the plugin.

**Antigravity CLI** (`agy`)

```sh
agy plugin install https://github.com/patrickpassosb/renor-skills
```

Then run `/mcp`, choose `renor`, choose Authenticate, approve in the browser, and paste the
code the browser shows back into the prompt. If you only want the server, put this in
`~/.gemini/config/mcp_config.json` (or `.agents/mcp_config.json` in a workspace):

```json
{ "mcpServers": { "renor": { "serverUrl": "https://mcp.renor.cloud/mcp" } } }
```

**OpenCode** — add this to `opencode.json` in your project, then run `opencode mcp auth renor`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "renor": { "type": "remote", "url": "https://mcp.renor.cloud/mcp" }
  }
}
```

**Cline** — add `https://mcp.renor.cloud/mcp` as a remote server with the Streamable HTTP
transport and click Authenticate. The exact steps for the extension and the CLI are in
[`llms-install.md`](llms-install.md).

## What it does

- `skills/renor-memory/SKILL.md` — the skill itself, in the Agent Skills format (YAML
  frontmatter with `name` and `description`, then the body your agent reads).

Install it, then add the Renor MCP server to your agent (one endpoint, over Streamable
HTTP):

```text
https://mcp.renor.cloud/mcp
```

Connecting needs a Renor account: the agent adds the URL, you sign in once in the
browser and click **Approve**.
