# FlowStudio MCP

AI agent tools to read, debug, build, and monitor Microsoft Power Automate cloud flows, and to read, edit, and publish Power Apps canvas apps (preview).

**Endpoint:** `https://mcp.flowstudio.app/mcp`
**Auth:** API key (`x-api-key` header; `Authorization: Bearer <key>` also works), or Microsoft sign-in at `https://mcp.flowstudio.app/mcp/oauth` in apps that support MCP sign-in
**Website:** [mcp.flowstudio.app](https://mcp.flowstudio.app)

Works with Copilot, Claude, and any MCP-compatible agent.

---

## Tools

### Environments, Connections & Data

| Tool | Description |
|------|-------------|
| `list_live_environments` | List the Power Platform environments you can see |
| `list_live_connections` | List connections in an environment, with status and paste-ready templates |
| `create_live_connection` | Create a connection, or sign one in again |
| `test_live_connection` | Test a connection |
| `describe_live_connector` | Describe a connector's operations, inputs and outputs |
| `resolve_live_parameters` | Get an action's dropdown options and list or table columns |
| `read_live_table` | Read rows from a Dataverse table, SharePoint list, SQL table or Excel table |

### Flow Management

| Tool | Description |
|------|-------------|
| `list_live_flows` | List flows in an environment |
| `get_live_flow` | Fetch a flow definition; `includeTriggerUrl: true` adds the HTTP trigger URL |
| `get_live_flow_versions` | List a flow's earlier versions |
| `diff_live_flow` | Compare two versions of a flow |
| `update_live_flow` | Create or update a flow (`dryRun: true` previews without saving) |
| `set_live_flow_state` | Start or stop a flow |
| `delete_live_flow` | Delete a flow; refused while your Delete Lock is on (the default) |
| `restore_live_flow` | Restore a deleted flow |
| `add_to_solution` | Add a flow or canvas app to a solution |

### Run History & Debugging

| Tool | Description |
|------|-------------|
| `get_live_flow_runs` | Fetch run history, with status and date filters |
| `get_live_flow_run_error` | List every failed action in a run with its error code |
| `get_live_flow_run_action_outputs` | Read action inputs and outputs, including loop iterations |
| `trigger_live_flow` | Run an HTTP, Button, PowerApps, Skills or scheduled flow |
| `resubmit_live_flow_run` | Replay a run with its original trigger data |
| `cancel_live_flow_run` | Cancel a running flow run |

### Power Apps Canvas Apps (preview)

| Tool | Description |
|------|-------------|
| `list_canvas_apps` | List canvas apps in an environment |
| `get_canvas_app` | Open an app for reading: screens, controls and data sources |
| `list_canvas_controls` | List an app's screens and controls |
| `describe_canvas_control` | Read a control's properties and formulas |
| `get_canvas_app_sources` | List the app's data sources and their columns |
| `check_canvas_formula` | Check Power Fx formulas |
| `score_canvas_app` | Score an app's quality |
| `get_canvas_app_files` | List or read the files in the app package |
| `list_canvas_app_versions` | List versions, draft and live |
| `diff_canvas_app` | Compare two versions of an app |
| `create_canvas_app` | Create an app, or copy one |
| `update_canvas_app` | Stage edits and push them as a draft |
| `update_canvas_app_files` | Create, update or delete files in the app package |
| `discard_canvas_app_session` | Drop staged edits |
| `publish_canvas_app` | Publish the newest version to the app's users |

### Helpers

`tool_search` and `list_skills` find the right tools, `get_task` follows a long save to the end, `report_bug` sends a bug report, and `grant_flow_studio_access` shows a sign-in card when a tool needs one more grant.

### Cached Store (MCP Pro+)

Cached analytics and governance data for your tenant. Not available in Microsoft 365 Copilot (Copilot Chat or Cowork), even on Pro+.

| Tool | Description |
|------|-------------|
| `list_store_environments` | List environments from the cache |
| `list_store_connections` | List connections from the cache |
| `list_store_flows` | List flows with governance filters (monitoring, notification rules) |
| `list_store_makers` | List makers |
| `list_store_power_apps` | List Power Apps canvas apps |
| `get_store_flow` | Flow details with run statistics and governance metadata |
| `get_store_flow_runs` | Cached run history with failed actions and remediation hints |
| `get_store_flow_summary` | Success and failure rates, average and maximum duration |
| `get_store_maker` | Maker details |
| `update_store_flow` | Update governance metadata (tags, owner, monitoring, notifications) |

---

## Connect

For full setup instructions, see the [Getting Started guide](https://learn.flowstudio.app/mcp-getting-started).

### Sign in instead of using a key

In ChatGPT (Developer mode on), claude.ai, or another app that supports MCP sign-in, add `https://mcp.flowstudio.app/mcp/oauth` as a connector and sign in with your Microsoft account.

### Claude Desktop

File: `%APPDATA%\Claude\claude_desktop_config.json` (Windows) or `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS)

```json
{
  "mcpServers": {
    "flowstudio": {
      "command": "npx",
      "args": [
        "mcp-remote@latest",
        "https://mcp.flowstudio.app/mcp",
        "--header",
        "x-api-key: YOUR_API_KEY",
        "--header",
        "User-Agent: FlowStudio-MCP/1.0"
      ]
    }
  }
}
```

Requires Node.js and the `mcp-remote` package as a bridge.

### Claude Code

Add to `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "flowstudio": {
      "type": "http",
      "url": "https://mcp.flowstudio.app/mcp",
      "headers": {
        "x-api-key": "${FLOWSTUDIO_API_KEY}"
      }
    }
  }
}
```

Or via CLI:

```bash
claude mcp add --transport http flowstudio https://mcp.flowstudio.app/mcp \
  --header "x-api-key: $FLOWSTUDIO_API_KEY"
