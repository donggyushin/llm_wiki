---
title: "Ubiquitous Language — 도메인과 코드와 대화의 어휘 일치"
type: concept
tags: [ddd, domain-driven-design, ai-coding, vocabulary]
created: 2026-04-28
updated: 2026-04-28
sources: [matt-pocock-software-fundamentals]
aliases: ["ubiquitous language", "유비쿼터스 언어", "공통 어휘", "공유 어휘"]
---

# Ubiquitous Language

## 핵심 요약

도메인 주도 설계 (DDD) 의 핵심 개념. **개발자 간 대화, 코드의 표현, 도메인 전문가와의 대화가 모두 같은 도메인 모델에서 파생된 어휘를 공유한다**. [[sources/matt-pocock-software-fundamentals]] 는 이 옛 개념을 **AI 와 사용자 사이의 어휘 정렬 도구** 로 재해석한다 — 마크다운 표 한 장이 AI 의 verbose 함을 줄이고 구현 정렬을 끌어올린다.

## 정의 (DDD 원전)

> Conversations among developers, expressions of the code, and conversations with domain experts are all derived from the same domain model.

도메인 전문가, 개발자, 코드가 *서로 다른 단어* 를 쓰면 번역이 끼어들고, 번역마다 의미가 새거나 어긋난다. ubiquitous language 는 **그 번역 단계를 제거** 한다.

## AI 코딩 재해석 (Pocock)

- AI 도 일종의 *도메인 전문가* — 사용자와 어휘가 다르면 같은 패턴이 발생.
- AI 의 verbose 함은 *어휘 부재* 의 증상: 짧은 도메인 용어가 없으니 긴 풀어쓰기로 의미를 전달하려 한다.
- 처방: 코드베이스 전체에서 통용되는 **도메인 용어 마크다운 표**. 사용자도, AI 도 항상 열어 두고 참조.

## Ubiquitous Language 스킬 (Pocock)

```
1. 코드베이스를 스캔.
2. 등장하는 핵심 용어를 추출.
3. 마크다운 테이블로 정리 (용어 · 정의 · 도메인 위치).
4. 결과 파일을 사용자가 검토 · 정정.
5. 항상 열어 두고 grilling, planning, 구현 시 참조.
```

관찰된 효과:

- **AI 의 사고 흔적(thinking traces) 자체가 덜 verbose 해진다** — 짧고 정확한 용어로 추론.
- **계획과 구현의 정렬도 향상** — 같은 단어가 단계를 가로질러 일관되게 사용됨.

## 위치 — 다른 컨텍스트 도구와의 비교

| 도구 | 무엇을 공유 | 산출물 형태 |
|---|---|---|
| **Ubiquitous Language** | 어휘 (도메인 용어) | 마크다운 표 |
| **CLAUDE.md** | 규칙·프로젝트 메타 | 마크다운 문서 → [[concepts/claude-md]] |
| **세컨드 브레인** | 결정·패턴·디버깅 노트 | 자유 형식 노트 → [[concepts/context-engineering#2-세컨드-브레인]] |
| **`.claude/rules/*.md`** | 파일 패턴별 규칙 | frontmatter glob → [[concepts/conditional-rule-loading]] |

ubiquitous language 는 *어휘 축*. 다른 도구들과 직교한다.

## Design Concept 과의 관계

[[concepts/design-concept]] = *설계 이해* 의 공유, ubiquitous language = *어휘* 의 공유. 둘은 같은 정렬 작업의 두 면:

- design concept 이 흐릿하더라도 **공유 어휘** 가 있으면 grilling 단계가 짧아진다.
- 합의된 design concept 으로부터 **새 도메인 용어** 가 출현하면 ubiquitous language 에 추가.

## 도메인 전문가 ↔ 개발자 비유 (Pocock)

> "마이크로칩 회사 도메인 전문가가 마이크로칩 앱을 만들어 달라고 한다. 당신은 마이크로칩이 뭔지 모른다. 공통 언어를 합의하지 않으면 그 사람의 말을 당신도 이해 못 하는 코드로 옮기게 된다."

AI 와의 작업도 같다 — *AI 가 도메인 전문가일 수도, 사용자가 도메인 전문가일 수도* 있다.

## 관련 페이지

- [[concepts/domain-driven-design]] — 상위 우산 (DDD 의 한 패턴)
- [[sources/matt-pocock-software-fundamentals]] — 출처
- [[concepts/design-concept]] — 어휘 정렬의 자매 작업
- [[concepts/context-engineering]] — 세컨드 브레인이라는 더 큰 우산
- [[concepts/claude-md]] — 항상 로드되는 규칙의 영역
- [[concepts/claude-skills]] — Pocock 의 *ubiquitous language* 스킬

## 출처

- [[sources/matt-pocock-software-fundamentals]] — DDD 원문 인용 + AI 코딩 재해석 + 스킬 사례
