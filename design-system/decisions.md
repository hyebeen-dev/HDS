# Decision Log

> **문서 성격**: 짧은 의사결정·버그수정·스코프 결정을 날짜순으로 **누적만 하는** 로그다. 기존 항목은 수정하지 않고, 새 결정이 생기면 맨 아래에 추가한다. 각 항목은 1~3줄 요약 + 근거가 된 `diagnosis/` 원본 파일 링크로 구성한다. 전체 경위·실행 과정이 궁금하면 링크된 원본을 확인한다.

---

### 2026-09-01 — Primitive 색상 "이중 네이밍"은 실제로는 레거시 Style이었다
`Color/Primitive/*`와 `color/light/*`가 별개로 중복 존재하는 것처럼 보였으나, 후자는 Variables 도입 이전에 쓰던 레거시 Paint Style이었다. 대응하는 Variable로 전량 재바인딩 후 레거시 Style 77개 전부 삭제. → `diagnosis/color-foundation-history.md`

### 2026-09-01 — Item Card의 Boolean성 속성을 "Y/N" 문자열 대신 실제 Boolean/Variant로 재설계
Price Block/Spec Row/Rating Display를 독립 컴포넌트로 분리하고 `isExposedInstance`로 상위에 노출하는 방식을 채택. `componentPropertyReferences`는 `visible`/`characters`/`mainComponent` 세 필드만 지원해 임의 하위 속성 별칭 연결은 불가능함을 확인. → `diagnosis/item-card-family-history.md`

### 2026-09-01 — Price Block 가격 색상 버그: Boolean으로는 fill을 조건부로 만들 수 없음
Figma의 Boolean 컴포넌트 속성은 `visible`만 제어 가능하고 색상(fill)은 바인딩 불가. Price Block을 `Discount=Applied/None` 2-Variant로 전환해 해결. → `diagnosis/item-card-family-history.md`

### 2026-09-01 — Spacing 예외값은 "2px 단위"로 허용
8-point/4배수 원칙을 기본으로 하되, 컴포넌트 특성상 불가피한 경우(Button 상하 padding 11 등) 2px 단위 예외를 명시적으로 허용하기로 확정. 티셔츠 사이즈 네이밍(xxxs~xxxl)은 도입하지 않고 숫자 이름 유지. → `diagnosis/spacing-foundation-history.md`

### 2026-09-01 — Grid는 모바일 단일 브레이크포인트만 정의
PRD 범위(Mobile-first)에 맞춰 Wanted 참고 자료의 멀티 브레이크포인트 체계를 따르지 않고 360/16/8/4컬럼 하나만 정의. → `diagnosis/grid-foundation.md`

### 2026-09-01 — Success Primitive 컬러 라벨 버그: Variable은 정상, hex 텍스트 라벨만 복사 오류
50 단계의 라벨이 나머지 9단계에 그대로 복사되어 있던 것으로 확인. 각 스와치의 실제 바인딩된 색상값을 다시 읽어 hex 라벨 9개 재작성. → `diagnosis/color-foundation-history.md`

### 2026-09-01 — Button Text 타입의 트레일링 아이콘은 항상 텍스트 뒤에만 배치
실제 쿠팡 화면 3개 사례(샤워메이트, 할인받기, 절약 금액 기준)가 전부 트레일링 패턴이라 리딩 아이콘은 만들지 않음. → `diagnosis/button-history.md`

### 2026-09-01 — button_medium 아이콘 Boolean 기본값은 true로 유지
다른 버튼(Show Icon 기본 false)과 달리, 사용자가 이미 두 아이콘 자리를 항상 보이게 만들어둔 상태를 그대로 보존하기 위해 기본값을 true로 설정. Leading/Trailing을 독립 속성으로 분리(4가지 조합 모두 표현 가능하도록). → `diagnosis/button-history.md`(사이즈 미반영 버그 수정 포함)

### 2026-09-01 — 구조적 이름 중복 정리: "진짜 병합 대상" vs "이름만 같은 다른 것"을 구분
Button("Buttopns") 세트 병합은 즉시 처리, Sheet(`Confirmation`/`Selector`)·Review(`Summary`/`Card`)처럼 실제로 다른 컴포넌트는 병합하지 않고 이름만 구분. 표기 통일(대소문자 등)과 페이지 전체 배치는 체크리스트 완료 후 일괄 처리하기로 시점을 미룸. → `diagnosis/component-naming-cleanup.md`

### 2026-09-01 — Action Bar는 Wanted의 "Action Area" 하위 버튼 체계를 새로 만들지 않고 기존 Button 재사용
PRD DG-3(Product List/Detail 컴포넌트 공유 원칙)에 따라 범위를 좁힘. Wanted의 "Extra" 영역(가격 요약 등)은 이번 범위에서 제외. → `diagnosis/action-bar-component.md`

### 2026-09-02 — Review Card는 Type(Photo/Only Text) × Status(한달사용/재구매)로 분리
헤더의 리뷰 태그 배지가 원래 Type에 종속된 고정 예시였으나, 실제로는 Type과 무관하게 독립 조합돼야 하는 값이라 별도 Status 속성으로 분리. → `diagnosis/review-card-naming-status.md`

### 2026-09-02 — Section Header는 App Bar에서 Boolean(Show Action)으로 분리
`type=section`/`type=section-action`이 액션 버튼 유무만 다르고 구조는 동일해 Variant 대신 Boolean이 적절하다고 판단. App Bar를 12→10 variant로 정리. → 세션 기록(플랜 파일 참고, 대응 diagnosis 미작성)

### 2026-09-03 — 텍스트 스타일 15px 계열 이름은 "small-plus"가 아니라 "compact"
`small`(14)과 `medium`(16) 사이에 끼는 15px 티어의 명칭을 사용자가 직접 "compact"로 확정. → `diagnosis/typography-foundation-history.md`

### 2026-09-03 — 17px 텍스트는 정식 토큰화하지 않고 16px로 흡수
사용 빈도가 낮고(4건) 16px와 시각 차이가 미미해 별도 토큰을 만들지 않기로 결정. → `diagnosis/typography-foundation-history.md`

### 2026-09-03 — 텍스트 스타일 카테고리를 body/heading/detail 3개에서 display/heading/body/label/navigation/underline 6개로 재편
하나의 시각 스펙이 여러 역할(예: 14px Regular가 Chip=label, Tab=navigation, Toast=body)로 쓰이는 경우, 통합하지 않고 카테고리별로 완전히 분리된 의미적 토큰을 만들기로 결정 — 신규 생성 스타일 수는 늘지만 각 역할을 독립적으로 발전시킬 수 있음. Display 카테고리는 실사용이 없어 빈 채로 예약만 함(임의 값 발명 금지). → `diagnosis/typography-foundation-history.md`

### 2026-09-03 — 문서 구조 재정리: 버전 파일 누적 방식을 중단
`diagnosis/NN-제목-v0.1.md` 형식으로 매번 새 파일을 만드는 대신, 계속 갱신되는 현황 문서(`status.md`)·관찰 문서(`design-language.md`)·이 로그(`decisions.md`)를 신설. 기존 `diagnosis/` 30개 파일은 삭제·병합 없이 이력 아카이브로 보존. 앞으로 diagnosis에 새 파일을 추가할 때도 버전 접미사는 붙이지 않는다. → 플랜 파일(`~/.claude/plans/hazy-scribbling-creek.md`) 참고

### 2026-09-03 — diagnosis/ 30개 중 진짜 중복 있는 22개만 6개로 병합, 나머지 7개는 유지
같은 대상을 여러 번 다시 다룬 파일(Color 4개, Typography 5개, Spacing 2개, Item Card 5개, Button 4개, 프로젝트 감사 2개)만 원본 그대로 이어붙이는 방식으로 병합. 서로 겹치는 내용이 없는 7개(Grid, 체크리스트/테스트조립, 구조적 네이밍 정리, Action Bar, Info Row, Review Card)는 억지로 합치면 탐색성만 나빠진다고 판단해 그대로 유지. 결과: 30개 → 13개. → `diagnosis/color-foundation-history.md`, `diagnosis/typography-foundation-history.md`, `diagnosis/spacing-foundation-history.md`, `diagnosis/item-card-family-history.md`, `diagnosis/button-history.md`, `diagnosis/project-audit-history.md`

### 2026-09-03 — Option Chips 중복 variant는 "3개 남음" 재고부족 콘텐츠라 `status=low_stock`으로 명명
`size` 세트의 중복된 `status=Default` 2개를 스크린샷으로 확인한 결과 하나는 재고부족 안내("3개 남음")가 붙은 실사용 variant였다. 기존 `sold_out`과 같은 소문자 스네이크 케이스로 `low_stock`으로 명명 — `status` 축이 Default/low_stock/sold_out 3개로 정리됨. → `diagnosis/chip-components.md`

### 2026-09-03 — Figma 렌더링 규칙 발견: 부모 프레임 자신의 stroke는 항상 자식보다 위에 그려진다
Boolean으로 파란 테두리를 켜는 오버레이 자식을 추가했는데도 안 보이는 문제의 원인 — 부모가 자기 자신의 stroke를 갖고 있으면 자식 순서와 무관하게 그 stroke가 항상 최상단에 렌더링돼 자식의 테두리를 가린다. 해결: 부모 stroke를 제거하고 기본 테두리도 별도 자식 프레임으로 옮겨 z-order를 정상화. 앞으로 "Boolean으로 테두리 색 전환" 패턴을 쓸 때는 부모 자체의 stroke를 반드시 제거하고 모든 테두리를 자식 레이어로 옮겨야 한다. → `diagnosis/chip-components.md`

### 2026-09-03 — 실수: "같은 이름/같은 타입" 중복 variant를 삭제 전 실제 콘텐츠까지 확인해야 한다 (재발)
Button 통합 중 `Variant4`를 `line_type`과 "같은 타입의 중복"으로 판단해 병합 후 삭제했는데, 실제로는 마스터 자체가 다른 텍스트("장바구니 담기")를 갖고 있었다 — Option Chips 때 이미 겪었던 것과 같은 유형의 실수를 반복함. `swapComponent()`는 인스턴스에 실제 오버라이드가 있을 때만 보존되고, 마스터 자체의 콘텐츠 차이는 보존하지 못한다. 삭제 직후 인스턴스 텍스트를 재확인해 발견, detachInstance() 후 실제 텍스트를 다른 곳에서 clone해 복구(단 폰트 크기는 원래대로 복구 못함 — 관련 인스턴스 2곳이 컴포넌트 링크가 끊긴 상태로 남음). **앞으로 원칙**: 이름이 같거나 시각적으로 비슷해 보여도, 병합·삭제 전에 반드시 실제 텍스트/콘텐츠까지 스크린샷이나 `.characters` 조회로 확인한다. → `diagnosis/button-history.md`

### 2026-09-03 — Component Checklist를 Tier 1/2/3 체계에서 사용자 확정 플랫 15개 항목으로 전면 교체
기존 PRD 기반 Tier 1/2/3(18개 항목) 체크리스트를 폐기하고, badge/banner/button/chip/dialog/heading/message box/item card/label/list/navigation/bottom sheet/tab/thumbnail/radio·checkbox 15개 항목으로 교체. 완료 기준은 "컴포넌트가 만들어져 있으면 완료"(실사용 여부 무관)로 확정. list=범용 수직 리스트 패턴, navigation=하단 내비게이션바, thumbnail=범용 이미지 썸네일(별도 컴포넌트)로 각각 뜻을 확인, 셋 다 미착수로 판정. radio는 Selected 없이 이름 중복(×2)된 미선택 상태만 존재하고 checkbox 자체가 없어 미완료로 판정 — 11/15 완료. → `diagnosis/component-checklist.md`

### 2026-09-04 — List 후보 컴포넌트의 네이밍 충돌은 병합하지 않고 이름만 구분(icon/Sheets/review 때 확립한 원칙 재적용)
사용자가 만든 `menu`(카테고리 그리드 메뉴 항목)가 기존 하단 내비게이션의 `menu`(Home/Category/Search/Mypage/Cart 탭 세트)와 이름이 완전히 겹쳤다. 서로 무관한 컴포넌트라 병합 대상이 아니고, `Category Menu Item`으로 개명해 구분. 같은 작업에서 `Category-Tab`/`Quick-Badge-Row`도 프로젝트 기존 Title Case+공백 표기(예: "Section Header")에 맞춰 `Category Tab`/`Quick Badge Row`로 통일. `Quick Badge Row`의 5개 배지 고정 구조는 재설계하지 않고 유지하기로 확정(실사용 0곳, 현재 카테고리 화면 흐름과 무관). → `diagnosis/list-component.md`

### 2026-09-04 — Navigation 체크리스트 항목 정정: 기존에 이미 존재하던 컴포넌트를 발견
전일 체크리스트 작성 시 미완료(❌)로 판정했던 "Navigation"이, List 컴포넌트 리뷰 과정에서 `bottom_navigation`(Platform=iOS/Android, 실사용 2곳)이 이미 존재함을 확인해 ✅로 정정. 체크리스트 완료 기준 자체는 바뀌지 않음 — 조사 시점에 놓쳤던 컴포넌트를 뒤늦게 발견한 것. → `diagnosis/list-component.md`, `design-system/status.md`

