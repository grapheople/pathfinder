# Pathfinder — 여행 LLM Wiki

이 저장소는 [Karpathy의 LLM Wiki 패턴](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)을
여행 도메인에 적용한 개인 지식 베이스다. 사용자는 큐레이션·방향 설정을 하고, LLM은 위키를 점진적으로 빌드·유지하며,
저장된 지식을 바탕으로 일정·선호도에 맞는 여행 계획과 콘텐츠를 산출한다.

이 문서는 LLM(Claude)의 행동 규약이다. 위키에서 어떤 작업을 하든 이 문서의 규칙을 따른다.

---

## 1. 3계층 구조

- **Raw Sources** (`sources/`) — 외부 자료의 요약·출처. LLM은 읽고 추가만 한다. 원본 사실은 여기로 거슬러 추적된다.
- **Wiki** (`wiki/`, `trips/`, `profile.md`) — LLM이 빌드·유지하는 정제된 지식과 산출물. 사람은 읽고, LLM이 쓴다.
- **Schema** (`CLAUDE.md`, `index.md`, `log.md`) — 행동 규약과 네비게이션. 이 문서가 schema의 핵심이다.

---

## 2. 디렉토리 구조

```
.
├── CLAUDE.md                  # 이 문서 (행동 규약)
├── index.md                   # 콘텐츠 카탈로그 (lint가 갱신)
├── log.md                     # 시간순 행위 로그 (append-only)
├── profile.md                 # 사용자 선호도·제약 (단일 파일)
├── sources/                   # raw 자료 요약본
│   └── YYYY-MM-DD-<slug>.md
├── wiki/
│   ├── countries/             # 국가 개관
│   ├── cities/                # 기본 단위. 도시별 페이지
│   ├── pois/                  # 명소·식당·숙소·액티비티·교통
│   └── themes/                # 교차 분류 (예: cherry-blossom-spots)
├── trips/
│   └── YYYY-MM-<destination>/
│       ├── index.md           # 여행 메타페이지 (필수)
│       ├── itinerary.md       # 일정·동선 (필수)
│       ├── content.md         # 6관점 콘텐츠 설계 (필수, 사전준비 3섹션 채움이 plan 종료조건)
│       ├── packing-list.md
│       ├── budget.md
│       ├── shot-list.md       # 시간순 촬영 컷 (콘텐츠 제작 시 선택)
│       ├── food-options.md    # 동선별 식당 후보 (선택)
│       ├── hotel-options.md   # 동네별 호텔 후보 (선택)
│       ├── logistics.md       # 출발 전 액션·돌발 대응 (선택)
│       ├── quick-reference.md # 현지 휴대용 한 화면 요약 (선택)
│       └── debrief.md         # 여행 후 회고 (선택)
└── assets/
    └── <page-slug>/           # 페이지별 이미지·파일
```

---

## 3. 엔티티 타입과 관계

| `type`      | 위치                        | 역할                                | 관계 필드               |
| ----------- | ------------------------- | --------------------------------- | ------------------- |
| `country`   | `wiki/countries/`         | 국가 개관 + 도시 목록                     | —                   |
| `city`      | `wiki/cities/`            | **기본 단위**. 역사·문화·교통·계절성 + 6관점 본문  | `country:`          |
| `poi`       | `wiki/pois/`              | 명소·식당·숙소·액티비티·교통 (`category`로 구분) | `city:`             |
| `theme`     | `wiki/themes/`            | 교차 분류 (벚꽃·야경·온천 등)                | `applies_to: []`    |
| `trip`      | `trips/<여행>/index.md`     | 한 여행의 메타페이지 + **이번 여행 한정 선호도**       | `destination:`      |
| `itinerary` | `trips/<여행>/itinerary.md` | 일정·동선 운영 문서                       | `trip:`, `content:` |
| `content`   | `trips/<여행>/content.md`   | 6관점 콘텐츠 설계                        | `trip:`             |
| `shot-list` | `trips/<여행>/shot-list.md` | 시간순 촬영 컷 + 6관점 매핑                  | `trip:`, `content:`, `itinerary:` |
| `food-options` | `trips/<여행>/food-options.md` | 동선별 식당 후보 (즉흥 방문용)            | `trip:`, `itinerary:` |
| `hotel-options` | `trips/<여행>/hotel-options.md` | 동네별 호텔 후보 매트릭스               | `trip:`, `itinerary:` |
| `logistics` | `trips/<여행>/logistics.md` | 출발 전 액션·예약·돌발 대응 통합            | `trip:`, `itinerary:` |
| `quick-reference` | `trips/<여행>/quick-reference.md` | 현지 휴대용 한 화면 압축 요약        | `trip:`             |
| `source`    | `sources/`                | raw 자료 요약·출처                      | `about: []`         |
| `profile`   | `profile.md`              | 사용자 선호도·제약                        | —                   |

