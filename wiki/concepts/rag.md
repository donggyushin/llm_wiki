---
title: "RAG (Retrieval-Augmented Generation)"
type: concept
tags: [rag, retrieval, vector-db, llm, knowledge-base]
created: 2026-04-23
updated: 2026-04-23
sources: []
aliases: ["Retrieval-Augmented Generation", "검색 증강 생성"]
---

# RAG (Retrieval-Augmented Generation)

## 핵심 요약

외부 지식 저장소에서 관련 조각을 **질의 시점에 검색**해, 그 조각을 프롬프트에 끼워 넣은 뒤 LLM 이 답을 생성하는 패턴.
모델 파라미터에 지식을 구워 넣지 않고 **외부 코퍼스를 비파라메트릭 메모리**처럼 쓴다.
지식 업데이트가 재학습 없이 되고, 출처 제시가 가능하며, hallucination 을 일정 부분 낮춘다.

> [!note] 용어 기원
> Lewis et al. 2020 "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" (Meta AI). 이후 LangChain · LlamaIndex 확산으로 **엔지니어링 통칭**이 됨.

---

## 동작 파이프라인

질의 시점에 다음 단계가 돈다.

1. **Index (사전 단계)** — 원본 문서를 **청킹(chunking)** → 임베딩 모델로 벡터화 → 벡터 DB 에 저장.
2. **Retrieve** — 사용자 질의를 같은 임베딩 공간으로 투영 → 유사도 top-k 청크 검색 (보통 코사인 유사도 + ANN 인덱스).
3. **(선택) Rerank** — cross-encoder 로 정밀 재정렬, 또는 BM25 와의 하이브리드.
4. **Augment** — 검색된 청크를 프롬프트에 삽입 (보통 "아래 문맥을 바탕으로 답하라" 형태).
5. **Generate** — LLM 이 증강된 프롬프트로 답 생성. 인용/출처 포함 가능.

질의마다 1–5 단계를 처음부터 다시 돈다는 점이 핵심. **상태는 저장소에만 있고, 해석은 휘발된다.**

---

## 구성 요소

| 층 | 대표 선택지 | 역할 |
|----|------------|------|
| 임베딩 모델 | OpenAI `text-embedding-3`, Cohere `embed-v3`, `bge`, `e5` | 텍스트 → 고차원 벡터 |
| 벡터 DB | Pinecone, Weaviate, Qdrant, Milvus, pgvector, FAISS | 유사도 검색 인프라 |
| 청킹 전략 | fixed-size, recursive, semantic, document-aware | 의미 단위 보존 vs 인덱스 비용 |
| Retriever | dense, sparse (BM25), hybrid | 어휘 일치 vs 의미 일치 |
| Reranker | cross-encoder (bge-reranker, Cohere Rerank) | top-k 정밀 정렬 |
| Orchestration | LangChain, LlamaIndex, Haystack | 파이프라인 조립 |

---

## 왜 RAG 인가

- **지식 업데이트 비용**: 모델 재학습 없이 코퍼스만 갱신하면 반영.
- **출처 제시**: 검색된 청크를 그대로 인용 가능 → 감사·규제 대응.
- **도메인 특화**: 사내 문서·최신 논문·제품 매뉴얼처럼 학습 데이터에 없는 지식 주입.
- **Hallucination 완화**: 근거 없는 추측을 줄임(완전 제거는 아님 — 검색 실패 시 그대로 hallucinate).
- **프라이버시**: 민감 데이터를 모델 파라미터 밖에 둠.

---

## 한계

- **Chunk boundary 문제** — 정답이 청크 경계에 걸리면 누락. 해결 시도: 오버랩, parent-child 청킹, 계층 인덱스.
- **Retrieval miss** — 질의 어휘와 문서 어휘가 불일치하면 못 찾음. HyDE · query rewriting 으로 완화.
- **컨텍스트 비용** — top-k 를 늘리면 토큰 비용·지연 상승. Reranker 로 k 축소.
- **해석·교차참조 부재** — 검색기는 **청크 단위 독립성**을 가정한다. 문서 간 모순·계보·합성은 매 질의마다 LLM 이 즉석으로 재구성하고 **저장되지 않는다**.
- **Evaluation 어려움** — retrieval 품질과 generation 품질이 분리 측정돼야 함 (RAGAS, TruLens 등).

---

## 변형

- **GraphRAG** (Microsoft 2024) — 엔티티 추출 → 지식 그래프 → 커뮤니티 요약. 전역 질의에 강함.
- **HyDE** — 질의로 *가상의 답*을 먼저 생성, 그 답을 임베딩해 검색. 질의-문서 어휘 간극 해소.
- **Self-RAG** — 모델이 *검색 필요 여부*를 스스로 판단하고 자기 검증 토큰을 생성.
- **Agentic RAG** — 단일 retrieve 대신 **도구 호출 루프**로 반복 검색·정제. Claude Code 류의 파일 시스템 탐색도 일종의 agentic retrieval.
- **Long-context + RAG** — 컨텍스트 창이 커져도 RAG 가 사라지지 않는 이유: 비용·지연·정확도(lost-in-the-middle) 때문.

---

## LLM Wiki 와의 관계

[[concepts/llm-wiki-pattern]] 은 RAG 를 *대체*하는 게 아니라 **저장 층의 성격**을 바꾼다.

| 축 | RAG | LLM Wiki |
|----|-----|----------|
| 저장 단위 | 원본 청크 + 벡터 | 편집된 마크다운 페이지 |
| 해석 누적 | ❌ (매 질의 휘발) | ✅ (페이지에 고정) |
| 교차참조 | 질의 시 합성 | 링크로 이미 존재 |
| 모순 처리 | 응답에서 일회성 | 페이지에 명시 콜아웃 |
| 질의 동작 | retrieve → augment → generate | index 읽기 → 페이지 읽기 → 합성 |
| 인프라 | 벡터 DB · 임베딩 · reranker | 파일 시스템 · grep |
| 적합 규모 | 수십만+ 문서, 빈번한 대량 질의 | 수백~수천 페이지, 심층 주제 |

> [!note] 상보 관계
> 대규모 코퍼스 위에 LLM Wiki 를 *얹을 수도* 있다. RAG 가 원본 검색을 맡고, 위키가 **축적된 해석 층**을 제공하는 하이브리드가 자연스러운 진화 경로.

핵심 차이를 한 줄로: **RAG 는 검색 시스템이다. Wiki 는 편집된 지식 그 자체다.**

---

## 관련 페이지

- [[concepts/llm-wiki-pattern]] — 지속형 대안 패턴
- [[concepts/context-engineering]] — RAG 도 lazy loading 의 한 구현
- [[concepts/agentic-engineering]] — Agentic RAG 의 이론적 토대

## 출처

_현재 페이지는 [[concepts/llm-wiki-pattern]] 의 대비 서술과 일반 RAG 문헌(Lewis 2020, LangChain/LlamaIndex 문서, GraphRAG 논문)을 바탕으로 합성. 특정 RAG 논문을 `raw/` 에 ingest 하면 출처 링크 채움._

> [!question] 출처 확인 필요
> Lewis et al. 2020 원 논문 URL 및 Microsoft GraphRAG 2024 공식 발표 링크는 raw ingest 시 보강.
