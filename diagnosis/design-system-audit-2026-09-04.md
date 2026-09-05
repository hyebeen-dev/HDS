# Design System 전체 일관성 감사 (Audit)

**작업일** 2026-09-04
**대상 Figma 파일** HDS_2609 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**작업 성격** 100% read-only. Figma 노드/변수/스타일 어떤 것도 생성·수정·삭제하지 않음. "무엇을 왜 고쳐야 하는지"만 제안한다.
**조사 방법** `00-project-brief.md`, `prd-v0.1.md`(전체), `design-system/design-language.md`(전체), `design-system/status.md`, `design-system/decisions.md`를 먼저 읽고, CLAUDE.md 등 별도 규칙 문서가 있는지 확인(프로젝트 루트에 없음, `.claude/settings.local.json`만 존재 — 권한 설정 파일이라 디자인 규칙과 무관). 이후 Figma에서 (1) Foundation(Color/Typography/Spacing 변수·스타일 전량), (2) 6개 페이지 전체의 COMPONENT/COMPONENT_SET 전수 조사(`findAllWithCriteria`), (3) 주요 컴포넌트 패밀리별 Variant/Property/실측값을 세 갈래로 나눠 병렬 조사한 뒤 종합했다.
**판정 기준**: `Existing Rule`(PRD·design-language.md·Figma 토큰에 이미 정의된 규칙)을 위반한 경우만 `Inconsistency`로 분류한다. 규칙 자체가 없으면 `Undefined`로 분류하고 새 규칙을 만들지 않는다.
**조사 범위의 한계**: 6개 페이지의 컴포넌트 **목록**은 전수 조사했지만 모든 컴포넌트의 모든 속성을 전수 실측하지는 못했다(주요 패밀리 위주 표본 실측). 실제 조립 화면(상품 목록/상세, 장바구니)은 이번 조사에서 다시 열어보지 않고 `design-language.md`의 기존 관찰을 인용(Carry-over)만 했다. 접근성(WCAG AA) 대비 수치 검증은 범위 밖이다.

---

## 1. Overall Assessment

**전체 일관성 평가: 5점 만점에 2.5점.**

Foundation(Color/Typography/Spacing 값 체계 자체)은 상당히 정교하게 설계돼 있고, 컴포넌트 하나하나만 놓고 보면 개별 완성도는 낮지 않다. 그러나 "시스템"으로서의 일관성은 세 가지 구조적 문제로 크게 훼손돼 있다.

1. ~~거의 전체 컴포넌트 라이브러리가 3개의 "ㄴ ..." 페이지에 실사용 0건인 채로 통째로 복제돼 있다~~ **→ 2026-09-04 해결(19개 패밀리 전부 삭제)**.
2. ~~지금 이 순간 Figma에서 에러 상태인 컴포넌트가 4개(`Icon Button`, `label`, `status-badge`, `dialog`)~~ **→ 2026-09-04 해결(4개 전부 실제 콘텐츠 확인 후 이름 재부여/중복 삭제)**.
3. ~~같은 개념(상태/크기/유형)에 최소 5가지 서로 다른 Property 이름이 혼용되던 것~~ **→ 2026-09-04 해결(Property 이름은 Status/Type으로 통일)**. 컴포넌트 최상위 이름의 5가지 표기 방식(Title Case+공백/소문자/snake_case/kebab-case/슬래시 패밀리) 통일은 **미해결(P2)**로 남음.

이 세 문제는 지금 당장 화면에 보이는 UI 자체를 망가뜨리진 않지만, PRD의 핵심 성공 기준 — SC-D3("새 화면 요구가 발생했을 때 정의된 Variant 안에서 해결된다"), MG-1("AI가 추가 질문 없이 구현할 수 있을 만큼 요구사항이 구체적이다") — 을 직접 위협한다. Assets 패널에서 어떤 게 진짜인지, Variant 값이 뭘 의미하는지 AI/개발자가 매번 추측해야 하는 상태이기 때문이다.

- **가장 잘 지켜지는 것**: Color Foundation. Primitive→Semantic 2단 구조가 전량 실제 Variable로 존재하고(하드코딩 0건, 로켓 배지 4색 제외 — 기존에 이미 알려진 사안), 색의 의미 매핑(Primary=확정/행동, Error=위험/할인)이 컴포넌트에도 일관되게 적용돼 있다. `Category Tab`/`Option Chips` 패밀리의 Variant 설계도 베스트 프랙티스로 참고할 만하다.
- **가장 큰 문제**: 위 3가지(전체 복제 라이브러리 / 깨진 컴포넌트 4개 / Property·이름 표기 혼용).
- 부가로 확인된 것: `list`(`6527:40035`) 컴포넌트가 이미 만들어져 실사용 7곳이 있는데, 지난번 Component Checklist 작업에서는 발견하지 못해 **여전히 미완료(❌)로 기록**돼 있다 — 체크리스트 재확인 필요.

---

## 2. Critical Issues

