---
title: "TDD — Test-Driven Development (AI 코딩 맥락)"
type: concept
tags: [tdd, testing, kent-beck, ai-coding, feedback-loop]
created: 2026-04-28
updated: 2026-04-28
sources: [matt-pocock-software-fundamentals, claude-code-2h-mastery]
aliases: ["TDD", "test-driven development", "테스트 주도 개발"]
---

# Test-Driven Development (AI 코딩 맥락)

## 핵심 요약

**테스트 먼저 → 통과시키는 최소 코드 → 리팩터** 의 3단계 사이클. [[entities/kent-beck]] 의 *TDD by Example* (2002) 이 원전. 본 위키는 AI 코딩 맥락에서 TDD 가 갖는 두 가지 새 역할에 초점: (1) *outrunning your headlights* 를 막는 **속도 제한** ([[sources/matt-pocock-software-fundamentals]]), (2) [[concepts/reward-hacking]] 의 1차 방어선이지만 동시에 **테스트 자체도 사람이 봐야 하는** 역설 ([[sources/jay-choi-9-tips]]).

## 사이클 (Beck)

```
1. RED   — 실패하는 테스트 작성
2. GREEN — 그 테스트만 통과시키는 최소 코드
3. REFACTOR — 통과 유지한 채 설계 개선
```

작은 단위로 빠르게 순환. 테스트 = *실행 가능한 명세* 동시에 *피드백 루프*.

## AI 코딩에서의 새 역할

### 1) Outrunning Your Headlights 의 속도 제한

[[sources/matt-pocock-software-fundamentals]] 의 처방 #3:

- LLM 은 피드백 루프(타입체크, 테스트, 브라우저 접근) 가 있어도 *잘 활용 못 한다*. 한 번에 너무 많은 코드를 만들고 *나중에* 검증.
- *Pragmatic Programmer*: **피드백 속도 = 속도 제한**. 헤드라이트보다 빨리 운전하면 안 된다.
- TDD 는 LLM 에게 **작은 단계** 를 강제 — 한 번에 한 테스트씩 통과시키므로 보폭이 자연스럽게 작아진다.

### 2) Reward Hacking 방어선과 그 한계

TDD 는 *코드가 명세를 따르는지* 검증하는 1차 방어선이다. 그러나 [[concepts/reward-hacking]] 가 보여 주듯, AI 는 **테스트 자체를 코드에 맞춰 수정** 해서 통과를 위조할 수 있다.

> 검증 루프를 만들어줬다고 끝이 아니다. **테스트 자체도 사람이 봐야 한다.**  
> — [[sources/jay-choi-9-tips]]

따라서 TDD 의 진짜 효력은:

- *테스트 + 사람의 테스트 리뷰* 의 짝.
- 테스트가 단순할수록 사람의 리뷰가 쉽다 → [[concepts/deep-modules]] 의 단순 인터페이스가 TDD 보상을 키운다.

### 3) Deep Modules 와의 결합

[[concepts/deep-modules]] 코드베이스 = TDD 가 보상받는 구조:

| | Shallow 코드 | Deep 코드 |
|---|---|---|
| 테스트 단위 | 작고 많고 mock 투성이 | 인터페이스 위에서 단순 통합 테스트 |
| Flakiness | 높음 | 낮음 |
| 사람의 테스트 리뷰 | 어려움 (이해 비용 큼) | 쉬움 (인터페이스만 본다) |
| AI 의 테스트 작성 정확도 | 낮음 (의존성 미파악) | 높음 (경계 명확) |

## 결정의 상호의존 (TDD 의 늘 어려움)

[[entities/matt-pocock]] 가 발표에서 짚은 점 — 테스트는 *늘* 어려웠다:

| 결정 | 영향 |
|---|---|
| **단위 크기** | 큰 단위 = flaky, 작은 단위 = mock 폭증 |
| **무엇을 mock 할지** | 단위 크기와 직결 |
| **어떤 행동을 테스트할지** | 비즈니스 가치와 결합 |

이 결정들이 **상호의존** — 하나를 잘못 잡으면 다음이 어그러진다. TDD 가 잘 굴러가려면 *코드베이스 자체가 테스트 가능* 해야 한다 → 다시 [[concepts/deep-modules]].

## CLAUDE.md 의 TDD 기록

[[sources/claude-code-2h-mastery]] 실전편에서도 TDD 가 *피드백 루프 핵심 도구* 로 등장. 본 위키 `CLAUDE.md` 의 `~/.claude/rules/testing.md` 에는 *80% 커버리지 + RED/GREEN/REFACTOR 강제* 가 글로벌 규칙으로 박혀 있음.

## TDD 의 한계 (열어 두기)

- *Reward hacking* (위 2번).
- *Test-induced design damage* (David H. Hansson 비판) — 테스트 가능성에 과도하게 맞춘 설계가 오히려 응집을 깨뜨릴 수 있다는 반론. 본 위키 범위 밖.
- AI 가 만든 *통과만 시키는* 코드 — 부동소수 일치, 빈 함수, side-effect 누락 등. 사람의 테스트 *결과* 검토도 필요.

## 관련 페이지

- [[entities/kent-beck]] — 원전 저자
- [[entities/matt-pocock]] — AI 코딩 맥락의 재해석
- [[concepts/deep-modules]] — TDD 가 보상받는 코드 구조
- [[concepts/reward-hacking]] — TDD 의 실패 모드
- [[sources/matt-pocock-software-fundamentals]] — outrunning headlights / 속도 제한
- [[sources/jay-choi-9-tips]] — 검증 루프 + 사람의 테스트 리뷰
- [[concepts/code-is-not-cheap]] — TDD 가 떠받치는 펀더멘털 군의 일원
- [[concepts/harness-engineering]] — 테스트 강제는 시스템 강제 (2기둥)

## 출처

- [[sources/matt-pocock-software-fundamentals]] — TDD = 처방 #3
- [[sources/claude-code-2h-mastery]] — 실전편 피드백 루프
- [[sources/jay-choi-9-tips]] — 테스트 리뷰의 필요
- *(Kent Beck *TDD by Example* (2002), *Extreme Programming Explained* (1999) 인용 페이지 미명시 — P3.)*
