---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
name: "Sébastien Levert"
---

# Building Declarative Agents

## Session 6: Connected Agents — Composing an Agent Ecosystem

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

Today we **compose** — connecting specialist agents into one seamless conversation.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# The Problem with Growing Scope

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# When One Agent Isn't Enough

As scope grows, quality degrades:

| Symptom | Cause |
|:---|:---|
| Agent reaches for the wrong tool | Too many capabilities competing |
| Shallow answers on deep topics | Instructions spread too thin |
| Generic troubleshooting steps | No access to specialized knowledge bases |
| Approximate cost comparisons | No dedicated calculation models |

> *"My laptop won't connect to the monitor"* deserves the full IT Knowledge Base — not just the IT Setup Guide summary.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Microservices Analogy

| Monolith | Microservices |
|:---|:---|
| One big agent with everything | Specialized agents composed together |
| Simple to manage, works at small scale | More setup, better quality at scale |
| Breaks at 8,000+ chars of instructions | Each agent stays focused |
| One team owns everything | Teams iterate independently |

**Start monolith. Split when quality degrades or another team already has an agent.**

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# The `worker_agents` Property

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Configuring Connected Agents

In schema v1.6, use the `worker_agents` property:

```json
"worker_agents": [
  {
    "id": "${{IT_SUPPORT_TITLE_ID}}"
  },
  {
    "id": "${{BENEFITS_ADVISOR_TITLE_ID}}"
  }
]
```

Each entry references the **title ID** of a deployed agent in your tenant.

- IDs come from provisioning or the developer mode metadata
- Use environment variables — different agents per environment

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# How Connected Agents Work

| Aspect | Behavior |
|:---|:---|
| **Same tenant or public** | Worker agents must be in the same M365 tenant **or** be publicly available agents |
| **Transparent delegation** | User sees one conversation — delegation is invisible |
| **Independent config** | Each agent has its own instructions, knowledge, plugins, and guardrails |
| **Permissions respected** | Each agent operates within the user's permission context |
| **Team ownership** | IT team maintains IT agent; HR team maintains Benefits agent |

> The user never has to pick which agent to talk to. The orchestration just works.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Building the Specialist Agents

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Multi-Agent Repo Structure

```
building-declarative-agents/
├── agents/
│   ├── hr-onboarding-buddy/     ← The agent we've been building
│   │   ├── appPackage/
│   │   ├── env/
│   │   └── m365agents.yml
│   ├── it-support/              ← NEW: IT troubleshooting specialist
│   │   ├── appPackage/
│   │   ├── env/
│   │   └── m365agents.yml
│   └── benefits-advisor/        ← NEW: Benefits & enrollment expert
│       ├── appPackage/
│       ├── env/
│       └── m365agents.yml
├── slides/
└── README.md
```

Each agent is a standalone project — provision and deploy independently.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# IT Support Agent

A specialist for deep troubleshooting — device issues, driver problems, network diagnostics.

```json
{
  "name": "IT Support Agent",
  "instructions": "$[file('instruction.txt')]",
  "capabilities": [
    { "name": "OneDriveAndSharePoint",
      "items_by_url": [{ "url": "${{IT_KNOWLEDGE_BASE_URL}}" }] },
    { "name": "CodeInterpreter" }
  ]
}
```

- Grounded in the **full IT Knowledge Base** (not just the Setup Guide summary)
- Known issues database, device compatibility matrices, resolution procedures
- Focused instructions for step-by-step troubleshooting

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Benefits Advisor Agent

A specialist for plan comparisons, cost modeling, and enrollment guidance.

```json
{
  "name": "Benefits Advisor",
  "instructions": "$[file('instruction.txt')]",
  "capabilities": [
    { "name": "OneDriveAndSharePoint",
      "items_by_url": [{ "url": "${{BENEFITS_DOCUMENTS_URL}}" }] },
    { "name": "CodeInterpreter" }
  ]
}
```

