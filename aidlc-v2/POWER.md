---
name: "aidlc-v2"
displayName: "AI-DLC Workflows v2"
description: "AI-Driven Life Cycle (AI-DLC) v2 adaptive workflow steering for autonomous software development. Downloads and installs AIDLC v2 into your workspace, then offers to use the AI-DLC workflow for every prompt. Use when you want structured, deterministic software development with requirements analysis, design, and code generation phases."
keywords: ["aidlc", "ai-dlc", "workflow", "software development", "requirements", "design", "code generation", "autonomous", "lifecycle", "sdlc", "development lifecycle", "structured development"]
author: "AWS"
---

# AI-DLC Workflows v2

## Overview

This Power installs the [AI-DLC Workflows v2](https://github.com/awslabs/aidlc-workflows/tree/v2) into your current workspace. AI-DLC (AI-Driven Life Cycle) is an autonomous software development workflow that provides structured, deterministic phases for building software — from intent capture through requirements, design, and code generation.

Once installed, at the beginning of each new conversation the power asks once whether you want to use the AI-DLC workflow or proceed with normal Kiro behavior. Your choice applies for the entire session.

## What Gets Installed

The power downloads the AIDLC v2 branch archive, builds the distribution, and copies it into your workspace's `.kiro/` directory:

```
<project-root>/
└── .kiro/
    ├── agents/          # AI-DLC builder and validator agents
    ├── aidlc-common/    # Shared conventions, protocols, and scripts
    │   ├── conventions/ # Folder structure, question format, state schema, workflow format
    │   ├── protocols/   # Builder, orchestrator, and validator protocols
    │   └── scripts/     # Deterministic process checker (Node.js)
    ├── hooks/           # Process-check hook for deterministic verification
    └── skills/          # AI-DLC skill set (orchestrator, requirements, design, etc.)
```

### Skills Included

- **aidlc-orchestrator** — Routes your intent to the appropriate workflow
- **aidlc-intent-bootstrap** — Captures and structures your development intent
- **aidlc-requirements-analysis** — Generates structured requirements
- **aidlc-user-stories** — Creates user stories from requirements
- **aidlc-functional-design** — Produces functional design artifacts
- **aidlc-application-design** — Application architecture design
- **aidlc-infrastructure-design** — Infrastructure design
- **aidlc-nfr-assessment** — Non-functional requirements assessment
- **aidlc-nfr-design** — Non-functional requirements design
- **aidlc-wireframes** — UI wireframe generation
- **aidlc-code-generation** — Deterministic code generation
- **aidlc-units-generation** — Unit test generation
- **aidlc-reverse-engineering** — Reverse-engineer existing codebases
- **aidlc-workflow-composition** — Compose custom workflows

### Agents Included

- **aidlc-builder-agent** — Executes code generation and implementation tasks
- **aidlc-validator-agent** — Validates outputs against requirements and design

## How to Use

1. **Activate this power** — It will download and install AIDLC v2 into your workspace automatically.
2. **Start a task** — At the beginning of the conversation, you'll be asked once: "Would you like to use AI-DLC for this session, or proceed normally?"
3. **If using AI-DLC** — Prefix your intent with "Using AI-DLC, ..." or simply confirm when asked.
4. **Review artifacts** — AI-DLC generates artifacts at each stage for your review before proceeding.

## Steering Files

Load the setup workflow to install AIDLC v2:

- **Install AIDLC v2 into workspace** → `readPowerSteering("aidlc-v2", "setup-aidlc.md")`
- **Usage guide and workflow details** → `readPowerSteering("aidlc-v2", "usage-guide.md")`

## Prerequisites

- **Node.js** must be installed (used by the build script and deterministic process checker)
- Internet access to download from GitHub

## When to Use This Power

Activate this Power when the user:

- Mentions AI-DLC, AIDLC, or development lifecycle workflows
- Wants structured, phased software development
- Asks for requirements-driven development
- Wants deterministic code generation with verification
- Mentions autonomous software development workflows

## Important Notes

- v2 is pre-release and actively evolving — expect breaking changes
- Use Kiro in **Vibe mode** for AI-DLC workflows (decline if nudged to Spec mode)
- Artifacts land in `org-ai-kb/aidlc-docs/intent-<nnn>-<slug>/` next to your project
- Generative AI can make mistakes — review all output

## Hook — Create at Runtime

When this power is activated, create the following hook in the workspace so the user is prompted at the start of each conversation. Create the file `.kiro/hooks/aidlc-prompt-check.kiro.hook` with this content:

```json
{
  "name": "AI-DLC Workflow Choice",
  "version": "1.0.0",
  "description": "At the start of a new conversation, asks the user once whether they want to use the AI-DLC structured workflow or proceed with normal Kiro behavior for the entire session.",
  "when": {
    "type": "promptSubmit"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Only if this is the FIRST message in the conversation (no prior exchanges exist), ask the user: 'Would you like to use **AI-DLC (recommended)** for this session (structured requirements → design → code workflow), or proceed normally?' Present AI-DLC as the recommended option. Do NOT ask again on subsequent prompts in the same conversation. If the user has already prefixed their message with 'Using AI-DLC' or explicitly mentioned AIDLC, skip asking and proceed with the AI-DLC workflow by activating the aidlc-orchestrator skill. If the user previously chose 'proceed normally' or 'no' at the start of this conversation, continue with standard Kiro behavior without asking again. If the user previously chose AI-DLC, continue using the AI-DLC workflow without asking again."
  }
}
```

This hook must use the `.kiro.hook` extension to appear in the Kiro Agent Hooks tab.

## License

MIT-0 License. See the [LICENSE](https://github.com/awslabs/aidlc-workflows/blob/v2/LICENSE) file.
