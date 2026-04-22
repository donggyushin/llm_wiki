---
title: "클로드 코드 2시간 안에 마스터하기 (입문→실전→심화 풀버전)"
type: source
tags: [claude-code, tutorial, video, 한국어]
created: 2026-04-22
updated: 2026-04-22
sources: []
aliases: ["클로드 코드 2시간", "claude-code-mastery-video"]
---

# 클로드 코드 2시간 안에 마스터하기

## 메타

- **유형**: YouTube 통합 강의 (3부작: 입문/실전/심화 풀버전)
- **URL**: https://www.youtube.com/watch?v=vxEvo2BLM6A
- **채널**: 실벨 개발자 (영상 내 자기소개)
- **길이**: 약 2시간
- **원본**: `raw/[전체강의 통합본] 클로드 코드 2시간  안에 마스터하기 ｜ 입문→실전→심화 완전 정복 (풀버전).ko.vtt` (자동 자막, yt-dlp 다운로드)
- **전처리**: `raw/claude-code-2h-mastery-transcript.md` (중복 제거, 2547 라인)

## 핵심 요약

Claude Code 를 **에이전틱 엔지니어링 플랫폼**으로 다루는 실전 튜토리얼. 단순 CLI 사용법이 아니라 **컨텍스트 관리 + 워크플로우 철학 + 자동화 스택**의 통합 관점으로 접근한다.

시리즈 구성:
1. **입문편** — `CLAUDE.md`, 단축키 (Shift+Tab 플랜 모드, Esc 되감기), `/clear` · `/compact` · `/mcp` 필수 명령어, 커스텀 슬래시 명령어
2. **실전편** — 컨텍스트가 왕이다 (lazy loading, 세컨드 브레인, 한 세션 한 피처), 플랜 모드 필수, TDD, TODO.md, WAT 프레임워크
3. **심화편** — Skills (2단계 로딩), Subagents (격리·병렬·체인·재개), Hooks (이벤트 자동화), 통합 데모 (피드백 10건 → 30분 처리)

핵심 메시지: **1인 개발자가 팀급 생산성을 내는 방법 = AI 추론과 코드 실행을 분리하라**.

## 다루는 토픽 전체 목록

### 입문편
- Root 디렉토리에서 `claude` 실행 (`/init` → CLAUDE.md 생성)
- CLAUDE.md 구성 요소: 절대 규칙 · 아키텍처 · 빌드/테스트 명령 · 도메인 컨텍스트 · 코딩 컨벤션 · 핵심 패턴
- 300줄 상한, 폴더별 CLAUDE.md 분할 전략
- 트리거 키워드 (CLAUDE.md 내) vs 커스텀 슬래시 명령어 (`.claude/commands/`) 차이
- Shift+Tab — Default → Plan → Accept Edits 모드 순환
- Esc 1회 중단, Esc 2회 rewind UI (되감기)
- 스크린샷 drag&drop, Mermaid 다이어그램을 아키텍처 설명에 권장
- iTerm 분할 단축키 (Cmd+D, Cmd+Shift+D), Claude 내부 `!` bash 실행
- `/clear`, `/compact`, `/context`, `/mcp`, `/resume`, `/export`, `/output-style`, `/help`, `/config`

### 실전편
- "컨텍스트는 왕이다" — 세컨드 브레인 (로컬 md 누적) + 신기능 `/memory`
- **Lazy loading 전략**: CLAUDE.md 는 참조만, 상세는 별도 파일 (예: `docs/db-schema.md`)
- MCP 는 세션 시작 시 전부 로드 → **최소화 + 커스텀 MCP 권장**
- 한 세션 = 한 피처 원칙 (`/clear` 의존 대신 세션 분리)
- 무거운 데이터는 스크립트로 오프로드 (10만 행 CSV 는 스크립트가 처리, 결과만 Claude 에게)
- Mermaid 다이어그램 아키텍처 저장
- 플랜 모드 필수 (토큰 최적화, 방향 오류 방지)
- TDD 스마트 코딩 (작은 변경 → 테스트 → 커밋 루프)
- Claude 의 **Thinking** 출력을 무시하지 말 것 (잘못된 가정 조기 차단)
- 다른 AI 교차 비평 (`/export` → GPT/Gemini)
- 에러 로그는 통째로 붙여넣기 (해석 금지)
- `TODO.md` 로 세션 간 작업 연속성
- **WAT 프레임워크** = Workflow + Agent + Tools (Ken Kai 유튜버 제안)