**관계 방향 규약**: 자식이 부모를 가리킨다 (`poi.city → city`, `city.country → country`).
부모 페이지의 자식 목록은 손으로 유지하지 않고 **lint 워크플로우가 갱신**한다.

**교차 분류는 frontmatter `tags:` + theme 페이지로**: 한 POI가 야경이면서 벚꽃 명소면
`tags: [night-view, cherry-blossom]`. theme 페이지는 이 태그를 모아 보여주는 큐레이션.

---

## 4. 페이지 메타데이터 (YAML frontmatter)

### 공통 필드 (모든 페이지)
```yaml
---
type: city | poi | country | theme | trip | itinerary | content | shot-list | food-options | hotel-options | logistics | quick-reference | source | profile
title: 교토
aliases: [Kyoto, きょうと]
tags: [japan, kansai, historic]
created: 2026-05-27
updated: 2026-05-27
---
```

### `wiki/cities/*.md` 추가
```yaml
country: japan
region: 간사이
best_seasons: [spring, autumn]
duration_hint: 3-5일
```

### `wiki/pois/*.md` 추가
```yaml
city: kyoto
category: sight | food | stay | activity | transport | district
price_band: $ | $$ | $$$ | $$$$       # food / stay만
opening: "09:00-17:00 (월 휴무)"        # 선택
duration: 1-2h                          # 선택, sight만
status: candidate | confirmed | rejected | visited
```

### `wiki/themes/*.md` 추가
```yaml
scope: city | country | global
applies_to: [kyoto, tokyo]
```

### `trips/<여행>/itinerary.md` 추가
```yaml
trip: 2026-09-kyoto
content: "[[trips/2026-09-kyoto/content]]"
```
여행 컨텍스트(누가·언제·어디·예산·status)는 같은 디렉토리의 `index.md`에 단일 출처로 둔다. itinerary는 일정 운영에만 집중.

### `trips/<여행>/index.md` 추가 (trip 메타페이지)
```yaml
destination: kyoto
start: 2026-09-12
end:   2026-09-18
travelers: [me, partner]
budget_total: 2_500_000_KRW
status: planning | booked | ongoing | done
# 이번 여행 한정 선호도 (profile.md의 기준선을 덮어쓰거나 추가)
overrides:
  pace: relaxed              # tight | relaxed
  daily_walk_km_max: 10
  themes: [autumn-foliage, traditional-craft]
  must_avoid: [tourist-crowds]
```
본문에는 이번 여행의 동기·기대·맥락을 한두 문단으로 서술. `overrides`로 표현하기 어려운 미묘한 톤은 본문에 둔다.

### `trips/<여행>/content.md` 추가
```yaml
trip: 2026-09-kyoto
status: planning | shooting | editing | published
video_url:
```

### `sources/*.md` 추가
```yaml
source_url: https://...                 # 또는 source_file
fetched: 2026-05-27
language: ko
about: [kyoto, autumn-foliage]          # 환류된 wiki 페이지들
```

---

## 5. 명명·링크 규약

