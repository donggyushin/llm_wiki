---
title: Wiki Operations — Ingest / Query / Lint
type: concept
tags: [operations, workflow, llm-wiki]
created: 2026-04-22
updated: 2026-04-22
sources: [[llm-wiki-karpathy]]
---

# Wiki Operations

[[llm-wiki-pattern]] 이 실제로 돌아가는 3 가지 기본 루프. [[three-layer-architecture]] 의 `wiki/` 레이어에 대한 모든 변경은 이 셋 중 하나.

## 1. Ingest — 소스 통합

**트리거**: 사용자가 새 소스를 `raw/` 에 드롭하거나 "ingest 해" 라고 말할 때.

**플로우**:
1. LLM 이 소스 전체를 읽음
2. 사용자와 **키 테이크어웨이 대화** (2~4 줄 요약 + 주요 엔티티 + 기존 위키와의 연결점)
3. 라우팅 결정 — 어떤 source/concept/entity 페이지를 만들지·갱신할지 (보통 1 소스 = 5~15 페이지 터치)
4. 쓰기:
   - `wiki/sources/<slug>.md` 생성
   - 신규 개념·엔티티 → `concepts/`·`entities/` 생성
   - 기존 페이지 → **Edit** 으로 보강 (덮어쓰기 금지)
5. 색인 갱신: `wiki/index.md`
6. 로그 추가: `wiki/log.md` 에 `## [YYYY-MM-DD] ingest | <제목>`

**스타일**: 저자는 **한 번에 하나씩, 대화하며** ingest 를 선호. 배치 ingest 도 가능하지만 감독이 약해짐.

## 2. Query — 질의·탐구

**트리거**: 사용자의 질문.

**플로우**:
1. `index.md` 먼저 읽어 관련 페이지 후보 파악
2. 후보 페이지 정독 → 인용 기반 답변 합성 (인용은 `[[page-name]]` 링크)
3. **역파일링 판단**: 답변이 재사용 가치 있으면 `wiki/concepts/` 또는 `wiki/sources/analysis-*.md` 로 저장 → 탐구 누적
4. `log.md` 에 `query | <질문 요약>` 추가

**출력 형태**: 마크다운 페이지·비교 테이블·Marp 슬라이드·matplotlib 차트·캔버스 등 질문에 맞춰 변주. 핵심은 **좋은 답변을 위키로 환류**.

## 3. Lint — 건강 검진

**트리거**: 사용자 요청("lint") 또는 주기적 습관.

**탐지 대상**:
- **모순**: 페이지 간 충돌하는 주장
- **스테일**: 새 소스로 무효화된 오래된 주장
- **고아**: 인바운드 링크 0 페이지
- **누락**: 여러 곳에서 언급되는데 자체 페이지 없는 개념
- **미싱 링크**: 평문으로만 적힌 엔티티 이름 (`[[...]]` 로 승격 필요)
- **데이터 갭**: 추가 소스 탐색이 가치 있을 영역
- **오버사이즈 페이지**: 800 줄+, 분할 후보

**출력**: 우선순위별 리포트 → 사용자 승인 → 자동 수정 → log 에 `lint | <요약>`.

## 인덱스와 로그 — 두 특수 파일

위키가 커지면 LLM 도 "어디에 뭐가 있는지" 잃는다. 두 파일이 내비게이션 허브.

### `index.md` — 콘텐츠 지향
- 전 페이지 카탈로그, 카테고리별 그룹
- 엔트리: `- [[slug]] — 한 줄 설명 (updated: YYYY-MM-DD)`
- 매 ingest 마다 갱신
- **임베딩 없이도** ~수백 페이지 규모까지 충분. 더 커지면 qmd 같은 검색 도구 통합.

### `log.md` — 시간 지향
- Append-only 연대기
- 포맷: `## [YYYY-MM-DD] <op> | <제목>` (grep 파싱 가능)
- 예: `grep "^## \[" wiki/log.md | tail -5`
- LLM 이 "최근 뭘 했지?" 질문에 즉답.

## 선택적 확장

- **CLI 도구**: 위키 규모 커지면 로컬 검색 엔진 (예: qmd — Tobi Lütke). BM25 + vector + LLM 리랭커. MCP 서버로 LLM 네이티브 툴화 가능.
- **git**: `wiki/` 는 단순 마크다운 디렉터리 — git 로 버전·브랜치·협업 자연 획득.

## 관련 개념

- [[llm-wiki-pattern]] — 이 오퍼레이션들이 속한 전체 패턴
- [[three-layer-architecture]] — 이 오퍼레이션들이 영향 미치는 레이어

## 근거 소스

- [[llm-wiki-karpathy]] "Operations"·"Indexing and logging" 섹션
