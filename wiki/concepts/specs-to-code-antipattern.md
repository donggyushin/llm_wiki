---
title: "Spec-to-Code 안티패턴 — 명세만 고치고 코드는 무시"
type: concept
tags: [ai-coding, antipattern, specs-to-code, spec-driven-development]
created: 2026-04-28
updated: 2026-04-28
sources: [matt-pocock-software-fundamentals]
aliases: ["specs to code", "spec-driven development", "명세 주도 개발", "스펙투코드"]
---

# Spec-to-Code 안티패턴

## 핵심 요약

**"명세를 적으면 AI 가 코드로 컴파일한다. 버그가 나면 코드 보지 말고 명세만 고쳐라. 다시 컴파일하면 새 코드가 나온다."** [[entities/matt-pocock]] 가 [[sources/matt-pocock-software-fundamentals]] 에서 정면 반박한 운동. 직접 시도한 결과 **컴파일을 다시 돌릴수록 코드가 더 나빠지는** [[concepts/software-entropy]] 가속을 목격했다. 본질은 *vibe coding 의 변종* — 코드를 무시하면 코드가 자기 자신을 관리할 거라는 환상.

## 운동의 주장

| 단계 | spec-to-code |
|---|---|
| 1 | 애플리케이션 동작에 대한 명세 작성 |
| 2 | AI 가 명세를 코드로 *컴파일* |
| 3 | 버그? 코드 보지 말고 **명세를 고쳐라** |
| 4 | 컴파일러 (AI) 재실행 → 새 코드 |
| 5 | 코드는 휘발성 산출물. 영구 자산은 명세뿐 |

겉으로는 단정한 분리 — *what* (명세) 과 *how* (코드) 의 깔끔한 격리.

## 실험 결과 (Pocock)

> "I would run it and try not to look at the code, but I would look at the code, and I realized I would get worse code first, then worse code again, then worse code again. I just ended up with garbage."

- 컴파일 1회: 그럭저럭.
- 2회: 더 나빠짐.
- 3회: 더 나빠짐.
- ⋯ 결국 *garbage*.

청중에서 같은 경험을 한 사람들이 손을 들었다. 일관된 실패 모드.

## 왜 실패하는가 — 펀더멘털 진단

### 1) [[concepts/software-entropy]] 가속

매 변경에서 *시스템 전체 설계* 를 무시하고 *그 변경만* 처리 → 엔트로피 증가의 교과서적 사례.

### 2) [[concepts/code-is-not-cheap]] 의 부정

운동의 전제: *code is cheap*. 그러므로 코드는 일회용. 그러나 나쁜 코드베이스에서는 AI 가 다음 변경을 못 한다 — 결국 코드가 *가장 비싸지는* 방향으로 강화 학습된다.

### 3) [[entities/kent-beck]] 의 *divesting from design*

매일 설계에 투자해야 한다는 원칙의 정반대. spec-to-code 는 *설계에서 손을 떼는 행위* — 영구 자산은 명세이므로 코드 설계는 *일회성*.

### 4) [[concepts/design-concept]] 의 무시

명세 = *자산*. 그러나 [[entities/frederick-brooks]] 의 design concept 은 자산이 아니다. 사람과 AI 사이에 *떠다니는 합의* 가 더 본질이다. 명세만 다듬어도 그 떠다니는 이해가 정렬되지 않으면 매번 어긋난다.

## Vibe Coding 과의 관계

> "The idea that we can just ignore the code and have the code let it manage itself is sort of vibe coding by another name."

표면 형태는 다르지만 본질은 같다 — *코드를 보지 않는다*. spec-to-code 는 vibe coding 에 *명세* 라는 가짜 안전망을 더한 변종.

## 무엇을 대체할 것인가

Pocock 이 제안하는 5가지 펀더멘털:

| 실패 모드 | 처방 |
|---|---|
| AI 가 엉뚱 | [[concepts/design-concept]] 합의 (grill me) |
| AI 가 verbose | [[concepts/ubiquitous-language]] |
| 작동 안 함 | TDD + 피드백 루프 |
| 테스트 어려움 | [[concepts/deep-modules]] |
| 머리 안 따라감 | 인터페이스 설계 + 구현 위임 |

핵심 메시지: *명세를 잘 적는 것이 답이 아니다. 펀더멘털로 돌아가는 것이 답이다.*

## 관련 페이지

- [[sources/matt-pocock-software-fundamentals]] — 출처
- [[concepts/code-is-not-cheap]] — 이 안티패턴이 부정하는 테제
- [[concepts/software-entropy]] — 이 안티패턴이 가속하는 메커니즘
- [[concepts/design-concept]] — 명세가 *자산이 아니라는* 통찰의 자리
- [[concepts/deep-modules]] · [[concepts/ubiquitous-language]] — 진짜 처방
- [[entities/kent-beck]] — *divesting from design*

## 출처

- [[sources/matt-pocock-software-fundamentals]] — 직접 비판 + 실험 결과
