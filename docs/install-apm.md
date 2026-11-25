# Install APM

[Back to overview](../README.md)

**Verified version: 0.35.1.** This is the one place the lab names a version. If your output differs from the guides, check `apm --version` first and then the [official docs](https://microsoft.github.io/apm/).

## macOS (Homebrew)

```bash
brew install apm
```

## Linux, or macOS without Homebrew

Read the script first if you prefer.

```bash
curl -sSL https://aka.ms/apm-unix | sh
```

## Windows

The only PowerShell step in the lab is the installer. Run it once in PowerShell, then close it and do everything else in **Git Bash** (or WSL).

```powershell
irm https://aka.ms/apm-windows | iex
```

If `apm` is not found in Git Bash afterwards, install it with Python from Git Bash:

```bash
pip install --user "apm-cli==0.35.1"
```

If `apm` is still not found, pip's user scripts folder is not on your `PATH`. Add it, and put the same line in `~/.bashrc` to keep it for new shells:

```bash
export PATH="$(cygpath -u "$(python -c 'import sysconfig; print(sysconfig.get_path("scripts", sysconfig.get_preferred_scheme("user")))')"):$PATH"
```

## Check it worked

Open a new shell, then:

```bash
apm --version
```

## Container or Codespace

The installer can exit 0 with a broken binary. Always run `apm --version`. If it fails with a sqlite error, run `pip install --user "apm-cli==0.35.1"` instead.

## Still stuck?

| Symptom | Likely cause and fix |
|---------|----------------------|
| `apm: command not found` | Open a new shell, or run the `PATH` line the installer printed |
| TLS or certificate errors | See the [SSL troubleshooting page](https://microsoft.github.io/apm/troubleshooting/ssl-issues/) |

More symptoms are in the [appendix](../labs/appendix.md).
