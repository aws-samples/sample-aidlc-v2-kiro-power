# AIDLC v2 Kiro Power

A Kiro Power that automatically sets up [AI-DLC Workflows v2](https://github.com/awslabs/aidlc-workflows/tree/v2) in your workspace.

## Power Structure

```
aidlc-v2/
├── POWER.md                        # Power manifest and documentation
└── steering/
    ├── setup-aidlc.md              # Installation guide (clone, build, copy)
    └── usage-guide.md              # How to use AI-DLC workflows
```

## Installation

### Option 1: Install from Kiro Powers Tab

1. Open Kiro IDE
2. Navigate to the **Powers** tab
3. Click **Add Custom Power**
4. Select **Import from GitHub**
5. Paste the repository URL:
   ```
   https://github.com/awslabs/sample-aidlc-v2-kiro-power
   ```

### Option 2: Download from GitHub and Add via Powers Tab

1. Download the repository as a ZIP from GitHub
2. Unzip the downloaded file
3. Open Kiro IDE
4. Navigate to the **Powers** tab
5. Click **Add Custom Power**
6. Select **Import from Local Folder**
7. Browse to the extracted folder and select the `aidlc-v2/` directory

## Compatibility

This power works in Kiro in both **Vibe** and **Spec** modes.

## What It Does

When activated, this power:

1. **Downloads** the `v2` branch of `awslabs/aidlc-workflows` from GitHub
2. **Builds** the Kiro distribution using `make build-kiro`
3. **Copies** the built artifacts (agents, skills, hooks, common files) into your workspace's `.kiro/` directory
4. **Installs a hook** that offers the AI-DLC workflow on every prompt

## Prerequisites

- Node.js (for the build script and process checker)
- Internet access

## License

MIT-0