- **파일명**: `kebab-case.md`, 영어. 음차는 일관된 표준 — 일본어 헵번식(`fushimi-inari-shrine`), 중국어 병음 무성조(`xian-bell-tower`).
- **고유명사 충돌**: 동명이지(`paris`)는 국가 접미 `paris-france`, `paris-texas`.
- **본문 언어**: 한국어. 제목·태그도 한국어. 외래 고유명은 첫 등장 시 원어 병기.
- **위키링크**: 본문 내 다른 위키 페이지 참조는 항상 `[[file-slug]]`. 표시명이 다르면 `[[file-slug|표시명]]`. 한 페이지에서 처음 등장 시 반드시 링크, 같은 문단 내 반복은 자유.
- **외부 링크**: 일반 markdown `[텍스트](url)`. raw 가치가 있는 외부 자료는 본문에 박지 않고 `sources/`로 페치한 뒤 source 페이지를 `[[...]]` 링크. 본문에 살아있는 외부 URL은 *공식·예약·실시간성*이 본질인 경우(공식 운영 공지, 예약 페이지)에만.
- **alias**: 한국어·원어·로마자 표기는 frontmatter `aliases:`. Obsidian 자동완성에 노출됨.
- **이미지·파일**: `assets/<page-slug>/<filename>`. 본문엔 `![](assets/kyoto/gion.jpg)`.

---

## 6. 본문 권장 섹션

### `wiki/cities/*.md` · `wiki/pois/*.md`
**6관점을 미러링**하는 본문 구조. 모르는 항목은 섹션 자체를 생략한다(빈 섹션 두지 말 것).

```markdown
## 개요
한 문단 요약.

## 봐야할것 — 이 장소를 가장 잘 드러내는 풍경·공간
## 먹어야할것 — 기후·문화·생활이 녹은 음식 (왜 발전했는지까지)
## 배울것 — 사회가 문제를 푸는 방식·가치관
## 실용 정보 — 교통·시간·예약·계절성 (POI는 frontmatter로 일부 흡수)
## 출처
- [[sources/2026-05-27-kyoto-autumn]]
```

### `sources/*.md`
```markdown
## 핵심 요약
- ...

## 원문에서 인용
> ...
```

---

## 7. 콘텐츠 6관점 (`trips/<여행>/content.md`)

유튜브 콘텐츠 제작용 6관점은 **계획 단계부터 채우고**, **여행 중·후에 보완**하며, **콘텐츠 제작의 단일 진입점**이다.

```markdown
## 사전준비 (계획 단계에서 itinerary와 함께 채움)

### 봐야할것
- 여행지를 가장 잘 드러내는 풍경과 공간

### 먹어야할것
- 지역의 기후·문화·생활 습관이 녹아있는 음식 (왜 발전했는지까지)

### 배울것
- 그 사회가 문제를 해결하는 방식과 가치관 (시스템·서비스·공간·태도)

## 현지경험 (여행 중·후 채움)

### 발견한것
- 예상하지 못했지만 우연히 마주친 것

### 느낀것
- 몸과 감정이 먼저 반응한 것

### 생각해볼것
- 의미·구조로 해석한 것
```

- 사전준비 3섹션은 도시·POI 페이지의 같은 이름 섹션에서 인용/요약해 채운다.
- 현지경험 3섹션은 여행 중 메모·사진·대화에서 길어 올린다.
- 두 단계가 모두 채워지면 영상 스크립트·아웃라인(`script.md` 등)을 같은 디렉토리에 산출한다(필요 시).

---

## 8. 선호도의 두 계층 — `profile.md` + trip overrides

선호도는 여행마다 달라질 수 있다. 두 계층으로 분리해 다룬다.

### 8.1 `profile.md` — 글로벌 기준선
사람으로서 잘 변하지 않거나 천천히 변하는 것. 매 여행의 기본값.

자유 서술이되 다음을 포함한다:
- 체력 한계·식이 제약·알러지
- 일반적인 여행 스타일·강도 (하루 도보 거리, 새벽형/야간형, 빡빡한/느슨한 페이스)
- 매 여행 공통으로 추구하는 가치 (콘텐츠 제작 관점 포함)
- 평소 좋아하는 분위기·테마, 평소 피하는 것
- 평소 예산 패턴 (숙소·식사·교통의 우선순위)