### 2026-09-04 — "라디오/체크박스 추가됐다"는 사용자 보고와 실제 Figma 상태가 달라 재조회 후 정확히 구분
사용자가 Thumbnail/Radio·Checkbox/List/Navigation이 추가됐다고 보고했으나, 재조회 결과 실제로는 Thumbnail만 신규 완료(4-size variant, 실사용 10곳)였다. `radio`는 두 variant가 여전히 "Property 1=Default"로 이름이 겹쳐 `componentPropertyDefinitions` 에러 상태였는데, 스크린샷으로 확인하니 실제로는 항상 진짜 Default(회색)/Selected(파랑, `Primary/500` 바인딩 정상)였다 — 콘텐츠 확인 없이 이름만 보고 실수로 착각할 뻔한 걸 스크린샷 확인 원칙대로 방지, `Property 1=Selected`로 이름만 수정해 에러 해소. Checkbox 컴포넌트 자체와 List 컨테이너(개별 아이템 컴포넌트는 있지만 감싸는 List는 없음)는 실제로 없어 두 항목 모두 미완료 유지 — 체크리스트 13/15. **사용자의 진행 보고도 실제 Figma 상태 재조회 없이 그대로 반영하지 않는다**는 원칙 재확인. → `design-system/status.md`

### 2026-09-04 — 전체 Design System 감사(audit) 실시, 3개 하위 페이지의 컴포넌트 라이브러리 전체 중복을 발견해 삭제
Foundation/컴포넌트 네이밍·계층/Variant·Property/시각 일관성을 3갈래로 나눠 read-only 감사(사용자 요청, 5점 만점 2.5점 평가). 가장 큰 발견: `ㄴ Button, Chips`·`ㄴ Dialog, Sheets, Toast`·`ㄴ Item Card, Review Card` 3개 페이지가 `① Component`의 진짜 컴포넌트를 인스턴스가 아니라 COMPONENT_SET 자체를 복제해 담고 있었음(19개 패밀리, 실사용 전부 0). 사용자에게 처리 방식(삭제/Instance로 재구성/페이지 삭제)을 확인한 결과 "사본 삭제"로 확정 — 삭제 전 페이지별 실사용 재검증 + `① Component` 크로스 레퍼런스 확인(모두 0)까지 마친 뒤 19개 COMPONENT_SET 전부 삭제. `① Component`의 진짜 컴포넌트는 영향 없음(Button-large 28 variant, chip 10 variant 등 확인). 그 외 발견 사항(깨진 `label`/`dialog`/`Icon Button`/`status-badge`, Property 이름 5종 혼용, Button Small/XSmall 스펙 미반영 등)은 P0~P3 Action Plan으로 정리. → `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — 감사에서 발견한 P0(깨진 컴포넌트 4개) 전부 해결 — 매번 스크린샷으로 실제 콘텐츠 확인 후 처리
`label`(4개 variant 전부 "Property 1=Default")은 전부 진짜 다른 콘텐츠("특가 진행중"/"무료반품"/"무료배송"/"4회 구매")였음 — 이미 실사용 인스턴스가 따르던 "Type=Shipping/Return" 관례를 그대로 확장해 `Type=Promotion/Return/Shipping/RepeatPurchase`로 개명. `Icon Button`(6개 variant 동일 이름)은 x/y 위치·배경색으로 재구성한 결과 진짜 원본(Medium, 실사용 10곳) + 새로 추가된 Small 사이즈·Pressed 상태 조합 5개였음 — `Type × Size × State` 2×2×2 매트릭스로 완성. `status-badge`(2개 variant 동일 이름)는 "재구매"/"한달사용" 진짜 다른 콘텐츠 — `Type=Repurchase/OneMonthUse`로 개명. `dialog`(variant 이름 2번 중복)는 스크린샷 결과 진짜 중복(이미 있던 "Status=Warning, Button=2 Button"과 텍스트까지 동일)이라 실사용 0곳 확인 후 삭제. 4건 모두 "이름만 보고 판단하지 않는다"는 기존 원칙을 그대로 적용 — merge/rename 전 항상 스크린샷·실측으로 진짜 콘텐츠 확인. → `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — Typography Noto Sans KR 혼입 전량 Pretendard로 전환, 실제 컴포넌트(Category Tab)에도 영향 있었음을 확인
감사에서 발견한 Text Style 6개(Noto Sans KR)를 사용자 확인 후 전부 Pretendard로 전환(같은 weight 유지). 이 6개 스타일이 스타일 정의뿐 아니라 **실제로 쓰이고 있었는지** 사용자가 직접 질문해 재확인한 결과, `Category Tab`의 "Category-Name" 텍스트가 `label/xsmall-medium` 스타일을 통해 Noto Sans KR을 그대로 쓰고 있었다 — 스타일 자체를 고치는 방식이라 인스턴스 텍스트까지 자동으로 함께 수정됨(별도 조치 불필요). 추가로 Foundation 페이지의 타이포그래피 스펙 표 안에 스타일 바인딩 없이 raw로 Noto Sans KR이 박힌 텍스트 28개(스펙 샘플 문구, "미토큰화" 주석 등)도 함께 발견해 전부 Pretendard로 전환 — 총 35곳. 전체 페이지 재스캔으로 Noto Sans KR 잔존 0건 확인. **참고**: 3개 "ㄴ ..." 페이지는 이번 조사 시점에 이미 사용자가 직접 삭제한 상태였음(파일에 Cover/Foundation/① Component 3페이지만 남음). → `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — Button-large Small/XSmall padding·폰트 크기, 승인 스펙대로 마저 적용
세션 초반 승인받은 스펙(Small 좌우8·상하6·13px / XSmall 좌우8·상하5·12px)이 Pretendard 폰트 블로커로 미적용 상태였던 것을, 감사에서 재확인 후(실제로는 둘 다 상하11·폰트14/13px) Pretendard 로드 가능 확인(2026-09-04)과 함께 마저 적용. Small/XSmall 각 7 variant, 총 14개 노드 — 실사용 0곳 확인 후 진행. → `diagnosis/button-history.md`, `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — `Text` 변수 컬렉션의 17px orphan 변수 2개(`Body/medium`, `Navigation/depth-medium`) 삭제
`decisions.md`에 이미 "17px는 정식 토큰화하지 않고 16px로 흡수"라 결정돼 있었는데도 두 변수가 여전히 17 값으로 남아있었다. 텍스트 노드·텍스트 스타일 전량 조회로 어디에도 바인딩되지 않았음을 확인한 뒤 삭제(값을 16으로 정정하지 않은 이유: 이미 다른 16px 변수/스타일이 존재해 중복 토큰을 만들 필요가 없었음). `Text` 컬렉션 16개→14개 변수. → `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — Button-large `Size` 값 대소문자 통일: `Large/Medium/Small/XSmall`로 확정
`Large`(대문자)/`medium`/`small`/`xsmall`(소문자)로 섞여 있던 것을 정리. "첫 글자 대문자+나머지 소문자" 규칙을 그대로 적용하면 `Xsmall`이 되는데, 프로젝트가 지금까지 써온 "XSmall"(약어 단위로 S도 대문자) 표기와 다르다는 점을 확인받은 뒤 `XSmall`로 최종 확정(21개 노드: `Large/Medium/Small/XSmall`). → `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — Component Checklist "List" 상태 정정: `list` 컴포넌트가 이미 존재함을 확인
2026-09-03 체크리스트 작성 시점엔 발견하지 못했던 `list`(`6527:40035`, `type=Default/Image/history × status=Default/pressed`, 실사용 7곳, 에러 없음)가 이미 만들어져 있었음을 감사 과정에서 확인 — Navigation/Thumbnail 때와 같은 유형의 누락. `status.md`와 Figma 체크리스트 보드(`6144:769`) 모두 ❌→✅로 정정, 체크리스트 14/15. → `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — Property 이름 표준화: 상태/선택 계열은 `Status`, 콘텐츠·글리프 종류는 `Type`으로 확정
20개 세트에 걸쳐 `status`/`Status`/`State`/`Property 1`로 흩어져 있던 것을 재조사한 결과 실제로는 3가지 의미(순수 상호작용/선택여부/콘텐츠·심각도 종류)가 섞여 있었다. 사용자 확정에 따라 전부 `Status`(Title Case) 하나로 통일(Button-large/Icon Button의 `State`, radio·icon(`6009:10568`)의 `Property 1` 포함, 약 90개 노드). 다만 `Icon / Utility`(아이콘 글리프 종류: filter/star/default)와 `Order Deadline`(콘텐츠 구성 차이: 단독/할인포함)은 상태가 아니라 종류 구분이라 판단해 `Type`으로 재분류 — `Status`로 무조건 통일하지 않고 개념에 안 맞으면 다르게 분류. `Sheet / Selector`·`roket_badge`의 소문자 `type`도 `Type`으로 표기만 통일.

부수적으로 두 가지를 함께 처리: (1) `Icon / Checkbox`(4개 variant)가 스크린샷 확인 결과 체크박스 아이콘 2개 + 라디오 아이콘 2개가 섞인 것으로 드러나 사용자 확정에 따라 `Icon / Checkbox`/`Icon / Radio`로 분리(`combineAsVariants`로 신규 세트 생성). (2) 조사 중 `Icon / Status`가 실제로 깨져 있음(2개 variant 모두 "Property 1=Default")을 새로 발견 — 범위 밖이었지만 사용자 확정으로 함께 수정(빨간 원=Error, 주황 삼각형=Warning, 색상 직접 조회로 확인).

`Quantity Stepper`는 예시값(1/5/999)이 아니라 −/+ 버튼의 실제 활성화 여부(stroke 색상 직접 조회)로 진짜 의미를 확인 — `Default`(둘 다 활성)/`MinReached`(−비활성)/`MaxReached`(+비활성)로 명명. 전체 42개 세트 재스캔으로 에러 0건 확인. 범위에서 제외: `Section Header`의 `State`(Show Action Boolean과 의미 중복 가능성, 새 발견으로만 기록), `ai_review`(값 1개), `menu`(하단내비 15개 값의 구조 재설계는 별도 P2). → `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — design.md §2.1 Badge 피드백 3건 반영: Rocket Badge 실측 정정, Status Badge를 Review Card에 실제 연결, Countdown Badge→Banner 재분류
사용자가 design.md §2.1을 리뷰하며 3가지를 지적. (1) `Rocket Badge`를 "pill 형태"로 잘못 서술한 것을 실측(radius 4, `small` 토큰)으로 정정 — `seller`/`fresh`/`global` 아이콘의 색상 레이어(`images 1`)가 변수에 바인딩 안 된 것은 버그가 아니라 하단 파란 로켓 이미지(`images 2`, MULTIPLY)를 COLOR 블렌드 모드로 틴트하기 위한 의도된 구조임을 확인, 바인딩은 그대로 두고 문서에만 예외로 명시. (2) `Status Badge`가 `Review / Card`와 실제로 연결돼 있지 않고(raw "status-badge" 프레임으로 흉내만 냄) 4개 variant 전부 이 상태였음을 발견 — raw 프레임을 삭제하고 진짜 `Status Badge` 인스턴스(Type을 Review Card 자신의 Status 값에 맞춰 매칭)로 교체, 실사용 9곳 스크린샷으로 시각 변화 없음 확인. (3) `Cart / Countdown Badge`와 `Order Deadline`이 둘 다 빨간 카운트다운으로 역할이 겹쳐 보인다는 지적에, 실측 결과 전자는 장바구니 전체 단위 배너(체크박스·펼침 화살표 포함)이고 후자는 상품 단위 인라인 텍스트로 실제 용도가 다르다고 판단 — 병합 대신 `Cart / Countdown Badge`를 `Cart / Countdown Banner`로 개명하고 체크리스트 분류를 Badge에서 Banner로 이동(구조상 pill 배지가 아니라 배너였음), 두 컴포넌트의 역할 차이를 design.md에 상호 문서화. → `design-system/design.md`

### 2026-09-04 — design.md §2.2 Banner 정의 보류, §2.3 Button을 3개 컴포넌트로 재구성 + Line type gray 고아 컴포넌트 발견
사용자가 §2.2 Banner의 "Cart / Notification Banner=알림 수신 동의 배너" 설명이 실제 Banner 정의와 맞지 않는다고 판단 — 임의로 정의를 만들지 않고 "정의 작성 전" 상태로 되돌림(Banner란 무엇인지 먼저 합의 필요).

§2.3 Button 피드백 반영: (1) Button이 사실 `Button`(CTA)/`Icon Button`/`Text Button` 3개 독립 컴포넌트임을 반영(기존엔 Text Button 누락). (2) `Type=Secondary` vs `Type=Line type gray`의 실제 사용 맥락을 전수 조사 — Secondary는 실사용 11곳 전부 "2-Button 조합의 차선책" 또는 "Action Bar 단독 저강조 행동" 두 패턴으로만 쓰였고, **Line type gray는 현재 Button 세트 안에서 실사용이 0곳**임을 확인. 시각적으로 회색 아웃라인처럼 보이는 "재입고 알림 신청"(Item Card/Cart 품절 상태) 버튼은 현재 세트가 아니라 **부모 COMPONENT_SET이 없는 고아 컴포넌트**(`6009:9856`, `Button/Line type Gray/Medium/Default/1 Button`)를 참조 중임을 새로 발견 — `component-naming-cleanup.md`에 2026-09-01 한 번 정식 세트로 편입시켰다는 기록이 있는데, 이후 여러 차례의 Button 구조 변경(CTA 개명, Icon Button 분리, Status 전환, Small/XSmall 추가 등) 과정에서 다시 분리된 것으로 추정. 재연결 여부는 사용자 확인 대기 — 이번엔 발견만 하고 Figma는 건드리지 않음. → `design-system/design.md`, `design-system/status.md`

