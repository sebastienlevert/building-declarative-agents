# HR Onboarding Buddy

The main agent in the Zava Insurance onboarding ecosystem. Helps new employees navigate their first weeks — answering questions about onboarding logistics, benefits, IT setup, and company policies.

## Capabilities

| Capability | Source |
|------------|--------|
| 🧠 Instructions & persona | `appPackage/instruction.txt` |
| 📦 Embedded Knowledge | Employee Handbook, Benefits Guide, IT Setup Guide |
| 📚 SharePoint | HR Onboarding document library |
| 💬 Teams | New Hires 2026 channel |
| 🌐 Web Search | Zava Insurance careers website |
| ✉️ Email & 👥 People | Microsoft Graph |
| 🧮 Code Interpreter | Financial calculations |
| 🎨 Image Generation | Welcome graphics and visual content |
| 🔌 API Plugin | Repairs API for onboarding status |
| 🛰️ MCP Plugin | Zava Claims MCP server |
| 🤝 Connected Agents | IT Support Agent + Benefits Advisor |

## Connected Agents

This agent delegates deep domain questions to specialist worker agents:

- **IT Support Agent** — For hardware troubleshooting, driver issues, network diagnostics
- **Benefits Advisor** — For plan comparisons, cost modeling, enrollment guidance

## Deployment

1. Deploy the worker agents first (see `../it-support/` and `../benefits-advisor/`)
2. Copy their M365 Title IDs into `env/.env.dev`
3. Open this folder in VS Code and press **F5**