### 8.2 Trip overrides — 이번 여행 한정 선호도
`trips/<여행>/index.md` frontmatter의 `overrides:` 블록 + 본문 서술.

이번 여행에만 달라지는 것:
- 동행자 (혼자 vs 파트너 vs 가족) — 페이스·예산이 통째로 달라짐
- 이번 여행의 테마 (이번엔 미식 중심, 다음엔 자연 중심)
- 시즌·날씨로 인한 일정 강도 조정
- 이번만의 예산 (성수기 프리미엄, 특별한 항공권 등)
- 일시적 제약 (부상, 동행자 컨디션)

### 8.3 머지 규칙
**Plan·Debrief·Query 워크플로우는 항상 두 계층을 합쳐서 사용**한다.
- 동일 키가 양쪽에 있으면 **trip overrides가 우선**.
- overrides에 없는 항목은 profile의 값을 그대로 사용.
- 충돌이 의심스러울 땐 사용자에게 확인 후 진행.

### 8.4 갱신 흐름
- **Debrief에서 발견된 학습**이 일회성이면 그 trip의 본문에만 남기고 profile은 건드리지 않는다.
- **여러 trip에서 반복 확인된 패턴**이면 profile 갱신 후보 → 사용자 동의 후 반영(예: "최근 3회 모두 하루 도보 10km에서 지침" → profile의 `daily_walk_km_max`를 10으로 갱신).
- `profile.md` 변경 시 `updated:` 갱신 + `log.md`에 `profile-update` 엔트리.
- **사용자가 명시하지 않으면 profile은 수정하지 않는다**. 후보만 제시한다.

---

## 9. `index.md`와 `log.md`

### `index.md` (콘텐츠 카탈로그, lint가 갱신)
```markdown
# Index

_Last linted: 2026-05-27_

## Countries
- [[japan]] — 4개 도시
- [[vietnam]] — 2개 도시

## Cities
- [[kyoto]] · japan · 봄·가을 · 3-5일 — 천 년 수도, 사찰·정원·전통 공예
- [[tokyo]] · japan · 사계절 · 3-7일 — 거대도시, 동네별 캐릭터

## POIs by city
### kyoto
- [[fushimi-inari-shrine]] · sight · 1-2h
- [[nishiki-market]] · food · $$

## Themes
- [[cherry-blossom-spots]] — 7개 도시
- [[night-views]] — 5개 도시

## Trips
- [[trips/2026-09-kyoto/index]] · planning · 2026-09-12 ~ 2026-09-18
- [[trips/2025-11-hanoi/index]] · done · 2025-11-03 ~ 2025-11-09

## Sources
### japan · tokyo
- [[sources/2026-05-27-dcinside-tokyo-weather]] — 월별 기후, 6월 장마
### korea · jecheon
- [[sources/2026-06-12-foret-resom-jecheon]] — 해브나인 스파·임산부 제한
```
**Sources 정렬 규칙**: `### <country> · <city>` 헤더로 묶고, 각 그룹 안은 날짜 오름차순. city가 불분명한 소스(국가 일반·다도시)는 `### <country> · 공통`에 둔다. 도시 없는 국가 일반은 `### <country>`. (차수·수집 회차로 묶지 않는다.)

### `log.md` (append-only, 시간순)
첫 줄은 고정 포맷: `## [YYYY-MM-DD] action | 제목` — Unix 도구로 grep·필터 가능.

```
## [2026-05-27] ingest | 교토 가을 단풍 명소 10선 (blog.example.com)
- sources/2026-05-27-kyoto-autumn-foliage.md 추가
- 영향: wiki/cities/kyoto.md(가을 섹션), wiki/pois/eikando.md(신규), wiki/themes/autumn-foliage.md(applies_to에 kyoto)

## [2026-05-27] plan | 2026-09-kyoto itinerary 초안
- trips/2026-09-kyoto/{index,itinerary,content,packing-list,budget}.md 생성
- content.md 사전준비 3섹션 채움

## [2026-05-25] lint
- 고아 0건, 깨진 링크 0건, 모순 1건(eikando 운영시간)
```

