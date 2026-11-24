# The APM security model

[Back to overview](../README.md) · Deep dive for Part 4.

A prompt is a program for an LLM, so APM treats agent context like code. These layers sit around it (per the APM docs):

| Layer | What it does | In the lab |
|-------|--------------|------------|
| Hidden-Unicode scan | Flags invisible characters that can smuggle instructions into a prompt | **Demonstrated** in Part 4 |
| Content hashing | The lockfile pins a SHA-256 per deployed file | **Demonstrated** in Part 4 |
| Pinned refs | A tag or commit instead of a moving branch | Compare `#v1.0.0` with the unpinned `github/awesome-copilot` entry in [How APM works](how-apm-works.md) |
| Transitive MCP gating | MCP servers from dependencies are blocked unless you declare them | [MCP](mcp.md) |
| Policy | `apm-policy.yml` limits allowed sources, tighten-only from enterprise to org to repo | Below |

## Discussion

The sample package pulled an unpinned dependency that tracks a branch tip, which is mutable third-party content. Which layer would have caught a poisoned prompt? What would you add?

## `apm audit` vs `apm audit --ci`

`apm audit` rebuilds your agent context in a scratch folder and diffs it against the working tree. It **reports** drift but exits 0. `apm audit --ci` exits non-zero, so use it for pull-request gates. A line saying it could not determine the org from the git remote is harmless.

## Why order matters in CI

An `apm install` can overwrite deployed files and hide the drift that `apm audit --ci` is meant to catch. Run the audit first, then the frozen install. Newer APM versions reject edited deployed files in `--frozen` mode instead.

## Poisoned packages are blocked at the door

`apm install` refuses to deploy files with critical findings (such as a hidden U+202E) unless you pass `--force`. The lab demo edits a file that is already on disk; removing it lets the install restore the package copy.

## Policy

Organisations can enforce rules with an `apm-policy.yml` that restricts allowed sources at install time, tighten-only from enterprise to org to repo. See the [policy docs](https://microsoft.github.io/apm/enterprise/apm-policy/).
