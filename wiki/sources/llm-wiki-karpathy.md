---
title: llm-wiki.md (Karpathy 스타일 LLM 위키 패턴)
type: source
tags: [knowledge-management, llm, wiki, rag, second-brain]
created: 2026-04-22
updated: 2026-04-22
origin: raw/llm-wiki.md
---

# llm-wiki.md — 소스 요약

> 원본 문서: `raw/llm-wiki.md`. 저자 본인이 "아이디어 파일"이라고 명시하며, 자기 LLM 에이전트(Codex·Claude Code 등)에 복붙해서 **구체 구현은 에이전트와 함께 만들라**고 권장함.

## 한 줄 요약

LLM 을 **RAG 검색기**가 아니라 **위키 유지보수자**로 쓰자. 소스를 매 쿼리마다 재탐색하는 대신, LLM 이 지속적으로 성장하는 마크다운 위키를 유지하게 하면 지식이 누적된다.

## 핵심 주장

1. **RAG 의 한계**: NotebookLM·ChatGPT 파일 업로드·일반 RAG 는 매 질문마다 지식을 처음부터 재발견한다. 5개 문서를 합성해야 하는 섬세한 질문엔 매번 단편을 재조합. **축적이 없다.**
2. **대안**: LLM 이 원시 소스 **앞단에 위키**를 세우고 증분 유지. 새 소스가 들어오면 인덱싱이 아니라 **엔티티 페이지 갱신·토픽 요약 수정·기존 주장과의 모순 플래그**를 수행.
3. **비대칭 분업**: 인간 = 소싱·탐구·질문. LLM = 요약·상호참조·파일링·부기(bookkeeping). **위키 메인터넌스의 인지 비용을 0 에 가깝게** 만드는 게 핵심.
4. **Obsidian 통합**: IDE = Obsidian, 프로그래머 = LLM, 코드베이스 = 위키. 사용자는 그래프 뷰·링크를 실시간 브라우징, LLM 은 대화 기반으로 편집.

## 주요 개념 (→ 위키 페이지로 승격)

- [[llm-wiki-pattern]] — 전체 패턴
- [[three-layer-architecture]] — raw / wiki / schema 3층
- [[wiki-operations]] — Ingest · Query · Lint
- `index.md` · `log.md` 규약 (→ [[wiki-operations]] 내 서술)

## 언급된 엔티티·도구

- [[obsidian]] — 뷰어·IDE
- [[memex]] — Vannevar Bush 1945, 사상적 조상
- **Tolkien Gateway** — 수천 페이지 수작업 팬위키. LLM 위키가 지향하는 스케일의 예시.
- **qmd** (Tobi Lütke, github.com/tobi/qmd) — 로컬 마크다운 BM25+vector 검색 + LLM 리랭커. 위키가 커지면 통합 고려.
- **Obsidian Web Clipper** — 웹 기사 → 마크다운. raw 소스 수집 도구.
- **Marp** — 마크다운 슬라이드 덱.
- **Dataview** — Obsidian 플러그인, YAML frontmatter 쿼리.

## 적용 영역 (저자 제시)

1. **개인**: 목표·건강·심리·자기개발 — 저널·기사·팟캐스트 노트 파일링 → 자기 자신에 대한 구조화된 그림.
2. **리서치**: 한 주제를 수주~수개월 딥다이브. 논문·리포트 → 증거 기반 테제 진화.
3. **독서**: 책 챕터별 파일링 → 캐릭터·테마·플롯 페이지 — 개인 팬위키.
4. **팀/비즈니스**: Slack·회의록·고객콜 피드 → LLM 메인터넌스, 인간 review. **아무도 하기 싫어하는 메인터넌스**를 LLM 이 처리하니 내부 위키가 실제로 최신 상태를 유지함.
5. 경쟁사 분석·실사·여행 계획·코스 노트·취미 딥다이브 — 축적형 지식 전반.

## 사상적 계보

Vannevar Bush 의 [[memex]] (1945) — 개인 소유·능동 큐레이션·문서 간 연관 경로. Bush 가 풀지 못한 건 **누가 메인터넌스를 하느냐**. LLM 이 그 공백을 채운다.

## 저자의 메타 주석

> "이 문서는 의도적으로 추상적이다. 디렉터리 구조·스키마·페이지 포맷·도구는 도메인·취향·LLM 에 따라 다르다. 전부 선택·모듈러. 이 문서의 유일한 역할은 **패턴 전달**. 나머진 당신의 LLM 이 알아낸다."

## 이 소스에서 파생된 위키 페이지

- [[llm-wiki-pattern]]
- [[three-layer-architecture]]
- [[wiki-operations]]
- [[obsidian]]
- [[memex]]
