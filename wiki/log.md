---
title: "Wiki Log"
type: meta
---

# Wiki Log

Append-only 연대기. 각 항목은 `## [YYYY-MM-DD] <op> | <title>` 형식.

빠른 조회: `grep "^## \[" wiki/log.md | tail -5`

---

## [2026-04-22] bootstrap | 위키 시스템 초기화
- 생성: [[concepts/llm-wiki-pattern]], `wiki/index.md`, `wiki/log.md`
- 스키마 확정: `CLAUDE.md` (3-layer 아키텍처, 4대 operation, 페이지 포맷)
- 다음 단계: `raw/` 에 첫 소스 드롭 → ingest 시작

## [2026-04-22] ingest | 클로드 코드 2시간 안에 마스터하기 (YouTube 3부작 풀버전)
- 소스: `raw/[전체강의 통합본] 클로드 코드 2시간  안에 마스터하기 ｜ 입문→실전→심화 완전 정복 (풀버전).ko.vtt` (yt-dlp 한국어 자동자막)
- 전처리: `raw/claude-code-2h-mastery-transcript.md` (중복 제거, 2547 라인)
- 생성 (Source): [[sources/claude-code-2h-mastery]]
- 생성 (Concepts): [[concepts/claude-md]], [[concepts/plan-mode]], [[concepts/context-engineering]], [[concepts/claude-skills]], [[concepts/subagents]], [[concepts/hooks]]
- 갱신: `wiki/index.md` (1 → 8 페이지)
- 메모: 핵심 6개 개념만 선별. 통합 데모 · WAT 프레임워크 · 음성입력 · worktree · `/loop` 등 세부 토픽은 소스 페이지에 요약만 유지 (필요 시 별도 페이지로 분리 가능)
