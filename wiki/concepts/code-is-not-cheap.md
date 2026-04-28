---
title: "Code Is Not Cheap — AI 시대의 나쁜 코드는 가장 비싸다"
type: concept
tags: [thesis, ai-coding, software-fundamentals]
created: 2026-04-28
updated: 2026-04-28
sources: [matt-pocock-software-fundamentals]
aliases: ["code is not cheap", "code is cheap 반박", "bad code is expensive"]
---

# Code Is Not Cheap

## 핵심 요약

[[entities/matt-pocock]] 의 AI Engineer 발표 [[sources/matt-pocock-software-fundamentals]] 의 **테제 한 줄**. *"Code is cheap"* 를 전제로 코드를 일회용으로 다루는 [[concepts/specs-to-code-antipattern]] 운동을 정면 반박. **AI 시대일수록 좋은 코드베이스가 더 중요하고, 따라서 나쁜 코드는 어느 때보다 비싸다** — AI 의 잠재력을 누리려면 변경하기 쉬운 코드가 필요한데, 나쁜 코드베이스에서 AI 는 빛나지 않기 때문.

## 정식화

```
좋은 코드베이스 = 변경하기 쉬운 코드베이스 (Ousterhout)
좋은 코드베이스 ⇒ AI 가 잘 한다
∴ AI 시대에는 좋은 코드베이스가 어느 때보다 중요
∴ AI 시대에는 소프트웨어 펀더멘털이 어느 때보다 중요
```

→ [[entities/john-ousterhout]] 의 복잡성 정의를 1차 전제로 사용.

## "Code is cheap" 의 매력과 함정

| | 매력 | 함정 |
|---|---|---|
| 매력 | AI 가 짠 코드는 무한히 다시 짤 수 있다는 환상 | 매 재컴파일마다 [[concepts/software-entropy]] 가속 |
| 매력 | 명세만 영구 자산 | 명세는 [[concepts/design-concept]] 의 그림자일 뿐 |
| 매력 | 설계에서 해방 | [[entities/kent-beck]] 의 *invest in design every day* 위배 |

> "Bad code is the most expensive it's ever been. Because if you have a codebase that's hard to change, you're not able to take all of the bounty that AI can offer."

## 따름 정리들

### 따름 1 — 펀더멘털이 어느 때보다 중요

옛 책 3권이 이 발표의 1차 매뉴얼이 된다:

- [[entities/john-ousterhout]] *A Philosophy of Software Design*
- *Pragmatic Programmer* (Hunt & Thomas) — [[concepts/software-entropy]]
- [[entities/frederick-brooks]] *The Design of Design* — [[concepts/design-concept]]

### 따름 2 — 5개의 실패 모드는 모두 펀더멘털로 해결

| 실패 모드 | 펀더멘털 처방 |
|---|---|
| AI 가 내가 원한 걸 안 만든다 | [[concepts/design-concept]] |
| AI 가 너무 verbose | [[concepts/ubiquitous-language]] |
| 작동 안 함 | TDD + 피드백 루프 |
| 테스트 어렵다 | [[concepts/deep-modules]] |
| 머리가 안 따라간다 | 인터페이스 설계 + 구현 위임 |

### 따름 3 — Vibe / Spec-to-Code 의 거부

코드를 *보지 않는* 모든 흐름은 같은 함정에 빠진다 → [[concepts/specs-to-code-antipattern]].

## AI 의 자리매김

> AI 는 지상의 *전술* 프로그래머 — 명령을 받아 코드 변경을 수행하는 sergeant. 그 위에 *전략* 레벨에서 사고하는 사람이 필요하다.

코드 = 전략의 산물. 코드를 일회용으로 보는 순간 전략이 사라진다.

## 위키 내 다른 *반-경량화* 신호와의 정렬

이 테제는 위키의 다른 *덜어내지 말 것* 신호들과 같은 맥락:

- [[sources/jay-choi-9-tips]] *플랜 적극 편집* — Plan 자산을 그냥 받아들이지 마라.
- [[concepts/harness-engineering]] *vanilla 우선* — 미리 다 깔지 마라, 실패가 발생한 곳에 울타리를 세워라.
- [[concepts/reward-hacking]] — 검증 루프를 신뢰만 하지 마라, 테스트 자체도 사람이 봐라.

세 신호 모두 *AI 가 자율적으로 알아서 할 거라는 믿음* 의 다른 형태에 대한 경고.

## 한 줄 메시지

> **Code is not cheap. Code is important.**

## 관련 페이지

- [[sources/matt-pocock-software-fundamentals]] — 출처 (테제의 발화처)
- [[entities/matt-pocock]] — 테제 제안자
- [[concepts/specs-to-code-antipattern]] — 직접 반박 대상
- [[concepts/software-entropy]] · [[concepts/design-concept]] · [[concepts/ubiquitous-language]] · [[concepts/deep-modules]] — 5개 펀더멘털 처방
- [[entities/john-ousterhout]] — 1차 전제 (복잡성 정의)
- [[entities/kent-beck]] — *invest in design every day*
- [[concepts/harness-engineering]] · [[concepts/reward-hacking]] — 같은 결의 다른 신호

## 출처

- [[sources/matt-pocock-software-fundamentals]] — 발표의 명시적 테제
