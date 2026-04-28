---
title: "Domain-Driven Design (DDD) — 도메인 모델을 코드의 중심에"
type: concept
tags: [ddd, domain-driven-design, software-design, eric-evans]
created: 2026-04-28
updated: 2026-04-28
sources: [matt-pocock-software-fundamentals]
aliases: ["DDD", "Domain-Driven Design", "도메인 주도 설계"]
---

# Domain-Driven Design (DDD)

## 핵심 요약

**Eric Evans** *Domain-Driven Design: Tackling Complexity in the Heart of Software* (2003) 의 설계 접근. **소프트웨어의 핵심 가치는 도메인의 복잡성을 잘 표현하는 데 있다** 는 전제 위에, 도메인 전문가와 개발자 사이의 *모델 정렬* 을 1순위로 두는 일련의 패턴 모음. 본 위키 맥락에서는 [[concepts/ubiquitous-language]] 의 **상위 우산** 으로, [[sources/matt-pocock-software-fundamentals]] 가 AI ↔ 사용자 어휘 정렬에 끌어다 쓴다.

## 핵심 패턴 (이 위키 범위)

### 1) Ubiquitous Language (편재 언어)

도메인 전문가, 개발자, 코드가 *같은 어휘* 를 공유. → [[concepts/ubiquitous-language]] 별도 페이지.

> AI 시대 재해석: AI 도 *도메인 전문가* 의 한 종류 — 사용자와 어휘가 다르면 verbose 해진다. 마크다운 표 한 장이 어휘 정렬의 산출물.

### 2) Bounded Context (경계 맥락)

같은 단어가 다른 도메인에서 다른 의미를 갖는다. *Customer* 는 영업 컨텍스트와 회계 컨텍스트에서 다른 모델. 각 컨텍스트 내부에서만 ubiquitous language 가 통일된다.

AI 코딩 함의: 모노레포가 여러 도메인에 걸치면 ubiquitous language 마크다운도 컨텍스트별로 분리. → [[concepts/conditional-rule-loading]] 의 *파일 패턴별 규칙 로드* 와 결이 같다 (도메인 단위 lazy loading).

### 3) Domain Model (도메인 모델)

비즈니스 규칙을 *코드의 1차 시민* 으로 표현. 데이터 객체와 분리. **Entities, Value Objects, Aggregates, Repositories** 같은 빌딩 블록이 표준화되어 있다.

본 위키 범위에서는 깊이 다루지 않음 — *Pocock 발표가 ubiquitous language 만 인용* 하기 때문. 신규 소스가 들어오면 별도 페이지로 확장.

### 4) Strategic Design

여러 bounded context 사이의 관계 (Customer-Supplier, Conformist, Anti-corruption Layer 등). 마이크로서비스 경계 설계에 영향. 본 위키 범위 밖 (현재).

## 본 위키 클러스터에서의 위치

```
DDD (이 페이지)
├── Ubiquitous Language → [[concepts/ubiquitous-language]] (Pocock 의 인용 핵심)
├── Bounded Context (간략 언급)
├── Domain Model (외부 참조)
└── Strategic Design (외부 참조)
```

## AI 코딩에서의 의미

### Pocock 의 재해석

[[sources/matt-pocock-software-fundamentals]] 는 DDD 전체를 가져온 게 아니라 **ubiquitous language 한 패턴만** 끌어다 쓴다. 그 한 패턴이 AI verbose 문제의 처방으로 드러난 것 자체가 시사 — DDD 의 다른 패턴들도 AI 코딩 맥락에서 *재해석 여지* 가 크다:

| DDD 패턴 | AI 코딩 가능성 |
|---|---|
| Ubiquitous Language | ✅ 이미 입증 (Pocock) |
| Bounded Context | 모노레포 + 멀티 컨텍스트 코딩 시 어휘 충돌 방지 |
| Anti-corruption Layer | 외부 API 응답을 내 도메인 모델로 변환 — AI 가 자주 망치는 지점 |
| Aggregates | deep module 의 도메인 버전 |

→ [[concepts/deep-modules]] 와 *aggregate* 의 유사성은 향후 ingest 시 추적 가치 있음.

### Ubiquitous Language ↔ Design Concept

| | [[concepts/ubiquitous-language]] | [[concepts/design-concept]] |
|---|---|---|
| 출처 | DDD (Eric Evans) | Brooks *Design of Design* |
| 공유 대상 | **어휘** | **설계 이해** |
| 산출물 | 마크다운 표 | (자산 아님 — 합의된 대화) |
| 도구 | `ubiquitous language` 스킬 | `grill me` 스킬 |

DDD 와 Brooks 가 같은 *정렬* 작업의 **두 면** 을 다룬다.

## 관련 페이지

- [[concepts/ubiquitous-language]] — DDD 의 가장 핵심 패턴 (이 위키에서 우선 다룸)
- [[concepts/design-concept]] — 어휘 정렬의 자매 작업 (Brooks)
- [[concepts/deep-modules]] — DDD 의 *aggregate* 와 인접
- [[sources/matt-pocock-software-fundamentals]] — DDD 를 AI 코딩에 끌어다 쓴 1차 사례
- [[concepts/code-is-not-cheap]] — 같은 펀더멘털 군의 일원
- [[concepts/conditional-rule-loading]] — bounded context 의 파일 단위 구현 비유

## 출처

- [[sources/matt-pocock-software-fundamentals]] — DDD / ubiquitous language 직접 인용
- *(Eric Evans, *Domain-Driven Design* (2003) 인용 페이지 미명시 — P3.)*
