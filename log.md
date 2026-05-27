# Log

이 파일은 위키의 모든 변경을 시간순으로 기록한다(`CLAUDE.md` 섹션 9).
첫 줄 포맷: `## [YYYY-MM-DD] action | 제목`. action ∈ {ingest, query, plan, debrief, content, lint, profile-update, schema-update}.

---

## [2026-05-27] profile-update | profile.md 초기 작성

부트스트랩 인터뷰로 글로벌 기준선 작성.

- 생성: `profile.md`
- baseline 키: pace=balanced, start_time=standard, daily_walk_km_max=5, themes=[history-tradition, food-local, design-architecture-systems], must_avoid=[tight-schedule, group-tour], dietary_constraints=none, budget_pattern=balanced
- 본문: 여행 스타일·테마·식이·예산·콘텐츠 제작 가치

## [2026-05-27] plan | 2026-06-tokyo

도쿄 3박 4일 가족 여행 계획 초안 작성.

- 생성: `trips/2026-06-tokyo/{index,itinerary,content,packing-list,budget}.md`
- 생성: `index.md` (위키 루트 카탈로그 초기 버전)
- trip overrides: pace=relaxed (baseline 덮어씀), daily_walk_km_max=3 (baseline 5 → 3), themes에 kid-pokemon 추가, must_avoid에 long-queues·hot-onsen·smoking-restaurants·many-stairs 추가, transport_pattern=taxi-takkyubin, hotel_rotation=[asakusa, shinjuku, ginza]
- content.md 사전준비 3섹션 채움(봐야할것·먹어야할것·배울것)
- 영향 wiki: 아직 시드 안 함. itinerary에 wikilink만 박음(깨진 링크 상태) — 후속 단계로 `wiki/cities/tokyo.md` + 핵심 POI 시드 예정

### itinerary에 박힌 POI 시드 후보

- [[senso-ji]] (sight)
- [[sumida-river]] (sight)
- [[tokyo-skytree]] (sight)
- [[shinjuku-gyoen]] (sight)
- [[tokyo-metropolitan-government-building]] (sight, free observatory)
- [[pokemon-center-mega-tokyo]] (activity, kid)
- [[pokemon-center-shibuya]] (activity, kid)
- [[shibuya-scramble]] (sight)
- [[ginza]] (district)
- [[tsukiji-outer-market]] (food)

## [2026-05-27] lint | 3차

신규 5개 파일(shot-list·food-options·hotel-options·logistics·quick-reference) 추가 + schema-update 후의 정합성 점검.

- 고아 페이지: **0건** (모든 신규 파일이 trip index 산출물 섹션에서 참조됨)
- 깨진 wikilink 후보: **2건** → 모두 false positive 또는 의도된 미래 페이지
  - `[[hanayashiki]]` (log.md 151줄) — **false positive**: backtick 안의 인용 표기. Obsidian은 backtick 안의 wikilink를 인식하지 않음. grep regex만 잡음.
  - `[[trips/2026-06-tokyo/debrief|debrief]]` (shot-list.md·food-options.md 각 1건) — **의도된 미래 페이지**: trip 종료 후 `debrief.md` 생성 예정이며 사후 환류 절차 인용 차원에서 미리 박음.
- 부모↔자식 정합성: **0건** (japan↔tokyo, tokyo↔14 POI, theme↔tokyo)
- 모순 후보: **0건**
- 오래된 자료(12개월+): **0건**
- index.md: `_Last linted_` 헤더 갱신, 3차 ingest 표기

위키 상태: **완전 정합** (실유효 깨짐 0건). 42개 마크다운 파일 (CLAUDE+index+log+profile+.gitignore 5 / sources 14 / wiki 16 / trips 10 / .claude/commands 5 + .obsidian 3 = 시스템 외).

## [2026-05-27] schema-update | trip 보조 type 5종 정식 등록

이번 trip(2026-06-tokyo)에서 누적된 trip 보조 type 5종을 CLAUDE.md schema에 정식 등록.

추가된 type:
- `shot-list` — 시간순 촬영 컷 + 6관점 매핑
- `food-options` — 동선별 식당 후보 (즉흥 방문용)
- `hotel-options` — 동네별 호텔 후보 매트릭스
- `logistics` — 출발 전 액션·예약·돌발 대응 통합
- `quick-reference` — 현지 휴대용 한 화면 압축 요약

수정 위치:
- CLAUDE.md 섹션 3 엔티티 타입표 (5행 추가, 관계 필드 명시)
- CLAUDE.md 섹션 4 공통 필드 type 값 enum 확장
- CLAUDE.md 섹션 2 trips 디렉토리 구조 — 모든 보조 파일 명시(필수/선택 구분)

