# Building Declarative Agents with the Microsoft 365 Agents Toolkit

An 8-week video series that progressively builds a production-ready **Zava Insurance HR Onboarding Buddy** — from scaffold to org-wide deployment.

## The Series

| # | Session | Status |
|---|---------|--------|
| 1 | [**Your First Declarative Agent — From Scaffold to Persona**](slides/session-1.md) | ✅ Complete |
| 2 | Configuring Agent Capabilities — Embedded Knowledge, Code Interpreter & Image Generation | 🔜 Coming soon |
| 3 | Grounding Your Agent with Knowledge | 🔜 Coming soon |
| 4 | Extending with Actions — API Plugins & MCP Servers | 🔜 Coming soon |
| 5 | Advanced Manifest Capabilities | 🔜 Coming soon |
| 6 | Connected Agents — Composing an Agent Ecosystem | 🔜 Coming soon |
| 7 | Testing and Debugging your Agent | 🔜 Coming soon |
| 8 | Publishing, Governance & Real-World Patterns | 🔜 Coming soon |

## Current State of the Agent

After **Session 1**, the HR Onboarding Buddy has:

- ✅ Scaffolded project with the Microsoft 365 Agents Toolkit
- ✅ Rich 5-part instruction framework (role, tone, scope, guardrails, response guidelines)
- ✅ 5 conversation starters for new employee scenarios
- ✅ Deployed to Microsoft 365 Copilot

The agent runs purely on instructions — no knowledge sources, no API connections, no external actions yet. Those come in future sessions.

## Prerequisites

- [Node.js](https://nodejs.org/) (v18, 20, or 22)
- [Microsoft 365 account for development](https://docs.microsoft.com/microsoftteams/platform/toolkit/accounts)
- [Microsoft 365 Agents Toolkit VS Code Extension](https://aka.ms/teams-toolkit)
- [Microsoft 365 Copilot license](https://learn.microsoft.com/microsoft-365-copilot/extensibility/prerequisites#prerequisites)

## Getting Started

1. Clone this repo and open in VS Code
2. Sign in with your Microsoft 365 account in the Agents Toolkit sidebar
3. Press **F5** to provision and deploy
4. Select the **HR Onboarding Buddy** agent in Copilot and start chatting

## Project Structure

| Path | Description |
|------|-------------|
| `appPackage/declarativeAgent.json` | Agent manifest — name, description, conversation starters |
| `appPackage/instruction.txt` | Agent instructions — role, tone, scope, guardrails |
| `appPackage/manifest.json` | Teams app manifest |
| `m365agents.yml` | Provisioning and deployment configuration |
| `slides/` | Demo Time slide decks for each session |
| `.demo/` | Demo Time act files and theme |

## Resources

- [Declarative agents for Microsoft 365](https://aka.ms/teams-toolkit-declarative-agent)
- [Microsoft 365 Agents Toolkit](https://aka.ms/teams-toolkit)
