# Figma 작업 기록 — List 컴포넌트(Category Tab / Category Menu Item) 리뷰 및 수정

**작업일** 2026-09-04
**대상 Figma 파일** HDS_2609 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**대상** `Category Tab`(`6514:37753`), `Category Menu Item`(`6527:38312`), `Quick Badge Row`(`6527:38307`), 문서 보드 `list`(`6527:38356`)

---

## 배경

Component Checklist의 "List" 항목(사용자 정의: "텍스트 기반 선택 요소 정보를 섹션 또는 그룹으로 나눌 수 있는 연속적인 수직 집합체", 예: 상품 카테고리 전체보기)이 미완료로 남아있던 상태에서, 사용자가 바텀시트에서 카테고리 버튼을 눌렀을 때 나오는 화면용으로 새 컴포넌트를 직접 추가하고 리뷰를 요청했다.

## 발견된 컴포넌트

- **`Category Tab`**(당시 이름 `Category-Tab`, `6514:37753`) — `status=Default`/`status=Selected` 2-variant. 사이드바 카테고리 목록의 개별 항목.
- **`Category Menu Item`**(당시 이름 `menu`, `6527:38312`) — 이미지(74×74, 아직 빈 placeholder) + 라벨. 바텀시트 콘텐츠 영역의 그리드 메뉴 항목. 18곳에서 실사용 중(정상 재사용).
- **`Quick Badge Row`**(당시 이름 `Quick-Badge-Row`, `6527:38307`) — 배지 5개(와우카드/로켓프레시/R.LUX/골드박스/쿠팡이츠)가 고정된 단일 COMPONENT. 실사용 0곳.

## 리뷰 결과 — 4건 지적, 전부 수정 완료

### 1. 네이밍 충돌: `menu`가 기존 하단 내비게이션의 `menu`(`6501:35862`, `button` 프레임 하위, Home/Category/Search/Mypage/Cart 탭 세트)와 이름이 완전히 동일
Assets 패널에서 서로 무관한 두 컴포넌트가 이름으로 구분되지 않는 문제. `icon`/`Sheets`/`review` 때 확립한 방식(병합하지 않고 이름만 구분)을 그대로 적용해 **`Category Menu Item`**으로 개명.

### 2. Sidebar 데모(13개 `Category Tab` 인스턴스)에 "식품"이 3번 중복
2번째(Selected)·12번째·13번째가 전부 "식품"이었음. 12번째를 **"반려동물용품"**, 13번째를 **"헬스/건강식품"**으로 교체(실제 커머스 카테고리명, 기존 13개 목록과 중복 없음). Pretendard 폰트가 이번 실행 환경에서 로드 가능함을 확인(`loadFontAsync` 성공) — 과거 세션들에서 반복됐던 폰트 블로커가 이번엔 발생하지 않아 `.characters` 직접 수정으로 처리.

### 3. `Quick Badge Row` 구조 — 5개 배지가 Variant/속성 없이 통째로 고정
카테고리 화면 흐름과 직접 관련은 없어 보이나, 재사용 가능한 구조로 바꿀지 사용자에게 확인. **결정: 구조는 그대로 유지, 이름만 통일**(재설계하지 않음).

### 4. 표기 통일: `Category-Tab`→`Category Tab`, `Quick-Badge-Row`→`Quick Badge Row`
프로젝트 기존 컴포넌트 표기(Title Case + 공백, 예: "Section Header", "Action Bar")에 맞춰 하이픈 표기를 공백으로 변경.

## 검증

- 이름 변경 후 페이지 전체에서 "menu" 이름을 가진 컴포넌트가 기존 하단 내비게이션 것 하나만 남았는지 재조회로 확인
- Sidebar 스크린샷으로 13개 항목에 중복 없이 표시되는지 확인
- `Category Tab`의 `componentPropertyDefinitions` 재조회로 이름 변경 후에도 `status` 속성 에러 없음 확인

