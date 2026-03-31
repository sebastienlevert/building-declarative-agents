---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
name: "Sébastien Levert"
---

# Building Declarative Agents

## Session 4: Adding Actions — API Plugins & MCP Plugins

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# From Knowledge to Action

So far, our agent **reads**. Now it needs to **do**.

| Sessions 1–3 | Session 4 |
|:---|:---|
| Answer questions from static content | Call live APIs for real-time data |
| Cite documents | Create, update, and delete records |
| Read-only | Read **and** write |

> Actions turn the agent from a **knowledge assistant** into an **interactive workflow participant**.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Two Ways to Add Actions

| | 🔌 API Plugin | 🛰️ MCP Plugin |
|:---|:---|:---|
| **Runtime** | `OpenApi` | `RemoteMCPServer` |
| **Spec format** | OpenAPI YAML/JSON | MCP tool descriptions |
| **Best for** | Existing REST APIs | New tooling with rich rendering |
| **Rendering** | Adaptive Cards (file-based) | Adaptive Cards (inline) or UI widgets |

Both are configured in the **manifest** — Copilot handles orchestration.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# API Plugins

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# API Plugins — The Concept

An API plugin connects your agent to a **REST API** via an **OpenAPI spec**.

You provide three things:

1. **OpenAPI spec** — endpoints, parameters, and responses
2. **Plugin manifest** — maps operations to named functions
3. **Adaptive Cards** — templates that render responses as rich cards

> The agent reads function descriptions, decides when to call them, and renders results with your card templates.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Anatomy of an API Plugin

```
appPackage/
├── repairs-plugin.json        ← Plugin manifest
├── apiSpecificationFile/
│   └── openapi.yaml           ← OpenAPI spec
└── adaptiveCards/
    ├── repairs_get.json       ← Card for listing repairs
    └── repairs_item_get.json  ← Card for a single repair
```

The **plugin manifest** is the glue — it references the spec and declares which operations the agent can use.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Plugin Manifest

```json
{
  "schema_version": "v2.3",
  "name_for_human": "Repairs API",
  "functions": [
    { "name": "repairs_get", "description": "Get all repairs" },
    { "name": "repairs_post", "description": "Create a new repair" },
    { "name": "repairs_item_patch", "description": "Update a repair by ID" }
  ],
  "runtimes": [{
    "type": "OpenApi",
    "spec": { "url": "apiSpecificationFile/openapi.yaml" },
    "run_for_functions": ["repairs_get", "repairs_post", "..."]
  }]
}
```

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Wiring It Into the Agent

In `declarativeAgent.json`, add an **actions** array:

```json
{
  "capabilities": [ "..." ],
  "actions": [
    { "id": "repairsActions", "file": "repairs-plugin.json" }
  ]
}
```

- Each action references a plugin manifest file
- The agent picks up all declared functions
- **No code changes** — just JSON configuration

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Repairs API Plugin

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Adaptive Cards — Rich Responses

API responses render as **Adaptive Cards** — structured UI inside the chat.

```json
{
  "type": "AdaptiveCard",
  "body": [
    { "type": "TextBlock", "text": "${title}", "weight": "bolder" },
    { "type": "FactSet", "facts": [
      { "title": "Assigned To", "value": "${assignedTo}" },
      { "title": "Date", "value": "${date}" }
    ]}
  ]
}
```

> `${field}` placeholders are populated from the API response. One card template per function.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# MCP Plugins

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# MCP Plugins — The Concept

**Model Context Protocol (MCP)** is an open standard for connecting AI models to external tools.

Instead of OpenAPI, you run an **MCP server** that exposes **tools** the agent can call.

| API Plugin | MCP Plugin |
|:---|:---|
| You write an OpenAPI spec | The server **self-describes** its tools |
| Static spec file in the package | Live tool discovery at runtime |
| Adaptive Cards from separate files | Adaptive Cards inline **or** UI widgets |

> MCP is the emerging standard for tool integration across AI platforms.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The MCP Plugin Manifest