`action`은 다음 중 하나: `ingest`, `query`, `plan`, `debrief`, `content`, `lint`, `profile-update`.

---

## 10. 워크플로우 5종

각 워크플로우는 **트리거 → 단계 → 산출물 → 종료조건 → 로그**의 같은 모양이다.

### 10.1 Ingest — 소스 추가
**트리거**: 사용자가 URL·텍스트·사진·PDF를 던지거나 "이 자료 정리해줘"라고 함.

1. 소스를 읽는다(WebFetch / Read / 첨부).
2. 한 문단으로 핵심을 보고하고 위키 반영 가치를 사용자와 합의.
3. `sources/YYYY-MM-DD-<slug>.md` 작성: frontmatter + 핵심 요약 + 원문 인용.
4. 영향받는 wiki 페이지(보통 5~15개)를 식별·갱신. 본문에 `[[sources/...]]` 인용을 반드시 박는다.
5. 새 theme 후보면 `wiki/themes/` 신설 또는 `applies_to:` 갱신.
6. `index.md` 갱신, `log.md`에 `ingest` 엔트리.

**종료**: source 1개 + 영향 페이지 모두 + index·log 반영.

### 10.2 Query — 질문에 답
**트리거**: 위키 지식으로 답할 수 있는 질문 ("교토 가을 어디?", "이 도시 며칠?").

1. `index.md`를 먼저 본다. 후보 페이지 추린다.
2. 후보·인접 페이지 읽고, **답에 사용한 페이지를 `[[...]]`로 인용**.
3. 위키에 근거 없는 추측은 명시("위키에 없음, 일반 지식으로는 …").
4. 답이 "재사용 가치 있는 새 종합"이면 사용자에게 페이지화 제안 → 동의 시 `wiki/themes/` 등에 저장하고 log에 `query→theme` 기록.

**종료**: 답변 + 출처 인용. (페이지화는 선택)

### 10.3 Plan — 여행 계획 수립
**트리거**: "X로 N박 계획 짜줘" 또는 새 여행 시작.

1. **`profile.md` 먼저 읽기 (글로벌 기준선)**. 여기 있는 정보는 다시 묻지 않는다.
2. **이번 여행 한정 컨텍스트 질의** — 동행자·기간·이번 테마·이번 예산·특이 제약 등 profile에서 달라질 부분. 짧게.
3. `trips/<YYYY-MM-destination>/index.md`를 먼저 만들어 응답을 `overrides:`와 본문에 기록한다(트립 시작점).
4. **머지된 선호도**(profile ⊕ trip overrides)를 기준으로 다음 단계 진행.
5. 대상의 `wiki/cities/`·`wiki/pois/`·`wiki/themes/` 읽기. 정보 공백이 크면 **Ingest로 일시 분기**해 보강.
6. 나머지 trip 산출물 작성:
   - `itinerary.md` — 시간 단위로 채움. 같은 도시 안 동선 군집화. 이동·식사·휴식 버퍼 명시.
   - **`content.md` — 사전준비 3섹션을 동시에 채움 (필수)**
   - `packing-list.md`, `budget.md` 개요
7. itinerary·content를 서로 `[[...]]`로 묶기.
8. 루트 `index.md`·`log.md` 반영.

**정상 종료**: trip `index.md`에 overrides 기록됨 + itinerary 시간 단위 + **content.md 사전준비 3섹션 빈 칸 없음**. 일정만 있고 content가 비면 미완 — 그 사실을 사용자에게 알리고 마무리.

### 10.4 Debrief & 콘텐츠 제작 — 여행 중·후
**트리거**: 여행 중 메모를 던지거나, 여행 후 회고·콘텐츠 작업 시작.

1. `content.md`의 **현지경험 3섹션**(발견·느낀·생각해볼)을 함께 채움. 초안→사용자 확인·수정.
2. 새 사실·인상을 위키로 환류:
   - 운영시간·가격 등 사실 갱신 → POI 페이지, `status: visited`.
   - "이 도시를 잘 드러낸다" 같은 발견 → `wiki/cities/<city>.md`의 봐야할것 섹션 보강. 출처는 `[[trips/.../content]]`.
