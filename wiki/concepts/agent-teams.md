---
title: "Agent Teams — Opus 4.6 의 멀티세션 협업 모드"
type: concept
tags: [claude-code, agents, opus-4-6, tmux, parallel, multi-session]
created: 2026-04-28
updated: 2026-04-28
sources: [claude-code-agent-teams-opus46]
aliases: ["Agent Team", "에이전트 팀", "Claude Code Agent Teams", "팀 모드"]
---

# Agent Teams

## 핵심 요약

Agent Teams 는 **Opus 4.6 와 함께 도입된 Claude Code 의 세 번째 사용 모드**. default 와 sub-agent 모드가 *단일 메인 세션* 을 공유하는 반면, agent-team 은 **각 agent 가 자기 세션을 가진다**. 작업은 **team leader** 가 받아 task 로 분해한 뒤 각 팀원 agent 에게 prompt + context 를 주입하고, 팀원끼리는 tmux split-pane 으로 분리되어 사용자가 개별 대화 가능. 핵심 가치: **tunnel-vision 회피** (단일 LLM 의 한 영역 수렴 문제) + **다각도 병렬 조사** (좌/우/위 동시 deploy → 360° 리포트). 트레이드오프: 컨텍스트 격리가 더 강해져 **사전 MD 파일 조립** 의무 + **agent 별 model 지정** 으로 비용 통제 필요.

## 3-Mode 분류 — Claude Code 사용법의 좌표축

| 모드 | 세션 수 | 메인 비유 | 주 용도 |
|---|---|---|---|
| **Default** | 1 (사용자 ↔ 1 agent) | 보안 회사 *control room* | 일반 대화·디버깅·세팅 패널 |
| **Sub-agent** | 1 main + N 단발성 위임 | 한 명을 *3층 5호* 로 출동 | 단일 추가 기능·격리된 탐색 |
| **Agent Team** | **N 병렬 + team leader** | 빌딩 전체 *보안 인력 배치* | 다각도 리서치·대규모 리팩토링·멀티모듈 작업 |

> [!note] Default 의 새 정체성
> Agent Teams 도입 이후 default 모드의 역할은 *시스템 어드민 패널* 로 재정의된다. 모델 토글, skill 점검, project 셋업, 어떤 sub-agent / team 을 띄울지 결정하는 *컨트롤 패널* 로 쓰는 것이 권장.

→ sub-agent 와의 정밀 비교: [[concepts/subagents]]

## 왜 팀이 더 나은가 — Tunnel Vision 해소

단일 LLM 세션은 한 영역으로 *수렴* 한다. 사용자 의견을 echo 하는 *yes-man 증후군* 도 같은 원인. 영상 인용:

> *"These models tend to gravitate towards one specific area and get tunnel vision."*

팀 모드의 답: 각 agent 도 결국 한 영역에 수렴하지만, **여러 agent 를 동시에** 띄우면 좌/우/위 다른 시야가 모인다. team leader 가 사용자 요청을 분해하면서 각 agent 에 *서로 다른 관점* 을 prompt 로 주입한다. 사용자는 360° 통합 리포트만 받는다.

→ 인접 개념: [[concepts/agentic-engineering]] (멀티에이전트 조율 일반론)

## Team Leader — 분배·중재·정리

team leader 는 자동 생성되는 **메타 agent**. 사용자 요청을 받아:

1. *job-to-be-done* 식별 — 어떤 task 가 있는가?
2. **task 분해** — 각 task 의 prompt 와 필요 context 결정.
3. **agent 스폴업** — 인원 수, 역할 라벨, 모델 (영상 권고: 적절히 sonnet/haiku 혼용) 지정.
4. **race condition 차단** — 두 agent 가 같은 task 를 동시에 점유하지 못하게 lock 관리.
5. **shutdown** — 모든 task 완료 시 자동 정리. coding 케이스에선 일부 agent 를 idle 로 남겨 follow-up 대기.

