---
title: "프롬프트 엔지니어링은 끝났습니다: 이제 '하네스'의 시대입니다"
type: source
tags: [harness-engineering, ai-engineering, video, 한국어]
created: 2026-04-22
updated: 2026-04-22
author: "실벨 개발자"
medium: video
raw_path: "raw/프롬프트 엔지니어링은 끝났습니다： 이제 '하네스'의 시대입니다.ko.vtt"
aliases: ["harness 시대", "하네스 엔지니어링 영상"]
---

# 프롬프트 엔지니어링은 끝났습니다: 이제 '하네스'의 시대입니다

## 메타

- **유형**: YouTube 단편 강의
- **URL**: https://youtu.be/6gvnDSAcZww
- **채널**: 실벨 개발자 (`[[entities/silbel-developer]]`) — [[sources/claude-code-2h-mastery]] 와 동일 저자
- **원본**: `raw/프롬프트 엔지니어링은 끝났습니다： 이제 '하네스'의 시대입니다.ko.vtt`
- **전처리**: `raw/harness-engineering-era-transcript.md` (중복 제거, 558 라인)
- **핵심 인용 인물**: Martin Fowler ([[entities/martin-fowler]])

## 핵심 요약

AI 개발의 네 가지 축 — **Prompt → Context → Harness → Agentic Engineering** — 을 정리하고, 그중 **Harness Engineering** 에 집중 해설한 강의. 핵심 은유는 **말(Agentic)과 마구(Harness)**. AI 에이전트가 아무리 강력해도 마구 없이는 방향을 못 잡고 울타리를 부순다. 실패 시 프롬프트를 수정하는 대신 **하네스(시스템) 를 수정해 그 실수를 구조적으로 불가능하게** 만들어야 한다.

## 주요 주장 · 데이터 · 인용

### 4축 프레임워크

저자가 여러 레퍼런스를 종합한 프레임. 순차적 졸업 구조가 아닌 **상호보완**:

1. **Prompt Engineering** — AI 에게 말을 잘 거는 기술. 천장 존재.
2. **Context Engineering** — 프로젝트 구조/코드/API 문서/규칙을 적절량 제공 (Anthropic 정의: "많이 주는 게 아니라 필요한 것만")
3. **Harness Engineering** — 가드레일·테스트 게이트·도구 경계
4. **Agentic Engineering** — 추론 루프, 멀티에이전트 조율, 도구 사용법 가르치기

### 말과 마구 비유

| | Agentic | Harness |
|---|---|---|
| 무엇 | 말을 훈련 | 마구 제작 |
| 대상 | AI 자체 | AI 가 일할 환경 |
| 초점 | "어떻게 생각하는가" | "무엇을 할 수 있는가 / 없는가" |
| 실패 시 | 프롬프트/루프 조정 | 규칙·테스트 자동 추가 |
| 인간 역할 | 위임자·감독자 | 설계자·경계 설정자 |

> "말을 아무리 잘 훈련시켜도 마구 없이는 밭을 갈 수 없다."

### 하네스 철학 (가장 중요한 문장)

> "에이전트가 규칙을 어겼을 때 '더 잘해 봐' 라고 프롬프트를 고치는 게 아니다. 그 실패가 **구조적으로 반복 불가능**하도록 하네스를 고치는 것이다."

**예시**: 프론트엔드가 DB 를 직접 호출 → 프롬프트에 "DB 직접 호출하지 마" 추가 (❌ 부탁) → 아키텍처 테스트로 프론트엔드 폴더에서 DB import 시 **빌드 실패** (✅ 강제).

### Harness 4대 기둥

1. **Context Files** — `CLAUDE.md`, `AGENTS.md`, Cursor의 `.cursorrules`. **AI 실행 런타임 설정**. 사람이 읽는 문서가 아니라 AI 가 작업 시작 시 먼저 읽는 제약 정의.
2. **System Enforcement** — Linter (맞춤법 검사기), 구조 테스트 (폴더 간 import 금지), Pre-commit Hook. 실패 시 에이전트가 스스로 자동 수정.
3. **Tool Boundaries** — 물리적 차단. 파일 시스템 (폴더별 read/write), API (내부 O / 외부 X), DB (SELECT O / DROP X), 터미널 whitelist.
4. **Garbage Collection** — Martin Fowler 용어. 나쁜 패턴 자동 감지, 중복 코드 탐지, 리팩토링 PR 자동 생성, 데드 코드 제거. **AI 가 나쁜 패턴을 복제하며 눈덩이처럼 불어나는 현상** 방지.

