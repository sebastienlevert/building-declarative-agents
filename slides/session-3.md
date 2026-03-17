---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
name: "Sébastien Levert"
---

# Building Declarative Agents

## Session 3: Grounding Your Agent with Knowledge — SharePoint, Teams, Web, Email & People

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# What Is Grounding?

Grounding gives an AI model access to **specific, authoritative data** so responses are based on facts — not general training knowledge.

&nbsp;

| Without Grounding | With Grounding |
|:---|:---|
| Well-instructed chatbot | Knowledge worker |
| Guesses from training data | Retrieves actual documents |
| Generic answers | Cited, specific answers |

&nbsp;

> You don't build a RAG pipeline. You don't manage embeddings. You say **"look here"** and the platform does the rest.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Six Knowledge Sources

&nbsp;

| Capability | What It Grounds On | Example |
|:---|:---|:---|
| 📄 **OneDriveAndSharePoint** | Document libraries, sites, files | Employee Handbook, Benefits Guide |
| 💬 **TeamsMessages** | Channel conversations, replies | New Hires 2026 tips & questions |
| 🌐 **WebSearch** | Scoped external websites | Zava Insurance careers & culture page |
| 📧 **Email** | User's Outlook mailbox | Onboarding emails, welcome messages |
| 👥 **People** | Microsoft 365 profiles & org chart | Manager lookup, team structures |
| 🔌 **GraphConnectors** | External data indexed into M365 | HRIS, internal wikis, custom KBs |

&nbsp;

All configured in the manifest — no infrastructure, no APIs, no pipelines.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: SharePoint & Teams

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# SharePoint — Your Document Library

```json
{
  "name": "OneDriveAndSharePoint",
  "items_by_url": [
    { "url": "https://zavainsurance.sharepoint.com/sites/HR/..." }
  ]
}
```

&nbsp;

- Point at a **site**, a **library**, or a **specific file**
- Add **multiple URLs** for different sources
- Respects **SharePoint permissions** — if a user can't access a doc, the agent won't surface it

&nbsp;

> When someone asks *"How many PTO days do I get?"*, the agent pulls from the Employee Handbook — with a citation.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Teams Messages — Institutional Knowledge

```json
{
  "name": "TeamsMessages",
  "urls": [
    { "url": "https://teams.microsoft.com/l/channel/19%3A...@thread.tacv2/..." }
  ]
}
```

&nbsp;

Captures the **tribal wisdom** that never gets documented:

- *"The compliance training portal works best in Edge"*
- *"Block out 2 hours — it doesn't save progress if you close the tab"*

&nbsp;

> The agent blends **official policy** with **peer-validated advice** — and cites both sources.

---
layout: section
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Demo: Web, Email & People

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Scoped Web Search

```json
{
  "name": "WebSearch",
  "sites": [
    { "url": "https://zava-insurance.com" }
  ]
}
```

&nbsp;

Not general web search — **scoped to specific sites**.

The agent answers *"What's the culture like at Zava Insurance?"* from the public careers page — content that isn't in any internal doc.

&nbsp;

> Richness of web content. None of the risk of the agent wandering off to random websites.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Email & People — Personal Context

Two capabilities that make the agent **personally relevant**:

&nbsp;

| 📧 Email | 👥 People |
|:---|:---|
| Searches the user's **Outlook mailbox** | Queries **Microsoft 365 profiles** & org data |
| Finds onboarding emails, welcome messages, action items | Looks up managers, team members, department contacts |
| *"Did I get a welcome email from IT?"* | *"Who's my skip-level manager?"* |

&nbsp;

```json
{ "name": "Email" },
{ "name": "People" }
```

&nbsp;

> No URLs needed — these capabilities tap into the user's own M365 data with their permissions.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# The Complete Capabilities Picture

Our manifest now has **seven** capabilities working together:

&nbsp;

| From Session 2 | New in Session 3 |
|:---|:---|
| 📦 Embedded Knowledge | 📄 SharePoint & OneDrive |
| 🧮 Code Interpreter | 💬 Teams Messages |
| 🎨 Image Generation | 🌐 Scoped Web Search |
| | 📧 Email |
| | 👥 People |

&nbsp;

> The agent decides **which source to query** based on the user's question. You configure it. Copilot orchestrates it.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Recap & What's Next

Today we expanded the agent's knowledge from **3 embedded files** to **the full breadth of organizational content**:

- 📄 **SharePoint** — official policies with citations and access control
- 💬 **Teams** — peer wisdom and informal tips
- 🌐 **Web** — public culture and careers content
- 📧 **Email** — personal onboarding communications
- 👥 **People** — org chart and colleague profiles

No RAG pipeline. No embeddings. Just manifest configuration.

&nbsp;

**Next session** — we add **actions**. API plugins for real-time data and MCP servers for write operations. The agent goes from read-only to interactive. 🚀

---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Try it now! 🚀

[github.com/sebastienlevert/building-declarative-agents](https://github.com/sebastienlevert/building-declarative-agents)
