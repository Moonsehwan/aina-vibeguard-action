# VibeGuard Security Scanner — GitHub Action

[![Marketplace](https://img.shields.io/badge/GitHub%20Marketplace-VibeGuard-red?logo=github)](https://github.com/marketplace/actions/vibeguard-security-scanner)
[![PyPI](https://img.shields.io/pypi/v/aina-vibeguard?label=aina-vibeguard)](https://pypi.org/project/aina-vibeguard/)

**AST-based security scanner for AI-generated Python code.**  
May flag false positives. Never misses a real one.

---

## Quick Start

```yaml
# .github/workflows/vibeguard.yml
name: VibeGuard Security Scan

on: [pull_request]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: aina/aina-vibeguard-action@v1
        with:
          api-key: ${{ secrets.VIBEGUARD_KEY }}
```

Add `VIBEGUARD_KEY` to **Settings → Secrets → Actions**.  
Get a free API key → **[vibeguard.dev](https://vibeguard.dev)**

PR fails automatically if security blocks are found.

---

## Real Finding

Scanned **serena** (25K ⭐) — AI coding assistant:

```
CRITICAL  COMMAND_INJECTION  agent.py:1222
          subprocess.Popen(cmd, shell=True)
          → any config file can execute arbitrary commands
```

Found in 3 seconds. Missed by Semgrep. Missed by the maintainers.

---

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `api-key` | VibeGuard API key (**required**) | — |
| `scan-path` | File or directory to scan | `.` |
| `fail-on-block` | Fail PR when blocks found | `true` |
| `agent-friendly` | JSON+Markdown with before/after code | `false` |
| `max-files` | Max Python files per run | `200` |

## Outputs

| Output | Description |
|--------|-------------|
| `blocks` | `1` if security blocks found, `0` if clear |

---

## Examples

### Block PRs on security issues

```yaml
- uses: aina/aina-vibeguard-action@v1
  with:
    api-key: ${{ secrets.VIBEGUARD_KEY }}
    fail-on-block: 'true'
```

### Agent-friendly output (auto-fix with Claude Code)

```yaml
- uses: aina/aina-vibeguard-action@v1
  with:
    api-key: ${{ secrets.VIBEGUARD_KEY }}
    agent-friendly: 'true'
```

### Scan specific directory

```yaml
- uses: aina/aina-vibeguard-action@v1
  with:
    api-key: ${{ secrets.VIBEGUARD_KEY }}
    scan-path: 'src/'
```

### Warning-only mode (non-blocking)

```yaml
- uses: aina/aina-vibeguard-action@v1
  with:
    api-key: ${{ secrets.VIBEGUARD_KEY }}
    fail-on-block: 'false'
```

---

## What It Detects

13 security patterns including `COMMAND_INJECTION`, `PATH_TRAVERSAL`, `SQL_INJECTION_RISK`, `INSECURE_RANDOM`, `WEAK_CRYPTO`, `HARDCODED_SECRET`, `EVAL_EXEC_RISK`, and more.

Full pattern list → [aina-vibeguard](https://github.com/shanyshany3528/aina-vibeguard)

---

## Pricing

| | Free | Pro | Team |
|--|------|-----|------|
| Price | $0 | $9/mo | $29/mo |
| Scans/day | 20 | 500 | Unlimited |
| Causal attack chains | ❌ | ✅ | ✅ |
| Project scan | ❌ | ❌ | ✅ |

---

## Contact

- Issues: [github.com/shanyshany3528/aina-vibeguard/issues](https://github.com/shanyshany3528/aina-vibeguard/issues)
- Email: Aina.vibeguard@gmail.com

---

MIT License
