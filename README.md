# aina-vibeguard-action

Automatically scan every PR for security vulnerabilities. Blocks merge if issues are found.

[![Patterns](https://img.shields.io/badge/patterns-48-orange)](https://github.com/Moonsehwan/aina-scan)
[![Languages](https://img.shields.io/badge/languages-9-green)](https://github.com/Moonsehwan/aina-scan)

> 🇰🇷 [한국어 README](./README.ko.md)

---

## Setup — 2 Minutes

**Step 1:** Add secret to your repo → **Settings → Secrets → New secret**
- Name: `VIBEGUARD_KEY`
- Value: `vg_free_test`

**Step 2:** Create `.github/workflows/vibeguard.yml`:

```yaml
name: Security Scan
on: [pull_request]
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Moonsehwan/aina-vibeguard-action@v1
        with:
          api-key: ${{ secrets.VIBEGUARD_KEY }}
          fail-on-block: 'true'
```

Done. Every PR is now scanned automatically.

---

## 🎉 Free Promo Key

`vg_free_test` — Full Pro features free until **2026-06-25**. No signup.

---

## 🔒 Your Code Is Safe

Files are processed **in memory only** — never saved, never stored, never shared. Discarded immediately after scan.

---

## What Gets Scanned

**48 patterns, 9 languages** — same engine as [AINAScan API](https://github.com/Moonsehwan/aina-scan)

- 33 security patterns: SQL injection, XSS, hardcoded secrets, command injection, SSRF, path traversal, and more
- 15 vibe-coding patterns: stubs with no logic, fake async, missing DB writes, AI copy-paste encoding bugs

---

## Options

```yaml
- uses: Moonsehwan/aina-vibeguard-action@v1
  with:
    api-key: ${{ secrets.VIBEGUARD_KEY }}
    fail-on-block: 'true'   # 'false' = report only, don't fail the check
```

---

## Pricing

| | Free | Pro | Team |
|--|------|-----|------|
| Price | $0 | $19/mo | $60/mo |
| Files/day | 50 | Unlimited | Unlimited |

> 🎉 Use `vg_free_test` free until **2026-06-25**

---

**Docs & API:** [aina-scan](https://github.com/Moonsehwan/aina-scan) · **Issues:** [open one here](https://github.com/Moonsehwan/aina-vibeguard-action/issues) · **Email:** shanyshany3528@gmail.com