### 심화편 (1인 개발자 풀 워크플로우)
- **Skills** = AI 업무 매뉴얼. `skill.md` + 지원 파일
- 2단계 로딩: description 상시 로드 (50~100B), 본문 on-demand → CLAUDE.md 보다 경량
- Skill Creator 플러그인으로 자동 생성
- 저장 범위: 프로젝트 / 개인 / 조직 플러그인 / 세션 일회성 (CLI)
- **Subagents** = 격리 컨텍스트. 병렬·컨텍스트 보호·전문화·재사용
- 4대 패턴: 격리, 병렬, 체인, 재개
- 내장 서브에이전트: Explore (Haiku), Plan, General Purpose, Bash, Claude Code Guide
- 서브에이전트는 Sonnet/Haiku 권장 (Opus 금지 — 토큰 폭발)
- **Agent Teams** (신기능) — 서브에이전트 간 상호 통신
- **Hooks** = 이벤트 자동화 엔진. `PreToolUse` / `PostToolUse` / `Notification` / `Stop`
- 구조: matcher → hooks 배열 → command
- 훅 실행 중 Claude 대기 → timeout 필수
- 멀티 인스턴스 운영 (iTerm 탭별 front/back/db/test)
- Git Worktree 내장 (충돌 방지, 브랜치별 폴더)
- `/voice` 음성 입력 (한국어 지원, 스페이스바 press-to-talk)
- 커스텀 MCP 예: Plan Review MCP (Gemini API 로 플랜 검증)
- `/loop` · 스케줄 자동화 (시간 기반)
- 통합 데모: 클라이언트 피드백 10건 → 음성 → 플랜 → Gemini 리뷰 → 병렬 서브에이전트 → 훅 알림 → 코드 리뷰 플러그인 (5개 리뷰어 병렬) → 커스텀 리포트 스킬 → 30분 완료

## 저자가 반복적으로 강조하는 원칙

1. **컨텍스트 위생이 품질을 결정한다** — 200k 토큰은 금방 찬다, 신선한 컨텍스트 > 부풀어진 컨텍스트
2. **플랜 먼저, 구현 나중** — 계획 세션과 구현 세션 분리
3. **Opus 는 메인, Sonnet/Haiku 는 서브** — 토큰이 총알
4. **작은 단위로 쪼개라** — 큰 스크립트 1개보다 작은 스크립트 여러 개
5. **AI 추론 ↔ 코드 실행 분리** — WAT 프레임워크의 핵심
6. **팀 공유 가능한 자산 만들기** — CLAUDE.md, Skills, Subagents 모두 git 배포 가능 → Compound Engineering (Anthropic 용어)

## 모순 · 주의사항

> [!warning] 모델 이름 혼동
> 영상에서 "오프스 4.6" 등 한국어 발음/자막 오기. 본 위키에서는 정식 표기 (`Opus 4.x`, `Sonnet 4.x`, `Haiku 4.x`) 사용.

> [!note] 자동 자막 노이즈
> "클러드 램 MD", "크달 MD", "크아 MD" 등은 모두 `CLAUDE.md` 의 자동 자막 오인식. 본 위키는 정식 표기로 통일.

> [!question] 출처 확인 필요
> "엔트로픽에서도 플랜 모드로 시작을 모두가 추천을 합니다" 라는 주장은 공식 문서 인용 없음. 추후 Anthropic 문서로 검증 필요.

## 파생 페이지

이 소스에서 파생된 개념 페이지:

- [[concepts/claude-md]]
- [[concepts/plan-mode]]
- [[concepts/context-engineering]]
- [[concepts/claude-skills]]
- [[concepts/subagents]]
- [[concepts/hooks]]

## 동일 저자의 다른 소스

- [[sources/harness-engineering-era]] — 같은 채널의 후속 영상. 이 영상에서 등장한 "세컨드 브레인", "에이전틱 엔지니어" 의 상위 프레임을 정리 → [[concepts/four-axes-ai-development]]
- 저자 페이지: [[entities/silbel-developer]]

## 출처

- 원본 영상: https://www.youtube.com/watch?v=vxEvo2BLM6A
- 자막 파일: `raw/[전체강의 통합본] 클로드 코드 2시간  안에 마스터하기 ｜ 입문→실전→심화 완전 정복 (풀버전).ko.vtt`
- 정제본: `raw/claude-code-2h-mastery-transcript.md`
