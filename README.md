# Claude Plugins Directory MCP server

A hosted, read-only [MCP](https://modelcontextprotocol.io) server for searching [Cowork Plugins](https://www.claudecoworkplugins.com/). Cowork Plugins is an independent directory of plugins and skills for Claude, Claude Cowork and Claude Code. Ask from Claude Code, Claude or any MCP client:

- find a plugin for a job;
- see its skills;
- get its install commands.

You don't have to leave your session.

```
https://www.claudecoworkplugins.com/mcp/
```

- **Transport:** Streamable HTTP (stateless, JSON responses)
- **Auth:** none. No account, no API key.
- **Read-only:** it serves the same public data as the website, writes nothing, and keeps no record of what you ask.
- **Official MCP Registry:** [`com.claudecoworkplugins/directory`](https://registry.modelcontextprotocol.io/v0/servers?search=com.claudecoworkplugins/directory)

This repository holds the server's documentation and its registry entry ([`server.json`](server.json)). The server itself is hosted; there is nothing to install or run.

## Add it

### Claude Code

```bash
claude mcp add --transport http claude-plugins https://www.claudecoworkplugins.com/mcp/
```

Add `--scope user` to use it in every project. `claude mcp list` should show it as connected.

### Claude (claude.ai and the desktop app)

1. Open **Customize → Connectors**.
2. Add a custom connector with the address `https://www.claudecoworkplugins.com/mcp/`.

### Cursor, VS Code and other clients

Use your client's remote (HTTP) server setting:

```json
{
  "mcpServers": {
    "claude-plugins": {
      "type": "http",
      "url": "https://www.claudecoworkplugins.com/mcp/"
    }
  }
}
```

## Tools

All four tools are read-only (`readOnlyHint: true`).

| Tool | What it does | Inputs |
|---|---|---|
| `search_plugins` | Find plugins by what they do. Every word of the query must match the name, description, category or tags. | `query`, `category`, `source` (`all`, `official` or `community`), `limit` (1–25) |
| `get_plugin` | One plugin's details: description, source, licence, stars, its skills, and its install commands | `slug` (required) |
| `search_skills` | Find individual skills inside plugins, with the command that runs each one | `query` (required), `limit` (1–25) |
| `list_categories` | Every category and how many plugins it has | none |

Things to try:

- "Which Claude Code plugins help with code review?"
- "Show me the Sales plugin's skills."
- "Is there a skill for writing an NDA?"

## Try it with curl

```bash
curl -s -X POST https://www.claudecoworkplugins.com/mcp/ \
  -H 'content-type: application/json' \
  -H 'accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"search_plugins","arguments":{"query":"code review","limit":3}}}'
```

## Before you install what it finds

The directory lists:

- official plugins from Anthropic's marketplaces;
- partner plugins;
- community plugins from GitHub.

Community plugins pass automated checks (a licence, recent updates, a manifest, real skills), then the site owner approves each one. That approval is not a code or security review, and the server says so in its answers. Read [Are Claude plugins safe?](https://www.claudecoworkplugins.com/blog/are-claude-plugins-safe/) before installing one.

## More

- **Docs page:** https://www.claudecoworkplugins.com/use-in-claude/
- **Other ways to read the directory:**
  - [AGENTS.md](https://www.claudecoworkplugins.com/AGENTS.md)
  - [llms.txt](https://www.claudecoworkplugins.com/llms.txt)
  - the JSON APIs listed there
- **Problems or suggestions:** open an issue here, or use the [contact page](https://www.claudecoworkplugins.com/contact/).

Cowork Plugins is independent and is not affiliated with, endorsed by or sponsored by Anthropic. Claude is a trademark of Anthropic, PBC.

The documentation and `server.json` in this repository are MIT-licensed. The directory's data belongs to each plugin's authors.
