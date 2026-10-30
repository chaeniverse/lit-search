# lit-search

논문을 찾고, 거르고, 원문 PDF를 받아 정리하는 Claude Code 스킬(플러그인).

A Claude Code skill for literature search, screening, and full-text PDF retrieval (open access first, then your own library proxy). Instructions are written in Korean.

## 하는 일

주제(또는 저자·연도·저널 목록)를 주면 포함 조건 한 줄 → 여러 경로 검색 → 중복·철회 정리 → Tier A/B → 원문 PDF → 논문 카드로 정리한다.

- 검색 경로: PubMed · Scite · Consensus · bioRxiv MCP, 방법론 주제는 arXiv · OpenReview 가 1순위
- 원문 순서: PMC · arXiv · 오픈액세스 → 소속 기관 도서관 프록시(설정했을 때만, 로그인은 사용자가 직접)
- 하지 않는 것: 저널 IF 추정, 본문 통째 옮기기, 비합법 원문 경로, 개인정보를 외부 API에 넣기

규칙 전체는 [skills/lit-search/SKILL.md](skills/lit-search/SKILL.md).

## 설치

플러그인으로:

```
/plugin marketplace add chaeniverse/lit-search
/plugin install lit-search@lit-search
```

또는 스킬 폴더만 연결:

```bash
git clone https://github.com/chaeniverse/lit-search.git
ln -s "$PWD/lit-search/skills/lit-search" ~/.claude/skills/lit-search
```

## 설정

`~/.config/lit-search/config.json` 에 둔다. 저장소에는 넣지 않는다. 형식은 [config.example.json](config.example.json).

| 키 | 뜻 |
|---|---|
| `pdf_root` | 원문 PDF 저장 루트. 실행마다 `YYYY-MM-DD <주제>` 하위 폴더를 만들고, 키워드를 묶음으로 나눠 찾았으면 묶음마다 하위 폴더를 만든다 |
| `library_proxy.url_template` | 도서관 프록시 URL, `{url}` 자리에 출판사 URL. 비우면 무료 경로만 쓴다 |
| `library_proxy.pubmed_otool` | PubMed "Find it" 버튼용 기관 코드(선택) |
| `json_export_dir` | 검색 결과 JSON 을 쓸 폴더(선택). 아래 "웹 뷰어" 참고 |

설정 파일이 없으면 첫 실행 때 PDF 저장 위치를 묻고, 원하면 파일을 만들어 준다.
아이디·비밀번호·이메일은 설정에도 넣지 않는다.

필요한 것: Claude Code, 쓰고 싶은 논문 MCP(claude.ai 커넥터의 PubMed·Scite·Consensus·bioRxiv 등, 없으면 건너뛴다), 도서관 프록시를 쓸 때는 Claude in Chrome.

## 웹 뷰어 (선택)

`json_export_dir` 을 정하면 검색 결과를 [docs/json-schema.md](docs/json-schema.md) 형식의 JSON 으로도 쓴다.
이 JSON 을 읽는 정적 사이트(Next.js 등)를 Vercel 에 올리면 주간 스캔처럼 웹에서 훑어볼 수 있다.
뷰어 앱은 이 저장소에 없다 — JSON 형식만 정해 두고 앱은 각자 만든다.
미발표 연구와 이어지는 메모가 들어가면 사이트를 비공개로 두거나 인증을 건다. 원문 PDF 는 올리지 않는다.

## 파생 저장소

| 저장소 | 공개 | 내용 |
|---|---|---|
| `petct-paper-scrap` | 비공개 | 이 스킬을 특정 영상 연구(PET-CT) 주간 문헌 스캔에 맞춘 변형. 갈래·관문·Tier 규칙을 더하고, 결과를 JSON 으로 써서 같은 저장소의 Vercel 뷰어(Next.js, 비밀번호 보호)로 본다 |
| `paper-scrap` | 비공개 | 이 스킬을 저장소 주인의 연구 관심사(생성모델의 통계적 평가 등 3축)에 맞춘 주간 AI 논문 스캔 변형. arXiv·OpenReview·통계 저널을 1순위로 두고 소속·통계 연관성·응용 세 관문으로 거른 뒤, 결과를 JSON 으로 써서 같은 저장소의 Vercel 사이트로 본다 |

## 기여

이슈 → 브랜치 → PR 순서로 한다. 브랜치·커밋·PR 규칙은 [docs/git-workflow.md](docs/git-workflow.md).

## 라이선스

MIT