- Full plan documents (Bronze, Silver, Gold) with every detail
- **Code Interpreter** for precise cost modeling and take-home pay calculations
- Enrollment workflows and deadline awareness

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Delegation in Action

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# IT Delegation

User asks: *"My laptop won't connect to the external monitor — I've tried two different cables."*

The HR Buddy delegates to the **IT Support Agent**, which returns:

> *"This is a known issue with the Dell WD22TB4 Thunderbolt dock and the latest Windows 11 update. Here's the fix:*
> 1. *Open Device Manager → Display adapters*
> 2. *Right-click Intel Iris Xe → Update driver*
> 3. *Browse to `C:\Dell\Drivers\Display\31.0.101.4953`*
> 4. *Restart with the dock connected*

From the user's perspective, they asked the HR Buddy and got an expert answer.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Benefits Delegation

User asks: *"Can you explain the difference between Silver and Gold health plans? My spouse is self-employed and we're thinking about kids."*

The HR Buddy delegates to the **Benefits Advisor**, which returns:

| | Silver | Gold |
|---|--------|------|
| Monthly premium (Employee + Spouse) | $410 | $625 |
| Annual deductible | $3,000 | $1,000 |
| Maternity coverage | 80% after deductible | 90% after $500 copay |
| Estimated cost (moderate usage) | ~$7,920 | ~$9,100 |
| Estimated cost (pregnancy year) | ~$12,400 | ~$10,200 |

> Gold saves ~$2,000+ in a pregnancy year due to lower deductible and better maternity coverage.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Worker Agent Delegation

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# When to Use Connected Agents

| Scenario | Approach |
|:---|:---|
| Questions well-served by your documents | Add knowledge (SharePoint/Embedded) |
| Another team owns the expertise | Connect to their agent |
| Agent scope is too broad, quality degrading | Split into connected agents |
| Reuse a capability across multiple agents | Build a shared worker agent |
| Deep domain expertise with specialized tools | Connect to a specialist agent |

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Building for Reuse

Build **horizontal worker agents** that multiple teams can connect to:

- 🖥️ **Helpdesk Agent** — any department's agent can delegate to
- 💰 **Benefits Agent** — HR, Finance, and manager agents all use
- ⚖️ **Compliance Agent** — regulatory info across the organization
- 📊 **Analytics Agent** — data queries from any business unit

Each maintained by the expert team. Every agent in the org leverages their expertise.

> That's how you build an **agent ecosystem** — not one massive agent, but a network of specialists.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Complete Agent — Session 6

| Layer | What We've Built |
|:---|:---|
| 🧠 **Persona** (Session 1) | Instructions, guardrails, conversation starters |
| ⚡ **Capabilities** (Session 2) | Embedded Knowledge, Code Interpreter, Image Generation |
| 📚 **Knowledge** (Session 3) | SharePoint, Teams, Web, Email, People |
| 🔧 **Actions** (Session 4) | API Plugin (Repairs), MCP Plugin (Zava Claims) |
| 🎛️ **Advanced Config** (Session 5) | Disclaimers, behavior overrides, env vars, lifecycle hooks |
| 🤝 **Connected Agents** (Session 6) | IT Support + Benefits Advisor, full ecosystem orchestration |

> Feature-complete — grounded, accurate, configurable, composable, and production-ready.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Recap & What's Next

Today we composed our agent into an ecosystem:

- 🏗️ **Multi-agent repo structure** — each agent is its own project
- 🤝 **`worker_agents` property** — transparent delegation to specialists
- 🖥️ **IT Support Agent** — deep troubleshooting with full Knowledge Base
- 💰 **Benefits Advisor Agent** — detailed plan comparisons and cost modeling
- 🎯 **Full orchestration** — every capability from all six sessions in one conversation

**Next session** — **testing and debugging** with the Agents Playground. The session that'll make you 10x faster at building agents. See you next week! 🚀

---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Try it now! 🚀

[github.com/sebastienlevert/building-declarative-agents](https://github.com/sebastienlevert/building-declarative-agents)
