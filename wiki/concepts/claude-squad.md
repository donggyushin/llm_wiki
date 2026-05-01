---
title: "Claude Squad — tmux + worktree 기반 병렬 Claude 세션 운영"
type: concept
tags: [claude-code, tmux, worktree, parallel, productivity, automation]
created: 2026-05-01
updated: 2026-05-01
sources: [work-prep-2026-05-01]
aliases: ["claude-squad", "Claude Squad", "병렬 Claude", "worktree 병렬"]
---

# Claude Squad

## 핵심 요약

**Claude Squad** 는 `tmux` (터미널 다중 세션 매니저) + `git worktree` (한 저장소의 여러 브랜치를 동시에 별도 디렉토리로 체크아웃) 를 결합해 **여러 Claude Code 세션을 동시에 돌리는 패턴/도구**. 핵심 가치는 *독립적으로 진행 가능한 작업들을 직렬이 아니라 병렬로 처리*. 한 세션이 빌드 / 디버깅 / 리서치로 블로킹된 동안 다른 세션이 다른 피처를 진행. 본 위키 사용자는 이를 *AI 비즈니스 임팩트 사례* 의 한 축으로 사용 ([[sources/work-prep-2026-05-01]]).

> [!note] 명칭 주의 — Anthropic 공식 *Agent Teams* 와 다름
> [[concepts/agent-teams]] 는 Opus 4.6 이 도입한 **공식 멀티세션 모드** (team leader 가 task 를 분해해 N agent 에게 분배). Claude Squad 는 그보다 한 단계 더 **외부적·OS 수준** 에서 *세션 자체* 를 병렬로 띄우는 운영 기법. Worktree 가 핵심이라는 점에서 Agent Teams 보다 **격리 강도가 더 강하다** (파일시스템·git index 까지 분리).

## 두 핵심 구성 요소

### 1) `git worktree` — 동일 저장소, 다중 작업 트리

`git worktree` 는 한 저장소에서 **여러 브랜치를 동시에 별도 폴더로 체크아웃** 하는 git 기능.

```bash
# 기본 사용
git worktree add ../proj-feat-auth feat/auth
git worktree add ../proj-fix-billing fix/billing
```

결과:
- `proj/` — main 브랜치
- `proj-feat-auth/` — `feat/auth` 브랜치
- `proj-fix-billing/` — `fix/billing` 브랜치

각 디렉토리는 **독립적 working tree** 다. `node_modules`, 빌드 산출물, IDE 인덱스도 완전히 분리. **한 브랜치의 변경이 다른 브랜치의 작업을 안 깬다**.

### 2) `tmux` — 다중 세션 매니저

`tmux` 는 한 터미널 안에서 여러 세션·창·팬을 띄우고 관리하는 도구. SSH 끊어져도 세션 유지. split-pane 으로 동시 보기 가능.

```bash
tmux new -s squad           # 세션 생성
# Ctrl+b , c     → 새 윈도우
# Ctrl+b , %     → 수직 split
# Ctrl+b , "     → 수평 split
```

### 3) 결합 — Claude Squad 패턴

```
tmux session "squad"
├── window 1: cd ../proj-feat-auth   && claude   # 인증 피처
├── window 2: cd ../proj-fix-billing && claude   # 결제 버그
├── window 3: cd ../proj-refactor    && claude   # 리팩토링
└── window 4: cd ../proj             && claude   # main 컨트롤
```

각 윈도우 = 독립 worktree + 독립 Claude 세션. 사용자가 윈도우를 옮겨다니며 동시에 4개 작업 진행.

## 언제 쓰는가 — 적합한 작업 유형

| 적합 | 부적합 |
|---|---|
| 서로 의존성 없는 피처 (auth / billing / 리팩토링) | 같은 파일을 동시 수정해야 하는 작업 |
| 한 작업이 빌드·테스트로 자주 블로킹됨 | 한 세션 안에서의 깊은 컨텍스트 누적이 필수인 작업 |
| 리서치 · 코드 탐색 · 다른 PR 리뷰 병행 | 컨텍스트 길이가 짧고 직렬이 더 빠른 작업 |
| 모노레포의 분리된 패키지 | 머지 충돌이 빈번할 작업 |

## 운영 주의사항

### 컨텍스트 분리는 자동 — 하지만 *지식 공유* 는 수동

각 worktree 의 Claude 세션은 **서로의 메모리를 모른다**. A 세션에서 결정한 패턴을 B 세션이 따르려면:

- `CLAUDE.md` 에 결정을 기록 → 모든 worktree 가 공유 (파일은 동일 저장소)
- `docs/decisions.md` ADR 패턴
- `/memory` 자동 메모리 (단, worktree 마다 별도일 수 있음 — 확인 필요)

→ [[concepts/context-engineering#2-세컨드-브레인]]

### 머지 시점 충돌 관리

각자 다른 브랜치니 *작업 중* 충돌은 없지만, **PR 단계에서 충돌**. 작업을 분배할 때 같은 모듈을 두 worktree 가 동시에 수정하지 않도록 사전 분리.

### 리소스 — 빌드/테스트 동시 실행

4개 worktree 모두 `npm install` / `pnpm dev` 를 동시에 띄우면 RAM·CPU 빠르게 소진. 빌드는 직렬 또는 캐시 공유 (turbo/nx) 권장.

### `worktree` 정리

작업 끝난 worktree 는 명시적 제거 필요:

```bash
git worktree remove ../proj-feat-auth
git worktree prune
```

## Agent Teams 와의 비교

| 축 | Claude Squad | [[concepts/agent-teams]] |
|---|---|---|
| 격리 단위 | OS 프로세스 + 파일시스템 + git working tree | Claude Code 내부 세션 |
| 통합 주체 | 사람 (사용자) | team leader agent |
| 사전 컨텍스트 패키징 | `CLAUDE.md` + ADR | 별도 brief MD 의무 |
| 작업 분해 | 사람이 사전 분리 | team leader 가 자동 분해 |
| 적합 | 독립 피처 병렬 | 다각도 리서치, tunnel-vision 회피 |
| 도입 비용 | tmux + worktree 설정 | Opus 4.6 + 명령어만 |

> [!note] 함께 쓸 수 있는가
> 가능. Claude Squad 의 한 윈도우에서 Agent Teams 모드를 띄우면 *프로세스 병렬 위에 세션 병렬* 이 쌓인다. 단, 사용자 주의력이 한계.

## 본 위키 사용자 사례

[[sources/work-prep-2026-05-01]] 의 Q3 *AI 비즈니스 임팩트 사례* 에서 Claude Squad 를 병렬화 사례로 명시:

> *"claude squad(tmux + worktree)로 병렬로 처리할 수 있는 작업들 병렬화"*

함께 언급된 자동화: Slack/Figma/Jira URL → 작업 티켓 자동 생성 스킬. 둘 다 *반복 가능한 자동화 자산* 이라는 공통점.

## 관련 페이지

- [[concepts/agent-teams]] — 비교 대상. 한 단계 안쪽의 격리.
- [[concepts/subagents]] — 더 안쪽 격리 (단일 메인 세션 내 위임).
- [[concepts/context-engineering]] — 세션 위생 / `CLAUDE.md` 공유 전략.
- [[concepts/claude-md]] — 다중 worktree 가 공유하는 규칙 파일.
- [[concepts/harness-engineering]] — *시스템적 격리* 의 한 형태.

## 출처

- [[sources/work-prep-2026-05-01]] — Q3 비즈니스 임팩트 사례. *tmux + worktree* 명시.
