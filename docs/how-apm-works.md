# How APM works

[Back to overview](../README.md) · Deep dive for Part 1. Read it when you are curious; the lab does not depend on it.

## Three things do all the work

| Item | Written by | Purpose |
|------|-----------|---------|
| `apm.yml` | you | The manifest: your dependencies and targets |
| `apm.lock.yaml` | APM | Exact commits and SHA-256 hashes of everything deployed |
| `apm_modules/` | APM | Package cache. Gitignored, rebuildable |

## What `apm install` does, every time

```text
apm install
  1. Resolve    turn "owner/repo#v1.0.0" into an exact commit
  2. Policy     check apm-policy.yml, if your org ships one
  3. Scan       reject hidden-Unicode tricks in package content
  4. Integrate  copy files into the folders each detected tool reads
  5. Lockfile   record commits and content hashes
```

## Where files land

Package authors write one layout (`.apm/`). APM rewrites files into the folders each tool reads:

| Author writes | Copilot reads |
|---------------|---------------|
| `.apm/skills/x/SKILL.md` | `.agents/skills/x/SKILL.md` |
| `.apm/agents/x.agent.md` | `.github/agents/x.agent.md` |
| `.apm/instructions/x.instructions.md` | `.github/instructions/x.instructions.md` |

Agents and instructions go to `.github/`, a Copilot-specific location. Skills go to `.agents/skills/`, a location several tools read.

## Reading the lockfile: the unpinned dependency

`apm.lock.yaml` has one entry per package under `dependencies:`. When you install `microsoft/apm-sample-package#v1.0.0` you get two entries. Find both:

```bash
grep -E "repo_url|resolved_ref|resolved_commit|resolved_by|depth" apm.lock.yaml
```

| Field | `microsoft/apm-sample-package` (you asked for it) | `github/awesome-copilot` (came along) |
|-------|---------------------------------------------------|----------------------------------------|
| `resolved_ref` | `v1.0.0`, the tag you typed | **missing** |
| `resolved_commit` | an exact commit | an exact commit (a snapshot of today) |
| `resolved_by` | missing: you installed it directly | `microsoft/apm-sample-package`: it was pulled in by the sample package |
| `depth` | missing (direct) | `2` (a dependency of a dependency) |

A missing `resolved_ref` is the unpinned dependency. The sample package's own `apm.yml` lists `github/awesome-copilot/skills/review-and-refactor` with no `#tag` or `#sha`, so APM followed the default branch. The lockfile records the commit it got, so *your* repeat installs stay the same. A fresh resolve, such as an install without the lockfile or after an update, can return newer content. Only the package author can fix it, by adding a ref.

You can confirm the author's side:

```bash
cat apm_modules/microsoft/apm-sample-package/apm.yml
```

## What to commit

The rule differs for the two kinds of repo in the lab:

| Path | Consumer repo | Package repo (you publish it) | Why |
|------|:---:|:---:|-----|
| `apm.yml` | yes | yes | The manifest |
| `apm.lock.yaml` | yes | no | Reproducible installs for consumers; a package has no dependencies to pin |
| `.apm/` | n/a | yes | The source you author |
| `.github/`, `.agents/` | yes | no | A consumer's teammates get the context on clone, even without APM. In a package repo these are regenerated copies |
| `apm_modules/` | no | no | Rebuildable cache, already in `.gitignore` |

## Same package, another tool

Run the same install in a second folder with `--target claude` and compare: the package lands in `.claude/`. Warnings about dropped frontmatter keys are expected; they show APM adapting files per tool.
