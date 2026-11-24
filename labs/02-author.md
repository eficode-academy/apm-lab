# Part 2: Author a shared set

> **Core · ~15 minutes** · [Back to overview](../README.md)

**Goal:** deploy a skill, an agent and an instruction, and see each one behave differently in Copilot.

| Primitive | Invoked by |
|-----------|-----------|
| Skill | The model, automatically |
| Agent | You, by selecting it |
| Instruction | A file glob that matches |

## 2.1 Scaffold

```bash
cd ~/apm-work
mkdir team-skills && cd team-skills
git init -b main
apm init -y
mkdir -p .apm
```

## 2.2 Start from the finished set

```bash
cp -R "$LAB/solution/.apm/." .apm/
ls .apm/skills .apm/agents .apm/instructions
```

**Expect:** `pr-description`, `team-reviewer.agent.md`, `pr-conventions.instructions.md`.

Want to write them from scratch instead? [Authoring primitives](../docs/authoring-primitives.md) has every file.

## 2.3 Read the three files

Each is under 25 lines. Look at the first lines of each: that is where the invocation is decided.

```bash
head -8 .apm/skills/pr-description/SKILL.md      # description: when the model loads it
head -6 .apm/agents/team-reviewer.agent.md       # a role you pick
head -5 .apm/instructions/pr-conventions.instructions.md   # applyTo: a file glob
```

## 2.4 Deploy

```bash
apm install --target copilot
```

**Expect:** one agent in `.github/agents/`, one instruction in `.github/instructions/`, one skill in `.agents/skills/`.

## 2.5 Try them in Copilot

Make something to review:

```bash
printf 'function add(a, b) { return a - b }\n' > add.js
git add add.js
```

Open Copilot Chat (or the Copilot CLI) in this folder. **Predict first:** which prompt needs you to name something, and which two work without naming anything?

1. `draft a PR description for my staged changes`. **Expect:** the skill activates without being named.
2. Select the **team-reviewer** agent yourself (VS Code: agent picker; CLI: `/agent`), then `review my staged changes`. **Expect:** it spots the `a - b` bug and ends with a verdict.
3. Create `CHANGELOG.md`, then ask Copilot to add an entry. **Expect:** the entry follows the conventions, because the glob matched.

> **Skill did not fire?** Reword its `description` to use the words you typed, then `apm install --target copilot` again.

## 2.6 Make one change

Edit one line in the skill, the agent or the instruction (for example, add a rule about secrets to the agent). Run `apm install --target copilot` again and repeat the matching trial.

✅ **Checkpoint:** `"$LAB/scripts/check.sh" author` passes in `team-skills`, and you can say what decided *when* each primitive ran.

Next: [Part 3, Publish and share](03-publish-and-share.md)
