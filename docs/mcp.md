# Declaring an MCP server (Stretch C)

[Back to overview](../README.md) · Optional, for Part 5.

In `consumer-test`, add an MCP server to `dependencies.mcp` in `apm.yml`, replacing the empty `mcp: []` that `apm init` wrote. This one needs no API key:

```yaml
dependencies:
  mcp:
    - io.github.ChromeDevTools/chrome-devtools-mcp
```

Keep the existing `apm:` list as it is. Run `apm install --target copilot`, then `git status` to see which file APM wrote for Copilot.

APM blocks transitive MCP servers unless they are declared, which is why this one is listed explicitly.

Find other servers with `apm mcp search <word>`. Many ask for a token or API key during install; do not paste a real secret into a lab repo.
