# Setup AI-DLC v2

This steering file guides the automatic installation of AI-DLC Workflows v2 into the current workspace.

## Overview

AI-DLC v2 installs as a `.kiro/` directory at your project root. The installation process detects the operating system, downloads the repository archive directly into the workspace, builds the distribution, copies the output into your workspace's `.kiro/` directory, and then removes the downloaded files.

## Installation Steps

Follow these steps exactly to install AIDLC v2:

### Step 1: Detect Operating System

Determine the user's OS by running:

```bash
uname -s
```

- If the output is `Darwin` → the system is **macOS**
- If the output is `Linux` → the system is **Linux**
- If the command fails or returns `MINGW`, `MSYS`, or `CYGWIN` → the system is **Windows**

Alternatively on Windows (PowerShell):

```powershell
$env:OS
```

Use the detected OS to select the correct commands in subsequent steps.

### Step 2: Check Prerequisites

**macOS / Linux:**

```bash
curl --version
unzip -v
node --version
```

**Windows (PowerShell):**

```powershell
node --version
```

`curl` and `unzip` must be available on macOS/Linux (they are pre-installed on most systems). `node` must be installed and on PATH on all platforms. If any are missing, inform the user and stop.

### Step 3: Download the Repository into Workspace

Download the v2 branch as a zip archive and extract it:

**macOS / Linux:**

```bash
curl -L -o aidlc-workflows-v2.zip https://github.com/awslabs/aidlc-workflows/archive/refs/heads/v2.zip
unzip aidlc-workflows-v2.zip -d .
mv aidlc-workflows-v2 aidlc-workflows-v2 2>/dev/null || mv aidlc-workflows-2 aidlc-workflows-v2 2>/dev/null || mv aidlc-workflows-* aidlc-workflows-v2
```

**Windows (PowerShell):**

```powershell
Invoke-WebRequest -Uri "https://github.com/awslabs/aidlc-workflows/archive/refs/heads/v2.zip" -OutFile aidlc-workflows-v2.zip
Expand-Archive -Path aidlc-workflows-v2.zip -DestinationPath .
Rename-Item -Path aidlc-workflows-* -NewName aidlc-workflows-v2
```

This creates an `aidlc-workflows-v2/` folder in the workspace root without requiring Git.

### Step 4: Build the Kiro Distribution

**macOS / Linux:**

```bash
cd aidlc-workflows-v2 && make build-kiro
```

**Windows (PowerShell):**

Windows does not have `make` by default. Run the build script directly with bash (Git Bash):

```powershell
cd aidlc-workflows-v2
bash targets/kiro/build.sh
```

If Git Bash is not available, install `make` via Chocolatey (`choco install make`) and then run:

```powershell
cd aidlc-workflows-v2
make build-kiro
```

This produces `dist/kiro/.kiro/` containing agents, aidlc-common, hooks, and skills.

### Step 5: Prepare Workspace

**macOS / Linux:**

```bash
mkdir -p .kiro
```

**Windows (PowerShell):**

```powershell
New-Item -ItemType Directory -Force -Path .kiro
```

### Step 6: Copy to Workspace

**macOS / Linux:**

```bash
cp -R aidlc-workflows-v2/dist/kiro/.kiro/agents .kiro/
cp -R aidlc-workflows-v2/dist/kiro/.kiro/aidlc-common .kiro/
cp -R aidlc-workflows-v2/dist/kiro/.kiro/skills .kiro/
```

**Windows (PowerShell):**

```powershell
Copy-Item -Recurse -Force aidlc-workflows-v2\dist\kiro\.kiro\agents .kiro\
Copy-Item -Recurse -Force aidlc-workflows-v2\dist\kiro\.kiro\aidlc-common .kiro\
Copy-Item -Recurse -Force aidlc-workflows-v2\dist\kiro\.kiro\skills .kiro\
```

Then create the AI-DLC prompt-check hook in `.kiro/hooks/` using the `.kiro.hook` extension so it appears in the Kiro Agent Hooks tab:

**macOS / Linux:**

```bash
mkdir -p .kiro/hooks
cat > .kiro/hooks/aidlc-prompt-check.kiro.hook << 'HOOK'
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
HOOK
```

**Windows (PowerShell):**

```powershell
New-Item -ItemType Directory -Force -Path .kiro\hooks
@'
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
'@ | Set-Content -Path .kiro\hooks\aidlc-prompt-check.kiro.hook -Encoding UTF8
```

