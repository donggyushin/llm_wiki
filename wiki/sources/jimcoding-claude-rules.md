---
title: "클로드 코드 대규모 프로젝트 컨텍스트 엔지니어링 - .claude/rules 규칙 쪼개기"
type: source
tags: [video, youtube, claude-code, context-engineering, korean, jimcoding]
created: 2026-04-23
updated: 2026-04-23
sources: []
aliases: ["jimcoding-claude-rules", "짐코딩 rules 쪼개기"]
---

# 클로드 코드 대규모 프로젝트 컨텍스트 엔지니어링 - .claude/rules 규칙 쪼개기

## 메타

- **채널**: 짐코딩 ([[entities/jimcoding]])
- **URL**: https://youtu.be/FBv8hK_DtJ8
- **업로드**: 2026-03-26
- **길이**: 11분 25초
- **유형**: video · 한국어 · 실습형
- **전사 경로**: `raw/jimcoding-claude-rules-transcript.md` (yt-dlp 자동자막)
- **시리즈 포지션**: 3부작 중 1편 — 2·3편은 하위/상위 디렉터리 메모리 전략 예고

## 핵심 요약

대규모 프로젝트에서 `CLAUDE.md` 단일 파일이 비대해지면 **지금 필요한 규칙이 불필요 규칙에 묻히는** 현상이 발생한다. 저자는 **3대 전략**으로 해결을 제안하는데, 이 영상은 그중 첫 번째 — `.claude/rules/*.md` 디렉터리 + **frontmatter glob 조건**을 이용한 조건부 로딩 — 에 집중한다. 핵심 포인트: 파일을 쪼개는 것이 아니라 **파일 패턴별 자동 로드 조건**을 설정해 작업 시점에 필요한 규칙만 컨텍스트에 진입시키는 것.

## 상세 내용

### 비대화 문제 — "라떼 비유"

카페에서 바리스타에게 라떼 만드는 법을 알려주며 청소·재고 정리·마감까지 한꺼번에 설명하면, 정작 중요한 **라떼 레시피가 다른 정보 속에 묻혀 버린다**. CLAUDE.md 도 동일: 프로젝트 초기엔 단일 파일이 충분하지만 API 규칙 · 테스트 규칙 · 프런트 규칙이 누적되면 Claude 가 **느려지고 규칙을 무시하기 시작**한다.

### 실 사례 — 47,000 단어 / 80% 감소

한 개발자가 Reddit 커뮤니티에 공유한 경험:
- 프론트/백엔드 모노레포에서 `CLAUDE.md` 가 **47,000 단어**까지 비대해짐
- 규칙을 조건부 로딩으로 재구성 → **루트 메모리 80% 축소**
- "단순 파일 분할이 아닌 **필요 시점 로딩**으로 설계한 것이 요점"

> [!question] Reddit 원글 URL 미제공
> 영상에서 구체적 Reddit 스레드 링크 제공 없음. 실제 수치 검증 불가. 추정 출처: r/ClaudeAI / r/programming. 추후 검색으로 보강 가능.

### 3대 전략 (저자 제안)

1. **`.claude/rules/*.md` + frontmatter glob** ← 이 영상
2. 하위 디렉터리 메모리 파일 (다음 영상 예고)
3. 상위 디렉터리 메모리 파일 (다음 영상 예고)

세 가지를 조합하면 **"프로젝트가 아무리 커져도 지금 작업에 필요한 규칙만 Claude 머릿속에 들어가는"** 상태가 된다.

### 전략 ① 동작 원리

비유: 맥도날드 매장에 주방 직원용 · 카운터 직원용 · 배달 직원용 매뉴얼이 따로 있다. 주방 직원은 배달 매뉴얼을 볼 필요가 없다.

구현:
- `.claude/rules/` 디렉터리에 주제별 `.md` 파일 배치
- 각 파일 **맨 위 frontmatter 에 glob 패턴** 지정
- Claude 는 해당 패턴에 일치하는 파일을 건드릴 때만 규칙을 로드

```markdown
---
globs: ["src/api/**/*.ts", "src/routes/**/*.ts"]
---

# API 작성 규칙
...
```

> [!question] Frontmatter 필드 정확 이름
> 영상은 "해당 패턴" 이라고만 언급. 공식 문서의 필드명(`globs` / `applyTo` / `patterns` 등) 은 영상에서 명시 안 함. Anthropic Claude Code docs 교차 확인 필요.

