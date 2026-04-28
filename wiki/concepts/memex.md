---
title: "Memex — As We May Think 의 개인 지식 기계"
type: concept
tags: [history, knowledge-management, hypertext, second-brain]
created: 2026-04-28
updated: 2026-04-28
sources: []
aliases: ["memex", "Vannevar Bush memex"]
---

# Memex

## 핵심 요약

**Vannevar Bush** 가 *As We May Think* (1945, *The Atlantic*) 에서 제안한 가상의 기계. **개인이 일생 동안 누적한 모든 책·기록·소통을 기계적으로 저장하고, 연관(*associative trail*) 으로 자유롭게 연결·재호출하는 책상형 장치**. 하이퍼텍스트, 위키, 노트앱, 세컨드 브레인의 **개념적 조상**. 본 위키 [[concepts/llm-wiki-pattern]] 의 계보에서 *AI 작성자가 추가되기 이전의 원형*.

## 정의 (Bush, 1945)

> A device in which an individual stores all his books, records, and communications, and which is mechanized so that it may be consulted with exceeding speed and flexibility. It is an enlarged intimate supplement to his memory.

번역: 개인이 자기 책·기록·소통을 전부 저장하고 *놀라운 속도와 유연성* 으로 참조할 수 있는 장치. 기억의 친밀하고 확장된 보조 장치.

## 핵심 특징

| 특징 | Memex (1945) | 현대적 대응 |
|---|---|---|
| **개인 소유** | 책상에 놓인 마이크로필름 기계 | 로컬 마크다운 / Obsidian / 본 위키 |
| **누적성** | 평생 자료 저장 | 세컨드 브레인 / [[concepts/llm-wiki-pattern]] |
| **연관(associative trail)** | 두 페이지를 *trail* 로 묶어 재호출 | 하이퍼링크 / wiki-link 문법 |
| **외부 보조 기억** | 사람의 망각을 보완 | LLM 컨텍스트 윈도우 한계 보완 |
| **범용성** | 글·그림·녹음 모두 | 멀티모달 노트 |

## 시대 위치

```
1945: Memex 제안 (Vannevar Bush, As We May Think)
   ↓
1965: Hypertext 용어 (Ted Nelson, Project Xanadu)
   ↓
1989: World Wide Web (Tim Berners-Lee)
   ↓
2001: Wikipedia
   ↓
2010s: Notion / Roam / Obsidian (개인 노트)
   ↓
2024: Karpathy LLM Wiki — AI 가 작성자/유지보수자 ([[concepts/llm-wiki-pattern]])
```

## 본 위키와의 관계

[[concepts/llm-wiki-pattern]] 의 *씨앗 인용* 으로 [[entities/andrej-karpathy]] 가 직접 언급. 위키 — 그리고 본 저장소 — 는 memex 의 후예 중 *AI 가 작성자로 한 단계 추가된* 노드.

핵심 차이:

| | Memex | LLM Wiki |
|---|---|---|
| 작성자 | 사람 | LLM |
| 검색 | associative trail | wiki-link + 인덱스 + LLM 합성 |
| 유지 | 사람의 의식적 정리 | LLM 의 ingest / lint |

## 관련 페이지

- [[concepts/llm-wiki-pattern]] — 직계 후손
- [[entities/andrej-karpathy]] — LLM Wiki 발화자, memex 계보 명시 인용
- [[concepts/rag]] — 다른 계보의 분기 (검색 우선 vs 누적 우선)
- [[concepts/context-engineering]] — *세컨드 브레인* 섹션이 memex 의 일상 구현

## 출처

- Vannevar Bush, *As We May Think*, *The Atlantic*, July 1945.
- *(1차 원문 URL · 페이지 번호 미인용 — 후속 lint 시 atlantic.com 아카이브 링크 보강.)*
- [[concepts/llm-wiki-pattern]] 본문에서 간접 인용된 계보 서술.
