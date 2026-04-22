---
title: Advisor 패턴 노트
type: source
tags: [agent-architecture, claude-code, cost-optimization, advisor]
created: 2026-04-22
updated: 2026-04-22
origin: raw/Advisor.md
---

# Advisor 패턴 노트 — 소스 요약

> 원본: `raw/Advisor.md`. 저자는 사용자 본인. Anthropic 의 Advisor 패러다임을 자기 언어로 정리.

## 한 줄 요약

**작은 모델(Executor)이 전체 작업을 주도하고, 고차원 판단이 필요한 순간에만 큰 모델(Advisor)에게 조언을 요청**하는 계층형 에이전트 아키텍처. Sonnet 단독보다 성능 ↑, Opus 단독보다 비용 ↓.

## 핵심 주장

1. **기존 딜레마**: Opus(성능 ↑ 비싸다) vs Sonnet(충분 저렴) — 둘 중 하나 선택
2. **Advisor 해법**:
   - Executor = Sonnet, 전체 실행
   - Advisor = Opus, **조언만**. 도구 사용 X, 사용자 출력 X
   - **단일 API 통합** — 복잡한 오케스트레이션 불필요
3. **서브에이전트와의 차이** — 오케스트레이터 방향이 반대:
   - 서브에이전트: Top-down (Opus → 작업 분배 → 작은 모델 실행)
   - Advisor: Bottom-up (Sonnet 실행 → 필요 시 Opus 조언 호출)
4. **비용 구조**: 서브에이전트는 전체에 Opus 요금. Advisor 는 **판단 순간(10~20%)에만** Opus 요금.
5. **권장 호출 타이밍**:
   - 작업 **시작 전** — 방향 설정·접근법 검토
   - 작업 **완료 후** — 결과 검증·누락 점검
   - **막혔을 때** — 반복 오류·방향 전환
6. **Claude Code 커맨드**: `/advisor` — Advisor 를 Opus, 기본 모델을 Sonnet 4.6 으로 설정

## 핵심 인용

> "정답은 더 큰 모델이 아니라, 더 똑똑한 시스템 아키텍처다." — Anthropic (l.61)

## 아키텍처 패러다임 전환

- 계층형 모델 조합 — 역할에 맞는 모델
- 판단 밀도에 따른 모델 등급 동적 배분
- **단일 최강 모델 < 협력하는 모델 시스템**

## 기존 위키와의 관계

이 소스는 기존 [[sub-agents]] 페이지의 주장을 **보완·확장**한다. 서브에이전트가 "독립 컨텍스트 병렬 실행" 측면에서 좋다면, Advisor 는 "**비용 구조**" 측면에서 sub-agent 대비 우위. 두 패턴은 상호 대립이 아닌 **상호보완**:

| 상황 | 적합한 패턴 |
|---|---|
| 넓은 탐색·병렬 검증 | [[sub-agents]] |
| 고차 판단만 Opus 필요 | **Advisor** |
| 단순 실행 | 메인 에이전트만 |

## 파생된 위키 페이지

- Concepts: [[advisor-pattern]], [[model-routing]]
- Updated: [[sub-agents]] (대조 섹션), [[claude-code]] (/advisor 커맨드)

## 스크린샷 (미해석, 추후 OCR)

- `![[스크린샷 2026-04-21 오후 2.46.42.png]]` — Advisor 동작 방식
- `![[스크린샷 2026-04-21 오후 2.47.50.png]]` — Advisor 동작 방식
- `![[스크린샷 2026-04-21 오후 2.54.36.png]]` — Advisor vs 서브에이전트
- `![[스크린샷 2026-04-21 오후 2.55.27.png]]` — Advisor vs 서브에이전트
