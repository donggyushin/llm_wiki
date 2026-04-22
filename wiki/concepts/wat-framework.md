---
title: WAT 프레임워크 (Workflows · Agents · Tools)
type: concept
tags: [framework, claude-code, agents, tools, workflow]
created: 2026-04-22
updated: 2026-04-22
sources: [[claude-code-study-notes]]
---

# WAT 프레임워크

Claude Code 에서 일을 조합하는 3 층 멘탈 모델. 각 층은 **관심사 분리**로 서로 다른 생산성 자원을 다룬다.

## 구성

### W — Workflows (작업 흐름)

- **핵심 도구**: `todo.md` (로컬 마크다운)
- 장기·중기 작업의 단계·의존성을 문서로 박제
- Claude 가 스스로 "다음 할 일"을 참조할 수 있도록
- 출처 원문: *"작업 흐름을 정의 → AI가 스스로 판단하고 복구 → 도구 조합"* (l.112)

### A — Agents (에이전트)

- **핵심 도구**: `/agents` 명령, 서브에이전트, [[sub-agents]]
- **장점**: 메인 컨텍스트와 분리된 독립 컨텍스트 → 컨텍스트 오염 방지
- **권장 모델**: Sonnet (opus 비권장, 병렬 서브에이전트 시 특히)
- 스크린샷: `![[스크린샷 2026-04-17 오후 3.20.42.png]]` (WAT Agents 상세)

→ [[sub-agents]]

### T — Tools (도구)

- **핵심 도구**: MCP·Skills·bash 스크립트·커스텀 도구
- [[skills]] 가 간단한 작업에는 MCP 보다 더 가벼운 선택
- [[mcp]] 는 양날의 검 — 커스텀 MCP 가 최적
- 스크린샷: `![[스크린샷 2026-04-17 오후 3.21.17.png]]` (WAT Tools 상세)

## 왜 이 분리가 중요한가

각 층이 **서로 다른 자원**을 다룸:

| 층 | 다루는 자원 | 실패 모드 |
|---|---|---|
| Workflows | **기억 · 지속성** | todo.md 없으면 세션 간 단절 |
| Agents | **컨텍스트 · 병렬성** | 메인에 다 밀어넣으면 오염·윈도우 터짐 |
| Tools | **능력 · 외부 세계** | 과도 연결 시 시작부터 컨텍스트 잠식 |

## 이 위키와의 대응

- 이 위키 시스템 = Workflows 의 구체적 인스턴스 (Ingest/Query/Lint 가 `todo.md` 의 자리를 대신)
- [[wiki-operations]] — 위키 Ops 자체가 WAT 의 W 에 해당
- [[three-layer-architecture]] — 본 프레임워크와 유사하게 **관심사를 3 층으로 분리**한다는 점에서 정신적 동류

## Harness Engineering 과의 차이

[[harness-engineering]] 는 **통제·규율**에, WAT 는 **조합·구성**에 방점. 같은 재료(CLAUDE.md · Hooks · Agents · Skills · MCP)를 서로 다른 앵글로 정리. 실전에선 둘 다 필요:

- WAT 관점: "이 기능을 **어떻게 조립**할까?"
- Harness 관점: "AI 가 **어떻게 못하게** 만들까?"

## 관련 개념

- [[harness-engineering]] — 동일 재료의 통제 축
- [[context-as-king]] — WAT 의 궁극 목적은 컨텍스트 최적화
- [[plan-mode-workflow]] — Workflows 의 가장 기본 루프

## 근거 소스

- [[claude-code-study-notes]] l.109-124
