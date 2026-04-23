---
title: "Conditional Rule Loading — 파일 패턴별 규칙 자동 로드"
type: concept
tags: [claude-code, context-engineering, rules, frontmatter, lazy-loading]
created: 2026-04-23
updated: 2026-04-23
sources: [jimcoding-claude-rules]
aliases: ["조건부 규칙 로딩", ".claude/rules 패턴", "frontmatter glob rules"]
---

# Conditional Rule Loading

## 핵심 요약

`.claude/rules/*.md` 에 규칙 파일을 주제별로 쪼갠 뒤, 각 파일의 **frontmatter 에 glob 패턴을 지정**해 해당 파일 패턴을 작업할 때만 규칙이 컨텍스트에 자동 진입하게 만드는 패턴. [[concepts/context-engineering|lazy loading]] 의 가장 구체적인 구현체. 핵심은 파일 분할이 아니라 **조건** — 조건 없이 쪼개면 토큰 절감 효과 없다.

> [!note] 상위 개념
> [[concepts/context-engineering]] 의 **Lazy Loading** 섹션의 실행 가능한 형태. 저자 짐코딩이 Trigger.dev · CockroachDB 같은 인기 오픈소스의 실제 구현에서 역공학해 체계화.

---

## 왜 필요한가

### 실패 모드 — CLAUDE.md 비대화

프로젝트가 커지면서 API · 테스트 · 프런트 · DB 규칙이 루트 `CLAUDE.md` 한 파일에 누적된다. 결과:

- 매 세션 수천 토큰을 상시 점유
- 지금 작업과 무관한 규칙이 관련 규칙을 **의미적으로 묻어버림**
- Claude 가 느려지고 규칙 무시 빈도 증가

Reddit 사례: 모노레포에서 `CLAUDE.md` 가 47,000 단어까지 비대. 조건부 로딩으로 재구성 후 **루트 메모리 80% 감소** (출처 확인 필요).

### 비유

- **라떼 레시피** — 라떼 만드는 법을 알려주면서 청소·재고·마감까지 동시에 가르치면 핵심 레시피가 묻힌다.
- **맥도날드 매뉴얼** — 주방 직원에게 배달 직원 매뉴얼을 쥐여줄 필요 없다. 각자 자기 업무 매뉴얼만.

---

## 구현

### 디렉터리 구조

```
project/
├── CLAUDE.md                    # 항상 로드 — 절대 규칙만
└── .claude/
    └── rules/
        ├── coding-style.md      # 조건 없음 = 항상 로드
        ├── react-components.md  # src/components/**/*.tsx 작업 시
        ├── api-design.md        # src/api/**/*.ts 작업 시
        ├── testing.md           # **/*.test.ts 작업 시
        └── database.md          # src/db/**/* 작업 시
```

### 파일 포맷

각 규칙 파일 상단 frontmatter 에 적용 패턴 지정:

```markdown
---
globs: ["src/api/**/*.ts", "src/routes/**/*.ts"]
---

# API 작성 규칙

- 엔드포인트는 REST 명명 규칙 준수
- 응답 포맷은 `ApiResponse<T>` 사용
- 에러는 `APIError` 로 throw
...
```

> [!question] 정확한 Frontmatter 필드명
> 소스 영상은 "해당 패턴" 표현만 사용하고 필드명(`globs` / `applyTo` / `patterns` 등)을 명시하지 않음. Anthropic Claude Code 공식 문서 교차 확인 필요.

### 로드 동작

Claude 가 파일을 **열거나 편집하려 할 때** 경로를 glob 패턴과 매칭 → 매칭된 규칙만 해당 세션 컨텍스트에 삽입. 작업 외 규칙은 디스크에만 존재.

---

## 단순 파일 분할과의 차이

| 접근 | 파일 | Frontmatter 조건 | 로드 동작 |
|------|------|-----------------|-----------|
| 단순 분할 | `.claude/rules/*.md` | ❌ 없음 | **전부 항상 로드** → 토큰 절감 ❌ |
| 조건부 로딩 | `.claude/rules/*.md` | ✅ glob | **해당 파일 작업 시만** 로드 → 절감 ✅ |

**본질은 조건**. 파일 쪼개기만으론 아무 효과 없다.

---

## 오픈소스 실제 사례

### Trigger.dev (⭐14K · 반복 작업 자동 실행 플랫폼)

`.claude/rules/` 에 주제별 5개 파일. 대표 예:

- **database-safety.md** — DB 관련 파일 건드릴 때만 로드. "데이터 삭제는 반드시 승인 후 진행" 류 규칙.
- 문서 / 테스트 / API 등 주제별 분리.

