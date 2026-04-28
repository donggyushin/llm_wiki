---
title: "Software Fundamentals Matter More Than Ever — Matt Pocock"
type: source
tags: [ai-coding, software-fundamentals, claude-code, ddd, tdd, deep-modules]
created: 2026-04-28
updated: 2026-04-28
author: "Matt Pocock"
medium: video
raw_path: "raw/matt-pocock-software-fundamentals-transcript.md"
url: "https://youtu.be/v4F1gFy-hqg"
event: "AI Engineer (2026)"
duration: "18:26"
aliases: ["matt-pocock-fundamentals", "Software Fundamentals Matter More"]
---

# Software Fundamentals Matter More Than Ever

## 메타

- **연사**: [[entities/matt-pocock]] · TypeScript 교육자, *Claude Code for Real Engineers* 강좌 운영, aihero.dev
- **이벤트**: AI Engineer 컨퍼런스 (영상 업로드 2026-04-23)
- **GitHub**: `mattpocock/skills` (영상에 등장하는 모든 스킬 공개)
- **요약 한 줄**: AI 코딩 시대에도 — 아니 **AI 코딩 시대이기 때문에** — 소프트웨어 펀더멘털이 더 중요해졌다.

## 핵심 요약

Matt Pocock 은 자신이 운영하는 *Claude Code for Real Engineers* 강좌 커리큘럼을 짜다가, **"새 패러다임이니 옛 규칙은 다 버려야 한다"** 는 통념을 정면으로 반박하는 결론에 도달한다. spec-to-code 운동(스펙만 고치고 코드는 무시) 을 직접 시도해 봤더니 **컴파일러를 돌릴수록 코드가 더 나빠지는** 엔트로피 현상을 목격했다고 증언. 그가 본 다섯 가지 실패 모드는 모두 **수십 년 된 펀더멘털** 로 해결된다 — 도메인 주도 설계의 *ubiquitous language*, John Ousterhout 의 *deep modules*, Frederick Brooks 의 *design concept*, Pragmatic Programmer 의 *outrunning your headlights*, Kent Beck 의 *invest in the design every day*.

테제 한 줄: **"Code is not cheap."** 좋은 코드베이스에서 AI 가 빛나기 때문에, AI 시대에 **나쁜 코드는 어느 때보다 비싸다**.

## 줄거리

### 도입 — 통념 흔들기

- 일부 사람들은 자기 스킬셋이 AI 시대에 무의미해질까 두려워하지만, Pocock 은 **그 반대** 라 본다.
- "spec-to-code" 운동: 명세 → AI → 코드. 버그 나면 코드 보지 말고 명세만 고쳐라. 다시 컴파일.
- 직접 시도: **재컴파일할수록 코드가 더 나빠짐**. "코드를 보지 않으면 된다" 는 vibe coding 의 변종일 뿐.

### 컴파일러를 어떻게 고칠까 — 책장 뒤지기

1. **John Ousterhout** *Philosophy of Software Design* — 나쁜 코드 = "복잡한 코드". 변경하기 어려운 코드. 좋은 코드 = 변경하기 쉬운 코드.
2. **Pragmatic Programmer** — software entropy 챕터. 시스템 전체 설계를 생각하지 않고 변경할 때마다 코드는 점점 더 나빠진다. spec-to-code 가 정확히 그 패턴.

### 테제 — Code is not cheap

> "Code is cheap" 이라는 spec-to-code 의 전제는 틀렸다. **나쁜 코드는 어느 때보다 비싸다** — 변경하기 어려운 코드베이스에서는 AI 의 잠재력을 누릴 수 없기 때문에.

좋은 코드베이스에서 AI 는 정말 잘 한다. 따라서:

- 좋은 코드베이스가 어느 때보다 중요 →
- 소프트웨어 펀더멘털이 어느 때보다 중요.

## 5가지 실패 모드 + 5가지 펀더멘털

### 실패 1 — AI 가 내가 원한 걸 안 만든다

