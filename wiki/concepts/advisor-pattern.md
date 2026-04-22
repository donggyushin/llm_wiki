---
title: Advisor 패턴
type: concept
tags: [agent-architecture, cost-optimization, claude-code, pattern]
created: 2026-04-22
updated: 2026-04-22
sources: [[advisor-pattern-note]]
---

# Advisor 패턴

## 정의

에이전트 아키텍처의 한 형태로, **실행 주도권은 작은·저렴한 모델(Executor)**이 쥐고, **고차원 판단이 필요한 순간에만 크고 비싼 모델(Advisor)**을 호출해 **조언만** 받는 Bottom-up 구조.

## 핵심 규칙

1. Executor 는 전체 작업을 **단독 실행** (도구 사용·파일 편집·출력 생성 전부)
2. Advisor 는 **조언만** 반환. 원 소스 l.24:
   > "도구를 사용하지 않고, 사용자에게 출력물도 생성하지 않음"
3. **단일 API 통합** — 복잡한 멀티 에이전트 오케스트레이션 불필요

## 왜 작동하는가

| 구분 | 이유 |
|---|---|
| 성능 | 작은 모델이 막히는 **10~20% 판단 순간**을 큰 모델이 커버 → Sonnet 단독 대비 ↑ |
| 비용 | 큰 모델 호출이 전체의 10~20% → Opus 단독 대비 ↓ |
| 단순성 | 메인 모델이 서브를 관리하는 오케스트레이션 부담 없음 |

## Sub-Agent 패턴과의 대조

| 구분 | 서브에이전트 | **Advisor** |
|---|---|---|
| 오케스트레이터 | Opus (큰 모델) | **Sonnet (작은 모델)** |
| 흐름 방향 | Top-down 분배 | **Bottom-up 요청** |
| 호출 트리거 | 작업 시작 시 분해 | **실행 중 판단 필요 순간** |
| 전체 비용 | Opus 요금 + 서브 요금 | **대부분 Sonnet + 판단시 Opus** |
| 적합 상황 | 넓은 병렬 탐색 | **깊은 판단·검증 보강** |

→ 두 패턴은 **상호보완**. 상세 [[sub-agents]] 참조.

## Claude Code 에서 사용

```
/advisor
```

→ Advisor 모델 = Opus, 기본(Executor) 모델 = Sonnet 4.6 설정.

## 권장 호출 타이밍

1. **작업 시작 전** — 방향 설정, 접근법 검토 (Plan 과 결합)
2. **작업 완료 후** — 결과물 검증, 누락 점검
3. **막혔을 때** — 반복 오류, 방향 전환 고려

> 📌 [[plan-mode-workflow]] 의 "Plan → 검토 → Execute" 패턴에서 **검토 단계**를 Advisor 가 자동화하는 셈.

## 패러다임 선언 (Anthropic)

> "정답은 더 큰 모델이 아니라, 더 똑똑한 시스템 아키텍처다."

→ [[model-routing]] 로 일반화.

## 관련 개념

- [[model-routing]] — 역할에 맞는 모델 선택 일반 원칙
- [[sub-agents]] — 상호보완 패턴
- [[plan-mode-workflow]] — Plan 단계에서의 자연스러운 접점
- [[context-as-king]] — 비용·토큰 절감 전술로서의 위치

## 근거 소스

- [[advisor-pattern-note]]
