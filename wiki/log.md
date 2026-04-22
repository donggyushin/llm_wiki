# Wiki Log

위키 오퍼레이션 연대기. Append-only. 최신이 아래로.

`grep "^## \[" wiki/log.md | tail -5` 로 최근 활동 확인.

---

## [2026-04-22] schema | 위키 시스템 초기 부트스트랩

- `CLAUDE.md` 를 LLM 위키 스키마로 재작성 (3층 아키텍처·운영 규약·트리거 키워드 정의)
- 폴더 구조 생성: `raw/`, `wiki/{sources,concepts,entities}/`
- `wiki/index.md`, `wiki/log.md` 초기화

## [2026-04-22] ingest | llm-wiki.md (Karpathy 스타일 LLM 위키 패턴)

- 원본: `raw/llm-wiki.md` (기존 vault 루트에서 이동)
- 생성 페이지 (6):
  - Source: [[llm-wiki-karpathy]]
  - Concepts: [[llm-wiki-pattern]], [[three-layer-architecture]], [[wiki-operations]]
  - Entities: [[obsidian]], [[memex]]
- 핵심 주장: LLM 이 RAG 처럼 매 쿼리마다 재탐색하는 대신, **지속·증분적으로 유지되는 위키**를 상시 유지하면 지식이 누적된다. 인간은 소스 큐레이션과 질문, LLM 은 요약·상호참조·메인터넌스 담당.
- 후속 탐구 아이디어: `[[qmd]]`(Tobi Lütke 의 로컬 마크다운 검색 엔진), `[[dataview]]` 플러그인, `[[marp]]` 슬라이드 통합.

## [2026-04-22] schema | STUDY/ 폴더 → raw/ 로 이동 확인

- 사용자가 기존 `STUDY/` 폴더 (Advisor.md, CLAUDE study.md, Harness Engineering.md) 를 `raw/` 로 이동
- `CLAUDE.md` 의 "STUDY/ 는 기존 보존" 조항은 이제 무효 — 다음 schema 갱신 대상
- `raw/` 에 미수집 소스 2 건 대기: `Advisor.md`, `Harness Engineering.md`

## [2026-04-22] ingest | CLAUDE study.md (사용자의 Claude Code 학습 노트)

- 원본: `raw/CLAUDE study.md` (원 출처: YouTube vxEvo2BLM6A)
- 생성 페이지 (8):
  - Source: [[claude-code-study-notes]]
  - Concepts: [[plan-mode-workflow]], [[wat-framework]], [[context-as-king]]
  - Entities: [[claude-code]], [[skills]], [[sub-agents]], [[mcp]]
- 갱신 페이지 (2): [[llm-wiki-pattern]] (세컨드 브레인 교차참조 보강), `index.md`
- 핵심 주장: **"컨텍스트는 왕"**. Plan Mode · LazyLoading · WAT(Workflows/Agents/Tools) 로 토큰·컨텍스트·에이전트 관심사를 분리하라.
- ⚠️ **Lint 후보 발견**: 이 vault 의 `CLAUDE.md` 가 ~250 줄 → [[context-as-king|LazyLoading]] 원칙에 저촉. 향후 분할·다이어트 제안.
- 📌 **미처리**: 원 노트의 스크린샷 14 장(Compound Engineering, Hooks, Agent Teams, Git worktree 등). 텍스트 요약 없이 임베드만 연결 — 추후 OCR ingest 로 확충.
- 후속 탐구 아이디어: [[git-worktree]], Compound Engineering 개념 페이지, Hooks 엔티티, `raw/Advisor.md`·`raw/Harness Engineering.md` ingest.
