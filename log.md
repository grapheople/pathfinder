# Log

이 파일은 위키의 모든 변경을 시간순으로 기록한다(`CLAUDE.md` 섹션 9).
첫 줄 포맷: `## [YYYY-MM-DD] action | 제목`. action ∈ {ingest, query, plan, debrief, content, lint, profile-update, schema-update}.

---

## [2026-06-12] schema-update | index.md Sources를 국가·도시별 그룹으로

CLAUDE.md 9절 index.md 템플릿의 Sources 정렬 규칙 변경.

- 변경: `최근 10개만 표시` → `### <country> · <city>` 그룹, 그룹 내 날짜 오름차순. (차수·수집 회차로 묶지 않음)
- 도시 불분명 소스는 `### <country> · 공통`, 국가 일반은 `### <country>`.
- 반영: `CLAUDE.md`(9절 템플릿+규칙), `index.md`(Sources 섹션 japan·tokyo / korea·jecheon로 재구성)
- 영향: 앞으로 `/lint`의 index 재생성도 이 규칙을 따라야 함.

## [2026-06-12] plan | 2026-06-jecheon (제천 포레스트 리솜 1박 2일)

휴양형 리조트 plan. 6/16(화) 13:00 자가용 출발, 1박 2일, 모든 식사 리조트 내. 동행: 아내(임신 22주, 컨디션 양호) + 아들 6세.

- 생성(trip): `trips/2026-06-jecheon/{index,itinerary,content,packing-list,budget}.md`
- 생성(wiki): `wiki/countries/korea.md`, `wiki/cities/jecheon.md`, `wiki/pois/foret-resom-jecheon.md`, `wiki/pois/haevnine-wellness-spa.md`
- 생성(source): `sources/2026-06-12-foret-resom-jecheon.md` (공식+웹검색 교차확인)
- content.md 사전준비 3섹션: 채움 (봐야할것/먹어야할것/배울것 — "자연 위의 휴양 설계"가 척추)
- overrides: pace=relaxed, daily_walk_km_max=3, themes=[spa-wellness, forest-nature, kid-water-play], transport=self-drive
- ⚠️ 핵심 리스크: **임산부 스파풀 이용 제한**(공식 안내) → index에 D-Day 전화 확인 액션. 운영시간 출처 충돌도 확인 대상.
- 영향: 루트 `index.md`(Countries/Cities/POIs/Trips/Sources에 korea·jecheon·제천 trip 추가)

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

## [2026-06-02] plan-update | 2026-06-tokyo 호텔 1박 확정 + 입국 이동 수단 변경

사용자 결정 사항:
- **6/3 숙소: [[onyado-nono-asakusa|天然温泉 凌雲の湯 御宿 野乃 浅草 (본관)]]** (Dormy Inn 그룹 다다미·천연 온천). 본관 확정 (별관 凌天の湯 아님).
- **6/3 입국 이동: 케이세이 본선 액세스 특급 직통** (NRT → 아사쿠사역). 환승 0, 가족 3인 약 3,500엔, 60~75분. 정액 택시 대비 약 21,000엔 절감 + 호텔까지 도보 동선.
- 6/6 출국은 정액 택시 유지(잠정).

신규 wiki:
- `wiki/pois/onyado-nono-asakusa.md` (category: **stay**, status: confirmed) — 새 category 첫 사용
- `sources/2026-06-02-general-knowledge-onyado-nono-asakusa.md` (공식 fetch 부분 성공, 일반 지식 + 표지)

영향 산출물 갱신:
- `itinerary.md` — 6/3 정액 택시 → 케이세이 본선 액세스 특급으로 교체, 호텔명 명시, 21:30 무료 야식 라멘 슬롯 추가, 보완 필요 섹션 갱신
- `budget.md` — 이동비 약 21,000엔 절감, 호텔 1박 가격 갱신
- `quick-reference.md` — 호텔란·이동란 채움, 6/3 동선 갱신
- `logistics.md` — §2 공항 이동 섹션 재작성 (입국=케이세이 본선 확정 / 출국=정액 택시 + 대안), D-7 체크리스트 갱신 (정액 택시 2회→1회, 호텔 메일 확인), 비상 동선 표 갱신
- `shot-list.md` — 6/3 컷에 케이세이 본선·호텔 다다미·야식 라멘 추가, 콘텐츠 6관점 "배울것" 매핑
- `hotel-options.md` — 아사쿠사 결정 표시, 후보들은 참고로 보관
- `trip index.md` — overrides에 `airport_transfer_in/out` 키 추가, 확정 사항 섹션 신설
- `wiki/cities/tokyo.md` — POI 목록에 stay 카테고리 신설
- `index.md` — POI by city에 stay 섹션 + Sources 4차 항목

