---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
name: "Sébastien Levert"
---

# Building Declarative Agents

## Session 1: Your First Declarative Agent — From Scaffold to Persona

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Welcome to the Series! 👋

Over the next **8 weeks**, we're building a fully-featured declarative agent from scratch — piece by piece, session by session.

By the end, you'll have a production-ready **Zava Insurance HR Onboarding Buddy** that:

- Answers questions from company documents
- Calls APIs in real time
- Creates helpdesk tickets through an MCP server
- Is published to your entire organization

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# What Are Declarative Agents?

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Declarative Agents — The Concept

A declarative agent is an agent you **describe**, not one you **code**.

You write a JSON manifest that tells Microsoft 365 Copilot:

- **Who** this agent is
- **What** it knows
- **What** it can do

> Copilot handles the rest — the reasoning, the orchestration, the responses.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Agents "as a Service" 🍕

&nbsp;

| 🏠 Made at Home | 🛒 Take and Bake | 🛵 Delivery |
|:---|:---|:---|
| **Custom Engine Agents** | **Copilot Studio** | **Declarative Agents** |
| You make the dough, sauce, cheese | Pre-made pizza, you customize toppings | Describe what you want on the app |
| You own the oven, the kitchen | Bake it in your oven | Kitchen handles everything |
| Total control, total effort | Easy, but limited options | You get exactly what you ordered — fast |

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Scaffold the Project

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Scaffolded Project

Four files and we have a working agent:

| File | Purpose |
|------|---------|
| `manifest.json` | Teams app manifest — registers the agent with M365 |
| `declarativeAgent.json` | The heart of it — agent name, description, capabilities |
| `instruction.txt` | The agent's behavior instructions |
| `m365agents.yml` | Provisioning and deployment configuration |

**No server code. No complex framework. No Azure resources.**

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: First Deploy

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Baseline Agent

After the first deploy with **F5**:

- ✅ Agent is listed in Copilot
- ❌ No personality — responds generically
- ❌ No boundaries — will try to answer anything
- ❌ No conversation starters — blank experience

> Good enough to prove deployment works. But let's make it useful **right now**.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Why Instructions Are Everything

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Instructions = Agent Quality

You don't write code to control behavior — you write **instructions**.

Think of instructions like an onboarding document for a new employee:

❌ *"Help people with HR stuff"*

✅ *"You're the go-to person for new hires. You cover benefits, IT setup, and onboarding checklists. You don't handle payroll disputes — send those to the HR Business Partner. Be friendly but professional."*

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Writing Rich Instructions

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The 5-Part Instruction Framework

&nbsp;

| # | Part | Example |
|---|------|---------|
| 1 | **Role** | *"You are the Zava Insurance HR Onboarding Buddy..."* |
| 2 | **Tone & Personality** | *"Be warm, encouraging, and welcoming..."* |
| 3 | **Scope** | *Onboarding, benefits, IT setup, org info, support* |
| 4 | **Guardrails** | *"Never give legal advice. Never discuss compensation."* |
| 5 | **Response Guidelines** | *"Concise — 2 to 4 paragraphs. Bullet points. Cite sources."* |

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Conversation Starters

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Conversation Starters

Suggestion chips that appear when users open the agent — great for discoverability.

- 💬 *"What's my first-week checklist?"*
- 💬 *"Who do I contact for benefits questions?"*
- 💬 *"How do I set up my laptop and accounts?"*
- 💬 *"What's the PTO policy at Zava Insurance?"*
- 💬 *"I need help with my badge access"*

> Phrased as things a real new employee would say — not feature descriptions.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Before & After

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Before vs. After

&nbsp;

| ❌ Before (First Deploy) | ✅ After (With Instructions) |
|:---|:---|
| *"What's the PTO policy?"* | *"What's the PTO policy?"* |
| Vague, generic answer about typical PTO policies | Stays in character as the HR Buddy |
| Not wrong, but not useful | Acknowledges what it doesn't know |
| | Redirects appropriately |

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Guardrails in Action

Testing: *"What's the salary range for a Senior Engineer?"*

✅ Agent politely declines

✅ Directs to the HR Business Partner

✅ Stays in character

> The guardrails work. Night and day from where we started.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Recap & What's Next

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# What We Built Today

From **zero** to a deployed declarative agent with a real identity:

- ✅ Scaffolded the project with the Agents Toolkit
- ✅ Understood the 4-file structure
- ✅ Deployed to Copilot with F5
- ✅ Applied the 5-part instruction framework
- ✅ Added meaningful conversation starters
- ✅ Tested guardrails and persona behavior

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Next Session — Agent Capabilities

The agent runs purely on instructions today. It has **no access to real company data**.

**Next session** we fix that:

- 📦 **Embedded Knowledge** — Bundle real Zava Insurance documents in the app package
- 🧮 **Code Interpreter** — Enable precise calculations
- 🎨 **Image Generation** — Add visual content creation

See you next week! 🚀