```

### GitHub Copilot (VS Code)

Add `.vscode/mcp.json` to your project (and add it to `.gitignore`):

```json
{
  "servers": {
    "flowstudio": {
      "url": "https://mcp.flowstudio.app/mcp",
      "headers": {
        "x-api-key": "YOUR_API_KEY"
      }
    }
  }
}
```

### Copilot Studio

See [Microsoft documentation](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agent-extend-mcp-overview) for adding remote MCP servers to Copilot Studio agents.

### JetBrains IDEs (IntelliJ, Rider, WebStorm)

Via **Settings > Tools > AI Assistant > Model Context Protocol (MCP)**:

```json
{
  "mcpServers": {
    "flowstudio": {
      "command": "npx",
      "args": [
        "mcp-remote@latest",
        "https://mcp.flowstudio.app/mcp",
        "--header",
        "x-api-key: YOUR_API_KEY"
      ]
    }
  }
}
```

Requires Node.js and `mcp-remote` wrapper.

### OpenAI Codex CLI

In `~/.codex/config.toml`:

```toml
[mcp_servers.flowstudio]
url = "https://mcp.flowstudio.app/mcp"
startup_timeout_sec = 10
tool_timeout_sec = 120

[mcp_servers.flowstudio.env_http_headers]
x-api-key = "FLOWSTUDIO_API_KEY"
```

### Any MCP-Compatible Agent

**Endpoint:** `https://mcp.flowstudio.app/mcp`
**Protocol:** JSON-RPC 2.0 over HTTP POST
**Required headers:**

```
Content-Type: application/json
x-api-key: YOUR_API_KEY
User-Agent: FlowStudio-MCP/1.0
```

`Authorization: Bearer YOUR_API_KEY` works in place of `x-api-key`.

---

## Agent Skills

Skill guides for agents that support the Agent Skills standard (SKILL.md):

| Skill | What it does |
|-------|-------------|
| [power-automate-mcp](https://github.com/ninihen1/power-automate-mcp-skills/tree/main/skills/power-automate-mcp) | Connection, tool discovery and response handling |
| [power-automate-debug](https://github.com/ninihen1/power-automate-mcp-skills/tree/main/skills/power-automate-debug) | Step-by-step debugging for failed flow runs |
| [power-automate-build](https://github.com/ninihen1/power-automate-mcp-skills/tree/main/skills/power-automate-build) | Build and deploy flows from natural language |
| [power-automate-monitoring](https://github.com/ninihen1/power-automate-mcp-skills/tree/main/skills/power-automate-monitoring) | Tenant-wide flow health (Pro+) |
| [power-automate-governance](https://github.com/ninihen1/power-automate-mcp-skills/tree/main/skills/power-automate-governance) | Tag, audit and classify flows (Pro+) |
| [power-apps-canvas-mcp](https://github.com/ninihen1/power-automate-mcp-skills/tree/main/skills/power-apps-canvas-mcp) | Ground rules for the canvas tools (preview) |
| [power-apps-canvas-read](https://github.com/ninihen1/power-automate-mcp-skills/tree/main/skills/power-apps-canvas-read) | Inspect, audit and compare canvas apps |
| [power-apps-canvas-edit](https://github.com/ninihen1/power-automate-mcp-skills/tree/main/skills/power-apps-canvas-edit) | Change screens, controls, formulas and data sources |
| [power-apps-canvas-ship](https://github.com/ninihen1/power-automate-mcp-skills/tree/main/skills/power-apps-canvas-ship) | Push and publish canvas changes safely |

Available on [GitHub](https://github.com/ninihen1/power-automate-mcp-skills), [awesome-copilot](https://github.com/github/awesome-copilot) and [ClawHub](https://clawhub.ai).

---

## Links

- [FlowStudio MCP](https://mcp.flowstudio.app) - Subscription and API key management
- [Skills repo](https://github.com/ninihen1/power-automate-mcp-skills) - Agent skill definitions and references
