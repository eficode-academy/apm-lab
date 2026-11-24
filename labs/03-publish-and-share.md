# Part 3: Publish and share

> **Core · ~15 minutes** · [Back to overview](../README.md)

**Goal:** publish `team-skills` to GitHub, tag a release, and install it in a second repo.

## 3.1 Publish

Open `apm.yml` first: `author:` was filled from your git config, so edit it if you do not want that public.

Preview what a consumer would receive:

```bash
apm pack --dry-run --verbose
```

**Expect:** your agent, instruction and skill in the list. Anything missing is in the wrong folder (see [authoring primitives](../docs/authoring-primitives.md)). A license warning is normal.

Commit the manifest and `.apm/` only. Never `git add .`:

```bash
cd ~/apm-work/team-skills
git rm --cached -q add.js 2>/dev/null || true
git add apm.yml .apm/ .gitignore
git commit -m "feat: initial team-skills package"
git remote add origin https://github.com/<your-handle>/team-skills.git
git push -u origin main
```

## 3.2 Tag a release

A tag lets consumers stay stable while you keep changing `main`.

```bash
git tag -a v1.0.0 -m "v1.0.0"
git push --tags
```

## 3.3 Install it in a second repo

```bash
cd ~/apm-work
mkdir consumer-test && cd consumer-test
git init
apm init -y
apm install <your-handle>/team-skills#v1.0.0 --target copilot
```

**Expect:** `apm.yml` records the dependency, `apm.lock.yaml` pins the commit, and the three files are deployed.

```bash
cat apm.yml apm.lock.yaml
ls .github/agents .github/instructions .agents/skills
```

Pair option: use your partner's handle (their repo must be public) in a fresh folder such as `consumer-partner`.

## 3.4 Try it

This repo is empty, so give Copilot something to review, then repeat the three trials from [Part 2.5](02-author.md):

```bash
printf 'function add(a, b) { return a - b }\n' > add.js && git add add.js
```

**Expect:** the same skill, agent and instruction work in a repo that has none of your source files.

## 3.5 Delete and restore (optional)

> ⚠️ **Destructive.** Only in `consumer-test`, the disposable repo. The `if` does nothing anywhere else.

```bash
if [ "$(basename "$PWD")" = consumer-test ]; then
  rm -rf .github .agents apm_modules
  apm install --target copilot
  ls .github/agents
else
  echo "Wrong folder: cd to consumer-test first"
fi
```

**Why:** `git clone` plus `apm install` reproduces the setup on any machine.

✅ **Checkpoint:** `"$LAB/scripts/check.sh" publish` passes in `team-skills` (remote and tag) and again in `consumer-test`.

> **Package not found?** The repo is private or misspelled. Make it public, or run `gh auth login`. Never put a token in a file or commit it.

Next: [Part 4, Verify and secure](04-verify-and-secure.md)