Also copy any additional hooks from the AIDLC build output and rename them from `.json` to `.kiro.hook` so Kiro recognizes them in the Agent Hooks tab:

**macOS / Linux:**

```bash
for f in aidlc-workflows-v2/dist/kiro/.kiro/hooks/*.json; do
  [ -f "$f" ] && cp "$f" ".kiro/hooks/$(basename "${f%.json}.kiro.hook")"
done
```

**Windows (PowerShell):**

```powershell
Get-ChildItem aidlc-workflows-v2\dist\kiro\.kiro\hooks\*.json -ErrorAction SilentlyContinue | ForEach-Object {
  $newName = $_.BaseName + ".kiro.hook"
  Copy-Item -Force $_.FullName -Destination ".kiro\hooks\$newName"
}
```

### Step 7: Clean Up — Remove the Downloaded Files

**macOS / Linux:**

```bash
rm -rf aidlc-workflows-v2
rm -f aidlc-workflows-v2.zip
```

**Windows (PowerShell):**

```powershell
Remove-Item -Recurse -Force aidlc-workflows-v2
Remove-Item -Force aidlc-workflows-v2.zip
```

This ensures the workspace stays clean with only the `.kiro/` installation remaining.

### Step 8: Verify Installation

Confirm the following directories exist in your workspace:

```
.kiro/agents/aidlc-builder-agent.json
.kiro/agents/aidlc-validator-agent.json
.kiro/aidlc-common/conventions/
.kiro/aidlc-common/protocols/
.kiro/aidlc-common/scripts/aidlc-process-checker.js
.kiro/hooks/aidlc-prompt-check.kiro.hook
.kiro/skills/aidlc-orchestrator/
.kiro/skills/aidlc-intent-bootstrap/
.kiro/skills/aidlc-requirements-analysis/
.kiro/skills/aidlc-code-generation/
```

The skills directory should contain these subdirectories:
- aidlc-orchestrator
- aidlc-intent-bootstrap
- aidlc-requirements-analysis
- aidlc-user-stories
- aidlc-functional-design
- aidlc-application-design
- aidlc-infrastructure-design
- aidlc-nfr-assessment
- aidlc-nfr-design
- aidlc-wireframes
- aidlc-code-generation
- aidlc-units-generation
- aidlc-reverse-engineering
- aidlc-workflow-composition

### Step 9: Confirm to User

Tell the user:
- AI-DLC v2 has been installed successfully
- They should restart their Kiro session to load the new skills
- They can start using AI-DLC by prefixing prompts with "Using AI-DLC, ..."
- Use Kiro in Vibe mode (not Spec mode) for AI-DLC workflows

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Download fails | Ensure you have internet access. On macOS/Linux, verify `curl` is installed. On Windows, PowerShell 5+ includes `Invoke-WebRequest` by default. Check if `aidlc-workflows-v2/` already exists in the workspace and remove it first. |
| `make build-kiro` fails (macOS/Linux) | Ensure Node.js is installed and on PATH. Run `cd aidlc-workflows-v2 && make clean && make build-kiro`. |
| `make` not found (Windows) | Use Git Bash to run `bash targets/kiro/build.sh`, or install make via `choco install make`. |
| Skills not loading in Kiro | Confirm `.kiro/` exists at project root with agents/, aidlc-common/, hooks/, skills/ subfolders. Restart Kiro session. |
| Permission denied | Check write permissions on the workspace directory. On macOS/Linux, try `chmod -R u+w .kiro/`. |
| File encoding issues | Ensure files are UTF-8 encoded. |
| Windows paths not resolving | Use forward slashes `/` inside markdown files. Backslashes can break path resolution. |
| PowerShell execution policy blocks scripts | Run `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned` and retry. |

## Re-installation / Update

To update to the latest v2, simply re-run all the setup steps. The `cp -R` will overwrite existing files with the latest version from the repository.

## What Happens After Installation

Once installed, the AI-DLC skills and hooks become active in your Kiro workspace:

1. The **process-check hook** validates deterministic process compliance
2. The **aidlc-orchestrator** skill routes your intent to the appropriate workflow stage
3. All other skills handle their respective phases (requirements, design, code generation, etc.)
4. The **builder and validator agents** handle implementation and verification tasks