- *Pragmatic Programmer*: **아무도 자기가 뭘 원하는지 정확히 모른다**. AI 와 대화하는 것은 곧 AI 가 사용자에게 *requirements gathering* 하는 일.
- **Frederick P. Brooks** *Design of Design* — 핵심 개념: **design concept**. 두 사람이 함께 설계할 때 둘 사이에 떠 있는 무형의 *공유된 설계 이해*. 마크다운 자산이 아니라 **보이지 않는 이론**.
- AI 와 사용자는 design concept 을 공유하지 않은 채로 일을 시작하기 때문에 어긋난다.
- **스킬: `grill me`** ([[concepts/design-concept]] 참고)
  ```
  Interview me relentlessly about every aspect of this plan
  until we reach a shared understanding.
  Walk down each branch of the design tree (Frederick P. Brooks)
  resolving dependencies between decisions one by one.
  ```
  - 단 두 줄짜리지만 GitHub 에서 13K stars 받음.
  - 이 스킬을 켜면 AI 가 사용자에게 40~60 (많을 땐 100개) 질문을 던지며 합의에 도달한다.
  - 합의된 대화 → PRD 또는 이슈 → AFK(자율) 에이전트가 받아 실행.
  - "**Don't @ me on this** but I think this is better than default plan mode" — Claude Code 의 기본 plan mode 는 자산(plan 문서) 을 만들고 싶어 안달이지만, *공유 design concept 을 먼저 도달하는 것* 이 더 낫다.

### 실패 2 — AI 가 너무 장황하다 (verbose)

- AI 가 너무 많은 단어로 의도를 전달하려 할 때, 사용자와 같은 언어로 말하고 있지 않다.
- 도메인 전문가와 협업해 본 시니어 개발자에겐 익숙한 패턴 — 마이크로칩 도메인 전문가와 일하면 용어부터 정렬해야 한다.
- **Domain-Driven Design (DDD)** 의 **ubiquitous language**: *개발자 간 대화, 코드의 표현, 도메인 전문가와의 대화가 모두 같은 도메인 모델에서 파생된다*.
- 본질은 **마크다운 파일에 도메인 용어 표** — 사용자와 AI 가 공유하는 어휘.
- **스킬: `ubiquitous language`**
  - 코드베이스를 스캔, 용어 추출, 마크다운 표로 정리.
  - 항상 열어 두고 grilling, planning 시 참조.
  - 효과: AI 의 사고 흔적(thinking traces) 자체가 덜 verbose 해진다 + 구현이 계획과 더 정렬.

### 실패 3 — AI 가 만든 게 작동 안 한다

- 명백한 처방: **피드백 루프** — 정적 타입(TypeScript), 브라우저 접근, 자동 테스트.
- 그런데 LLM 은 피드백 루프가 있어도 잘 활용 못 한다. 한 번에 너무 많은 코드를 만들고 *나중에* 타입체크/테스트.
- *Pragmatic Programmer*: **outrunning your headlights** — 헤드라이트보다 빨리 운전하는 것. **피드백 속도가 곧 속도 제한**.
- → **테스트 주도 개발 (TDD)**. 테스트 먼저 → 통과시킨다 → 리팩터.
- TDD 가 AI 에게 **작은 단계** 를 강제한다.

### 실패 4 — 테스트가 어렵다

- 테스트는 늘 어려웠다 — 단위 크기, 무엇을 mock 할지, 어떤 행동을 테스트할지 등 상호의존적인 결정이 많기 때문.
- **좋은 코드베이스 = 테스트하기 쉬운 코드베이스.** 즉 더 좋은 피드백 루프 = 더 좋은 AI 코드.
- **John Ousterhout: deep modules** — *얕은 모듈* (작고 많은, 인터페이스 복잡) 보다 *깊은 모듈* (적고 큰, 인터페이스는 단순하지만 안에 많은 기능) 이 좋다.
  - 얕은 모듈이 가득한 코드베이스 = AI 가 미로처럼 헤맨다. 의존성 못 따라잡고 잘못된 모듈에 코드를 짠다.
  - 깊은 모듈 코드베이스 = 같은 코드를 *경계 안에 넣고 위에 인터페이스* 를 둔 형태. 인터페이스는 사람이 직접 설계, 구현은 AI 에 맡길 수 있다.
- **스킬: `improve codebase architecture`** — 얕은 → 깊은 모듈로 점진 변환. 관련된 코드를 찾아 묶어서 deep module 로 감싸는 단계가 반복 가능한 절차.
- 이렇게 만든 코드베이스는 **TDD 가 보상받는 구조** — 인터페이스에서 테스트하면 됨.

