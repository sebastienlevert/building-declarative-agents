---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
name: "Sébastien Levert"
---

# Building Declarative Agents

## Session 7: Testing & Debugging — The Developer's Inner Loop

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Where We Are

| Session | What We Built |
|:---|:---|
| 🧠 **Session 1** | Persona, instructions, conversation starters |
| ⚡ **Session 2** | Embedded Knowledge, Code Interpreter, Image Generation |
| 📚 **Session 3** | SharePoint, Teams, Web, Email, People |
| 🔧 **Session 4** | API Plugins, MCP Plugins |
| 🎛️ **Session 5** | Disclaimers, behavior overrides, env vars, lifecycle hooks |
| 🤝 **Session 6** | Connected Agents, full ecosystem orchestration |

Today we **debug** — dev tenant setup, developer mode, version management, environments, sharing, and API tracing.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Getting a Dev Tenant

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Setting Up Your Dev Tenant

Don't test on production — get a dedicated sandbox.

1. Join the [M365 Developer Program](https://developer.microsoft.com/microsoft-365/dev-program) → get an **E5 subscription**
2. Add the **Microsoft 365 Copilot** license (not included by default)
3. Enable **custom app upload** and **sideloading**

> Visual Studio subscribers, ISV Success members, and Microsoft Partners all qualify.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Developer Mode

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Developer Mode

Type `-developer on` in any Copilot Chat to get a **debug card** with every response.

| Section | What You Learn |
|:---|:---|
| 🏷️ **Agent Metadata** | Agent ID, version, conversation ID, request ID |
| ⚙️ **Capabilities** | Which knowledge sources were queried, search text, result counts |
| 🔧 **Actions** | Matched vs. selected functions, request/response details, latency |

> No debug card? The orchestrator didn't use your agent for that prompt.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Developer Mode in Action

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Bumping Version Numbers

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Version Bumping & The Inner Loop

The `version` in `manifest.json` controls what gets deployed. **Always bump it.**

```
TEAMS_APP_VERSION=1.0.0   →   1.0.1 (instructions)
                           →   1.1.0 (new capability/plugin)
                           →   2.0.0 (major restructure)
```

Your fastest iteration cycle: **edit → bump version → F5 → `-developer on` → verify version → repeat**

> If the Agent Version in the debug card doesn't match, your changes aren't live.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Sharing Agents with Colleagues

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Sharing Agents

| Approach | Scope |
|:---|:---|
| `atk provision` (F5) | Personal — only you |
| `atk share` | Tenant — specific users or test groups |
| `atk publish` | Organization — admin catalog |

```bash
atk share --scope shared --emails colleague@zavainsurance.com --env dev
```

> Especially useful for testing OAuth — you need **multiple users** to validate consent flows.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Sharing an Agent

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Managing Multiple Environments

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Environment Files

ATK uses the `env/` folder — one `.env` file per environment:

| Environment | Tenant | Purpose |
|:---|:---|:---|
| `local` | Dev | F5 inner loop |
| `dev` | Dev | Shared testing |
| `staging` | Production | Pre-release validation |
| `prod` | Production | Org-wide deployment |

Each has its own App ID, tenant ID, SharePoint URLs, and `TEAMS_APP_VERSION`. The `.user` files hold secrets and are **never committed**.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Promotion & Switching Environments

```
Local (1.2.8)  →  Dev (1.2.7)  →  Staging (1.1.0)  →  Prod (1.0.4)
```

```bash
# Provision to a specific environment
atk provision --env dev
atk provision --env staging

# Create a new environment from an existing one
atk env add staging --env dev
```

> The version number is your **deployment receipt**. If it doesn't match in developer mode, your changes aren't live.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Local Debugging with Dev Tunnels

Copilot needs an internet-reachable URL — use **dev tunnels** to expose your localhost.

```bash
devtunnel create --allow-anonymous
devtunnel port create --port-number <port> --protocol https
devtunnel host
```

Then point your OpenAPI spec at the tunnel URL:

```yaml
servers:
  - url: ${{OPENAPI_SERVER_URL}}
```

```bash
# env/.env.dev.user (gitignored)
OPENAPI_SERVER_URL=https://<tunnel-id>-<port>.devtunnels.ms
```

> ATK handles this automatically with F5 — but for standalone APIs, dev tunnels give you the same inner loop.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Dev Tunnels & Debugging

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Troubleshooting & Reporting

| Symptom | Check |
|:---|:---|
| Agent responds generically | Are your capabilities executing? |
| Plugin never invoked | Is `descriptionForModel` semantically relevant? |
| Stale behavior after changes | Is the **Agent Version** current? |
| Plugin returns errors | Inspect Action Execution Details |
| OAuth consent loops | Test with shared scope — multiple users |

**Reporting:** 👍/👎 on the Copilot response + include `#extensibility` — IDs are captured automatically.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Complete Agent — Session 7

| Layer | What We've Built |
|:---|:---|
| 🧠 **Persona** (Session 1) | Instructions, guardrails, conversation starters |
| ⚡ **Capabilities** (Session 2) | Embedded Knowledge, Code Interpreter, Image Generation |
| 📚 **Knowledge** (Session 3) | SharePoint, Teams, Web, Email, People |
| 🔧 **Actions** (Session 4) | API Plugin (Repairs), MCP Plugin (Zava Claims) |
| 🎛️ **Advanced Config** (Session 5) | Disclaimers, behavior overrides, env vars, lifecycle hooks |
| 🤝 **Connected Agents** (Session 6) | IT Support + Benefits Advisor, full ecosystem orchestration |
| 🔍 **Testing & Debugging** (Session 7) | Dev tenant, developer mode, versioning, environments, sharing, API tracing |

> You can now build **and** debug with confidence.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Recap & What's Next

- 🏗️ Dev tenant + Copilot license
- 🔍 Developer mode — `-developer on`
- 🔢 Version bumping — always bump
- 🤝 Sharing — `atk share --scope shared`
- 🌍 Multiple environments — `.env` per tenant
- 📡 Dev tunnels — debug locally with internet-reachable URLs

**Next session** — **publishing and lifecycle management**. See you next week! 🚀

---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Try it now! 🚀

[github.com/sebastienlevert/building-declarative-agents](https://github.com/sebastienlevert/building-declarative-agents)
