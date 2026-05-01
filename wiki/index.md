---
title: "Wiki Index"
type: meta
updated: 2026-05-01
---

# Wiki Index

*마지막 갱신: 2026-05-01 · 총 60개 페이지*

LLM이 모든 페이지를 이 인덱스에 등록한다. 질의 시 이 파일을 먼저 읽어 후보를 찾는다.

---

## Entities
*(인물 · 책 · 제품 · 조직 · 장소)*

- [[entities/martin-fowler]] — Harness Engineering 개념 체계화. Garbage Collection (AI 코드 맥락) 용어 명명.
- [[entities/silbel-developer]] — 한국어 YouTube 개발 교육 채널. Claude Code · 4축 프레임 해설.
- [[entities/jimcoding]] — 한국어 YouTuber + 인프런 "클로드 코드 완벽 마스터" 강사. 오픈소스 역공학 기반 실전 팁.
- [[entities/coding-apple]] — 한국어 YouTube 개발 교육 채널. CS 기초 + 업계 이슈 해설.
- [[entities/maplestory]] — 넥슨의 2D MMORPG. 2026 모듈로 편향 드랍률 버그 사례 대상.
- [[entities/nexon]] — 메이플스토리 등을 운영하는 한국 게임 기업. 확률형 아이템 이슈 반복.
- [[entities/jay-choi]] — 한국어 YouTube 채널 "인디해커 라이프" 운영자. 인디 개발자 · Caramell · Riftshot.
- [[entities/boris-cherny]] — Claude Code 창시자 (Anthropic). vanilla 우선 + 검증 1순위 발언의 출처.
- [[entities/matt-pocock]] — TypeScript 교육자 · *Claude Code for Real Engineers* · `mattpocock/skills`. *code is not cheap* 테제.
- [[entities/john-ousterhout]] — 스탠퍼드 CS · *Philosophy of Software Design*. 복잡성 정의 + deep modules.
- [[entities/frederick-brooks]] — *Mythical Man-Month* · *Design of Design*. design concept 개념 원작자.
- [[entities/kent-beck]] — XP · TDD 창시자. *invest in the design every day*.
- [[entities/andrej-karpathy]] — OpenAI 창립 멤버 · Tesla AI · Eureka Labs. LLM Wiki 아이디어 + Software 3.0 발화처.

## Concepts
*(아이디어 · 이론 · 패턴)*