이유: 한 trip을 끝까지 굴려보니 9개 산출물이 안정적으로 나옴. 다음 trip부터 같은 패턴을 자연스럽게 재사용 가능. 모든 보조 파일은 선택(optional), itinerary·content·index만 필수.

## [2026-05-27] schema-update | poi.category에 district 추가

CLAUDE.md 섹션 4의 `wiki/pois/*.md` frontmatter `category` 값에 `district` 추가.
이유: 신주쿠처럼 단일 명소가 아닌 "구(區) 단위 지구"를 POI로 두기 위함.
긴자·시부야·아사쿠사 등 향후 동네 페이지에 일관 적용.

## [2026-05-27] ingest | 디시인사이드 노숙·간토 갤러리 도쿄 시리즈 6편

도쿄 위키 시드 1차.

소스:
- `sources/2026-05-27-dcinside-tokyo-weather.md` (월별 기후·6월 장마)
- `sources/2026-05-27-dcinside-tokyo-onsen.md` (하코네·구사츠)
- `sources/2026-05-27-dcinside-tokyo-day-trips.md` (요코하마·가마쿠라·에노시마·하코네)
- `sources/2026-05-27-dcinside-tokyo-preparations.md` (Visit Japan Web·환전·통신·교통)
- `sources/2026-05-27-dcinside-tokyo-shinjuku.md` (4개 에리어·지하 터널)
- `sources/2026-05-27-dcinside-tokyo-asakusa-skytree.md` (센소지·나카미세 매너·스카이트리)

신규 위키 페이지:
- `wiki/countries/japan.md` (stub)
- `wiki/cities/tokyo.md` (개관·6관점·계절성·동네)
- `wiki/pois/senso-ji.md` (sight)
- `wiki/pois/tokyo-skytree.md` (sight)
- `wiki/pois/tokyo-metropolitan-government-building.md` (sight, 무료 전망대)
- `wiki/pois/shinjuku-gyoen.md` (sight)
- `wiki/pois/shinjuku.md` (**district** — 신규 category 사용)
- `wiki/pois/omoide-yokocho.md` (food)
- `wiki/themes/tokyo-day-trips.md`

trip 산출물 보강:
- `trips/2026-06-tokyo/itinerary.md` — 6/4 센소지에 나카미세 매너 메모, 도청에 지하 터널 메모, 6월 장마 대비 섹션 추가
- `trips/2026-06-tokyo/packing-list.md` — Visit Japan Web, 트래블월렛, IC카드 강조, 우비 추가, 나리타 터미널 주의

루트 `index.md` 전체 갱신.

남은 깨진 wikilink (다음 ingest 후보):
- `[[sumida-river]]`, `[[pokemon-center-mega-tokyo]]`, `[[pokemon-center-shibuya]]`, `[[shibuya-scramble]]`, `[[ginza]]`, `[[tsukiji-outer-market]]`

## [2026-05-27] lint

1차 점검 결과 (report 모드).

- 고아 페이지: **0건**
- 깨진 wikilink: **6건** (sumida-river, pokemon-center-mega-tokyo, pokemon-center-shibuya, shibuya-scramble, ginza, tsukiji-outer-market) — 모두 시드 안 된 POI. 페이지 신설(ingest)로만 해결 가능.
- 부모↔자식 정합성: **0건** (japan↔tokyo, tokyo↔모든 POI, tokyo-day-trips↔tokyo 모두 정합)
- 모순 후보: **0건** (위키가 신생이라 비교 데이터 부족)
- 오래된 자료(12개월+): **0건** (모든 source가 2026-05-27 fetched)
- index.md: `_Last linted_` 헤더만 갱신. 콘텐츠 카탈로그는 직전 ingest에서 이미 갱신됨.

추가 발견:
- `sources/2026-05-27-dcinside-tokyo-asakusa-skytree.md`의 `about:`에 `asakusa`가 있지만 `wiki/pois/asakusa.md`는 없음 — 동네 페이지 시드 후보(district).

## [2026-05-27] ingest | 2차 — 공식 사이트 + 한국어 위키피디아 7건

`/lint` 1차에서 식별된 깨진 wikilink 6건 + asakusa 동네 = 7건 시드.

