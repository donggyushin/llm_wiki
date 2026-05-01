---
title: "Wiki Log"
type: meta
---

# Wiki Log

Append-only 연대기. 각 항목은 `## [YYYY-MM-DD] <op> | <title>` 형식.

빠른 조회: `grep "^## \[" wiki/log.md | tail -5`

---

## [2026-04-22] bootstrap | 위키 시스템 초기화
- 생성: [[concepts/llm-wiki-pattern]], `wiki/index.md`, `wiki/log.md`
- 스키마 확정: `CLAUDE.md` (3-layer 아키텍처, 4대 operation, 페이지 포맷)
- 다음 단계: `raw/` 에 첫 소스 드롭 → ingest 시작

## [2026-04-22] ingest | 클로드 코드 2시간 안에 마스터하기 (YouTube 3부작 풀버전)
- 소스: `raw/[전체강의 통합본] 클로드 코드 2시간  안에 마스터하기 ｜ 입문→실전→심화 완전 정복 (풀버전).ko.vtt` (yt-dlp 한국어 자동자막)
- 전처리: `raw/claude-code-2h-mastery-transcript.md` (중복 제거, 2547 라인)
- 생성 (Source): [[sources/claude-code-2h-mastery]]
- 생성 (Concepts): [[concepts/claude-md]], [[concepts/plan-mode]], [[concepts/context-engineering]], [[concepts/claude-skills]], [[concepts/subagents]], [[concepts/hooks]]
- 갱신: `wiki/index.md` (1 → 8 페이지)
- 메모: 핵심 6개 개념만 선별. 통합 데모 · WAT 프레임워크 · 음성입력 · worktree · `/loop` 등 세부 토픽은 소스 페이지에 요약만 유지 (필요 시 별도 페이지로 분리 가능)

## [2026-04-22] ingest | 프롬프트 엔지니어링은 끝났습니다: 이제 '하네스'의 시대입니다
- 소스: `raw/프롬프트 엔지니어링은 끝났습니다： 이제 '하네스'의 시대입니다.ko.vtt` (yt-dlp 한국어 자동자막)
- 전처리: `raw/harness-engineering-era-transcript.md` (중복 제거, 558 라인)
- 저자: 실벨 개발자 ([[sources/claude-code-2h-mastery]] 와 동일 채널)
- 생성 (Source): [[sources/harness-engineering-era]]
- 생성 (Concepts): [[concepts/harness-engineering]], [[concepts/agentic-engineering]], [[concepts/four-axes-ai-development]]
- 생성 (Entities): [[entities/martin-fowler]], [[entities/silbel-developer]]
- 갱신 (Concepts): [[concepts/context-engineering]] (4축 중 2번째 축 맥락 추가), [[concepts/claude-md]] (하네스 1기둥 연결), [[concepts/hooks]] (하네스 2기둥 메커니즘), [[concepts/subagents]] (Worker Isolation 패턴)
- 갱신 (Source): [[sources/claude-code-2h-mastery]] (동일 저자 교차참조)
- 갱신: `wiki/index.md` (8 → 14 페이지)
- 미해결: [[entities/martin-fowler]] 의 "harness" · "garbage collection" 용어 귀속 1차 출처 미확인 (영상 저자의 주장에 의존), OpenAI 2026-02 블로그 URL 미제공 — 추후 lint 시 확인 플래그

## [2026-04-22] lint
- orphan 0 · stale 0 · 모순 0 · broken 3 · missing 4(유의미) · oversized 0 · gap 4
- 리포트: [[syntheses/lint-2026-04-22]]
- 최우선 조치: [[concepts/rag]] 생성 (broken link 해결 + llm-wiki-pattern 가독성 향상)
- 다음 라운드: [[entities/andrej-karpathy]], Agent Teams 독립 페이지, WAT Framework 독립 페이지, Gap 4블록 웹검색 검증