### 2026-09-04 — "고아 컴포넌트" 용어를 "미아 컴포넌트"로 변경, 재연결 실행
사용자가 "고아"라는 표현이 세다며 "미아"로 바꿔 부르기로 함 — 앞으로 이 프로젝트에서 부모 COMPONENT_SET 없이 캔버스에 홀로 남은 컴포넌트를 가리킬 땐 "미아 컴포넌트"로 통일한다. `6009:9856`("재입고 알림 신청" 버튼)을 정식 `Button` 세트의 `Type=Line type gray, Size=Medium, Status=Default` variant로 재연결 — 인스턴스에 텍스트 override가 없음을 먼저 확인한 뒤(과거 Variant4 사고의 교훈 적용) `swapComponent()`로 안전하게 전환, 기존 정식 variant 쪽의 placeholder 텍스트("Button")를 실제 프로덕션 문구("재입고 알림 신청")로 갱신, 미아 컴포넌트는 실사용 0곳 확인 후 삭제. 실제 화면 스크린샷으로 변화 없음 확인. → `design-system/design.md`, `design-system/status.md`

### 2026-09-04 — 최상위 컴포넌트 이름 Title Case로 통일, 그 과정에서 진짜 중복 컴포넌트(`icon`/`Icon Checkbox`) 하나 더 발견해 병합
P2 작업으로 소문자(`chip`,`dialog`,`toast`,`radio`,`label`,`list`,`search`,`thumbnail`,`tab`,`menu`), snake_case(`roket_badge`,`bottom_navigation`,`ai_review`), kebab-case(`status-badge`), 하이픈(`Button-large`) 등 18개 최상위 세트 이름을 Title Case로 통일. `Tab`(그룹)과 `tab`(아이템)이 같은 이름이 될 뻔한 충돌은 `Tab Group`/`Tab Item`으로 분리. 화면 종속적이던 `Button / 2 Button Layout (legacy — Primary Large Default only)` 프레임 이름도 `2 Button Layout`으로 단순화.

이 과정에서 `icon`(`6009:10568`, 실사용 6곳)이 `Icon / Checkbox`(실사용 4곳)와 스크린샷상 완전히 동일한 파랑/회색 체크박스 아이콘임을 발견 — 이름만 다른 진짜 중복이라 사용자 확정 후 6개 실사용 인스턴스를 `swapComponent()`로 `Icon / Checkbox`로 옮기고 `icon`(`6009:10568`) 세트를 삭제. 전체 41개 세트 재스캔으로 에러 0건 확인. → `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — 감사 P2 나머지 3건 완료: Sheet/Confirmation 죽은 축 정리, Bottom Navigation Item 구조 재설계, Review Card 영문화
`Sheet / Confirmation`의 값이 1개뿐이던 `Item`/`Type` 축을 모든 variant 이름에서 제거해 `Status × Button`만 남김. `Bottom Navigation Item`(구 `menu`)의 `Status` 15개 평면값(`Home_on`, `Category_off` 등)을 `Icon(Home/Category/Search/Mypage/Cart) × State(On/Off/Pressed)` 5×3 매트릭스로 재구성 — 인스턴스는 노드 ID로 원본을 참조해 이름 변경만으로는 실사용 25곳에 영향 없음을 스크린샷으로 재확인. `Review / Card`의 한글 Variant 값(`한달사용`/`재구매`)을 `OneMonthUse`/`Repurchase`로 전환(`Status Badge`와 동일 영문 표기로 통일). 이것으로 감사 P0~P2 전 항목 완료, 전체 41개 세트 에러 0건. → `diagnosis/design-system-audit-2026-09-04.md`

### 2026-09-04 — design.md 초안 리뷰 중 Foundation Color 4건 처리: Primary "확정 금액" 실측 불일치 인정, Success=파랑 실사용 인정, Commercial 미사용 변수 삭제, Rocket 배지 색상 정식 등록
`design.md` 초안을 사용자가 검토하며 Color 섹션 4가지를 하나씩 확인:
- **(a) "확정 금액=파랑" 원칙**: `Price Block` 실측 결과 최종가가 할인 시 `Error/400`(빨강), 미할인 시 `Gray/900`(검정)이었고 파랑은 어디에도 안 쓰임 — 사용자가 "실제 상태를 인정하고 원칙 정정"으로 확정(파랑 원칙 폐기, 문서만 정정 예정).
- **(b) Success=초록 원칙**: `Toast`의 `Status=Success` 상태 아이콘이 전부 `Primary/400`(파랑)이고 `Success` semantic 변수는 실사용 0건 — 마찬가지로 "실제 상태 인정" 확정.
- **(c) Commercial(프로모션) 색상**: `Commercial/Background`·`Text`·`Border` 3개 변수 실사용 0건 확인 후 삭제, Foundation 문서의 대응 스와치 3개(Rectangle+라벨)도 함께 제거.
- **(d) 로켓 배송 배지 4색**: 그동안 미등록 상태였던 `Rocket Badge`의 seller/fresh/global/tomorrow 4색을 사용자 확정("배지당 배경+텍스트 2개씩, Rocket 패밀리")에 따라 `Color/Primitive/Rocket/{Seller,Fresh,Global,Tomorrow}/{Background,Text}` 8개 변수로 신규 등록, 컴포넌트 4 variant 전량 바인딩. Foundation 페이지에 이미 있던 문서 프레임이 이 4색을 **Success/Error/Warning/Info로 완전히 잘못 라벨링**하고 있던 것도 함께 발견해 Seller/Fresh/Global/Tomorrow로 정정.

(a)(b)는 아직 design.md 본문 반영 전(리뷰 계속 진행 중), (c)(d)는 Figma·status.md까지 실행 완료. → `design-system/design.md`(리뷰 대상 초안), `design-system/status.md`

### 2026-09-04 — 15px Medium 텍스트 전수 리스트업 결과, 외부 팀 라이브러리 원격 스타일 참조라는 새 유형의 문제 발견
`design.md` 리뷰 중 Typography 섹션의 "15px Medium 텍스트 리스트업" 요청에 따라 `① Component` 페이지 전수 조회. 기존에 알려졌던 "상품명 미바인딩"(3곳) 외에, `Quantity Stepper` 숫자와 "총 4,000원 할인" 텍스트(9곳)가 이름은 로컬 스타일과 같은 `text/body/small-medium`인데 실제로는 **외부 팀 라이브러리의 원격(remote) 스타일**을 참조하고 있어 로컬(14px)과 다른 값(15px)을 쓰고 있던 것을 새로 발견 — `style.remote` 플래그로 확인. 사용자 확정("상품명은 compact-medium으로 정식 토큰화, 나머지는 외부 참조 끊고 로컬 스타일로 재연결")에 따라 12곳 전부 로컬 `text/body/compact-medium`(15px, 시각적으로 기존과 동일)에 재바인딩. 전체 재스캔으로 미바인딩·원격 참조 0건 확인. → `diagnosis/typography-foundation-history.md`

### 2026-09-04 — 컴포넌트 라이브러리 padding/gap 전체를 Spacing Variable에 실제 바인딩
`design.md` 리뷰의 Spacing "알려진 이슈"(값은 원칙과 일치하나 Variable에 바인딩 안 됨) 처리. 41개 COMPONENT_SET 전체를 대상으로 16개 Spacing 값과 일치하는 padding/gap을 전수 스캔해 `setBoundVariable()`로 바인딩 — 각 variant 최상위 420곳 + 내부 중첩 프레임 960곳, 총 1,380곳(에러 0). 남은 160곳은 COMPONENT_SET 자체의 variant 갤러리 배치용 레이아웃(실제 UI 아님)이라 의도적으로 제외. 주요 컴포넌트 스크린샷으로 시각 변화 없음 확인. → `diagnosis/spacing-foundation-history.md`

### 2026-09-04 — Radius 공식 토큰 5단계 확정: `none`/`small`/`medium`/`large`/`full`
`design.md` 리뷰의 마지막 Foundation 항목. 실측값을 사용자와 대화로 하나씩 확인하며 확정 — `none`(0)/`small`(4)/`medium`(8)/`large`(16)/`full`(pill·원형), 필요해지면 `large` 상위를 계속 추가하는 open-ended 구조. 과정에서 `Icon Button`이 실제로는 8px 계열이 아니라 완전 pill(radius 50)이었던 것과, `Dialog`·`Sheet / Confirmation`·`Sheet / Selector` 3곳 전부가 동일하게 상단 모서리만 16px인 `large` 실사용 사례를 발견(사용자가 "bottom sheet, dialog에서 쓰고 있다"고 먼저 지적해 재확인). `full` 계열은 컴포넌트마다 제각각이던 큰 값(20/50/80/100/200)을 공유 토큰 999로 통일. `Radius` Variable Collection 신규 생성 후 컴포넌트 라이브러리 전체 1,346곳 바인딩(0이 아닌 값 1,238곳 전수 스캔 + 의도적으로 각진 컨테이너 8개 108곳 선별), 에러 0건, 스크린샷으로 시각 변화 없음 확인. → `diagnosis/radius-foundation.md`

### 2026-09-04 — design.md §2.4 Chip에 토글 동작 한 줄 추가
사용자 요청으로 `Selected` 상태의 Chip을 한 번 더 누르면 `Default`로 돌아간다(선택 해제)는 내용을 Variants/States에 한 줄 추가. Figma 변경 없음(문서만). → `design-system/design.md`

### 2026-09-04 — Dialog에 스크롤 UI 패턴 신규 바인딩(`Show Scroll` Boolean)
사용자가 콘텐츠 초과 시 Dialog에 스크롤이 생기는 UI를 참고 프레임(`6624:3353`, "① Component" 페이지)으로 직접 만들어두고, 이를 실제 `Dialog` COMPONENT_SET(`4043:2815`)에 정식 반영해달라고 요청. 참고 프레임을 실측(header 56/body 340·clipsContent/button 78, 구분선 2개+스크롤바(track+thumb))하고, 색상은 구분선→`Background/Divider`, scroll-thumb(회색 45%+50% opacity)→`Gray/700`(불투명도 유지) 바인딩, scroll-track(검정+6% opacity)은 Rocket Badge 블렌드 틴팅 때와 같은 원칙으로 **의도적으로 미바인딩**(반투명 검정 표현 기법이 토큰 색상값과 안 맞음).

실행: `Dialog` COMPONENT_SET에 `Show Scroll`(Boolean, 기본값 false) 속성 신규 추가 후, 7개 variant 전부에 `divider-top`/`divider-bottom`/`scroll-track`/`scroll-thumb` 4요소를 추가해 전부 `Show Scroll`에 `visible`로 바인딩(`Button=X` variant는 하단 버튼 영역 자체가 없어 `divider-bottom` 제외 3요소만 적용). 스크롤 발생 기준(뷰포트 70%·Body 최대 426px 등, 사용자 제공 스펙 그대로)은 Figma 정적 속성으로 표현할 수 없어 design.md에 개발 구현 규칙으로 문서화.

**겪은 문제**: 바인딩 직후 새로 만든 4요소 각각에 `node.visible = true`를 명시적으로 설정(마스터에서 보이게 하려는 의도)했더니, `Show Scroll` 속성 자체의 `defaultValue`가 의도치 않게 `true`로 바뀌어 있었음(componentPropertyDefinitions 재조회로 발견) — `editComponentProperty()`로 `defaultValue: false`로 재수정해 바로잡음. 테스트 인스턴스로 `Show Scroll=false`(기본, 오버라이드 없음)/`true` 양쪽 상태에서 4요소의 실제 visible 값이 정확히 토글되는 것을 3개 variant(Default 2-Button/Error 1-Button/X)로 확인 후 테스트 인스턴스 삭제. → `design-system/design.md`

### 2026-09-04 — Dialog 스크롤 기준을 상대 단위로 수정, scroll-thumb 색상 정정
사용자가 design.md에 문서화된 "Body 최대 높이 426px 초과 시 스크롤" 기준이 800px 뷰포트 가정의 고정 px 하드코딩이라 실제 개발에서는 위험하다고 판단 — `max-height: 70vh`로 다이얼로그 전체 높이를 잡고 거기서 고정 영역(Header+Button 134px, `Button=X`는 Header 56px만)을 뺀 값을 Body의 `max-height`로 쓰는 상대 단위 방식을 채택하기로 확정. 800px 기준 426px는 계산 예시로만 design.md에 남기고 실제 트리거 조건에서는 제거. Figma 쪽은 이미 `Show Scroll` Boolean으로 상태만 미리보기하는 구조라 변경 없음(문서만 수정).

동시에 `scroll-thumb` 색상을 `Gray/700`에서 `Gray/400`으로 변경 요청 — 7개 Dialog variant 전부의 `scroll-thumb` fill을 `Color/Primitive/Gray/400`으로 재바인딩(opacity는 기존 값 그대로 유지, 재조회 결과 실제로는 1이었음 — 원래 참고 프레임의 "50% opacity" 서술과 달랐던 것을 이번에 발견, 별도 조치 없이 그대로 둠). 테스트 인스턴스 스크린샷으로 밝은 회색(`#b2b5be` 계열)으로 정상 렌더링 확인 후 삭제. → `design-system/design.md`

