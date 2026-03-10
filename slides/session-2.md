---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
name: "Sébastien Levert"
---

# Building Declarative Agents

## Session 2: Configuring Agent Capabilities — Embedded Knowledge, Code Interpreter & Image Generation

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# What Are Capabilities?

In the declarative agent manifest, `capabilities` is an array that tells Copilot what your agent can do **beyond basic chat**.

Think of it as a **feature menu** — you enable what you need and leave the rest off.

&nbsp;

> **Intentional configuration** — every capability you add is a capability the agent might use when you don't expect it. Be deliberate.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Three Built-in Capabilities

&nbsp;

| Capability | What It Does | When to Use |
|:---|:---|:---|
| 📦 **Embedded Knowledge** | Bundle documents (Word, PDF, Excel…) directly in the app package | Ground responses in real company content — no cloud setup needed |
| 🧮 **Code Interpreter** | Write & execute Python on the fly | Precise calculations, data analysis, charts & visualizations |
| 🎨 **Image Generation** | Create images via image models | Welcome cards, creative visuals, illustrative content |

&nbsp;

All three are configured **entirely in the manifest** — no external services, no APIs, no infrastructure.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Embedded Knowledge — Key Decisions

**Constraints**: Max **10 files**, **1 MB each** — supports `.docx`, `.pptx`, `.xlsx`, `.pdf`, `.txt`

&nbsp;

| Scenario | Use |
|:---|:---|
| A few stable documents (< 10 files, < 1 MB each) | **Embedded Knowledge** |
| Large or frequently updated document libraries | **SharePoint** |
| Documents requiring per-user access control | **SharePoint** |
| Simplest possible setup & maximum portability | **Embedded Knowledge** |

&nbsp;

> When you share an agent, you also share its embedded knowledge — no "make sure you have access to this SharePoint site" setup.

---
layout: default
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: slideRight
---

# Recap & What's Next

Today we configured three built-in capabilities — all in the manifest:

- 📦 **Embedded Knowledge** — ground the agent in real documents shipped in the app package
- 🧮 **Code Interpreter** — precise math, data analysis, and visualizations
- 🎨 **Image Generation** — creative visual content on demand

The agent decides **when** to use each capability based on the user's question.

&nbsp;

**Next session** — we expand knowledge reach with **SharePoint**, **Teams messages**, and **web search**. See you next week! 🚀

---
layout: intro
theme: quantum
customTheme: .demo/templates/microsoft-theme.css
transition: fadeIn
---

# Try it now! 🚀

[github.com/sebastienlevert/building-declarative-agents](https://github.com/sebastienlevert/building-declarative-agents)