잔여 결정 사항 (사용자 보류 — 2026-06-02 정정):
- 신주쿠 호텔 픽 (보류)
- 긴자 디너 가게 (보류)

## [2026-06-02] plan-update | 2026-06-tokyo 6/5 호텔 + 6/6 복귀 플랜 확정

사용자 결정 사항:
- **6/5 숙소: [[super-hotel-premier-ginza|Super Hotel Premier Ginza]]** (Super Hotel 그룹 Premier 라인, 대욕장·무료 조식). 가성비 + 츠키지·히가시긴자역 도보권 = 6/6 복귀 동선과 정합.
- **6/6 출국: 정액 택시 (긴자 → NRT)** 확정. 임산부 22주차 + 5시간 비행 직전 컨디션 보전 우선. 차액 약 19,000엔이 합리적 투자. 대안(액세스 특급)은 logistics에 정보 보존.

복귀 플랜 시간 단위 동선:
- 07:30 Super Hotel 무료 조식
- 08:30 큰 캐리어 호텔 클로크 보관
- 09:00 츠키지 장외시장 (호텔 도보 5~10분)
- 09:30 마지막 식사 (스시잔마이·타마고야키 등)
- 10:30 호텔 복귀·짐 픽업
- 11:00 체크아웃
- 11:15 정액 택시 출발 → 12:30 NRT → 14:00 출국 → 17:30 인천 입국

신규 wiki:
- `wiki/pois/super-hotel-premier-ginza.md` (category: stay, status: confirmed)
- `sources/2026-06-02-general-knowledge-super-hotel-premier-ginza.md` (공식 404, 일반 지식 + 표지)

영향 산출물 갱신:
- `itinerary.md` — 6/5 호텔 표기·대욕장 추가, 6/6 복귀 플랜 시간 단위로 재작성, 출국 이동 결정 명시
- `budget.md` — 긴자 1박 약 25,000엔 절감(상급→Super Hotel Premier 가성비 전환)
- `quick-reference.md` — 6/5 호텔란·6/6 동선 채움
- `logistics.md` — 6/6 출국 정액 택시 확정, 대안(액세스 특급) 정보 보존, 비상 동선 표 갱신
- `shot-list.md` — 6/6 컷에 Super Hotel 어메니티·츠키지·정액 택시 안 갱신
- `hotel-options.md` — 긴자 결정 표시, 후보 참고 보관
- `trip index.md` — 확정 사항 섹션에 6/5·6/6 정보 추가, overrides 코멘트 갱신
- `wiki/cities/tokyo.md`·루트 `index.md` — stay 섹션에 super-hotel-premier-ginza 추가

잔여 결정 사항:
- 긴자 디너 가게 — 보류

## [2026-06-02] plan-update | 긴자 디너 예약 안 함 결정 — trip planning 잔여 결정 0

사용자 결정: **모든 식당 예약 안 함, 즉흥 방문**. 이유: 6세 동반으로 미슐랭급 고급 가게 회피.

영향:
- itinerary.md 6/5 18:30 슬롯 — "회·스시 메인 디너 예약" → "즉흥 방문, 가족 친화 후보(스시잔마이·이타마에 스시·노다이와·긴자 토라지 등)"
- itinerary.md 보완 필요 섹션 — 긴자 디너 결정 사항 제거, 모든 결정 ✅로 닫힘
- food-options.md 긴자 디너 섹션 — 결정 메모, 캐주얼·가족 친화 ⭐ 표시 재정렬. 미슐랭급(Onodera·큐베이) "제외 (6세)" 표기
- logistics.md D-21 체크리스트 — 긴자 디너 예약 액션 취소선 처리
- quick-reference.md 6/5 디너 — 즉흥·가족 친화 명시
- trip index.md overrides `must_avoid`에 `fine-dining-with-kid` 추가, 확정 사항에 식당 예약 안 함 명시

