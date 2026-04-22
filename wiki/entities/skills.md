---
title: Skills (Claude Code)
type: entity
tags: [claude-code, feature, extension, manual]
created: 2026-04-22
updated: 2026-04-22
sources: [[claude-code-study-notes]]
---

# Skills

## 한 줄 정의

Claude Code 의 확장 기능. **AI 에게 주는 업무 매뉴얼** — 특정 작업 유형의 절차·규약을 마크다운으로 기술해 필요 시 Claude 가 참조하게 한다.

## 왜 좋은가

- **비활성 시 컨텍스트 거의 안 먹음** — `/memory` 나 MCP 대비 가벼움 (l.134)
- **간단 작업엔 MCP 보다 Skills 가 적절** — MCP 는 시작부터 컨텍스트 점유
- **로컬 파일**이라 버전 관리·공유 쉬움

## 만드는 법

- `skill-creator` 플러그인으로 원하는 Skill 생성 (l.139)
- 스크린샷 참조:
  - `![[스크린샷 2026-04-17 오후 5.20.38.png]]`
  - `![[스크린샷 2026-04-17 오후 5.21.24.png]]`
  - `![[스크린샷 2026-04-17 오후 5.23.32.png]]`
  - `![[스크린샷 2026-04-17 오후 5.28.40.png]]`
  - `![[스크린샷 2026-04-17 오후 5.29.01.png]]`

> 📌 스크린샷 내용은 아직 텍스트로 요약되지 않음. 추후 별도 ingest 로 OCR·해석 시 페이지 확충 예정.

## MCP 와의 경계

| 판단 | 선택 |
|---|---|
| 외부 시스템 실시간 연결 필요 | [[mcp]] |
| 로컬 매뉴얼·규약·절차 | **Skills** |
| 자주 안 쓰지만 있으면 좋은 능력 | **Skills** (비활성 시 무부담) |
| 커스텀 깊이 필요 | 커스텀 MCP |

## 이 vault 에서의 관련성

- 이 위키의 [[wiki-operations|Ingest/Query/Lint]] 자체가 Skills 로 포팅 가능 — 각 오퍼레이션을 별도 Skill 파일로 분리하면 [[context-as-king|LazyLoading]] 극대화

## 관련 엔티티

- [[claude-code]] — 호스트 도구
- [[mcp]] — 보완적·대립적 확장 수단
- [[sub-agents]] — 또 다른 확장 수단

## 근거 소스

- [[claude-code-study-notes]] l.126-139