> [!tip] 명령 키워드
> Anthropic 공식 예시는 모두 *"create an agent team"* 으로 시작. 인원수·역할·모델을 같은 한 줄에 박아 두면 team leader 가 그대로 따라간다.

## 메모리 격리 — 가장 빠지기 쉬운 함정

team agent 들은 **main session 의 대화 thread 를 받지 못한다**. default 모드에서 30분 토론한 결정사항, 웹 서치 결과, 파일 탐색 메모는 *팀에 전달되지 않는다*.

**대응 패턴**:

```
1. default 모드에서 합의 도달 → MD 파일에 컨텍스트 패키징
   (예: docs/research-brief.md, docs/refactor-plan.md)
2. team 생성 prompt 안에 "read docs/research-brief.md first" 명시
3. team leader 가 각 agent 의 prompt 에 동일 지시 자동 포함
```

세션 간 지속에는 **shared memory MD** 패턴 — 모든 agent 가 자신의 진척·debug 시도를 한 파일에 append. 다음 세션에서 새 agent 가 이 파일을 먼저 읽고 시작.

→ 토큰·컨텍스트 관점: [[concepts/context-engineering]]

## Setup — tmux + settings.json

```bash
claude update                          # Opus 4.6 활성화에 필요
brew install tmux                       # split pane 의 OS 의존
# settings.json 글로벌 (영상 그대로 복붙):
#   "tmux.split-panes": true
#   "claude-code.experimental-agents": 1
tmux new -s claude                     # 반드시 Claude 실행 *전*
claude --dangerously-skip-permissions  # 또는 일반 claude
```

> [!warning] VS Code/Cursor 내장 터미널 X
> tmux split-pane 은 OS 코어 터미널에서만 동작. IDE 내장 터미널에선 한 패널에 모든 agent 가 떠들어 *interview-in-one-room* 상태가 된다.

## 모델 선택 — 비용 통제의 1차 레버

영상 전반의 강력한 권고: **team 전체에 Opus 4.6 을 깔지 말 것**.

| 레이어 | 권장 모델 |
|---|---|
| Team leader | Opus 4.6 (low effort 부터) |
| 단순 정보 수집 agent | Haiku |
| 합성/추론 agent | Sonnet |
| 복잡 추론·아키텍처 결정 | Opus 4.6 (medium/high) |

Opus 4.6 의 effort 토글 (low → medium → high → max) 은 *체감 품질 vs 토큰 비용* 의 dial. 영상 권고: **low 로 시작 → 막힐 때만 단계 올리기**. max effort 는 *"steering wheel 놓고 floor it"* — 마지막 수단.

