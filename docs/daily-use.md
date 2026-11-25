# Using APM day to day

[Back to overview](../README.md)

Four situations where you would reach for APM after today. Each is a few commands from the lab.

## 1. Start a new repo with the team's setup

You open a new repo and want the team's skills, reviewer agent and PR conventions in it.

```bash
apm init -y
apm install <team>/team-skills#v1.0.0 --target copilot
git add apm.yml apm.lock.yaml .github .agents && git commit -m "chore: add team agent context"
```

Covered in Part 3.

## 2. Improve a shared skill once, update everywhere

Someone tightens the PR-description skill. They tag `v1.1.0` in `team-skills`. Each consumer repo runs:

```bash
apm outdated
apm install <team>/team-skills#v1.1.0 --target copilot
```

The lockfile diff in the pull request shows exactly which commit moved. Covered in Stretch A.

## 3. Onboard a teammate

A new colleague clones the consumer repo. The deployed `.github/` and `.agents/` folders are already committed, so Copilot has the context immediately. If they want to refresh it: `apm install`. Covered in Part 3.

## 4. Make agent context reviewable and checked

Agent instructions are code that changes behaviour. Add the CI gate ([ci-gate.md](ci-gate.md)) so a hand-edited agent file, or a hidden character, fails the pull request instead of reaching production. Covered in Part 4.

## Where would it fit in your team?

Ask yourself: which instruction or skill do we already copy between repos? That is your first package.