### 실패 5 — 머리가 못 따라간다

- AI 덕에 어느 때보다 많이 ship 하지만 **개발자 본인의 뇌가 지친다**.
- 깊은 모듈 코드베이스가 사람 뇌도 살린다 — 모듈을 **gray box** 로 다룰 수 있다. 인터페이스만 설계, 안쪽 구현은 깊이 들여다보지 않는다 (금융처럼 critical 한 영역은 예외).
- **Tip 5: design the interface, delegate the implementation.** 모듈 외부에서 테스트 가능한 경계 + 모듈의 목적 이해 + 외부에서 설계 가능 → 안쪽은 AI 에 맡긴다.
- 단 이건 코드를 만질 때마다 **모듈 지도** 를 늘 의식해야 한다는 뜻 — ubiquitous language 와 plan 스킬에 모듈 정보를 넣어 둔다.
- **Kent Beck: invest in the design of the system every day.** spec-to-code 는 정확히 *시스템 설계에서 손을 떼는* 행위 — divesting from design.

## 마무리 메시지

> AI 는 지상의 훌륭한 *전술* 프로그래머 — 코드 변경을 수행하는 sergeant. 그 위에 **전략 레벨** 에서 사고하는 사람이 필요하다. 그게 당신이다. 그 일에는 **20년 이상 사용해 온 소프트웨어 펀더멘털 스킬** 이 필요하다.

자료:
- 영상에 나온 모든 스킬: GitHub `mattpocock/skills`
- 강좌·뉴스레터: aihero.dev

## 내가 주목한 부분

- **두 줄짜리 grill me 스킬이 13K stars** 라는 사실 자체가 시그널 — *공유된 design concept* 의 부재가 현재 AI 코딩의 가장 큰 마찰임을 시장이 검증했다.
- "**plan mode 보다 grill me 가 낫다**" 는 도발은 [[concepts/plan-mode]] 의 한계 (자산을 너무 빨리 만든다) 와 [[sources/jay-choi-9-tips]] 의 *플랜 적극 편집 + 명시적 지시* 와 정확히 같은 진단의 다른 처방.
- *ubiquitous language* 라는 DDD 의 오래된 도구를 **AI 에이전트의 어휘 정렬용 공유 마크다운** 으로 재해석한 것 — [[concepts/context-engineering]] 의 "세컨드 브레인" 과 결이 같지만, **공유 어휘** 라는 축으로 분리된다.
- *deep modules* / *interface design + delegate implementation* 은 [[concepts/harness-engineering]] 의 "구조적으로 어길 수 없는 환경" 과 같은 결의 코드 아키텍처 처방. 모듈 인터페이스가 일종의 *코드 레벨 하네스*.
- *outrunning your headlights / 피드백 속도 = 속도 제한* 은 [[concepts/reward-hacking]] 와 마주 보는 면 — TDD 가 보상 해킹의 1차 방어이지만 테스트 자체도 사람이 봐야 한다.
- 책 3권의 인용 — 1990년대~2020년대 *디자인 책들이* AI 시대의 1차 매뉴얼이 되는 역설.

## 관련 페이지

- 핵심 인물: [[entities/matt-pocock]]
- 인용된 인물·책: [[entities/john-ousterhout]] · [[entities/frederick-brooks]] · [[entities/kent-beck]]
- 신규 개념: [[concepts/code-is-not-cheap]] · [[concepts/specs-to-code-antipattern]] · [[concepts/software-entropy]] · [[concepts/design-concept]] · [[concepts/ubiquitous-language]] · [[concepts/deep-modules]]
- 강하게 연결: [[concepts/claude-skills]] · [[concepts/plan-mode]] · [[concepts/context-engineering]] · [[concepts/harness-engineering]] · [[concepts/reward-hacking]]

## 출처

- 원본 자막: `raw/＂Software Fundamentals Matter More Than Ever＂ — Matt Pocock [v4F1gFy-hqg].en.vtt` (yt-dlp auto-sub, en)
- 전처리: `raw/matt-pocock-software-fundamentals-transcript.md` (timestamp 제거 후 병합)
- 비디오 URL: https://youtu.be/v4F1gFy-hqg
