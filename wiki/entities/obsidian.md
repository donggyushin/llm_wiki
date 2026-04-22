---
title: Obsidian
type: entity
tags: [tool, markdown, pkm, viewer]
created: 2026-04-22
updated: 2026-04-22
sources: [[llm-wiki-karpathy]]
---

# Obsidian

## 한 줄 정의

로컬 마크다운 파일을 기반으로 하는 개인 지식관리(PKM) 앱. 위키 링크(`[[...]]`)·그래프 뷰·플러그인 생태계가 핵심.

## 이 위키에서의 역할

**뷰어 · IDE**. LLM 이 파일을 쓰면 Obsidian 이 실시간 렌더링. 저자의 표현:

> Obsidian 은 IDE, LLM 은 프로그래머, 위키는 코드베이스.

- 한쪽 창에 LLM 에이전트, 반대쪽에 Obsidian 열어두고 대화 기반 편집
- 링크 따라가기·그래프 뷰·백링크로 실시간 브라우징
- 사용자는 직접 편집보다는 **LLM 에 편집을 지시**하는 쪽

## 관련 기능

- **Wiki-link (`[[note]]`)** — 이 위키의 표준 상호참조
- **Graph view** — 위키 연결 구조 시각화. 고아·허브·클러스터 식별
- **Dataview 플러그인** — YAML frontmatter 쿼리. 예: "최근 7 일 ingest 된 source 목록"
- **Marp 플러그인** — 마크다운 슬라이드 덱 (LLM 쿼리 결과를 프레젠테이션으로)
- **Web Clipper 확장** — 웹 기사 → 마크다운, `raw/` 로 자동 수집
- **Download attachments 단축키** — 기사 내 이미지 로컬 저장 (LLM 이 이미지도 참조 가능)

## 스크린샷·이미지 컨벤션 (이 vault)

- `Resources/` 폴더에 `스크린샷 YYYY-MM-DD ....png` 형식으로 저장
- 위키에서 참조 시 `![[스크린샷 ....png]]` — **한국어 파일명 정확히 유지**
- 현재 vault 의 `Resources/` 는 기존 자산 (위키 시스템 외부, 손대지 않음)

## 관련 엔티티·개념

- [[llm-wiki-pattern]] — 이 도구가 IDE 역할로 참여하는 패턴
- **qmd** — 위키 규모 확장 시 통합 고려되는 로컬 검색 엔진
- **Zettelkasten** — Obsidian 이 종종 구현 도구로 쓰이는 다른 PKM 기법

## 근거 소스

- [[llm-wiki-karpathy]] "Tips and tricks" 섹션