3. `trips/<여행>/debrief.md` — 좋았던 것/아쉬웠던 것/적용 점. 학습 사항은 일단 여기에 기록. **여러 trip에서 반복 확인된 패턴이라고 판단될 때만** `profile.md` 갱신 후보를 제시 → 사용자 동의 시 반영(섹션 8.4 규칙). 일회성 학습은 trip debrief에만 남기고 profile은 건드리지 않는다.
4. 콘텐츠 제작 단계로 들어가면 `content.md` 기반으로 `script.md` 등 산출.
5. `content.md` status: `shooting → editing → published`. published 시 `video_url:` 채움.
6. 단계별로 `log.md`.

**종료**: 현지경험 3섹션 채움 + 위키 환류 + (콘텐츠 단계 진입 시) 산출물 status 갱신.

### 10.5 Lint — 주기 점검
**트리거**: 사용자가 "정리해줘" / "위키 상태 점검" 또는 ingest·plan 누적이 일정량 이상.

1. **고아 페이지** — 어디서도 링크되지 않은 wiki 페이지 (sources·profile 제외).
2. **깨진 wikilink** — 존재하지 않는 슬러그.
3. **부모↔자식 정합성** — `poi.city`가 가리키는 city 페이지의 본문 자식 목록.
4. **모순** — 같은 사실에 서로 다른 진술(운영시간·가격 등).
5. **오래된 정보** — `fetched`가 12개월 이상 지난 운영 정보.
6. **`index.md` 재생성** — frontmatter 스캔으로 카테고리·요약·메타 갱신. 마지막 줄 `_Last linted: YYYY-MM-DD_`.
7. **자동 수정은 1·2·3·6번까지만**. 4·5는 후보 제시 후 사용자 결정 대기.
8. `log.md`에 `lint` 엔트리(요약 통계).

**종료**: index.md 갱신 + 모순·오래된 정보 후보 목록 보고.

---

## 11. 동작 원칙

- **출처 우선**. 사실 주장 옆에는 가급적 `[[sources/...]]` 또는 `[[trips/.../content]]` 인용.
- **위키에 없으면 명시**한다. 일반 지식으로 보충했으면 "위키 외부 지식" 표지.
- **모순을 만나면 덮어쓰지 말고 두 진술과 출처를 함께 남긴다**. lint에서 사용자가 판단.
- **추측·환각 금지**. 운영시간·가격·예약 가능 여부는 1차 출처 없으면 비워두고 "확인 필요" 표시.
- **위키 변경은 항상 `log.md`로 추적**. 추적되지 않는 변경은 없다.
- **빈 섹션·플레이스홀더 금지**. 모르면 섹션 자체를 생략.
- **사용자가 명시적으로 요청하지 않은 페이지 신설·삭제·대규모 재편은 하지 않는다**. 변경 후보가 떠오르면 제안한다.
- **`profile.md`는 사용자 동의 없이 수정하지 않는다**. Debrief 결과로 갱신 후보가 나와도 묻고 반영.

---

## 12. 부트스트랩

위키가 비어 있는 초기 상태에서는:

1. 사용자에게 `profile.md`의 핵심 항목을 한두 차례에 걸쳐 채울 수 있도록 묻는다(전부 한 번에 묻지 않는다).
2. 다음 여행지·관심 지역이 있으면 그 도시의 첫 `wiki/cities/<city>.md`를 Ingest로 시작한다.
3. `index.md`·`log.md`는 첫 작업과 함께 만들어진다.
4. 한 여행을 끝까지 굴리면(plan → debrief → content) 위키의 모든 영역이 자연스럽게 채워지도록 설계되어 있다.

---

이 문서 자체도 시간이 지나며 진화한다. 사용 중 마찰이 생기면 사용자와 합의해 이 규약을 갱신한다.
변경 시 `log.md`에 `## [날짜] schema-update | 요약` 엔트리를 남긴다.
