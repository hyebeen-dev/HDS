# Figma 작업 완료 기록 — 미착수 컴포넌트 체크리스트 구역 신설

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)

---

## 배경

기존 "to make" 메모(파일 내 원본 디자이너 메모)는 프로젝트 초기에 작성되어 이미 완료된 항목(Price Block, Spec Row, Rating Display 등)과 미완료 항목이 구분 없이 섞여 있었다. PRD 기반 Tier 1 점검(직전 대화)에서 확인한 최신 상태를 반영해, **아직 만들지 않은 컴포넌트만** 체크박스 형태로 추적할 수 있는 구역을 💠 Components 페이지에 새로 만들었다.

## 실행 내용

"📋 Component Checklist — 미착수" 구역을 기존 "to make" 메모 바로 아래에 배치. Tier 1/2/3 3개 블록, 총 18개 항목. 각 항목은 체크박스 + 컴포넌트 이름 + PRD 요구사항 ID(근거)로 구성했다.

- **Tier 1 (P0 필수, 3개)**: Section Header, App Bar Sticky, Action Bar
- **Tier 2 (있으면 좋음, 11개)**: Ad Container, Reason Label, Ratio Display, Confidence Indicator, Review Summary, Review Card(정식화 필요 — 현재 raw 목업만 존재), Evidence Chip, Anchor Navigation, Option Selector, Accordion, Chip/Filter(App Bar 내 chip이 유사 기능 수행 중이라 분리 검토 필요로 별도 표기)
- **Tier 3 (여유 시, 4개)**: Empty State, Skeleton, Carousel + Indicator, Icon Button

## 실행 중 발견한 것

Figma 파일에 저장된 페이지 이름이 API 조회 시 "💠 Components"가 아니라 "ㄴ Components"로 반환되는 인코딩 이슈를 발견했다(이모지가 다른 문자로 치환됨). 페이지를 이름으로 찾는 스크립트가 실패해, 이후 페이지 ID(`1:9`)로 직접 접근하도록 수정했다 — 향후 이 페이지를 다룰 때도 이름 매칭 대신 ID 사용을 권장한다.

## 검증

스크린샷으로 18개 항목 전체가 체크박스·이름·PRD 근거와 함께 올바르게 렌더링되는지 확인.

## 업데이트 — Tier 1/2/3 구조를 플랫 15개 항목으로 전면 교체 (2026-09-03)

사용자가 기존 Tier 1/2/3(P0/있으면 좋음/여유 시) 체계 대신, 다음 15개 항목으로 체크리스트를 완전히 바꿔달라고 요청: badge / banner / button / chip / dialog / heading(=app bar, section header, cart utility) / message box / item card(, review card) / label / list / navigation / bottom sheet / tab / thumbnail / radio·checkbox.

**완료 기준**: 사용자가 명시적으로 확정 — "컴포넌트가 만들어져 있으면 완료"(실사용 인스턴스 개수와 무관, 0건이어도 완료로 취급).

**모호한 항목에 대한 사용자 확인**:
- **list** = "텍스트 기반 선택 요소 정보를 섹션 또는 그룹으로 나눌 수 있는 연속적인 수직 집합체. 대량의 정보를 목록 형태로 깨끗하고 효율적으로 정리(예: 상품 카테고리 전체보기 - 커머스)" — 범용 세로 리스트 패턴. 파일 내 어디에도 이 형태의 독립 컴포넌트 없음 → **미완료**
- **navigation** = "하단 내비게이션바(앱 전반 화면 전환)" — Bottom Navigation Bar. 파일 내 존재하지 않음 → **미완료**
- **thumbnail** = 범용 이미지 썸네일 컴포넌트를 별도로 의미(= Option Chips의 상품 이미지와는 다른 것). 독립 컴포넌트 없음 → **미완료**

**최종 15개 항목 판정 (2026-09-03 작성 시점: 11 완료 / 4 미완료 → 2026-09-04 감사로 List·Navigation·Thumbnail 재발견 반영, 14 완료 / 1 미완료)**:

| 항목 | 상태 | 근거 |
|---|---|---|
| Badge | ✅ | roket_badge, status-badge, Cart/Countdown Badge |
| Banner | ✅ | Cart/Notification Banner |
| Button | ✅ | Button-large(CTA) + Icon Button |
| Chip | ✅ | Filter Chips + Option Chips(Size/Thumbnail) |
| Dialog | ✅ | dialog 컴포넌트 세트 |
| Heading (=App Bar, Section Header, Cart Utility) | ✅ | App Bar, App Bar Small, Section Header, Cart 전용 컴포넌트 |
| Message Box | ✅(추정) | Toast로 매핑했으나 사용자에게 직접 확인받지 않은 추론 — 재확인 필요 |
| Item Card(, Review Card) | ✅ | Item Card 계열 + Review Card/Summary |
| Label | ✅ | Info Label |
| List | ✅ | `list`(`6527:40035`), 실사용 7곳 — 2026-09-04 감사 중 재발견해 정정(`diagnosis/design-system-audit-2026-09-04.md`) |
| Navigation | ✅ | `bottom_navigation`, 실사용 2곳 — 2026-09-04 정정 |
| Bottom Sheet | ✅ | Sheet/Confirmation + Sheet/Selector |
| Tab | ✅ | tab 컴포넌트 세트 |
| Thumbnail | ✅ | `thumbnail`(`Size=small/medium/large/Xlarge`), 실사용 10곳 — 2026-09-04 정정 |
| Radio / Checkbox | ❌ | radio(`6474:33379`)는 2026-09-04에 Default/Selected 2-variant로 이름 정리 완료(실제로는 처음부터 진짜 다른 상태였음), 단 실사용 0곳. Checkbox는 여전히 컴포넌트 자체가 존재하지 않아 항목 전체는 미완료 |

**Figma 반영**: 기존 Tier 1/2/3 카드 3개(`6232:1171`, `6144:790`, `6144:847`)를 삭제하고, 위 15개 항목을 하나의 플랫 리스트 카드로 교체. 제목을 "📋 Component Checklist"로, 설명 텍스트를 완료 기준 문구로 변경. 체크박스 스타일(초록 채움=완료 / 회색 1.5px 스트로크=미완료)은 기존 Tier 카드에서 쓰던 스타일 그대로 재사용.

**후속 확인 필요 항목**:
1. "Message Box" → Toast 매핑이 맞는지 사용자 확인
2. Radio 컴포넌트의 이름 중복(×2, 둘 다 "Property 1=Default") 정리 + Selected variant 추가는 이번 작업 범위 밖 — 별도 요청 시 처리
3. Checkbox 컴포넌트는 아직 미착수

## 다음 단계 후보

각 컴포넌트를 완성할 때마다 이 체크리스트의 체크박스를 채우거나 항목을 갱신해 최신 상태로 유지.

## 변경 이력

| 일자 | 내용 |
|---|---|
| 2026-09-01 | 미착수 컴포넌트 체크리스트 구역 신설 (Tier 1/2/3, 18개 항목) |
| 2026-09-03 | Tier 1/2/3 구조를 사용자 확정 플랫 15개 항목으로 전면 교체, 11개 완료/4개 미완료 판정 |
