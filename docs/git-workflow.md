# Git 작업 흐름

공개 저장소다. 작업은 이슈 → 브랜치 → PR 순서로 한다.

## 파생 저장소의 원칙

이 스킬로 JSON 을 써서 웹 뷰어 저장소(예: `petct-paper-scrap`)에 올리는 경우, 그 저장소는 아래 두 원칙을 따른다.

| 바꾸는 것 | 방법 |
|---|---|
| 키워드로 찾은 문헌 정리 결과(데이터 파일만) | 찾은 뒤 바로 `main` 에 커밋·push |
| 저장소의 구조·틀·형식(화면, JSON 형식, 스킬, 문서, 설정) | 이슈 → 브랜치 → PR |

이 저장소(lit-search) 자체에는 데이터 파일이 없으므로 모든 변경을 PR 로 올린다.

## 이슈

- 할 일·버그는 `.github/ISSUE_TEMPLATE/task.md` 양식으로 이슈를 먼저 연다.
- 이슈 본문에 개인 경로·이메일·미발표 연구 세부를 적지 않는다.

## 브랜치

- 기준 브랜치는 `main`. 작업은 `main` 에서 분기해 PR 로 병합한다. `main` 에 바로 push 하지 않는다.
- 이름은 `타입/간단한-설명`.

| 접두사 | 용도 | 예시 |
|---|---|---|
| `feat/` | 새 단계·규칙·설정 | `feat/openalex-source` |
| `fix/` | 잘못된 규칙·경로 수정 | `fix/pdf-html-check` |
| `docs/` | 문서 | `docs/install-guide` |
| `refactor/` | 동작 변경 없는 구조 개선 | `refactor/split-steps` |
| `chore/` | 플러그인 설정·저장소 관리 | `chore/github-templates` |

## 커밋 메시지

Conventional Commits, 소문자 타입. `<타입>: <요약>` 한 줄, 명령형·현재형.

- 예: `feat: OpenAlex 를 검색 경로에 추가`, `fix: HTML 로 받아진 PDF 를 걸러냄`

## PR

- 제목은 커밋과 같은 `타입: 요약` 형식.
- 본문은 `.github/pull_request_template.md` 를 채운다. 공개 저장소 체크리스트는 필수.
- 이슈를 끝내는 PR 이면 본문에 `Closes #번호`.
- 스킬 동작이 바뀌는 PR 이면 `.claude-plugin/plugin.json` 의 `version` 을 올린다.

## 커밋 전 점검

```bash
git status
git diff --cached | grep -nE '/Users/|Dropbox|Obsidian|@[a-z0-9.-]+\.(ac|edu|com)|proxy\.' && echo "STOP: 개인 정보로 보이는 문자열"
git diff --cached --name-only | grep -Ei '(\.pdf|config\.json)$' && echo "STOP: PDF 또는 개인 설정이 스테이징됨"
```