**잔여 결정 사항: 0건. trip planning 완료.**

남은 액션은 모두 D-21~D-1 시점의 **사용자 직접 액션**: 임산부 검진·모자수첩 영문 요약·Visit Japan Web·teamLab 예매·액세스 특급 시간표·온야도 노노 임산부 정책 메일·일기예보·정액 택시 백업 메모. logistics 체크리스트 참조.

## [2026-06-02] plan-update | 메가도쿄 영업 안 함 확정 → 6/4 분기 시스템 제거

사용자 확정: [[pokemon-center-mega-tokyo]]가 trip 시점에 영업 안 함. **분기 A/B 시스템 제거**, 6/4 오후를 **[[teamlab-planets-tokyo]] 단일 일정**으로 정착.

영향:
- itinerary.md — 14:30 메가도쿄 슬롯을 13:30 도요스 이동 + 14:00 teamLab으로 교체. "6/4 백업 분기" 섹션 제거하고 "메가도쿄 영업 안 함 확정" 간단 메모로 대체. 우천 plan B에서 메가도쿄·선샤인시티 대체안 제거
- content.md — 백업 시나리오 섹션 → 단일 시나리오 (teamLab 메인)로 재작성. "배울것" 축 한층 강화 명시
- shot-list.md — 분기 A·B 두 표 → 단일 표로 통합. 14:00~16:00 teamLab 메인 클로징 컷
- logistics.md D-7 체크리스트 — 메가도쿄 재오픈 확인 액션 제거, teamLab Planets 사전 예매 단일 액션
- budget.md — 포켓몬 메가도쿄 굿즈(8,000~15,000엔) → teamLab 입장료(약 9,100엔)
- trip index.md — overrides themes에서 `kid-pokemon` → `kid-anchor` (teamLab + 시부야 PARCO 조합)
- quick-reference.md — 6/4 동선에서 분기 표기 제거, teamLab 단일
- wiki/pois/pokemon-center-mega-tokyo.md — 본문 "임시 휴업" → "영업 안 함, 2026-06 trip 미사용" 표기
- wiki/cities/tokyo.md·루트 index.md — 동일 표기

6세 아이 후크: 6/4 오후 teamLab + 6/5 오후 시부야 PARCO 6F([[pokemon-center-shibuya|포켓몬]]·닌텐도·캡콤) 조합으로 유지.

잔여 결정 사항:
- 긴자 디너 가게 — 보류

## [2026-06-02] plan-update | 2026-06-tokyo 6/4 신주쿠 호텔 확정 + 6/4 동선 재구성

사용자 결정: **6/4 신주쿠 = [[hotel-groove-shinjuku|Hotel Groove Shinjuku, A PARKROYAL Hotel]]** (도큐 가부키쵸 타워 18~38F).

신규 wiki (4개):
- `sources/2026-06-02-general-knowledge-tokyu-kabukicho-tower.md`
- `sources/2026-06-02-general-knowledge-hotel-groove-shinjuku.md`
- `wiki/pois/tokyu-kabukicho-tower.md` (category: sight — 쿠마 켄고 설계 48층 복합)
- `wiki/pois/hotel-groove-shinjuku.md` (category: stay, status: confirmed)

6/4 동선 재구성:
- 09:00 takkyubin → 호텔 그루브 신주쿠 (가부키쵸 타워) 발송
- 17:00 (이케부쿠로/도요스에서) **택시 → 가부키쵸 타워** 약 25분
- 17:30 체크인 (18F 라운지)
- 18:00 객실 휴식
- 19:00 저녁 (A. 타워 내 푸드홀 / B. 외출, [[food-options]] 신주쿠)
- **20:30 택시 → 도청 전망대** (지하 터널 대신 가부키쵸→도청 택시 5~10분, 1,200~1,800엔) — 야간 가부키쵸 도보 임산부 부담 회피
- 21:30 택시 → 호텔 복귀
- **22:00 (선택) 타워 39F 무료 Skydeck** — 컨디션 OK시 보너스 야경 컷, 동선 0

