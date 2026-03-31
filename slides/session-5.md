---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
name: "Sébastien Levert"
---

# Building Declarative Agents

## Session 5: Advanced Manifest Capabilities

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

Today we **polish** — disclaimers, behavior overrides, environment variables, and lifecycle commands.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Disclaimers

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Adding a Disclaimer

```json
"disclaimer": {
  "text": "I'm an AI assistant — always verify critical HR information
           with your HR Business Partner."
}
```

Every user sees this when they open the agent:

- **Trust** — users know they're talking to AI; mistakes are possible
- **Compliance** — organizations require AI disclosure; built into the manifest

> It's part of the agent definition — no one can accidentally remove it.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Disclaimer in Action

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Behavior Overrides

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# What Are Behavior Overrides?

A top-level manifest object that changes how the agent behaves.

```json
"behavior_overrides": {
  "suggestions": { "disabled": false },
  "special_instructions": { "discourage_model_knowledge": true }
}
```

| Setting | Effect |
|:---|:---|
| `suggestions.disabled` | Toggle follow-up suggestions (e.g. *"How do I request PTO?"*) |
| `discourage_model_knowledge` | Rely only on grounding sources, not model training data |

> Suggestions reduce the blank-page problem. Discouraged model knowledge prevents hallucinated policies.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Discouraging Model Knowledge

| Without | With |
|:---|:---|
| *"What's the dress code?"* → generic policy from training data | *"I couldn't find a dress code policy. Check with HR."* |
| Sounds authoritative but **isn't your policy** | Honest about what it **doesn't know** |
| Blends training data with your documents | Relies **only** on your grounding sources |

> For an enterprise agent, **always** turn this on. Cite your docs or say "I don't know."

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Behavior Overrides

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Environment Variables

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Why Environment Variables?

Hardcoded URLs and settings create problems across environments.

The Agents Toolkit supports `${{VARIABLE_NAME}}` syntax — resolved at build time from `env/.env.{environment}` files.

```json
{
  "name": "OneDriveAndSharePoint",
  "items_by_url": [
    { "url": "${{ZAVA_ONBOARDING_DOCUMENTS_URL}}" }
  ]
}
```

- **Different SharePoint sites** per environment (dev, staging, production)
- **Different API endpoints** per tenant
- **Sensitive values** that shouldn't live in source control

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Environment Files

```
env/
├── .env.dev              ← Shared dev config (committed)
├── .env.dev.user         ← Personal secrets (git-ignored)
├── .env.local            ← Local config (committed)
└── .env.local.user       ← Local secrets (git-ignored)
```

| File | Committed? | Use Case |
|:---|:---|:---|
| `.env.dev` | ✅ Yes | Shared team values — URLs, app names, feature flags |
| `.env.dev.user` | ❌ No | Personal tokens, tenant-specific secrets |
| `.env.local` | ✅ Yes | Local development overrides |
| `.env.local.user` | ❌ No | Local secrets |

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Variables in Practice

```bash
# env/.env.dev
ZAVA_ONBOARDING_DOCUMENTS_URL=https://cxdev01.sharepoint.com/sites/HR/...
ZAVA_NEW_HIRE_TEAMS_CHANNEL_URL=https://teams.cloud.microsoft/l/channel/19%3A...
ZAVA_WEBSITE_URL=https://sebastienlevert.github.io/zava-insurance-website/
AGENT_SCOPE=shared
AGENT_DISCLAIMER=I'm an AI assistant — always verify critical HR information...
```

Every configurable string lives in environment files — **not** in the manifest JSON.

> Switching from dev to production? Change the environment. The manifest stays the same.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Environment Variables

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Lifecycle Commands

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The `m365agents.yml` Lifecycle

The Agents Toolkit defines **what happens** during provision, deploy, and publish.