- [[concepts/llm-wiki-pattern]] — Karpathy의 지속형 LLM 위키 패턴. RAG와 대비되는 누적형 지식베이스.
- [[concepts/rag]] — Retrieval-Augmented Generation. 매 질의마다 외부 코퍼스를 검색해 프롬프트 증강. 비파라메트릭 메모리.
- [[concepts/four-axes-ai-development]] — Prompt / Context / Harness / Agentic 의 4축 상호보완 프레임.
- [[concepts/agentic-engineering]] — 말 자체를 훈련시키는 기술. 추론 루프 · 멀티에이전트 · 도구 사용법.
- [[concepts/harness-engineering]] — 마구 · 울타리 제작. AI 가 실수할 수 없는 환경 설계. 4기둥 + 시스템 4부품.
- [[concepts/context-engineering]] — 컨텍스트가 왕. Lazy loading · 세컨드 브레인 · MCP 다이어트 · 세션 위생. (4축 중 2번째)
- [[concepts/claude-md]] — Claude Code 프로젝트 루트 컨텍스트 허브. 하네스 1기둥 실체.
- [[concepts/conditional-rule-loading]] — `.claude/rules/*.md` + frontmatter glob 으로 파일 타입별 규칙 자동 로드. Lazy Loading 의 구체 구현.
- [[concepts/plan-mode]] — Shift+Tab 플랜 모드. 토큰 최적화와 방향 오류 방지의 관문.
- [[concepts/claude-skills]] — AI 업무 매뉴얼. 2단계 로딩으로 CLAUDE.md 보다 경량.
- [[concepts/subagents]] — 격리 컨텍스트 워커. Worker Isolation 패턴 + 병렬/체인/재개.
- [[concepts/agent-teams]] — Opus 4.6 멀티세션 모드. tmux split-pane + team leader + race-condition lock + skill 부트스트랩.
- [[concepts/hooks]] — 이벤트 기반 자동화 엔진. 하네스 2기둥 (System Enforcement) 메커니즘.
- [[concepts/modulo-bias]] — `rand() % N` 매핑의 구조적 편향. 옛 게임 확률 버그 대표 원인.
- [[concepts/prng-rand]] — C `rand()` 계열 PRNG 기초와 한계. 현대 대안 정리.
- [[concepts/fresh-context-principle]] — *신선한 컨텍스트 > 비대한 컨텍스트*. Context Engineering 의 한 줄 원칙.
- [[concepts/rewind-pattern]] — Esc×2 로 실패 시도 도려내기. 컨텍스트 관리의 단일 핵심 습관.
- [[concepts/reward-hacking]] — AI 가 테스트를 통과시키지 않고 테스트를 통과하도록 테스트를 수정하는 실패 모드.
- [[concepts/code-is-not-cheap]] — Pocock 의 테제. AI 시대의 나쁜 코드는 가장 비싸다 — 펀더멘털 강조의 한 줄.
- [[concepts/specs-to-code-antipattern]] — 명세만 고치고 코드는 무시하는 운동. 컴파일을 돌릴수록 코드가 더 나빠진다.
- [[concepts/software-entropy]] — *Pragmatic Programmer*. 시스템 설계를 의식하지 않은 변경의 누적이 부패를 만든다.
- [[concepts/design-concept]] — Brooks. 두 사람 사이에 떠 있는 무형의 설계 합의. `grill me` 스킬의 토대.
- [[concepts/ubiquitous-language]] — DDD 의 공유 어휘. AI 어휘 정렬용 마크다운 표로 재해석.
- [[concepts/deep-modules]] — Ousterhout. 단순 인터페이스 + 풍부한 구현. AI 가 길을 잃지 않는 코드 레벨 하네스.
- [[concepts/memex]] — Vannevar Bush 1945. *As We May Think* 의 개인 지식 기계. LLM Wiki 의 개념적 조상.
- [[concepts/tdd]] — Kent Beck. 테스트 먼저. AI 코딩 맥락에서 *outrunning headlights* 속도 제한 + reward-hacking 1차 방어선.
- [[concepts/domain-driven-design]] — Eric Evans. ubiquitous-language / bounded-context 의 상위 우산.
- [[concepts/business-writing-clarity]] — *명확성 = 친절* 메타 원칙. 직장 글쓰기 30 안티패턴의 우산.
- [[concepts/double-passive-translationese]] — 이중 수동 + 일제/영어 번역체. "되어집니다/사료됩니다/회의를 가졌습니다".
- [[concepts/cushion-words-redundancy]] — 쿠션어·군더더기. "다름이 아니라/일단/좀/한 번/혹시 시간 되시면" 삭제 알고리즘.
- [[concepts/vague-time-deadlines]] — "추후/조만간/빠른 시일/ASAP/시간 되실 때" → 구체 날짜·시각.
- [[concepts/object-honorifics-overformality]] — 사물 존칭("커피 나오셨습니다") + 사극 톤("외람되지만/송구하오나").
- [[concepts/action-spec-three-elements]] — 요청 3요소: 무엇 / 어떤 관점 / 언제까지. AI 프롬프트와 동형.
- [[concepts/what-not-to-delegate-to-ai]] — *컨텍스트 깊이* 기준의 AI 위임 금지 영역 3종 (레거시 / 조직 정치 / Production incident). 시니어 시그널.
- [[concepts/claude-squad]] — tmux + worktree 결합으로 다중 Claude Code 세션 병렬 운영. Agent Teams 보다 강한 OS 수준 격리.

## Sources
*(raw/ 에 1:1 대응하는 요약 페이지)*

- [[sources/claude-code-2h-mastery]] — "클로드 코드 2시간 안에 마스터하기" YouTube 3부작 통합본 · 실벨 개발자 · video
- [[sources/harness-engineering-era]] — "프롬프트 엔지니어링은 끝났습니다: 이제 '하네스'의 시대입니다" · 실벨 개발자 · video
- [[sources/maple-modulo-bias]] — "20년 만에 밝혀진 메이플 확률문제의 원인" · 코딩애플 · video
- [[sources/jimcoding-claude-rules]] — "클로드 코드 대규모 프로젝트 컨텍스트 엔지니어링 - `.claude/rules` 규칙 쪼개기" · 짐코딩 · video
- [[sources/jay-choi-9-tips]] — "클로드 코드 800시간 쓰고 깨달은 9가지 꿀팁" · Jay Choi (인디해커 라이프) · video
- [[sources/matt-pocock-software-fundamentals]] — *"Software Fundamentals Matter More Than Ever"* · Matt Pocock · AI Engineer 컨퍼런스 · video
- [[sources/claude-code-agent-teams-opus46]] — "Learn 90% Of Claude Code Agent Teams in 22 Minutes (Opus 4.6)" · 영어 YouTube · video
- [[sources/business-writing-30-mistakes]] — "직장 글쓰기 자주 틀리는 표현 30" · 본인 정리 · personal-note · 2026-04-29
- [[sources/work-prep-2026-05-01]] — "출근 전 준비 — AI 코딩 입장 정리" · 본인 정리 · personal-note · 2026-05-01

## Syntheses
*(합성 · 비교 · 분석 · lint 리포트)*

- [[syntheses/lint-2026-04-22]] — 건강검진 리포트. orphan 0 · stale 0 · broken 3 · missing 4 · gap 4. 최우선: `concepts/rag` 생성 권고.
- [[syntheses/lint-2026-04-23]] — 건강검진 리포트 (메이플 ingest 후). orphan 0 · 신규 broken 0 · 모순 0. 이월 broken 3건만 남음.
- [[syntheses/lint-2026-04-28]] — 건강검진 리포트 (Pocock ingest 후). orphan 0 · 신규 broken 0 · 모순 0. P1 missing 4건 (karpathy / memex / tdd / ddd) 권고.