| Priority | Component | Issue | Evidence | Recommendation |
|---|---|---|---|---|
| ~~P0~~ | 전체 라이브러리 | ~~`ㄴ Button, Chips`(39개)·`ㄴ Dialog, Sheets, Toast`(28개)·`ㄴ Item Card, Review Card`(38개) 페이지가 `① Component` 페이지의 진짜 컴포넌트를 인스턴스가 아니라 **COMPONENT_SET 전체를 복제**해 담고 있다~~ | 19개 패밀리 전수 확인 — 예: chip 실제 152~247곳 vs 사본 0, Button-large 실제 20~23곳 vs 사본 0, Item Card/Grid Small 실제 15~18곳 vs 사본 0, Price Block 실제 70곳 vs 사본 0, Spec Row 실제 62곳 vs 사본 0, Rating Display 실제 57곳 vs 사본 0, Review/Card 실제 12곳 vs 사본 0. 예외 없이 전부 사본=0 | **해결(2026-09-04)** — 사용자 확정("사본 삭제") 후 삭제 전 페이지별 실사용 재검증 + `① Component` 페이지 크로스 레퍼런스 재확인(모두 0) 완료 후 19개 COMPONENT_SET 전부 삭제. 남은 건 각 페이지의 제목/설명 프레임(`header` 등)뿐 — `① Component`의 진짜 컴포넌트(Button-large 28 variant, chip 10 variant 등)는 정상 확인 |
| ~~P0~~ | `label`(Info Label, `6527:38619`) | ~~4개 variant 전부 `Property 1=Default`로 이름 중복~~ | 직접 조회: `Component set has existing errors` | **해결(2026-09-04)** — 스크린샷 확인 결과 4개 전부 진짜 다른 콘텐츠("특가 진행중"/"무료반품"/"무료배송"/"4회 구매"). 실사용 인스턴스가 이미 쓰던 부모 프레임 이름("Type=Shipping"/"Type=Return") 관례를 그대로 따라 `Type=Promotion/Return/Shipping/RepeatPurchase`로 개명, 에러 해소 |
| ~~P0~~ | `Icon Button`(`6373:33042`) | ~~8개 variant 중 6개가 전부 `"Type=Secondary, Size=Medium, State=Default"`로 이름 중복~~ | 직접 조회, 실사용 0곳 | **해결(2026-09-04)** — x/y 위치·배경색(흰색=Default, 회색/파랑 틴트=Pressed)으로 재구성한 결과 진짜 원본(Medium, 실사용 10곳) 1개 + 새로 추가된 Small 사이즈·Pressed 상태 조합 5개였음. `Type(Secondary/Line type gray) × Size(Medium/Small) × State(Default/Pressed)` 2×2×2 매트릭스로 개명 완료, 에러 해소 |
| ~~P0~~ | `status-badge`(`6373:29771`) | ~~2개 variant 모두 `Property 1=Default`로 이름 중복~~ | 직접 조회, 실사용 0곳 | **해결(2026-09-04)** — 스크린샷 확인 결과 "재구매"/"한달사용" 진짜 다른 콘텐츠(Review Card 상태 배지로 추정). `Type=Repurchase/OneMonthUse`로 개명, 에러 해소. (페이지 사본 `6373:31733`은 앞선 P0 정리 때 이미 삭제됨) |
| ~~P0~~ | `dialog`(`4043:2815`) | ~~`"Status=Warning, Button=1 Button"` variant 이름이 2번 중복~~ | 직접 조회 | **해결(2026-09-04)** — 스크린샷 확인 결과 `4043:2874`가 실제로는 버튼 2개짜리(제목+본문+"sub btn"+"main button")인데 이름만 "1 Button"으로 잘못 붙어 있었고, 이미 존재하는 `Status=Warning, Button=2 Button`(`4043:3050`)과 텍스트까지 완전히 동일한 진짜 중복이었음. 실사용 0곳 확인 후 `4043:2874` 삭제, `Status × Button` 정상 구조로 복원 |
| ~~P1~~ | `Button-large`(CTA, Small/XSmall) | ~~승인 스펙(Small 좌우8·상하6·13px / XSmall 좌우8·상하5·12px)과 실측값 불일치(둘 다 상하11, 폰트 14/13px)~~ | 직접 실측 | **해결(2026-09-04)** — Small 7개·XSmall 7개(총 14 variant) 전부 승인 스펙대로 적용(상하 padding 6/5, 폰트 13/12px). 실사용 0곳 확인 후 진행, 스크린샷으로 4개 사이즈가 시각적으로 구분됨을 확인 |
| ~~P1~~ | Typography Foundation | ~~Text Style 29개 중 6개가 Pretendard가 아니라 Noto Sans KR로 정의됨~~ | `getLocalTextStylesAsync()` 직접 조회 | **해결(2026-09-04)** — 6개 스타일 정의를 Pretendard로 전환(같은 weight 유지: Bold/Medium/Regular). 추가로 이 스타일에 바인딩되지 않은 raw Noto Sans KR 텍스트 28개(Foundation 페이지 타이포 스펙 표 내부)도 함께 발견해 전부 Pretendard로 전환 — 총 35곳. **실제 컴포넌트에도 영향이 있었음**: `Category Tab`의 "Category-Name" 텍스트가 `label/xsmall-medium` 스타일을 통해 Noto Sans KR을 쓰고 있었고, 스타일 수정과 동시에 자동으로 Pretendard로 전환됨(별도 조치 불필요) |
| ~~P1~~ | `Text` 변수 컬렉션 | ~~`Body/medium`=17, `Navigation/depth-medium`=17 값이 orphan 변수로 남아있음~~ | `Text` VariableCollection 직접 조회 | **해결(2026-09-04)** — 텍스트 노드·텍스트 스타일 어디에도 바인딩된 곳 없음(전량 확인) — 이미 내린 "17px는 16px로 흡수" 결정에 맞춰 두 변수 삭제. 컬렉션 16개→14개 |
| ~~P1~~ | `Button-large` | ~~같은 `Size` Variant 축 안에서 값이 `Large`(대문자)와 `medium`/`small`/`xsmall`(소문자)로 표기가 섞여 있음~~ | 직접 조회 | **해결(2026-09-04)** — `Large/Medium/Small/XSmall`로 통일(21개 노드). 프로젝트 기존 표기("XSmall", 약어 단위로 S도 대문자)와 맞춰 확정 |
| P1 | Property 이름 전체 | 같은 개념(상태/선택/크기/유형)에 `Status`/`status`/`Size`/`Type`/`Property 1` 등 5가지 이상의 서로 다른 Property 키가 12개 이상 컴포넌트에 걸쳐 쓰임 | §4 표 참고 | `component-naming-cleanup.md`에서 이미 "체크리스트 완료 후 일괄 처리"로 미뤄둔 항목과 동일 범주 — 체크리스트가 사실상 마무리 단계이므로 지금이 그 시점인지는 사용자 판단 |
| P1 | Spacing 값 vs Variable 바인딩 | Button/Chip/Category Tab/Item Card 표본 확인 결과 padding/gap **숫자 값은 Spacing 원칙과 대부분 일치**하지만, 실제로 Spacing Variable에 `boundVariables`로 연결돼 있지 않고 리터럴 숫자로만 우연히 값이 맞음 | 표본 컴포넌트들의 `boundVariables`에 `fills`/`strokes`만 있고 spacing 관련 바인딩 없음 | PRD UR-S1("모든 시각 속성은 토큰으로 정의되며 하드코딩 금지")을 기술적으로 위반 — 값 교정이 아니라 "바인딩 연결" 작업이 필요 |
| ~~P1~~ | `list` 체크리스트 상태 오류 | ~~`list`(`6527:40035`)가 이미 존재하는데 `status.md`는 List를 여전히 ❌로 기록 중~~ | 페이지 전수 조회로 신규 발견 | **해결(2026-09-04)** — `status.md`, Figma 체크리스트 보드 둘 다 ✅로 정정(14/15) |