## [2026-04-23] ingest | 20년 만에 밝혀진 메이플 확률문제의 원인
- 소스: `raw/20년 만에 밝혀진 메이플 확률문제의 원인 [RVngRYqs7kA].ko.vtt` (yt-dlp 자동자막)
- 저자: 코딩애플 (https://www.youtube.com/@codingapple)
- URL: https://youtu.be/RVngRYqs7kA
- 생성 (Source): [[sources/maple-modulo-bias]]
- 생성 (Concepts): [[concepts/modulo-bias]], [[concepts/prng-rand]]
- 생성 (Entities): [[entities/coding-apple]], [[entities/maplestory]], [[entities/nexon]]
- 갱신: `wiki/index.md` (15 → 21 페이지)
- 도메인: 새 섬 진입 — 게임/PRNG/확률. 기존 AI·하네스 도메인과 교차 참조 없음 (의도적 분리).
- 미해결: 메이플 실제 PRNG 함수 비공개 (rand 가정은 영상 저자 명시), "17% 경계" 수치 해석적 재산출 여지.

## [2026-04-23] lint
- orphan 0 · stale 0 · 모순 0 · broken 3(이월) · missing 0(신규) · oversized 0 · gap 5 blocks
- 리포트: [[syntheses/lint-2026-04-23]]
- 평가: 신규 ingest 6페이지 orphan 0 / broken 신규 0 / 모순 0 — 새 도메인 자체 클러스터 내부 연결성 양호.
- 최우선 조치: 이월 broken 3건 중 [[concepts/rag]] 우선 생성 권고. 나머지 2건(memex, andrej-karpathy) 후속 ingest 시 자연 생성 대기.

## [2026-04-23] quick-add | concepts/rag 페이지 생성
- 트리거: lint-2026-04-22 · lint-2026-04-23 공통 P0 권고 (broken link 해소 + llm-wiki-pattern 문맥 보강)
- 생성: [[concepts/rag]] — 파이프라인 · 구성요소 · 변형(GraphRAG/HyDE/Self-RAG/Agentic RAG) · LLM Wiki 대비표
- 갱신: [[concepts/llm-wiki-pattern]] (관련 페이지 섹션 활성 링크로 전환, updated 2026-04-23)
- 갱신: `wiki/index.md` (22 → 23 페이지)
- 잔여 broken link: 2건 ([[concepts/memex]], [[entities/andrej-karpathy]]) — 후속 ingest 대기
- 출처: 씨앗은 [[concepts/llm-wiki-pattern]] 대비 서술 + 일반 RAG 문헌. 특정 논문 raw ingest 는 미수행 → `[!question]` 블록으로 표시

## [2026-04-23] ingest | 클로드 코드 대규모 프로젝트 컨텍스트 엔지니어링 - .claude/rules 규칙 쪼개기
- 소스: `raw/클로드 코드 대규모 프로젝트 컨텍스트 엔지니어링 - .claude⧸rules 규칙 쪼개기 [FBv8hK_DtJ8].ko.vtt` (yt-dlp 자동자막)
- 전처리: `raw/jimcoding-claude-rules-transcript.md` (중복 제거, 298 라인)
- 저자: 짐코딩 (https://www.youtube.com/channel/UCZ30aWiMw5C8mGcESlAGQbA)
- URL: https://youtu.be/FBv8hK_DtJ8 (2026-03-26 업로드, 11분 25초)
- 생성 (Source): [[sources/jimcoding-claude-rules]]
- 생성 (Concept): [[concepts/conditional-rule-loading]] — `.claude/rules/*.md` + frontmatter glob 패턴으로 파일 타입별 자동 로드. Lazy Loading 의 구체 구현.
- 생성 (Entity): [[entities/jimcoding]]
- 갱신 (Concepts): [[concepts/claude-md]] (비대화 실패 모드 + rules 디렉터리 섹션 추가), [[concepts/context-engineering]] (lazy loading 에 `.claude/rules` 사례 추가), [[concepts/claude-skills]] (관련 페이지에 conditional-rule-loading 대비 링크)
- 갱신: `wiki/index.md` (23 → 26 페이지)
- 도메인: Claude Code 실전 메모리 관리 — 기존 AI·하네스 클러스터 확장. 오픈소스 레퍼런스 (Trigger.dev, CockroachDB) 는 1회 언급으로 entity 생성 보류.
- 미해결: Reddit 47K 단어 원글 URL · Anthropic 공식 frontmatter 필드명 (`globs`/`applyTo` 등) — 두 건 모두 `[!question]` 블록으로 기록. 공식 docs 교차 확인 시 보강.

## [2026-04-25] ingest | 클로드 코드 800시간 쓰고 깨달은 9가지 꿀팁
- 소스: `raw/클로드 코드 800시간 쓰고 깨달은 9가지 꿀팁 [hXlB1QstQ-Y].ko.vtt` (yt-dlp 자동자막)
- 저자: Jay Choi · 인디해커 라이프
- URL: https://youtu.be/hXlB1QstQ-Y (2026-04-23 업로드, 8분 30초)
- 생성 (Source): [[sources/jay-choi-9-tips]]
- 생성 (Concepts): [[concepts/fresh-context-principle]] (한 줄 표어 페이지화 — 4단계 줌으로 다른 기법 묶음), [[concepts/rewind-pattern]] (Esc×2 + /clear + /compact 방향 지정), [[concepts/reward-hacking]] (검증 루프 악용 — 테스트 자체를 코드에 맞춰 수정)
- 생성 (Entities): [[entities/jay-choi]], [[entities/boris-cherny]] (Claude Code 창시자, vanilla 일화 + 검증 1순위 발언 출처)
- 갱신 (Concepts): [[concepts/claude-md]] (80% 준수 통념 + 150~200줄 권장), [[concepts/plan-mode]] (플랜 적극 편집 + 명시적 지시 원칙), [[concepts/claude-skills]] (남의 스킬 먼저 채택 순서), [[concepts/context-engineering]] (리와인드 우선 + /compact 방향 지시), [[concepts/harness-engineering]] (vanilla 우선 원칙 + reward-hacking 연결)
- 갱신: `wiki/index.md` (26 → 32 페이지)
- 도메인: Claude Code 실전 사용 — 기존 클러스터(claude-md, plan-mode, skills, context-engineering, harness) 깊이 보강. 첫 인디 개발자 회고 시점 소스.
- 미해결: 보리스 X 게시 원문 URL · Anthropic 공식 *"AI 자율 ≠ 인간 검증 패키지"* 인용 1차 출처 · Claude Code 팀 *"리와인드 = 단 하나의 습관"* 발언 1차 출처 · *"디스텍 스퍼파"* 추정 스킬 리포지토리 정확 명칭 — 4건 모두 `[!question]` 블록 기록. 후속 lint 시 교차 확인.

## [2026-04-28] lint
- orphan 0 · stale 0 · 모순 0 (본문) · broken 11 ref / 2종(이월 그대로) · missing 4 P1 + 3 P2 · oversized 0 · gap 4 신규 + 22 이월
- 리포트: [[syntheses/lint-2026-04-28]]
- 평가: Pocock ingest 신규 11페이지 전부 inbound ≥3 (최고 41) — orphan / 모순 / oversized 0 유지. 위키 32 → 43 (+34%) 에도 건강 지표 후퇴 없음.
- 최우선 조치 (P1):
  1. `entities/andrej-karpathy` quick-add (broken 7 ref 해소)
  2. `concepts/memex` quick-add (broken 4 ref 해소)
  3. `concepts/tdd` quick-add (Pocock + Kent Beck + reward-hacking 방어선 묶음, 5곳 인용)
  4. `concepts/domain-driven-design` quick-add (ubiquitous-language 상위 우산 분리)
- P2 4건 웹 검색 일괄: OpenAI 블로그 URL · Martin Fowler 1차 · Reddit 47K · Anthropic frontmatter 필드명

## [2026-04-28] quick-add | P1 lint 권고 4건 일괄 처리
- 트리거: [[syntheses/lint-2026-04-28]] P1 4건 즉시 처리 사용자 요청
- 생성 (Entities): [[entities/andrej-karpathy]] (LLM Wiki 발화자 · Software 3.0 분류 · OpenAI/Tesla/Eureka 이력)
- 생성 (Concepts): [[concepts/memex]] (Vannevar Bush 1945 *As We May Think*), [[concepts/tdd]] (Beck 원전 + AI 코딩 맥락의 두 새 역할), [[concepts/domain-driven-design]] (Evans 책 + ubiquitous-language / bounded-context 등 상위 우산)
- 갱신 (백링크): [[concepts/ubiquitous-language]] (DDD 우산 링크), [[concepts/reward-hacking]] (TDD 1차 방어선 + deep-modules 링크), [[entities/kent-beck]] (TDD 페이지 링크), [[concepts/llm-wiki-pattern]] (updated 갱신 — 본문은 이미 memex/karpathy 인용 중이라 broken 자연 해소)
- 갱신: `wiki/index.md` (44 → 48 페이지)
- 효과: 이월 broken 2종 (`entities/andrej-karpathy` × 7 ref + `concepts/memex` × 4 ref = 11 ref) **전부 해소** — 6일 묵은 채무 청산. Missing P1 4건 → 0건.
- 미해결: P3 출처 (Karpathy X 게시 정확 URL · Bush 원문 atlantic.com 아카이브 링크 · Beck *TDD by Example* 페이지 번호 · Evans *DDD* 책 페이지 번호) 모두 페이지 본문에 명시 — 후속 lint 시 보강.

## [2026-04-28] ingest | "Software Fundamentals Matter More Than Ever" — Matt Pocock
- 소스: `raw/＂Software Fundamentals Matter More Than Ever＂ — Matt Pocock [v4F1gFy-hqg].en.vtt` (yt-dlp 자동자막, 한국어 sub 부재 → 영어 사용)
- 전처리: `raw/matt-pocock-software-fundamentals-transcript.md` (timestamp 제거, 17,802 문자)
- 저자: Matt Pocock · AI Hero (aihero.dev) · TypeScript 교육자
- URL: https://youtu.be/v4F1gFy-hqg (AI Engineer 컨퍼런스 발표, 18분 26초, 2026-04-23 업로드)
- 생성 (Source): [[sources/matt-pocock-software-fundamentals]]
- 생성 (Concepts): [[concepts/code-is-not-cheap]] (테제), [[concepts/specs-to-code-antipattern]] (반박 대상), [[concepts/software-entropy]] (Pragmatic Programmer 챕터), [[concepts/design-concept]] (Brooks), [[concepts/ubiquitous-language]] (DDD), [[concepts/deep-modules]] (Ousterhout)
- 생성 (Entities): [[entities/matt-pocock]] (테제 발화자), [[entities/john-ousterhout]] (복잡성 + deep modules 원작), [[entities/frederick-brooks]] (design concept 원작), [[entities/kent-beck]] (invest in design every day)
- 갱신 (Concepts): [[concepts/claude-skills]] (`mattpocock/skills` 사례 + 13K stars + grill me 두 줄), [[concepts/plan-mode]] (한계 — 자산 너무 빨리 생성, grill me 대안), [[concepts/harness-engineering]] (코드 레벨 하네스 = deep modules 섹션), [[concepts/context-engineering]] (7번 섹션 ubiquitous-language 어휘 정렬 축 추가)
- 갱신: `wiki/index.md` (32 → 43 페이지, +6 concepts +4 entities +1 source)
- 도메인: AI 코딩의 *펀더멘털 회귀* — 기존 클러스터(claude-skills, plan-mode, harness, context, reward-hacking) 와 강하게 결합. *Code is not cheap* 테제가 위키 내 다른 *반-경량화* 신호(vanilla 우선, 플랜 적극 편집, reward-hacking 경고) 와 같은 결로 정렬됨.
- 미해결: 인용된 책 3권의 1차 페이지 (Ousterhout *Philosophy of Software Design* 의 deep modules 챕터, Pragmatic Programmer 의 software entropy 챕터, Brooks *Design of Design* 의 design concept 챕터) — 영상 화면에 책 표지만 노출, 페이지 번호 미상. `mattpocock/skills` 리포지토리 URL 정확 확인 필요. AI Engineer 컨퍼런스 공식 발표 페이지 URL 미확보. lint 시 교차 확인.

## [2026-04-29] ingest | 직장 글쓰기 자주 틀리는 표현 30 (본인 정리)
- 소스: `raw/business-writing-30-mistakes-personal.md` (본인 정리, 2026-04-29)
- 도메인: 한국어 비즈니스 커뮤니케이션 — **위키 첫 비-기술 도메인 진입**
- 생성 (Source): [[sources/business-writing-30-mistakes]]
- 생성 (Concepts): [[concepts/business-writing-clarity]] (메타 우산: 명확성=친절), [[concepts/double-passive-translationese]] (A. 이중수동/번역체), [[concepts/cushion-words-redundancy]] (B. 쿠션어/군더더기), [[concepts/vague-time-deadlines]] (C. 모호한 시간/마감), [[concepts/object-honorifics-overformality]] (E. 사물존칭/과한격식), [[concepts/action-spec-three-elements]] (F. 요청 3요소)
- 생성 보류: D 카테고리(맞춤법) 어휘 단위 항목이라 별도 concept 미생성 — 필요 시 `concepts/korean-spelling-traps` 분리 가능
- 갱신: `wiki/index.md` (50 → 57 페이지, +6 concepts +1 source)
- 도메인 교차 발견: AI 코딩의 핵심 원칙(*모호함 제거 · 액션 명시 · 비대화 회피*)이 한국어 직장 글쓰기와 동일 결로 정렬됨. 기술 페이지([[concepts/code-is-not-cheap]], [[concepts/fresh-context-principle]], [[concepts/ubiquitous-language]], [[concepts/plan-mode]], [[concepts/specs-to-code-antipattern]])와 메타 공명 링크 형성.
- 핵심 정리: *프롬프트 엔지니어링과 한국어 직장 글쓰기는 같은 기술의 두 표면* — `action-spec-three-elements` 의 (무엇/관점/마감) 구조가 AI 프롬프트의 (무엇/관점/형식/제약) 구조와 동형.
- 미해결: 항목별 1차 출처 (국립국어원 표준국어대사전, 일제 잔재 한자어 학술 목록 등) — 후속 lint 시 보강 여지. *수고하셨습니다* 윗사람 결례 통념 vs 실제 직장 사용 현실 (페이지에 `[!note]` 로 주석 처리).

## [2026-04-28] ingest | Learn 90% Of Claude Code Agent Teams in 22 Minutes (Opus 4.6)
- 소스: `raw/Learn 90% Of Claude Code Agent Teams in 22 Minutes (Opus 4.6) [cSkoaCCmq0w].en.vtt` (yt-dlp 영어 자동자막, 한국어 sub 부재)
- 전처리: `raw/cskoa-transcript.txt` (중복 제거, 723 라인 / 27,753 자)
- URL: https://www.youtube.com/watch?v=cSkoaCCmq0w (약 22분, "Hello legends" 인사 영어권 채널)
- 저자: 미확정 — 자막 메타에 채널명 미노출. entity 미생성, lint 시 후속 확인.
- 생성 (Source): [[sources/claude-code-agent-teams-opus46]]
- 생성 (Concept): [[concepts/agent-teams]] — Opus 4.6 멀티세션 모드. 3-mode 분류 + team leader + tmux split-pane + memory 격리 + race condition + 모델 혼용 비용 통제 + skill 부트스트랩 + shutdown idle 유지 + cost 모니터링 + 안티패턴.
- 갱신 (Concepts): [[concepts/subagents]] (alias 에서 "Agent Teams" 제거 + 자매 모드 비교 표 + 출처 추가), [[concepts/claude-skills]] (생성법 3번 *한 번 굴리고 추출* 부트스트랩 패턴 추가), [[concepts/context-engineering]] (9번 섹션 agent-team 사전 MD 패키징 의무 추가, 기존 8번 Mermaid → 10번으로 시프트)
- 갱신: `wiki/index.md` (48 → 50 페이지, +1 concept +1 source)
- 도메인: Claude Code Opus 4.6 신기능 — 기존 sub-agent / context-engineering / claude-skills / agentic-engineering 클러스터 직접 확장. 영상 본인이 *"진짜 핵심 기능"* 으로 강조한 모드를 wiki 내 첫 시야 정합 (sub-agent 의 Agent Teams 절을 분리 페이지로 승격).
- 미해결: 채널/저자 정확 식별 필요 (entity 미생성), Anthropic Opus 4.6 Agent Teams 공식 docs URL 미확보 (영상 내 언급), `/cost` 멀티에이전트 duration 계산 로직 (영상 본인도 의문 표시), `--dangerously-skip-permissions` 보안 권고 vs 영상 데모 갭. 후속 lint 시 교차 확인.

## [2026-05-01] ingest | 출근 전 준비 — AI 코딩 입장 정리 (본인 노트)
- 소스: `raw/Things to prepare before go to work.md` (본인 정리 personal-note, 27 라인)
- 맥락: 새 직장 출근 전. 리드가 *"AI 관련 전문 지식·노하우 팀 내 공유"* 명시 → 첫 한 달 안에 카드를 꺼내야 하는 상황. 4개 질문 (도구 스택 / 코드베이스 구조화 / 비즈니스 임팩트 / AI 안 시키는 일) 에 대한 본인 답.
- 생성 (Source): [[sources/work-prep-2026-05-01]]
- 생성 (Concepts):
  - [[concepts/what-not-to-delegate-to-ai]] — *컨텍스트 깊이* 단일 기준으로 AI 위임 금지 영역 3종 (레거시 / 조직 정치 / Production incident) 정의. 4축 확장 프레임 (검증 가능성 / 되돌리기 비용 / 컨텍스트 깊이 / 책임 소재) 도 함께 박음. 면접·팀 공유용 한 줄 답변 2종 포함.
  - [[concepts/claude-squad]] — tmux + worktree 결합 패턴. Anthropic 공식 [[concepts/agent-teams]] 와의 격리 강도·통합 주체 비교 표. *명칭 충돌* 주의 박스로 혼동 방지.
- 갱신 (Concepts):
  - [[concepts/claude-md]] — *실무 적용 패턴* 박스 추가 (root 간소화 + 참조 허브화). sources frontmatter + 출처 갱신.
  - [[concepts/hooks]] — *강제 동기화 패턴* 신규 섹션 (markdown writer agent + TDD hook 두 사례). [[concepts/what-not-to-delegate-to-ai]] · [[concepts/tdd]] 관련 페이지 링크 추가.
  - [[concepts/context-engineering]] — MCP 다이어트 섹션에 *직접 구축 우선* 박스 추가 (script 대체 운영 원칙).
- 갱신: `wiki/index.md` (57 → 60 페이지, +2 concepts +1 source). 기존 60 표기.
- 도메인 교차 발견: 4개 질문 답이 한 줄로 묶임 — *"AI 의 신뢰 영역과 사람의 신뢰 영역을 명확히 분리"*. 본인의 hook 운영 / MCP 다이어트 / TDD 강제 / `CLAUDE.md` 분할이 모두 [[concepts/harness-engineering]] 의 *시스템 강제* 사고 위에 정렬됨. 면접에서 *추상 원칙* 보다 *왜 이 hook 들이 있는지* 설명이 시니어 시그널.
- 미해결: 본인 entity 페이지 미생성 (위키 주체이자 personal-note 작성자). 향후 면접·이력 정리 시 entity 신설 여지. *Claude Squad* 의 공식 도구·저장소 URL 미확정 (현재는 *패턴/관행* 기술). 4축 중 컨텍스트 깊이 외 3축 (검증 가능성 · 되돌리기 비용 · 책임 소재) 은 [[concepts/what-not-to-delegate-to-ai]] 안에 표로만 박음 — 후속 분리 가능.
