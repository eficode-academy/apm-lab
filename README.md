# Lab: APM and Sharing

> **Core path ~55 minutes**, plus stretch · **Level:** Beginner to Intermediate · **Format:** self-service, hands-on

## Objective

A teammate writes a great Copilot skill. A month later it exists as five slightly different copies in five repos, and nobody knows which one is current.

APM (Agent Package Manager) is a dependency manager for agent configuration. You declare what you need in `apm.yml`, `apm install` deploys it where Copilot reads it, and a lockfile pins exact commits and content hashes.

**Challenge:** publish a shared set, install it in a second repo, and pass `apm audit --ci` on the consumer side.

![Three steps. 1: the team-skills folder holds apm.yml and a .apm folder with a skill, an agent and an instruction. 2: it is pushed and tagged v1.0.0 on GitHub. 3: the consumer-test folder installs it with apm install and holds apm.yml, apm.lock.yaml and the deployed .github and .agents folders. CI runs apm audit --ci.](docs/images/apm-full-picture.jpg)

## Before you start

This lab **publishes a GitHub repository** (`team-skills`) under your account. Public is simplest. Use a personal or sandbox account if your employer restricts public repos.

**Prerequisites**

- A terminal and Git. **Windows: use Git Bash (or WSL) for the whole lab.**
- A git identity (`git config user.name` and `user.email` print values; if not, set them inside each project folder, no `--global`)
- A GitHub account with Copilot access, and the [GitHub CLI](https://cli.github.com/) signed in. This is required: APM uses it for GitHub lookups.

  ```bash
  gh auth login
  gh auth setup-git
  gh auth status         # must say "Logged in to github.com"
  ```

- VS Code with Copilot Chat (the Copilot CLI works for most checks)
- An **empty** GitHub repo named `team-skills` (tick nothing: no README, no .gitignore, no license)

Need to install Copilot or APM? See [get the tools](docs/get-the-tools.md) and [install APM](docs/install-apm.md). A [devcontainer](.devcontainer/devcontainer.json) with everything pre-installed is included.

## Get the lab

```bash
git clone https://github.com/eficode-academy/apm-lab ~/apm-lab
export LAB=~/apm-lab           # re-run this in every new terminal
mkdir -p ~/apm-work && cd ~/apm-work
```

## Your path

| Part | Guide | What you do | Track | Min |
|------|-------|-------------|-------|-----|
| 1 | [Install and consume](labs/01-setup-and-consume.md) | Install APM and a published package, inspect the result | **Core** | 15 |
| 2 | [Author a shared set](labs/02-author.md) | Deploy a skill, an agent and an instruction and try them | **Core** | 15 |
| 3 | [Publish and share](labs/03-publish-and-share.md) | Push, tag, install into a second repo | **Core** | 15 |
| 4 | [Verify and secure](labs/04-verify-and-secure.md) | Catch a hand-edit and a hidden character | **Core** | 10 |
| 5 | [Wrap-up and stretch](labs/05-wrap-up-and-stretch.md) | New version, marketplace, MCP, CI gate | Stretch | 30+ |

Each part has steps and a **Checkpoint**. Verify it with the helper script, from the folder the checkpoint names:

```bash
"$LAB/scripts/check.sh" <setup|consume|author|publish|verify>
```

Stuck? See the [appendix](labs/appendix.md). Behind? The finished package is in [`solution/`](solution/).

## Go deeper (optional)

| Topic | Page |
|-------|------|
| Where APM fits in your day | [daily-use.md](docs/daily-use.md) |
| How installs, lockfiles and targets work | [how-apm-works.md](docs/how-apm-works.md) |
| Writing skills, agents, instructions | [authoring-primitives.md](docs/authoring-primitives.md) |
| Security layers and policy | [security-model.md](docs/security-model.md) |
| Diagrams of the full picture | [architecture.md](docs/architecture.md) |
| Marketplace, MCP, CI gate | [marketplace.md](docs/marketplace.md), [mcp.md](docs/mcp.md), [ci-gate.md](docs/ci-gate.md) |

## Web view and pairs

- **Web view:** `cd "$LAB" && python3 -m http.server 8000`, then open http://localhost:8000. It loads scripts from a CDN, so it needs internet.
- **Pairs:** work solo through Part 2. In Part 3, swap GitHub handles and install your partner's package into a **fresh folder** (for example `consumer-partner`), because two packages with the same file names overwrite each other.

> **APM moves fast.** The guides were verified against the version in [install-apm.md](docs/install-apm.md). If output differs, check the [official docs](https://microsoft.github.io/apm/).