### 오픈소스 레퍼런스

**Trigger.dev** (⭐14K, 반복 작업 자동 실행 플랫폼)
- `.claude/rules/` 에 주제별 파일 5개
- 예: **database-safety** — DB 파일 건드릴 때 로드. "데이터 삭제는 반드시 승인 후 진행" 류 규칙

**CockroachDB** (⭐32K, 대용량 분산 데이터베이스)
- 모든 `.go` 파일에 **개인정보 로그 마스킹 규칙** 자동 적용
- "에러 메시지/로그 출력 시 PII 가 될 수 있는 항목은 마스킹된 상태로 출력"
- Claude 가 Go 파일을 건드릴 때마다 자동 강제 → **개인정보 로그 노출 원천 차단**

### 쇼핑몰 예시 (저자 추천 구성)

| 파일 | glob | 로드 시점 |
|------|------|-----------|
| `coding-style.md` | 항상 (조건 없음) | 전 작업 |
| `react-components.md` | `src/components/**/*.tsx` | 상품 목록 만들 때 |
| `api-design.md` | `src/api/**/*.ts` | 결제 API 만들 때 |
| `testing.md` | `**/*.test.ts` | 테스트 작성 시 |
| `database.md` | `src/db/**/*` | DB 작업 시 |

결과: 상품 목록 작업 시 `coding-style + react-components` 만 로드. 결제 API 작업 시 `coding-style + api-design` 만 로드. **각자 자기 업무 매뉴얼만 펼쳐보는 시스템**.

### 핵심 메시지

> 단순히 규칙 파일을 쪼개는 게 포인트가 아니다.
> 진짜 효과는 **불필요한 규칙이 Claude 의 주의를 뺏지 않게** 하는 것.
> 지금 작업과 관계없는 규칙이 Claude 머릿속에 없으니 정작 중요한 규칙에 **온전히 집중**할 수 있다.

### 즉시 적용 가이드

1. 현재 프로젝트 `CLAUDE.md` 열기
2. 파일 종류별로 섞여 있는 규칙 찾기 (테스트 / CSS / API 등)
3. `.claude/rules/{주제}.md` 로 분리
4. frontmatter 에 해당 파일 패턴 지정
5. 루트 `CLAUDE.md` 는 **항상 필요한 절대 규칙**만 유지

## 핵심 Takeaway

1. 대규모 프로젝트에서 `CLAUDE.md` 비대화는 **토큰 낭비 + 규칙 무시** 를 유발
2. `.claude/rules/*.md` + frontmatter **glob 조건**으로 작업 파일 패턴별 자동 로드 가능
3. **본질은 파일 분할 아닌 조건부 로딩** — 조건 없이 쪼개면 효과 없음
4. 대형 오픈소스(Trigger.dev, CockroachDB) 가 이미 실제 적용 중
5. CockroachDB 의 PII 마스킹 사례처럼 **보안 규칙 강제**에도 효과적 — [[concepts/harness-engineering|하네스]] 관점으로도 해석 가능

## 위키 영향

- 생성: [[concepts/conditional-rule-loading]], [[entities/jimcoding]]
- 갱신: [[concepts/claude-md]] (비대화 실패 모드 + rules 디렉터리 연결), [[concepts/context-engineering]] (lazy loading 구체 구현 사례), [[concepts/claude-skills]] (대비 — glob 기반 자동 vs 의미 기반 트리거)

## 관련 페이지

- [[concepts/conditional-rule-loading]] — 이 영상의 핵심 개념 추출
- [[concepts/claude-md]] — 비대화 실패 모드 출발점
- [[concepts/context-engineering]] — Lazy loading 의 구체 구현
- [[concepts/harness-engineering]] — 보안 규칙 강제의 하네스 관점
- [[entities/jimcoding]] — 저자

## 출처

- 원본 YouTube: https://youtu.be/FBv8hK_DtJ8
- 전사: `raw/jimcoding-claude-rules-transcript.md`
- 자막 방식: yt-dlp 자동자막 (ko) — 전사 품질상 일부 음성 인식 오타 존재 ("제고" → "재고", "기제" → "기재", "오프스" → "Opus" 등)

> [!question] Reddit 47K 단어 사례 원글, Anthropic docs 공식 frontmatter 필드명
> 둘 다 영상에서 직접 링크/명시 없음. 추후 웹 검색으로 보강.