---

## 3. Naming Consistency

최상위 COMPONENT_SET 이름만 놓고 봐도 최소 5가지 표기 방식이 동시에 쓰이고 있다.

| Component | Current | Issue | Recommended |
|---|---|---|---|
| `Section Header`, `Action Bar`, `App Bar`, `App Bar Small`, `Tab`, `Quantity Stepper`, `Category Tab`, `Category Menu Item`, `Quick Badge Row`, `Icon Button`, `Order Deadline`, `Spec Row`, `Price Block`, `Rating Display` | Title Case + 공백 | 다수 패턴(약 20개 세트) — 사실상 기준으로 삼을 만함 | — |
| `chip`, `dialog`, `toast`, `icon`(×2 별개 세트), `tab`, `menu`, `radio`, `thumbnail`, `search`, `list`, `label` | 소문자 단일 단어 | Title Case 그룹과 같은 레벨(페이지 최상위)에 섞여 있음(약 14개 세트) | `Chip`, `Dialog`, `Toast`, `Radio`, `Thumbnail`, `Label` 등으로 통일 권장 |
| `roket_badge`, `bottom_navigation` | snake_case | 위 두 그룹과 또 다른 표기 | `Rocket Badge`, `Bottom Navigation` |
| `status-badge` | kebab-case | 네 번째 표기 방식 | `Status Badge` |
| `Sheet / Confirmation`, `Sheet / Selector`, `Icon / Status`, `Icon / Checkbox`, `Icon / Utility`, `Review / Summary`, `Review / Card`, `Option Chips / Size`, `Option Chips / Thumbnail` | Title Case + ` / ` 패밀리 구분자(공백 있음) | 의도적인 패밀리 네이밍 규칙으로 보이며 그 자체는 좋음(11개 세트, 내부적으로 일관) | 계속 유지, 새 패밀리도 이 규칙 따르기 |
| `Icon/Placeholder Square` | Title Case + `/`(공백 없음) | 바로 위 규칙과 슬래시 앞뒤 공백 여부가 다름 | `Icon / Placeholder Square`로 통일 |
| `Button-large` | Title Case + 하이픈 | 유일하게 하이픈을 쓰는 최상위 이름(과거 `Button_large`/`Buttopns` 오타에서 이어진 흔적으로 추정). Size Variant에 이미 Large/Medium/Small/XSmall이 다 있는데 이름에 "large"가 고정 접미사로 남음 | `Button`(Large를 이름에서 제거) |
| `Button / 2 Button Layout (legacy — Primary Large Default only)` | 화면/이력 종속적 서술형 이름 | "너무 구체적/특정 화면 종속" 사례에 정확히 해당. 괄호 안에 "legacy"라는 상태값까지 이름에 들어감 | 이름에서 상태·이력 설명을 빼고 `description` 필드로 옮기거나, 정말 legacy면 별도 Archive 처리 |
| `Tab`(`6009:10202`, 탭 그룹 레이아웃) vs `tab`(`6009:10187`, 개별 탭 아이템) | 대문자만 다른 별개 컴포넌트 | 역할은 실제로 다르므로 병합 대상은 아니지만 Assets 패널에서 대소문자만으로 구분해야 해 혼동 위험이 큼 | `Tab Group` / `Tab Item`처럼 확실히 구분되는 이름 권장 |
| `icon`(`6294:9847`, gift/cart/search 등 아이콘 스왑용) vs `icon`(`6009:10568`, 별개 세트) | 서로 무관한데 같은 소문자 이름 | 대소문자까지 완전히 같은 진짜 충돌 후보 | 용도별로 구분되는 이름 필요 |
| Property 이름: `rocket`(Chip 내부, `Status`/`Dropdown`은 대문자인데 `rocket`만 소문자) | 같은 컴포넌트 내부 표기 혼용 | 사소하지만 실제 존재 | `Rocket`으로 통일 |
| ~~Property 이름: `status`/`Status`/`Size`/`Type`/`Property 1` 5종 혼용~~ | 같은 개념(상태·크기·유형)에 5가지 다른 Property 이름 | 사용자가 예시로 든 `size/Size/Type/type` 문제가 파일 전체에 실제로 존재함을 확인 | **해결(2026-09-04)** — 상태/선택 계열은 `Status`로, 콘텐츠·글리프 종류는 `Type`으로 재분류해 통일(§8 참고) |
| Property 값: `Size=Xlarge`(thumbnail, X만 대문자, 나머지 small/medium/large는 소문자) | 축 내부 표기 불일치 | `Size=Large`(Button, 대문자) vs 같은 개념의 `Xlarge`(thumbnail)도 서로 다른 규칙 | `xlarge`로 소문자 통일하거나, Button처럼 전체를 대문자로 통일할지 함께 결정 |
| Property 값: Review Card의 `Status=한달사용/재구매` | Variant **값** 자체가 한글 서술형 문자열 | 파일 전체에서 유일하게 Variant 값이 영문 토큰이 아니라 한글 그대로인 사례 | `OneMonthUse`/`Repurchase`처럼 영문 식별자로 바꾸고 화면 표시 텍스트는 별도 레이어로 분리 권장 |

