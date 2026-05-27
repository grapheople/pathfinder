---
description: Raw 소스를 위키에 흡수 (CLAUDE.md 10.1 Ingest)
argument-hint: [URL · 파일 경로 · 붙여넣은 텍스트 설명]
---

# Ingest — 소스 추가

`CLAUDE.md`의 워크플로우 10.1(Ingest)을 그대로 수행한다. 이 문서가 규약과 어긋나면 CLAUDE.md가 우선이다.

## 사용자 입력

$ARGUMENTS

위 입력이 비어 있으면 사용자에게 URL/파일/텍스트를 요청하고 시작한다.

## 절차

### 1. 소스 읽기
- URL → `WebFetch`
- 로컬 파일 (PDF·텍스트·이미지) → `Read`
- 붙여넣은 텍스트 → 그대로 사용
- 가져온 날짜·언어·도메인을 기록해둔다 (다음 단계 frontmatter용).

### 2. 핵심 보고 + 가치 합의
한 문단으로 사용자에게 보고:
- 무엇에 관한 자료인지
- 위키에 새로 줄 수 있는 정보 3~5개 핵심
- 영향이 갈 만한 wiki 페이지 후보

**사용자 동의 없이 wiki/는 수정하지 않는다.** "이대로 위키에 흡수할까요?" 묻고 대기.

### 3. `sources/YYYY-MM-DD-<slug>.md` 작성
- 슬러그: 제목을 kebab-case로(영문 짧게).
- frontmatter:
  ```yaml
  type: source
  title: <원본 제목>
  aliases: []
  tags: [<관련 도시·테마 영문 슬러그>]
  created: <오늘>
  updated: <오늘>
  source_url: <원본 URL>          # 또는 source_file: <경로>
  fetched: <오늘>
  language: ko | en | ja | ...
  about: [<환류된 wiki 페이지 슬러그>]   # 4단계 후 갱신
  ```
- 본문:
  - `## 핵심 요약` — 불릿 5~10개. **사실과 의견 구분**.
  - `## 원문에서 인용` — 짧은 인용 몇 개(저작권 고려).

### 4. 영향 페이지 갱신
보통 5~15개. 각각에 대해:
- **신규 페이지**가 필요하면 사용자에게 신설 여부 확인 후 생성. POI는 `wiki/pois/`, 도시는 `wiki/cities/`, 등.
- **기존 페이지** 보강 시 해당 섹션(6관점 또는 실용 정보·출처)만 수정.
- 본문에 출처 인용 `[[sources/<날짜>-<slug>]]` 반드시 박는다.
- 변경된 페이지의 frontmatter `updated:`를 오늘로 갱신.

### 5. Theme 처리
새로운 교차 분류(벚꽃·야경·온천 등)가 떠오르면:
- 기존 `wiki/themes/` 페이지가 있으면 `applies_to:`에 도시 추가.
- 없으면 사용자에게 새 theme 신설 여부 확인 후 `wiki/themes/<slug>.md` 생성.

### 6. `sources/` frontmatter `about:` 채움
4·5단계에서 실제 환류된 wiki 페이지 슬러그들을 source 페이지의 `about:`에 기록(역추적용).

### 7. 루트 `index.md` 갱신
- Cities·POIs·Themes·Sources 섹션에 신규/갱신 반영.
- 카운트(`— N개 도시` 같은)도 갱신.

### 8. `log.md` 추가
```
## [YYYY-MM-DD] ingest | <원본 제목> (<도메인 또는 파일명>)
- sources/<날짜>-<slug>.md 추가
- 영향: <변경된 wiki 페이지 목록>
- 신규 theme: <있으면>
```

## 정상 종료 조건

- `sources/` 페이지 1개가 frontmatter·요약·인용 다 갖춰 생성됨
- 영향 wiki 페이지가 모두 갱신되고 본문에 `[[sources/...]]` 인용 박힘
- 영향 페이지의 `updated:` 갱신
- source 페이지의 `about:`에 실제 환류 페이지 기록
- `index.md`·`log.md` 반영

## 마지막 보고

한 줄 요약 + 신규/갱신 파일 목록. 예:
> 교토 가을 단풍 자료 흡수 완료. 신규 `sources/2026-05-27-kyoto-autumn-foliage.md`, `wiki/pois/eikando.md`. 갱신 `wiki/cities/kyoto.md`(가을 섹션), `wiki/themes/autumn-foliage.md`(applies_to에 kyoto). index·log 반영.