```json
{
  "schema_version": "v2.4",
  "name_for_human": "Contoso HR",
  "functions": [
    { "name": "lookup_pto_balance", "description": "Returns PTO balance..." },
    { "name": "search_hr_policies", "description": "Searches HR policies..." }
  ],
  "runtimes": [{
    "type": "RemoteMCPServer",
    "spec": {
      "url": "https://your-server.devtunnels.ms/mcp",
      "mcp_tool_description": { "tools": ["..."] }
    },
    "run_for_functions": ["lookup_pto_balance", "search_hr_policies"]
  }]
}
```

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# MCP Tool Descriptions

Each tool is fully self-describing with **input and output schemas**:

```json
{
  "name": "lookup_pto_balance",
  "title": "Lookup PTO Balance",
  "description": "Returns an employee's PTO balance...",
  "inputSchema": {
    "properties": {
      "employee_email": { "type": "string", "description": "Employee email" }
    },
    "required": ["employee_email"]
  },
  "outputSchema": { "..." }
}
```

> Rich schemas help Copilot understand **what to send** and **what to expect back**.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Inline Adaptive Cards in MCP

MCP plugins define Adaptive Cards **inline** — no separate card files:

```json
{
  "capabilities": {
    "response_semantics": {
      "data_path": "$",
      "static_template": {
        "type": "AdaptiveCard",
        "body": [
          { "type": "TextBlock", "text": "${name} — ${availableDays} days" },
          { "type": "FactSet", "facts": [
            { "title": "Used", "value": "${usedDays} days" },
            { "title": "Available", "value": "${availableDays} days" }
          ]}
        ]
      }
    }
  }
}
```

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: PTO & HR Policies MCP Plugin

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Multiple Actions, One Agent

Two plugins declared in the actions array:

```json
{
  "actions": [
    { "id": "repairsActions", "file": "repairs-plugin.json" },
    { "id": "ptoPlugin", "file": "pto-plugin.json" }
  ]
}
```

Copilot decides **which plugin to invoke** based on the user's question:

- *"Show me all active repairs"* → Repairs API plugin
- *"What's my PTO balance?"* → PTO MCP plugin
- *"Find the remote work policy"* → HR Policies MCP plugin

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# API vs. MCP — When to Use Which

| Scenario | Recommended |
|:---|:---|
| Existing REST API with an OpenAPI spec | 🔌 **API Plugin** |
| Rich rendering with UI widgets | 🛰️ **MCP Plugin** |
| Simplest possible setup | 🔌 **API Plugin** |
| Building new tooling from scratch | 🛰️ **MCP Plugin** |
| Cross-platform tool compatibility | 🛰️ **MCP Plugin** |

> They're not mutually exclusive — use both in the same agent.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Complete Agent — So Far

| Layer | What We've Added |
|:---|:---|
| 🧠 **Persona** (Session 1) | Instructions, guardrails, conversation starters |
| ⚡ **Capabilities** (Session 2) | Embedded Knowledge, Code Interpreter, Image Generation |
| 📚 **Knowledge** (Session 3) | SharePoint, Teams, Web, Email, People |
| 🔧 **Actions** (Session 4) | API Plugin (Repairs), MCP Plugin (PTO & HR Policies) |

> The agent can now **read**, **reason**, **create visuals**, and **take action** — all from JSON.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Recap & What's Next

Today we gave the agent the ability to **act** — not just answer:

- 🔌 **API Plugin** — Repairs REST API via OpenAPI spec
- 🛰️ **MCP Plugin** — PTO balance & HR policy search via MCP server
- 🎴 **Adaptive Cards** — rich responses for both plugin types
- 🧩 **Multiple Actions** — one agent, multiple plugins, automatic orchestration

**Next session** — we dive into **advanced agent configuration** — fine-tuning behavior, handling edge cases, and making the agent production-ready. See you next week! 🚀

---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Try it now! 🚀

[github.com/sebastienlevert/building-declarative-agents](https://github.com/sebastienlevert/building-declarative-agents)
