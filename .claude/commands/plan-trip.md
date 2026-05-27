---
description: 여행 계획을 위키 워크플로우대로 산출 (CLAUDE.md 10.3 Plan)
argument-hint: [목적지 기간 동행자 등 자유 서술, 비워두면 묻습니다]
---

# Plan — 여행 계획 수립

`CLAUDE.md`의 워크플로우 10.3(Plan)을 그대로 수행한다. 이 문서가 규약과 어긋나면 CLAUDE.md가 우선이다.

## 사용자 입력

$ARGUMENTS

위 입력이 비어 있거나 정보가 부족하면 절차 2에서 짧게 묻는다. 이미 충분하면 추가 질의 없이 4단계로 직행한다.

## 절차

### 1. 글로벌 기준선 읽기
`profile.md`를 먼저 읽는다.
- 파일이 없으면 사용자에게 알린다: "profile.md가 비어 있어요. 지금 짧게 기준선을 잡고 시작할까요, 아니면 이번 여행만의 컨텍스트로만 진행할까요?" 후자라면 머지 단계에서 profile 쪽을 빈 셋으로 둔다.
- profile에 이미 있는 항목(체력 한계, 평소 페이스, 식이 제약 등)은 **다시 묻지 않는다**.

### 2. 이번 여행 한정 컨텍스트 파악
입력으로 채워지지 않은 다음 항목만 짧게 질의(여러 개를 한 번에 묻지 말고, 한두 묶음으로 끊는다):
- 목적지(도시/지역)
- 기간(시작·종료 날짜 또는 N박)
- 동행자
- 이번 여행의 테마·목적
- 이번만의 예산(특별한 경우만)
- 일시적 제약(부상, 동행자 컨디션, 시즌 등)

### 3. trip 슬러그 결정
`YYYY-MM-<destination>` (kebab-case). 같은 슬러그가 이미 있으면 `-2`, `-3` 접미.

### 4. trip `index.md`를 가장 먼저 생성
경로: `trips/<슬러그>/index.md`
- frontmatter: 공통 필드(`type: trip`, `title`, `aliases`, `tags`, `created`, `updated`) + `destination`, `start`, `end`, `travelers`, `budget_total`, `status: planning`.
- **`overrides:` 블록**: 이번 여행 한정 선호도(예: `pace`, `daily_walk_km_max`, `themes`, `must_avoid`, 동행자 페이스 등). overrides로 표현하기 어려운 미묘한 톤은 본문에 한두 문단으로.

### 5. 머지된 선호도 산출
`merged = profile ⊕ overrides` (같은 키는 overrides 우선). 이후 모든 결정은 `merged`를 기준으로 한다. `merged`를 본문이나 frontmatter에 저장하지는 않는다 — 항상 계산해서 사용.

### 6. 목적지 위키 읽기 (필요 시 Ingest 분기)
- `wiki/cities/<city>.md`, 관련 `wiki/pois/`, 관련 `wiki/themes/`를 읽는다.
- 정보 공백이 크면 사용자에게 알리고 옵션 제시:
  - URL 던져주면 **Ingest 워크플로우(10.1)로 일시 분기** 후 돌아옴
  - 일반 지식으로 임시 보강(이 경우 "위키 외부 지식" 표지 + content/itinerary 본문에 명시)

### 7. 나머지 trip 산출물 작성
같은 디렉토리(`trips/<슬러그>/`)에:

- **`itinerary.md`** (`type: itinerary`)
  - frontmatter: `trip: <슬러그>`, `content: "[[trips/<슬러그>/content]]"`
  - 본문: 날짜별·시간 단위 일정. 같은 도시 안 동선 군집화. 이동·식사·휴식 버퍼 명시. **`merged`의 페이스·도보 한계·테마·회피 사항을 반영**.
  - 일정에 POI를 박을 때는 반드시 `[[<poi-slug>]]` 위키링크. 위키에 없는 POI는 사용자에게 신설 여부 확인 후 `wiki/pois/`에 페이지 생성.

- **`content.md`** (`type: content`) — **필수**
  - frontmatter: `trip: <슬러그>`, `status: planning`.
  - 본문 **사전준비 3섹션을 빈 칸 없이 채움**:
    - 봐야할것 — 여행지를 가장 잘 드러내는 풍경·공간
    - 먹어야할것 — 기후·문화·생활이 녹은 음식(왜 발전했는지까지)
    - 배울것 — 사회가 문제를 푸는 방식·가치관
  - 도시·POI 페이지의 같은 이름 섹션에서 인용/요약. 출처는 `[[...]]`로 표시.
  - 현지경험 3섹션(발견/느낀/생각해볼)은 비워둔 채 헤더만 둠(여행 중·후에 채움).

- **`packing-list.md`**, **`budget.md`** — 개요만. 시즌·동행자·예산에 맞춰.

### 8. 상호 링크
- `itinerary.md` ↔ `content.md`를 본문에서도 `[[...]]`로 연결.
- 일정에 등장하는 모든 POI는 `[[...]]` 위키링크.

### 9. 루트 `index.md` 갱신
Trips 섹션에 `- [[trips/<슬러그>/index]] · planning · <start> ~ <end>` 추가.

### 10. `log.md` 추가
```
## [YYYY-MM-DD] plan | <슬러그>
- trips/<슬러그>/{index,itinerary,content,packing-list,budget}.md 생성
- content.md 사전준비 3섹션: 채움
- 영향: <wiki 변경이 있었다면 목록>
```

## 정상 종료 조건

다음이 모두 충족돼야 종료:
1. trip `index.md`에 `overrides:` 기록됨
2. `itinerary.md`에 시간 단위 일정이 채워짐
3. `content.md`의 **사전준비 3섹션(봐야할것/먹어야할것/배울것)이 빈 칸 없이** 채워짐

③이 비어 있으면 사용자에게 "사전준비가 비어 있어 미완"임을 알리고, 어떤 정보가 막혔는지(자료 부족? 도시 위키 부족?) 진단해 해결한 뒤 마무리한다.

## 마지막 보고

한 줄 요약 + 생성된 파일 목록을 보고. 예:
> 2026-09-kyoto 계획 초안 완료. 생성: `trips/2026-09-kyoto/{index,itinerary,content,packing-list,budget}.md`. content 사전준비 3섹션 채움. 다음 단계: 출발 전 `/plan-trip` 다시 호출해 보완하거나, 현지에서 `content.md`의 현지경험 섹션을 채워주세요.