→ sub-agent 의 모델 원칙과 동일: [[concepts/subagents#모델-선택-원칙]]

## Skill 부트스트랩 패턴 — Team → Skill

agent team 사용이 반복적이라면 **skill 로 굳히는 메타 워크플로**:

```
1. team 한 번 수동 실행 (예: 리서치 / 리팩토링 / QA 사이클)
2. 결과 만족스러우면 main 세션에서:
   "방금 한 과정을 skill 로 만들어줘. 변수는 topic, model."
3. Claude 가 .claude/skills/<name>/skill.md 자동 생성
4. 새 세션에서 /<skill-name> 으로 호출 (변수와 함께)
5. 프로세스 변경 시 같은 세션에서 "skill 업데이트해줘" 메타 프롬프트
```

> [!tip] 의무 조건
> skill 이 적용되려면 **새 Claude 세션** 필요. 동일 세션에서는 방금 만든 skill 이 슬래시 메뉴에 안 뜬다.

→ 본 패턴이 [[concepts/claude-skills]] 의 *생성 방법* 항목 중 가장 실전적인 경로.

## Tmux Split-Pane UX — 5명 인터뷰의 방 분리

영상 비유: 5명을 같은 방에서 동시에 인터뷰하는 건 불가능. tmux split pane = **각 agent 에 별도 방**. 사용자는:

- 우측 사이드바에서 agent 를 골라 직접 대화 가능 ("hey, how are you going?" 식 prompt)
- 특정 agent 종료 요청을 team leader 에게 전달 ("이 agent 는 더 필요 없어, 정리해 줘")
- agent 가 mission-critical 작업 중이면 종료 요청을 *reject + 사유* 응답

> [!note] 사이드 채널의 가치
> 개별 agent 에 직접 prompt 를 넣을 수 있다는 점이 sub-agent 와 가장 큰 UX 차이. sub-agent 는 결과 요약만 받지만, team agent 는 작업 *도중* 에 사용자가 미세 조정 가능.

## Shutdown — 자동 vs 의도적 idle 유지

| 시나리오 | shutdown 방식 |
|---|---|
| 리서치 (단발성) | team leader 가 모든 task 완료 후 전원 정리 |
| 코딩·리팩토링 | 핵심 agent (예: backend, QA) idle 유지 → 후속 *"X 부분 다시 고쳐줘"* 시 동일 context 로 재진입 |
| 사용자 명시 종료 | team leader 에 *"shut down team"* — 공유 리소스 정리 후 main session 단독 복귀 |

idle 유지의 가치: 그 agent 가 그동안 쌓은 *try/fail* 컨텍스트가 보존된다. 새 agent 로 같은 영역에 들어가면 모든 디버깅 사이드 메모를 잃는다.

## Cost 모니터링

`/cost` 탭에서 세션별 비용 확인. 영상 사례: 4분 세션 \$1.15 (5 agent 병렬, sonnet 위주). 단 영상 본인이 *"API duration 계산이 정확해 보이지 않는다"* 고 인정 — 5명이 5분 33초씩 일했으면 25분 누적이 떠야 하지만 4분으로 표기됨.

> [!warning] cost 정확도
> 멀티 agent 세션의 비용 계산은 신생 기능. 자체 추정으로 *(agent 수 × 세션 분 × 모델 단가)* 를 별도 산정해 두는 게 안전.

## 안티 패턴

- ❌ **default 모드 컨텍스트 의존** — main 에서 토론한 결정을 MD 파일 없이 team prompt 에 *암묵적으로* 의존. 팀원은 모른다.
- ❌ **Opus 4.6 전체 도배** — sonnet/haiku 로 충분한 task 까지 Opus 로 굴리면 cost 가 5–10× 폭증.
- ❌ **VS Code 내장 터미널 사용** — split pane 안 됨. 한 방에 5명이 떠드는 화면.
- ❌ **race condition 직접 관리 시도** — 사용자가 task 를 수동 분배하지 말 것. team leader 의 lock 메커니즘에 맡긴다.
- ❌ **shutdown 강제** — coding 케이스에 핵심 agent 를 일찍 정리하면 follow-up 시 컨텍스트 손실.

## sub-agent vs agent-team — 결정 기준 한 줄

> *코드 한 군데에 단발성 추가 → sub-agent. 빌딩 전체에 다각도 동시 작업 → agent-team.*

## 관련 페이지

- [[concepts/subagents]] — 자매 모드, 격리 패턴의 직접 조상
- [[concepts/agentic-engineering]] — 멀티에이전트 조율 일반론
- [[concepts/context-engineering]] — main session 격리 대응 (사전 MD 조립)
- [[concepts/claude-skills]] — team 워크플로의 skill 화 부트스트랩
- [[concepts/claude-md]] — 사전 컨텍스트 패키징의 정공법
- [[concepts/harness-engineering]] — Worker Isolation 의 더 큰 버전 (N 병렬 격리)
- [[concepts/plan-mode]] — team 생성 prompt 의 task 분해 시 선행 단계
- [[sources/claude-code-agent-teams-opus46]] — 본 페이지의 1차 자료 (22분 핸즈온)

## 출처

- [[sources/claude-code-agent-teams-opus46]] — 모드 분류, setup, team-leader 메커니즘, race condition, skill 부트스트랩 패턴, cost 사례 모두 본 영상에서 추출.