## 부가 발견 — Navigation 체크리스트 항목 상태 정정 필요

이번 화면 리뷰 과정에서 `bottom_navigation`(`6501:36218`, `Platform=iOS`/`Platform=Android` 2-variant, 실사용 2곳)이 이미 존재함을 확인했다. Component Checklist 재구성(2026-09-03) 당시엔 없었던(또는 발견 못한) 컴포넌트로, "Navigation"(하단 내비게이션바, 앱 전반 화면 전환) 정의에 정확히 부합 — **완료 기준("만들어져 있으면 완료")상 ✅로 정정 필요**. `design-system/status.md`에 반영함. Figma 상의 체크리스트 보드(`6144:769`)는 아직 갱신 안 함 — 필요시 별도 요청.

## 다음 단계 후보

- List 자체(개별 항목을 담는 "List" 컨테이너/래퍼)는 아직 컴포넌트가 아니라 raw Sidebar 프레임 — `Category Tab` 인스턴스를 담는 재사용 가능한 List 컨테이너를 만들지는 사용자 판단
- `Category Menu Item`의 `img > Thumbnail` 자리는 아직 빈 placeholder — 아래에서 완료된 `thumbnail` 컴포넌트를 실제로 연결할지는 사용자 판단
- `Quick Badge Row` 재사용 가능 구조화는 필요해지면 별도 요청
- Checkbox 컴포넌트는 여전히 없음

## 업데이트 — Thumbnail 완료 확인 + Radio 이름 중복 버그 수정 (2026-09-04)

사용자가 "썸네일/라디오,체크박스/리스트/바텀네비게이션 추가됐다"고 보고해 재조회한 결과:

- **`thumbnail`**(`6502:36818`, `button` 보드 프레임 하위) — `Size=small/medium/large/Xlarge` 4-variant, 실사용 10곳. 정상 완료로 확인. 단 variant 값 표기가 `Xlarge`만 대문자 X라 `small/medium/large`와 대소문자 불일치 — 표기 통일 시점에 정리 필요.
- **`radio`**(`6474:33379`) — 여전히 두 variant가 "Property 1=Default"로 이름이 겹쳐 `componentPropertyDefinitions`가 에러 상태였다. 스크린샷으로 실제 내용을 먼저 확인(이 프로젝트에서 반복 강조된 "이름만 보고 병합/판단하지 않는다" 원칙 적용) — 회색 `Frame 1430106640`(`6474:33377`)은 진짜 Default, 파란 `Primary/500` 바인딩된 쪽(`6474:33378`)은 진짜 Selected였다(둘 다 처음부터 정상 콘텐츠, 이름만 안 고쳐져 있었음). `6474:33378`을 `Property 1=Selected`로 개명해 에러 해소. 실사용은 여전히 0곳.
- **Checkbox**는 여전히 파일 어디에도 없음(전체 페이지 검색 완료).
- **List 컨테이너**도 여전히 없음(개별 아이템 컴포넌트만 존재).

**결론**: Thumbnail만 실제로 새로 완료됨. Radio/Checkbox는 radio 자체는 정리됐지만 Checkbox 부재로 항목 전체는 미완료 유지. 체크리스트 12/15 → **13/15**로 갱신, Figma 체크리스트 보드(`6144:769`)도 Navigation·Thumbnail 체크 반영 + Radio/Checkbox 설명 텍스트 갱신.

## 변경 이력

| 일자 | 내용 |
|---|---|
| 2026-09-04 | 최초 작성. 신규 컴포넌트 3종 리뷰, 네이밍 충돌·콘텐츠 중복·표기 통일 4건 수정, Navigation 체크리스트 상태 정정 발견 |
| 2026-09-04 | 사용자 진행 보고 재조회 — Thumbnail 완료 확인, radio 이름 중복 버그 수정(콘텐츠는 원래부터 정상이었음), Checkbox·List 컨테이너는 여전히 부재. 체크리스트 13/15, Figma 보드 동기화 |
