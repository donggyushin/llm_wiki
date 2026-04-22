---
name: ingest
description: 새 소스를 raw/에서 읽어 wiki/로 통합. LLM Wiki 패턴의 핵심 워크플로. 사용자가 "ingest 해줘", "이 소스 넣어줘", "raw/xxx 처리해줘", "/ingest", "위키에 추가해줘" 등을 말할 때 실행. 소스 읽기 → takeaway 대화 → sources/ 요약 → entities/concepts 갱신 → 모순 표시 → index/log 갱신까지 한 번에 처리.
triggers: ["ingest", "ingest 해줘", "소스 넣어줘", "위키에 추가", "raw/ 처리", "/ingest"]
---

# Ingest Skill

새 소스를 `raw/`에서 읽어 `wiki/`로 통합하는 **LLM Wiki 패턴의 핵심 워크플로**.

> 자세한 스키마·페이지 포맷·링크 규칙은 **저장소 루트 `CLAUDE.md`** 참조. 이 스킬은 그 규칙을 실행 시 체크리스트로 변환한 것.

---

## 언제 실행

사용자가 다음 중 하나를 말할 때:
- "raw/xxx ingest 해줘"
- "이 소스 위키에 넣어줘"
- "/ingest <파일경로>"
- "방금 raw/에 파일 드롭했어"
- `raw/` 하위에 새 파일이 추가되었음을 확인한 경우

## 실행 전 체크

- `raw/` 경로의 실제 파일 존재 확인 (`ls raw/...`)
- `.md` / `.pdf` / `.txt` / 이미지 등 포맷 파악
- 이미지 포함 시 → 텍스트 먼저 읽고 이미지는 별도로 View (LLM 멀티모달 한계)

---

## 실행 절차 (반드시 순서대로)

### 1) 읽기
`Read` 로 원본 전체를 읽는다. 이미지 있으면 개별 View.
PDF면 필요 페이지 범위 지정. 큰 문서면 섹션별로.

### 2) Takeaway 대화 (쓰기 전 필수)
사용자에게 **핵심 포인트 3~5개** 를 불릿으로 제시.
- "맞다/아니다/강조할 부분" 피드백 받기
- 이 단계를 건너뛰고 바로 쓰지 말 것
- 사용자가 즉시 ingest 하라고 하면 스킵 가능

### 3) Source 페이지 작성
`wiki/sources/<slug>.md` 생성. frontmatter 필수:

```yaml
---
title: "원본 제목"
type: source
tags: [...]
created: YYYY-MM-DD
updated: YYYY-MM-DD
author: "저자"
medium: article | paper | book | podcast | video | screenshot | personal-note
raw_path: "raw/xxx.md"
aliases: []
---
```

본문 구조:
- 핵심 요약 (3~5줄)
- 주요 주장 / 데이터 / 인용
- 내가 주목한 부분
- 관련 페이지 `[[...]]`
- 원본 위치: `raw/xxx.md`

### 4) Entity/Concept 페이지 갱신
원본에서 추출한 고유명사·개념마다:
- **신규**: `wiki/entities/<slug>.md` 또는 `wiki/concepts/<slug>.md` 생성
- **기존**: 정보 병합 + `updated` 날짜 갱신 + `sources:` 배열에 현재 소스 slug 추가

### 5) 모순 처리
기존 페이지와 충돌하는 주장 발견 시 — 조용히 덮어쓰지 말 것.
양쪽 페이지에 콜아웃 추가:

```markdown
> [!warning] 모순
> [[sources/새-소스]] 는 X라고 주장. [[sources/기존-소스]] 는 Y라고 주장.
> 현재 미해결.
```

### 6) Index 갱신
`wiki/index.md` 를 Read → Edit.
- 신규 페이지마다 해당 섹션(Entities/Concepts/Sources/Syntheses)에 한 줄 추가
- 상단 "총 N개 페이지" 카운트 갱신
- `updated` frontmatter 갱신

### 7) Log 기록
`wiki/log.md` 맨 아래 append:

```markdown
## [YYYY-MM-DD] ingest | <소스 제목>
- 생성: [[sources/...]], [[concepts/...]], [[entities/...]]
- 갱신: [[기존-페이지]] (+ 무엇이 추가됐는지 한 줄)
- 모순: (있으면 명시, 없으면 생략)
```

### 8) 보고
사용자에게 다음 형식으로 요약:

```
Ingest 완료: <소스 제목>
- 신규 페이지 N개: <경로 목록>
- 갱신 페이지 M개: <경로 목록>
- 모순 발견: (있으면)
- 다음 제안: (추가 질의 · 관련 소스 찾기 · lint 권고 등)
```

---

## 한 번 ingest가 건드릴 수 있는 범위

**정상**: 10~15 페이지. source 1개 + entity 3~5개 + concept 3~7개 + index + log.

**주의 신호**:
- 1~2 페이지만 건드림 → 교차 참조 누락 의심
- 30+ 페이지 → 소스 너무 큼. 장/섹션 단위로 분할 ingest 고려

---

## Anti-patterns

- `raw/` 파일 수정/삭제/이동 ❌
- 출처 없이 주장 작성 ❌
- 단일 페이지에 여러 주제 섞기 ❌ (분할할 것)
- `[[...]]` 교차참조 없이 평문만 ❌
- `index.md` / `log.md` 갱신 누락 ❌
- 모순을 조용히 덮어쓰기 ❌

---

## Quick Add (경량 버전)

한 페이지짜리 메모·발견이면 ingest 전체 파이프라인 불필요:
1. 해당 디렉토리에 페이지 1개 생성 (frontmatter 포함)
2. `log.md` 에 `## [YYYY-MM-DD] quick-add | <제목>` 기록
3. index 갱신은 선택 (핵심 페이지면 갱신)

사용자가 "quick add", "메모만 추가" 라고 할 때 사용.
