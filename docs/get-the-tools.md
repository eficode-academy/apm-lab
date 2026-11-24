# Get the tools

[Back to overview](../README.md) · Do this **before** the learnathon. It takes about 15 minutes. Steps come from the official docs (linked under each tool) and were checked on 2026-11-24. Menus and package names change often, so if something differs, trust the linked page.

You need a **GitHub account with a Copilot seat** and **Git**. Which of the three tools you need depends on the exercise:

| Tool | Needed for |
|------|-----------|
| Copilot CLI | Hooks, APM (most checks) |
| Copilot app | The Copilot app and automations lab |
| VS Code with Copilot Chat | APM Part 1 step 4 (optional) and any exercise you prefer to run in an editor |

> **Short on time or on Windows?** Use the **devcontainer** in this repo (VS Code: *Dev Containers: Reopen in Container*). It avoids most install and PATH problems.

## 1. Copilot CLI

Official docs: [Install Copilot CLI](https://docs.github.com/en/copilot/how-tos/set-up/install-copilot-cli) · [CLI quickstart](https://docs.github.com/en/copilot/get-started/cli-quickstart)

Pick **one** install method:

| OS | Command |
|----|---------|
| macOS or Linux (script) | `curl -fsSL https://gh.io/copilot-install \| bash` |
| Windows (WinGet) | `winget install GitHub.Copilot` |
| Any OS (npm, needs Node.js 22+) | `npm install -g @github/copilot` |
| macOS or Linux (Homebrew) | `brew install --cask copilot-cli` |

Windows needs PowerShell 6 or newer. The docs and the CLI repo disagree on the Homebrew command (`--cask` or not); if one fails, try the other, or use the script.

**Sign in:**

1. `cd` into a project folder and run `copilot`.
2. Type `/login` and follow the prompts. You only do this once.
3. Confirm you trust the files in the folder.

## 2. Copilot app

Official docs: [Quickstart](https://docs.github.com/en/copilot/get-started/quickstart-copilot-app) · [Getting started](https://docs.github.com/en/copilot/how-tos/github-copilot-app/getting-started) · [All builds](https://github.com/github/app/releases)

1. Download the installer for your OS from the [download page](https://github.com/features/ai/github-app) or the [releases page](https://github.com/github/app/releases) (the download page only shows macOS Apple Silicon):

   | OS | File |
   |----|------|
   | Windows | `GitHub-Copilot-windows-x64-setup.exe` (or `-arm64-`) |
   | macOS | `GitHub-Copilot-darwin-arm64.dmg` (Apple Silicon) or `-darwin-x64.dmg` (Intel) |
   | Linux | `GitHub-Copilot-linux-x64.AppImage` (or `-arm64-`) |

2. Install and open it, click **Sign in to GitHub**, and follow the prompts.
3. Pick repositories (or skip), choose a theme, finish onboarding.
4. To add work: click the plus next to **Sessions**, then **Open local folder**, **Add GitHub repository** or **Clone repository**.

Git must be installed. On Windows, agent sessions can also run in WSL 2.

If you hit trouble on Windows: restart the PC after installing, and check that you are not behind a VPN or proxy that blocks the app's connections (the "global connector" issue some teams saw).

## 3. VS Code with Copilot Chat

Official docs: [Set up Copilot in VS Code](https://code.visualstudio.com/docs/copilot/setup)

1. Install VS Code from [code.visualstudio.com/download](https://code.visualstudio.com/download).
2. Sign in: Status Bar Copilot icon, **Use AI Features**, then choose a sign-in method. (Or Command Palette: **GitHub Copilot: Sign in**.)

If chat does not appear, install the [Copilot Chat extension](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat).

## If it does not work

| Problem | What to check |
|---------|---------------|
| No Copilot in the tool after sign-in | You need an assigned Copilot seat on the account you signed in with |
| Copilot CLI is blocked | An enterprise owner controls it under *AI controls, Copilot, Configure features & clients*. Ask your admin whether the CLI policy is enabled for your org |
| Copilot app is blocked | It has its own policy, but the June 2026 GA announcement says the CLI policy must also be on. Check both |
| MCP servers do not load | Org and enterprise MCP policies apply to the CLI and VS Code |
| The `copilot` command is missing in VS Code | A device policy (`CopilotCliCommand`) can remove it. Run **Developer: Policy Diagnostics** to see which policy applies |
| Hooks do not fire (hooks exercise) | Use a trusted workspace and start a fresh session after adding hook files. We found no org-level hooks toggle in the docs, so ask your admin if policy might block them |

Ask early: if your organisation restricts any of these, tell the facilitator before the day.
