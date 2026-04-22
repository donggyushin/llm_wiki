---
title: MCP (Model Context Protocol)
type: entity
tags: [claude-code, protocol, integration, tool]
created: 2026-04-22
updated: 2026-04-22
sources: [[claude-code-study-notes]]
---

# MCP — Model Context Protocol

## 한 줄 정의

Claude Code 가 외부 시스템·서비스와 **표준화된 방식으로 연결**되는 프로토콜. Supabase·Notion 등 다수 서비스가 MCP 서버 제공. `/mcp` 로 관리.

## 사용자 노트의 입장 — "양날의 검"

원 소스 l.50-53 의 경고:

1. **아무것도 안 해도 시작부터 컨텍스트 잠식** — 연결된 MCP 는 유휴 시에도 정의를 로드
2. **무자비하게 쓰면 비추** — 많이 붙일수록 매 세션 오버헤드
3. **커스텀 MCP 를 직접 만드는 게 제일 좋음** (l.53, l.164)
4. 범용 무거운 MCP 는 자제 대상

## 추천 MCP (원 소스 l.50)

- **Supabase** — DB·인증
- **Notion** — 문서·지식관리

## Skills 와의 경계

[[skills]] 페이지의 비교표 참조. 요약: **외부 실시간 연결이 꼭 필요할 때만 MCP**, 그 외엔 Skills.

## 대안·확장

- **커스텀 MCP 서버** — 필요한 기능만 담고 나머지 제거 (l.164)
- [[skills]] — 로컬 매뉴얼은 이쪽
- [[sub-agents]] — 병렬 실행은 이쪽

## WAT 내 위치

[[wat-framework]] 의 **T (Tools)** 층. 단, 비용(컨텍스트) 때문에 신중 선택.

## 관련 개념

- [[context-as-king]] — MCP 신중 선택의 근거
- [[wat-framework]] — Tools 층 구성원

## 관련 엔티티

- [[claude-code]] — 호스트
- [[skills]] — 가벼운 대안

## 근거 소스

- [[claude-code-study-notes]] l.49-53, l.164
