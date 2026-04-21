# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Type

This is an **Obsidian vault** — a personal knowledge base, not a software project. There are no build, test, or lint commands. Work here is creating, editing, linking, and organizing Markdown notes.

## Conventions

- **Primary language for notes is Korean.** Write responses and note content in Korean unless the user asks otherwise.
- **Obsidian link syntax:**
  - `[[note name]]` — wiki-link to another note (create the target file if it doesn't exist when the user wants the link realized)
  - `[[note name|alias]]` — aliased link
  - `![[image.png]]` or `![[note name]]` — embed an image or note inline
- **File names are the note titles** (including spaces and Korean characters). Don't rename files to "sanitize" them — the name is the identity used by every `[[link]]`.
- **Screenshots** live at the vault root with Korean filenames like `스크린샷 YYYY-MM-DD ...png` and are referenced via `![[...]]` embeds. Preserve exact filenames when editing references.

## Vault Structure

Flat at the moment — notes live at the repo root. If reorganizing into folders, update every `[[link]]` that targets moved notes (Obsidian normally does this automatically in the app, but edits made outside Obsidian must fix links manually).

`.obsidian/` holds Obsidian app config (workspace layout, enabled plugins, appearance). Generally don't hand-edit these; `workspace.json` in particular changes on every Obsidian session.

## Working with Notes

- Prefer `Edit` over `Write` for existing notes to preserve formatting and surrounding content.
- Do not create new `.md` files unless the user asks — even stub files for unresolved `[[links]]`. Unresolved links are a normal Obsidian state.
- When the user references `CLAUDE study.md`, they are studying Claude Code itself; the notes there describe *their* preferred workflow (Plan Mode, Compound Engineering, trigger keywords) and should be treated as the user's own reference material, not instructions for this repo.

## Advisor 활용 가이드

Claude Code에서 `/advisor` 명령으로 Advisor 모델(Opus)을 설정할 수 있다. Advisor는 Sonnet(Executor)이 실행하다가 고차원적 판단이 필요한 순간에만 Opus에게 조언을 구하는 구조 — 저비용 고효율.

### Advisor를 호출해야 하는 시점

1. **작업 시작 전** — 실질적인 구현을 시작하기 전에 방향·접근법을 검토
2. **작업 완료 후** — 결과물을 저장·커밋한 뒤 누락·오류 최종 점검
3. **막혔을 때** — 오류가 반복되거나 방향 전환을 고려할 때

### 출력 토큰 절약

Advisor 호출 시 프롬프트에 명시:
> "100단어 이내로, 번호 매긴 단계 형태로만 응답하라."

### 주의

- Advisor는 도구를 사용하지 않고 사용자에게 직접 출력물을 생성하지 않음 — Executor에게 조언만 전달
- Advisor 조언에 무게를 두되, 직접 관찰한 파일/오류 내용이 조언과 충돌하면 한 번 더 Advisor에게 확인

---

## Trigger Keywords

사용자가 아래 키워드를 입력하면 Claude Code가 해당 워크플로우를 자동 실행한다.

### `커밋` or `commit`

1. `git status` 와 `git diff` (필요하면 `git diff --staged` 도) 를 실행해 변경된 파일과 내용을 확인
2. 변경 내용을 분석해 간결하고 적합한 커밋 메시지를 한글로 작성 (형식: `<type>: <설명>` — feat / fix / docs / chore 등)
3. 필요한 파일을 `git add` 후 `git commit` 실행
4. 커밋 후 `git status` 로 결과 확인 및 한 줄 요약 보고

주의:
- `.env`, 자격 증명, 큰 바이너리 등 민감·불필요 파일은 스테이징 전 사용자에게 경고
- 사용자가 명시적으로 요청하기 전에는 `git push` 하지 않음
- pre-commit hook 이 실패하면 원인을 수정한 뒤 **새 커밋** 생성 (`--amend` 금지)