### 2026-09-04 — design.md §2.6 Heading 피드백 2건: App Bar 고정 여부 실측 정정, Section Header Show Action Boolean 정상 배선
사용자가 §2.6을 리뷰하며 두 가지 지적. Explore 서브에이전트 2개를 병렬로 띄워 각각 조사.

**(a) App Bar**: "화면 상단에 고정되는 최상단 내비게이션 바"라는 서술이 `Index`·`Filter Compact` 등엔 안 맞는다는 지적. `App Bar`(`6008:8709`) COMPONENT_SET의 6개 Type(Title/Filter/Filter Compact/Search/Category/Index) 전부 실사용 인스턴스 위치를 전수 확인한 결과: `Title`/`Category`/`Search`는 진짜 Status Bar 바로 아래 화면 최상단(각 4곳/8곳/1곳), `Filter`는 `Category` App Bar 아래 2번째 줄(5곳, 독립적으로 최상단 아님), `Filter Compact`는 상품 상세페이지 스크롤 콘텐츠 중간의 리뷰 섹션 전용 필터 행(3곳), `Index`는 `Sheet / Selector`(Bottom Sheet) 내부의 자모 인덱스 점프 스트립(2곳) — 사용자 지적이 정확했음을 확인. Figma 자체는 이미 맞게 만들어져 있어 변경 없이 design.md 서술만 Type별로 분리해 정정. `Filter`/`Filter Compact`/`Index`를 계속 같은 `Type` 축에 둘지 별도 컴포넌트로 분리할지는 이번엔 다루지 않고 §5 후속 과제로 등록만 함(Cart/Countdown Badge→Banner 재분류 때와 유사한 성격의 미결 질문).

**(b) Section Header**: 우측 액션 버튼을 Boolean으로 on/off 가능하게 해달라는 요청. `Section Header`(`6324:1546`) 조사 결과 `State`(text/text + button) 축과 `Show Action` Boolean이 공존했는데 실제로 버튼 유무를 결정한 건 `State` 하나뿐이었고 `Show Action`은 어디에도 `componentPropertyReferences` 바인딩이 안 된 죽은 속성이었음(실사용 13곳 전부 `State=text`만 사용, `Show Action` 값은 true 7곳/false 6곳으로 제각각이었지만 뭘 넣어도 무의미했음). 실행: 실사용 13곳 전부가 쓰는 `text` variant(`6324:1542`)에 `Text Button` 인스턴스(전체보기 〉)를 새로 추가하고 그 `visible`을 `Show Action#6324:0`에 바인딩, 실사용 0곳이던 `text + button` variant(`6527:38876`) 삭제. 테스트 인스턴스로 `Show Action=true`/`false` 토글이 정확히 작동함을 확인. 이어서 `deleteComponentProperty('State')`로 이제 값 1개(`text`)만 남은 죽은 축을 완전히 제거하려 했으나 **"Invalid component property name" 에러로 실패** — Figma Plugin API가 COMPONENT_SET의 마지막 남은 variant 축 삭제는 허용하지 않는 것으로 보임. 강제 우회하지 않고 값 1개짜리 축인 채로 남겨두기로 하고 정직하게 문서화(기능상 문제는 없음 — Boolean 토글은 정상 동작). 실사용 인스턴스 3곳(`Show Action=true`)을 재조회해 2곳은 스크린샷으로 버튼이 정상적으로 새로 나타남을 확인, 1곳(`6474:34460`)은 스크린샷이 빈 이미지로 나왔으나 노드 레벨(fills/font/visible/opacity) 조회로는 완전히 정상이라 스크린샷 캡처 도구의 일시적 결함으로 판단(다른 2곳은 정상 캡처됨). `Show Action=true`였던 기존 인스턴스 7곳은 이번 수정으로 버튼이 처음으로 실제 노출되게 됨(의도된 정상 동작, 사용자에게 명시 보고). → `design-system/design.md`(v0.9)

### 2026-09-04 — Message Box와 Toast를 별개로 확정, Toast 동작(위치·노출시간) 신규 정의
사용자가 design.md §2.7이 "Message Box" 체크리스트 항목을 `Toast`에 매핑("사용자 미확인 추론")해둔 것에 대해 확정 지어줌 — **Message Box와 Toast는 별개 개념이고, Message Box는 아직 컴포넌트로 만든 적이 없다.** 체크리스트 7번 항목을 `✅(추정)`→`❌`로 정정(전체 14/15→13/15), `Toast`는 기존에 존재하는 별도 컴포넌트로 명시하고 "매핑 미확인" 캐비어트 제거.

동시에 사용자가 Toast의 동작(정확한 하단 위치, 노출 시간)을 "고민 필요"로 제기 — Explore 서브에이전트로 근거 조사: `Toast` 실사용 0곳 재확인(실측 배치 사례 없음), 컴포넌트 자체 폭 328px(360px 화면폭 − 좌우 16px, 기존 Spacing `16` 토큰과 일치), 높이 52~66px, radius 8px. `Bottom Navigation` 높이 87px(iOS/Android 동일), `Action Bar`(고정 CTA 바)가 이미 16px 하단 padding을 쓰는 선례 확인. 실측 근거가 없어(0곳) 순수 규칙으로 새로 정해야 했으므로 AskUserQuestion으로 두 가지 결정: **위치 = 화면 최하단 16px 고정(Bottom Navigation 유무 무관, 있으면 겹쳐서 뜸)**, **노출 시간 = 3초**. Figma 쪽 변경은 없음(이 규칙은 Dialog 스크롤 기준과 동일하게 개발 구현 규칙으로 design.md에만 문서화). → `design-system/design.md`(v0.10)

### 2026-09-04 — Item Card / Grid Small을 Recommendation으로 개명(신규 발견된 Item Card / Grid와 역할 분리), Price Block에 Show Unit Price Boolean 추가
사용자가 §2.8을 리뷰하며 두 가지 지적. Explore 서브에이전트 2개를 병렬로 띄워 각각 조사.

**(a) Item Card / Grid Small**: "이름에 오해가 있는 것 같다 — 실제로는 상품 상세/장바구니 하단에서 추가구매를 유도할 때 쓰이고 횡스크롤도 지원하며, Grid와 100% 같은 동작은 아니다"는 지적. `Item Card / Grid Small`(`6253:1706`, 130px) 실사용 15곳을 전수 확인한 결과 5개 사용처 전부 장바구니 "다시 구매하세요"(2곳)·상품 상세 "같이 둘러볼만한 상품"(3곳)의 횡스크롤 캐러셀(컨테이너 460px가 360px 화면보다 넓어 카드가 잘려서 노출)이었음 — 사용자 지적이 정확했음을 확인. 조사 중 지금까지 design.md에 전혀 문서화되지 않았던 별도 컴포넌트 `Item Card / Grid`(`6053:673`, 160px, variant 없는 단일 컴포넌트)를 새로 발견 — 실사용 8곳이 "상품목록_그리드 타입" 화면의 정확히 360px 폭 2열 그리드(횡스크롤 아님)를 담당하고 있었음. 즉 design.md가 `Grid Small`의 When to use에 써둔 "카테고리 목록, 검색 결과"는 실제로는 이 미문서 컴포넌트의 역할이었던 오기재. 두 컴포넌트는 실사용에서 완전히 분리(겹침 0건). 처리 방향을 AskUserQuestion으로 확인한 결과 "Figma에서도 이름 변경" 확정 — `Item Card / Grid Small`(`6253:1706`)을 `Item Card / Recommendation`으로 개명(실사용 15곳은 노드 ID 참조라 영향 없음, 재조회로 확인), `Item Card / Grid`를 §2.8과 요약 표에 신규 문서화(Description은 Figma 컴포넌트 자체 설명 "목록에서 상품을 표현하는 카드 컴포넌트(grid 밀도)... Price Block/Rating Display/Spec Row를 내부에 품는다" 인용, 상세 콘텐츠 패턴은 이번 조사에서 확인 못해 "확인 필요"로 정직하게 남김).

**(b) Price Block**: "상품을 그람이나 밀리미터로 구분할 수 없는 경우도 있어서 단위가 표시를 on/off할 수 있어야 한다"는 요청. `Price Block`(`6065:640`) 조사 결과 `Discount` 변형 축 하나뿐이고 단위가 관련 Boolean/Text 속성이 전무했음 — 양쪽 variant의 단위가 텍스트("(100ml당 715원)", `6036:451`/`6065:639`)가 `visible:true` 고정, 바인딩 없음. 실사용 46곳 전부 단위가가 노출된 상태(숨긴 사례 0건 — 이 요구사항 자체가 지금까지 처리된 적 없었음). 두 variant 모두 `VERTICAL` auto-layout hug라 구조적 리스크가 낮음을 확인 후 실행: `Show Unit Price`(Boolean, 기본값 true) 속성을 COMPONENT_SET에 신규 추가하고 양쪽 variant의 단위가 텍스트 `visible`을 바인딩. 테스트 인스턴스로 `Show Unit Price=false` 시 카드 높이가 60→44px로 자연스럽게 줄어드는 것을 확인(auto-layout hug 정상 반영). → `design-system/design.md`(v0.11)

### 2026-09-04 — Price Block 정가(회색) 텍스트 취소선 누락 버그 수정
사용자가 "Price Block에 회색으로 표시된 정가 영역에 취소선 표시가 누락됐어"라고 지적. `Discount=Applied` variant(`6036:452`)의 정가 텍스트 노드("20,300원", `6036:445`, Gray 계열 fill) 조회 결과 실제로 `textDecoration: "NONE"`이었음 — design.md §2.8은 이미 "정가 취소선"이라고 서술해뒀었는데(의도는 맞았음) Figma 쪽 구현이 그 의도를 안 따라가고 있던 순수 버그였음. 폰트 로드 후 `textDecoration: 'STRIKETHROUGH'`로 수정. 실사용 인스턴스 2곳을 재조회해 오버라이드 없이 마스터 변경이 정상 전파됨을 확인(스크린샷). design.md는 이미 정가 취소선이라고 정확히 서술돼 있어 문서 변경 불필요.

### 2026-09-04 — Review / Card의 Status 축을 Show Status Badge Boolean으로 대체, Review / Summary 유용성 평가
사용자가 §2.8을 리뷰하며 두 가지를 제기.

**(a) Review / Card**: "Status는 필요 없지 않아? Status Badge만 Boolean으로 조절할 수 있게 하면 될 것 같은데"라는 의견. `Review / Card`(`6013:12578`) 조사 결과 `Type`(Photo/Only Text) × `Status`(OneMonthUse/Repurchase) 4개 variant였고, 각 variant 내부의 실제 `Status Badge` 인스턴스가 이미 자기 `Type` 프로퍼티를 갖고 있어 Review/Card의 `Status` 축은 그 값을 그대로 미러링하는 중복 구조였음. 실사용 9곳을 전수 확인한 결과 **전부 `Type=Photo, Status=OneMonthUse` 하나만 사용** — design.md에 "4개 조합 전부 실사용"이라고 써뒀던 건 오기재였고 나머지 3개 조합은 실사용 0곳이었음. 사용자 의견이 정확했음을 확인 후 실행: 실사용 0곳인 `Type=Photo, Status=Repurchase`(`6289:1820`)·`Type=Only Text, Status=Repurchase`(`6013:12626`) 삭제, 남은 2개 variant(Photo/Only Text 각각의 OneMonthUse 쪽)에 `Show Status Badge`(Boolean, 기본값 true) 신규 추가해 `Status Badge` 인스턴스 `visible`을 바인딩. `deleteComponentProperty('Status')`로 이제 값 1개만 남은 죽은 축 제거를 시도했으나 Section Header 때와 동일하게 "Invalid component property name" 에러로 실패 — 강제 우회하지 않고 단일 값 축으로 남김. 검증: 실사용 3곳 재조회로 참조 정상, 테스트 인스턴스로 `Show Status Badge=false` 시 배지 노드가 트리에서 완전히 사라짐을 확인(`findOne` 결과 `undefined`), 중첩된 `Status Badge` 인스턴스의 `Type`을 `Repurchase`로 개별 override하는 것도 여전히 정상 작동함을 확인(Review/Card 자체 variant가 아니라 내부 인스턴스 속성으로 신뢰 신호 종류를 바꾸는 방식). → `design-system/design.md`(v0.12)

**(b) Review / Summary 유용성 평가**: 사용자가 사용자 입장에서 유용한지 평가를 요청 — Figma 실측(스크린샷+구조 재확인, 실사용 여전히 0곳 재확인)에 근거해 의견만 제공, Figma·문서 변경 없음(대화로 답변).

