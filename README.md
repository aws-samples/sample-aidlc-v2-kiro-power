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
5. Paste the repository URL: `https://github.com/awslabs/sample-aidlc-v2-kiro-power`

### Option 2: Add as Local Power in Kiro (All Platforms)

1. Open Kiro IDE
2. Open Command Palette → "Powers: Configure"
3. Add this directory as a local power source pointing to the `aidlc-v2/` folder

### Option 3: Copy to Powers Directory (from local clone)

#### macOS

```bash
cp -R aidlc-v2/ ~/.kiro/powers/installed/aidlc-v2/
```

#### Linux

```bash
cp -R aidlc-v2/ ~/.kiro/powers/installed/aidlc-v2/
```

#### Windows (Command Prompt)

```cmd
xcopy /E /I aidlc-v2 %USERPROFILE%\.kiro\powers\installed\aidlc-v2
```

#### Windows (PowerShell)

```powershell
Copy-Item -Recurse -Path aidlc-v2 -Destination "$env:USERPROFILE\.kiro\powers\installed\aidlc-v2"
```

Then restart Kiro.

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