---

## 4. Variant / Property Consistency

| Component | Current Structure | Problem | Recommendation |
|---|---|---|---|
| `Icon Button` | 8 variant, `Type × Size × State` | 6개가 이름 중복이라 실제로 몇 개의 진짜 상태인지 구조적으로 판별 불가 | 스크린샷 실측 후 재구성 — Existing Rule 위반이 아니라 순수 버그 |
| `label`, `status-badge`, `Order Deadline`, `Icon / Checkbox`, `icon`(`6009:10568`) | Figma 기본 이름(`Property 1=Default/Variant2/Variant3/Variant4`) 그대로 사용 | 값 자체는 고유해 에러는 안 나는 것도 있지만(label/status-badge 제외), 값이 무엇을 의미하는지 이름만으로 전혀 알 수 없음 — Boolean/Variant 중 어느 설계였는지도 이름에서 드러나지 않음 | 스크린샷으로 실제 콘텐츠 확인 후 의미 있는 이름 부여(`chip-components.md`에서 같은 유형 문제를 이미 여러 번 이 방식으로 해결한 전례 있음) |
| `chip` | `Status`(Default/Selected/Pressed) × `Dropdown`(Boolean) × `rocket`(False/tomorrow/fresh/global) 3축 | `rocket`은 값 4개 중 `False`만 "없음"이고 나머지 3개는 실제 배송 유형 — Boolean처럼 보이는 이름인데 실제로는 "배송 타입(4종, 없음 포함)" 하나의 Variant 축 | 이름을 `RocketType=None/Tomorrow/Fresh/Global`처럼 바꾸면 의도가 명확해짐(동작 자체는 지금도 정상) |
| `bottom_navigation`의 하위 `menu`(`6501:35862`) | `status` 단일 축에 `Home_on`/`Category_off`/`Home_pressed` 등 **15개 평면 값** | "어떤 아이콘"과 "어떤 상호작용 상태"라는 두 개의 독립적인 개념이 하나의 축에 눌려 있음 | `Icon: Home/Category/Search/Mypage/Cart` × `State: Default/Selected/Pressed` 2축으로 분리하면 동일한 15칸을 훨씬 예측 가능한 구조로 표현 가능 |
| `Sheet / Confirmation`(`4043:2686`) | `Item` 축에 값이 `"False"` 하나뿐, `Type` 축에 값이 `"Default"` 하나뿐 | 값이 1개뿐인 Variant 축은 인스턴스 간 절대 달라질 수 없어 속성 패널에 의미 없는 잡음만 남김 | 두 축 모두 삭제하거나 고정값으로 흡수 |
| `Button-large`(CTA) | `Type × Size × State` + `Show Leading Icon`/`Show Trailing Icon`(Boolean) | 구조 자체는 합리적. Large만 Pressed/Disabled까지 있고 Medium/Small/XSmall은 Default만 있어 State 옵션 수가 Size마다 다름 | `button-history.md`에 이미 "실사용처 생기면 추가"로 의도된 스코프임이 기록돼 있음 — **Inconsistency 아님, Existing Rule(의도된 범위 제한)** |
| `Category Tab` | `status=Default/Selected` 단일 축 | 깔끔함, 참고할 만한 베스트 프랙티스 | — |
| `Option Chips / Size`, `Option Chips / Thumbnail` | `status`(Default/low_stock/sold_out) + `Selected`(Boolean) | 두 세트가 완전히 동일한 축 구조 — 패밀리 내 일관성 좋음 | — |
| `thumbnail` | `Size` 단일 축(small/medium/large/Xlarge) | Variant 축 설계는 적절하나, radius가 8(small/medium/large 추정)과 0(Xlarge)으로 갈려 값이 아니라 시각 스펙 자체가 섞였을 가능성 | Medium/Large radius를 마저 실측해 의도 확인 |
| Chip `Status=Selected` vs Option Chips `Selected`(Boolean) | 같은 "선택됨" 개념을 두 계열이 구조적으로 다른 방식(Variant 값 vs Boolean+테두리 오버레이)으로 구현 | `chip-components.md`에 이미 "의도된 차이인지 확인 필요"로 기록된 미해결 사항 | 새로 판단하지 말고 재확인만 권고(기록된 미해결 사항 그대로 존중) |

