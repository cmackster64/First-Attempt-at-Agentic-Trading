# First Attempt at Agentic Trading

Experiments in driving a brokerage account from an AI agent.

## Robinhood trading MCP server

This repo ships a project-scoped MCP server config in `.mcp.json`:

```json
{
  "mcpServers": {
    "robinhood-trading": {
      "type": "http",
      "url": "https://agent.robinhood.com/mcp/trading"
    }
  }
}
```

It was added with:

```
claude mcp add robinhood-trading --transport http https://agent.robinhood.com/mcp/trading -s project
```

Project scope (`-s project`) writes the config into `.mcp.json` at the repo root so it
is version controlled and shared, rather than into the per-user config where it would
only exist on one machine.

### First-run setup

1. Open Claude Code in this directory. It will ask once whether to trust the
   project-scoped MCP servers in `.mcp.json`. Approve it.
2. Run `/mcp`, select `robinhood-trading`, and complete the OAuth login in the
   browser. Tokens are stored by Claude Code outside the repo, never in `.mcp.json`.
3. Run `/mcp` again to confirm the server shows as connected and to see the tools
   it exposes.

To reset a bad auth state, use `/mcp` and reconnect the server.

### Safety notes

- This server acts on a real brokerage account. Tool calls can move real money.
- Never commit tokens, account numbers, or exported positions to this repo.
- Prefer read-only exploration (quotes, positions, history) before anything that
  submits an order.

### Known limitation in remote sessions

Claude Code sessions running on the web or in a remote container reach the network
through an egress proxy. `agent.robinhood.com` is denied by that policy, so the
server will fail to connect there. Use a local Claude Code session for anything
that talks to Robinhood.