영향 산출물 갱신:
- itinerary.md — 6/4 분기 A·B 호텔명 정정, 도청 야경 동선을 지하 터널 대신 택시로, 22:00 Skydeck 보너스 슬롯 추가
- budget.md — 신주쿠 1박 가격 갱신 (가족 약 45,000엔 추정)
- quick-reference.md — 6/4 호텔란·동선 갱신
- logistics.md — 신주쿠 비상 동선 표에 호텔 인접 NCGM 명시
- shot-list.md — 6/4 17:30 호텔 그루브 컷, 18:00 객실 야경, 19:00 타워 푸드홀(선택), 22:00 Skydeck(선택) 추가
- hotel-options.md — 신주쿠 결정 표시, 후보 참고 보관
- trip index.md — 확정 사항 섹션에 6/4 호텔 추가
- wiki/cities/tokyo.md — POI sight에 tokyu-kabukicho-tower, stay에 hotel-groove-shinjuku 추가
- 루트 index.md — POI sight·stay 섹션 갱신, Sources 4차에 항목 추가

콘텐츠 6관점 영향:
- **"배울것"** 축 강화: 6/4 저녁 슬롯에 **가부키쵸 재개발 = 도시 재생 메시지** + **쿠마 켄고 현대 일본 건축** + **수직 도시(vertical city)** 컷 응집.
- 호텔 그루브(PARKROYAL Collection) + Dormy Inn + Super Hotel 비교로 **일본 호텔 산업 다층 브랜딩** 컷.

잔여 결정 사항:
- 긴자 디너 가게 — 보류

## [2026-06-02] plan-update | 6/6 출국 이동 정정 — 정액 택시 → 액세스 특급 직통

사용자 결정: 정액 택시 → **도에이 아사쿠사선 액세스 특급 직통**으로 정정.

- 동선: Super Hotel Premier Ginza → 히가시긴자(東銀座)역 도보 5~10분 → 액세스 특급 직통 → NRT
- 비용: 가족 3인 약 4,000엔 (정액 택시 대비 약 19,000엔 절감)
- 시간: 약 80~90분
- 환승: 0
- 입·출국 모두 액세스 특급으로 일관된 가성비 패턴

영향:
- itinerary.md — 6/6 14:00 호텔 출발 → 14:10 히가시긴자역 액세스 특급 탑승 → 15:30~15:45 NRT 도착. 행선지 표시 주의 명시
- budget.md — 이동비 약 19,000엔 추가 절감. 누적 약 40,000엔 절감
- quick-reference.md — 공항 이동 표 + 6/6 동선 정정
- logistics.md §2 — 6/6 결정 정정, 행선지 표시 주의·시간표 사전 확인 명시, 정액 택시는 D-1 컨디션 백업으로 보존, D-7 체크리스트 갱신
- shot-list.md — 14:00 히가시긴자역 도보·14:10 액세스 특급 차창 컷, "배울것" (대중교통 인프라) 매핑
- trip index.md — overrides `airport_transfer_out: toei-asakusa-access-express`, 확정 사항 갱신

D-1 백업: 컨디션 변수 크면 정액 택시 전환 가능 (회사 연락처 D-7에 메모)

## [2026-06-02] plan-update | 항공편 시간 정정 + 6/6 풀데이 전환

사용자 정정: 출국·복귀 시간 모두 **탑승시간 기준**.

- 6/3 14:10 = ICN 탑승 → NRT 도착 약 17:00 (기존 가정과 일치)
- 6/6 17:30 = **NRT 탑승** (기존엔 ICN 도착으로 잘못 가정) → ICN 도착 약 20:00~20:30

**6/6 전체 일정 +5시간 여유** 발생. "오전 반일"에서 "풀데이"로 격상.

