---
title: "John Ousterhout"
type: entity
tags: [computer-science, software-design, academia, tcl]
created: 2026-04-28
updated: 2026-04-28
sources: [matt-pocock-software-fundamentals]
aliases: ["Ousterhout", "John K. Ousterhout"]
---

# John Ousterhout

## 핵심 요약

스탠퍼드 대학 컴퓨터과학 교수. **Tcl/Tk** 의 창시자. 위키에서는 그의 책 ***A Philosophy of Software Design*** (2018, 확장판 2021) 의 두 핵심 주장이 [[sources/matt-pocock-software-fundamentals]] 의 골격을 이룬다는 점에서 등장한다 — (1) **복잡성의 정의**, (2) **deep modules** 의 옹호.

## 주요 사상 (이 위키 맥락)

### 복잡성 = 시스템을 이해하고 변경하기 어렵게 만드는 모든 구조

> "Complexity is anything related to the structure of a software system that makes it hard to understand and modify the system."

- **나쁜 코드 = 변경하기 어려운 코드**.
- AI 시대 함의: 변경하기 어려운 코드베이스에서는 AI 의 잠재력을 누릴 수 없다 → [[concepts/code-is-not-cheap]] 의 토대.

### Deep modules vs Shallow modules

- **Deep module**: 단순 인터페이스 + 풍부한 기능. 복잡성을 *내부에 숨김*.
- **Shallow module**: 인터페이스가 복잡하고 기능은 적음 — 추상화의 비용만 부담하고 이득은 없다.
- AI 코딩에서: shallow 한 코드베이스는 AI 가 의존성을 못 따라잡고 길을 잃는다. → [[concepts/deep-modules]].

## 다른 활동 (참고)

- **Tcl** 스크립팅 언어와 **Tk** 위젯 툴킷 창시자.
- **RAMCloud** 프로젝트 등 분산 시스템 연구.
- 본 위키 범위에서는 *A Philosophy of Software Design* 의 두 챕터만 인용된다.

## 관련 페이지

- [[sources/matt-pocock-software-fundamentals]] — 두 차례 인용 (복잡성 정의 + deep modules)
- [[concepts/deep-modules]] — 그의 핵심 처방을 페이지화
- [[concepts/code-is-not-cheap]] — 복잡성 정의가 이 테제의 토대
- [[concepts/specs-to-code-antipattern]] — 복잡성 누적의 사례

## 출처

- [[sources/matt-pocock-software-fundamentals]] — *A Philosophy of Software Design* 인용 두 곳