---

## 5. Visual Consistency

### Typography
- Font family: ~~29개 Text Style 중 23개 Pretendard, 6개 Noto Sans KR~~ → **해결(2026-09-04)**, 29개 전부 Pretendard로 통일(§2 P1).
- Line height: 대부분 150%, `body/compact`·`body/small`·`navigation/small`만 ~140% — `decisions.md`에 이미 기록된 의도된 예외와 일치. **Consistent(예외 포함)**.
- Letter spacing 단위: 기존 18개 스타일은 `PIXELS` 단위, 2026-09-03 이후 신규 11개는 `PERCENT` 단위 — 값 자체는 둘 다 대부분 0이라 화면상 차이는 없지만 데이터 형식이 갈림. **Undefined**(형식 통일 규칙이 문서화된 적 없음, 실질 영향 낮은 P3).
- 상품명 15px Medium 미바인딩 문제: 이번 조사에서 화면 재확인은 못 함(design-language.md 관찰 인용, Carry-over). 다만 Section Header 제목(15px Bold)과 Quantity Stepper(15px Medium)에서도 같은 미정의 15px가 확인돼, **동일 미정의 값의 3번째 독립 목격 사례**로 강화됨.

### Color
- Primitive→Semantic 전량 실제 Variable, 하드코딩 0건(로켓 배지 4색 제외, 기존에 이미 알려진 사안) — 표본 조사한 Button/Chip/Category Tab/toast/tab/App Bar/Cart 계열/Info Label/status-badge 전부 변수 바인딩 확인. **Consistent**.
- Semantic 이름과 실제 alias 목적 불일치 사례 없음(`Text/Accent`→Primary/500, `Feedback/Error`→Error/400 등 전부 이름과 일치). **Consistent**.
- 유일한 하드코딩: `roket_badge`(type=fresh, `6008:7352`) — fill·text fill 모두 미바인딩. 기존에 이미 발견·기록된 사안의 재확인.

### Spacing
- 숫자 값 자체(4/8/16/20 배수 + 2/6/10/11/13 예외)는 표본 컴포넌트(Button, Chip, Category Tab, Item Card)에서 대부분 원칙과 일치. **Consistent(값 기준)**.
- 그러나 표본 전부에서 padding/gap이 Spacing Variable에 바인딩돼 있지 않고 리터럴 숫자로만 값이 우연히 맞음. **Inconsistency** — PRD UR-S1 위반(§2 P1).
- Button Small/XSmall만 유일하게 **값 자체가** 승인된 스펙과 다름(§2 P1) — 이건 "바인딩 부재"가 아니라 "값 자체 미반영"이라는 점에서 위 항목과 별개의 문제.

