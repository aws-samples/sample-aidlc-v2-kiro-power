# AIDLC v2 Kiro Power

A Kiro Power that automatically sets up [AI-DLC Workflows v2](https://github.com/awslabs/aidlc-workflows/tree/v2) in your workspace.

## Power Structure

```
aidlc-v2/
├── POWER.md                        # Power manifest and documentation
├── hooks/
│   └── aidlc-prompt-check.json     # Hook to offer AI-DLC on every prompt
└── steering/
    ├── setup-aidlc.md              # Installation guide (clone, build, copy)
    └── usage-guide.md              # How to use AI-DLC workflows
```

## Installation

### Option 1: Add as Local Power in Kiro

1. Open Kiro IDE
2. Open Command Palette → "Powers: Configure"
3. Add this directory as a local power source pointing to the `aidlc-v2/` folder

### Option 2: Copy to Powers Directory

```bash
cp -R aidlc-v2/ ~/.kiro/powers/installed/aidlc-v2/
```

Then restart Kiro.

## What It Does

When activated, this power:

1. **Clones** the `v2` branch of `awslabs/aidlc-workflows` from GitHub
2. **Builds** the Kiro distribution using `make build-kiro`
3. **Copies** the built artifacts (agents, skills, hooks, common files) into your workspace's `.kiro/` directory
4. **Installs a hook** that offers the AI-DLC workflow on every prompt

## Prerequisites

- Git (for cloning the repository)
- Node.js (for the build script and process checker)
- Internet access

## License

MIT-0
