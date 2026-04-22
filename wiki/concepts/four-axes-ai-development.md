---
title: "AI 개발의 4축 — Prompt / Context / Harness / Agentic"
type: concept
tags: [ai-engineering, framework, meta]
created: 2026-04-22
updated: 2026-04-22
sources: [harness-engineering-era]
aliases: ["4축 프레임워크", "Four Axes", "AI 개발 축"]
---

# AI 개발의 4축

## 핵심 요약

실벨 개발자가 여러 레퍼런스를 종합해 정리한 **AI 를 활용하는 4가지 엔지니어링 축**. 순차적 졸업 구조가 **아니다** — 넷 다 필요한 **상호보완적 축**. 프로젝트가 성숙할수록 하위 축에서 상위 축으로 **투자 비중이 이동**하지만, 하위 축을 버리는 게 아니라 **누적된다**.

## 4축 전체 구조

```
[4] Agentic Engineering  — 말 자체를 훈련 (추론 루프, 멀티에이전트)
         ↑
[3] Harness Engineering  — 마구·울타리 (규칙 강제, 도구 경계)
         ↑
[2] Context Engineering  — 정보 큐레이션 (필요한 것만, 적절히)
         ↑
[1] Prompt Engineering   — 말 걸기 (지시를 잘)
```

## 각 축 요약

### 1) Prompt Engineering

**AI 에게 말을 잘 거는 기술**. "계산기 만들어 줘" → "공학용 계산기, sin/cos/log 지원, GUI 포함" 같이 구체화.

**천장**: 프로젝트의 기술 스택 · 코드 구조 · DB 스키마를 AI 가 모르면 아무리 정교한 프롬프트도 한계.

### 2) Context Engineering

Anthropic 정의: **"AI 가 일할 때 필요한 정보를 적절한 양으로 제공하는 기술"**.

핵심: **많이 주는 게 아니라 지금 필요한 것만 정확히**. 정보가 너무 많으면 오히려 성능 저하.

구성 요소:
- 프로젝트 구조
- 기존 코드
- 예시 코드
- API 문서
- 디자인 규칙

→ [[concepts/context-engineering]] 참조 (Lazy loading, 세컨드 브레인, MCP 다이어트, 세션 위생).

**한계**: 정보를 다 줘도 AI 가 엉뚱한 짓을 한다. 정보의 문제가 아니라 규칙·울타리의 문제.

### 3) Harness Engineering

**AI 에이전트가 자율적으로 일하되 안전하게 통제되는 환경을 만드는 기술**. Martin Fowler 가 체계화.

4대 기둥:
1. Context Files (`CLAUDE.md`, `AGENTS.md`)
2. System Enforcement (Lint, Architecture Test, Pre-commit Hook)
3. Tool Boundaries (물리적 접근 차단)
4. Garbage Collection (나쁜 패턴 자동 정리)

→ [[concepts/harness-engineering]].

### 4) Agentic Engineering

**AI 자체를 더 강하고 똑똑하게 만드는 기술**.

구성:
- 추론 루프 설계
- 멀티에이전트 조율 (격리·병렬·체인·Agent Teams)
- 도구 사용법 학습 (Skills, MCP)

→ [[concepts/agentic-engineering]].

## 축 사이 관계

### 서로 대체 불가

| 시도 | 결과 |
|---|---|
| Prompt 만 | 프로젝트 컨텍스트 0 → 일반적 코드만 |
| Context 까지만 | 정보 주고도 AI 가 엉뚱한 행동 |
| Harness 까지만 | 울타리만 있고 추진력 약함 |
| Agentic 까지만, Harness 없이 | 강력한 말이 통제 없이 폭주 |

**전부 다 필요**.

### 투자 이동 패턴

초반 프로젝트는 Prompt/Context 비중이 크다. 하지만 AI 자동화 수준이 높아질수록 **Harness/Agentic 이 지렛대**.

OpenAI 사례 (3명이 코드 0줄로 제품 배포) 가 보여주는 건: **하네스와 에이전틱에 극도로 투자한 결과**.

## 말과 마구 메타포로 다시 보기

| 축 | 비유 |
|---|---|
| Prompt | 말에게 명령을 잘 내리는 법 |
| Context | 말에게 지도와 주변 정보를 보여주기 |
| Harness | 마구 · 곱비 · 울타리 제작 |
| Agentic | 말의 근육과 지능 단련 |

## 개발자 역할의 상향 이동

| 시대 | 역할 |
|---|---|
| Pre-AI | 코드 직접 작성 |
| Prompt | AI 에 의도 정확히 전달 |
| Context | AI 참조 지식 체계 설계 |
| Harness | AI 가 실수할 수 없는 환경 설계 |
| Agentic | 에이전트 팀 오케스트레이션 |

**직접 공을 차는 선수 → 전술 짜고 팀을 운영하는 감독**.

## 관련 페이지

- [[concepts/context-engineering]] — 2번째 축
- [[concepts/harness-engineering]] — 3번째 축
- [[concepts/agentic-engineering]] — 4번째 축
- [[concepts/claude-md]] — Context + Harness 1기둥 교차
- [[concepts/subagents]] — Agentic 의 실체
- [[concepts/hooks]] — Harness 의 2기둥 메커니즘

## 출처

- [[sources/harness-engineering-era]] — 4축 프레임 원출처
