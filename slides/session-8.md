---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
name: "Sébastien Levert"
---

# Building Declarative Agents

## Session 8: Shipping — Publishing, CI/CD & Governance

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
| 🔍 **Session 7** | Dev tenant, developer mode, versioning, environments, sharing, API tracing |

Today we **ship** — publishing, CI/CD automation, admin governance, and broad sharing.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# The Publishing Journey

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Four Stages to Production

Publishing a declarative agent follows the standard Teams app lifecycle — your IT admins already know this workflow.

| Stage | Who | What Happens |
|:---|:---|:---|
| 🧑‍💻 **Developer Sideload** | You | F5 / sideloading — only you can see it |
| 🤝 **Developer Sharing** | You | Share with your team / stakeholders for testing before broader access |
| 📦 **Org Submission** | You | Submit to the tenant app catalog — pending until reviewed |
| ✅ **Admin Approval** | IT Admin | Review permissions, data access, then approve for users |

The key mental shift: once you submit, you're **handing control to the admin**. IT needs to understand what data the agent accesses and who can use it before it reaches employees.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# The `publish` Command

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Publishing from the CLI

The Agents Toolkit provides a `publish` command that packages and submits your agent in one step.

```bash
# Publish to your organization's app catalog
atk publish --env prod
```

This does two things:

1. **Packages** your agent — bundles manifest, icons, instructions, and plugins into a `.zip`
2. **Submits** the package to your tenant's app catalog as a pending app

You can also publish from the VS Code sidebar: **Agents Toolkit → Publish to Organization**.

> After publishing, the app appears in the Teams Admin Center under **Manage Apps** with a "Pending approval" status.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Publishing to the Org Catalog

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# CI/CD Automation

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Why Automate Publishing?

Manual publishing works for a single agent. But in a real organization:

- Multiple agents across teams
- Instruction changes need **PR review** before going live
- You want a **repeatable, auditable** deployment pipeline
- No human should need to click "Publish" in VS Code

The Agents Toolkit CLI supports **non-interactive authentication** via environment variables — designed for CI/CD pipelines.

```bash
export M365_ACCOUNT_NAME=admin@contoso.com
export M365_ACCOUNT_PASSWORD=********
atk publish --env prod
```

> Store credentials as **pipeline secrets** — never commit them to source control.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# GitHub Actions Pipeline

```yaml
name: Publish Declarative Agent
on:
  push:
    branches: [main]

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install Agents Toolkit
        run: npm install -g @microsoft/m365agentstoolkit-cli

      - name: Publish to org catalog
        run: atk publish --env prod
        env:
          M365_ACCOUNT_NAME: ${{ secrets.M365_ACCOUNT_NAME }}
          M365_ACCOUNT_PASSWORD: ${{ secrets.M365_ACCOUNT_PASSWORD }}
```

Every merge to `main` automatically publishes. Pair with **branch protection** and **PR reviews** for safe instruction changes.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Azure DevOps Pipeline

```yaml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: NodeTool@0
    inputs:
      versionSpec: '20.x'

  - script: npm install -g @microsoft/m365agentstoolkit-cli
    displayName: 'Install Agents Toolkit'

  - script: atk publish --env prod
    displayName: 'Publish to org catalog'
    env:
      M365_ACCOUNT_NAME: $(M365_ACCOUNT_NAME)
      M365_ACCOUNT_PASSWORD: $(M365_ACCOUNT_PASSWORD)
```

Store `M365_ACCOUNT_NAME` and `M365_ACCOUNT_PASSWORD` as **pipeline variables** (secret) or in Azure Key Vault.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# CI/CD Best Practices

| Practice | Why |
|:---|:---|
| 🔐 **Secrets in pipeline variables** | Never commit credentials to source control |
| 🔀 **Branch protection on `main`** | PR review required before instruction changes go live |
| 🏷️ **Bump version in PR** | Each publish should increment `TEAMS_APP_VERSION` |
| 🧪 **Run evals before publish** | Catch regressions in agent behavior before production |
| 🌍 **Environment promotion** | `dev` → `staging` → `prod` with separate `.env` files |

> Treat your agent like a microservice — version it, test it, promote it through environments.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: CI/CD Pipeline in Action

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Staged Apps & Admin Approval

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Admin Review Experience

When your agent lands in the Teams Admin Center, the admin sees a full review surface:

| Section | What the Admin Evaluates |
|:---|:---|
| 📋 **App Details** | Name, description, developer info |
| 🔑 **Permissions** | Graph scopes, API connections |
| 📂 **Data Access** | Which SharePoint sites the agent reads |
| 🤖 **Copilot Agent Details** | Declarative agent type, actions, capabilities |

This is where the admin makes a judgment call — are the permissions reasonable? Does the data access make sense for the agent's purpose?

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Controlling Access After Approval

Once approved, the admin decides **who** can use the agent:

| Access Model | Use Case |
|:---|:---|
| 👥 **Everyone** | Org-wide utility agents |
| 🎯 **Specific Entra ID Groups** | Targeted rollout — e.g., "New Hires 2026" |
| 🚫 **No One (Approved, Not Assigned)** | Staged deployment — approved but held back |