### 2026-09-04 — Review / Summary "실사용 0곳" 평가 오류 정정(실제 인스턴스 복원) + 카테고리 유연성 확보(Show Row 1~4 Boolean)
직전 턴에서 Review / Summary "유용성 평가"에 "실사용 0곳"을 근거로 답했는데, 사용자가 실제로는 상품 상세페이지(`6249:9356`)에 썼었고 컴포넌트 인스턴스 정리 과정에서 지워진 것 같다고 정정해줌. 확인 결과 정확했음 — 그 자리엔 진짜 INSTANCE가 아니라 `Review / Summary`의 `Status=Default` 마스터(`6013:12528`)를 완전히 그대로 복제한 raw FRAME 트리가 남아있었음(328×282 구조 100% 일치, 텍스트 15개 노드 전부 placeholder와 글자 하나 안 틀리고 동일 — "미아 컴포넌트"·Status Badge 때와 같은 유형의, 컴포넌트 정리 과정에서 인스턴스 연결이 끊긴 사고). 콘텐츠가 placeholder와 동일해 손실 위험 없이 raw 프레임을 지우고 같은 위치·같은 부모·같은 인덱스에 실제 인스턴스를 새로 생성해 교체(전/후 스크린샷 시각적으로 동일 확인) — "실사용 0곳" 서술을 "1곳"으로 정정.

동시에 "카테고리 종속적이라 유연하게 가져가려면 어떻게 해야할지" 의견을 요청받음. 검토 결과 각 속성 행의 텍스트(라벨/정성평가/퍼센트)는 이미 일반 TEXT 레이어라 인스턴스별 override가 원래부터 가능했고, 진짜 부족한 건 "이 카테고리엔 이 속성 자체가 필요 없다"를 표현할 방법(4행 전부 강제 노출)이었다고 판단 — `Spec Row`의 `Show ETA`/`Show Reward`와 동일한 기존 컨벤션을 따라 `Show Row 1`~`Show Row 4`(Boolean, 기본값 true) 4개를 COMPONENT_SET에 신규 추가하고 3개 variant(Default/none photo/no rating) 전부의 4개 속성 행에 바인딩. 텍스트 자체는 프로퍼티로 추가 노출하지 않기로 함(4행×3텍스트=최대 12개 프로퍼티는 이 파일의 기존 관례 대비 과도). 테스트 인스턴스로 2개 행을 꺼봤더니 높이가 282→214px로 자연스럽게 줄고 남은 행들이 정상적으로 붙어 올라옴을 확인. → `design-system/design.md`(v0.13)

### 2026-09-04 — List의 8자 규칙을 history 타입에서 제외(말줄임 처리로 확정), Category Menu Item 실제 화면 재확인
사용자가 §2.10을 리뷰하며 두 가지 제기.

**(a) List**: "history(검색 기록)엔 8자 내외 규칙을 적용하지 않는 게 맞고, 텍스트가 길어지면 말줄임 처리하자"는 의견에 대해 먼저 의견을 요청받아, 실제 `type=history` 인스턴스 텍스트("삼성전자 615L 2도어 냉장고", 18자)가 8자를 훌쩍 넘는 것을 근거로 동의 — 8자 규칙은 `Default`/`Image` 타입의 실제 관찰(냉동 블루베리 등 장바구니 명사)에서 나온 건데 `history`(자유 입력 검색어)에 잘못 일반화됐던 것. 동시에 현재 List 텍스트 노드의 `textTruncation`이 전부 `DISABLED`(고정폭 248~280px)로 돼 있어, 말줄임 규칙을 채택해도 실제로는 작동 안 하는 상태였음을 발견해 함께 보고. 사용자가 "진행해줘"로 확정 — 5개 variant(Default/Image × Default/pressed + history) 전부의 텍스트 노드에 `textTruncation: 'ENDING'` 적용(폰트 로드 후 실행). 테스트 인스턴스에 긴 문자열("삼성전자 비스포크 인피니트 라인 냉장고 4도어 프리스탠딩 콤보")을 넣어 실제로 폭에 맞춰 잘리는 것을 스크린샷으로 확인.

**(b) Category Menu Item**: 사용자가 Content rules 문구("마스터 라벨이 카테고리 placeholder뿐")를 이해 못 하겠다고 해서, 이 컴포넌트가 실제로 파일 전체 18곳 어디에도 진짜 카테고리명이 들어간 적 없이 전부 placeholder "카테고리" 그대로였다는 뜻이라고 설명. 이어서 사용자가 실제 화면 링크(`6527:39578`, "★ 상품목록_그리드 타입" 내 Bottom Sheet 사용 예시)를 공유하며 "이렇게 보여주면 될까?"라고 확인 요청 — 해당 프레임 내 Category Menu Item 인스턴스 9개(3×3 그리드, 바텀시트 안)를 직접 조회한 결과 **배치 맥락(3열 그리드·바텀시트 내부)은 기존 When to use 서술과 정확히 일치**함을 확인했으나, 이 9곳도 전부 여전히 placeholder "카테고리" 그대로라 **Content rules(글자 수·말줄임 규칙)를 뒷받침할 실제 콘텐츠는 여전히 없음**. Figma 변경 없이 design.md에 이 재확인 결과와 "실제 카테고리명 예시 확보 전까지 `Category Tab`의 실측 규칙을 잠정 준용" 권고만 추가. → `design-system/design.md`(v0.14)

### 2026-09-05 — Bottom Sheet를 Informational/Interactive 2개로 명칭 분리, 양쪽에 Show Scroll Boolean + 80% 높이 규칙 반영
`design.md`를 섹션 순서대로 훑어보던 중 §2.12 Bottom Sheet 차례에서 사용자가 4가지를 요청. Explore 서브에이전트 2개를 병렬로 띄워 조사한 뒤 직접 Figma를 재조회해 실행.

**(a) 개명**: `Sheet / Confirmation`(`4043:2686`)을 `Bottom Sheet / Informational`로, `Sheet / Selector`(`6005:6973`)를 `Bottom Sheet / Interactive`로 개명(Figma 실행, COMPONENT_SET 이름만 변경). 조사 결과 파일 안에 이미 두 컴포넌트 위에 각각 "Informational Bottom Sheet"(`6366:16362`)/"Interactive Bottom Sheet"(`6366:16408`) 섹션 제목 텍스트가 붙어 있어 — 이 구분이 이미 파일에 암묵적으로 존재하던 개념이었음을 확인. 내부 variant 축(Informational: `Status`×`Button`+`Show Icons` / Interactive: `Type`=Item·Filter)은 그대로 유지, decisions.md:124에서 이미 정리된 죽은 축(`Item`/`Type` 단일값)은 재도입하지 않음. 개명 직후 재조회로 이름만 바뀌고 축이 그대로임을 확인.

**(b) `Show Scroll` 추가**: Dialog(§2.5)와 동일한 패턴을 양쪽에 독립적으로 적용 — 각각 `Show Scroll`(Boolean, 기본값 false) 속성 신규 추가 후 `divider-top`/`divider-bottom`(`Background/Divider` 토큰)+`scroll-track`(미바인딩, 검정 6% opacity)/`scroll-thumb`(`Gray/400`) 4요소를 만들어 바인딩. **구조적 차이를 사전에 실측**: `Bottom Sheet / Informational`은 6개 variant 전부 `Button=1/2 Button` 조합이라 예외 없이 버튼 푸터가 있어 4요소 전부 적용. `Bottom Sheet / Interactive`는 `Type=Filter`(`6005:6974`, header→body 408px 고정+기존 `overflow-clip`, 내부 콘텐츠 실제 724px로 이미 진짜 스크롤 상황→button 푸터 88px)는 Dialog와 동일한 3분할 구조라 4요소 전부 적용, `Type=Item`(`6005:6972`)은 실측 결과 header→body(447px, Hug)만 있고 **푸터 자체가 없어** Dialog `Button=X`와 동일한 원칙으로 `divider-bottom`을 생략하고 3요소(divider-top+scrollbar)만 적용. Item의 body(`6005:6062`)에는 신규로 `clipsContent:true`를 부여.

**겪은 문제**: 스크롤바(`scroll-track`/`scroll-thumb`) 레이어를 auto-layout 부모에 `appendChild`한 뒤 `x`/`y`를 먼저 설정하고 `layoutPositioning`을 나중에 `'ABSOLUTE'`로 바꿨더니, 그 시점에 x/y가 auto-layout이 계산해둔 흐름상 위치(예: `x:0, y:326`)로 리셋돼버림 — `layoutPositioning='ABSOLUTE'`를 **먼저** 설정한 뒤 `x`/`y`를 설정하는 순서로 고쳐 해결(Informational 6개 variant는 사후 수정, Interactive 2개 variant는 처음부터 올바른 순서로 실행). 양쪽 COMPONENT_SET 모두 `componentPropertyDefinitions` 재조회로 `Show Scroll`의 `defaultValue`가 `false`로 유지됨을 확인. 각 컴포넌트(Informational 1개, Interactive 2개 — Filter/Item 각각)에 테스트 인스턴스를 만들어 `Show Scroll=true`/`false` 양쪽 상태를 스크린샷으로 확인(4요소/3요소가 정확히 토글, `false`일 때 관련 노드 `visible` 전부 0건)한 뒤 전부 삭제.

**(c) 80% 높이 규칙**: Dialog의 70vh 규칙과 동일한 이유로 Figma 프레임 자체에는 손대지 않고, `max-height: 80vh`로 design.md에 개발 구현 규칙 문서화. 파일에 이미 있던 주석("bottom sheet의 전체 길이는 화면의 80%를 넘어가지 않는다")과 360×640 참고 프레임(이 파일의 표준 아트보드 360×800의 80%)을 계산 예시로 인용.

**(d) Content rules 갱신**: Informational=순수 텍스트 전달 전용, Interactive=장바구니 담기 완료 후 교차판매·업셀 확인(`Type=Item`) 또는 전체 화면급 필터 선택(`Type=Filter`)이라는 실제 용도를 명시.

날짜는 이 레포의 모든 기존 기록이 2026-09-04로 통일돼 있었으나, 실제 세션 날짜(2026-09-05)로 처음 갱신 — `discription` TEXT 프로퍼티의 기존 오탈자(정상 철자 description)는 이번 작업 범위 밖이라 임의로 고치지 않고 design.md에 한 번만 캐비어트로 남김. → `design-system/design.md`(v0.15)

### 2026-09-05 — Bottom Sheet / Interactive의 스크롤바 인디케이터 제거(후속 수정)
직전 항목에서 `Bottom Sheet / Interactive`에도 Dialog와 동일한 스크롤바(`scroll-track`+`scroll-thumb`)를 넣었는데, 사용자가 "interactive bottom sheet에는 스크롤바 표시 제거해줘"라고 요청. 범위를 AskUserQuestion으로 확인 — 구분선(`divider-top`/`divider-bottom`)과 `Show Scroll` Boolean 자체는 유지하고 **스크롤바 인디케이터만** 제거하는 것으로 확정(기능 전체 롤백이 아님).

실행: 방금 전 작업에서 이미 노드 ID를 알고 있었으므로 `Type=Filter`(`6005:6974`)의 `scroll-track`(`6684:4`)/`scroll-thumb`(`6684:5`), `Type=Item`(`6005:6972`)의 `scroll-track`(`6684:7`)/`scroll-thumb`(`6684:8`) 4개 노드를 이름 확인 후 `remove()`. `Bottom Sheet / Informational`의 스크롤바는 요청 대상이 아니라 손대지 않음.

검증: `componentPropertyDefinitions` 재조회로 `Show Scroll#6684:0` 정의와 `Type` variant 축이 그대로임을 확인, 테스트 인스턴스(Filter/Item 각 1개)로 `Show Scroll=true` 시 구분선만 보이고 스크롤바는 없음(`findAll`에 scroll-track/thumb 자체가 안 잡힘), `false` 시 구분선도 사라짐을 스크린샷·`visible` 카운트로 확인 후 삭제. → `design-system/design.md`(v0.16)

### 2026-09-05 — Bottom Sheet / Interactive의 Show Scroll 기능 완전 제거(재정정)
직전 항목에서 스크롤바만 지우고 구분선+`Show Scroll` Boolean은 남겨뒀는데, 사용자가 "boolean이 아직 지워지지 않았어"라고 재요청. Figma를 재조회해 실제로 직전 작업 그대로(Boolean+구분선 존재, 스크롤바만 없음)인 것을 확인한 뒤, "boolean"이 `Show Scroll` 속성을 가리키는 게 맞는지 AskUserQuestion으로 재확인 — **Show Scroll 기능 자체를 완전히 삭제**하는 것으로 범위 재확정(직전 결정을 뒤집음).

실행: `Type=Filter`(`6005:6974`)의 `divider-top`(`6684:2`)/`divider-bottom`(`6684:3`), `Type=Item`(`6005:6972`)의 `divider-top`(`6684:6`) 3개 노드를 이름 확인 후 `remove()`. 이어서 `Bottom Sheet / Interactive` COMPONENT_SET(`6005:6973`)에서 `deleteComponentProperty('Show Scroll#6684:0')` 실행 — Section Header·Review/Card 때와 달리 이번엔 variant 축이 아니라 순수 Boolean 속성이라 에러 없이 정상 삭제됨. 재조회로 `componentPropertyDefinitions`에 `Type`(Item/Filter)만 남고 `Show Scroll`이 완전히 사라졌음을 확인, Filter는 `header`/`body`/`button` 3개, Item은 `header`/`body` 2개 자식만 남아 2026-09-05 작업 이전 원래 구조로 정확히 복원됐음을 스크린샷으로 확인. `Bottom Sheet / Informational`의 `Show Scroll`(구분선+스크롤바 4요소)은 이번 요청 대상이 아니라 변경 없음. → `design-system/design.md`(v0.17)