소스:
- `sources/2026-05-27-pokemon-center-megatokyo-official.md` (shop.pokemon.co.jp)
- `sources/2026-05-27-pokemon-center-shibuya-official.md` (shop.pokemon.co.jp)
- `sources/2026-05-27-wikipedia-ginza.md` (ko.wikipedia.org)
- `sources/2026-05-27-wikipedia-tsukiji.md`
- `sources/2026-05-27-wikipedia-asakusa.md`
- `sources/2026-05-27-wikipedia-shibuya.md`
- `sources/2026-05-27-wikipedia-sumida-river.md`

신규 위키 페이지 (모두 city: tokyo):
- `wiki/pois/asakusa.md` (district)
- `wiki/pois/ginza.md` (district)
- `wiki/pois/sumida-river.md` (sight)
- `wiki/pois/shibuya-scramble.md` (sight)
- `wiki/pois/pokemon-center-mega-tokyo.md` (activity)
- `wiki/pois/pokemon-center-shibuya.md` (activity)
- `wiki/pois/tsukiji-outer-market.md` (food)

부모 페이지 갱신:
- `wiki/cities/tokyo.md` — 동네·POI 목록에 신규 7건 반영. asakusa·ginza district 추가, shibuya-scramble·sumida-river·pokemon 2곳·tsukiji 추가.

trip 산출물 보강:
- `trips/2026-06-tokyo/itinerary.md` — 6/4 14:30 포켓몬 메가도쿄에 **임시휴업 경고**(출발 1주 전 재확인) + 대체안(피카츄 스위츠·포켓몬 카드 스테이션·Pokémon GO Lab). 6/6 08:30 츠키지에 휴무·좌석 정보.

⚠️ **중요 발견 — 사용자 결정 필요**:
- `pokemon-center-mega-tokyo` 공식 사이트가 페치 시점(2026-05-27) **임시휴업** 표기. 재개 시기 미정. 6/4 일정의 6세 아이 핵심 후크이므로 **재오픈 여부 출발 전 반드시 재확인**. 재오픈 안 되면 시부야점(6/5) + 카드 스테이션·포켓몬 카페로 대체 또는 다른 후크(지브리·산리오 등) 고려.

남은 깨진 wikilink:
- (없음) — 1차 식별된 6건 모두 시드. 새로 박힌 `hanayashiki`(아사쿠사 페이지에 등장)은 wikilink로 박지 않았으므로 깨짐 아님.

루트 `index.md` 전면 갱신 — district/sight/activity/food 카테고리별 정렬.

## [2026-05-27] lint

2차 점검 (report 모드).

- 고아 페이지: **0건**
- 깨진 wikilink: **1건** → 자동 수정 1건 = 잔여 **0건**
  - log.md 자체 메모 안에서 `[[hanayashiki]]`로 잘못 박힌 자기 모순(아이러니컬) — backtick으로 정정
- 부모↔자식 정합성: **0건** (japan↔tokyo, tokyo↔모든 POI(13), tokyo-day-trips↔tokyo 모두 정합)
- 모순 후보: **0건**
- 오래된 자료(12개월+): **0건**
- index.md: `_Last linted_` 헤더 갱신

위키 상태: **완전 정합**. 36개 마크다운 파일 (CLAUDE+index+log+profile 4 / sources 13 / wiki 12 / trips 5 / countries+themes 2).

## [2026-05-27] ingest | 3차 — teamLab Planets 백업 후크 시드

`6/4 메가도쿄 임시휴업 백업`으로 [[teamlab-planets-tokyo]] 시드.

소스:
- `sources/2026-05-27-general-knowledge-teamlab-planets.md` — 공식 사이트(`teamlab.art/e/planets/`) fetch 시 빈 응답(JS 렌더링 추정). ko.wikipedia 404. en.wikipedia 다른 회사 페이지로 잘못 라우팅. **일반 지식 + 출발 전 공식 재확인 표지**.

신규 위키 페이지:
- `wiki/pois/teamlab-planets-tokyo.md` (activity, city: tokyo)

trip 산출물 보강:
- `trips/2026-06-tokyo/itinerary.md` — **6/4 백업 분기 섹션 신설** (분기 A: 재오픈→원안 / 분기 B: 휴업→teamLab Planets 교체, 시간 단위 동선 포함). **6월 장마 우천 Plan B 섹션 재작성** (슬롯별 대체 + 일반 원칙).
- `trips/2026-06-tokyo/content.md` — 백업 시나리오의 콘텐츠 6관점 의미 메모(teamLab는 "배울것" 축 강화).

부모 페이지 갱신:
- `wiki/cities/tokyo.md` POI 목록에 teamlab-planets-tokyo 추가.

루트 `index.md` — POIs/Sources 섹션 갱신.

## [2026-05-27] plan-update | 2026-06-tokyo 백업 분기 결정

