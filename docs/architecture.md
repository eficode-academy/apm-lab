# The full picture

[Back to overview](../README.md)

APM in everyday use: write once, publish a tag, and many repos consume it, with CI as the gate. Each folder in the picture is a real one you create in the lab.

![Three steps. 1: the team-skills folder, the producer, holds apm.yml and a .apm folder with a skill, an agent and an instruction. 2: it is pushed and tagged v1.0.0 on GitHub. 3: the consumer-test folder installs it with apm install and holds apm.yml, apm.lock.yaml, and the deployed .github and .agents folders. CI runs apm audit --ci.](images/apm-full-picture.jpg)

## Detailed version: what each repo commits, and the CI order

![Diagram of the lab repos. team-skills (producer) holds apm.yml and a .apm folder with a skill, an agent and an instruction. It is pushed to GitHub and tagged v1.0.0. consumer-test installs it with apm install and holds apm.yml, apm.lock.yaml and the deployed .github and .agents folders. CI runs apm audit --ci, then apm install --frozen.](images/apm-repos.svg)

## Text version

```text
┌─────────────────────────────┐
│ AUTHOR     team-skills      │   Part 2: you write it once
│   apm.yml                   │
│   .apm/skills/              │
│   .apm/agents/              │
│   .apm/instructions/        │
└──────────────┬──────────────┘
               │ git push, git tag v1.0.0       Part 3
               ▼
┌─────────────────────────────┐
│ GITHUB   <you>/team-skills  │   the shared source: v1.0.0, v1.1.0, ...
└──────┬───────────────┬──────┘   (optional: a marketplace repo indexes it
       │               │           by name, Stretch B)
       │ apm install <you>/team-skills#v1.0.0
       ▼               ▼
┌──────────────┐ ┌──────────────┐
│ repo A       │ │ repo B       │ ...  any number of repos
│ (Copilot)    │ │ (Claude Code)│      you build one: consumer-test
│ .github/     │ │ .claude/     │
│ .agents/     │ │              │      each one keeps its own
│ apm.lock.yaml│ │ apm.lock.yaml│      lockfile: exact commit + hashes
└──────┬───────┘ └──────┬───────┘
       └───────┬────────┘
               ▼ on every pull request          Part 4
┌─────────────────────────────┐
│ CI    apm audit --ci        │   catches hand-edits and hidden characters
│       apm install --frozen  │   then restores exactly what the lockfile says
└─────────────────────────────┘
```
