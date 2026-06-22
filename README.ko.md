# aina-vibeguard-action (한국어)

PR마다 보안 취약점 자동 스캔. 문제 발견 시 머지 차단.

[![Patterns](https://img.shields.io/badge/패턴-48개-orange)](https://github.com/Moonsehwan/aina-scan)
[![Languages](https://img.shields.io/badge/언어-9개-green)](https://github.com/Moonsehwan/aina-scan)

> 🇺🇸 [English README](./README.md)

---

## 설정 — 2분

**Step 1:** 레포에 시크릿 추가 → **Settings → Secrets → New secret**
- Name: `VIBEGUARD_KEY`
- Value: `vg_free_test`

**Step 2:** `.github/workflows/vibeguard.yml` 파일 생성:

```yaml
name: 보안 스캔
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

끝. 이제 모든 PR이 자동으로 스캔된다.

---

## 🎉 무료 프로모 키

`vg_free_test` — **2026-06-25까지** Pro 기능 전체 무료. 회원가입 불필요.

---

## 🔒 코드는 안전하다

파일은 **메모리에서만** 처리 — 저장 안 됨, 보관 안 됨, 공유 안 됨. 스캔 완료 즉시 삭제.

---

## 뭘 스캔하나

**48개 패턴, 9개 언어** — [AINAScan API](https://github.com/Moonsehwan/aina-scan)와 동일한 엔진

- 보안 33개: SQL 인젝션, XSS, 하드코딩 시크릿, 명령어 인젝션, SSRF, 경로 탐색 등
- 바이브코딩 15개: 로직 없는 뼈대 함수, 가짜 async, DB write 없는 저장 함수, AI 복붙 인코딩 버그

---

## 옵션

```yaml
- uses: Moonsehwan/aina-vibeguard-action@v1
  with:
    api-key: ${{ secrets.VIBEGUARD_KEY }}
    fail-on-block: 'true'   # 'false' = 리포트만, 실패 처리 안 함
```

---

## 가격

| | 무료 | Pro | Team |
|--|------|-----|------|
| 가격 | $0 | $19/월 | $60/월 |
| 파일/일 | 50개 | 무제한 | 무제한 |

> 🎉 `vg_free_test`로 **2026-06-25까지** 무료

---

**문서/API:** [aina-scan](https://github.com/Moonsehwan/aina-scan) · **이슈:** [여기](https://github.com/Moonsehwan/aina-vibeguard-action/issues) · **이메일:** shanyshany3528@gmail.com