### Harness 시스템 4부품 (실행 구조)

1. **Router (분류기)** — 마구의 곱비. 요청을 AI 에 바로 넘기지 않고 단순 질문 / 실제 작업 / 모호한 요청 분류.
2. **Context Manager (가림막)** — 전체 코드 노출 대신 지금 작업에 필요한 파일/규칙만.
3. **Execution Loop (심장)** — 코드 생성 → 테스트 자동 실행 → 통과 or 실패 피드백 → 재수정. **사용자 개입 불필요한 셀프 루프**.
4. **Worker Isolation** — 코드 작성 AI 와 코드 리뷰 AI 분리. "같은 사람이 쓰고 같은 사람이 검토하면 실수 못 잡는다."

### 진화적 특성

에이전트가 실수할 때마다 그 실수가 **새로운 규칙** 이 됨 → 린터 룰 추가 → 테스트 추가 → 제약 추가. 시간이 지날수록 하네스가 **점점 정교**. "말이 한 번 실수한 곳엔 울타리가 세워진다."

### OpenAI 사례 (2026-02 공식 블로그)

엔지니어 **3명이 코드를 한 줄도 쓰지 않고 5개월** 만에 대규모 소프트웨어 제품 배포. 저자 분석 — 하네스 관점에서 4요소:

1. `agents.md` 로 AI 지침서 설계
2. CI/CD 게이트 (lint + test + hook)
3. Tool 접근 범위/권한 사전 설정
4. 피드백 루프 (AI 코딩 → 리뷰 → 규칙 보강)

> "인간은 조종한다. 에이전트는 실행한다." — OpenAI

조종 = harness.

### 역사적 배경

`test harness` 개념은 1970년대 소프트웨어 엔지니어링부터 존재 (다양한 조건에서 프로그램 실행·모니터링). 전통 SW 는 static/deterministic 이라 별도 하네스 엔지니어 불필요. LLM 은 환각·자신감 넘치는 오류로 **오래된 개념에 새 의미** 부여.

## 내가 주목한 부분

- **"프롬프트는 부탁이고 도구 경계는 물리적 차단"** — 많은 엔지니어가 system prompt 에 금지 조항 쌓는 안티패턴을 정확히 포인트.
- **Garbage Collection** 이 4기둥 중 한 축이라는 점 — 대부분의 AI 코딩 담론이 "생성" 에만 집중하는데, **지속 가능한 코드베이스 유지** 관점을 명시.
- **Worker Isolation** 이 [[concepts/subagents]] 의 실전 패턴과 직결. "쓰는 AI != 검토 AI" 원칙.
- 4축 프레임에서 **Context Engineering** 이 이미 기존 `[[concepts/context-engineering]]` 에 정립됨. 이번 영상은 그 **상위 관점** (4축 중 하나) 을 제공.

## 모순 · 주의

> [!note] 자막 오인식
> "하네스" → "한네스", "크라이드", "마리 훈련", "harness" 한/영 혼재. 본 위키는 영문 `Harness`, 한글 `하네스` 로 통일.

> [!question] OpenAI 블로그 날짜
> 영상에서 "2026년 2월에 OpenAI 공식 블로그" 언급. 출처 URL 미제공 → 추후 원문 추적 필요.

> [!question] Martin Fowler 체계화 시점
> 영상이 "마틴 파울러가 체계화한" 이라 명시하지만 구체 문서/블로그 포스트 미인용. `Garbage Collection` 용어 또한 Fowler 귀속인데 출처 미상 — [[entities/martin-fowler]] 페이지에 TODO 로 남김.

## 관련 페이지

신규:
- [[concepts/harness-engineering]]
- [[concepts/agentic-engineering]]
- [[concepts/four-axes-ai-development]]
- [[entities/martin-fowler]]
- [[entities/silbel-developer]]

기존 (관점 추가):
- [[concepts/context-engineering]] — 4축 중 2번째 축
- [[concepts/claude-md]] — 하네스 1기둥 실체
- [[concepts/hooks]] — 하네스 2기둥 메커니즘
- [[concepts/subagents]] — Worker Isolation 패턴 근거

## 출처

- 원본 영상: https://youtu.be/6gvnDSAcZww
- 자막 원본: `raw/프롬프트 엔지니어링은 끝났습니다： 이제 '하네스'의 시대입니다.ko.vtt`
- 정제본: `raw/harness-engineering-era-transcript.md`
