---
type: trip
title: 2026-06 도쿄
aliases: [Tokyo, 東京, 도쿄여행]
tags: [japan, tokyo, family, with-pregnancy, with-kid]
created: 2026-05-27
updated: 2026-05-27
destination: tokyo
start: 2026-06-03                       # ICN 탑승 14:10
end:   2026-06-06                       # NRT 탑승 17:30, ICN 도착 약 20:00~20:30
travelers: [me, wife-21w, son-6]
budget_total:                          # 추후 budget.md에서 산정 후 채움
status: planning
# 이번 여행 한정 선호도 — profile.md의 baseline을 덮어쓰거나 추가 (CLAUDE.md 8.3)
overrides:
  pace: relaxed                        # 임산부·6세 동반으로 baseline(balanced)에서 한 단계 낮춤
  start_time: standard                 # baseline 유지
  daily_walk_km_max: 3                 # baseline 5 → 3 (임산부 + 6세)
  themes: [food-local, kid-anchor, design-architecture-systems]  # kid-anchor = teamLab(6/4) + 시부야 PARCO 포켓몬·닌텐도·캡콤(6/5)
  must_avoid:
    - tight-schedule
    - group-tour
    - long-queues                      # 임산부 장시간 서기
    - hot-onsen                        # 임산부 노천·뜨거운 온욕
    - smoking-restaurants              # 일본은 흡연 가능 식당 여전히 있음
    - many-stairs                      # 계단 많은 명소
    - fine-dining-with-kid             # 6세 동반으로 미슐랭급 고급 식당 회피
  must_include:
    - frequent-rest-stops              # 카페·벤치 자주
    - taxi-friendly-routes
    - same-neighborhood-clustering     # 한 동네 안에서 일정 군집화
    - kid-attention-anchor             # 매일 6세가 기대할 만한 후크 1개
  transport_pattern: taxi-takkyubin    # 렌터카 X, 택시 + 짐 직배송 + 도보+전철
  hotel_rotation: [asakusa, shinjuku, ginza]  # 매일 호텔 이동
  airport_transfer_in: keisei-access-express  # 6/3 NRT→아사쿠사역 직통, 환승 0
  airport_transfer_out: toei-asakusa-access-express  # 6/6 히가시긴자→NRT 직통 (2026-06-02 정정), 환승 0
---

## 이번 여행의 동기·맥락

가족 휴식과 콘텐츠 촬영의 균형. 아내가 임신 21주차로 비교적 안정기지만 페이스·동선 무리는 금물.
6살 아들이 즐길 수 있는 후크(포켓몬)를 매일 한 개씩 박아 아이의 컨디션을 일정의 축으로 잡는다.

호텔을 매일 다른 동네(아사쿠사 → 신주쿠 → 긴자)로 옮기는 변형이라
캐리어는 `takkyubin`(야마토 운수) 호텔 간 직배송으로 처리하고, 본인은 손가방만 들고 가볍게 이동한다.

콘텐츠 6관점 중 "배울것" 축은 **도쿄의 공공 공간·대중교통·세심한 서비스 디자인** —
임산부·아동 동반이라는 컨텍스트가 오히려 이 관점을 또렷하게 만들어준다(유아차·임산부 배지·역 엘리베이터·자판기·편의점의 동선 설계).

## 확정 사항 (planning 진행 중 확정된 결정)

- **6/3 숙소**: [[onyado-nono-asakusa|天然温泉 凌雲の湯 御宿 野乃 浅草 (본관)]] (Dormy Inn 그룹 다다미·천연 온천)
  - ⚠️ 임산부 노천 온천 정책 D-7 메일 확인 액션
- **6/3 입국 이동**: 케이세이 본선 액세스 특급 직통 (NRT → 아사쿠사역, 환승 0, 가족 약 3,500엔, 약 60~75분)
- **6/4 숙소**: [[hotel-groove-shinjuku|Hotel Groove Shinjuku, A PARKROYAL Hotel]] (도큐 가부키쵸 타워 18~38F, 가부키쵸 재개발 신축 + 같은 빌딩 내 영화관·푸드홀·39F 무료 Skydeck)
- **6/5 숙소**: [[super-hotel-premier-ginza|Super Hotel Premier Ginza]] (가성비 + 츠키지 도보권 + 히가시긴자역 도보권)
  - 무료 조식·대욕장(임산부는 객실 욕조)
- **6/6 출국 이동**: **도에이 아사쿠사선 액세스 특급 직통** (히가시긴자역 → NRT, 환승 0, 가족 약 4,000엔, 약 80~90분). 임산부 우대석 활용. 입·출국 모두 액세스 특급으로 일관된 가성비 패턴.
- **6/6 추가 슬롯**: 츠키지 본격(09:15~11:00) + 긴자 보행자 천국·쇼핑(12:00~13:30, 이토야·MUJI·긴자식스 중 1~2곳). 콘텐츠 6관점 "배울것" 클로징 컷.
- **모든 식당: 예약 안 함** — 6세 동반으로 고급 가게 회피, [[food-options]]에서 가족 친화 후보로 즉흥 방문 (회·스시 캐주얼: 스시잔마이·이타마에 스시 / 우나기: 노다이와·이즈에이 / 야끼니쿠: 긴자 토라지 등).

## 핵심 제약 요약

- **이동**: 6/3 입국은 케이세이 본선 / 6/6 출국은 정액 택시(잠정) / 도심 짧은 이동은 택시 / 도심 관광은 도보 + 전철 보조
- **짐**: 매일 아침 다음 호텔로 `takkyubin` 직배송 (1캐리어 약 1,500~2,500엔, 익일 도착)
- **식사**: 회·생선 OK (아내가 회 좋아함). 흡연 가능 식당·길거리 푸드코트 등 위생 우려 동선 회피
- **온천**: 임산부 노천·뜨거운 온욕 ✗. 객실 욕조 정도만
- **카시트**: 일본 택시는 영업용으로 카시트 의무 면제 → 6세 아들 그대로 탑승 가능

## 산출물

- **현지 휴대용 요약**: [[trips/2026-06-tokyo/quick-reference|quick-reference]] ⭐
- 일정: [[trips/2026-06-tokyo/itinerary|itinerary]]
- 콘텐츠 설계: [[trips/2026-06-tokyo/content|content]]
- 촬영 컷 리스트: [[trips/2026-06-tokyo/shot-list|shot-list]]
- 식당 후보: [[trips/2026-06-tokyo/food-options|food-options]]
- 호텔 후보: [[trips/2026-06-tokyo/hotel-options|hotel-options]]
- 출발 전 액션·돌발 대응: [[trips/2026-06-tokyo/logistics|logistics]]
- 짐 챙기기: [[trips/2026-06-tokyo/packing-list|packing-list]]
- 예산: [[trips/2026-06-tokyo/budget|budget]]