trip 일정에 **출발 1주 전 트리거** 추가:
- 메가도쿄 재오픈 확인 → 분기 A·B 결정
- 분기 B 발동 시 TeamLab Planets 사전 예매 동시 처리

## [2026-05-27] content | 2026-06-tokyo shot-list.md 신설

콘텐츠 촬영 동선·컷 리스트를 `trips/2026-06-tokyo/shot-list.md`로 분리(type: shot-list 신규 — CLAUDE.md 스키마에 명시 안 됨, 향후 schema-update 후보).

- 4일 × 슬롯별 시간순 컷 매핑 (총 약 30컷)
- 필수(必)·선택(選)·우천 대체(☔) 분류
- 6관점별 컷 충당표 — "배울것" 축에 takkyubin·도청 무료·지하 터널·신주쿠 교엔의 공공성·시부야 스크램블·긴자 룰·(분기 B 시) teamLab
- 가족 페이스 보호 원칙(인물 컷 하루 누적 10분 이하, 컷 5~15초)
- 장비·셋업 메모(6월 장마 카메라 방수)

trip index·content에 shot-list 링크 추가.

## [2026-05-27] plan-update | 2026-06-tokyo hotel-options.md 신설

동네별 호텔 후보 리스트를 `trips/2026-06-tokyo/hotel-options.md`로 분리(type: hotel-options 신규, schema-update 후보, food-options와 동일 패턴).

- 아사쿠사 6곳 / 신주쿠 7곳 / 긴자 7곳 후보
- 결정 기준 매트릭스 (가족룸·금연·욕조·takkyubin·역 도보·짐 보관)
- 가격·욕조·가족룸·역 접근 표시, ⭐ 추천 1순위
- 묶음 시너지(Hyatt 멤버십 / Mitsui Garden 그룹)
- 비교 사이트별 특이점·환불 정책 우선순위·후기 키워드
- 영문 요청 템플릿 (임산부·아동 동반 사정 명기)
- 사후 환류 절차 — 결정 후 갱신할 5개 파일(index·budget·itinerary·logistics·shot-list) 명시

trip index 산출물 섹션에 hotel-options 링크 추가. 잔여 결정 사항: 호텔 3곳 + 긴자 디너 가게.

## [2026-05-27] plan-update | 2026-06-tokyo logistics.md 신설

출발 전 액션 체크리스트(D-30~D-1) + 정액 택시 회사 픽 + Visit Japan Web 가이드 + 돌발 대응(임산부·아동) + 일본어 카드 + 비상 동선 표를 통합한 `trips/2026-06-tokyo/logistics.md`(type: logistics, schema-update 후보) 신설.

핵심 결정·정보:
- **정액 택시 1순위 MK 택시**, 2순위 Nihon Kotsu. 미니밴(JPN Taxi 또는 8인승). 출발 D-7 ~ D-5 예약.
- Visit Japan Web — D-14 가족 3인 등록, QR 폰 캡처·인쇄 백업.
- 임산부 친화 의료기관 — **세이루카 국제병원(츠키지·긴자 인접)**, **NCGM(신주쿠)** 2곳을 일자별 비상 동선과 매핑.
- 한국 영사콜센터 +82-2-3210-0404 / 일본 119·110 명시.
- 임산부·아동·식당 일본어 카드 작성 (폰 캡처용).
- 도쿄도 의료정보 검색 사이트(히마와리) 메모.

itinerary.md "보완 필요" 섹션을 단순화 — 세부는 logistics로 위임. 호텔·긴자 디너 결정만 잔여 결정 사항으로 명시.

trip index 산출물 섹션에 logistics 링크 추가.

## [2026-05-27] plan-update | 2026-06-tokyo food-options.md 신설

동선별 식당 후보 리스트를 `trips/2026-06-tokyo/food-options.md`로 분리(type: food-options 신규 — schema-update 후보, shot-list와 같은 패턴).

- 6/3~6/6 슬롯별 후보 + 도요스 백업
- 카테고리 구성: 텐동·우나기·야끼니쿠·회/스시·라멘·돈카츠·해산물덮밥 등
- 🦴 우나기 / 🥩 야끼니쿠 후보 별도 묶음 (사용자 요청에 따라 추가)
- 임산부 비흡연 + 6세 입장 가능 가게 위주, 가격대·예약 난이도 표시
- 일반 주의: 흡연 정책·고급 스시 어린이 정책·날 음식·카운터 좌석
- 사후 환류: 실제 방문 가게는 debrief → 필요 시 `wiki/pois/<slug>.md`(category: food)로 시드

trip index 산출물 섹션에 food-options 링크 추가.
