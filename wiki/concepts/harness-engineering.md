---
title: Harness Engineering
type: concept
tags: [harness, governance, ai-safety, pattern, meta-engineering]
created: 2026-04-22
updated: 2026-04-22
sources: [[harness-engineering-note]]
---

# Harness Engineering

## 정의

AI 에이전트가 자율적으로 일하되 **동시에 안전하게 통제될 수 있는 환경**을 프로젝트 수준에서 설계·유지하는 엔지니어링. 프롬프트나 모델이 아니라 **환경(harness) 자체**에 규칙·게이트·경계를 박아 **실수가 구조적으로 반복 불가능**하게 만든다.

## 두 축 — Agentic vs Harness

| 축 | 목표 | 수단 |
|---|---|---|
| **Agentic Engineering** | 에이전트를 더 똑똑·강하게 | 모델, 프롬프트, 도구, 메모리 |
| **Harness Engineering** | 에이전트를 더 통제·규율 | 규칙 파일, CI 게이트, 권한 경계, 교정 루프 |

두 축은 **상호보완**. 아무리 똑똑해도 통제 없으면 위험, 아무리 통제돼도 똑똑하지 않으면 가치 없음.

## 왜 필요한가

- 전통 SW: 같은 인풋 → 같은 아웃풋, 예측 가능
- AI 시대: 같은 인풋 → 다른 아웃풋 가능. 확률적.
- → **prob-by-design 실패 시 시스템이 catch** 해야 함. 개별 실패가 아니라 **실패 반복 불가능성**을 설계.

## 4 개의 기둥

### 1. 기계가 읽는 컨텍스트

- 저장소 안에 AI 런타임 설정 파일 배치
- 예: `CLAUDE.md`, `docs/`, 프로젝트별 스키마
- **정적 규칙**을 문서로 박제
- → [[context-as-king]] 의 LazyLoading 원칙이 이 기둥의 구현 전술

### 2. 자동 교정 루프 (CI/CD 게이트)

- Linter, 구조 테스트, pre-commit hook
- **AI 는 규칙을 읽고도 무시할 수 있다** — 읽기만으론 부족
- 시스템 자체가 실수를 **거부**하도록
- → [[claude-code]] 의 Hooks 기능이 로컬 예시

### 3. 명시적 도구 경계

권한을 **화이트리스트**로:

| 영역 | 허용 | 금지 |
|---|---|---|
| 파일시스템 | `src/` 읽쓰기, `config/` 읽기만 | `config/` 쓰기 |
| API | 내부 API | 외부 서비스 |
| DB | `SELECT` | `DROP TABLE` |
| 시스템 | 화이트리스트 명령 | 임의 shell |

### 4. 지속적 피드백 루프 (Garbage Collection)

- 규칙 위반 감지 → 자동 리팩토링 → 코드 정리 → 아키텍처 체크
- **진화 사이클** — 시간이 지날수록 하네스가 더 견고해짐

## 핵심 원칙

> **"부탁은 안할 수 있지만, 규칙은 지켜야 함."**

> **"AI 가 실수했을 때 프롬프트를 고치지 말고 하네스를 고쳐라."**

→ 개별 실수 수정이 아니라 **재발 방지 구조**에 작업을 투자하라는 선언.

## 기획 단계의 절대 중요성

저자 인용 (원 소스 l.28-30):

> "OpenAI 직원 3명이 5개월간 코드 0 줄 쓰고 제품을 출시함. AI 가 안전하게 코드를 작성할 수 있게 규제를 강하게 걸어놓고 AI 를 활용함."

→ **구조가 코드보다 먼저**. 규칙을 먼저 박아야 AI 속도의 혜택을 안전하게 받는다.

## 이 vault 자체가 Harness 의 실천 예시

| 하네스 요소 | 이 vault 의 대응 |
|---|---|
| 기계가 읽는 컨텍스트 | `CLAUDE.md` (위키 스키마) |
| 자동 교정 루프 | Lint 오퍼레이션 ([[wiki-operations]]) |
| 명시적 도구 경계 | `raw/` = read-only, `wiki/` = LLM 소유, `STUDY/Resources/` = 외부 (스키마 명시) |
| 피드백 루프 | `log.md` append-only + 주기 lint |

→ [[llm-wiki-pattern]] 은 **지식관리**에 하네스 엔지니어링을 적용한 한 사례.

## 실천 도구·프레임워크

- [[oh-my-claudecode]] — 범용 하네스 오픈소스. 바로 "2층" 쌓기 좋음
- [[claude-code]] 의 CLAUDE.md + Hooks + `.claude/settings.local.json`
- git pre-commit hooks, linters, type checkers
- CI (GitHub Actions 등) 게이트

## 관련 개념

- [[context-as-king]] — 첫째 기둥(기계가 읽는 컨텍스트)의 구체 전술
- [[wat-framework]] — W(Workflows)는 하네스가 강제하는 흐름
- [[plan-mode-workflow]] — 기획 단계 중요성의 작은 표현
- [[llm-wiki-pattern]] — 이 vault 가 구현한 하네스 인스턴스

## 근거 소스

- [[harness-engineering-note]]
