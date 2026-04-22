---
title: "Hooks — 이벤트 기반 자동화 엔진"
type: concept
tags: [claude-code, hooks, automation]
created: 2026-04-22
updated: 2026-04-22
sources: [claude-code-2h-mastery, harness-engineering-era]
aliases: ["Hooks", "훅스", "PreToolUse", "PostToolUse"]
---

# Hooks

## 핵심 요약

Hooks 는 Claude Code 의 **이벤트 트리거 자동화**. 특정 이벤트 (도구 실행 전후 · 응답 대기 · 턴 종료) 발생 시 셸 명령을 자동 실행한다. 흐름은 **이벤트 → matcher 검사 → command 실행**. Skills 가 "요청 기반" 이라면 Hooks 는 **"이벤트 기반"**.

## 4대 이벤트 타입

| 이벤트 | 시점 | 용도 |
|---|---|---|
| **PreToolUse** | 도구 호출 직전 | 입력값 검증, 승인/차단, 파라미터 수정 |
| **PostToolUse** | 도구 실행 직후 | 결과 검증, lint/format 같은 후처리 |
| **Notification** | 사용자 응답 대기 시 | 알림 전송, 외부 서비스 연동 |
| **Stop** | 에이전트 턴 종료 시 | 최종 정리, 보고서 생성, 상태 저장 |

### 전체 흐름

```
PreToolUse → 도구 실행 → PostToolUse → ... → Notification / Stop
```

## 구조

`.claude/settings.json` 에 정의:

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"응답이 완료되었습니다\" with title \"Claude Code\" sound name \"Glass\"'",
            "timeout": 10
          }
        ]
      }
    ]
  }
}
```

| 필드 | 의미 |
|---|---|
| `matcher` | 어떤 상황에서 매칭할지 |
| `hooks[].type` | 훅 종류 (`command`) |
| `hooks[].command` | 실제 실행할 셸 명령 |
| `hooks[].timeout` | 타임아웃 (초) |

## 예제 — macOS 알림 훅

**목표**: Claude 응답 완료 시 알림 + 사운드.

**생성 방법**: 수동 JSON 편집도 되지만 **Claude 에게 시키는 게 제일 쉽다**.

```
"Claude 가 응답 대기 상태에 들어가면 macOS 알림과 효과음이 나오는 훅을 만들어줘"
```

→ Claude 가 `settings.json` 에 Stop 훅 등록.

**효과**: 긴 작업 중 다른 일 하다가 완료 알림으로 바로 복귀 가능. 생산성 향상 팁으로 저자가 강조.

## 적용 사례

| 이벤트 | 사용 예 |
|---|---|
| PreToolUse | 위험한 bash 명령 차단, `.env` 접근 방어 |
| PostToolUse | 파일 저장 후 자동 `prettier`/`ruff` 실행 |
| Notification | Slack · Discord · 이메일 전송 |
| Stop | 세션 요약, 커밋 메시지 초안 생성 |

## 주의 사항

### 훅 실행 중 Claude 는 대기

훅이 끝날 때까지 Claude 는 다음 작업을 못 한다. **타임아웃 필수**.

### 무거운 작업 금지

- ❌ 훅 안에서 테스트 전체 실행 (수 분 소요)
- ✅ 백그라운드로 분리 (`nohup command &`) 하거나 알림 전송 같은 가벼운 작업만

## Skills · Subagents · Hooks 역할 분담

| | 트리거 | 목적 |
|---|---|---|
| **Skills** | Claude 가 요청 의미 해석 | 반복 작업 매뉴얼 |
| **Subagents** | 메인이 명시적 위임 | 컨텍스트 격리 + 병렬 |
| **Hooks** | 이벤트 자동 | 프로세스 자동화 · 알림 · 정책 강제 |

셋이 함께 쓰일 때 힘이 배가된다. 예 (저자 통합 데모):

```
음성 입력 → Plan Mode
→ Subagent 병렬 (프런트·백·테스트)
→ Stop Hook 이 완료 알림 발송
→ 사용자가 돌아와 Code Review Skill 트리거
```

## 커스텀 MCP 와의 차이

| | MCP | Hook |
|---|---|---|
| 호출 주체 | Claude 가 필요하다고 판단하면 | 이벤트 발생 시 자동 |
| 예시 | "DB 쿼리해줘" → Claude 가 DB MCP 호출 | 파일 저장 후 자동 lint |

MCP = Claude 주도, Hook = 시스템 주도.

## 확장: `/loop` · 스케줄 자동화

Hooks 가 **이벤트 기반** 자동화라면, `/loop` 는 **시간 기반** 자동화:

```
/loop 5m 배포 상태 확인해줘
```

5분마다 Claude 가 자동 실행. 자연어로 리마인더도 가능.

## 하네스 엔지니어링 관점

Hooks 는 [[concepts/harness-engineering|Harness Engineering]] 의 **2번째 기둥 (System Enforcement)** 의 핵심 메커니즘. Pre-commit Hook 으로 규칙을 시스템에 내장하면, AI 가 규칙을 읽고도 어기는 문제를 **구조적으로 차단**할 수 있다. "프롬프트는 부탁, 훅은 강제" — 하네스 철학의 실체.

## 관련 페이지

- [[concepts/harness-engineering]] — 2번째 기둥의 구현 메커니즘
- [[concepts/claude-skills]] — 요청 기반 자동화
- [[concepts/subagents]] — 명시적 위임 자동화
- [[concepts/context-engineering]] — 컨텍스트 외부 자동화로 오염 방지

## 출처

- [[sources/claude-code-2h-mastery]] — 심화편 "Hooks" 섹션 + 통합 데모
- [[sources/harness-engineering-era]] — System Enforcement 기둥 해설