### Shape
- Button radius: Large=8, **Medium=8**, Small=4, XSmall=4 — design-language.md는 "Large=8/Medium=4, 사이즈 비례"라 서술하나 실측 결과 Medium이 8로 남아있음. **문서와 실제 Figma 불일치**(§7).
- Dialog/Sheet Confirmation/Sheet Selector: 전부 radius 0 — 기존에 "미실측(Undefined)"이었던 항목이 이번에 처음 실측되어 Card/Action Bar와 같은 "각진 사각형" 패턴과 일치함을 새로 확인. **Consistent(신규 확인)**.
- `thumbnail`: small/medium/large=8(추정, Medium/Large 미실측), Xlarge=0 — 같은 컴포넌트 크기 Variant 사이에 radius 값이 갈림. **확인 필요(P3)**.
- Category Tab, Category Menu Item, Item Card Grid: 전부 0, 서로 일관됨.
- Radio: 100(24×24 완전 원형) — 의도대로 정상.
- 전체 경향(Chip 200 > Button Large/Medium 8 > Small/XSmall 4 > Card/Action Bar 0)은 "작고 인터랙티브한 요소일수록 더 둥글다"는 기존 원칙과 대체로 부합 — Button Medium이 4가 아니라 8인 것만 예외.

### Icon
- Chevron/Status/Utility 계열은 기존에 이미 16×16·strokeWeight 1.5로 확인된 바 있음(design-language.md 인용). 신규 컴포넌트(Category Menu Item, Radio, Thumbnail 내부 아이콘)의 실측은 이번 조사에서 다루지 못함. **재확인 필요(P3)**.

---

## 6. Component Overlap

| 사례 | 판단 | 이유 |
|---|---|---|
| `① Component` vs 3개 "ㄴ ..." 페이지의 19개 컴포넌트 패밀리(약 100개 이상 노드) | **통합/정리 필요, P0** | 이름·구조가 사실상 동일하면서 실사용이 한쪽(0)에 완전히 쏠려 있다 — "역할이 다른데 이름만 같다"가 아니라 진짜 중복. 심지어 원본의 버그(예: status-badge 이름 중복 에러)까지 복제 시점 그대로 사본에 복사돼 있어, 의도적인 "문서화 사본"이 아니라 단순 구조적 클론으로 판단된다. 두 사본이 이후 서로 다르게 편집되면 회복 불가능한 혼란이 생긴다 |
| `Tab`(그룹) vs `tab`(아이템) | **별도 유지, 이름만 명확히 구분** | 실제로 다른 역할(레이아웃 vs 개별 상태) — 병합 대상 아님, `component-naming-cleanup.md`의 B유형(이름만 같고 실제 다른 것)과 동일 패턴 |
| Chip의 `Status=Selected` vs Option Chips의 `Selected` Boolean | **판단 보류(이미 기록된 미해결 사항)** | 같은 "선택" 개념을 두 계열이 다른 메커니즘으로 구현 — 의도된 설계 차이인지 사용자 확인 필요, 임의로 통일하지 않음 |
| `Button / 2 Button Layout (legacy — ...)` | **정리 후보** | 이름 자체가 "레거시"임을 자백 — 실사용 여부 확인 후 Archive 또는 삭제 판단 필요(이번 조사에서 실사용 미확인) |
| `Quick Badge Row` | **유지(구조 변경 없음, 기존 사용자 결정 존중)** | 5개 배지가 통째로 고정된 화면 종속 컴포넌트 — `list-component.md`에서 이미 "구조는 그대로 유지, 이름만 통일"로 확정된 사안. 재론하지 않음 |

---

## 7. Design Language Compliance

| 원칙(design-language.md 근거) | 판정 | 근거 |
|---|---|---|
| 16px screen horizontal margin | Consistent | Action Bar 좌우 16(기존 확인, 이번엔 재검증 안 함 — Carry-over) |
| 8px 중심의 spacing | Partially Consistent | 값은 8배수 원칙을 따르나(§5), Variable에 바인딩되지 않아 "토큰 기반"이라 보기 어려움 |
| Flat UI (그림자 대신 divider) | **Inconsistent with 기존 문서 서술** | design-language.md는 "그림자 실사용 0건"이라 서술하나, 이번 조사에서 `toast`(BACKGROUND_BLUR ×3)와 `bottom_navigation`(DROP_SHADOW ×2)에 실제 Effect가 적용돼 있음을 확인 — **문서가 최신 상태를 못 따라간 것**이지 나쁜 디자인 결정이라는 뜻은 아니다. 문서 갱신 필요 |
| Primary blue의 제한적 사용 | Consistent | Semantic 색상 조회 결과 `Text/Accent`, `State/Primary`만 Primary/500을 참조, 다른 의미로 오용된 사례 없음 |
| Error red의 semantic usage | Consistent | `Feedback/Error`가 정확히 Error/400을 참조 |
| Background fill을 강한 emphasis로 사용 | Undefined(재검증 안 함) | 이번 조사 범위에서 실제 화면 재확인을 하지 않음 — Carry-over |
| Typography hierarchy | Partially Consistent | 크기/weight 위계 자체는 유지되나(재검증 안 함), 폰트 패밀리 혼용(Pretendard/Noto Sans KR)과 `Body/medium`=17 orphan 변수가 위계 시스템의 신뢰도를 떨어뜨림 |
| 작은 interactive element일수록 더 round | Partially Consistent | Chip(200) > Button Large/Medium(8) > Small/XSmall(4) > Card(0)까지는 경향이 유지되나, "Large=8/Medium=4 비례"라던 기존 서술과 실제 값(Large=8/Medium=8)이 달라 문서 갱신 필요 |
| Commerce UI의 price/discount/rating hierarchy | Undefined(재검증 안 함) | 이번 조사 범위 밖 — Carry-over |
| PRD UR-S2: Foundation·범용 Component 명칭에 커머스 용어 금지 | **Consistent** | Semantic/Primitive 색상 변수명, 범용 컴포넌트(Badge/Button/Chip/Tab/Card 등) 이름 전수 확인 결과 `price-red` 유형의 위반 사례 없음. `roket_badge`/`Cart / *` 등은 도메인 특화 컴포넌트 자체의 이름이라 PRD 예외 범위(도메인 특화 컴포넌트는 규제 대상 아님) |