### CockroachDB (⭐32K · 대용량 분산 DB)

모든 `.go` 파일에 **개인정보 로그 마스킹 규칙** 자동 적용:

```markdown
---
globs: ["**/*.go"]
---

# PII Logging Rule

에러 메시지 및 로그 출력 시 개인 식별 정보는
반드시 마스킹된 상태로 출력할 것.
```

효과: Claude 가 Go 파일을 건드릴 때마다 자동 강제 → **PII 로그 노출 원천 차단**. 이건 단순 스타일 규칙을 넘어 **하네스 엔지니어링**([[concepts/harness-engineering]])의 시스템 강제로 해석 가능.

---

## 추천 구성 — 쇼핑몰 예시

| 파일 | glob | 적용 시점 |
|------|------|-----------|
| `coding-style.md` | — (조건 없음) | 전 작업 |
| `react-components.md` | `src/components/**/*.tsx` | 상품 목록 UI |
| `api-design.md` | `src/api/**/*.ts` | 결제 API |
| `testing.md` | `**/*.test.ts` | 테스트 작성 |
| `database.md` | `src/db/**/*` | 스키마 작업 |

결과:
- 상품 목록 작업 → `coding-style + react-components` 만 컨텍스트 진입
- 결제 API 작업 → `coding-style + api-design` 만 진입
- 테스트 작성 → `coding-style + testing` 만 진입

---

## 관련 메커니즘과의 비교

| 메커니즘 | 트리거 방식 | 로드 단위 | 적합 대상 |
|---------|-----------|----------|-----------|
| 루트 `CLAUDE.md` | 매 세션 자동 | 파일 전체 | 절대 규칙 |
| 폴더별 `CLAUDE.md` (하위/상위) | 작업 경로 기반 | 파일 전체 | 도메인 규칙 |
| **`.claude/rules/*.md` + glob** | **파일 패턴 매칭** | **조건 일치 파일만** | **파일 타입별 규칙** |
| [[concepts/claude-skills\|Skills]] | description **의미 해석** | description → 본문 2단계 | 재사용 워크플로우 |
| MCP | 연결 시 상시 | 도구 설명 전체 | 외부 시스템 통합 |

**Skills 와의 차이**: Skills 는 사용자 발화를 의미적으로 해석해 트리거 — `"PPT 만들어줘"` 같은 자연어. Conditional Rule Loading 은 Claude 가 **실제 파일을 읽거나 편집하려는 순간**의 경로 기반 자동 매칭. 언어 해석 불필요.

---

## 즉시 적용 가이드

1. 현재 `CLAUDE.md` 를 열어 **파일 종류별로 섞인 규칙**을 식별 (테스트 / CSS / API 등).
2. 각 주제를 `.claude/rules/{topic}.md` 로 분리.
3. 각 파일 상단 frontmatter 에 해당 파일 패턴 glob 지정.
4. 루트 `CLAUDE.md` 는 **전 작업 공통 절대 규칙**만 유지 (300줄 이하 권장).
5. Claude 재시작 후 관련 없는 작업에서 해당 규칙이 로드되지 않는지 `/context` 로 확인.

---

## 한계 · 주의

- **조건 없는 파일 = 상시 로드**. 파일 쪼개기 자체에 의미 부여 말 것.
- glob 과잉 매칭 주의 — `**/*.ts` 는 사실상 상시 로드와 같음.
- 규칙 간 **모순 방지** — 두 파일이 동시에 로드되는 상황에서 상충 금지. 공통 규칙은 루트로 승격.
- Claude 가 glob 매칭을 **실제로 어떻게 수행하는지** (경로 문자열 매칭인지, 파일 내용 기반인지) 공식 문서 확인 필요.

---

## 관련 페이지

- [[concepts/claude-md]] — 비대화 실패 모드 출발점
- [[concepts/context-engineering]] — 상위 개념 (Lazy Loading)
- [[concepts/claude-skills]] — 다른 트리거 방식 (의미 해석)
- [[concepts/harness-engineering]] — 보안 규칙 강제 관점 (CockroachDB PII 사례)
- [[concepts/hooks]] — 이벤트 기반 자동화와 대비

## 출처

- [[sources/jimcoding-claude-rules]] — 유일 출처. Trigger.dev · CockroachDB 사례 포함.

> [!question] 공식 문서 검증 필요
> Frontmatter 필드명, glob 매칭 알고리즘, 로드 우선순위 (rules vs 폴더별 CLAUDE.md) 등은 Anthropic Claude Code docs 에서 1차 확인 후 보강 예정.
