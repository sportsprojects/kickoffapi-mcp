# KickoffAPI MCP Server

Model Context Protocol (MCP) server for [KickoffAPI](https://kickoffapi.com) — live football scores, fixtures, standings, projections, odds and player stats for AI agents, Claude Desktop, Cursor and any MCP client.

**Endpoint (remote, SSE):** `https://api.kickoffapi.com/mcp/sse`
**Auth:** `x-api-key` header with your KickoffAPI key — [free tier, no card](https://kickoffapi.com/pricing.html) (100 requests/day).

## Tools

| Tool | Description |
|---|---|
| `get_live_scores` | All currently live matches with real-time scores, minute, status and events. Optional league filter. |
| `get_fixtures` | Fixtures, schedules and past results — filter by league, team, season, date range or status. |
| `get_fixture_details` | Lineups, goals, cards, substitutions and match statistics for one fixture. |
| `get_standings` | League standings tables. |
| `get_projections` | KickoffAPI xG-based match projections. |
| `get_team_stats` | Team-level statistics. |
| `get_player_stats` | Player-level statistics. |
| `get_odds` | Pre-match odds. |
| `search_teams_and_leagues` | Resolve team/league IDs for the other tools. |
| `get_season_sim` | Season simulation. |

## Connect from Claude Desktop / Cursor (SSE remote)

```json
{
  "mcpServers": {
    "kickoffapi": {
      "url": "https://api.kickoffapi.com/mcp/sse",
      "headers": { "x-api-key": "YOUR_KICKOFFAPI_KEY" }
    }
  }
}
```

## Python example

```python
# pip install mcp
import asyncio
from mcp import ClientSession
from mcp.client.sse import sse_client

async def main():
    async with sse_client(
        "https://api.kickoffapi.com/mcp/sse",
        headers={"x-api-key": "YOUR_KICKOFFAPI_KEY"},
    ) as (read, write):
        async with ClientSession(read, write) as session:
            await session.initialize()
            tools = await session.list_tools()
            print([t.name for t in tools.tools])
            result = await session.call_tool("get_live_scores", {})
            print(result.content)

asyncio.run(main())
```

## Registry metadata

[`server.json`](server.json) follows the official MCP Registry schema (`com.kickoffapi/mcp`). Domain ownership is proven via the well-known record at [kickoffapi.com/.well-known/mcp-registry-auth](https://kickoffapi.com/.well-known/mcp-registry-auth).

## Links

- API docs: https://docs.kickoffapi.com/
- OpenAPI v2 spec: https://kickoffapi.com/openapi-v2.yaml
- MCP reference page: https://kickoffapi.com/docs/mcp.html
- Pricing (free tier): https://kickoffapi.com/pricing.html
