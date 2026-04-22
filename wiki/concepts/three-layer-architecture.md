---
title: 3층 아키텍처 (raw / wiki / schema)
type: concept
tags: [architecture, llm-wiki, separation-of-concerns]
created: 2026-04-22
updated: 2026-04-22
sources: [[llm-wiki-karpathy]]
---

# 3층 아키텍처

[[llm-wiki-pattern]] 의 구조적 토대. 세 레이어로 관심사를 분리해, LLM 이 경계를 지키면서도 큰 자유도로 위키를 유지하게 한다.

## 세 레이어

### 1. Raw sources — 원본 (불변)

- 사용자가 큐레이트한 소스 문서 모음: 기사·논문·이미지·데이터 파일·클리핑
- **Immutable**: LLM 은 **읽기만** 하고 절대 수정 금지
- **진실의 근원** (source of truth)
- 이 vault 에선 `raw/` 폴더

### 2. Wiki — LLM 생성물

- LLM 이 쓴 마크다운 페이지 집합: 소스 요약·엔티티 페이지·개념 페이지·비교·개요·합성
- **LLM 소유**: 페이지 생성·갱신·상호참조 전부 담당
- 사용자는 **읽기 전용** (수정은 LLM 에게 요청)
- 이 vault 에선 `wiki/` 폴더 (`sources/`·`concepts/`·`entities/` 하위)

### 3. Schema — 운영 규칙

- LLM 이 위키를 어떻게 구조화·운영할지 명시한 문서 (Claude Code 의 경우 `CLAUDE.md`, Codex 는 `AGENTS.md`)
- 컨벤션·워크플로우·트리거·페이지 포맷 정의
- **핵심 설정 파일**: 이게 없으면 LLM 은 그냥 일반 챗봇. 이게 있어야 **규율 있는 위키 유지보수자**가 됨
- 사용자와 LLM 이 **공동 진화**시킴

## 왜 분리하는가

- **Raw immutability** 는 신뢰 기반. 나중에 위키 주장을 소스로 역추적 가능해야 함. LLM 이 소스를 수정하면 이 신뢰가 깨짐.
- **Wiki 소유권** 은 유지보수 일관성. 인간이 끼어들어 직접 편집하면 LLM 의 멘탈 모델과 어긋남. (예외: 사소한 타이포는 OK — 하지만 구조 변경은 LLM 에게 지시)
- **Schema 분리** 로 "어떻게 일할지"를 데이터와 분리. 규칙이 진화해도 위키 내용은 안전.

## 이 vault 에서의 구현

```
raw/            ← immutable, LLM read-only
  llm-wiki.md
wiki/           ← LLM-owned
  index.md
  log.md
  sources/      ← 소스별 요약 (1 source = 1 page)
  concepts/     ← 개념·패턴
  entities/     ← 사람·도구·조직·장소
CLAUDE.md       ← schema, 공동 진화
```

세부 규약은 [[CLAUDE.md|CLAUDE]] 참조.

## 관련 개념

- [[llm-wiki-pattern]] — 이 아키텍처가 속한 전체 패턴
- [[wiki-operations]] — 이 구조 위에서 돌아가는 3대 오퍼레이션
- **Functional core / imperative shell** — Hickey 스타일 분리와 유사. raw = 순수 데이터, wiki = 파생물, schema = 변환 규칙

## 근거 소스

- [[llm-wiki-karpathy]] "Architecture" 섹션
