---
title: Harness Engineering 노트
type: source
tags: [harness, ai-safety, claude-code, agent-control, governance]
created: 2026-04-22
updated: 2026-04-22
origin: raw/Harness Engineering.md
---

# Harness Engineering 노트 — 소스 요약

> 원본: `raw/Harness Engineering.md`. 저자 = 사용자 본인.

## 한 줄 요약

에이전트를 더 **똑똑하게** 만드는 "Agentic Engineering" 과 대비되는, 에이전트를 더 **통제·규율**시키는 엔지니어링 분야. 프로젝트에 **커스텀 하네스 층**(CLAUDE.md + docs + hooks + 자동 교정)을 쌓아 실수가 구조적으로 반복 불가능하게 만든다.

## 핵심 주장 6

1. **Claude Code · Codex 엔 내장 하네스가 이미 있다.** 그 위에 프로젝트 맞춤 커스텀 하네스 한 층 더 쌓으면 성능이 크게 개선. (l.3)
2. **커스텀 하네스의 재료**: `CLAUDE.md`(규칙), `docs/`(기획·아키텍처), `hooks/`(자동 검증). 가볍게 시작 가능. (l.9, l.22)
3. **오픈소스 활용**: [[oh-my-claudecode]] 같은 범용 하네스를 가져다 쓰면 **2층이 바로 올라감**. (l.11-13)
4. **기획단계의 결정적 중요성**: "OpenAI 직원 3명이 5개월간 코드 0줄 쓰고 제품 출시" — AI 에 강한 규제를 걸어두고 활용. (l.28-30)
5. **두 가지 축**:
   - **Agentic Engineering** = 에이전트를 더 똑똑·강하게
   - **Harness Engineering** = 에이전트를 더 통제·규율
6. **AI 시대의 새 원칙**: 같은 인풋이 다른 아웃풋을 낳는다 → **실패가 구조적으로 반복 불가능하도록** 시스템 설계.

## 4 개의 기둥 (Harness 의 구성)

1. **기계가 읽는 컨텍스트** — 저장소 안에 AI 런타임 설정 파일 배치 (예: `CLAUDE.md`)
2. **자동 교정 루프 (CI/CD 게이트)** — 린터·구조 테스트·pre-commit hook. **시스템 자체가 실수하지 못하도록**
3. **명시적 도구 경계** — 파일·API·DB·시스템 명령 각각 화이트리스트:
   - 파일: `src/` 읽쓰기, `config/` 읽기만
   - API: 내부 OK, 외부 불가
   - DB: `SELECT` OK, `DROP TABLE` 불가
   - 시스템: 화이트리스트만
4. **지속적 피드백 루프 (Garbage Collection)** — 규칙 위반 감지 → 자동 리팩토링 → 코드 정리 → 아키텍처 체크. 시간이 지날수록 시스템이 더 견고해지는 **진화 사이클**

## 핵심 인용

> **"부탁은 안할 수 있지만, 규칙은 지켜야함."** (l.78)

> **"AI 에이전트가 실수했을 때, 프롬프트를 고치지 마세요. 마구(harness)를 고치세요. 그것이 하네스 엔지니어링의 핵심."** (l.91)

## 기존 위키와의 연결

- [[context-as-king]] 에서 말한 "LazyLoading · 세컨드 브레인" 은 **기계가 읽는 컨텍스트** 기둥의 한 구현.
- [[claude-code]] 의 Hooks 기능은 **자동 교정 루프** 기둥의 Claude Code 표현.
- [[wat-framework]] 의 W(Workflows) 와 공명 — 다만 WAT 는 *조합*, 하네스는 *통제*에 방점.
- **이 vault 자체** 가 하네스 엔지니어링의 실천 예시: `CLAUDE.md`(스키마) + `wiki/`(규약화된 출력) + `raw/`(immutable) + `.omc/`(oh-my-claudecode 상태) 가 전부 하네스의 요소.

## 파생된 위키 페이지

- Concept: [[harness-engineering]]
- Entity: [[oh-my-claudecode]]
- Updated: [[context-as-king]], [[claude-code]], [[wat-framework]], [[llm-wiki-pattern]]

## 스크린샷 (미해석, 추후 OCR)

- `![[스크린샷 2026-04-20 오후 5.12.35.png]]` — 하네스 개요
- `![[스크린샷 2026-04-20 오후 5.16.41.png]]` — oh-my-claudecode
- `![[스크린샷 2026-04-20 오후 5.30.10.png]]` — 커스텀 하네스 요소
- `![[스크린샷 2026-04-20 오후 6.45.46.png]]` — 기획단계 중요성
- `![[스크린샷 2026-04-20 오후 6.49.02.png]]` — 에이전틱 vs 하네스
- `![[스크린샷 2026-04-20 오후 6.58.59.png]]` — 하네스 4단계 작동
- `![[스크린샷 2026-04-20 오후 7.03.07.png]]` — 하네스 4단계 작동
