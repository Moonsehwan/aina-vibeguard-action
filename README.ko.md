# aina-vibeguard-action (한국어)

> [AINAScan VibeGuard](https://github.com/Moonsehwan/aina-scan) GitHub Action — AI 생성 코드용 48개 패턴 보안·바이브코딩 스캐너.

[![Marketplace](https://img.shields.io/badge/GitHub%20Marketplace-aina--vibeguard--action-blue)](https://github.com/marketplace/actions/aina-vibeguard-scan)
[![Patterns](https://img.shields.io/badge/패턴-48개-orange)](https://github.com/Moonsehwan/aina-scan)
[![Languages](https://img.shields.io/badge/언어-9개-green)](https://github.com/Moonsehwan/aina-scan)

> 🇺🇸 [English README here](./README.md)

---

## 무슨 역할을 하나

PR마다 코드를 자동 스캔한다. 보안 취약점 발견 시 머지를 차단한다. 설정 최소화.

```
Pull Request 생성
       ↓
aina-vibeguard-action
       ↓
AST 정적 분석 (결정론적, LLM 없음)
       ↓
48개 패턴 검사 (보안 33개 + 바이브코딩 15개)
       ↓
PASS → 머지 허용
FAIL → 머지 차단 (fail-on-block: true 시)
```

---

## 🎉 무료 프로모 키

> **`vg_free_test`** — Pro 기능 전체 무료, **2026-06-25까지**. 회원가입 불필요.

GitHub 시크릿에 추가하면 2분 안에 설정 완료.

---

## 🔒 프라이버시 — 당신의 코드는 안전하다

| | 실제로 하는 일 |
|---|---|
| **코드 처리** | 서버 메모리에서만 — 디스크나 DB에 절대 기록 안 함 |
| **기록하는 것** | 파일명, BLOCK/WARN 개수, 타임스탬프만 |
| **코드 보관** | **제로.** 스캔 완료 즉시 삭제 |
| **제3자 공유** | 절대 없음 |
| **LLM 학습** | 절대 없음 |

> 코드는 API 서버로 전송돼 메모리에서 처리되고 **즉시 삭제**된다. 반환되는 것은 JSON 결과뿐이다. [오픈소스 스캐너 코드](https://github.com/Moonsehwan/aina-scan)에서 직접 확인 가능하다.

**네트워크 전송 자체가 싫다면?** `aina-vibeguard` Python 패키지로 완전 로컬 스캔 — 업로드 없음.

---

## 빠른 설정 (2분)

**Step 1 — API 키를 GitHub 시크릿에 추가:**
1. 레포 → **Settings → Secrets and variables → Actions**
2. **New repository secret** 클릭
3. Name: `VIBEGUARD_KEY`
4. Value: `vg_free_test`

**Step 2 — 워크플로우 파일 생성:**

```yaml
# .github/workflows/vibeguard.yml
name: VibeGuard 보안 스캔
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

완료. 이제 모든 PR이 자동으로 보호된다.

---

## 입력값 (Inputs)

| Input | 필수 | 기본값 | 설명 |
|-------|------|--------|------|
| `api-key` | 예 | — | VibeGuard API 키. GitHub 시크릿으로 저장. |
| `fail-on-block` | 아니오 | `'true'` | `'false'`로 설정하면 리포트만 하고 실패 안 함. |
| `path` | 아니오 | `.` | 스캔할 디렉토리. 기본값은 레포 루트. |
| `extensions` | 아니오 | `py,js,ts,go,rb,java,php,kt,c,cpp` | 스캔할 파일 확장자 (쉼표 구분). |
| `max-files` | 아니오 | `100` | 최대 파일 수 (무료: 50개/일, Pro: 무제한). |

## 출력값 (Outputs)

| Output | 설명 |
|--------|------|
| `total-blocks` | 발견된 BLOCK 이슈 총 수 |
| `total-warns` | 발견된 WARN 이슈 총 수 |
| `files-scanned` | 스캔된 파일 수 |
| `scan-report` | 전체 이슈의 JSON 요약 |

---

## 고급 설정

**특정 디렉토리만 스캔:**
```yaml
- uses: Moonsehwan/aina-vibeguard-action@v1
  with:
    api-key: ${{ secrets.VIBEGUARD_KEY }}
    path: 'src'
    extensions: 'py,ts'
    fail-on-block: 'true'
```

**리포트만 (머지 차단 없음):**
```yaml
- uses: Moonsehwan/aina-vibeguard-action@v1
  with:
    api-key: ${{ secrets.VIBEGUARD_KEY }}
    fail-on-block: 'false'
```

---

## 탐지 패턴 (48개)

### 보안 패턴 (33개)

| 카테고리 | 예시 |
|----------|------|
| 인젝션 | `SQL_INJECTION_RISK`, `COMMAND_INJECTION`, `TEMPLATE_INJECTION` |
| XSS / 경로 | `XSS_RISK`, `PATH_TRAVERSAL`, `OPEN_REDIRECT` |
| 데이터 노출 | `HARDCODED_SECRET`, `SENSITIVE_DATA_EXPOSURE` |
| 네트워크 | `SSRF_RISK`, `UNVALIDATED_REDIRECT` |
| 암호화 | `INSECURE_HASH`, `WEAK_CIPHER` |
| 실행 | `EVAL_EXEC_RISK`, `DESERIALIZATION_RISK` |
| + 27개 더 | |

### 바이브코딩 패턴 (15개 — AI 생성 코드 전용)

다른 스캐너에는 없는 **AINAScan 전용 패턴** — AI 어시스턴트가 생성한 코드를 위해 설계됨.

| 패턴 | 탐지 내용 |
|------|----------|
| `STUB_SKELETON` | `def process(data): return {}` — 로직 없는 뼈대 |
| `MISSING_WRITE` | `def save_user(...)` INSERT/UPDATE 없음 |
| `FAKE_ASYNC` | `async def f():` `await` 없음 |
| `DEAD_CALL_RESULT` | 모듈 호출 후 반환값 전부 무시 |
| `HARDCODED_TABLE` | DB 조회 대신 딕셔너리 하드코딩 |
| `INPUT_OUTPUT_DISCONNECTED` | 파라미터가 반환값에 영향 없음 |
| `MOCK_PATTERN` | 프로덕션 코드에 `unittest.mock` 잔존 |
| `ENCODING_CORRUPTION` | AI Markdown에서 복붙된 스마트 따옴표 |
| + 7개 더 | |

### 지원 언어 9개

Python · JavaScript · TypeScript · Go · Ruby · Java · PHP · Kotlin · C/C++

---

## PR 주석 예시

```
VibeGuard Security Scan ❌

2개의 BLOCK 이슈 발견:

  app/routes/users.py:42  SQL_INJECTION_RISK
  → f-string SQL 인터폴레이션. 파라미터화된 쿼리 사용 필요.

  services/auth.py:18  HARDCODED_SECRET
  → 하드코딩된 API 키 탐지. 환경변수로 이동 필요.

머지가 차단됐습니다. 위 문제를 수정 후 다시 push하세요.
```

---

## 가격

| | 무료 | Pro | Team |
|--|------|-----|------|
| **가격** | $0 | **$19/월 얼리버드** | **$60/월 얼리버드** |
| **파일/일** | 50개 | 무제한 | 무제한 |
| **48개 패턴 전체** | ✅ | ✅ | ✅ |
| **스캔 히스토리** | 7일 | 90일 | 1년 |
| **우선 지원** | ❌ | ❌ | ✅ |

> 🎉 **프로모 (2026-06-25까지):** `vg_free_test` 키로 Pro 전체 기능 무료. 회원가입 불필요.

키 받기: [aina-scan API](https://github.com/Moonsehwan/aina-scan)

---

## 관련 링크

- [aina-scan](https://github.com/Moonsehwan/aina-scan) — API 문서, curl 예시, Python SDK
- [이슈 리포트](https://github.com/Moonsehwan/aina-vibeguard-action/issues)
- **이메일:** shanyshany3528@gmail.com
