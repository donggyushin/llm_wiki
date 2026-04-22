---
title: oh-my-claudecode (OMC)
type: entity
tags: [tool, harness, opensource, claude-code, framework]
created: 2026-04-22
updated: 2026-04-22
sources: [[harness-engineering-note]]
---

# oh-my-claudecode (OMC)

## 한 줄 정의

Claude Code 위에 올리는 **범용 하네스 오픈소스**. 멀티 에이전트 오케스트레이션 · 스킬 · 트리거 키워드 · 훅 · 상태 관리를 묶어 **AI 가 일하는 방식 자체를 체계적으로** 만들어주는 프레임워크.

## 이 vault 와의 관계

**현재 사용 중.** 흔적:
- `.omc/` 디렉터리 (상태·세션·메모리·project-memory.json)
- `.gitignore` 의 `.omc/` 항목 (tooling state 외부 공유 배제)
- 이 vault 의 CLAUDE.md 는 사용자의 글로벌 OMC 설정 (`~/.claude/CLAUDE.md`)과 함께 동작

## 원 소스의 평가

> "이런 오픈소스를 가져다 쓰면 바로 2층이 올라감. 시작하기 좋은 범용 하네스. AI 가 일하는 방식 자체를 되게 체계적으로 만들어줌."
> — [[harness-engineering-note]] l.13

**"2층"**: 1층 = Claude Code 내장 하네스, 2층 = OMC 같은 범용 하네스, 3층 = 프로젝트 커스텀 하네스.

## Harness Engineering 4 기둥에서의 위치

| 기둥 | OMC 의 제공 |
|---|---|
| 기계가 읽는 컨텍스트 | CLAUDE.md 컨벤션·rules/·skills registry |
| 자동 교정 루프 | hooks (PreToolUse, PostToolUse, Stop) |
| 명시적 도구 경계 | settings.local.json 권한 모델, agent 라우팅 |
| 피드백 루프 | project-memory, state, notepad, session logs |

## 주요 기능 (사용자의 글로벌 CLAUDE.md 기반)

- **다중 에이전트**: executor · architect · explore · planner · designer · writer · critic · debugger · tracer · verifier · ...
- **스킬 티어**: autopilot · ultrawork · ralph · team · ralplan 등 Tier-0 워크플로
- **트리거 키워드**: `autopilot`, `ralph`, `ulw`, `ccg`, `deep interview`, `deslop` 등
- **컨텍스트 허브**: `chub`, `context-manager`
- **상태 관리**: state read/write/clear, notepad, session_search, trace

## 관련 엔티티·개념

- [[claude-code]] — 호스트 도구
- [[harness-engineering]] — 이 도구가 구현하는 패러다임
- [[wat-framework]] — OMC 는 WAT 의 Tools 층에서 강력한 번들 역할

## 근거 소스

- [[harness-engineering-note]] l.11-15
- 이 vault 의 `.omc/` 디렉터리 존재 (실증)
- 사용자의 `~/.claude/CLAUDE.md` (OMC 글로벌 설정)
