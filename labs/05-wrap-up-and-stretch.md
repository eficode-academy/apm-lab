# Part 5: Wrap-up and stretch

> **Stretch · pick what interests you** · [Back to overview](../README.md)

## What you built

- ✅ A shared set with a skill, an agent and an instruction under `.apm/`
- ✅ A tagged release on GitHub
- ✅ A second repo that installs it with one command
- ✅ Lockfile restore and drift detection

Where would this fit in your team? [Using APM day to day](../docs/daily-use.md) has four everyday scenarios.

## Stretch options

| Option | What you do | Read |
|--------|-------------|------|
| A. Ship a new version | Bump `team-skills` to v1.1.0 and update the consumer | below |
| B. Marketplace | Install by short name instead of a git URL | [marketplace.md](../docs/marketplace.md) |
| C. MCP server | Declare an MCP server as a dependency | [mcp.md](../docs/mcp.md) |
| D. Gate a pull request | Write a GitHub Actions job for `apm audit --ci` | [ci-gate.md](../docs/ci-gate.md) |

## Stretch A: ship a new version

1. In `team-skills`, add a rule to the instruction or tighten the agent.
2. Commit, then `git tag -a v1.1.0 -m "v1.1.0"` and `git push && git push --tags`.
3. In `consumer-test`, run `apm outdated`, then `apm install <your-handle>/team-skills#v1.1.0 --target copilot`.
4. Compare the old and new commit in `apm.lock.yaml`.

## Challenges

1. **Break it on purpose.** Put the skill at the repo root instead of under `.apm/`, push, and reinstall in the consumer. What is missing, and did anything warn you?
2. **Two consumers, one source.** Install the same tag in a third repo with `--target claude`. What differs?
3. **Gate a pull request.** Order matters: `apm audit --ci` first, then `apm install --frozen`.

## Further reading

| Topic | Link |
|-------|------|
| Docs home | https://microsoft.github.io/apm/ |
| Quickstart | https://microsoft.github.io/apm/quickstart/ |
| Your first package | https://microsoft.github.io/apm/getting-started/first-package/ |
| Source | https://github.com/microsoft/apm |