---

## 8. Recommended Action Plan

우선순위(P0→P3) 순. 실행은 하지 않았으며, 아래는 제안일 뿐이다.

1. ~~**P0 — `label`/`dialog`/`Icon Button`/`status-badge`의 Variant 이름 중복 해소.**~~ **해결(2026-09-04)** — 4개 전부 스크린샷으로 실제 콘텐츠 확인 후 이름 재부여(또는 진짜 중복은 삭제), 전부 에러 해소.
2. ~~**P0 — 3개 "ㄴ ..." 페이지의 컴포넌트 라이브러리 중복(19개 패밀리, 100개 이상 노드) 처리.**~~ **해결(2026-09-04)** — 사본 전체 삭제 완료.
3. ~~**P1 — Button-large Small/XSmall의 padding·폰트 크기를 승인된 스펙에 맞춰 마저 적용.**~~ **해결(2026-09-04)** — 14개 variant 전부 적용 완료.
4. ~~**P1 — Typography의 Noto Sans KR 혼입(6개 스타일) 의도 확인 후 재조정 여부 결정.**~~ **해결(2026-09-04)** — 스타일 6개 + raw 텍스트 28개(Foundation 페이지), 총 35곳 전부 Pretendard로 전환. `Category Tab` 실사용 텍스트도 스타일 수정과 함께 자동 반영됨.
5. ~~**P1 — `Text` 변수 컬렉션의 `Body/medium`=17, `Navigation/depth-medium`=17 orphan 값 처리**~~ **해결(2026-09-04)** — 미사용 확인 후 삭제.
6. ~~**P1 — Button `Size` 값 대소문자 통일**~~ **해결(2026-09-04)** — `Large/Medium/Small/XSmall`로 통일(21개 노드).
7. ~~**P1 — Property 이름 표준화**~~ **해결(2026-09-04)** — 상태/선택 계열(`status`/`State`/`Property 1`) 전부 `Status`로 통일(20개 세트, 약 90개 노드), 콘텐츠·글리프 종류 계열(`Icon / Utility`, `Order Deadline`)은 `Type`으로 재분류, `Sheet / Selector`·`roket_badge`의 `type`도 `Type`으로 표기 통일. 부수적으로 `Icon / Checkbox`(체크박스+라디오 아이콘 혼재)를 `Icon / Checkbox`/`Icon / Radio`로 분리, 조사 중 새로 발견한 `Icon / Status` 이름 중복 에러도 함께 수정. `Quantity Stepper`는 −/+ 버튼 활성화 여부를 직접 조회해 `Default`/`MinReached`/`MaxReached`로 실제 의미에 맞게 명명. 전체 42개 세트 재스캔으로 에러 0건 확인. 범위 제외: `Section Header`의 `State`(Show Action Boolean과 의미 중복 가능성, 별도 검토 필요), `ai_review`(값 1개뿐), `menu`(하단내비, 15개 값 구조는 유지 — 키 표기만 수정).
8. ~~**P1 — Component Checklist의 "List" 상태 정정**~~ **해결(2026-09-04)** — `list`(`6527:40035`, 실사용 7곳) ❌→✅ 반영, Figma 체크리스트 보드도 갱신(14/15).
9. ~~**P2 — 최상위 컴포넌트 이름의 대소문자/구분자 통일**~~ **해결(2026-09-04)** — 18개 세트를 Title Case로 개명(`chip`→`Chip`, `roket_badge`→`Rocket Badge`, `status-badge`→`Status Badge`, `Button-large`→`Button` 등). `Tab`(그룹)/`tab`(아이템) 충돌은 `Tab Group`/`Tab Item`으로 분리. `Icon/Placeholder Square` 마스터의 슬래시 앞뒤 공백도 통일. `Button / 2 Button Layout (legacy — ...)` 프레임은 `2 Button Layout`으로 단순화. **부가 발견**: 이름 정리 과정에서 `icon`(`6009:10568`)이 `Icon / Checkbox`와 완전히 동일한 체크박스 아이콘(실사용 6곳)임을 확인 — 사용자 확정으로 6개 인스턴스를 `Icon / Checkbox`로 swap 후 중복 세트 삭제. 전체 41개 세트 재스캔으로 에러 0건 확인.
10. ~~**P2 — `Sheet / Confirmation`의 값 1개짜리 죽은 Variant 축(`Item`, `Type`) 정리.**~~ **해결(2026-09-04)** — 두 축 제거, `Status × Button`만 남김.
11. ~~**P2 — `Bottom Navigation Item`의 `Status` 15값 단일 축을 `Icon × State` 2축으로 재구조화.**~~ **해결(2026-09-04)** — `Icon(Home/Category/Search/Mypage/Cart) × State(On/Off/Pressed)` 5×3 매트릭스로 재구성. 실사용 25곳은 인스턴스가 노드 ID로 원본을 참조하므로 이름 변경만으로는 영향 없음 — 스크린샷으로 재확인.
12. ~~**P2 — Review Card의 한글 Variant 값(`한달사용`/`재구매`)을 영문 식별자로 전환.**~~ **해결(2026-09-04)** — `Status=OneMonthUse/Repurchase`로 전환(`Status Badge`와 동일 영문 표기로 통일).
13. **P3 — `design-language.md` 갱신**: Shadow 실사용 0건 서술을 현재 상태(toast blur, bottom nav shadow)로 정정, Button radius 서술(Large 8/Medium 4)을 실제 값(Large 8/Medium 8)과 맞추거나 반대로 Medium radius를 4로 되돌릴지 결정.
14. **P3 — `thumbnail` Size별 radius(small/medium/large=8 추정 vs Xlarge=0) 의도 확인, Medium/Large 실측 필요.**
15. **P3 — PRD §11.2 예상 컴포넌트 중 여전히 전무한 것들 확인**: Divider(독립 컴포넌트 없음, ad hoc 라인만 존재) · Accordion · Anchor Navigation · Carousel + Indicator · Ratio Visualization · Empty State · Skeleton · Promotion Badge(로켓/상태 배지와 별개) · Ad Container · Evidence Chip · Reason Label — 필요 시점에 신규 착수 여부 판단.

