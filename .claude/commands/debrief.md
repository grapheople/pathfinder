---
description: 여행 중·후 회고와 콘텐츠 환류 (CLAUDE.md 10.4 Debrief & Content)
argument-hint: [trip 슬러그 또는 현지 메모 · 예: "2026-09-kyoto"]
---

# Debrief & 콘텐츠 제작

`CLAUDE.md`의 워크플로우 10.4(Debrief & 콘텐츠 제작)를 그대로 수행한다. 이 문서가 규약과 어긋나면 CLAUDE.md가 우선이다.

## 사용자 입력

$ARGUMENTS

trip 슬러그가 들어왔으면 그 trip을 대상으로. 메모만 들어왔으면 어느 trip 소속인지 짧게 묻거나 가장 최근 `trips/`를 추정해 확인.

## 절차

### 1. 대상 trip 식별 + 컨텍스트 로드
- `trips/<슬러그>/index.md` 읽기 — destination, 날짜, overrides 확인.
- `trips/<슬러그>/itinerary.md`, `content.md`도 읽어 사전준비에 무엇이 있었는지 확인.

### 2. `content.md` 현지경험 3섹션 채움 — **필수**
사용자의 발화·사진·메모를 토대로 Claude가 초안을 만들고 사용자 확인·수정:

- **발견한것** — 예상하지 못했지만 우연히 마주친 것
- **느낀것** — 몸과 감정이 먼저 반응한 것
- **생각해볼것** — 의미·구조로 해석한 것

빈 섹션을 그대로 두지 않는다. 정보가 부족하면 사용자에게 짧게 추가 질문.

### 3. 위키 환류
새로 확인된 사실·인상을 위키로 되돌린다:

- **운영시간·가격 등 사실 갱신** → 해당 `wiki/pois/<slug>.md`의 frontmatter·실용 정보. `status: visited` 설정.
- **"이 도시를 잘 드러낸다" 같은 발견** → `wiki/cities/<city>.md`의 봐야할것 섹션 보강. 출처는 `[[trips/<슬러그>/content]]`로 인용.
- **새 POI 발견** → 사용자에게 신설 여부 확인 후 `wiki/pois/`에 페이지 생성.
- **갱신된 페이지의 `updated:`** 모두 오늘로.

### 4. `trips/<슬러그>/debrief.md` 작성
- frontmatter: `type: trip` 보조 페이지로 가볍게(또는 별도 type 안 둠).
- 본문:
  - `## 좋았던 것`
  - `## 아쉬웠던 것`
  - `## 다음 여행에 적용할 점`
  - `## profile.md 갱신 후보` (있을 때만)

### 5. 일회성 vs 반복 패턴 판단 (섹션 8.4 규칙)
- **일회성 학습**(이 여행만의 우연·특이 상황) → debrief에만 남기고 `profile.md`는 건드리지 않는다.
- **여러 trip에서 반복 확인된 패턴**으로 보이는 학습 → debrief의 "profile.md 갱신 후보"에 명시하고 사용자에게:
  > "최근 N회 trip에서 X가 반복됐어요. profile.md를 다음과 같이 갱신할까요?"
  
  동의 시에만 profile 수정 + `updated:` 갱신 + `log.md`에 `profile-update` 엔트리.

### 6. 콘텐츠 제작 단계 진행 (선택)
사용자가 영상 제작에 들어갔다면:
- `content.md`의 `status:`를 적절히 진행: `planning → shooting → editing → published`.
- 필요하면 `trips/<슬러그>/script.md` 등 산출물을 같은 디렉토리에.
- 발행 시 `content.md`의 `video_url:` 채움.

### 7. `log.md` 추가
단계별로 엔트리(보통 1~2개):
```
## [YYYY-MM-DD] debrief | <슬러그>
- content.md 현지경험 3섹션 채움
- 영향: <환류된 wiki 페이지 목록>
- profile-update 후보: <있으면>

## [YYYY-MM-DD] content | <슬러그> · status: editing
- script.md 작성
```

## 정상 종료 조건

- `content.md`의 현지경험 3섹션이 빈 칸 없이 채워짐
- 위키 환류가 끝남(갱신된 POI·도시 페이지)
- `debrief.md` 작성
- profile 갱신 후보가 있었으면 사용자 결정 + log 반영
- (콘텐츠 단계 진입 시) `content.md` status 갱신

## 마지막 보고

한 줄 요약 + 변경 파일 목록. 예:
> 2026-09-kyoto debrief 완료. 현지경험 3섹션 채움. 환류: `wiki/cities/kyoto.md`(봐야할것 보강), `wiki/pois/eikando.md`(운영시간 갱신·visited). profile 갱신 후보 1건 — 사용자 결정 대기.