### 2026-09-05 — Bottom Sheet Informational/Interactive의 Show Scroll 유무에 대한 설계 근거 문서화
사용자가 "같은 바텀시트인데 어떤 건 스크롤 핸들이 있고 어떤 건 없다, 그 이유를 어떻게 설명해야 하냐"고 질문. Figma 변경 없이 대화로 근거를 정리해 제공 — Informational은 길이가 예측 불가능한 자유 텍스트라 스크롤 경계를 명시적으로 알려줄 필요가 있고, Interactive는 리스트(Filter의 필터 칩)·캐러셀(Item의 가로 스크롤 상품 카드) 형태 자체가 이미 스크롤 가능함을 암시해 별도 인디케이터가 불필요하다는 논리(이 파일의 다른 캐러셀 `Item Card / Recommendation` 등도 스크롤바 없이 카드 크롭만으로 스크롤을 암시하는 것과 일관). 사용자가 이 설명을 design.md에 정식 근거 문장으로 남겨달라고 요청해 §2.12 "스크롤 동작" 서브섹션에 반영, 기존 경위 서술은 이 근거의 배경으로 재배치. → `design-system/design.md`(v0.18)

### 2026-09-05 — Tab Group/Tab Item Description·When to use를 실사용 근거로 재작성
사용자가 §2.13 Tab의 Description/When to use가 "부실하다, 어떤 역할을 하는 컴포넌트인지 설명해야 한다"고 지적. fork 서브에이전트로 페이지 `1:9`("① Component", 이 파일의 유일한 실제 화면+컴포넌트 통합 페이지) 전체 915개 INSTANCE 노드를 스캔해 `Tab Group`(`6009:10202`)/`Tab Item`(`6009:10187`)의 mainComponent 계보를 추적.

발견: `Tab Group` 실사용은 **4곳 전부 장바구니 화면(`★ 장바구니`) 단일 패턴**이고 전부 `Type=3 Tab` — 라벨은 "일반구매(6)"(Status=Selected, 기본 활성)/"자주산상품"/"찜한상품(40)". 즉 상품 카테고리를 나열하는 게 아니라 **사용자 개인의 구매·관심 이력을 관점별로 재구성해 같은 리스트 영역을 전환**하는 용도였음 — 기존 design.md의 "카테고리별 전환"이라는 프레이밍은 부정확했음(다만 예시로 들었던 "일반구매/자주산상품/찜한상품" 라벨 자체는 우연히 정확했음). `Type=2 Tab`/`Type=Swipe`는 라이브러리에 정의만 있을 뿐 실사용 0곳 — 기존에 "탭 4개 이상·가변 길이면 Swipe"라고 써뒀던 임계값은 실사용 근거가 전혀 없는 추측성 서술이었음을 확인. `Tab Item` 인스턴스는 23곳 발견됐으나 전수 확인 결과 예외 없이 `Tab Group` 마스터 variant 내부의 자식으로만 존재 — 기존 "Tab Group 내부 전용" 서술은 정확했음(변경 불필요).

실행: Description을 "같은 데이터를 다른 기준으로 재구성해서 보여주는 용도"로, When to use를 실사용 4곳의 구체적 라벨·맥락으로 재작성. Variants/States에 `3 Tab`만 실사용 확인·`2 Tab`/`Swipe`는 라이브러리 전용이라는 캐비어트 추가. Content rules의 근거 없는 Swipe 임계값 삭제. Figma 변경 없음, design.md 문서만. → `design-system/design.md`(v0.19)

### 2026-09-05 — design.md §2 Component를 공개 디자인 시스템 사이트 스타일로 전면 재구성
사용자가 당근마켓 Seed Design(`seed-design.io/components/alert-dialog`)과 지마켓 GDS(`gds.gmarket.co.kr/components/dialogs`) 두 사이트를 링크로 주며, design.md 작성 시 문체·문장 구조·내용을 참고해달라고 요청. WebFetch로 두 페이지를 각각 분석한 결과 공통 패턴을 확인: (1) 한 문장 존댓말 Description, (2) 구조 요소를 나열하는 Anatomy, (3) Variants/Props를 `Property | Values | Default` 표로 정리(쉼표 나열 산문이 아님), (4) When to use를 Do/Don't 쌍으로 명시, (5) 헷갈리기 쉬운 유사 컴포넌트끼리 비교표, (6) 코드 예시 없이 순수 사용 가이드 톤.

적용 범위를 AskUserQuestion으로 확정: **§2.1~2.15 전체 15개 컴포넌트 섹션**에 새 스타일을 적용하되, **기존 감사/조사 이력(✅ 정정, ⚠️ 확인 필요, node ID, 날짜, 실사용 N곳 등)은 전부 유지**하기로 함 — 참고 사이트들은 이런 이력 서술 없이 깔끔한 최종본만 보여주지만, 이 문서는 공개 마케팅 페이지가 아니라 Claude/개발자가 참고하는 내부 작업 기록이라 근거 추적성이 더 중요하다고 판단.