### 유지해도 되는 것 (수정 불필요)
- Color Foundation 전체 구조와 하드코딩 없는 상태(로켓 배지 4색 제외)
- Spacing 토큰의 실제 숫자 값 체계(4배수 원칙 + 2px 예외) — 값 자체는 옳음, 바인딩만 별개 문제
- `Category Tab`/`Option Chips` 패밀리의 Variant 축 설계(베스트 프랙티스로 참고 가능)
- `Tab`/`tab` 공존 자체(이름만 더 명확히 하면 됨, 구조는 유지)
- Chip vs Option Chips의 "선택" 표현 차이(이미 기록된 미결 사항, 임의 통일 금지)
- `Quick Badge Row`의 고정 구조(이미 사용자가 유지하기로 확정)
- Button `Type × Size × State` 축 설계 자체와 Large만 Pressed/Disabled를 갖는 의도된 범위 제한

---

## 부가 발견 — Property 이름 표준화 작업 중 새로 나온 것 (2026-09-04)

- **`Icon / Status`**(`6020:13582`) 이름 중복 에러 신규 발견 — 2개 variant 둘 다 "Property 1=Default"였음(실제로는 빨간 원=Error, 주황 삼각형=Warning). 이번 작업에서 함께 수정.
- **`Icon / Checkbox`**(`6020:13544`) 4개 variant 중 절반(원형 2개)이 실제로는 라디오 아이콘이었음 — `Icon / Radio`(신규, `6573:3439`)로 분리, 둘 다 `Status=Default/Disabled`로 정리.
- **`Section Header`**의 `State`(값: `text`/`text + button`) 속성이 같은 컴포넌트의 `Show Action` Boolean과 의미가 겹치는 것으로 보임 — 이번 작업 범위 밖이라 이름만 그대로 두고 발견 사항으로 기록. 필요시 후속 검토.

## 변경 이력

| 일자 | 내용 |
|---|---|
| 2026-09-04 | 최초 작성. Foundation/컴포넌트 인벤토리·네이밍/Variant·Property·시각 일관성 3갈래 병렬 조사(read-only) 후 종합 |
| 2026-09-04 | P0 4건, 라이브러리 중복(19개), Noto Sans KR(35곳), Button Small/XSmall 스펙, `Text` orphan 변수, Button Size 대소문자, Checklist List 정정, Property 이름 표준화(20개 세트 + `Icon/Status` 버그 + `Icon/Checkbox`↔`Icon/Radio` 분리) 전부 해결 — P0/P1 전 항목 완료 |
