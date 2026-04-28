---
title: "Andrej Karpathy"
type: entity
tags: [ai, deep-learning, software-3.0, llm-wiki, education]
created: 2026-04-28
updated: 2026-04-28
sources: []
aliases: ["Karpathy", "@karpathy", "안드레이 카파시"]
---

# Andrej Karpathy

## 핵심 요약

슬로바키아 출신 AI 연구자. **OpenAI 창립 멤버 → Tesla AI Director (2017–2022) → OpenAI 복귀 (2023) → Eureka Labs 창립 (2024)**. 본 위키 맥락에서 결정적인 두 발화의 출처:

1. **"LLM Wiki"** 아이디어 — 매 질의마다 RAG 인덱스를 재생성하지 말고, **누적되는 지속 가능한 마크다운 위키** 를 만들자는 X 게시. 본 위키 자체의 **씨앗 페이지** [[concepts/llm-wiki-pattern]] 의 직접 출처.
2. **"Software 3.0"** 분류 — Software 1.0(사람이 코드 작성) / 2.0(가중치 학습) / 3.0(자연어 프롬프트) 의 패러다임 분류. AI 코딩 담론의 기본 어휘.

## 주요 활동

- **OpenAI 창립 멤버** (2015) — 초기 연구.
- **Tesla AI Director** (2017–2022) — Autopilot / Full Self-Driving 비전 시스템 총괄.
- **OpenAI 복귀** (2023~2024) — 짧은 기간.
- **Eureka Labs** (2024–) — AI 교육 스타트업 창립.
- **YouTube `Neural Networks: Zero to Hero` 시리즈** — `nanoGPT`, `micrograd` 등 *처음부터 짜는* 교육 자료의 표준.
- **CS231n** (Stanford) — 컴퓨터 비전 강의 공동 운영.

## 본 위키와의 직접 관계

### 1) LLM Wiki 아이디어 (X 게시)

골자: RAG 의 ephemeral 한 검색 인덱스 대신, **사용자별로 누적되는 마크다운 위키** 를 LLM 이 작성·유지·검색하는 모델. 본 위키 `CLAUDE.md` 의 3-layer 아키텍처(raw / wiki / schema) 가 정확히 이 아이디어의 구현. → [[concepts/llm-wiki-pattern]].

### 2) Memex 계보의 현대 노드

[[concepts/memex]] (Vannevar Bush, 1945) → 위키피디아 → Notion / Roam → **Karpathy LLM Wiki**. AI 가 *작성자이자 유지보수자* 라는 한 단계가 추가된 형태.

### 3) Software 3.0

자연어 = 새로운 프로그래밍 언어. *프롬프트 엔지니어링* 운동의 토대. 이후 [[concepts/harness-engineering]] · [[concepts/agentic-engineering]] 으로 분화.

## 관련 페이지

- [[concepts/llm-wiki-pattern]] — 그의 아이디어를 그대로 구현한 페이지
- [[concepts/memex]] — 계보 상의 선조
- [[concepts/rag]] — Karpathy 가 *대안* 으로 LLM Wiki 를 제안한 대상

## 출처

- *(1차 X 게시 URL · 정확 게시일 미확인 — 후속 lint 시 보강. 본 위키는 [[concepts/llm-wiki-pattern]] 본문의 인용을 통해 간접 인용.)*
- 일반 이력은 공개된 LinkedIn / Wikipedia 항목 기반.
