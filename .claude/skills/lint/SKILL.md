---
name: lint
description: 위키 건강검진. orphan 페이지, stale 콘텐츠, 모순, 깨진 [[링크]], 누락 개념, oversized 페이지, 데이터 공백을 탐지하고 리포트. 사용자가 "lint 돌려줘", "위키 검진", "건강 체크", "/lint", "위키 정리" 등을 말할 때 실행. 결과는 wiki/syntheses/lint-YYYY-MM-DD.md 로 저장하고 log.md 에 기록.
triggers: ["lint", "lint 돌려줘", "위키 건강", "위키 검진", "위키 정리", "/lint", "health check", "위키 체크"]
---

# Lint Skill

LLM Wiki 건강검진. 주기적으로 또는 사용자 요청 시 실행.

> 자세한 위키 규칙은 저장소 루트 `CLAUDE.md` 참조.

---

## 언제 실행

- 사용자가 "lint 돌려줘", "/lint", "위키 건강검진"
- 소스 N개(10개 권장) ingest 후 정기 점검
- 위키 재구성/마이그레이션 전

---

## 체크 항목 (반드시 전부)

### 1) Orphan 페이지
**정의**: 다른 페이지로부터 `[[...]]` inbound 링크가 **0개**인 페이지 (meta 페이지 `index.md`, `log.md` 제외).

**탐지**:
```bash
# 모든 wiki 페이지 slug 목록
find wiki -name "*.md" -not -name "index.md" -not -name "log.md" | sed 's|wiki/||; s|\.md$||'

# 각 slug 가 다른 페이지에서 [[...]] 로 참조되는지 확인
grep -rh "\[\[$slug\]\]" wiki/ | grep -v "^wiki/$slug.md"
```

**조치**: inbound 0 → "orphan" 으로 표시. 제거할지, 다른 페이지에서 연결할지 사용자에게 제안.

### 2) Stale 콘텐츠
**정의**: `updated` 가 오래됐고, 그 이후 ingest된 소스가 **보완/수정/반박** 하는 페이지.

**탐지**:
- 각 페이지 frontmatter `updated` 확인
- `log.md` 에서 그 이후 ingest 로그 스캔
- 해당 소스 요약이 이 페이지의 주제를 다루는가?

**조치**: stale 후보 리스트업. 각각 재ingest 또는 수동 갱신 제안.

### 3) 모순
**정의**: 서로 충돌하는 주장을 담은 페이지 쌍. 또는 `> [!warning] 모순` 콜아웃이 존재하지만 미해결.

**탐지**:
```bash
grep -rn "\[!warning\] 모순" wiki/
```

**조치**: 미해결 모순 전부 리스트. 해결 방향(추가 소스 필요 / 한쪽 폐기 / 양립 서술) 제안.

### 4) Broken Link
**정의**: `[[slug]]` 가 존재하지 않는 파일을 가리킴.

**탐지**: 모든 `[[...]]` 추출 → 해당 `.md` 파일 존재 여부 확인.

**조치**: 깨진 링크 전부 위치·페이지·대상 리스트. 생성할지 / 링크 제거할지 결정.

### 5) Missing Page
**정의**: 다수(예: 3+ 페이지)에서 언급되지만 **독립 페이지가 없는 개념/고유명사**.

**탐지**: 본문에 자주 등장하는 고유명사·개념어 추출 → 대응 페이지 존재 여부.

**조치**: 페이지 만들 가치 있는 것 리스트. 각각 어느 소스들로 채울 수 있는지 제안.

### 6) Oversized
**정의**: 800줄 초과 페이지. 가독성·유지보수성 저하.

**탐지**:
```bash
find wiki -name "*.md" -exec wc -l {} \; | awk '$1 > 800 {print $2 " (" $1 " lines)"}'
```

**조치**: 분할 전략 제안 (하위 개념별 · 시간 축별 · 관점별).

### 7) Gap (데이터 공백)
**정의**: 페이지가 "출처 불명" 또는 "추가 조사 필요" 로 명시한 구간.

**탐지**:
```bash
grep -rn "출처 확인 필요\|추가 조사\|TODO\|TBD" wiki/
```

**조치**: 웹 검색 · 신규 소스 수집 후보 제안.

---

## 리포트 작성

결과는 `wiki/syntheses/lint-YYYY-MM-DD.md` 로 저장.

```markdown
---
title: "Lint Report YYYY-MM-DD"
type: synthesis
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [lint, meta]
---

# Lint Report YYYY-MM-DD

총 페이지 N개 스캔.

## Orphan (M개)
- [[경로]] — 제안: ...

## Stale (M개)
- [[경로]] — 마지막 갱신 YYYY-MM-DD. 그 이후 ingest된 [[소스]] 이 보완 가능.

## 모순 (M개)
- [[페이지A]] ↔ [[페이지B]] — <주제>. 제안: ...

## Broken Link (M개)
- `[[잘못된-slug]]` in [[페이지]] — 의도한 대상: ?

## Missing Page (M개)
- "<개념>" — [[페이지1]], [[페이지2]], [[페이지3]] 에서 언급. 신규 페이지 권장.

## Oversized (M개)
- [[경로]] — N줄. 분할 축: ...

## Gap (M개)
- [[경로]] 의 "<주제>" — 출처 미확인. 찾아볼 곳: ...

## 요약 · 다음 단계
- 가장 급한 조치 3개: ...
- 권고 순서: ...
```

---

## Log 기록

`wiki/log.md` 맨 아래 append:

```markdown
## [YYYY-MM-DD] lint
- orphan N · stale M · 모순 K · broken L · missing P · oversized Q · gap R
- 리포트: [[syntheses/lint-YYYY-MM-DD]]
- 최우선 조치: ...
```

---

## 실행 모드

### Full lint (기본)
전 7개 항목 모두 스캔. 위키 100 페이지 이하면 문제없음.

### Quick lint
broken link + 모순 만. 신속 점검용. 사용자가 "quick lint" 명시.

### Scoped lint
특정 디렉토리만 (예: `wiki/concepts/` 만). 사용자가 경로 지정 시.

---

## Anti-patterns

- 리포트만 생성하고 `log.md` 업데이트 누락 ❌
- orphan 페이지를 **자동 삭제** ❌ (사용자 확인 필수)
- broken link 를 **자동 생성** ❌ (의도한 대상 불분명)
- 모순을 리포트에서만 언급하고 원본 페이지 콜아웃 미추가 ❌
