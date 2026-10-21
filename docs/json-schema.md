# JSON 내보내기 형식

검색 결과를 정적 사이트(예: Vercel에 올린 Next.js 뷰어)에서 읽기 위한 형식이다. 쓰지 않아도 스킬은 그대로 동작한다.

한 번 실행이 한 파일이다: `<json_export_dir>/<YYYY-MM-DD>.json`. 같은 날 두 번 돌리면 `<YYYY-MM-DD>-2.json`.

```json
{
  "date": "2026-10-21",
  "title": "2026-10-21 SGLT2 심부전",
  "query": "성인 2형 당뇨에서 SGLT2 억제제의 심부전 입원 위험을 … 추정한 연구",
  "period": { "from": "2023-01-01", "to": "2026-10-21" },
  "meta": [
    { "label": "훑어본 범위", "value": "제목 수준 약 420건 → 초록 확인 31편" },
    { "label": "수록", "value": "Tier A 4편 / Tier B 6편" }
  ],
  "overview": ["이번 검색 요약 — 줄마다 한 문단"],
  "sections": [
    {
      "category": "Tier A",
      "items": [
        {
          "no": "1",
          "title_ko": "한국어 제목",
          "title": "English original title",
          "url": "https://doi.org/10.xxxx/xxxx",
          "source": "NEJM · doi:10.xxxx/xxxx · 2024-03-01",
          "authors": "Smith J, Lee K, …",
          "affiliation": "University of Oxford 통계학과",
          "summary": "한국어 3~6문장 요약",
          "methods": "n, 장비·코호트, 검증 방식, 공개 코드 여부",
          "relevance": "내 연구와의 연결 한두 문장",
          "questions": ["후속 질문 1", "후속 질문 2"],
          "notice": "",
          "pdf": "Smith 2024 NEJM - Empagliflozin and heart failure.pdf"
        }
      ]
    }
  ],
  "dropped_title": "포함 조건·철회로 뺀 논문",
  "dropped": [{ "title": "…", "url": "…", "reason": "철회 (2025-02)" }],
  "memo": ["검색한 도구와 검색어, 원문 못 받은 논문과 사유"]
}
```

| 필드 | 뜻 |
|---|---|
| `date` · `title` · `query` · `period` | 실행일, 제목, 포함 조건 한 줄, 대상 기간 |
| `meta` | 보고 머리말을 `{label, value}` 로 |
| `overview` | 전체 요약 — 줄마다 한 문단 |
| `sections[]` | `Tier A` · `Tier B` · `그 외 수집 목록` |
| `sections[].items[]` | 논문 카드. 스킬 5단계 항목과 같다. 없는 값은 빈 문자열 |
| `dropped_title` · `dropped[]` | 뺀 논문 기록 |
| `memo[]` | 검색 메모 |

- `seen.json`(선택): 다룬 논문 식별자 목록. 주간 스캔처럼 반복 실행할 때 중복을 막는다.
- `pdf`에는 `pdf_root` 기준 상대 경로(`<하위 폴더>/<파일명>`)만 넣는다. 절대 경로는 넣지 않는다. 원문 PDF는 저작권 때문에 저장소·사이트에 올리지 않는다.
- 파생 앱이 자기 필드를 더 붙여도 된다(예: 갈래 `branches`, 조건 `conditions`). 뷰어는 모르는 필드를 무시하게 만든다.
- 미발표 연구와 연결 짓는 메모(`relevance`·`questions`)가 들어가면 사이트를 비공개로 두거나 인증을 건다.
