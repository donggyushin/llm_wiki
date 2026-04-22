---
title: LLM 위키 패턴
type: concept
tags: [knowledge-management, pattern, llm, second-brain]
created: 2026-04-22
updated: 2026-04-22
sources: [[llm-wiki-karpathy]], [[claude-code-study-notes]], [[harness-engineering-note]]
---

# LLM 위키 패턴

## 정의

LLM 을 **쿼리타임 검색기**(RAG)가 아니라 **영속적 지식 아티팩트의 유지보수자**로 사용하는 개인 지식관리 패턴. 원시 소스 앞단에 LLM 이 쓰고 관리하는 **지속 성장형 마크다운 위키**를 둔다.

## 핵심 구분 — RAG vs LLM Wiki

| | 일반 RAG | LLM Wiki |
|---|---|---|
| 지식 형태 | 매 쿼리에 동적 재조합 | **영속적 누적 아티팩트** |
| 업데이트 단위 | 소스 인덱싱 | 엔티티/토픽 페이지 **갱신** |
| 모순 처리 | 없음 (쿼리 때 드러남) | **ingest 시점에 플래그** |
| 상호참조 | 없음 | **항상 유지됨** |
| 사용자 가치 | 답변 | 답변 + **읽을 수 있는 위키** |

## 핵심 아이디어

1. **지식은 컴파일된다**: 소스가 추가되면 위키에 **녹아든다**. 인덱싱이 아니라 **통합**.
2. **부기가 핵심**: 위키가 실패하는 이유는 읽기·사고가 아니라 **메인터넌스 부담** — 크로스레퍼런스 갱신, 요약 최신화, 모순 발견. LLM 은 지치지 않으므로 15 개 파일을 한 번에 수정 가능.
3. **비대칭 분업**:
   - 인간 → **소싱**(무엇을 읽을지), **탐구**(어떤 질문을 할지), **방향**(어디로 갈지)
   - LLM → **요약·파일링·상호참조·일관성 유지**
4. **쿼리 결과도 자산**: 좋은 답변은 위키에 **역파일링**. 탐구가 채팅에 휘발되지 않음.

## 왜 작동하는가

> 지식 베이스 유지의 지루한 부분은 읽기·사고가 아니라 부기다. 인간은 부가 가치가 수익 체감하는 지점에 도달하면 위키를 포기한다. LLM 은 지루해하지 않고, 크로스레퍼런스 갱신을 잊지 않으며, 한 번에 15 파일을 건드릴 수 있다. 유지 비용이 0 에 가까워지므로 위키가 살아남는다.
> — [[llm-wiki-karpathy]]

## 구성 요소

- [[three-layer-architecture]] — raw / wiki / schema 분리
- [[wiki-operations]] — Ingest · Query · Lint 루프
- 뷰어: [[obsidian]] (graph view, 링크, Dataview)

## 적용 영역

저자가 제시한 범위:
- 개인 성장 트래킹 (건강·목표·심리)
- 딥다이브 리서치 (논문·리포트)
- 독서 동반 위키 (캐릭터·테마·플롯)
- 팀 내부 위키 (Slack·회의록·고객콜)
- 경쟁 분석·실사·여행 계획·취미

## 사상적 계보

1945 년 Vannevar Bush 의 [[memex]] 비전 — 개인 소유의 활발히 큐레이트되는 지식 저장소, 문서 간 연관 경로. 현재 웹은 그 방향으로 가지 않았지만 LLM 위키는 그 공백을 채우는 시도.

## 사용자 본인의 Claude Code 실천과의 교차

[[claude-code-study-notes]] l.58-74 에서 사용자가 이미 **"세컨드 브레인 구축"** 을 Claude Code 운영의 핵심 원칙으로 기록해둠:

> "의사결정의 이유 등을 로컬 마크다운 파일에 저장해야 함. 수동으로 관리할 필요가 없음. `/memory` 커맨드로 자동으로 해줌."

이 위키 시스템은 그 원칙의 **본격 확장판**:
- `/memory` 의 flat 기억 → 구조화된 위키 페이지
- 단일 디렉터리 덤프 → [[three-layer-architecture]] 분리
- 수동 업데이트 → LLM 메인터너가 [[wiki-operations|Ingest/Query/Lint]] 수행

[[context-as-king]] 의 LazyLoading 원칙과도 정신적 동류 — 위키의 `index.md` 가 LazyLoading 의 엔트리 포인트 역할.

## Harness 관점에서의 재조명

이 패턴 전체는 [[harness-engineering]] 의 **지식관리 도메인 적용 사례**로도 읽힌다. 4 기둥과의 대응:

| Harness 기둥 | 이 패턴의 구현 |
|---|---|
| 기계가 읽는 컨텍스트 | `CLAUDE.md` (위키 유지보수자 스키마) |
| 자동 교정 루프 | [[wiki-operations]] 의 Lint 오퍼레이션 |
| 명시적 도구 경계 | `raw/` read-only, `wiki/` LLM-only, 외부 폴더 touch 금지 — [[three-layer-architecture]] |
| 피드백 루프 | `log.md` append-only + 주기 lint |

→ 이 위키가 "흩어진 노트"가 아니라 **하네스가 걸린 시스템** 이 되는 이유.

## 관련 개념

- [[context-as-king]] — 세컨드 브레인 원칙의 Claude Code 운영 버전
- [[harness-engineering]] — 이 패턴의 상위 일반 원칙
- [[three-layer-architecture]] — 이 패턴의 구조적 토대
- [[wiki-operations]] — 이 패턴의 실행 루프
- Zettelkasten — 루만의 종이 슬립 시스템. 상호참조 강조 면에서 유사하나 **인간이 직접 카드를 쓴다**.
- Second Brain (Tiago Forte) — PARA 구조 등. 파일링 체계이지만 **자동 메인터넌스가 없음**.
- RAG — 쿼리타임 검색. 이 패턴의 **대립항**이자 하위 요소 (위키가 덮지 못하는 원본 세부 질의에 여전히 유용).

## 근거 소스

- [[llm-wiki-karpathy]] — 주 출처
- [[claude-code-study-notes]] — 사용자 본인의 세컨드 브레인 실천 맥락 제공
