# Marketplace (Stretch B)

[Back to overview](../README.md) · Optional, for Part 5.

A marketplace is a second repo that indexes packages living elsewhere, so consumers install by short name instead of a git URL.

**You need:** a second empty GitHub repo named `team-marketplace`, and `team-skills` tagged `v1.0.0` (Part 3).

```bash
cd ~/apm-work
git clone https://github.com/<your-handle>/team-marketplace.git
cd team-marketplace
apm marketplace init --owner <your-handle> --name team-marketplace
apm marketplace package add <your-handle>/team-skills --name team-skills --version "^1.0.0"
```

1. Open `apm.yml`. `init` scaffolds a placeholder `example-package` entry: delete it, or validation fails.
2. Validate and build the index:

   ```bash
   apm marketplace check
   apm pack
   ```

   `check` resolves every entry against the remote. `pack` writes a `marketplace.json` that pins your range to a concrete tag and commit.
3. Commit `apm.yml` and the generated `marketplace.json`, push, then consume it from `consumer-test`:

   ```bash
   apm marketplace add <your-handle>/team-marketplace
   apm install team-skills@team-marketplace --target copilot
   ```

> ⚠️ Watch out: a `.gitignore` that excludes `*.json` will silently stop `marketplace.json` from being committed.

`apm marketplace check` is also the CI step for marketplace authors: it fails if an entry cannot resolve.

The Copilot Academy lab [APM in 60 Minutes](https://copilot-academy.github.io/labs/apm-in-60-minutes) walks through the same flow in more depth, plus the monorepo layout.