The "approved but not assigned" model is powerful for **staged rollouts**:

1. Publish and get admin approval
2. Hold in "No one" state while you validate in production
3. Gradually roll out to groups → eventually everyone

> Users discover approved agents in the **Microsoft 365 Copilot** app — browse or search by name.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Admin Approval & Staged Rollout

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Sharing Simple Agents Broadly

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The `share` Command — A Lightweight Alternative

Not every agent needs full admin approval. If your declarative agent **doesn't have actions** (no API plugins, no MCP servers), you can use `atk share` to distribute it directly.

```bash
# Share with specific colleagues
atk share --scope shared --emails colleague@contoso.com --env prod

# Share with the entire tenant
atk share --scope shared --env prod
```

| | `atk share` | `atk publish` |
|:---|:---|:---|
| **Audience** | Specific users or tenant | Org catalog (admin-gated) |
| **Admin approval** | Not required | Required |
| **Actions (API/MCP)** | ❌ Not supported | ✅ Supported |
| **Best for** | Knowledge-only agents, quick distribution | Full-featured agents with actions |

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# When to Use Share vs. Publish

Use **`atk share`** when your agent is:

- 📚 **Knowledge-only** — grounded in SharePoint, embedded files, or web content
- 🚀 **Needs fast distribution** — skip the admin approval queue
- 🧪 **In early feedback mode** — share with a test group before formal publishing
- 🏗️ **Pro-code but simple** — built with the Agents Toolkit, no actions needed

Use **`atk publish`** when your agent:

- 🔧 Has **API plugins or MCP servers** — actions require admin review
- 🏢 Needs **org-wide catalog presence** — users discover it in the Copilot store
- 🔐 Requires **formal governance** — audit trail, access control by Entra groups

> `share` is the fastest path to get a pro-code declarative agent into colleagues' hands.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Sharing a Knowledge-Only Agent

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Demo: The Employee Experience

Switching to a test account — Alex, a new employee at Zava Insurance.

Alex opens **Microsoft 365 Copilot** and sees the HR Onboarding Buddy:

1. 🛡️ **Disclaimer** — _"I'm an AI assistant — always verify critical HR info with your HR Business Partner."_
2. 💬 **Conversation starters** — _"What's my first-week checklist?"_, _"How do I set up my laptop?"_
3. ✨ **Grounded response** — pulls from SharePoint docs + live onboarding API

This is what 8 sessions of work looks like from the user's perspective — clean, helpful, and instant.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Real-World Patterns

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Pattern 1: Version Instructions Like Code

Your instructions file is the **most important artifact** in a declarative agent.

- 📂 Keep it in source control
- 🔀 Review changes in PRs
- 🧪 Test instruction changes before merging

> _"Never discuss compensation"_ vs. _"Avoid discussing compensation"_ produces **very** different results. Test every change.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Pattern 2: Start Narrow, Expand Gradually

Don't launch a "do everything" agent. Our HR Buddy started as a greeter and grew over 8 weeks.

The biggest failed agents tried to do **too much on day one**. Users got confused, the agent made mistakes, and the team lost confidence.

> Launch with instructions + knowledge → gather feedback → add actions.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Pattern 3: Monitor What Users Actually Ask

Once deployed, pay attention to what users ask that the agent **can't handle**. These gaps tell you what to build next.

> If 30% of conversations end with _"I can't help with that"_ — you've found your next sprint.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Pattern 4: MCP Servers Are Your Extensibility Strategy

Building multiple agents? MCP servers are **composable toolkits**.

A helpdesk MCP server built for the HR Buddy can be reused by an IT Support agent, a Facilities agent, or any agent that needs ticketing.

> Build tools once, compose everywhere.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Pattern 5: Know When NOT to Use a Declarative Agent

| Use Declarative Agents When | Consider Custom Engine Agents When |
|:---|:---|
| Knowledge retrieval + light actions | Full control over conversation flow |
| Rapid dev without custom bot infra | Real-time streaming or complex workflows |
| Deep M365 integration & governance | Channels outside Microsoft 365 |

> There's no wrong choice — just the right tool for the right job.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Complete Agent — All 8 Sessions

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

> From scaffold to production — defined entirely in JSON and text files. No bot framework. No custom hosting. **That's the promise of declarative agents.**

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Recap

- 📦 `atk publish` — submit to the org catalog
- 🔄 CI/CD with GitHub Actions & Azure DevOps — automate with `--username` / `--password`
- ✅ Staged apps — admin approval → Entra group targeting → gradual rollout
- 🤝 `atk share` — fast distribution for knowledge-only agents without actions
- 🏗️ Real-world patterns — version instructions, start narrow, monitor usage

**Next session** — **using coding agents to build declarative agents**. See you next week! 🚀

---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Try it now! 🚀

[github.com/sebastienlevert/building-declarative-agents](https://github.com/sebastienlevert/building-declarative-agents)