실행: fork 서브에이전트에게 §2.1~2.15 전체를 위 템플릿(Description 문장 + Anatomy(구조가 알려진 경우만) + 사용 가이드 Do/Don't + Variants 표 + Content 목록)으로 재작성하도록 위임 — 기존 텍스트에 이미 있는 사실만 재구성하고 새 규칙·비교는 만들어내지 않도록(순수 리포매팅) 명시. Button/Chip/Heading/Item Card/List/Navigation/Bottom Sheet/Tab처럼 형제 컴포넌트가 여러 개인 섹션에는 상단에 짧은 "비교" 노트를 추가해 역할 차이를 한눈에 보이게 함. §1 Foundation, §2 요약 체크리스트 표, §3 Naming Convention, §4 Design Principles, §5 후속 과제는 대상에서 제외.

**겪은 문제 없음** — 재작성 전/후로 모든 node ID(정규식 `` `[0-9]+:[0-9]+` ``)와 "실사용 N곳" 카운트를 diff해 완전히 일치함을 확인(정보 손실 0건). 유일한 표면적 차이는 `Bottom Sheet / Informational`·`Interactive` 각각에 반복돼 있던 "2026-09-05 개명" 문구를 섹션 상단 비교 노트 한 곳으로 합친 것뿐 — 사실 자체는 그대로 보존됨. → `design-system/design.md`(v0.20)

### 2026-09-05 — 당근/G마켓 벤치마킹 후속 반영: Figma 참조 라인 + 비교 표 전환
직전 재구성 후 사용자에게 "당근/G마켓에서 더 벤치마킹할 부분이 있냐"고 질문받아 3가지 제안: (1) 산문 "비교" 노트를 표로 전환(두 사이트의 Alert Dialog vs Menu Sheet 비교표 패턴), (2) 컴포넌트마다 Figma node-id 참조 라인 추가(두 사이트의 Figma/React/iOS 링크 패턴), (3) 컴포넌트별 "확정 주체"(담당자) 표기(G마켓의 담당자 표기 패턴). 사용자가 "개인 프로젝트라서 3번은 필요 없고, 1번/2번 반영하고 싶다"고 확정.

실행 전 Figma를 직접 재조회(`findAllWithCriteria`로 페이지 `1:9`의 모든 COMPONENT_SET/독립 COMPONENT를 스캔)해 40개 컴포넌트의 정확한 node ID를 확보 — 추측이나 과거 기억에 의존하지 않고 실시간 값만 사용. 이 중 design.md에 문서화된 37개 최상위 컴포넌트에 `Figma: node-id` 라인을 Description 바로 아래에 추가(Cart 전용 4종은 기존 표에 Figma 열 추가). 미착수 placeholder(Banner 정의, Message Box, Radio 본체)에는 링크할 실체가 없어 제외, `Icon / Radio`(`6573:3439`)만 실재하는 유일한 아티팩트라 예외로 추가.

산문 "비교" 노트는 형제 컴포넌트가 진짜 병렬 비교 가능한 6개 섹션(Badge: Rocket vs Status, Button: CTA/Icon/Text 3종, Chip: Chip vs Option Chips, Item Card: Grid/Recommendation/Cart 3종, List: List/Category Tab/Category Menu Item 3종, Bottom Sheet: Informational vs Interactive)만 표로 전환 — Tab Group/Tab Item, Bottom Navigation/Bottom Navigation Item처럼 상위 컴포넌트가 하위를 내부에 품는 포함 관계인 곳은 "비교"가 아니라 "구성"이라 표로 강제하지 않고 산문 그대로 유지. 표 전환 시 기존 산문이 담고 있던 수치(카드 폭, 실사용 곳수 등)는 전부 표 셀로 그대로 옮김.

**겪은 문제 없음** — 필요한 37개 ID 전부 design.md에 존재함을 grep으로 확인, 기존 node ID·실사용 카운트도 편집 전후 그대로 유지(삽입/치환만 했을 뿐 삭제 없음). → `design-system/design.md`(v0.21)

### 2026-09-05 — §2.15 Checkbox "처음부터 없음" 서술 정정 — `Icon / Checkbox` 실재·실사용 확인
사용자가 §2.15 Radio/Checkbox 섹션의 "Checkbox는 처음부터 만들어진 적이 없다"는 서술에 대해, 실제로는 있다며 Figma 링크(`node-id=6020-13544`)를 공유. 해당 노드를 직접 조회한 결과 `Icon / Checkbox`(`6020:13544`) COMPONENT_SET이 실재함을 확인 — `Status=Default`/`Disabled` 2개 variant, 둘 다 내부가 단일 VECTOR 아이콘 하나뿐이고 스크린샷 확인 결과 둘 다 "체크됨(✓)" 모양이 고정으로 그려져 있음(Default=파란 배경, Disabled=회색 아웃라인). `Icon / Radio`와 완전히 동일한 구조적 패턴(장식용 글리프, Unchecked/Selected를 나타내는 별도 variant 없음).

실사용 조사(`findAllWithCriteria`로 mainComponent 계보 추적, 총 10개 인스턴스 히트): `Item Card / Cart`(§2.8) 마스터 자체에 내장 — `Stock=Available`(`6067:664`)는 `Status=Default`, `Stock=OutOfStock`(`6067:791`)은 `Status=Disabled`를 씀(이는 design.md §2.8이 이미 "체크박스+Quantity Stepper"라고 서술해뒀던 바로 그 체크박스였음). 장바구니 화면(`★ 장바구니`)의 실제 상품 리스트 인스턴스 4곳(`I6223:5384`/`5386`, `I6474:35186`/`35188`)에서 `Status=Default`로 추가 확인. `Cart / Selection Toolbar`(§2.6)도 마스터+인스턴스 3곳에서 `Status=Disabled`로 내장 사용 중. 즉 "실사용 0곳"이 아니라 최소 10곳 이상 실사용 중이었음.

**정정 범위**: "Checkbox가 아예 없다"는 사실 오류만 정정하고, "진짜 토글 가능한 인터랙티브 Radio/Checkbox는 여전히 0/2"라는 핵심 결론은 그대로 유지 — `Icon / Checkbox`는 Unchecked(빈 박스) variant 자체가 없어 실제 체크/언체크를 표현할 수 없는 반쪽짜리 아이콘 글리프이기 때문. `Naming Convention`(§3)의 패밀리 네이밍 예시에 이미 `Icon / Checkbox`라는 이름이 등장했었는데, §2.15 작성 시 이 컴포넌트의 존재를 교차 확인하지 않았던 게 오류의 원인 — 앞으로 비슷한 "미착수/없음" 판정을 내릴 때는 Naming Convention이나 다른 섹션에 이미 언급된 이름과 교차 검증하는 습관이 필요함을 기록해둔다.

실행: §2.15을 Radio/Checkbox 각각 독립 블록으로 재작성(Figma 실행 없음, 문서만) — Icon/Checkbox에 실사용 근거(노드 ID·인스턴스 수 포함)를 명시하고 Unchecked 부재 한계를 명확히 함. §2 요약 체크리스트 표(15번 행)의 "실제 컴포넌트" 칸을 "없음"→"`Icon / Radio`, `Icon / Checkbox`"로 갱신. → `design-system/design.md`(v0.22)

### 2026-09-05 — §3 Naming Convention 3건 보강: Boolean 접두사 규칙, Foundation 토큰 표기 관찰, 오탈자 방지 권고
사용자가 §3 Naming Convention이 당근 Seed Design·지마켓 GDS 대비 체계적인지 질문. WebSearch/WebFetch로 두 사이트의 foundation/design-token 페이지, GDS의 `/brand/notation` 페이지까지 확인했으나 **둘 다 네이밍 컨벤션 자체를 공개 문서로 두지 않음**을 발견(Seed Design 페이지엔 `fg.brand`/`bg.brand`/`global-gutter` 같은 토큰 예시만 있고 규칙 설명은 없음, GDS `/brand/notation`은 브랜드 로고 병기 규정일 뿐 토큰/컴포넌트 네이밍과 무관). 구조 대 구조 비교가 불가능해, 대신 우리 §3 자체를 감사해 3개 공백을 짚어 보고했고 사용자가 셋 다 반영을 요청.

**(1) Boolean 프로퍼티 접두사**: 이 세션에서 만든 모든 Boolean(`Show Icons`, `Show Scroll`, `Show Unit Price`, `Show Status Badge`, `Show Row 1`~`4`, `Show Leading/Trailing Icon`)을 나열해보니 예외 없이 `Show {명사}` 패턴이었음 — 실제로는 지키고 있었지만 §3에 규칙으로 적힌 적이 없었던 "암묵적 관례"였음을 확인해 명문화.

**(2) Foundation 토큰 이름**: §1을 다시 훑어 카테고리별 실제 표기를 대조 — Semantic Color(`Background/Divider`, Title Case+슬래시) vs Typography(`body/large-bold`, 소문자+슬래시) vs Radius(`none`/`small` 등 소문자 단일 단어, 계층 없음) vs Spacing(숫자 그대로, 이름 레이어 자체가 없음)로 카테고리마다 관례가 다름을 확인. 통일된 규칙이 있는 척 지어내지 않고 "관찰된 현재 상태"로 정직하게 표로 기록, 통일 필요 여부는 §5 후속 과제로 등록(강제 통일하지 않음).

**(3) 오탈자 방지**: `Bottom Sheet / Informational`의 `Discription` 오탈자(§2.12, 2026-09-05 v0.15에서 이미 발견·문서화했었음)가 이미 실사용에 반영된 채로 굳어진 전례를 근거로, 새 Property·컴포넌트 이름을 추가하기 직전 `componentPropertyDefinitions` 재조회로 철자를 재확인하는 걸 권장 규칙으로 추가.

부수적으로 §5의 "Checkbox — 미착수" 문구가 v0.22의 재정정(`Icon / Checkbox` 실재 확인) 내용과 안 맞는 걸 발견해 함께 갱신(Checkbox/Radio 둘 다 "체크됨/선택됨 모양만 있는 장식용 글리프"라는 정확한 현재 상태로). Figma 변경 없음, design.md 문서만. → `design-system/design.md`(v0.23)

### 2026-09-05 — §4 Design Principles에 신규 원칙 2개 추가(세션 전체 리뷰 기반)
사용자가 "지금까지 대화한 내용을 기반으로 §4 Design Principles를 업데이트해달라"고 요청. 기존 8개 원칙(모두 2026-09-04 이전 감사에서 도출)에 더해, 이번 세션(2026-09-05)에서 실제로 확인된 사실 중 컴포넌트 하나에 국한되지 않고 여러 컴포넌트에 걸쳐 반복 확인된 "원칙" 수준의 패턴 2개만 골라 추가 — 개별 컴포넌트 사실(예: Checkbox 존재 여부, Figma 참조 라인 등)은 이미 §2/§3/§5에 반영돼 있어 §4엔 진짜 원칙급만 승격.

**(1) 모달형 오버레이 높이/스크롤 원칙**: §2.5 Dialog(`max-height: 70vh`)와 §2.12 Bottom Sheet(`max-height: 80vh`)가 서로 다른 시점에 독립적으로 "고정 px 대신 vh 상대 단위"라는 같은 규칙에 도달한 것, 그리고 Bottom Sheet Informational/Interactive의 Show Scroll 유무 차이(자유 텍스트엔 인디케이터 필요, 리스트·캐러셀엔 불필요)가 Dialog에도 그대로 적용 가능한 일반 원칙이라고 판단해 승격.

**(2) 탭형 선택 UI 역할 분리 원칙**: §2.13 Tab Group 실사용 조사(장바구니 3-tab, 사용자 이력 관점 전환)와 기존에 이미 있던 `Category Tab`(카테고리 나열)의 역할 차이가, 이후 비슷한 탭 UI를 새로 만들 때 "이게 Tab Group인지 Category Tab/Chip인지"를 판단하는 재사용 가능한 기준이 될 만하다고 판단해 원칙으로 승격.

Figma 변경 없음, design.md 문서만 수정(§4 8개→10개, 서문 문구도 갱신). → `design-system/design.md`(v0.24)

### 2026-09-05 — §5/§2 요약 표의 Section Header 관련 문서 정합성 오류 정정
사용자에게 현재 남은 후속 과제를 리스트업해주다가, §5 "아직 없는 것/후속 과제"의 "Section Header의 `State`/`Show Action` 의미 중복 가능성 검토" 항목이 실제로는 2026-09-04에 이미 해결됐음을 재확인 — §2.6 Section Header 상세 항목엔 "✅ 해결됨(2026-09-04)"으로 정확히 기록돼 있었으나(`Show Action`을 우측 액션 버튼의 `visible`에 재배선, 실사용 0곳이던 `text + button` variant 삭제), 그 결과를 §5 목록과 §2 요약 체크리스트 표(6번 행)에서 지우는 걸 그때 빠뜨렸던 것으로 보인다. 즉 새로 발견한 사실이 아니라, 이미 끝난 작업이 두 곳에 상반되게 기록돼 있던 순수 문서 정합성 버그.

실행: §5 항목에 취소선을 긋고 해결 근거·§2.6 참고 문구를 추가(기존 다른 해결 항목들과 동일한 서식). §2 요약 표 6번 행의 `Show Action` 셀 문구를 "의미 중복 가능성 있어 후속 검토 필요"에서 "✅ 2026-09-04 해결 — 우측 액션 버튼 `visible` 제어로 재배선 완료"로 갱신, `State` 축에도 "값 1개만 남음" 캐비어트를 함께 표기해 §2.6과 정확히 일치시켰다. Figma 변경 없음, design.md 문서만. → `design-system/design.md`(v0.25)

### 2026-09-05 — Figma 파일 페이지 재구성: Assets 패널에서 Components/Icon 분리
사용자가 Wanted 디자인 시스템 파일의 Assets 패널 스크린샷을 보여주며 위계 구조를 질문 — Figma Assets 패널의 최상위 폴더는 실제로 파일의 **Page**이고, 각 폴더 썸네일은 그 페이지 첫 콘텐츠의 자동 미리보기임을 확인해 설명. Wanted의 Theme/Element/Component/Resource 4단 구조를 그대로 따라할 필요는 없다며, 대신 개발자·기획자가 Assets 탭에서 바로 가져다 쓸 수 있도록 **Components / Icon 2개 페이지로만** 나눠달라고 요청.

페이지 이름 표기 방식(기존 `①`/`Ⓝ` 원형 숫자 접두사 컨벤션을 유지할지)을 AskUserQuestion으로 확인 — **접두사 없이 순수 `Components`/`Icon`으로 확정**(이 컨벤션 자체가 Figma 표준이 아니라 사용자가 만든 관례라 문제 없다고 판단).

실행(Figma): 기존 아이콘 글리프 컴포넌트 6개 — `Icon / Status`(`6020:13582`), `Icon / Utility`(`6008:9076`), `Icon / General`(`6294:9847`), `Icon / Checkbox`(`6020:13544`), `Icon / Radio`(`6573:3439`), `Icon / Placeholder Square`(`6202:776`, 유일하게 COMPONENT_SET이 아닌 단일 COMPONENT) — 를 신규 생성한 `Icon` 페이지(`6710:2`)로 `appendChild`해 이동, 가로 한 줄로 재배치. 기존 `① Component` 페이지(`1:9`)는 `Components`로 개명. 나머지 35개 COMPONENT_SET + 8개 독립 COMPONENT(실제 화면 목업 포함)는 `Components` 페이지에 그대로 유지.

검증: 페이지 목록 재조회로 5개 페이지(`Cover`/`⓪ Foundation...`/`Components`/`Ⓝ Note`/`Icon`) 확인, `Icon` 페이지 자식이 정확히 의도한 6개인지, `Components` 페이지의 COMPONENT_SET 수가 40→35(6개 중 5개가 SET)로 정확히 줄었는지 확인. `Icon` 페이지 스크린샷으로 6개 컴포넌트 전부 이동 후에도 정상 렌더링됨을 확인(별도 상태 깨짐 없음). 노드 ID는 페이지 이동으로 바뀌지 않으므로 design.md의 기존 `Figma: node-id` 참조 라인들은 전부 그대로 유효 — design.md는 별도 수정 불필요.

### 2026-09-05 — 아이콘 컴포넌트 재분류(Icon↔Components) + 전체 variant 분리("펼쳐보기")
사용자가 `Icon` 페이지에 있는 아이콘들 중 General/Placeholder Square를 제외한 나머지는 `Components`에 속해야 하지 않냐고 질문. `findAllWithCriteria`로 각 아이콘 세트의 실사용 인스턴스를 전부 추적한 결과:

- `Icon / Utility`(Filter/Star/Default): 실사용 24곳 — `Chip` COMPONENT_SET 자체 마스터(`button > contents 1 > heading > Chips` 내부 기본 아이콘 슬롯)에 내장 + 실제 화면(상품목록 그리드/리스트의 App Bar 필터, Bottom Sheet Interactive 필터 칩) 전반에 사용. **Components로 이동 확정.**
- `Icon / Checkbox`: 실사용 10곳(`Item Card / Cart` Stock=Available/OutOfStock 마스터, `Cart / Selection Toolbar`, 장바구니 화면). **Components로 이동.**
- `Icon / Radio`: 실사용 0곳이나 §2.15에서 Checkbox와 한 세트로 묶여 공식 체크리스트 항목으로 문서화돼 있어 함께 이동.
- `Icon / Status`(Error/Warning): 실사용 0곳. **예상 밖의 발견**: Dialog의 실제 Error/Warning variant 헤더가 참조하는 "icon" 인스턴스(`4043:2709` 등)의 mainComponent를 직접 추적해보니 `Icon / Status`가 아니라 **완전히 무관한 고아 컴포넌트("x-01", `17:1119`, 부모 없음)**였음 — `Icon / Status`는 어떤 실제 컴포넌트에도 통합되지 않은 죽은 아이콘 세트로 확인됨. 사용자에게 이 사실을 그대로 보고한 뒤 **Icon 페이지 잔류로 확정**(General/Placeholder Square와 같은 성격).
- `Icon / General`(실사용 20곳), `Icon / Placeholder Square`(실사용 83곳, 이미 단일 COMPONENT)는 범용 아이콘 소재라는 성격 그대로 Icon 페이지 잔류.

이어서 사용자가 "모든 아이콘이 Assets 패널에서 클릭 없이 펼쳐져 보이면 좋겠다"고 요청 — Figma Assets 패널은 COMPONENT_SET을 한 덩어리로 보여주고 클릭해야 변형이 펼쳐지므로, 각 variant를 Set에서 분리해 독립 COMPONENT로 만들어야 함을 설명. 실사용 137곳 이상이 걸려있어 AskUserQuestion으로 진행 여부 재확인 후 "모든 아이콘 대상으로 진행" 확정.

**실행**: 페이지 재배치(`Icon / Utility`(`6008:9076`)/`Icon / Checkbox`(`6020:13544`)/`Icon / Radio`(`6573:3439`)를 `Icon`→`Components` 이동) 후, 실사용 0곳→많은 순으로 단계적 분리 진행 — Radio(2개, 메커니즘 검증용) → Status(2개) → Checkbox(2개) → General(5개) → Utility(3개, 최고 위험군). 방법: 각 variant COMPONENT를 `appendChild`로 페이지에 직접 재배치하면 자동으로 Set에서 분리되고 독립 컴포넌트가 됨 — `Icon / {세트명} / {값}` 패밀리 네이밍으로 재명명. **예상치 못한 편의 발견**: variant를 전부 빼내 COMPONENT_SET이 빈 껍데기가 되면 Figma가 자동으로 그 SET을 삭제해줌(수동 삭제 불필요).

**검증**: 매 세트 분리 직후 실사용 인스턴스를 재조회·스크린샷 — `Item Card / Cart`(체크박스 정상), `App Bar`의 `Icon / General / Cart`(장바구니 아이콘, 인스턴스 이름도 자동 갱신 확인), `Chip` COMPONENT_SET 전체 스크린샷(드롭다운 화살표+로켓 배지 아이콘 전부 정상, 가장 위험도 높았던 Utility 분리 후에도 무손상) — 총 14개 신규 독립 컴포넌트 생성, 기존 실사용 인스턴스 전부 정상 렌더링 확인, 문제 발생 0건이라 롤백 불필요.

**후속 과제로 등록**: Dialog의 Error/Warning 헤더 아이콘이 정식 `Icon / Status`가 아니라 고아 컴포넌트(`17:1119`, "x-01")를 참조 중인 문제는 이번 작업 범위 밖이라 지금 고치지 않고 발견만 기록 — design.md §5에 신규 항목 추가. → `design-system/design.md`(§5만 갱신, 나머지는 노드 ID 불변이라 문서 변경 불필요)

### 2026-09-05 — design.md §1+§2 문체를 존댓말로 통일(지마켓 GDS 참고)
사용자가 design.md를 훑어보다 문체가 통일돼 있지 않다고 지적, 지마켓 GDS 스타일(존댓말, 짧고 직접적인 문장)을 참고해 일괄 정리해달라고 요청. Explore 서브에이전트로 파일 전체(1183줄)를 실측 감사한 결과, 불일치는 섹션 단위가 아니라 **필드 종류 단위**로 갈려 있었음을 확인 — §2의 각 컴포넌트 Description 문장(1~3문장)은 이미 100% 존댓말이었지만, 바로 아래 사용가이드 Do/Don't 불릿과 Content 불릿, Variants 표의 비고 칸 서술은 전부 평어("~한다")로 남아있어 한 컴포넌트 블록 안에서 문체가 뒤바뀌는 상태였다. §1 Foundation은 존댓말이 0건으로 전부 평어였고, §3 Naming Convention(규칙집)/§4 Design Principles(격언체)/§5 후속과제(백로그 명사구)/변경 이력(커밋로그)은 장르 자체가 달라 존댓말로 바꾸면 오히려 가독성이 떨어진다는 점도 함께 확인.

범위를 AskUserQuestion으로 확인한 결과 **§1 Foundation + §2 Component만** 존댓말로 통일하고(§3/§4/§5/변경이력·§2 요약 체크리스트 표는 현행 유지), 부수적으로 발견된 "바텀시트"/"다이얼로그"/"체크박스"(§2.15 자기 Description 안에서 컴포넌트 이름 대신 쓰인 경우) 같은 한글화 표기 불일치도 나머지 30여 개 컴포넌트와 동일하게 백틱 영문(`Bottom Sheet`/`Dialog`/`Icon / Checkbox`)으로 정규화하기로 확정.

실행: §1.1~1.7, §2.1~2.15를 순서대로 훑으며 평어 문장 종결(`~한다`/`~이다`/`~았다` 등)을 존댓말(`~합니다`/`~입니다`/`~했습니다`)로 전환. 이미 존댓말인 Description 문장, "실사용 49곳"류 명사형 라벨은 손대지 않아 문장을 늘리지 않았다. ✅/⚠️ 태그·날짜·node ID·"실사용 N곳" 등은 전부 원문 그대로 보존.

**검증**: 재작성 전/후로 node ID(정규식 `[0-9]+:[0-9]+`) 50개를 diff해 완전히 일치함을 확인, "실사용"(75건)·✅(60건)·⚠️(19건)·`2026-09-XX` 날짜(103건) 카운트도 전부 편집 전후 동일함을 확인해 정보 손실 0건임을 검증했다. "바텀시트"는 7→2(남은 2건은 모두 손대지 않은 변경 이력 표 안), "다이얼로그"는 1→0, "체크박스"는 §2.15 지적 사례만 정규화하고 나머지 2건(Item Card/Cart의 실제 체크박스 UI를 가리키는 일반명사 용법)은 그대로 남겼다 — 원래도 컴포넌트 이름 오용이 아니었기 때문. §3 `## 3. Naming Convention` 이하 섹션 헤더 줄번호가 편집 전후로 정확히 동일해(라인 삽입·삭제 없이 단어만 치환) §3 이후가 완전히 그대로임을 재확인했다. → `design-system/design.md`(v0.27)

### 2026-09-05 — design.md §1~§5 전체에서 과거 이력 서술 제거, decisions.md를 이력 저장소로 일원화
사용자가 design.md에 "해결된 이슈", "✅ 정정", "바로잡은 사실" 같은 과거 작업 이력이 섞여있는 게 실제 기업 공개 디자인 시스템 문서와 다르다고 지적. 파일을 두 벌(이력 포함/미포함)로 나누는 방안을 검토했으나, decisions.md가 이미 정확히 그 "왜/어떻게 바뀌었는지" 이력을 전담하는 문서로 존재하고, 두 파일을 나란히 유지하면 정합성 드리프트 위험이 구조적으로 두 배가 된다는 점을 근거로 — **design.md는 "현재 상태"만 남기고, decisions.md를 유일한 이력 저장소로 유지**하는 방향을 제안, 사용자 승인.

**분류 원칙**: Explore 에이전트로 design.md 전체(§1~변경이력)를 정밀 감사 — (1) **Type A(제거)**: "언제/어떻게 지금 상태에 도달했는지"를 narrate하는 과거 서술(예: "✅ 정정(2026-09-04): 이전 서술은 틀렸다"). 약 48건 발견, decisions.md에서 표본 16건을 대조한 결과 **예외 없이 전부 이미 더 상세하게 기록돼 있음**을 확인해 삭제해도 정보 손실이 아님을 검증. (2) **Type B(유지)**: "지금도 확정 안 됐다/불확실하다"는 현재 상태 경고(예: `⚠️ 확인 필요`, "실사용 0곳 — 확정 규칙 아님"). 약 30건, 날짜·감사 프레이밍만 벗기고 내용은 유지. (3) **Ambiguous(수술적 분리)**: 한 문장에 A/B가 섞인 경우 약 26건 — A 절은 삭제하고 B 절만 자연스럽게 남김.

**명시적으로 제외한 패턴**(과거 이력 서술과 다른 성격이라 손대지 않음): §2 요약 체크리스트 표의 ✅/❌ 상태 열(단순 완료 여부 표시), 각 컴포넌트의 ✅ Do/❌ Don't 사용가이드 불릿(표준 가이드라인 포맷), `## 변경 이력` changelog 표 전체(소프트웨어 CHANGELOG.md와 동일 성격의 의도된 역사 기록).

**범위**: §1 Foundation, §2 Component(요약 표 포함, 상태/Do·Don't 열 제외), §3 Naming Convention, §4 Design Principles, §5 후속 과제 — 전부 동일 원칙 적용. §5의 이미 해결된 취소선 항목 3개(design-language.md 갱신/Message Box·Toast 매핑/Section Header 중복)는 "아직 없는 것" 목록에 남아있을 이유가 없어 완전히 삭제(§2.6에 이미 현재 상태로 반영돼 있음).

**실행**: §1.1~1.7 → §2.1~2.15 → §2 요약 표 → §3 → §4 → §5 순으로 Type A 문장/괄호구 삭제, Type B는 감사 프레이밍만 제거, Ambiguous는 절 단위로 분리해 재구성. 부수적으로 §2.8 Item Card/Recommendation의 개명 경위, §2.6 Section Header의 Show Action 재배선 경위, §2.13 Tab Group의 재작성 경위 등 이번 세션에서 가장 공들여 썼던 서술들도 전부 같은 원칙으로 정리 — 결론(현재 규칙)만 남기고 "어떻게 발견했는지" 과정은 걷어냈다.

**검증**: 재작성 전/후로 node ID(정규식 `[0-9]+:[0-9]+`) 50→49개(삭제된 1개는 Review/Summary의 순수 조사 경위 서술 안에만 있던 참고 번호로, decisions.md에 이미 기록된 사실이라 손실 아님), "실사용" 언급 75→60건(narrate성 언급만 감소, "실사용 N곳"이라는 현재 사실 자체는 모든 컴포넌트에 보존), `✅ Do:`/`❌ Don't:` 불릿 49개 편집 전후 완전 동일, `## 변경 이력` 표는 diff로 byte 단위까지 완전히 동일함을 확인. §3 이후 섹션에서도 같은 원칙(주로 날짜 괄호 제거 수준)을 적용해 일관성을 맞췄다. → `design-system/design.md`(v0.28)

### 2026-09-05 — design.md에 "새 화면 제작 시 지킬 프로세스 규칙" 섹션 신규 추가
이 프로젝트의 목적이 design.md를 "잘 정리된 문서"가 아니라 "AI가 새 화면을 규칙대로 만들게 하는 도구"로 쓰는 것이라는 사용자 지적에 따라, §1~§5의 실제 내용과는 별개로 "이 문서를 어떻게 적용할지"에 대한 프로세스 규칙 5개(컴포넌트 우선/토큰 전용/컴포지션 우선/모호함 표면화/사후 검증)를 제안했고 사용자가 승인, `## 0. 이 문서 사용 원칙` 섹션으로 신규 반영. → `design-system/design.md`(v0.29)

### 2026-09-05 — 남은 별표 논의 항목 전부 배치 실행(Message Box 신규 제작, 신규 Semantic 토큰, 색상 사용 규칙 확정, Screen Composition Patterns 등)
"design.md가 실제로 새 화면을 규칙대로 만들게 해야 한다"는 목표로 순서대로 논의해온 나머지 별표 항목(1-1, 1-2, 1-3, 1-5, 2-2, 2-3, 4-1, 4-2, 4-3)이 전부 결론이 나서, 사용자가 정한 방침대로 한 번에 배치 실행했다.

**(a) Message Box 신규 제작(Figma 실행)**: 사용자가 공유한 참고 이미지(연핑크/연노랑 카드+원형 정보 아이콘+텍스트, 닫기 버튼 없음)를 근거로 `Message Box` COMPONENT_SET(`6735:3455`) 신규 생성 — `Status`(Error/Warning) 2 variant, 각 328px 폭 auto-layout 카드(radius 8)에 원형 "i" 아이콘(20px)+본문 텍스트. 기존 `Icon / Status`(느낌표 모양, 경고/에러 알림용)는 참고 이미지의 "정보(i)" 개념과 맞지 않아 재사용하지 않고 컴포넌트 내부에 새로 만들었다. 스크린샷으로 참고 이미지와 대조 확인.

**(b) 신규 Semantic 색상 토큰 4개(Figma 실행)**: `Feedback/Error Background`(→`Error/50`)·`Feedback/Error Text`(→`Error/600`)·`Feedback/Warning Background`(→`Warning/50`)·`Feedback/Warning Text`(→`Warning/600`) — 전량 기존 Primitive를 alias, 신규 값 발명 없음. Message Box 배경/아이콘/텍스트에 바인딩.

**(c) 색상 사용 규칙 확정(문서만)**: Dialog/Bottom Sheet dim 오버레이=`Background/Overlay` 50% 불투명도(정적 컴포넌트에 스크림 레이어가 없어 Toast 규칙과 같은 성격의 개발 구현 규칙으로 문서화), `Feedback/Success`가 실사용에서 파랑으로 렌더링되는 현재 동작을 공식 규칙으로 확정.

**(d) Grid 4컬럼 vs `Item Card/Grid` 160px 정합성(문서만)**: 컬럼폭 `(360-32-24)/4=76px` 계산을 근거로, 160px 카드가 4컬럼 중 2컬럼을 내부 거터(8px)로 묶은 것(`76×2+8=160`)임을 §1.4에 명문화 — 두 규칙이 모순이 아니라 같은 그리드를 다르게 묶어 쓴 것.

**(e) Label/Chip/Rocket Badge/Status Badge/Spec Row 비교표(문서만)**: 이미 전용 도메인·전용 슬롯이 있는 컴포넌트(배송=Rocket Badge/Spec Row, 리뷰 신뢰=Status Badge, 필터링=Chip)는 그대로 두고, 그 외 신규 태그는 `Label`에 `Type` 값을 추가하는 걸 기본으로 한다는 기준을 5열 비교표로 §2.9에 추가. `Label`과 `Spec Row`가 "배송/반품" 어휘를 공유하는 건 우연이며 실제 슬롯(독립 태그 vs Item Card 내장 로우)이 다름을 명시.

**(f) Variant vs Boolean 선택 기준(문서만)**: 값 2개+"요소 하나 visible on/off"만 다르면 Boolean, 3개 이상이거나 색상/아이콘/텍스트/레이아웃이 함께 바뀌면 Variant — 기존 실제 사례(Show Action류 vs Dialog Status/Button 등)로 검증된 기준을 §3에 명문화. Variant→Boolean 전환 후 축에 값 1개 남는 Figma API 제약도 함께 기록.

**(g) 긴 텍스트 넘침 처리 기본 원칙(문서만)**: 한 줄 자유 텍스트 슬롯은 기본 말줄임(`List`의 `history`, `Option Chips/Thumbnail` 실사례 근거), 여러 줄 슬롯은 자동 줄바꿈+가능하면 최대 줄 수 명시 — §4 원칙 11번으로 추가(8개→11개).

**(h) Screen Composition Patterns 신규 섹션(Figma 실행 1건 + 문서)**: 실제 화면 5개("UI" 섹션, `6294:10390`)를 재조회해 목록형/스크롤형/사이드바형 3패턴 + 공통 규칙을 도출, `## 6. Screen Composition Patterns`로 §5와 변경 이력 사이에 추가(기존 §1~§5 번호 유지, 재넘버링 안 함). 조사 중 5개 화면 중 `Category` 화면만 `Status Bar`가 32px로 다른 4개(44px)와 다른 걸 발견 — iOS 표준(44px)/Android 표준(24px) 실측값을 사용자에게 제시한 뒤, 이 프로젝트가 이미 단일 브레이크포인트 방침(Grid, 2026-09-01)을 쓰고 있다는 점을 근거로 44px 단일값 통일을 사용자가 확정. `Category` 화면의 `Status Bar`(`6544:43012`)를 32→44px로 수정(auto-layout 부모라 하위 요소는 자동으로 재배치됨), 스크린샷으로 레이아웃 정상 확인.

이번 배치로 Component Checklist는 13/15→14/15(Message Box 완료)가 됐고, §5 후속 과제에서 이미 결론 난 3항목(Message Box, App Bar Type 재검토, 토큰 표기 통일)을 제거했다. → `design-system/design.md`(v0.30)
