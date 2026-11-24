# Gating a pull request with APM

[Back to overview](../README.md) · Optional, for Part 5 (Challenge 3).

Two commands make the gate. **Order matters:**

```bash
apm audit --ci           # step 1: drift and security gate. Run it BEFORE any install
apm install --frozen     # step 2: fail if apm.yml and apm.lock.yaml disagree
```

An `apm install` can overwrite deployed files and hide the drift that `apm audit --ci` is meant to catch.

Try writing the GitHub Actions job yourself first.

<details>
<summary>Hint: a minimal workflow</summary>

```yaml
name: apm-gate
on: pull_request
permissions:
  contents: read
jobs:
  apm:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install apm-cli
      - name: Audit first
        run: apm audit --ci
      - name: Then frozen install
        env:
          GITHUB_TOKEN: ${{ github.token }}
        run: apm install --frozen --target copilot
```

Pin `apm-cli` to the version in [install-apm.md](install-apm.md) if you want identical results to the lab.

</details>
