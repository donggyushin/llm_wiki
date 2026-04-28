---
title: "Learn 90% Of Claude Code Agent Teams in 22 Minutes (Opus 4.6)"
type: source
tags: [claude-code, agent-teams, opus-4-6, tmux, video]
created: 2026-04-28
updated: 2026-04-28
sources: []
aliases: ["Agent Teams 22분 가이드", "cSkoaCCmq0w"]
---

# Source — Learn 90% Of Claude Code Agent Teams in 22 Minutes (Opus 4.6)

## 메타

- **유형**: YouTube 영상 (영어 강의), 약 22분
- **URL**: https://www.youtube.com/watch?v=cSkoaCCmq0w
- **저자/채널**: 미확정 (자막 메타에 채널명 미노출 — 후속 확인 필요)
- **인사말**: *"Hello legends"* — 영어권 튜토리얼 채널 스타일
- **수집일**: 2026-04-28 (yt-dlp 영어 자동자막)
- **전처리**: `raw/cskoa-transcript.txt` (중복 제거, 723 라인, 27,753 자)

## 핵심 요약

Opus 4.6 와 함께 도입된 **Agent Teams** 모드의 22분 핸즈온. Claude Code 의 사용 모드를 *(1) default, (2) sub-agent, (3) agent team* 3가지로 정리하고, 각 모드의 **세션 관리 방식 차이** 를 보안팀 비유 (control room → 단일 출동 요원 → 전체 보안팀) 로 설명. tmux 기반 split-pane 설정과 `settings.json` 의 `experimental-agents=1` 활성화 절차, team leader 의 task 분해/race-condition 방지 메커니즘, agent 별 model 지정으로 비용 절감하는 패턴, 그리고 한 세션의 워크플로를 그대로 **skill 로 전환** 하는 메타 프롬프트 부트스트랩 패턴까지 다룬다.

## 비디오 흐름

1. **Overview** — 3-mode 비교 (default / sub-agent / agent-team), tunnel-vision 문제와 다각도 병렬 조사로 해소.
2. **Setup** — `claude update`, tmux 설치, 글로벌 `settings.json` 에 `tmux.split-panes=true` + `claude-code.experimental-agents=1`.
3. **Session 시작** — Claude 실행 *전* tmux session 먼저, 그다음 `claude --dangerously-skip-permissions`, 모델은 Opus 4.6 (200K 또는 1M 컨텍스트, effort 토글 low/medium/high/max).
4. **Team 생성** — *"create an agent team"* 키워드 의무, 인원수/역할/모델까지 prompt 에 명시.
5. **Team UX** — 각 agent 가 별도 tmux pane → 우측 사이드바에서 직접 대화 가능, 개별 agent 종료 요청 가능 (단 agent 가 mission-critical 이면 reject).
6. **Memory** — team agent 들은 main session 의 대화 컨텍스트를 **받지 못함**. 사전 MD 파일 조립 필요. 세션 간 지속은 *shared memory MD* 파일로 모든 agent 가 로그 적재.
7. **Race condition** — Anthropic 측 보장: 동일 task 를 두 agent 가 동시에 잡지 못함 (선점한 쪽이 lock).
8. **Cost** — `/cost` 탭에서 4분 세션 \$1.15 사례 (단, API duration 계산이 정확해 보이지 않다는 영상 본인의 주의).
9. **Shutdown** — team leader 자동 정리. Coding 에선 일부 agent 를 idle 로 남겨 추후 fix 호출 가능.
10. **Skill 화** — 한 번 수동 실행 → *"방금 한 거 skill 로 만들어줘"* 메타 프롬프트 → 변수(topic, model)까지 포함한 `skill.md` 자동 생성. 기존 skill 도 재프롬프트로 업데이트 가능.

## 핵심 인용 (요지)

> *"In default mode, you have one session. In sub-agent mode, you have one session. But in agent team mode, each of these little guys actually has their own session."*

> *"These models tend to gravitate towards one specific area and get tunnel vision."* — 단일 세션의 한계, team mode 의 다각도 병렬화 정당화.

> *"Anthropic actually takes care of this. They have like a race condition check in place that basically whenever I take the task… you cannot take it. We don't double-use tokens on the same tasks."*

> *"You can also define which model you want to use for those team members."* — Opus 4.6 외 sonnet/haiku 혼용으로 비용 절감.

> *"These guys do not get the entire conversation thread."* — main session 컨텍스트 격리. MD 파일 조립이 의무.

> *"Best way to create skills is not to write it from scratch — complete the skill as a process first using Claude Code, then get Claude Code to create that skill for you."* — skill 부트스트랩 패턴.

## 설정 (정확 명령)

```bash
# 1. 최신 Claude
claude update

# 2. tmux 설치 (Mac)
brew install tmux

# 3. 글로벌 settings.json 패치 (영상에서 그대로 복붙한 명령 결과)
#    - tmux.split-panes: true
#    - claude-code.experimental-agents: 1

# 4. tmux 세션 먼저 시작
tmux new -s claude

# 5. 그 안에서 Claude 실행
claude --dangerously-skip-permissions
```

> [!note] tmux 는 코어 터미널에서만
> VS Code · Cursor 내장 터미널에선 split-pane 이 작동하지 않는다. 반드시 OS 의 기본 터미널.

## Sub-agent vs Agent Team — 결정 기준

| 상황 | 권장 모드 |
|---|---|
| 코드베이스에 단일 추가 기능 (예: Google OAuth) | sub-agent |
| 전체 보안 점검 / 대규모 리팩토링 / 다각도 리서치 | agent team |
| 단순 대화·디버깅·CLI 운영 | default |

> 영상 비유: **default** 는 control room 운영자, **sub-agent** 는 한 명을 3층 5호로 보내는 것, **agent team** 은 빌딩 전체를 행사 보안 인력으로 덮는 것.

## 모순/주의

- **Cost 계산 불일치** — 영상 본인이 `/cost` 가 보여준 4분/\$1.15 가 실제 5명 동시 실행 (개별 5분 33초) 시간과 안 맞아 보인다고 인정. 후속 확인 필요.
- **`--dangerously-skip-permissions`** 사용 — 영상에서 그대로 사용했지만 일반적인 보안 권고는 아님 (CLAUDE.md 의 git-workflow 도 별도 경고). 학습용 데모로 한정.
- **컨텍스트 격리의 함의** — sub-agent 와 동일 원리지만, team 의 경우 *팀 리더 ↔ 팀원* 의 prompt 전달 구조이기에 main 의 미묘한 합의 사항은 더 잘 누락된다. → MD 파일 의존이 더 강해진다.

## 관련 페이지

- [[concepts/agent-teams]] — 본 영상에서 추출한 메인 개념 페이지
- [[concepts/subagents]] — 3-mode 분류 정합 + 격리 패턴 공통점
- [[concepts/claude-skills]] — skill 부트스트랩 패턴 (한 번 실행 후 메타 프롬프트로 추출)
- [[concepts/context-engineering]] — main session 격리에 따른 MD 파일 사전 조립
- [[concepts/agentic-engineering]] — 멀티에이전트 조율의 실체

## 출처 (이 source 가 기반한 외부 자료)

- 자막: `raw/Learn 90% Of Claude Code Agent Teams in 22 Minutes (Opus 4.6) [cSkoaCCmq0w].en.vtt`
- Anthropic Opus 4.6 Agent Teams 공식 docs — 영상 내 언급, 정확 URL 미캡처. 후속 lint 시 보강.
