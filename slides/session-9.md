---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
name: "Sébastien Levert"
---

# Building Declarative Agents

## Session 9: Building Agents with Coding Agents

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
| 🤝 **Session 6** | IT Support + Benefits Advisor, full ecosystem orchestration |
| 🔍 **Session 7** | Dev tenant, developer mode, versioning, environments, API tracing |
| 🚀 **Session 8** | Publish, CI/CD, admin governance, sharing |

Today we let a **coding agent** build our agents for us — powered by **Agents Toolkit Skills**.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# What Are Agents Toolkit Skills?

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Skills = Curated Knowledge for Coding Agents

Skills are knowledge modules from the [microsoft/work-iq](https://github.com/microsoft/work-iq) marketplace that plug into your coding agent. They give it **deep expertise** in the M365 Copilot extensibility surface — schemas, conventions, CLI commands, and deployment workflows.

Without skills, a coding agent **guesses** at what a declarative agent manifest should look like.

With skills, it **knows**.

> The Microsoft 365 Agents Toolkit plugin and its skills are currently in **public preview**.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Three Skills, Three Superpowers

| Skill | What It Does |
|:---|:---|
| 🤖 **declarative-agent-developer** | End-to-end agent lifecycle: scaffolding, manifest authoring, capabilities, API & MCP plugins, localization, and deployment |
| 🔧 **install-atk** | Install or update the Agents Toolkit CLI and VS Code extension |
| 🎨 **ui-widget-developer** | Build MCP servers with rich interactive widget rendering for Copilot Chat |

Each skill carries its own decision trees, validation rules, and **safety guardrails** — so the agent does the right thing and refuses to do the wrong thing.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Why Skills Matter

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Without Skills vs. With Skills

**Without skills** — 7 manual steps:

1. Read the docs to understand the manifest schema
2. Manually scaffold the project structure
3. Hand-author JSON manifests (hope you get the nesting right)
4. Look up CLI commands for adding plugins
5. Debug schema version mismatches
6. Figure out the deployment flags
7. Copy-paste the test URL

**With skills** — 3 prompts:

1. _"Scaffold a new agent for expense approvals"_
2. _"Add an API plugin for our finance API"_
3. _"Deploy it"_

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Safety Guardrails Built In

Skills enforce a **Detect, Inform, Ask** protocol:

- ✅ Won't silently create files in non-agent projects
- ✅ Won't deploy broken manifests
- ✅ Validates schema versions before generating config
- ✅ Refuses to deploy when errors exist
- ✅ Always produces a clickable test link after deployment

> You stay in control. The skill handles the plumbing; you focus on the scenario.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# The Workflow

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Coding Agent + Skills Loop

A tight, iterative cycle — each round takes **minutes, not hours**:

| Step | What Happens |
|:---|:---|
| 💬 **Prompt** | Describe what you want in natural language |
| 📂 **Scaffold** | The coding agent generates the project structure using skills |
| 📝 **Generate Manifest** | Writes `declarativeAgent.json` with correct schema |
| ✍️ **Write Instructions** | Crafts behavior instructions based on your scenario |
| 🔍 **Review** | You (or another AI skill) review the output |
| 🔄 **Iterate** | Refine the prompt, regenerate, repeat |

Because the coding agent retains context across the session, each iteration **builds on the last**.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Sweet Spot

Coding agents excel at generating **structured configuration files** — which is exactly what declarative agents are.

You're not asking the AI to write complex orchestration logic. You're asking it to produce:

- ✅ Well-formed JSON manifests
- ✅ Thoughtful, structured instructions
- ✅ Clean project scaffolding
- ✅ Valid OpenAPI specs
- ✅ Correct plugin wiring

> That's squarely in the sweet spot of what coding agents do best.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Getting Set Up

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Two Steps to Get Started

Works with **GitHub Copilot** (VS Code or CLI) or **Claude Code**.

### Step 1: Add the Work IQ Marketplace

```
/plugin marketplace add microsoft/work-iq
```

### Step 2: Install the Plugin

```
/plugin install microsoft-365-agents-toolkit@work-iq
```

Restart your coding agent, and you're ready. One-time setup.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Empty Folder to Deployed Agent

![Teams chat](assets/session-9-teams-chat.png)

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Key Scenarios Beyond Scaffolding

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# What Else Can Skills Do?

| Scenario | Prompt | What Happens |
|:---|:---|:---|
| 🌍 **Localize** | _"Localize my agent into French and Japanese"_ | Extracts strings, generates resource files, wires `localizationInfo` |
| 🔌 **Wire MCP** | _"Connect my agent to this MCP server"_ | Adds `RemoteMCPServer` plugin with URL validation |
| 🎨 **Build Widgets** | _"Build an MCP server that renders a dashboard widget"_ | Scaffolds MCP server with HTML widget renderer |
| 🚀 **Deploy** | _"Deploy my agent"_ | Runs `atk provision`, returns a clickable test link |
| 📦 **Publish** | _"Publish my agent to my organization"_ | Runs `atk publish`, submits to the tenant app catalog for admin approval |

> The skill knows the exact CLI commands, schema constraints, and version gates.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Complete Agent — All 9 Sessions

| Layer | What We've Built |
|:---|:---|
| 🧠 **Persona** (Session 1) | Instructions, guardrails, conversation starters |
| ⚡ **Capabilities** (Session 2) | Embedded Knowledge, Code Interpreter, Image Generation |
| 📚 **Knowledge** (Session 3) | SharePoint, Teams, Web, Email, People |
| 🔧 **Actions** (Session 4) | API Plugin (Repairs), MCP Plugin (Zava Claims) |
| 🎛️ **Advanced Config** (Session 5) | Disclaimers, behavior overrides, env vars, lifecycle hooks |
| 🤝 **Connected Agents** (Session 6) | IT Support + Benefits Advisor, full ecosystem orchestration |
| 🔍 **Testing & Debugging** (Session 7) | Dev tenant, developer mode, versioning, environments, API tracing |
| 🚀 **Shipping** (Session 8) | Publish, CI/CD, admin governance, sharing |
| 🤖 **Coding Agents** (Session 9) | Agents Toolkit Skills, prompt-to-deploy workflow, review loops |

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Recap

- 🧩 **Agents Toolkit Skills** — curated knowledge from Work IQ that turn coding agents into agent-building experts
- 🔄 **Prompt → Scaffold → Generate → Review → Deploy** — the full loop in minutes
- 🎯 **Prompt quality matters** — be specific about schemas, scenarios, and conventions
- 🔍 **Always review** — generate, review (technical + content + security), refine
- 🚀 **Three prompts to a deployed agent** — scaffold, configure, deploy

The gap between _"I have an idea"_ and _"I have a deployed agent"_ is now **minutes**.

---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Try it now! 🚀

[github.com/sebastienlevert/building-declarative-agents](https://github.com/sebastienlevert/building-declarative-agents)
