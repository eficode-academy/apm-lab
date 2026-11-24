# Authoring primitives: skill, agent, instruction

[Back to overview](../README.md) · Deep dive for Part 2. The finished files live in [`solution/.apm/`](../solution/.apm/); this page explains them and shows how to write them from scratch.

## Three primitives, three invocation models

| Primitive | Invoked by | Use it when |
|-----------|-----------|-------------|
| Skill | The model, automatically | Copilot should reach for a guide based on what you asked |
| Agent | You, by selecting it | You want a persona or role on demand |
| Instruction | A file glob | A rule should apply whenever matching files are touched |

APM also handles prompts, hooks and MCP servers. Prompt files appear as slash commands in VS Code Chat; the Copilot CLI does not expose them, but skills do show up there as slash commands.

## Writing them from scratch

Scaffold first:

```bash
cd ~/apm-work
mkdir team-skills && cd team-skills
git init -b main
apm init -y
mkdir -p .apm/skills/pr-description .apm/agents .apm/instructions
```

### The skill

`.apm/skills/pr-description/SKILL.md`. Copilot reads the `description` to decide when to load it, so lead with the user's intent.

```markdown
---
name: pr-description
description: >-
  Use when the user asks for a pull-request description, a summary of
  uncommitted or staged changes, or release notes for a change.
---
# PR Description Skill

Produce these sections, in order:

## Summary
One sentence. What changes and why. No file lists.

## Changes
Bullets grouped by area (API, Tests, Docs). One bullet per logical change.

## Risk and rollback
Breaking changes, migrations, feature flags, and how to revert.

## Testing
Commands run and environments tested.
```

### The agent

`.apm/agents/team-reviewer.agent.md`:

```markdown
---
name: team-reviewer
description: Senior reviewer that critiques diffs against team standards before a PR.
---
# Team Reviewer

You review a teammate's diff before it becomes a pull request.

Check in this order: correctness, test coverage of changed paths,
naming and clarity, new public surface area.

Group findings as Blocking, Should fix, Nit. Cite file and line.
End with one verdict: "Ready to ship", "Address blockers then ship",
or "Needs another pass". Point and explain; do not rewrite the code.
```

### The instruction

`.apm/instructions/pr-conventions.instructions.md`:

```markdown
---
description: Tone and format rules for pull-request templates and changelogs.
applyTo: "**/PULL_REQUEST_TEMPLATE.md,**/CHANGELOG.md"
---
# PR and changelog conventions

- Write for the reviewer, not for the author. Say why, not only what.
- Present tense, one change per line ("Adds", "Fixes").
- Never paste secrets, tokens or internal hostnames into descriptions.
```

## Tuning tips

- **Skill did not fire?** Reword its `description` to include the exact phrases you typed, reinstall, and retry.
- **Agent:** it never runs by itself. Select it (VS Code: the agent picker in Chat; Copilot CLI: `/agent`).
- **Instruction:** `applyTo` is a glob. It only applies when a matching file is touched, so test with a file that matches.
- If you rename the skill, `scripts/check.sh author` will report it missing; that is fine.

## Where `apm pack` looks

`apm pack` only collects from `.apm/skills/`, `.apm/agents/` and `.apm/instructions/` (and `.apm/prompts/`, `.apm/hooks/`). A file outside those folders is silently left out; `apm pack --dry-run --verbose` shows what would be bundled.
