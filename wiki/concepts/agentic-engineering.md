---
title: "Agentic Engineering — 말 자체를 훈련시키기"
type: concept
tags: [agentic-engineering, ai-engineering, agents]
created: 2026-04-22
updated: 2026-04-22
sources: [harness-engineering-era]
aliases: ["Agentic Engineering", "에이전틱 엔지니어링", "말 훈련"]
---

# Agentic Engineering

## 핵심 요약

**AI 가 실행하고 인간이 설계·검증·책임지는 규율 있는 협업 방식**. AI 에이전트 자체를 **더 강하고 똑똑하게 만드는 기술**. 추론 루프 설계 · 멀티에이전트 조율 · 도구 사용법 전수가 핵심. 말과 마구 비유에서 **말을 훈련시키는 쪽**. [[concepts/harness-engineering]] 과 **상호보완** — 잘 훈련된 말도 마구 없이는 밭 못 갈고, 정교한 마구도 말이 없으면 수레가 안 움직인다.

## 초점

| 관점 | Agentic | Harness |
|---|---|---|
| 대상 | AI 자체 | AI 환경 |
| 핵심 질문 | "AI 가 **어떻게 생각하는가**" | "AI 가 **무엇을 할 수 있는가 / 없는가**" |
| 실패 시 | 프롬프트 수정, 추론 루프 조정 | 규칙·테스트 자동 추가 |
| 인간 역할 | 위임자 · 감독자 | 설계자 · 경계 설정자 |

## 주요 구성 요소

### 1) 추론 루프 (Reasoning Loop)

AI 가 계획 → 실행 → 관찰 → 수정을 반복하는 사이클. 설계 포인트:
- 단계별 검증 시점
- 오류 복구 전략
- Self-healing 메커니즘

### 2) 멀티에이전트 조율

여러 에이전트가 협업하는 패턴 설계:
- **격리** — 각자 별도 컨텍스트
- **병렬** — 독립 작업 동시 실행
- **체인** — 한 에이전트 결과를 다음에 전달
- **Agent Teams** — 상호 통신

→ [[concepts/subagents]] 의 4대 패턴과 직결.

### 3) 도구 사용법 학습

AI 가 어떤 도구를 언제 어떻게 써야 할지 가르치는 것:
- Skills 설계 → [[concepts/claude-skills]]
- MCP 서버 통합
- 커스텀 플러그인

## 하네스와의 관계

> 말과 마구가 따로 존재할 수 없다.

- 잘 훈련된 말 + 마구 無 → 방향 없이 달리다 울타리 부숨
- 정교한 마구 + 말 無 → 수레가 안 움직임
- **둘 다 있어야 밭이 갈린다**

Agentic 엔지니어링은 **추진력**, Harness 엔지니어링은 **제어**. 둘의 곱이 생산성.

## 실전 질문

- 에이전트가 문제를 해결하지 못할 때 → **Agentic** 개선 (더 나은 루프/도구/프롬프트)
- 에이전트가 엉뚱한 방향으로 가고 위험한 짓을 할 때 → **Harness** 개선 (경계/검사 추가)

## 관련 페이지

- [[concepts/harness-engineering]] — 짝 개념
- [[concepts/four-axes-ai-development]] — 4축 중 최상위
- [[concepts/subagents]] — 멀티에이전트 조율의 실체
- [[concepts/claude-skills]] — 도구 사용법 전수
- [[concepts/plan-mode]] — 추론 루프의 일부

## 출처

- [[sources/harness-engineering-era]] — "Agentic vs Harness" 구분 섹션
- [[sources/claude-code-2h-mastery]] — 심화편에서 "에이전틱 엔지니어" 로의 상향 이동 언급
