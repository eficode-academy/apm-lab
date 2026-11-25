# Part 1: Install and consume a package

> **Core · ~15 minutes** · [Back to overview](../README.md)

**Goal:** get APM running and install someone else's package. See which files land for Copilot.

## 1.1 Install APM

```bash
brew install apm                          # macOS
curl -sSL https://aka.ms/apm-unix | sh    # Linux
```

**Windows:** see [install-apm.md](../docs/install-apm.md), then work in **Git Bash** from here on.

```bash
apm --version
```

✅ It prints a version. Note it: version mismatches explain odd output.

## 1.2 Install a package

1. **Do:** create a project. **Expect:** one file, `apm.yml`.

   ```bash
   mkdir hack-lab && cd hack-lab
   git init
   apm init -y
   ```

2. **Predict first:** which folders will appear, and which file records exactly what was installed? Then run:

   ```bash
   apm install microsoft/apm-sample-package#v1.0.0 --target copilot
   ```

   **Expect:** prompts, an agent and an instruction go into `.github/`; a skill goes into `.agents/skills/`.
   **Why `--target`:** a fresh project has no Copilot marker yet, so APM cannot guess.

3. **Do:** look at what changed. How close was your guess?

   ```bash
   cat apm.lock.yaml
   ls .github/prompts .github/agents .github/instructions .agents/skills
   ```

4. **Optional:** open the folder in VS Code, type `/` in Copilot Chat and run one of the installed prompts. (The Copilot CLI does not list prompt files as slash commands; skills do appear there.)

## 1.3 Find the unpinned dependency

You installed one package but got a warning: `[!] 1 dependency unpinned`. Find it:

```bash
grep -E "repo_url|resolved_ref|resolved_by" apm.lock.yaml
```

**Expect:** `github/awesome-copilot` has **no** `resolved_ref`, and `resolved_by` says the sample package pulled it in.
**Why it matters:** an unpinned dependency follows a branch, so a fresh resolve can return newer content than today's.

*Curious?* [How APM works](../docs/how-apm-works.md) explains the install steps, the lockfile and what to commit.

✅ **Checkpoint:** `"$LAB/scripts/check.sh" consume` passes in `hack-lab`, and you can answer:

- Which file records the exact commit installed?
- Why did step 2 need `--target copilot`?
- Which package is unpinned, and which field gives it away?

<details>
<summary>Answers</summary>

1. `apm.lock.yaml`.
2. APM found no harness marker to auto-detect.
3. `github/awesome-copilot`: no `resolved_ref`.

</details>

Next: [Part 2, Author a shared set](02-author.md)
