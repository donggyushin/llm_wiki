---
title: "Plan Mode — 계획 먼저, 실행 나중"
type: concept
tags: [claude-code, workflow, planning]
created: 2026-04-22
updated: 2026-04-22
sources: [claude-code-2h-mastery]
aliases: ["플랜 모드", "Plan Mode", "Shift+Tab"]
---

# Plan Mode

## 핵심 요약

Claude Code 의 세 모드 중 하나. **파일 쓰기 권한이 없는 상태**에서 Claude 가 **계획만** 세우고 사용자에게 검토받는다. 단순 편의 기능이 아니라 **토큰 최적화 + 방향 오류 방지 + 컨텍스트 위생**이라는 세 가지 이유로 거의 모든 작업의 시작점이 되어야 한다.

## 모드 전환

`Shift + Tab` 으로 순환:

| 모드 | 쓰기 권한 | 용도 |
|---|---|---|
| **Default (Edit on)** | ✅ | Claude 가 자유롭게 파일 생성/수정 |
| **Plan Mode** | ❌ | 계획만, 검토 후 승인 시 실행 |
| **Accept Edits** | ✅ (무조건) | 모든 편집 자동 승인 |

## Plan Mode 를 써야 하는 세 가지 이유

### 1. 토큰 최적화

**잘못된 코드 생성 → 재수정 사이클**은 Default 모드에서 필연적이다. Claude 가 의도를 오해해 바로 파일을 만들면, 사용자가 검토 → 수정 요청 → 재생성으로 이어지며 **같은 작업을 2~3회 반복**한다. Plan 모드는 구현 전에 방향을 합의하므로 이 낭비를 차단한다.

### 2. 컨텍스트 윈도우 보존

파일을 즉시 생성하려면 Claude 는 수많은 관련 파일을 먼저 읽어야 한다 → 컨텍스트가 이미 채워진 상태에서 코딩 시작 → 성능 저하. Plan 모드는 **계획만 있는 가벼운 상태**에서 완료 후 새 세션으로 구현 가능 → fresh context 에서 실행.

### 3. 실행 정확도

사용자가 계획 단계에서 방향을 교정할 수 있다. 잘못된 가정 위에 쌓은 코드는 전부 폐기 대상.

## 권장 워크플로우

```
1. Plan 모드 진입 (Shift+Tab)
2. 작업 설명 ("X 기능 구현")
3. Claude 가 계획 제시 (어떤 파일 수정, 어떤 접근)
4. 사용자 검토 · 피드백
   - 틀렸으면 교정
   - 다른 옵션 요청해서 비교
   - 다른 AI 에 교차 비평 (/export → GPT/Gemini)
5. 합의된 계획 → Accept/Default 모드 전환 → 실행
```

## 관련 단축키

| 단축키 | 동작 |
|---|---|
| `Shift + Tab` | 모드 순환 |
| `Esc` (1회) | 현재 작업 중단 (Interrupted) |
| `Esc` (2회) | **Rewind UI** — 이전 프롬프트로 되감기 |

Esc 2회는 여러 프롬프트를 사용한 상태에서만 유용. 단일 프롬프트 세션에서는 rewind 할 대상 없음.

## 세션 분리 원칙

> Plan 세션과 구현 세션을 분리하라.

1. Plan 모드에서 전체 작업을 여러 단계로 쪼갬
2. `/clear` 또는 새 세션
3. 1단계만 Default 모드로 구현
4. 완료 → 다시 `/clear` → 다음 단계

**한 세션 = 한 피처** 원칙 ([[concepts/context-engineering]]) 과 직결.

## 함께 쓰면 강력한 패턴

- **Plan Review MCP** — Plan 모드 결과를 Gemini API 에 보내 리뷰받는 커스텀 MCP. 두 AI 의 교차 검증으로 플랜 품질 향상.
- **WAT 프레임워크의 W** — Workflow 를 Plain English 로 적는 단계 자체가 Plan 모드 세션.

## 관련 페이지

- [[concepts/context-engineering]] — 세션 위생과 토큰 관리
- [[concepts/claude-md]] — 트리거 키워드도 내부적으로 Plan 에 가까운 워크플로우
- [[concepts/subagents]] — 서브에이전트 결과를 모으는 코디네이터 역할

## 출처

- [[sources/claude-code-2h-mastery]] — 입문편 "키보드 단축키", 실전편 "플랜 모드의 중요성"