영향:
- `itinerary.md` — 6/3 도착 시간 표기 정정. 6/6 일정 재작성: 츠키지 본격 + 츠키지 산책 + **12:00 추가 슬롯**(사용자 결정 필요) + 14:15 정액 택시 출발 + 17:30 탑승 + 20:00~20:30 ICN.
- `quick-reference.md` — 비행 시간 표 갱신, 6/3·6/6 동선 갱신.
- `shot-list.md` — 6/3·6/6 시간 표기 정정, 6/6에 추가 슬롯 컷 + 츠키지 산책 컷 추가.
- `trip index.md` — frontmatter `start:`/`end:` 코멘트로 탑승 시간 명시.

**추가 슬롯 결정 (2026-06-02)**: ✅ **A. 긴자 보행자 천국 + 가벼운 쇼핑** 채택.

- 동선: 호텔→4초메 도보 10분 → 보행자 천국 산책 → 쇼핑 1~2곳(이토야·MUJI Ginza 플래그십·긴자식스·미츠코시 중) → 호텔 복귀
- 콘텐츠 6관점 "배울것" 영상 클로징 컷으로 결정적 — 긴자 룰·자율 도시 규제·보행자 도시 디자인
- 임산부 평지·벤치 풍부, 6세 잡화·서점·옥상 정원 가능

영향 산출물 갱신:
- itinerary.md — 12:00~13:30 슬롯 구체화 (쇼핑 후보 4곳 명시)
- quick-reference.md — 6/6 동선에 보행자 천국·쇼핑 추가
- shot-list.md — 보행자 천국·쇼핑 필수 컷 등록 (메인 클로징)
- trip index.md — 6/6 추가 슬롯 확정 명시

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

## [2026-06-02] ingest | 도쿄 스트릿패션 쇼핑 (BEAMS Japan 신주쿠 · Dover Street Market Ginza)

사용자 요청: 긴자·신주쿠에서 일본 스트릿패션 브랜드 쇼핑, 2026-06 트립 숙소 동선 기준.

소스:
- `sources/2026-06-02-tokyo-street-fashion-shops.md` — 공식(DSM Ginza·BEAMS) 직접 fetch 403, 웹검색(공식 발췌 + Time Out Tokyo + Good Luck Trip + 중앙구 관광청) 교차확인. 운영시간·휴무 출발 전 확인 표지.

신규 위키 페이지:
- `wiki/pois/beams-japan-shinjuku.md` (category: **activity** — shopping 전용 category 부재로 activity 사용, schema-update 후보)
- `wiki/pois/dover-street-market-ginza.md` (category: activity)
- `wiki/themes/tokyo-street-fashion.md` (scope: city, applies_to: [tokyo]) — 동네별 핵심 매장 + 2026-06 트립 동선 매핑 + 하라주쿠·시부야 본진 시드 후보 메모

부모·인접 페이지 갱신:
- `wiki/pois/shinjuku.md` — 쇼핑 에리어에 BEAMS Japan 추가, 출처 인용
- `wiki/pois/ginza.md` — 봐야할것에 DSMG 추가, 출처 인용
- `wiki/cities/tokyo.md` — POI에 "쇼핑·스트릿패션" 섹션 + 테마 섹션(tokyo-street-fashion) 추가
- 루트 `index.md` — POI 쇼핑·스트릿패션 섹션, Themes, Sources 5차 추가

동선 매핑: 6/4 신주쿠 숙소([[hotel-groove-shinjuku]]) → BEAMS Japan / 6/5 긴자 숙소([[super-hotel-premier-ginza]]) → DSMG (6/6 오전 긴자 보행자 천국·쇼핑 슬롯에 합치면 동선 최적). 임산부·6세 동반 제약상 한 동네 한 매장 압축 권장.

**schema-update 후보 (사용자 결정 대기)**: `poi.category`에 `shopping` 추가. 현재는 activity로 시드. 향후 쇼핑 POI 누적 시 district·stay처럼 정식 등록 제안.

## [2026-06-04] plan-update + ingest | 6/4 오후 동선 전면 교체 (도요스 Planets → 수상버스·도쿄타워·아자부다이)

여행 둘째날(6/4) 실시간 변경. 사용자 동선: **11:40 아사쿠사 부두 출발 → 수상버스 → 히노데 → 다이몬·도쿄타워 → 아자부다이 힐즈 → 신주쿠**.

