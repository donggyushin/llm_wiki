---
title: "Software Entropy — 코드는 가만히 두면 나빠진다"
type: concept
tags: [software-design, maintenance, ai-coding]
created: 2026-04-28
updated: 2026-04-28
sources: [matt-pocock-software-fundamentals]
aliases: ["software entropy", "소프트웨어 엔트로피", "code rot"]
---

# Software Entropy

## 핵심 요약

*The Pragmatic Programmer* (Hunt & Thomas) 의 한 챕터에서 형식화. **소프트웨어 시스템도 엔트로피의 법칙을 따른다 — 가만히 두면 점점 더 무질서해진다**. 변경할 때마다 *그 변경* 만 생각하고 *전체 시스템 설계* 를 생각하지 않으면, 코드베이스는 한 번씩 더 나빠진다. [[sources/matt-pocock-software-fundamentals]] 는 이 옛 개념이 [[concepts/specs-to-code-antipattern]] 의 실패를 정확히 설명한다고 본다 — *컴파일러를 다시 돌릴 때마다 코드가 더 나빠진다*.

## 정의 (Pragmatic Programmer)

> Things tend toward disaster — drifting apart, collapsing.

소프트웨어 시스템의 변형:

- 변경 → 시스템 전체를 의식하지 않고 *그 변경만* 처리 → 응집은 줄고 결합은 는다.
- 무수한 작은 변경의 누적 = 코드베이스 부패 (*code rot*).
- 자연스럽게 좋아지는 일은 없다 — *디자인에 일상적으로 투자* 해야만 (→ [[entities/kent-beck]] *invest in design every day*).

## AI 시대의 가속

### Spec-to-code 가 엔트로피를 가속

[[concepts/specs-to-code-antipattern]] 은 정확히 이 패턴:

1. 명세 작성 → AI → 코드 1.
2. 명세 살짝 수정 → AI → 코드 2 (1번을 의식하지 않고 다시 짠다).
3. 반복 → 매번 시스템 전체 설계는 무시되고 변경만 처리.
4. 결과: **컴파일을 돌릴수록 코드가 더 나빠진다** — Pocock 의 직접 경험.

> "I would just end up with garbage."

### AI 가 자체적으로 엔트로피를 만든다

- AI 는 **기존 코드를 참고해 새 코드를 짠다** — 기존이 나쁘면 그대로 복제. → [[concepts/harness-engineering#4-garbage-collection]].
- 시스템 전체 설계를 의식하는 *전략적* 사고는 사람이 해야 한다. AI 는 *전술* 부사관일 뿐.

## 막는 방법

### 1) 매일 설계에 투자

[[entities/kent-beck]] 의 격언. 인터페이스를 다듬고, [[concepts/deep-modules]] 로 정리하고, 결합을 끊는 작업을 *일상의 일부* 로 둔다.

### 2) Garbage collection 자동화

[[concepts/harness-engineering]] 의 4번째 기둥. 자동 청소 시스템:

- 코딩 규칙 위반 자동 감지.
- 중복 / 데드 코드 발견.
- 리팩터링 PR 자동 생성.
- 아키텍처 안티패턴 주기 체크.

### 3) Lint 루틴 (이 위키에선)

위키 자체에도 동일한 원리 적용 → [[syntheses/lint-2026-04-22]] · [[syntheses/lint-2026-04-23]]. 누적된 위키도 엔트로피를 따른다.

## 관련 페이지

- [[sources/matt-pocock-software-fundamentals]] — 출처
- [[concepts/specs-to-code-antipattern]] — 엔트로피 가속의 대표 사례
- [[concepts/code-is-not-cheap]] — 엔트로피 누적이 비용을 만든다
- [[concepts/harness-engineering]] — Garbage collection 으로 막는다
- [[concepts/deep-modules]] — 매일 다듬는 대상
- [[entities/kent-beck]] — *invest in design every day*

## 출처

- [[sources/matt-pocock-software-fundamentals]] — *Pragmatic Programmer* 의 software entropy 챕터 직접 인용
