---
title: "Design Concept — 둘 사이에 떠 있는 설계 이해"
type: concept
tags: [design, ai-coding, planning, ddd-adjacent]
created: 2026-04-28
updated: 2026-04-28
sources: [matt-pocock-software-fundamentals]
aliases: ["design concept", "공유 설계 개념", "디자인 컨셉"]
---

# Design Concept

## 핵심 요약

[[entities/frederick-brooks]] 의 *The Design of Design* (2010) 에 나오는 개념. **두 사람 이상이 함께 설계할 때 둘 사이에 떠 있는, 무형의 공유된 설계 이해**. 마크다운 파일이나 다이어그램으로 박제할 수 있는 자산이 아니라 *theory of the thing* — 만들고자 하는 것에 대한 보이지 않는 이론. AI 코딩 맥락에서 이 개념의 부재가 [[sources/matt-pocock-software-fundamentals]] 가 진단한 1번 실패 모드 — *AI 가 내가 원한 걸 안 만든다* — 의 근본 원인.

## 정의

> "The design concept is not an asset. It's not something you can put in a markdown file. It is the invisible sort of theory of what you're building."  
> — Matt Pocock, [[sources/matt-pocock-software-fundamentals]] (Brooks 인용)

`design concept ≠ PRD ≠ spec ≠ 다이어그램`. 이 산출물들은 design concept 의 *그림자* 이지 본체가 아니다.

## 왜 AI 와 어긋나는가

- 사용자는 머릿속에 *흐릿한* design concept 만 가지고 있다 — *Pragmatic Programmer*: **아무도 자기가 뭘 원하는지 정확히 모른다**.
- AI 는 그 흐릿한 이미지를 짐작으로 채운다. 두 design concept 의 교집합이 좁을수록 결과물이 어긋난다.
- 일반적인 *plan mode* 는 자산(plan 문서) 을 만드는 데 급급해서 design concept 합의 단계를 건너뛴다 → [[concepts/plan-mode]].

## 처방 — Grill Me 스킬

[[entities/matt-pocock]] 이 만든 두 줄짜리 스킬:

```
Interview me relentlessly about every aspect of this plan
until we reach a shared understanding.
Walk down each branch of the design tree (Frederick P. Brooks)
resolving dependencies between decisions one by one.
```

작동:

1. AI 가 사용자에게 40~60 (많을 땐 100 개) 질문 던지며 어긋난 부분을 메운다.
2. **AI 가 적대자(adversary)** 처럼 행동 — 끊임없이 아이디어를 사용자에게 핑.
3. 합의된 대화 → PRD / 이슈 / 직접 구현 단계로 진입.
4. AFK(자율) 에이전트가 그 결과물을 받아 실행 가능.

GitHub 에서 13K stars 를 받음. *공유 design concept 부재* 가 현재 AI 코딩의 가장 큰 마찰임을 시장이 검증.

## Design Tree (Brooks)

설계는 트리 — 한 결정이 다른 결정의 의존성을 만든다. 좋은 설계 프로세스는 가지마다 의존성을 하나씩 해소. Grill me 스킬의 두 번째 줄이 정확히 이 절차를 흉내 낸다.

## Plan Mode 와의 대비

| | Plan Mode | Grill Me |
|---|---|---|
| 산출물 | plan.md 자산 | 합의된 design concept (대화) |
| 흐름 | AI 가 빠르게 plan 작성 → 사용자 승인 | AI 가 사용자를 *심문* → 합의 도달 후 plan |
| 위험 | 자산을 너무 빨리 만든다 | 너무 길어질 수 있다 |
| 상호 보완 | grill me 결과를 plan mode 의 입력으로 사용 가능 |

[[sources/jay-choi-9-tips]] 의 *플랜 적극 편집* + *명시적 지시* 원칙은 같은 진단의 다른 처방 — plan 자산을 그대로 받아들이지 말고 사용자가 적극 개입하라.

## 관련 페이지

- [[sources/matt-pocock-software-fundamentals]] — 출처
- [[entities/frederick-brooks]] — 개념 원작자
- [[entities/matt-pocock]] — Grill Me 스킬 저자
- [[concepts/plan-mode]] — 보완/대안 도구
- [[concepts/ubiquitous-language]] — 어휘 정렬은 design concept 의 인접 작업
- [[concepts/specs-to-code-antipattern]] — design concept 무시의 사례
- [[concepts/claude-skills]] — Grill Me 스킬의 형태

## 출처

- [[sources/matt-pocock-software-fundamentals]] — Brooks 인용 + grill me 스킬 두 줄 노출
