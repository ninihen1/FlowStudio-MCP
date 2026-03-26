# Flow Studio MCP

AI agent tools to read, debug, build, and monitor Microsoft Power Automate cloud flows.

**Endpoint:** `https://mcp.flowstudio.app/mcp`
**Auth:** API Key (`x-api-key` header)
**Website:** [mcp.flowstudio.app](https://mcp.flowstudio.app)

Works with Copilot, Claude, and any MCP-compatible agent.

---

## Tools

### Environments & Connections

| Tool | Description |
|------|-------------|
| `list_live_environments` | List all Power Platform environments visible to the service account |
| `list_live_connections` | List connections in an environment (id, connector, status, expiration) |
| `list_live_flows` | List flows in an environment (id, displayName, state, trigger type) |

### Flow Management

| Tool | Description |
|------|-------------|
| `get_live_flow` | Fetch full flow definition JSON (triggers, actions, parameters, outputs) |
| `get_live_flow_trigger_url` | Get the live trigger URL and method for an HTTP-triggered flow |
| `get_live_flow_http_schema` | Inspect request/response JSON schemas for a Request-triggered flow |
| `set_live_flow_state` | Start or stop a flow |
| `update_live_flow` | Update an existing flow definition, or create a new flow |
| `add_live_flow_to_solution` | Migrate a non-solution flow into a solution |

### Run History & Debugging

| Tool | Description |
|------|-------------|
| `get_live_flow_runs` | Fetch run history (status, startTime, endTime, trigger, errors) |
| `get_live_flow_run_error` | List every failed action in a run with error code and message |
| `get_live_flow_run_action_outputs` | Download action inputs/outputs, including foreach iteration details |
| `trigger_live_flow` | POST to an HTTP-triggered flow's callback URL |
| `resubmit_live_flow_run` | Resubmit a failed/cancelled run with its original trigger payload |
| `cancel_live_flow_run` | Cancel a currently running flow run |

### Power Clarity Store (Pro+ plan)

Cached analytics and governance layer over your Power Platform tenant.

| Tool | Description |
|------|-------------|
| `list_store_environments` | List environments from cache |
| `list_store_connections` | List connections from cache |
| `list_store_flows` | List flows with governance filters (monitor, notification rules) |
| `list_store_makers` | List citizen developers / AAD users |
| `list_store_power_apps` | List Power Apps canvas apps |
| `get_store_flow` | Full flow details with run statistics and governance metadata |
| `get_store_flow_runs` | Cached run history with failed actions and remediation hints |
| `get_store_flow_errors` | Cached failed runs only (convenience filter) |
| `get_store_flow_summary` | Aggregated stats: success/fail rate, average/max duration |
| `get_store_flow_trigger_url` | Trigger URL from cache |
| `get_store_maker` | Maker details (flow/app counts, deleted status) |
| `update_store_flow` | Update governance metadata (tags, owner, monitoring, notifications) |
| `set_store_flow_state` | Start/stop a flow and persist state to cache |

---

## Connect

### Claude Code

```bash
claude mcp add --transport http flowstudio https://mcp.flowstudio.app/mcp \
  --header "x-api-key: YOUR_API_KEY"
```

### VS Code

Add to your VS Code `settings.json`:

```json
{
  "mcp": {
    "servers": {
      "flowstudio": {
        "type": "http",
        "url": "https://mcp.flowstudio.app/mcp",
        "headers": {
          "x-api-key": "YOUR_API_KEY"
        }
      }
    }
  }
}
```

### Cursor

Add to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "flowstudio": {
      "url": "https://mcp.flowstudio.app/mcp",
      "headers": {
        "x-api-key": "YOUR_API_KEY"
      }
    }
  }
}
```

### Windsurf

```json
{
  "mcpServers": {
    "flowstudio": {
      "serverUrl": "https://mcp.flowstudio.app/mcp",
      "headers": {
        "x-api-key": "YOUR_API_KEY"
      }
    }
  }
}
```

### OpenAI Codex

In `~/.codex/config.toml`:

```toml
[mcp_servers.flowstudio]
url = "https://mcp.flowstudio.app/mcp"

[mcp_servers.flowstudio.http_headers]
x-api-key = "YOUR_API_KEY"
```

---

## Agent Skills

Pre-built skill guides for agents that support the Agent Skills standard (SKILL.md):

| Skill | What it does |
|-------|-------------|
| [power-automate-mcp](https://github.com/ninihen1/power-automate-mcp-skills/tree/master/skills/power-automate-mcp) | Core MCP connection and flow CRUD operations |
| [power-automate-debug](https://github.com/ninihen1/power-automate-mcp-skills/tree/master/skills/power-automate-debug) | Step-by-step debugging workflow for failed flow runs |
| [power-automate-build](https://github.com/ninihen1/power-automate-mcp-skills/tree/master/skills/power-automate-build) | Build and deploy flows from natural language |

Available on [GitHub](https://github.com/ninihen1/power-automate-mcp-skills), [Smithery](https://smithery.ai/skills/flowstudio/power-automate-mcp), and [ClawHub](https://clawhub.ai).

---

## Links

- [Flow Studio MCP](https://mcp.flowstudio.app) - Subscription and API key management
- [Skills repo](https://github.com/ninihen1/power-automate-mcp-skills) - Agent skill definitions and references
- [Smithery](https://smithery.ai/servers/flowstudio/flowstudio-mcp) - MCP server listing
