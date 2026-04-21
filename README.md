# Building Declarative Agents with the Microsoft 365 Agents Toolkit

An 8-week video series that progressively builds a production-ready **Zava Insurance HR Onboarding Buddy** — from scaffold to org-wide deployment — and composes it with specialist worker agents into a full agent ecosystem.

## The Series

| # | Session | Status |
|---|---------|--------|
| 1 | [**Your First Declarative Agent — From Scaffold to Persona**](slides/session-1.md) | ✅ Complete |
| 2 | [**Configuring Agent Capabilities — Embedded Knowledge, Code Interpreter & Image Generation**](slides/session-2.md) | ✅ Complete |
| 3 | [**Grounding Your Agent with Knowledge**](slides/session-3.md) | ✅ Complete |
| 4 | [**Extending with Actions — API Plugins & MCP Servers**](slides/session-4.md) | ✅ Complete |
| 5 | [**Advanced Manifest Capabilities & Multi-Source Orchestration**](slides/session-5.md) | ✅ Complete |
| 6 | [**Connected Agents — Composing an Agent Ecosystem**](slides/session-6.md) | ✅ Complete |
| 7 | [**Testing and Debugging your Agent**](slides/session-7.md) | ✅ Complete |
| 8 | [**Shipping — Publishing, CI/CD & Governance**](slides/session-8.md) | ✅ Complete |
| 9 | Using Coding Agents to Build Declarative Agents | 🔜 Coming soon |

## The Agent Ecosystem

After **Session 6**, the repo contains three agents that work together:

| Agent | Folder | Purpose |
|-------|--------|---------|
| **HR Onboarding Buddy** | [`agents/hr-onboarding-buddy/`](agents/hr-onboarding-buddy/) | The main agent — onboarding logistics, policies, task tracking. Delegates deep questions to specialist agents. |
| **IT Support Agent** | [`agents/it-support/`](agents/it-support/) | Specialist for device troubleshooting, software access, network issues, and IT service requests. |
| **Benefits Advisor** | [`agents/benefits-advisor/`](agents/benefits-advisor/) | Specialist for health plan comparisons, 401(k) guidance, cost modeling, and enrollment workflows. |

The HR Onboarding Buddy uses `worker_agents` to transparently delegate to the IT Support Agent and Benefits Advisor when deep domain expertise is needed. The user stays in one conversation.

## Project Structure

```
building-declarative-agents/
├── agents/
│   ├── hr-onboarding-buddy/       ← Main agent (Sessions 1-6)
│   │   ├── appPackage/            ← Manifest, instructions, plugins, knowledge
│   │   ├── documents/             ← Supplementary documents
│   │   ├── env/                   ← Environment files
│   │   ├── m365agents.yml         ← Provisioning and deployment
│   │   └── m365agents.local.yml   ← Local development config
│   ├── it-support/                ← Worker agent (Session 6)
│   │   ├── appPackage/            ← Manifest, instructions, knowledge
│   │   ├── env/                   ← Environment files
│   │   └── m365agents.yml         ← Provisioning and deployment
│   └── benefits-advisor/          ← Worker agent (Session 6)
│       ├── appPackage/            ← Manifest, instructions, knowledge
│       ├── env/                   ← Environment files
│       └── m365agents.yml         ← Provisioning and deployment
├── slides/                        ← Demo Time slide decks for each session
├── .demo/                         ← Demo Time act files and theme
└── README.md
```

## Prerequisites

- [Node.js](https://nodejs.org/) (v18, 20, or 22)
- [Microsoft 365 account for development](https://docs.microsoft.com/microsoftteams/platform/toolkit/accounts)
- [Microsoft 365 Agents Toolkit VS Code Extension](https://aka.ms/teams-toolkit)
- [Microsoft 365 Copilot license](https://learn.microsoft.com/microsoft-365-copilot/extensibility/prerequisites#prerequisites)

## Getting Started

### Deploy the Worker Agents First

Connected agents must be deployed before the main agent can delegate to them.

1. Open the `agents/it-support/` folder in VS Code
2. Sign in with your Microsoft 365 account in the Agents Toolkit sidebar
3. Press **F5** to provision and deploy
4. Note the **M365 Title ID** from the output
5. Repeat for `agents/benefits-advisor/`

### Deploy the HR Onboarding Buddy

1. Copy the title IDs from the worker agents into `agents/hr-onboarding-buddy/env/.env.dev`:
   ```
   IT_SUPPORT_TITLE_ID=<title-id-from-it-support>
   BENEFITS_ADVISOR_TITLE_ID=<title-id-from-benefits-advisor>
   ```
2. Open the `agents/hr-onboarding-buddy/` folder in VS Code
3. Press **F5** to provision and deploy
4. Select the **HR Onboarding Buddy** agent in Copilot and start chatting

## Resources

- [Declarative agents for Microsoft 365](https://aka.ms/teams-toolkit-declarative-agent)
- [Microsoft 365 Agents Toolkit](https://aka.ms/teams-toolkit)
