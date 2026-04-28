---
title: "Deep Modules — 단순 인터페이스 + 풍부한 구현"
type: concept
tags: [software-design, architecture, ai-coding, modularity]
created: 2026-04-28
updated: 2026-04-28
sources: [matt-pocock-software-fundamentals]
aliases: ["deep modules", "shallow modules", "깊은 모듈", "얕은 모듈"]
---

# Deep Modules

## 핵심 요약

[[entities/john-ousterhout]] *Philosophy of Software Design* 의 핵심 처방. **인터페이스는 단순하게, 그 뒤에 많은 기능을 숨겨라** (*deep*). 반대 — 작고 많은 모듈에 복잡한 인터페이스가 노출된 코드베이스 (*shallow*) 는 추상화의 비용만 부담하고 이득은 없다. AI 코딩 맥락에서 [[sources/matt-pocock-software-fundamentals]] 는 이 개념을 **AI 가 길을 잃지 않는 코드베이스** 의 처방으로 끌어온다 — 깊은 모듈은 **테스트 가능한 경계** 를 제공하고, 사람은 인터페이스만 설계, 안쪽은 AI 에게 위임할 수 있다.

## 정의

| 모듈 종류 | 인터페이스 | 안쪽 기능 | 결과 |
|---|---|---|---|
| **Deep** | 단순, 작은 | 풍부 | 복잡성을 *내부에 숨김*. 사용자는 인터페이스만 본다. |
| **Shallow** | 복잡, 큰 | 빈약 | 추상화 비용 + 노출된 복잡성 = 최악. |

> 깊은 모듈을 들여다볼 *수* 는 있지만, 그럴 *필요* 는 없다.

## AI 코딩에서의 의미

### Shallow 코드베이스에서 AI 가 길을 잃는 패턴

- 작은 모듈이 많고 의존성이 거미줄.
- AI 가 코드베이스를 탐색해도 의존성을 다 따라잡지 못해 *잘못된 모듈* 에 코드를 짜거나 *모르는* 채로 작업.
- AI 가 *얕은 모듈만 잔뜩 만든 코드베이스를 만드는 데 능숙하다* — 따라서 의식적인 개입이 없으면 자연스럽게 shallow 한 결과로 수렴.

### Deep 코드베이스의 보상

- 인터페이스는 사람이 직접 설계, 매우 신중하게 — 잘못된 인터페이스는 AI 가 망친다.
- 구현은 AI 에게 *gray box* 로 위임 가능. **인터페이스 외부에서 테스트** 하면 안쪽 구현을 일일이 검토할 필요가 줄어든다.
- TDD 가 보상받는 구조 — 모듈 경계가 단순하니 테스트 짜기 쉬움.

## "Improve Codebase Architecture" 스킬 (Pocock)

shallow → deep 변환은 단번에 안 됨. 반복 가능한 절차:

1. 코드베이스를 탐색해 *관련된* 코드 (자주 함께 변하는, 같은 도메인 개념을 다루는) 식별.
2. 그 묶음을 deep module 로 감싼다 — 단순한 외부 인터페이스 정의.
3. 내부 구현은 그대로 두거나 AI 가 정돈.
4. 인터페이스 위에서 테스트 작성.
5. 다음 묶음으로.

## 다른 위키 페이지와의 결합

### [[concepts/harness-engineering]] — *코드 레벨 하네스*

하네스가 *프로젝트 환경* 에서 AI 가 어길 수 없는 울타리라면, deep module 인터페이스는 *코드 내부* 에서 AI 가 어길 수 없는 경계. 같은 사상의 다른 층:

- 외부 환경: 린터, architecture test, file system 권한 → [[concepts/harness-engineering#4대-기둥]].
- 내부 코드: deep module 의 단순 인터페이스.

### [[concepts/code-is-not-cheap]] — 테제와의 직접 연결

깊은 모듈로 짜인 코드베이스 = *변경하기 쉬운* 코드베이스 = AI 가 빛나는 코드베이스. *code is cheap* 를 **사실로 만들 수 있는 유일한 길** — 그러나 그 길은 펀더멘털을 통한다.

### [[concepts/reward-hacking]] — 테스트 신뢰도

deep module 인터페이스가 단순하면 테스트가 단순해지고, 테스트가 단순해지면 사람이 *테스트 자체* 를 검토하기 쉬워진다 → reward hacking 방어선이 두꺼워진다.

## Tip 5 — Design the interface, delegate the implementation

> 인터페이스를 설계하라. 구현은 위임하라.

조건:

- 모듈 외부에서 테스트 가능한 경계가 있어야 한다.
- 모듈의 *목적* 을 이해해야 한다 — 외부에서 설계할 만큼.
- 금융처럼 critical 한 영역은 안쪽 구현도 직접 검토.

이렇게 하면 사람의 뇌도 살아난다 (Pocock 의 5번 실패 모드 처방).

> "Invest in the design of the system every day." — [[entities/kent-beck]]

특히 인터페이스 설계는 매일 다듬어야 할 자본.

## 관련 페이지

- [[sources/matt-pocock-software-fundamentals]] — 출처
- [[entities/john-ousterhout]] — 개념 원작자
- [[entities/kent-beck]] — 인터페이스 설계 일상 투자
- [[concepts/code-is-not-cheap]] — 이 처방의 상위 테제
- [[concepts/harness-engineering]] — 같은 사상의 환경 층
- [[concepts/reward-hacking]] — deep module 이 강화하는 방어선
- [[concepts/claude-skills]] — *improve codebase architecture* 스킬

## 출처

- [[sources/matt-pocock-software-fundamentals]] — Ousterhout 인용 + Pocock 의 스킬 + Tip 5
