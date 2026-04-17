> source: https://www.youtube.com/watch?v=vxEvo2BLM6A

# Claude Code 학습 노트

## CLAUDE.md 운영

- 파일이 너무 커지면 **토큰 사용량이 증가**하므로 폴더별로 분할해 관리하는 것이 좋다.
- 수동으로 직접 수정할 필요는 없다. 클로드에게 "자동으로 업데이트해줘"라고 요청하는 방식이 가장 편하다.

## Trigger Keywords

![[스크린샷 2026-04-17 오전 10.28.02.png]]

## Compound Engineering

![[스크린샷 2026-04-17 오전 10.29.53.png]]

## 핵심 단축키

| 단축키 | 동작 |
| --- | --- |
| `Shift + Tab` | Plan Mode ↔ Accept Mode 전환 |
| `Escape` | 즉시 중단 |
| `Escape × 2` | 입력 삭제 / 복원 |

## Plan Mode를 써야 하는 실무적 이유

1. **토큰 최적화** — "잘못된 코드 생성 → 수정 요청 → 재생성" 악순환을 끊을 수 있다.
2. **컨텍스트 윈도우 보존**
3. **실행 정확도 향상**
4. **비용 관리** — input / output 중 output 토큰을 절약할 수 있다.

> 결론: **Plan → 검토 → Execute** 패턴을 습관화하자.
