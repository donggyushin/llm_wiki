---
title: 계층형 모델 라우팅 (Model Routing)
type: concept
tags: [architecture, cost-optimization, models, meta-principle]
created: 2026-04-22
updated: 2026-04-22
sources: [[advisor-pattern-note]]
---

# 계층형 모델 라우팅

## 정의

에이전트 시스템 설계에서 **단일 최강 모델**을 전체 작업에 쓰지 않고, **역할과 판단 밀도에 맞춰 여러 모델 등급을 동적으로 배분**하는 아키텍처 원칙.

## 선언적 명제 (Anthropic)

> "정답은 더 큰 모델이 아니라, 더 똑똑한 시스템 아키텍처다."
> — [[advisor-pattern-note]] l.61

## 핵심 전제

1. **모든 토큰이 동일한 가치가 아니다** — 단순 실행 토큰과 전략적 판단 토큰은 가치 편차가 크다
2. **비용은 성능의 제약** — 항상 최상위 모델을 쓰면 확장 불가
3. **아키텍처가 모델 선택보다 먼저** — 작업 구조를 먼저 쪼갠 뒤 각 슬롯에 맞는 모델 배치

## 구현 패턴들

| 패턴 | 모델 배치 | 적합 |
|---|---|---|
| [[advisor-pattern\|Advisor]] | Sonnet Executor + Opus Advisor | 판단 순간만 고급 모델 |
| [[sub-agents\|Sub-Agent]] | Opus 메인 + Sonnet 서브 | 넓은 병렬 탐색 |
| Fast/Slow 모드 | Haiku 초안 + Sonnet/Opus 다듬기 | 대량 반복 작업 |
| Skills per role | 에이전트별 권한·지식 분리 | 조직적 Agent Teams |

## 판단 밀도 맵

| 판단 밀도 | 예시 작업 | 추천 모델 |
|---|---|---|
| 낮음 | 보일러플레이트·단순 CRUD | Haiku 또는 Sonnet |
| 중간 | 일반 개발·리팩터링 | Sonnet |
| 높음 | 아키텍처 결정·보안 리뷰 | Opus (Advisor 로 호출) |

## 위키 시스템 내 라우팅

현재 이 위키의 ingest/query/lint 는 단일 모델이 수행. 향후 확장:

- **ingest** (Sonnet) — 페이지 생성·파일링
- **라우팅 판단·모순 감지** (Opus via Advisor) — 높은 판단 밀도
- **index 업데이트** (Haiku) — 단순 기계 작업

> 추론: 이 vault 에 적용 시 `/advisor` 를 켜두고 ingest 시 라우팅 결정만 Opus 에 위임하는 게 자연스러운 첫걸음.

## 관련 개념

- [[advisor-pattern]] — 이 원칙의 대표 구현
- [[wat-framework]] — A(Agents) 층에서 라우팅 결정
- [[context-as-king]] — 비용·토큰 최적화 관점의 동료 원칙

## 근거 소스

- [[advisor-pattern-note]] l.59-67