```yaml
version: v1.11
provision:
  - uses: teamsApp/create
  - uses: teamsApp/zipAppPackage
  - uses: teamsApp/update
  - uses: teamsApp/extendToM365
publish:
  - uses: teamsApp/validateManifest
  - uses: teamsApp/zipAppPackage
  - uses: teamsApp/update
  - uses: teamsApp/publishAppPackage
```

Each step is a **built-in action** that the toolkit runs in order.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Custom Shell Commands

Inject **custom scripts** at any point in the lifecycle using `script`:

```yaml
provision:
  - uses: script
    name: validate-environment
    with:
      run: node ./scripts/validate-env.js
      shell: bash
  - uses: teamsApp/create
    ...
```

- ✅ Validate required env vars before deployment
- ✅ Generate dynamic configuration files
- ✅ Run pre/post-deployment health checks
- ✅ Sync data or seed test content

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Lifecycle Hooks — Pre & Post

```yaml
provision:
  - uses: script
    name: pre-provision-check
    with: { run: "node ./scripts/validate-env.js", shell: bash }
  - uses: teamsApp/create
  - uses: teamsApp/extendToM365
  - uses: script
    name: post-provision-log
    with: { run: "echo 'Provisioned ${{APP_NAME_SUFFIX}}'", shell: bash }
```

> Scripts have full access to environment variables via the `${{VAR}}` syntax.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Real-World Script Examples

| Hook | Script | Purpose |
|:---|:---|:---|
| Pre-provision | `validate-env.js` | Ensure all required env vars are set |
| Post-provision | `log-deployment.sh` | Record deployment metadata |
| Pre-publish | `run-manifest-lint.sh` | Lint the manifest before publishing |
| Post-publish | `notify-team.sh` | Send a Teams notification on publish |

```yaml
publish:
  - uses: script
    name: pre-publish-lint
    with:
      run: node ./scripts/lint-manifest.js
      shell: bash
  - uses: teamsApp/validateManifest
    ...
```

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Custom Lifecycle Commands

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Orchestration in Action

Five turns, five capabilities:

| Turn | Question | Capability |
|:---|:---|:---|
| 1 | *"I started yesterday — what should I focus on?"* | 🧠 Instructions + 📦 Embedded Knowledge |
| 2 | *"What's my actual onboarding progress?"* | 🔌 API Plugin |
| 3 | *"Compliance training gives me a 403. Create a ticket?"* | 🛰️ MCP Server |
| 4 | *"If I contribute 8% to my 401(k), what's my take-home on $95k?"* | 📄 SharePoint + 🧮 Code Interpreter |
| 5 | *"Make me a welcome graphic for the Teams channel"* | 🎨 Image Generation |

> Zero routing logic. `discourage_model_knowledge` ensures accuracy.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Multi-Source Orchestration

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Complete Agent — Session 5

| Layer | What We've Built |
|:---|:---|
| 🧠 **Persona** (Session 1) | Instructions, guardrails, conversation starters |
| ⚡ **Capabilities** (Session 2) | Embedded Knowledge, Code Interpreter, Image Generation |
| 📚 **Knowledge** (Session 3) | SharePoint, Teams, Web, Email, People |
| 🔧 **Actions** (Session 4) | API Plugin (Repairs), MCP Plugin (Zava Claims) |
| 🎛️ **Advanced Config** (Session 5) | Disclaimers, behavior overrides, env vars, lifecycle hooks |

> Production-ready — grounded, accurate, configurable, and deployable across environments.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Recap & What's Next

Today we polished the agent for production:

- 📋 **Disclaimers** — AI disclosure built into the manifest
- 🎯 **Behavior Overrides** — suggestions + discouraged model knowledge
- 🔑 **Environment Variables** — one manifest, multiple environments
- ⚙️ **Lifecycle Commands** — custom scripts for validation, logging, and automation

**Next session** — **connected agents** with `worker_agents`. Think microservices, but for AI. See you next week! 🚀

---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Try it now! 🚀

[github.com/sebastienlevert/building-declarative-agents](https://github.com/sebastienlevert/building-declarative-agents)