소스:
- `sources/2026-06-04-tokyo-asakusa-cruise-tower-azabudai.md` — 수상버스(TOKYO CRUISE)·도쿄타워·아자부다이 힐즈·teamLab Borderless. 공식 + NAVITIME + 가이드 웹검색 교차확인. 시간표·예약 슬롯 당일 재확인 표지.

신규 위키 페이지(3):
- `wiki/pois/tokyo-tower.md` (sight, 메인데크 09:00-23:00·4세+ 600엔)
- `wiki/pois/azabudai-hills.md` (sight, 2023 복합·모리 JP타워 330m·헤더윅 녹지·마켓)
- `wiki/pois/teamlab-borderless-azabudai.md` (activity, 10:00-21:00·시간지정 예약·Planets와 별개)

기존 페이지 갱신:
- `wiki/pois/sumida-river.md` — 실용정보에 수상버스(隅田川ライン·히미코·호타루나) 추가, 출처 인용
- `wiki/cities/tokyo.md`·루트 `index.md` — POI 목록에 3개 추가, Sources 6차

trip 산출물 전면 갱신(6/4):
- `itinerary.md` — 6/4 표 전면 교체(수상버스 11:40·도쿄타워·아자부다이·teamLab Borderless), 도보거리 표(~3km 한계 근접), 우천 Plan B 6/4 행, 아이 후크 변천 섹션, 확정 사항. 도청 야경 슬롯 6/4에서 제외(도쿄타워+39F Skydeck로 충당)
- `quick-reference.md` — 6/4 한 눈에 동선 교체 + 당일 예약 확인 경고
- `content.md` — 6/4 콘텐츠 무게중심: "옛 도쿄타워 vs 신축 아자부다이(수직 도시)" 대비 + 수상버스
- `shot-list.md` — 6/4 컷 교체(수상버스 갑판·도쿄타워 룩다운·도쿄타워 프레이밍·아자부다이 광장·teamLab Borderless), 6관점 충당표
- `food-options.md` — 6/4 점심을 다이몬·도쿄타워·아자부다이 권역으로 교체(아자부다이 힐즈 마켓·Balcony by 6th·노다이와 본점·풋타운), 도요스 섹션 "제외(참고 보관)" 표기

아이 후크 변천: 메가도쿄(휴업) → teamLab Planets(도요스) → **수상버스 ⛴ + (선택)teamLab Borderless(아자부다이)**. ⚠️ Borderless·우주선 보트는 **당일 예약/잔여석 현장 확인** 필요.

미해결: teamLab Borderless 당일 예약 여부는 사용자 현장 확인 사항. 안 되면 도쿄타워+아자부다이 광장으로 가볍게.

## [2026-06-04] plan-update | 6/4 — teamLab Borderless 빼고 아자부다이 광장만 + 도청 야경 복귀

사용자 결정: 아자부다이 힐즈는 **광장 구경만**, 저녁에 **[[tokyo-metropolitan-government-building|도청 45F 무료 전망대]] 야경**.

영향:
- `itinerary.md` — 6/4 오후 teamLab Borderless 슬롯 제거(15:20 광장만 → 16:30 신주쿠 이동 → 17:15 체크인·휴식 → 18:30 저녁 → 20:00 도청 야경 → 21:00 복귀 → 21:30 선택 39F Skydeck). 우천 Plan B에 도청 지하터널 행 복귀, 아이 후크 변천·확정 사항 갱신
- `quick-reference.md`·`content.md`·`shot-list.md` — 6/4 도청 야경 복귀, Borderless 제거. 콘텐츠 "배울것"에 **낮 도쿄타워(유료) vs 밤 도청(무료) 전망 대비** 추가
- `wiki/pois/azabudai-hills.md` — 트립 동선 "광장만"
- `wiki/pois/teamlab-borderless-azabudai.md` — 이번 트립 미방문, 향후 후보로 보관 (POI 페이지 자체는 유지)

아이 후크: 6/4 수상버스 ⛴ + 도쿄타워 룩다운 윈도우 / 6/5 시부야 PARCO 포켓몬.
