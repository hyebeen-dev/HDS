# Design System 명세서 (초안)

> **문서 상태**: v0.1 **초안 — 검토 대기**. 사용자 검토 후 수정 예정이며, 검토 결과가 반영되기 전까지는 확정본이 아니다.
> **문서 성격**: PRD(`prd-v0.1.md`) §11 "Design System Requirements (v0.2에서 채울 영역)"에 해당하는 내용을 실제 Figma 확정값으로 채운 **Foundation + Component 공식 명세서**다. 개발자·AI Agent가 별도 질문 없이 바로 구현에 쓸 수 있는 확정값만 싣는다.
> **근거**: 이 문서의 모든 값은 Figma 파일(HDS_2609, fileKey `y8OcE4JLKi7ADIPCVFJQej`)에서 2026-09-04 직접 재조회한 실측값이거나, `design-system/decisions.md`에 이미 기록된 결정이다. 임의로 새 값이나 새 원칙을 만들지 않았다.
> **관련 문서**: 현재 진행 현황 → `design-system/status.md` / 시각 스타일 관찰 → `design-system/design-language.md` / 의사결정 로그 → `design-system/decisions.md` / 전체 감사 기록 → `diagnosis/design-system-audit-2026-09-04.md`

---

## 0. 이 문서 사용 원칙

이 문서의 목적은 참고 기록이 아니라 **새 화면을 만들 때 실제로 지키는 규칙**이다. §1~§6의 내용을 어떻게 적용할지 판단이 필요할 때, 아래 5개 원칙을 순서대로 확인한다.

1. **컴포넌트 우선** — 새 화면 요소가 필요하면 먼저 §2에 이미 정의된 컴포넌트로 해결되는지 확인한다. 없는 게 확실할 때만 새로 만든다.
2. **토큰 전용** — 색상·spacing·radius 등은 §1 Foundation에 정의된 토큰만 사용한다. raw 값(hex, px)을 직접 쓰지 않는다. 필요한 값에 맞는 토큰이 없으면 임의로 새 값을 만들지 않고 먼저 확인을 구한다.
3. **컴포지션 우선** — 새 화면은 기존에 확인된 실제 화면 패턴 중 목적이 가장 가까운 것을 뼈대로 삼는다. 그 화면의 목적상 다르게 가야 하는 부분만 벗어나고, 왜 벗어났는지 남긴다.
4. **모호함 표면화** — 이 문서에 없는 상황(정의되지 않은 컴포넌트 조합, 새로운 콘텐츠 종류 등)을 만나면 임의로 결정하지 않고 먼저 확인을 구한다.
5. **사후 검증** — 화면을 다 만든 뒤, 실제로 사용한 컴포넌트의 Variants/Content 규칙과 적용한 컴포지션 패턴에 맞는지 다시 대조한다.

---

## 1. Foundation

### 1.1 Color

**Primitive** — 8개 색상 패밀리(각 팔레트 단계별 값 보유) + 배송 배지 전용 `Rocket` 패밀리입니다. 개별 사용은 지양하고 아래 Semantic 토큰(또는 Rocket은 그 자체)을 통해서만 참조합니다.

| 패밀리 | 용도 |
|---|---|
| Gray | 배경·구분선·보조 텍스트 |
| Primary | 행동(CTA)·링크. **가격 강조에는 쓰이지 않습니다**(아래 사용 규칙 참고) |
| Secondary | Primary 계열 보조 톤 |
| Point | 강한 강조(주황빨강 계열) |
| Error | 위험·할인·최종 판매가 강조 |
| Warning | 경고 표기(현재 프로모션 용도 실사용 없음) |
| Success | 정의는 존재하나 **실사용 0건**(아래 사용 규칙 참고) |
| Static | 순수 흰색(`0`)/검정(`1000`) |
| **Rocket** | 배송 배지 4종 전용 — `Rocket/Seller/Background`·`Text`, `Rocket/Fresh/Background`·`Text`, `Rocket/Global/Background`·`Text`, `Rocket/Tomorrow/Background`·`Text` 8개. Semantic 레이어로 승격하지 않았습니다(도메인 특화 개념이라 Primitive로 충분) |

**Semantic** (23개, 전량 Primitive를 alias해서 사용 — 하드코딩 없음)

| 토큰 | 값 | 근거 Primitive |
|---|---|---|
| `Text/Primary` | `#020b18` | Primary/1000 |
| `Text/Description` | `#7b818e` | Gray/600 |
| `Text/On Color` | `#ffffff` | Static/0 |
| `Text/Accent` | `#106def` | Primary/500 |
| `Background/Background` | `#f4f6f6` | Gray/50 |
| `Background/Divider` | `#e3e5e8` | Gray/200 |
| `Background/Overlay` | `#000000` | (직접 값) |
| `State/Primary` | `#106def` | Primary/500 |
| `State/Primary Pressed` | `#0d57bf` | Primary/600 |
| `State/Secondary` | `#e7f0fd` | Primary/50 |
| `State/Secondary Pressed` | `#cfe2fc` | Primary/100 |
| `State/Disabled` | `#cdd0d5` | Gray/300 |
| `Feedback/Success` | `#1dc948` | Success/500 |
| `Feedback/Error` | `#e23636` | Error/400 |
| `Feedback/Warning` | `#ebb447` | Warning/400 |
| `Feedback/Info` | `#969ca6` | Gray/500 |
| `Feedback/Error Background` | `#fce9e9` | Error/50 |
| `Feedback/Error Text` | `#9c1616` | Error/600 |
| `Feedback/Warning Background` | `#fcf6e8` | Warning/50 |
| `Feedback/Warning Text` | `#b88114` | Warning/600 |
| `Emphasis/Primary` | `#f2460d` | Point/500 |
| `Emphasis/Secondary` | `#34373d` | Gray/900 |
| `Emphasis/Ambient` | `#7b818e` | Gray/600 |

**사용 규칙**
- **가격 강조는 파랑이 아니라 Error/Gray 계열입니다.** `Price Block`의 최종가는 할인 적용 시 `Error/400`(빨강), 미적용 시 `Gray/900`(거의 검정)이며 파랑은 어디에도 쓰이지 않습니다. 실제 장바구니 화면에서도 "총 결제 예상 금액"이 빨강으로 렌더링됩니다.
- Primary(파랑)는 **CTA 버튼 배경·링크 텍스트 등 "행동" 용도로만** 사용합니다. 가격 숫자 강조에는 쓰지 않습니다.
- **`Success` semantic은 정의만 있고 실사용 0건입니다.** `Toast`의 `Status=Success` 상태 아이콘은 실제로 초록이 아니라 `Primary/400`(파랑)을 쓰고 있습니다.
- Error(빨강)는 할인율·위험·긴급 신호 및 위 최종가 강조에 쓰입니다.
- 배경색 채움(fill)은 이 시스템에서 가장 강한 강조 수단입니다 — CTA 버튼과 할인율 배지 외에는 배경색을 채우지 않습니다.
- 프로모션 전용 색상은 별도로 정의돼 있지 않습니다 — 필요해지면 그때 정의합니다.
- **로켓 배송 배지 4색은 `Rocket` Primitive 패밀리로 관리됩니다**(위 Primitive 표 참고). `Rocket Badge` 컴포넌트 4 variant 전량이 여기에 바인딩돼 있습니다.
- **Dialog/Bottom Sheet의 dim 오버레이는 `Background/Overlay`(`#000000`)를 50% 불투명도로 적용합니다.** 화면 전체를 덮는 스크림 레이어는 정적 컴포넌트 자체에 포함되지 않고 구현 시 추가되는 영역이라, 이 값은 Toast의 위치·노출시간 규칙과 같은 성격의 개발 구현 규칙입니다.
- **`Feedback/Success`는 실사용에서 파랑(`Primary/400`)으로 렌더링되는 것이 공식 동작입니다.** `Toast`의 `Status=Success` 아이콘이 초록이 아니라 파랑을 쓰는 현재 상태를 그대로 규칙으로 확정합니다 — `Feedback/Success` 토큰 자체를 삭제하지는 않되, 신규 컴포넌트에서 "성공" 상태를 표시할 때도 파랑을 기준으로 합니다.
- **`Feedback/Error Background`/`Error Text`, `Feedback/Warning Background`/`Warning Text`는 `Message Box`(§2.7) 전용 배경·텍스트 페어입니다.** 옅은 배경 위에 진한 텍스트를 얹는 카드형 안내 배너에 씁니다 — 기존 `Feedback/Error`/`Feedback/Warning`(아이콘 단색용)과는 용도가 다릅니다.

### 1.2 Typography

Pretendard 단일 패밀리입니다. 6개 역할 카테고리로 나뉘며, 하나의 시각 스펙(크기+굵기)이라도 역할에 따라 별도 토큰으로 분리합니다(예: 14px Regular가 Chip에서는 `label`, Tab에서는 `navigation`, Toast에서는 `body`로 각각 별개 스타일).

| 카테고리 | 스타일 수 | 용도 |
|---|---|---|
| `display` | **0개(예약, 아직 미생성)** | 실사용 사례가 없어 임의로 값을 만들지 않고 빈 채로 예약 |
| `heading` | 7개 | 제목·헤더 |
| `body` | 15개 | 본문·가격·설명 등 일반 텍스트 |
| `label` | 3개 | 배지·라벨류 짧은 텍스트 |
| `navigation` | 3개 | 탭·내비게이션 텍스트 |
| `underline` | 1개 | 밑줄 텍스트(링크 등) |

**전체 스타일 표** (전부 family=Pretendard, 실측값)

| 스타일 | Weight | Size | Line Height | Letter Spacing |
|---|---|---|---|---|
| `heading/xlarge` | Bold | 30 | 150% | 0 |
| `heading/large` | Bold | 26 | 150% | 0 |
| `heading/medium` | Bold | 22 | 150% | 0 |
| `heading/small` | Bold | 18 | 150% | 0 |
| `heading/xsmall` | Bold | 16 | 150% | 0 |
| `heading/xsmall-medium` | Medium | 16 | 150% | 0 |
| `heading/xxsmall` | Bold | 15 | 150% | 0 |
| `body/large-bold` | Bold | 19 | 150% | -0.5px |
| `body/large-medium` | Medium | 19 | 150% | 0 |
| `body/large` | Regular | 19 | 150% | 0 |
| `body/medium-bold` | Bold | 16 | 150% | 0 |
| `body/medium-medium` | Medium | 16 | 150% | 0 |
| `body/medium` | Regular | 16 | 150% | 0 |
| `body/small-bold` | Bold | 14 | 150% | 0 |
| `body/small-medium` | Medium | 14 | 150% | 0 |
| `body/small` | Regular | 14 | **140%**(예외) | 0 |
| `body/compact-medium` | Medium | 15 | 150% | 0 |
| `body/compact` | Regular | 15 | **140%**(예외) | 0 |
| `body/xsmall-medium` | Medium | 13 | 150% | 0 |
| `body/xsmall` | Regular | 13 | 150% | 0 |
| `body/xxsmall` | Regular | 12 | 150% | 0 |
| `body/xxxsmall` | Medium | 11 | 150% | 0 |
| `label/small` | Regular | 14 | **140%**(예외) | 0 |
| `label/xsmall-bold` | Bold | 13 | 150% | 0 |
| `label/xsmall-medium` | Medium | 13 | 150% | 0 |
| `navigation/medium` | Medium | 16 | 150% | 0 |
| `navigation/small-bold` | Bold | 14 | 150% | 0 |
| `navigation/small` | Regular | 14 | **140%**(예외) | 0 |
| `underline/xsmall` | Regular | 13 | 150% | 0 |

**규칙**: Line height는 150%가 기본입니다. `body/small`, `body/compact`, `label/small`, `navigation/small`(전부 14px 또는 15px Regular 계열) 4개만 140%로 예외를 둡니다.

### 1.3 Spacing

16개 값입니다. 4배수를 기본 원칙으로 하되, 컴포넌트 특성상 불가피한 경우 2px 단위 예외를 명시적으로 허용합니다.

| 구분 | 값 |
|---|---|
| 4배수(권장) | 4, 8, 16, 20, 24, 32, 40, 48, 56, 64 |
| 예외(2px 단위, 버튼·칩류 등에서 발견된 실측값) | 2, 6, 10, 11, 13 |
| 12 | 4배수는 아니나 별도 예외로 분류되지 않고 그대로 존재합니다(재확인 필요 — 후속 과제) |

**반복되는 관계**: 화면 좌우 마진 **16px** / 카드 내부 요소 gap **8px**이 가장 지배적입니다. 섹션 간 구분은 8px 두꺼운 divider + 16px 여백 조합입니다. 컴포넌트 라이브러리 전체의 padding·gap은 실제 Variable에 바인딩돼 있어 리터럴 숫자로 하드코딩돼 있지 않습니다.

### 1.4 Grid

모바일 단일 브레이크포인트만 정의합니다(PRD 범위가 Mobile-first이므로 멀티 브레이크포인트를 도입하지 않았습니다).

| 항목 | 값 |
|---|---|
| 컨테이너 폭 | 360px |
| 마진 | 16px |
| 거터 | 8px |
| 컬럼 수 | 4 |

**미정의**: 캐러셀 영역(3열)의 별도 규칙은 문서에 아직 명시하지 않았습니다.

**4컬럼과 `Item Card / Grid` 160px의 관계**: 컬럼 폭은 `(360 − 마진 16×2 − 거터 8×3) ÷ 4 = 76px`입니다. 실제 2열 그리드 화면(`Item Card / Grid`, 160px)은 이 4컬럼 중 **2컬럼을 내부 거터(8px)로 묶어 하나의 카드 폭으로 쓴 것**입니다 — `76×2 + 8 = 160`. 즉 4컬럼 그리드와 2열 카드 배치는 서로 다른 규칙이 아니라, 같은 4컬럼 그리드를 카드 하나당 2컬럼씩 묶어 쓴 결과입니다.

### 1.5 Radius

**공식 토큰 5단계**입니다 — `Radius` Variable Collection으로 Figma에 등록돼 있고, 컴포넌트 라이브러리 전체에 바인딩돼 있습니다. 필요해지면 `large` 위로 `xlarge` 등을 계속 추가하는 open-ended 구조입니다.

| 토큰 | 값 | 해당 컴포넌트 |
|---|---|---|
| `none` | 0 | Category Tab, Item Card(Grid Small/Cart), Action Bar, Dialog 본체, Bottom Sheet 본체, Thumbnail XLarge |
| `small` | 4 | Button Small/XSmall |
| `medium` | 8 | Button Large/Medium, Thumbnail Small/Medium/Large |
| `large` | 16 | Dialog·`Bottom Sheet / Informational`·`Bottom Sheet / Interactive`의 `header` 상단 두 모서리(하단은 `none`) |
| `full` | 999(모든 실사용보다 충분히 큰 값 — 실제 렌더링은 요소 최단변 절반에서 자동 클램프되어 시각적 차이 없음) | Chip, Radio, Icon Button, Search, Bottom Sheet 드래그 핸들, Item Card/Cart 아이콘 |

**규칙**: 작고 인터랙티브한 요소일수록 더 둥급니다 — `full`(Chip/Radio/Icon Button 등) > `medium`(Button Large/Medium) > `small`(Button Small/XSmall) > `none`(Card/Action Bar 등) 순입니다. `large`는 이 축과 별개로, 모달성 오버레이(Dialog/Bottom Sheet)의 상단 모서리 전용입니다.

### 1.6 Elevation

기본 원칙은 **그림자를 쓰지 않고 divider/여백으로 깊이를 표현**하는 것입니다. 정의된 `shadow` 이펙트 스타일(DROP_SHADOW, radius 10, 검정 5%)이 있으나 원칙상 실사용을 지양합니다.

**알려진 예외**: `Toast`에 BACKGROUND_BLUR 3건, `Bottom Navigation`에 DROP_SHADOW 2건이 실제 적용돼 있습니다. 이 두 컴포넌트(화면 위에 떠 있는 고정 오버레이 성격)는 의도된 예외로 잠정 분류하며, 공식 원칙 문서화는 후속 과제로 남겨둡니다.

### 1.7 Icon

| 항목 | 값 |
|---|---|
| 기본 크기 | 16×16 |
| Stroke weight | 1.5px(가는 outline) |
| 스타일 구분 | 내비게이션/컨트롤 아이콘 = outline, 정보성 배지 아이콘(로켓 배송 등) = filled |
| 기본형 | 아이콘 단독보다 텍스트와 짝을 이루는 구성이 기본 |

---

## 2. Component

Component Checklist(사용자 확정 15개 항목) 기준, **14/15 완료**(Radio/Checkbox만 미착수). "완료 기준 = 컴포넌트가 만들어져 있으면 완료"(실사용 여부 무관).

| # | 체크리스트 항목 | 상태 | 실제 컴포넌트 | Variant / Property 구조 |
|---|---|---|---|---|
| 1 | Badge | ✅ | `Rocket Badge`, `Status Badge` | `Rocket Badge`: `Type`(seller/fresh/global/tomorrow), radius `small`(4) · `Status Badge`: `Type`(Repurchase/OneMonthUse), `Review / Card`에 실제 인스턴스로 연결됨 |
| 2 | Banner | ⚠️ **정의 작성 전** | `Cart / Notification Banner`, `Cart / Countdown Banner`(존재는 함) | 컴포넌트는 있으나 "Banner"의 정의 자체가 아직 합의되지 않음 — §2.2 참고 |
| 3 | Button | ✅ | `Button`(CTA), `Icon Button`, `Text Button` | `Button`: `Type`(Primary/Secondary/Line type gray) × `Size`(Large/Medium/Small/XSmall) × `Status`(Default/Pressed/Disabled) + `Show Leading Icon`/`Show Trailing Icon`(Boolean) · `Icon Button`: `Type`(Secondary/Line type gray) × `Size`(Medium/Small) × `Status`(Default/Pressed) · `Text Button`: `Type`(Default/accent) × `Size`(Default/Large) |
| 4 | Chip | ✅ | `Chip`, `Option Chips / Size`, `Option Chips / Thumbnail` | `Chip`: `Status`(Default/Selected/Pressed) × `Dropdown`(Boolean) × `rocket`(배송 타입) · Option Chips: `Status`(Default/low_stock/sold_out) + `Selected`(Boolean) |
| 5 | Dialog | ✅ | `Dialog` | `Status`(Default/Error/Warning) × `Button`(1 Button/2 Button/X) + `Show Scroll`(Boolean, 본문 초과 시 스크롤 UI 미리보기) |
| 6 | Heading(=App Bar, Section Header, Cart Utility) | ✅ | `App Bar`, `App Bar Small`, `Section Header`, Cart 전용 4종(Notification Banner/Countdown Banner/Address Row/Selection Toolbar) | App Bar: `Type`(Title/Filter/Filter Compact/Search/Category/Index) · Section Header: `State`(값 1개만 남음) + `Show Action`(Boolean, 우측 액션 버튼 `visible` 제어) |
| 7 | Message Box | ✅ | `Message Box` | `Status`(Error/Warning). `Toast`(스낵바)와는 별개 컴포넌트 — §2.7 참고 |
| 8 | Item Card(, Review Card) | ✅ | `Item Card / Grid`, `Item Card / Recommendation`(구 Grid Small), `Item Card / Cart`, `Price Block`, `Spec Row`, `Rating Display`, `Order Deadline`, `Review / Card`, `Review / Summary` | Grid: variant 없음(단일 컴포넌트) · Recommendation: `Type`(Default/Button) × `Discount`(Applied/None) · Cart: `Stock`(Available/OutOfStock) · Price Block: `Discount`(Applied/None) + `Show Unit Price`(Boolean) · Review/Card: `Type`(Photo/Only Text) + `Show Status Badge`(Boolean) · Review/Summary: `status`(Default/none photo/no rating) + `Show Row 1~4`(Boolean) |
| 9 | Label | ✅ | `Label` | `Type`(Promotion/Return/Shipping/RepeatPurchase) |
| 10 | List | ✅ | `List`, `Category Tab`, `Category Menu Item` | List: `type`(Default/Image/history) × `Status`(Default/pressed) · Category Tab: `Status`(Default/Selected) |
| 11 | Navigation | ✅ | `Bottom Navigation`, `Bottom Navigation Item` | Bottom Navigation: `Platform`(iOS/Android) · Item: `Icon`(Home/Category/Search/Mypage/Cart) × `State`(On/Off/Pressed) |
| 12 | Bottom Sheet | ✅ | `Bottom Sheet / Informational`(구 `Sheet / Confirmation`), `Bottom Sheet / Interactive`(구 `Sheet / Selector`) | Informational: `Status`(Default/Error/Warning) × `Button`(1/2 Button) + `Show Icons`(Boolean) + `Show Scroll`(Boolean, 기본값 false, 구분선+스크롤바 4요소) · Interactive: `Type`(Item/Filter) — `Show Scroll` 없음 |
| 13 | Tab | ✅ | `Tab Group`, `Tab Item` | Group: `Type`(2 Tab/3 Tab/Swipe) · Item: `Status`(Default/Selected/Pressed) |
| 14 | Thumbnail | ✅ | `Thumbnail` | `Size`(xsmall/small/medium/large/Xlarge) |
| 15 | Radio / Checkbox | ❌ **토글 가능한 진짜 컴포넌트 기준 0/2** | `Icon / Radio`, `Icon / Checkbox`(둘 다 장식용 글리프, 실제 체크/언체크 토글 불가) | `Icon / Radio`만 존재하며 실사용 0곳입니다. `Icon / Checkbox`는 실재하며 실사용도 있습니다(`Item Card / Cart` 내장 + 장바구니 화면 4곳 + `Cart / Selection Toolbar`) — 다만 Default/Disabled 둘 다 "체크됨" 모양만 있고 Unchecked variant가 없어 진짜 토글 컴포넌트로는 쓸 수 없습니다 |

> 아래는 각 컴포넌트의 상세 명세(Description / When to use / Variants·States / Content rules)입니다. Figma 실측·실사용 인스턴스 콘텐츠를 근거로 작성했으며, 확신이 낮은 부분은 "확인 필요"로 명시했습니다 — 전부 초안이며 검토 대상입니다.

### 2.1 Badge

**비교**

| 항목 | Rocket Badge | Status Badge |
|---|---|---|
| 용도 | 배송 유형 식별(정보성) | 리뷰 작성자의 구매 이력 신뢰 신호 |
| 형태 | 살짝 둥근 사각형(`small`, 4) | outline(파란 테두리+텍스트, 배경 없음) |
| 상태 변화 | 없음(정보 표시 전용) | 없음(정보 표시 전용) |
| 실사용 | 49곳 | 9곳(Review Card 경유) |

**Rocket Badge**

Figma: `6008:7346`

상품의 배송 유형(로켓배송 계열)을 한눈에 식별시키는 배지입니다. 아이콘+텍스트 조합으로 배송 신뢰도를 전달합니다.

**사용 가이드**
- ✅ Do: 상품 카드·상세 등에서 이 상품이 어떤 로켓배송 유형인지 표시할 때 사용합니다.
- ❌ Don't: 배송 유형이 없는 상품에는 노출하지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Seller(판매자로켓, 주황) / Fresh(로켓프레쉬, 초록) / Global(로켓직구, 보라) / Tomorrow(로켓내일, 파랑) | — | 상태(Pressed 등)는 없음 — 정보 표시 전용 |

Radius는 `small`(4)입니다 — pill(`full`)이 아니라 살짝 둥근 사각형(Chip과 다른 형태)입니다. 배경·텍스트 색은 `Rocket` Primitive 변수에 바인딩돼 있습니다.

**Anatomy(아이콘 구조, 의도된 예외)**: `Seller`/`Fresh`/`Global` 3종의 아이콘은 내부에 2개 레이어로 구성됩니다 — 실제 파란 로켓 이미지(`images 2`, blendMode `MULTIPLY`) 위에 단색 레이어(`images 1`, blendMode `COLOR`)를 얹어 타입별 색으로 틴트하는 방식입니다. `images 1`의 색상은 변수에 바인딩하지 않고 raw hex로 유지합니다 — 블렌드 모드 틴팅 기법을 위한 의도된 설계이며, 변수 바인딩 예외로 문서화합니다(`Tomorrow`만 별도 벡터 아이콘이라 이 구조가 아닙니다).

**Content**
- "판매자로켓"/"로켓프레쉬"/"로켓직구"/"로켓내일" 4개 고정 문구만 사용(자유 입력 아님, Type 값과 1:1 대응)
- 전부 5자 고정
- 실사용 49곳

**Status Badge**

Figma: `6373:29771`

리뷰 등에서 작성자의 구매 이력 신뢰 신호를 표시하는 outline 배지(파란 테두리+텍스트, 배경 없음)입니다.

**사용 가이드**
- ✅ Do: Review Card 등에서 리뷰 작성자가 재구매했는지/한 달 이상 사용했는지를 짧게 표시할 때 사용합니다.
- ❌ Don't: 근거 데이터가 없으면 노출하지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Repurchase(재구매) / OneMonthUse(한달사용) | — | 둘 다 동일한 outline 스타일, 상태 변화 없음. 추후 신뢰 신호 종류가 늘어나면(예: 장기사용, 대량구매 등) 값을 추가하는 방식으로 확장합니다 |

**Content**
- "재구매"/"한달사용" 2개 고정 문구(3~4자)
- 실사용 9곳(Review Card 경유)
- 신뢰 신호가 실제로 검증된 경우에만 표시해야 합니다(PRD UR-D3/UR-D4)

### 2.2 Banner

⚠️ **작성 전 — 아직 정의되지 않았습니다.** Figma에 `Cart / Notification Banner`, `Cart / Countdown Banner` 두 컴포넌트가 존재하긴 하지만, "이 디자인 시스템에서 Banner란 무엇인가"에 대한 정의 자체를 먼저 합의한 뒤 이 섹션을 채웁니다. 임의로 정의를 지어내지 않습니다.

### 2.3 Button

Button은 역할이 다른 **3개의 독립 컴포넌트**로 구성됩니다. 셋을 같은 축으로 헷갈리지 않습니다.

**비교**

| 항목 | Button (CTA) | Icon Button | Text Button |
|---|---|---|---|
| 용도 | 화면의 핵심 행동 | 라벨 없는 보조 액션 | 가장 낮은 강조의 내비게이션 |
| 텍스트 | 있음(라벨) | 없음(아이콘만) | 있음(텍스트+화살표) |
| 강조 수준 | 높음(Primary~Line type gray 3단계) | 중간 | 가장 낮음 |
| 실사용 맥락 | "구매하기" 등 CTA | 리뷰 "도움이 돼요" | "전체보기 〉" |

**Button (CTA)**

Figma: `2167:15918`

사용자의 핵심 행동(구매, 상세 확인 등)을 유도하는 기본 CTA 컴포넌트입니다. Primary/Secondary/Line type gray 3종 시각 위계와 4단계 크기를 조합해 행동의 중요도를 구분합니다.

**사용 가이드 — Type별 실제 사용 맥락**
- ✅ Do — `Primary`: 화면당 가장 중요한 단일 행동에 씁니다("구매하기", "총 1개 상품 구매하기").
- ✅ Do — `Secondary`: 실사용 11곳 전부 두 가지 맥락으로만 쓰입니다 — ① **2-Button 레이아웃에서 Primary와 짝을 이루는 차순위 행동**(예: "바로구매"+"장바구니 담기" 조합에서 "장바구니 담기"), ② **Action Bar 안에서 단독으로 쓰이는 낮은 강조의 1개 행동**("상품 정보 더보기"). 즉 "Primary와 짝지어진 차선책" 또는 "그 자체로 낮은 강조의 단독 행동" 두 상황에 씁니다.
- ✅ Do — `Line type gray`: **재입고 알림 신청**(Item Card/Cart 품절 상태)처럼, 진행 중인 다른 행동이 없을 때 재알림 신청 같은 **낮은 강조의 단독 대기 행동**에 씁니다.
- ✅ Do: 리스트/카드 내부처럼 좁은 공간에는 `Small`/`XSmall`을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Primary / Secondary / Line type gray | Primary | 실사용 맥락은 위 사용 가이드 참고 |
| Size | Large(48) / Medium(40) / Small(32) / XSmall(28) | Large | Radius는 Large·Medium `medium`(8), Small·XSmall `small`(4) |
| Status | Default / Pressed / Disabled | Default | Medium은 Default만 존재 |
| Show Leading Icon | Boolean | true | — |
| Show Trailing Icon | Boolean | true | — |

**Content**
- 실측 라벨은 전부 완료형 동사구, 4~9자("구매하기", "장바구니 담기", "총 1개 상품 구매하기", "상품 정보 더보기") — "~하기"로 끝나는 행동 지시형
- 아이콘은 라벨 보완용으로만 사용, 아이콘 단독 사용 없음

**Icon Button**

Figma: `6373:33042`

라벨 텍스트 없이 아이콘 하나로 동작을 표현하는 보조 액션 버튼입니다.

**사용 가이드**
- ✅ Do: 텍스트 라벨을 붙일 공간이 부족하거나, 반복 노출되는 짧은 보조 액션(리뷰 "도움이 돼요" 등)에 사용합니다.
- ❌ Don't: 화면의 주 행동(CTA)에는 쓰지 않습니다 — 그건 `Button (CTA)`의 역할입니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Secondary / Line type gray | Secondary | Primary형 없음 |
| Size | Medium(40) / Small(32) | Medium | **Radius는 `full`(pill)** — Button(CTA)의 `medium`(8) 계열과 다른 형태이므로 혼동 주의 |
| Status | Default / Pressed | Default | — |
| Show Leading/Trailing Icon | Boolean | — | — |

**Content**
- 텍스트 라벨 없음 — 아이콘 하나로 의미 전달
- 현재 실사용은 리뷰 "도움이 돼요"(공감) 단일 용례뿐 — 다른 용도 적용 시 아이콘 선택 기준 ⚠️ 확인 필요

**Text Button**

Figma: `6314:12442`

배경·테두리 없이 텍스트(+화살표)만으로 표현하는 가장 낮은 강조의 링크형 버튼입니다.

**사용 가이드**
- ✅ Do: 섹션 헤더 옆 "더보기"처럼, 리스트/카드 다음 단계로 이동하는 보조 내비게이션에 사용합니다.
- ❌ Don't: 행동을 완료시키는 CTA에는 쓰지 않습니다 — 그건 `Button (CTA)`의 역할입니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Default(회색 텍스트) / accent(파란 텍스트+Bold) | Default | — |
| Size | Default / Large | Default | 실사용 6곳 |

**Content**
- 실측 라벨은 전부 "전체보기 〉" — 명사+"보기"+화살표 아이콘 고정 패턴
- 다른 문구 변형은 관찰되지 않음(⚠️ 확인 필요)

### 2.4 Chip

**비교**

| 항목 | Chip | Option Chips |
|---|---|---|
| 용도 | 목록/검색 필터링 | 상품 옵션(용량·색상 등) 선택 |
| 선택 방식 | 다중 선택 | 단일 선택(옵션 하나) |
| 선택 표현 | `Status=Selected` 값 자체 | `Selected`(Boolean)+파랑 2px 테두리 오버레이 |
| 형태 | pill | pill |

**Chip**

Figma: `2103:5367`

목록/검색 결과를 정렬·필터링하기 위한 pill 형태의 선택형 태그입니다.

**사용 가이드**
- ✅ Do: 카테고리·속성·정렬 기준 등 다중 선택 가능한 필터 UI에 사용합니다.
- ❌ Don't: 상품 옵션 선택에는 쓰지 않습니다 — `Option Chips`를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / Selected / Pressed | Default | 선택 표현은 `Status=Selected` 값 자체로 합니다(Option Chips는 별도 Boolean 방식 — 같은 개념의 서로 다른 구현, 의도된 차이인지 미확정). 토글 동작: `Selected` 상태를 한 번 더 누르면 `Default`로 돌아갑니다(선택 해제) |
| Dropdown | Boolean | — | 하위 옵션 펼침 화살표 |
| rocket | False / tomorrow / fresh / global | False | 배송 아이콘+라벨 포함 특수 칩 |
| Show Icons | Boolean | — | — |

**Content**
- 실측 라벨 1~8자 — 초성 단독("ㄱ","ㄴ"), 브랜드명("도브"), 속성 키워드("무료배송","향이 좋아요") 등 짧은 명사/형용사구(Button과 달리 행동 동사형 아님)
- "전체"는 필터 리셋용 고정 관용구

**Option Chips / Size, Option Chips / Thumbnail**

Figma: `Option Chips / Size` `6373:23548` · `Option Chips / Thumbnail` `6373:23662`

상품 옵션(용량·색상 등)을 선택하는 pill 형태 컴포넌트입니다. 텍스트만 있는 `/ Size`와 이미지가 포함된 `/ Thumbnail` 두 하위 패밀리로 나뉘며, 상품 유형이 늘면 `Option Chips / {유형}` 패턴으로 계속 추가할 수 있습니다.

**사용 가이드**
- ✅ Do: 상품 상세/리스트에서 옵션을 하나만 선택해야 하는 UI에 사용합니다.
- ❌ Don't: 필터링 목적이면 쓰지 않습니다 — `Chip`을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / low_stock(재고부족·빨간 "N개 남음") / sold_out(품절·회색 비활성) | Default | "N개 남음"은 항상 빨강이나 정확한 노출 임계값은 ⚠️ 확인 필요 |
| Selected | Boolean | — | 파랑 2px 테두리 오버레이 — Chip과 다른 메커니즘 |

**Content**
- `/ Size`는 "240"처럼 단위 없는 짧은 숫자(용량 추정, 단위 표기 규칙 ⚠️ 확인 필요)
- `/ Thumbnail`은 "7CA 웜톤 브라운"처럼 옵션 코드+속성명 조합, 길면 말줄임(...) 처리

### 2.5 Dialog

**Dialog**

Figma: `4043:2815`

화면 위에 모달로 뜨는 확인창입니다. 제목과 본문 설명, 버튼 1~2개로 구성되며 사용자의 확인·선택이 필요할 때 화면 흐름을 멈추고 개입합니다.

**Anatomy**: `header`(제목+Status별 아이콘) → `body`(설명) → `button`(1~2개, `Button=X`는 버튼 영역 자체가 없음)

**사용 가이드**
- ✅ Do: 정보 확인(반품 정책 등)이나 명확한 예/아니오 결정이 필요할 때 사용합니다.
- ❌ Don't: 가볍게 스쳐가는 알림에는 쓰지 않습니다 — `Toast`(§2.7)를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / Error / Warning | Default | 헤더에 상태 아이콘만 추가, 레이아웃 동일 |
| Button | 1 Button / 2 Button / X | 1 Button | `X`는 버튼 영역 자체가 없음(구분선·스크롤바도 3요소만 적용, 아래 참고) |
| Show Scroll | Boolean | false | 본문 초과 시 스크롤 UI 미리보기(아래 "스크롤 동작" 참고) |

헤더 상단 모서리만 `large`(16), 본문은 `none`(0).

**Content**
- 제목은 1줄 간결한 명사형(실사용 예 "반품 안내")
- 본문은 여러 줄 허용, 실사용 예는 정책 설명문+최대 3개 불릿
- 버튼 라벨 규칙은 실사용 1건뿐이라 ⚠️ 확인 필요

**스크롤 동작** — 본문(Body) 내용이 길어질 경우 `Dialog`는 뷰포트 전체를 채우지 않고, 고정 크기 안에서 본문만 스크롤됩니다. Figma는 콘텐츠량에 따라 동적으로 스크롤 여부를 계산하지 못하므로, 아래 발생 기준은 **개발 구현 규칙**으로 문서화하고 Figma에는 `Show Scroll` Boolean으로 두 상태(스크롤 없음/스크롤 발생 중)를 미리보기할 수 있게 했습니다.

*스크롤 발생 기준 — 상대 단위 기준(고정 px 하드코딩 금지)*

디바이스마다 뷰포트 높이가 다르므로, 트리거 조건은 고정 px가 아니라 상대 단위로 계산합니다.

| 항목 | 값 |
|---|---|
| `Dialog` 전체 높이 | `max-height: 70vh` |
| 고정 영역(스크롤 안 됨) | Header 56px + Button 78px = 134px (`Button=X` variant는 Header 56px만 고정, 버튼 영역 자체가 없음) |
| Body 높이 | `max-height: calc(70vh - 134px)`(`Button=X`는 `calc(70vh - 56px)`) |
| 스크롤 트리거 | Body 콘텐츠 실제 높이가 위 `max-height`를 초과하면 `overflow-y: auto`로 자동 스크롤 |

> 참고: 뷰포트 800px 기준으로 환산하면 Body `max-height` ≈ 426px이지만, 이는 계산 예시일 뿐 실제 트리거 조건으로 하드코딩하지 않습니다(뷰포트가 다르면 값도 달라집니다).

*스크롤 UI 4요소* (`Show Scroll=true`일 때 노출, 전부 7개 variant에 반영 완료)
- **Header/Body 구분선**(`divider-top`) — 스크롤 영역의 시작을 시각적으로 분리합니다. `Background/Divider` 토큰을 바인딩했습니다.
- **Body/Button 구분선**(`divider-bottom`) — 아래에 더 많은 콘텐츠가 있음을 암시하고 버튼 영역이 고정임을 표현합니다. `Background/Divider` 토큰을 바인딩했습니다. **`Button=X` variant는 하단 버튼 영역 자체가 없어 이 구분선을 추가하지 않습니다**(3요소만 적용).
- **스크롤바**(`scroll-track`+`scroll-thumb`) — 전체 대비 현재 위치를 표시하는 얇은(4px) 인디케이터입니다. `scroll-thumb`은 `Gray/400`을 바인딩했습니다. **`scroll-track`은 의도적으로 미바인딩했습니다** — 반투명 검정(6% opacity) 표현 기법이 토큰 색상값과 맞지 않아 Rocket Badge 블렌드 틴팅과 같은 원칙으로 예외 처리합니다.
- **Body clipping** — `body` 프레임은 이미 `clipsContent:true`라 별도 작업 없이 넘치는 콘텐츠가 자동으로 잘림 처리됩니다.

### 2.6 Heading (=App Bar, Section Header, Cart Utility)

**비교**: `App Bar`는 화면 상단형 바 패밀리(Type별로 실제 위치는 제각각, 아래 참고)이고, `App Bar Small`은 그 아래 붙는 정렬·보기전환 보조 툴바, `Section Header`는 화면 내부 콘텐츠 그룹의 소제목, Cart 전용 4종은 장바구니 화면에서만 쓰는 단일 목적 바입니다.

**App Bar**

Figma: `6008:8709`

여러 상황에서 쓰이는 상단형 바 컴포넌트 패밀리입니다. **주의**: 전 Type이 "화면 최상단 고정"인 건 아닙니다 — Type별 실제 배치는 아래처럼 갈립니다.

**사용 가이드 — Type별 실제 배치**
- `Title`/`Category`/`Search`: ✅ **진짜 최상단 고정입니다**. Status Bar 바로 아래, 화면 프레임의 첫 자식으로 배치돼 있습니다(Title 4곳·Category 8곳·Search 1곳 확인, Search는 표본 1곳뿐이라 참고용).
- `Filter`: ❌ **최상단이 아닙니다**. 5곳 전부 `Category` App Bar(최상단) 바로 아래 "2번째 줄" 필터 바로 쌓여 있습니다(Status Bar → Category → Filter → App Bar Small → 콘텐츠 순서). 독립적으로 화면 맨 위에 오지 않습니다.
- `Filter Compact`: ❌ **최상단이 아닙니다**. 3곳 전부 상품 상세페이지처럼 긴 스크롤 콘텐츠 중간(예: 리뷰 섹션 부근)에 그 섹션 전용 필터 행으로 존재하며, 화면 상단과 무관합니다.
- `Index`: ❌ **최상단이 아닙니다**. 확인된 2곳 전부 `Bottom Sheet / Interactive`(구 `Sheet / Selector`) 내부의 자모 인덱스 점프 스트립으로 존재합니다 — 화면 레벨 요소가 아니라 모달 안의 보조 UI입니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Title / Filter / Filter Compact / Search / Category / Index | Title | 단일 축. `Filter`/`Filter Compact`/`Index`는 배치 맥락이 `Title`/`Category`/`Search`와 다르지만(2번째 줄 필터 / 섹션 내부 필터 / 모달 내부 요소), 별도 컴포넌트로 분리하지 않고 같은 축에 유지하기로 결정했다 |

**Content**
- `Title` 제목은 "장바구니"처럼 2~4자 명사형
- `Category`는 카테고리명 그대로("바디워시", "커피/차")
- `Filter` 칩은 2~6자 조건어("전체","로켓내일","4점 이상")
- `Search` 플레이스홀더는 "쿠팡에서 검색하세요" 형태(문서화된 규칙은 아님)

**App Bar Small**

Figma: `6210:2794`

목록형 콘텐츠 상단에서 정렬 기준과 보기 방식(리스트/그리드)을 전환하는 보조 툴바입니다.

**사용 가이드**
- ✅ Do: App Bar 아래, 정렬·보기전환이 필요한 콘텐츠 섹션 상단에 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | List / Grid | List | 현재 보기 방식에 따라 아이콘만 전환. 정렬 드롭다운은 공통 |

**Content**
- 정렬 기준 "추천순"처럼 2~4자 명사형 + 드롭다운 화살표

**Section Header**

Figma: `6324:1546`

화면 내 콘텐츠를 의미 단위로 구분하고, 필요 시 부가 액션(전체보기 등)을 제공하는 소제목입니다.

**사용 가이드**
- ✅ Do: 한 화면에 여러 정보 그룹이 있을 때(상품 상세의 "리뷰", "함께 사면 좋은 상품" 등) 그룹 경계를 표시할 때 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| State | text (값 1개만 남음) | text | Figma Plugin API가 COMPONENT_SET의 마지막 남은 variant 축 삭제를 허용하지 않아 값 1개짜리 축으로 남아있습니다(기능상 문제 없음) |
| Show Action | Boolean | true | `Show Action` Boolean이 우측 액션 버튼(`Text Button`, "전체보기 〉")의 `visible`을 직접 제어합니다 |

**Content**
- 실사용 텍스트를 찾지 못함(플레이스홀더 "Title"뿐) — ⚠️ 확인 필요
- 액션 텍스트는 "더보기 〉" 형태로 관찰

**Cart 전용 컴포넌트 4종** (Variant 없는 단일 컴포넌트, 장바구니 화면 전용)

| 컴포넌트 | Figma | Description | When to use | Content rules |
|---|---|---|---|---|
| Cart / Notification Banner | `6328:1544` | 혜택 알림 신청 유도 배너 | 장바구니 상단, 미신청 사용자에게 | "장바구니 혜택이 생기면 바로 알려드릴게요" + "알림받기" |
| Cart / Countdown Banner*(구 Countdown Badge, §2.2 참고)* | `6328:1551` | 할인 종료 카운트다운 배너 | 장바구니 전체 단위 시간제한 할인 안내 | "할인 종료 HH:MM:SS" + "남은 상품이 있어요" |
| Cart / Address Row | `6328:1561` | 배송지 표시+변경 진입점 | 배송지 확인·변경 필요 지점 | "이름 (행정구역)" + "변경 〉" |
| Cart / Selection Toolbar | `6328:1570` | 전체선택/선택삭제 툴바 | 다중 선택·일괄 삭제 리스트 상단 | "전체선택" / "선택삭제"(각 4자 고정) |

### 2.7 Message Box

**Message Box**

Figma: `6735:3455`

화면 내에 고정으로 노출되는 안내·유의사항 배너입니다. 별도 닫기 버튼이 없고, 사용자 조작과 무관하게 화면에 계속 남아 있습니다. 화면 최하단에 잠깐 떴다 사라지는 `Toast`(아래 참고)와는 별개 컴포넌트입니다.

**사용 가이드**
- ✅ Do: 결제 실패·재고 임박처럼 사용자가 화면을 벗어나기 전에 계속 인지해야 하는 안내·유의사항에 사용합니다.
- ❌ Don't: 잠깐 보여주고 사라져도 되는 조작 결과 피드백에는 쓰지 않습니다 — `Toast`를 씁니다.
- ❌ Don't: 닫기 버튼을 추가하지 않습니다 — 닫을 수 있어야 하는 안내는 `Dialog`(§2.5)나 `Toast`를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Error / Warning | Error | Error=`Feedback/Error Background`+`Error Text`(연핑크 배경+진빨강), Warning=`Feedback/Warning Background`+`Warning Text`(연노랑 배경+진골드). 아이콘 원도 각각 같은 Text 토큰에 바인딩 |

**Content**
- 안내 문구는 한 줄로 끝나지 않을 수 있어 자동 줄바꿈을 허용합니다(§4 긴 텍스트 처리 원칙 참고).
- 아이콘은 원형 배경 위에 "i"(정보) 글리프 — 기존 `Icon / Status`(경고 느낌표 아이콘)와는 별개로, Message Box 전용으로 새로 만들었습니다.

---

**Toast** (기존 컴포넌트, Message Box 아님)

Figma: `4001:18707`

화면 하단에 잠깐 떴다 사라지는 짧은 상태 메시지(스낵바)입니다. 사용자 조작 결과를 방해 없이 알려줍니다.

**사용 가이드**
- ✅ Do: 즉각 피드백은 필요하지만 화면 흐름을 막을 필요는 없을 때 사용합니다(예: "찜 목록에 추가됨").
- ❌ Don't: 확인이 필요한 결정에는 쓰지 않습니다 — `Dialog`(§2.5)를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / Success / Error | Default | `Success`는 실제로는 초록이 아니라 `Primary/400` 파랑으로 렌더링됨(용인된 사실) |
| Button | Boolean | — | 우측 텍스트 링크 |
| Line | 1 / 2 | — | — |
| Icon | Boolean | — | — |

**Content**
- 컴포넌트 안에 디자이너가 직접 남긴 규칙 인용 — "toast 메시지 문구는 두줄까지 사용할 수 있습니다. 3줄 이상 노출은 지양해주세요."
- 버튼 링크는 "View"/"바로가기" 국영문 혼재 — 언어 컨벤션 ⚠️ 확인 필요
- 실사용 0곳

**동작 정의** — Toast는 실사용 0곳이라 실측할 실제 배치 사례가 없어, 아래는 Figma 실측이 아니라 개발 구현 규칙입니다.

| 항목 | 값 |
|---|---|
| 위치 | 화면 최하단에서 **16px** 고정. `Bottom Navigation`(87px 높이) 유무와 무관하게 항상 이 위치 — Bottom Navigation이 있는 화면에서도 그 위로 피하지 않고 **겹쳐서 뜹니다** |
| 노출 시간 | **3초** |
| 근거 | Toast 자체 폭 328px = 360px 화면폭 − 좌우 16px(기존 Spacing `16` 토큰과 일치). `Action Bar`(고정 CTA 바)도 이미 16px 하단 padding을 쓰고 있어 "화면 최하단 여백 16px" 관례와 일관됨 |

### 2.8 Item Card(, Review Card)

**비교**(카드 3종 — 이름만 보면 Grid와 Recommendation이 헷갈리기 쉬우니 주의, 아래 Recommendation 항목 참고)

| 항목 | Item Card / Grid | Item Card / Recommendation | Item Card / Cart |
|---|---|---|---|
| 카드 폭 | 160px | 130px | — |
| 배치 방식 | 360px 화면폭에 꽉 차는 2열 그리드 | 460px 폭 컨테이너의 횡스크롤 캐러셀(카드가 잘려서 노출) | 장바구니 전용 확장형(수량 조절·삭제 포함) |
| 실사용 위치 | "상품목록_그리드 타입" 화면 2열 그리드(8곳) | 장바구니 "다시 구매하세요"(2곳)·상세 "같이 둘러볼만한 상품"(3곳) | 장바구니 화면 |

`Price Block`/`Spec Row`/`Rating Display`는 이 카드들 내부에서 재사용되는 하위 컴포넌트이고, `Order Deadline`은 상품 단위, `Cart / Countdown Banner`(§2.6)는 장바구니 단위 카운트다운으로 역할이 나뉩니다.

**Item Card / Grid**

Figma: `6053:673`

목록에서 상품을 표현하는 카드 컴포넌트(grid 밀도)입니다. 이미지·Price Block·Rating Display·Spec Row를 내부에 품습니다(컴포넌트 자체 설명 인용). 카드 폭 160px.

**사용 가이드**
- ✅ Do: 카테고리 목록·검색 결과 등 실제 화면 폭(360px)에 꽉 차는 2열 그리드에서 사용합니다(횡스크롤 아님). 실사용 8곳 전부 "상품목록_그리드 타입" 화면(App Bar+필터 칩+정렬 드롭다운+2열 그리드 구성)에서 확인했습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| — | Variant 없는 단일 컴포넌트 | — | Type/Discount 등 속성 없음 — 아래 `Item Card / Recommendation`과 달리 아직 세분화돼 있지 않음. **후속 검토 후보**: 할인 유무 등 상태 분기가 필요해지면 Variant 추가 검토 |

**Content**
- 상세 실사용 콘텐츠 패턴은 이번 조사에서 확인하지 못함(⚠️ 확인 필요) — 다른 Item Card 계열과 동일한 상품명/가격 표기 규칙을 따를 것으로 추정되나 검증 전

**Item Card / Recommendation** (구 `Item Card / Grid Small`)

Figma: `6253:1706`

이미지·상품명·가격·배송·평점을 한 번에 스캔 가능하게 묶은 상품 카드입니다. 카드 폭 130px.

**사용 가이드**
- ✅ Do: 장바구니·상품 상세페이지 하단의 횡스크롤 추가구매 유도 캐러셀에 사용합니다. 실사용 15곳 전부 장바구니 "다시 구매하세요"(2곳)·상품 상세 "같이 둘러볼만한 상품"(3곳) 섹션의 가로 스크롤 행(460px 폭 컨테이너가 360px 화면보다 넓어 카드가 잘려서 노출)에서만 확인됐습니다.
- ❌ Don't: "카테고리 목록, 검색 결과" 용도가 아닙니다 — 그 역할은 위 `Item Card / Grid`가 담당합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Default / Button | Default | 하단 CTA 노출 여부 |
| Discount | Applied / None | — | — |

**Content**
- 상품명은 "브랜드명+제품명+옵션"을 쉼표로 이어 쓰는 패턴("도브 화이트피치 리밸런싱 바디워시, 1kg, 2개")
- 가격은 천단위 콤마+"원", 할인율은 정수 %
- 배송 배지는 Rocket Badge, 평점은 Rating Display 재사용

**Item Card / Cart**

Figma: `6067:825`

장바구니에서 담긴 상품 1건을 보여주는 확장형 카드(수량 조절·삭제·재고 상태 포함)입니다.

**사용 가이드**
- ✅ Do: 장바구니 화면 전용으로 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Stock | Available / OutOfStock | Available | `Available`은 체크박스+Quantity Stepper. `OutOfStock`은 체크박스 비활성+"재입고 알림 신청" 버튼+"일시품절" 라벨 |

**Content**
- 상품명 패턴은 `Item Card / Recommendation`과 동일
- "일시품절" 고정 문구(관찰 1건, 변형 여부 ⚠️ 확인 필요)

**Price Block**

Figma: `6065:640`

정가·할인율·최종가·단위가를 하나로 묶어 보여주는 가격 표시 전용 컴포넌트입니다(실사용 46곳, 이 그룹에서 가장 널리 재사용).

**사용 가이드**
- ✅ Do: 가격이 노출되는 모든 곳에서 재사용합니다 — 화면마다 가격 UI를 새로 조합하지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Discount | Applied / None | — | `Applied`는 정가 취소선+할인율 배지+최종가 `Error/400`(빨강)+단위가입니다. `None`은 최종가만 `Gray/900`+단위가입니다. **파랑(Primary)은 어느 상태에서도 쓰이지 않습니다** |
| Show Unit Price | Boolean | true | — |

**Content**
- 가격 천단위 콤마+"원", 할인율 정수 %, 단위가 "(100ml당 715원)" 형식
- 그람/밀리리터 등으로 단위 환산이 불가능한 상품은 `Show Unit Price=false`로 꺼서 단위가 줄 자체를 숨깁니다

**Spec Row**

Figma: `6041:519`

배송 유형과 적립 혜택을 한 줄로 보여주는 아이콘+텍스트 로우입니다.

**사용 가이드**
- ✅ Do: 상품 카드/상세에서 배송 조건·적립 혜택을 안내할 때 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Free / Tomorrow / Fresh / Seller / Shipping / Return | — | — |
| Show ETA | Boolean | — | Show Reward와 독립적으로 조합 가능 |
| Show Reward | Boolean | — | — |
| ETA Text / Reward Text | TEXT | — | — |

**Content**
- ETA "내일 도착"/"내일 새벽 도착"("~도착" 어미)
- Reward "최대 770원 적립"("최대 N원 적립" 형식)

**Rating Display**

Figma: `6047:463`

별 아이콘+평점 숫자+리뷰 수를 한 세트로 보여주는 평가 요약 컴포넌트입니다(실사용 38곳).

**사용 가이드**
- ✅ Do: 평점 정보가 필요한 모든 곳에서 공통 재사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Review | Filled / Empty | Filled | `Filled`는 주황 별+평점+리뷰수. `Empty`는 회색 별+"−"+"(0)", 데이터 없음을 0이 아닌 "−"로 명시 구분 |
| Rating / Review Count | TEXT | — | — |

**Content**
- 평점은 소수 둘째 자리("4.83")
- 리뷰 수 9,999건 초과는 "(9,999+)" 상한 표기, 이하는 실수 그대로("(28,915)" 사례 관찰 — 정확한 임계값 규칙 ⚠️ 확인 필요)

**Order Deadline**

Figma: `6294:11695`

오늘 특정 시각까지 주문하면 언제 도착하는지 알려주는 카운트다운형 안내입니다. 상품 하나에 붙는 짧은 인라인 텍스트입니다.

**사용 가이드**
- ✅ Do: 로켓 배송처럼 주문 마감이 배송일에 영향을 주는 상품에서 구매를 서두르게 유도할 때 사용합니다.
- ❌ Don't: 장바구니 전체 단위 안내(체크박스·펼침 필요)에는 쓰지 않습니다 — `Cart / Countdown Banner`(§2.6)를 씁니다. 둘 다 빨간 카운트다운을 쓰지만 단위가 다릅니다(상품 단위 vs 장바구니 단위).

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Default / WithDiscount | Default | `Default`는 시계+"00:42:25 내 주문 시". `WithDiscount`는 앞에 "할인 · " 접두 |

**Content**
- "시:분:초 내 주문 시" 고정 형식
- 색상은 항상 Error(빨강)

**Review / Card**

Figma: `6013:12578`

개별 리뷰 1건 카드(평점·태그·작성자·본문·이미지·"도움이 돼요" 버튼 포함)입니다.

**사용 가이드**
- ✅ Do: 상품 상세의 리뷰 목록에 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Photo / Only Text | Photo | — |
| Show Status Badge | Boolean | true | 내부 `Status Badge`가 이미 자기 `Type`(OneMonthUse/Repurchase) 프로퍼티를 갖고 있어 Review/Card 자체의 `Status` 축은 중복입니다. 특정 신뢰 신호 종류를 바꾸고 싶으면 Review/Card의 variant가 아니라 **내부 `Status Badge` 인스턴스를 직접 선택해 그 `Type`을 override**하면 됩니다. `Status` 축 자체는 값이 1개(`OneMonthUse`)만 남았지만 Figma API 제약으로 완전 제거는 못 했습니다 |

**Content**
- 상단 태그는 "라벨 값" 페어를 `|`로 구분해 나열
- 본문 2~3문장, 자동 줄바꿈
- 작성자 "윤**"(성+마스킹), 날짜 "2026.09.01"(점 구분)

**Review / Summary**

Figma: `6013:12529`

상품 전체 리뷰 요약 위젯(평균 평점·총수·세부 만족도 막대그래프)입니다.

**사용 가이드**
- ✅ Do: 리뷰 섹션 최상단, 개별 리뷰 목록 진입 전 요약 지점에 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| status | Default / none photo / no rating | Default | `no rating`은 Rating Display의 Empty와 동일 표기 관례로 추정하며, `none photo`는 명확히 구분하지 못했습니다(⚠️ 확인 필요) |
| Show Row 1~4 | Boolean | true | **카테고리 유연성**: 4개 속성 행(향 만족도/거품/보습력/세정력)이 모든 카테고리에 다 맞진 않으므로, 해당 카테고리에 안 맞는 행은 Boolean으로 끄고 남은 행의 라벨·정성평가·퍼센트 텍스트는 인스턴스에서 직접 override합니다(별도 텍스트 프로퍼티 노출 없이도 텍스트 레이어라 항상 override 가능 — `Spec Row`처럼 텍스트까지 전부 프로퍼티화하진 않았습니다, 과도한 프로퍼티 증식 방지) |

**Content**
- 세부 항목명(향 만족도/거품/보습력/세정력) + 정성 평가 문구("아주 만족해요") + 퍼센트 막대
- 실사용 1곳(상품 상세페이지)

### 2.9 Label

**비교** — `Label`/`Chip`/`Rocket Badge`/`Status Badge`/`Spec Row`는 전부 "짧은 태그성 정보"를 보여준다는 점에서 겹쳐 보이지만, 이미 전용 도메인·전용 슬롯이 있는지로 갈립니다.

| 항목 | Label | Chip | Rocket Badge | Status Badge | Spec Row |
|---|---|---|---|---|---|
| 도메인 | 범용(신규 태그는 기본적으로 여기) | 필터링·옵션 선택(인터랙티브) | 배송 전용(고정 4종) | 리뷰 신뢰 신호 전용(고정, 확장 시 값 추가) | 배송+적립 혜택 전용(`Item Card` 내장) |
| 형태 | 독립 pill/outline 태그 | 필터 칩(선택 상태 있음) | 살짝 둥근 사각형(`small`, 4) | outline 배지(배경 없음) | 아이콘+텍스트 한 줄(배지 아님) |
| 상호작용 | 없음 | 있음(Default/Selected/Pressed) | 없음 | 없음 | 없음 |
| 신규 태그를 여기 추가? | ✅ 기본값 | 필터링 목적일 때만 | ❌ 배송은 전용 컴포넌트가 있음 | ❌ 리뷰 신뢰 신호는 전용 컴포넌트가 있음 | ❌ 구조가 고정 로우라 확장 대상 아님 |

`Label`과 `Spec Row`가 둘 다 "배송/반품" 어휘(`Shipping`/`Return`)를 쓰는 건 우연이며, 실제로는 서로 다른 슬롯입니다 — `Spec Row`는 `Item Card` 안에 고정으로 들어가는 배송+적립 전용 로우이고, `Label`은 독립적으로 붙는 낱개 태그입니다.

**Label**

Figma: `6527:38619`

상품 관련 짧은 안내 문구를 표시하는 범용 라벨입니다(프로모션/배송정책/구매빈도 등 다목적).

**사용 가이드**
- ✅ Do: 상품 카드/상세에서 "특가 진행 중", "무료배송/반품 여부", "누적 구매 횟수" 등 부가 정보를 한 줄로 붙일 때 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Promotion(특가진행중·빨강 outline) / Return(무료반품·회색 배경) / Shipping(무료배송·파란 배경) / RepeatPurchase(N회 구매·회색 배경) | — | — |

**Content**
- 4~6자 단문
- "무료OO"(무료반품/무료배송)와 "N회 구매"(카운트+명사) 두 패턴 — 새 Type 추가 시 이 중 하나를 따르는 것을 권장(강제 규칙 아님)

### 2.10 List

**비교**(셋 다 카테고리 탐색과 관련되지만 레이아웃 형태가 다릅니다)

| 항목 | List | Category Tab | Category Menu Item |
|---|---|---|---|
| 형태 | 세로 리스트 row | 사이드바형 탭 아이템 | 이미지+라벨 그리드 아이템 |
| 선택 표현 | `Status`(Default/pressed) | 텍스트 색·굵기(배경 채움 아님) | — (variant 없음) |
| 실사용 맥락 | 카테고리 전체보기, 검색 자동완성·최근 검색어 | 좌측 고정 사이드바 카테고리 내비게이션 | `Bottom Sheet` 하위 카테고리 3열 그리드 |

**List**

Figma: `6527:40035`

텍스트 기반 선택 항목을 세로로 나열하는 범용 리스트 행(row) 컴포넌트입니다.

**사용 가이드**
- ✅ Do: 상품 카테고리 전체보기, 검색 자동완성·최근 검색어처럼 텍스트 위주 세로 목록에 사용합니다.
- ❌ Don't: 이미지 중심 그리드에는 `Category Menu Item`, 탭형 선택에는 `Category Tab`을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| type | Default / Image(좌측 썸네일) / history(시계 아이콘+삭제 X) | Default | — |
| Status | Default / pressed(배경 회색) | Default | — |

**Content**
- `Default`/`Image` 타입은 "냉동 블루베리"처럼 짧은 명사구(8자 내외) 패턴이 관찰되지만, **`history` 타입(검색 기록)에는 이 글자 수 규칙이 적용되지 않습니다.** 실제 `history` 인스턴스 텍스트가 "삼성전자 615L 2도어 냉장고"(18자)로 8자를 훌쩍 넘습니다 — 검색 기록은 사용자가 입력한 자유 텍스트라 길이가 일정하지 않은 게 당연합니다.
- 대신 텍스트가 폭을 넘으면 말줄임(ellipsis) 처리됩니다.

**Category Tab**

Figma: `6514:37753`

카테고리 사이드바에서 카테고리 1개를 선택하는 탭 아이템입니다.

**사용 가이드**
- ✅ Do: 좌측 고정 사이드바형 카테고리 내비게이션(`Bottom Sheet` 카테고리 선택 등)에 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default(회색 배경+회색 텍스트) / Selected(흰 배경+파란 굵은 텍스트) | Default | 배경 채움이 아니라 텍스트 색·굵기로 선택 표현(다른 Chip류와 다른 패턴) |

**Content**
- "식품"(2음절)부터 "패션의류/잡화"(슬래시 결합)까지 — 카테고리명 원문 그대로, 별도 축약 규칙 없음

**Category Menu Item**

Figma: `6527:38312`

이미지 썸네일+라벨로 구성된 그리드형 카테고리/메뉴 항목입니다.

**사용 가이드**
- ✅ Do: `Bottom Sheet` 카테고리 화면의 하위 카테고리 그리드처럼 이미지와 함께 나열해야 할 때 사용합니다(실제 화면에서 3열 그리드로 배치됩니다).

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| — | Variant 없음(단일 컴포넌트) | — | 이미지 자리는 현재 빈 placeholder |

**Content**
- ⚠️ 여전히 실제 콘텐츠 예시가 없습니다 — 실제 화면의 인스턴스들도 전부 마스터 placeholder "카테고리" 그대로입니다. 실제 카테고리명이 들어간 사례가 파일 전체에 단 한 곳도 없어 글자 수·말줄임 규칙을 실측으로 정할 수 없는 상태입니다 — 실제 카테고리명 예시가 확보되면 정식 규칙을 작성할 예정입니다
- 잠정적으로는 같은 카테고리명을 표시하는 `Category Tab`의 실측 규칙("식품"~"패션의류/잡화", 원문 그대로·별도 축약 없음)을 참고 규칙으로 준용할 것을 권장

### 2.11 Navigation

**Bottom Navigation**

Figma: `6501:36218`

앱 최하단에 고정되어 주요 화면(홈/카테고리/검색/마이페이지/장바구니) 간 전환을 담당하는 전역 내비게이션 바입니다.

**사용 가이드**
- ✅ Do: 앱 최상위 5개 화면을 오갈 때 항상 하단 고정으로 사용합니다.
- ❌ Don't: 하위 화면(상품 상세 등)에는 일반적으로 노출하지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Platform | iOS / Android | iOS | `iOS`는 하단 홈 인디케이터 바 추가, `Android`는 없음. 내부 탭 5개는 Bottom Navigation Item 인스턴스 조합 |

**Content**
- 탭 라벨 2~4자 고정 명사(홈/카테고리/검색/마이페이지/장바구니), 항상 5개·순서 고정

**Bottom Navigation Item**

Figma: `6501:35862`

Bottom Navigation을 구성하는 개별 탭(아이콘+라벨)입니다. 선택 여부를 색으로 표현합니다.

**사용 가이드**
- ✅ Do: Bottom Navigation 내부 전용으로 사용합니다.
- ❌ Don't: 단독 배치하지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Icon | Home / Category / Search / Mypage / Cart | — | — |
| State | On(선택·파랑 filled) / Off(미선택·회색 outline) / Pressed(누르는 순간·연회색 배경) | Off | 아이콘은 On일 때만 filled |

**Content**
- 라벨은 5개로 고정, 그 외 텍스트 없음

### 2.12 Bottom Sheet

**비교**(`Show Scroll` 유무 차이의 설계 근거는 아래 "스크롤 동작" 참고)

| 항목 | Bottom Sheet / Informational | Bottom Sheet / Interactive |
|---|---|---|
| 구 이름 | `Sheet / Confirmation` | `Sheet / Selector` |
| 목적 | 순수 텍스트 전달 전용 | 교차판매/업셀 확인(`Type=Item`) 또는 전체 화면급 필터 선택(`Type=Filter`) |
| Variant 축 | `Status`×`Button`+`Show Icons` | `Type`(Item/Filter) |
| Show Scroll | 있음(6개 variant 전부, 구분선+스크롤바 4요소) | 없음(리스트·캐러셀 형태가 스크롤을 암시) |
| 실사용 | 1건(전부 더미 텍스트) | 0곳(컴포넌트 내 예시 기준) |

**Bottom Sheet / Informational** — 순수 텍스트 전달 전용

Figma: `4043:2686`

화면 하단에서 올라오는 `Bottom Sheet` 형태의 확인창입니다. `Dialog`와 구조는 유사하나 하단 진입 + 드래그 핸들을 보유합니다. 제목+본문 설명 외 이미지·목록·복합 위젯은 다루지 않습니다.

**사용 가이드**
- ✅ Do: `Dialog`와 같은 확인·안내 목적이나 설명이 길거나 한 손 조작에 더 적합한 하단 진입이 필요할 때 사용합니다.
- ❌ Don't: 목록 선택이나 필터링, 교차판매 유도처럼 상호작용이 필요하면 쓰지 않습니다 — `Bottom Sheet / Interactive`를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / Error / Warning | Default | — |
| Button | 1 Button / 2 Button | 1 Button | — |
| Show Icons | Boolean | — | — |
| Show Scroll | Boolean | false | Dialog(§2.5)와 동일 패턴(아래 "스크롤 동작" 참고) |
| Title text / Discription | TEXT | — | `Discription`은 Figma 실제 프로퍼티명에 있는 기존 오탈자(정상 철자 Description) — 이번 작업 범위 밖이라 임의로 고치지 않고 그대로 표기 |

헤더 상단 `large`(16), 드래그 핸들 `full`(pill).

**Content**
- 순수 텍스트 전달 전용 — 제목+본문 설명 외 이미지·목록·복합 위젯 없음
- 실사용 1건도 전부 더미 텍스트("제목을 입력해주세요" 등) — 실제 프로덕션 카피 사례 없음, 확정된 규칙 없이 "추후 실사용 발생 시 보강 필요"로 남김

**Bottom Sheet / Interactive** — 교차판매/업셀 확인, 전체 화면급 필터 선택

Figma: `6005:6973`

화면 하단에서 올라오는 `Bottom Sheet` 형태의 선택/목록 UI입니다. `Type=Item`은 장바구니 담기 완료 후 "다른 고객이 함께 구매한 상품"을 보여주는 교차판매·업셀 확인, `Type=Filter`는 전체 화면급 필터 옵션 선택입니다.

**사용 가이드**
- ✅ Do: 장바구니에 담긴 직후 추가구매를 유도해야 할 때(`Type=Item`), 검색/필터 조건을 화면 전체 규모로 선택해야 할 때(`Type=Filter`) 사용합니다.
- ❌ Don't: 단순 확인·안내에는 쓰지 않습니다 — `Bottom Sheet / Informational`을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Item / Filter | Item | 둘 다 헤더 상단 `large`(16) |

**`Show Scroll`은 없습니다** — 리스트·캐러셀 형태 자체가 이미 스크롤을 암시해 별도 인디케이터가 불필요하다는 판단입니다(근거는 아래 "스크롤 동작" 참고).

**Content**
- 완료 안내 "상품을 ~했어요"(완료형 어미), 아래 교차판매성 추가 상품 리스트 동반
- 필터 옵션 라벨 2~5자("전체","로켓","무료배송")
- 검색 placeholder "~를 검색하세요"
- 실사용 0곳 — 컴포넌트 내 예시 기준, 확정 규칙 아님

**높이 제한** — `Bottom Sheet`(Informational/Interactive 공통)는 화면 전체를 채우지 않고, 화면 높이의 최대 80%까지만 차지합니다. `Dialog`의 70vh 규칙(§2.5)과 같은 이유로 Figma 프레임 자체에는 하드 제약을 걸지 않고, 아래는 **개발 구현 규칙**으로 문서화합니다.

*높이 기준 — 상대 단위 기준(고정 px 하드코딩 금지, Dialog와 동일 원칙)*

| 항목 | 값 |
|---|---|
| `Bottom Sheet` 전체 높이 | `max-height: 80vh` |
| 스크롤 트리거 | 콘텐츠 실제 높이가 `80vh`를 초과하면 body 영역에 `overflow-y: auto` 적용 |

> 참고: 이 파일의 표준 모바일 아트보드(360×800px, `★ 상품목록_그리드 타입` 등) 기준으로 환산하면 80vh ≈ 640px이며, 파일 내에 이미 이 값을 보여주는 360×640 참고 프레임(주석 "bottom sheet의 전체 길이는 화면의 80%를 넘어가지 않는다")이 존재합니다 — 이는 계산 예시일 뿐 실제 트리거 조건으로 하드코딩하지 않습니다(뷰포트가 다르면 값도 달라집니다).

**스크롤 동작(Informational 전용)** — `Dialog`(§2.5)의 스크롤 UI 패턴을 `Bottom Sheet / Informational`에만 적용합니다. **`Bottom Sheet / Interactive`는 `Show Scroll` 자체가 없습니다.**

*근거 — 왜 Informational만 스크롤 인디케이터가 필요한가*: 두 컴포넌트는 콘텐츠 성격이 근본적으로 다릅니다.
- **Informational은 길이를 예측할 수 없는 자유 텍스트입니다.** 제목+본문 설명(`Discription`)은 글자 수 제한이 없어 짧을 수도, 화면을 넘길 만큼 길 수도 있습니다. 텍스트만 봐서는 "지금 보이는 게 전부인지, 아래에 더 있는지" 사용자가 판단할 수 없으므로, 구분선+스크롤바로 스크롤 경계를 명시적으로 알려줄 필요가 있습니다.
- **Interactive는 콘텐츠 형태 자체가 이미 스크롤 가능함을 암시합니다.** `Type=Filter`는 필터 칩·카테고리 목록처럼 리스트가 스크롤되는 게 당연한 패턴이고(body가 원래부터 고정 408px+`overflow-clip`으로 실제 스크롤되는 구조입니다 — 아래 참고, `Show Scroll`과 무관), `Type=Item`은 가로 스크롤 캐러셀이라 카드가 화면 끝에서 잘려 보이는 것 자체가 "더 있다"는 신호입니다. 이는 이 파일의 다른 캐러셀(`Item Card / Recommendation` 등, §2.8)도 전부 별도 스크롤바 없이 같은 방식으로 스크롤을 암시하는 것과 일관됩니다.
- 즉 `Show Scroll`은 "콘텐츠 길이가 가변적인 순수 텍스트 컴포넌트"에만 필요하고, 리스트·캐러셀처럼 형태 자체가 스크롤을 암시하는 컴포넌트에는 불필요하다는 원칙입니다.

Figma는 콘텐츠량에 따라 동적으로 스크롤 여부를 계산하지 못하므로, Informational은 `Show Scroll` Boolean으로 두 상태(스크롤 없음/스크롤 발생 중)만 미리보기합니다.

*스크롤 UI 요소* (`Show Scroll=true`일 때 노출, `Bottom Sheet / Informational` 6개 variant 전부)
- **상단 구분선**(`divider-top`) — 헤더(드래그 핸들)와 body 경계입니다. `Background/Divider` 토큰을 바인딩했습니다.
- **하단 구분선**(`divider-bottom`) — body와 하단 버튼 영역 경계입니다. `Background/Divider` 토큰을 바인딩했습니다. 6개 variant 전부 버튼 영역이 존재해 예외 없이 적용됩니다(`Dialog`의 `Button=X` 같은 생략 케이스 없음).
- **스크롤바**(`scroll-track`+`scroll-thumb`) — `scroll-thumb`은 `Gray/400`을 바인딩했고, `scroll-track`은 `Dialog`와 동일 원칙으로 의도적으로 미바인딩했습니다(반투명 검정 6% opacity 표현 기법이 토큰 색상값과 안 맞음).
- **Body clipping** — body 프레임에 `clipsContent:true`를 적용했습니다(원래 Hug라 신규 부여).

`Bottom Sheet / Interactive`의 `Type=Filter`는 원래부터 있던 고정 408px+`overflow-clip` body(내부 콘텐츠 724px로 실제 스크롤 상황)를 그대로 유지합니다 — 이건 `Show Scroll`과 무관한 기존 구조입니다.

### 2.13 Tab

**비교**: `Tab Group`은 같은 데이터를 다른 관점으로 재구성해 보여주는 컨테이너이고, `Tab Item`은 그 안의 개별 탭 버튼입니다(독립 배치 사례 없음, 아래 참고). 상품 카테고리 자체를 나열하는 용도가 아니라는 점에서 `Category Tab`(§2.10)과 역할이 다릅니다.

**Tab Group**

Figma: `6009:10202`

같은 종류의 콘텐츠 목록을 서로 다른 관점(필터 기준)으로 전환해서 보여주는 가로 탭 컨테이너입니다. 별개의 카테고리를 나열하는 게 아니라 **같은 데이터를 다른 기준으로 재구성**해서 보여주는 용도입니다 — 실사용 유일 맥락은 장바구니 화면에서 "지금 화면에 보이는 상품 목록의 출처"를 전환하는 것(아래 참고).

**사용 가이드**
- ✅ Do: 한 화면 안에서 같은 성격의 리스트를 여러 관점으로 오갈 때 사용합니다. 실사용 4곳 전부 장바구니 화면(`★ 장바구니`) 하나의 패턴이고, 전부 `Type=3 Tab`으로 "일반구매(6)"(Status=Selected, 기본 활성 탭)/"자주산상품"/"찜한상품(40)"을 전환합니다 — 일반 구매 흐름 상품, 자주 구매한 상품, 찜한 상품이라는 사용자 개인의 구매·관심 이력을 관점별로 보여줍니다.
- ❌ Don't: 상품 카테고리(식품/뷰티 등)를 나열하는 용도로 쓰지 않습니다 — 카테고리 나열에는 `Category Tab`(§2.10)을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | 2 Tab / 3 Tab / Swipe | — | `3 Tab`만 실사용 확인(4곳), `2 Tab`·`Swipe`는 라이브러리에 정의만 있고 실사용 0곳입니다 — `Swipe`의 "가로 스크롤 다수 탭" 동작은 구조상 그렇게 만들어져 있을 뿐 실제로 몇 개 탭에서 그렇게 쓰이는지 확인된 사례가 없습니다 |

**Content**
- 실측 근거가 있는 건 `3 Tab`의 라벨 패턴뿐 — "일반구매(6)"/"찜한상품(40)"처럼 `라벨(숫자)` 형태로 항목 수를 병기하거나("자주산상품"처럼 숫자 없이도 씀), 라벨 길이 4~7자 내외
- `2 Tab`/`Swipe`를 실제로 언제 쓸지는 후속 실사용 발생 시 보강이 필요합니다

**Tab Item**

Figma: `6009:10187`

Tab Group을 구성하는 개별 탭 버튼(라벨+선택 여부)입니다.

**사용 가이드**
- ✅ Do: Tab Group 내부 전용으로 사용합니다. 파일 내 Tab Item 인스턴스는 전부(23곳) Tab Group의 마스터 variant 내부 자식으로만 존재하며, 독립 배치 사례가 없습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default(회색) / Selected(파란 굵은 텍스트+밑줄) / Pressed(회색 배경 하이라이트) | Default | — |

**Content**
- "일반구매(6)"처럼 `라벨(숫자)` 형태로 항목 수 병기(의미 있는 경우만, 전부는 아님 — "자주산상품"은 숫자 없음)
- 라벨 길이 4~7자 내외

### 2.14 Thumbnail

**Thumbnail**

Figma: `6502:36818`

범용 이미지 자리 표시자 컴포넌트입니다. 실제 이미지가 들어갈 정사각형 영역을 크기별로 표준화합니다.

**사용 가이드**
- ✅ Do: 상품 이미지, 카테고리 아이콘 등 정사각형 이미지가 필요한 모든 곳에 사용합니다.
- ❌ Don't: Item Card처럼 이미지가 내장된 복합 컴포넌트에는 별도로 끼워 넣지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Size | xsmall / small / medium / large / Xlarge | — | 정확한 픽셀값은 추가 실측 ⚠️ 필요. 전 사이즈 공통으로 회색 배경+이미지 placeholder 아이콘 |

**Content**
- 텍스트 콘텐츠 없음(순수 이미지 컨테이너)
- 이미지 없을 때 항상 동일한 회색+아이콘 placeholder

### 2.15 Radio / Checkbox

⚠️ **토글 가능한 인터랙티브 Radio·Checkbox 컴포넌트는 없습니다.** 진짜 `Radio`(Status=Default/Selected) 컴포넌트는 삭제되어 `Icon / Radio`만 남아있고 실사용은 0곳입니다. `Icon / Checkbox`(`6020:13544`)는 실제로 존재하며 실사용까지 있습니다(아래 참고).

**Icon / Radio**

Figma: `6573:3439`

장식용 아이콘 글리프입니다. `Status=Default`(파란 원)/`Disabled`(회색 원) 두 상태뿐이며, 실제 "선택됨/선택안됨"을 토글하는 상호작용 상태가 아닙니다(진짜 `Radio` 컴포넌트가 삭제된 경위는 ⚠️ 확인 필요).

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default(파란 원) / Disabled(회색 원) | Default | 아이콘 색상 변형일 뿐 "선택됨/선택안됨" 상호작용 상태가 아님 |

**Content**
- 텍스트 없음(순수 아이콘)
- 실사용 0곳

**Icon / Checkbox**

Figma: `6020:13544`

`Icon / Radio`와 동일한 구조적 한계를 가진 장식용 `Icon / Checkbox` 글리프입니다. 두 variant(`Status=Default`/`Disabled`) 전부 내부 구조가 단일 VECTOR 하나뿐이고, 둘 다 "체크됨(✓)" 모양이 고정으로 그려져 있습니다 — **"선택 안 됨(빈 박스)"을 표현하는 variant 자체가 없어** 실제 체크/언체크 토글을 구현할 수 없습니다.

**사용 가이드**
- 실사용: `Item Card / Cart`(§2.8) 자체 마스터에 내장돼 있습니다 — `Stock=Available`(`6067:664`)은 `Status=Default`(파란 체크)를, `Stock=OutOfStock`(`6067:791`)은 `Status=Disabled`(회색 체크)를 씁니다. 장바구니 화면(`★ 장바구니`)의 실제 상품 리스트 인스턴스 4곳에서도 `Status=Default`로 쓰입니다. `Cart / Selection Toolbar`(§2.6)도 마스터+인스턴스 3곳에서 `Status=Disabled`로 내장해 사용 중입니다.
- ❌ Don't: 이 글리프만으로는 "선택 해제(빈 박스)" 상태를 표현할 수 없으므로, 실제로 체크/언체크를 토글해야 하는 다중 선택 UI에는 아직 쓸 수 없습니다 — 신규 제작이 필요합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default(파란 체크) / Disabled(회색 체크) | Default | 둘 다 "체크됨" 모양 고정 — Unchecked(빈 박스) variant 자체가 없음 |

**Content**
- 텍스트 없음(순수 아이콘)
- 실사용: `Item Card / Cart` 마스터 2곳(Available/OutOfStock) + 장바구니 화면 실제 인스턴스 4곳 + `Cart / Selection Toolbar` 마스터/인스턴스 3곳 — **실사용 0곳이 아닙니다**

**신규 착수 필요** — 진짜 토글 가능한 Radio/Checkbox는 여전히 0/2입니다. 다만 Checkbox는 완전한 백지 상태가 아니라, "체크됨 상태만 있는 반쪽짜리 아이콘 글리프가 이미 여러 곳에서 실사용 중"이라는 더 구체적인 출발점이 있습니다 — 새로 만들 때 이 기존 체크 아이콘 비주얼을 재사용하면서 Unchecked 상태를 추가하는 방식으로 접근할 수 있습니다.

---

## 3. Naming Convention

**새 컴포넌트를 만들 때 이 규칙을 그대로 따른다.**

- **컴포넌트 이름**: Title Case + 공백(예: `Section Header`, `Action Bar`, `Button`). snake_case·kebab-case·소문자 단일 단어는 쓰지 않는다.
- **패밀리(변형이 여러 세트로 나뉘는 경우)**: `이름 / 하위이름` 형식, 슬래시 앞뒤 공백 포함(예: `Option Chips / Size`, `Icon / Checkbox`).
- **Property 이름**:
  - 상태·선택 여부를 나타내는 축 → **`Status`**(Title Case) 하나로 통일. Default/Selected/Pressed/Error/Warning 등 성격이 달라도 전부 이 키를 쓴다.
  - 콘텐츠·글리프 종류를 나타내는 축(상태가 아닌 것) → **`Type`**.
  - 크기 축 → `Size`.
  - **Boolean 축** → **`Show {명사}`** 접두사로 통일한다 — 실제 예시: `Show Icons`(Bottom Sheet), `Show Scroll`(Dialog/Bottom Sheet), `Show Unit Price`(Price Block), `Show Status Badge`(Review/Card), `Show Row 1`~`Show Row 4`(Review/Summary), `Show Leading Icon`/`Show Trailing Icon`(Button 계열). 반례 없다.
  - Figma 기본값(`Property 1`, `Variant2` 등)을 그대로 남기지 않는다 — 반드시 의미 있는 이름으로 교체.
- **Variant vs Boolean 선택 기준**: 값이 2개이고 차이가 "요소 하나의 visible on/off"뿐이면 Boolean(`Show {명사}`)을 쓴다. 값이 3개 이상이거나, 값이 바뀔 때 색상·아이콘·텍스트·레이아웃 등 여러 속성이 함께 바뀌면 Variant를 쓴다(예: Dialog `Status=Error/Warning`, Review/Card `Type=Photo/OnlyText`, Button `Size`, Dialog `Button=1 Button/2 Button/X`). **알려진 제약**: Variant를 Boolean으로 전환하면서 원래 축에 값이 1개만 남는 경우(Section Header `State`, Review/Card `Status`) Figma API가 COMPONENT_SET의 마지막 남은 속성 축 삭제를 막아 빈 축이 남는다 — 설계 원칙 위반이 아니라 API 제약이다.
- **Variant 값**: 영문 식별자 사용(한글 서술형 값 금지 — 예: `한달사용` ❌ → `OneMonthUse` ⭕). 실제 화면 표시 텍스트는 별도 텍스트 레이어로 분리한다.
- **컴포넌트를 새로 만들거나 옮길 때**: 반드시 Instance로 참조한다. 컴포넌트 원본(COMPONENT/COMPONENT_SET)을 다른 페이지에 복제하지 않는다.
- **이름이 같거나 비슷해 보여도** 병합·삭제 전에 반드시 스크린샷이나 실제 콘텐츠 조회로 진짜 중복인지 확인한다(이름만 보고 판단하지 않는다).
- **새 Property·컴포넌트 이름을 추가하기 직전에 철자를 한 번 더 확인한다** — `Bottom Sheet / Informational`의 `Discription`(정상 철자 Description) 오탈자가 이미 실제 Figma 프로퍼티명으로 굳어져 있다(§2.12 참고). `addComponentProperty`/컴포넌트 이름 변경 직후 `componentPropertyDefinitions`를 재조회해 이름 철자를 육안으로 재확인하는 것을 권장한다 — 한 번 실사용에 반영되면 이름 변경이 인스턴스 오버라이드에 영향을 줄 수 있어 되돌리는 비용이 처음 확인하는 비용보다 훨씬 크다.

**Foundation 토큰 이름 — 관찰된 현재 상태(통일된 규칙 아님)**

위 규칙은 컴포넌트/프로퍼티 이름에 관한 것이고, Foundation(§1) 토큰 이름은 카테고리마다 실제로 서로 다른 표기 관례를 쓰고 있다 — 임의로 하나의 규칙으로 지어내지 않고 현재 실측 상태 그대로 기록한다:

| 카테고리 | 실제 패턴 | 예시 |
|---|---|---|
| Color · Semantic | Title Case + 슬래시(`카테고리/역할`) | `Background/Divider`, `Text/Primary`, `State/Primary Pressed` |
| Typography | 소문자 + 슬래시 + 하이픈(`카테고리/역할-수식어`) | `heading/xlarge`, `body/large-bold`, `label/xsmall-medium` |
| Radius | 소문자 단일 단어(계층 없음) | `none`, `small`, `medium`, `large`, `full` |
| Spacing | 숫자 그대로(이름 레이어 없음) | `4`, `8`, `16`, `20` |

**결정**: Semantic Color(Title Case)와 Typography(소문자)가 같은 "토큰"인데 대소문자 표기 관례가 다르지만, 강제로 통일하지 않고 카테고리별 현행 관례를 그대로 유지하기로 결정했다.

---

## 4. Design Principles

`design-language.md`의 8개 핵심 원칙(전체 관찰 근거는 원본 참고) + 이후 컴포넌트 리뷰 과정에서 확인된 신규 원칙 3개, 총 11개다.

1. **강조는 배경색 채움을 최상위 수단으로 아껴 쓴다** — 화면 전체에서 배경이 채워지는 요소는 CTA와 할인율 배지뿐이다.
2. **확정 금액=Error(빨강)/Gray(검정), 위험/할인=빨강으로 색의 의미가 고정돼 있다.** 파랑(Primary)은 CTA·링크 등 "행동" 전용이고, 가격 강조는 Error/Gray가 맡는다.
3. **인터랙션 가능한 작은 요소일수록 더 둥글다** — `full`(Chip/Radio/Icon Button 등) > `medium`(Button Large/Medium, 8) > `small`(Button Small/XSmall, 4) > `none`(Card/Action Bar, 0). 모달형 오버레이(Dialog/Sheet)의 상단 모서리는 별도로 `large`(16).
4. **깊이는 원칙적으로 그림자가 아니라 선으로 표현한다.** 다만 `Toast`(blur)와 `Bottom Navigation`(shadow)에 실제 예외가 존재한다 — 화면 위에 뜨는 고정 오버레이 성격의 컴포넌트에 한정된 예외로 잠정 분류한다.
5. **Commerce 숫자는 크기보다 weight로 위계를 나눈다.**
6. **여백은 16(화면 마진)/8(카드 내부) 두 값이 대부분을 지배하고, 버튼·칩류만 예외를 허용한다.**
7. **정보 밀도가 높아지면 리스트+divider 또는 가로 캐러셀로 낮춘다**, 새 레이아웃 패턴을 만들지 않는다.
8. **아이콘은 역할에 따라 outline(컨트롤)과 filled(정보성 배지)로 나뉜다.**
9. **모달형 오버레이의 높이는 뷰포트 상대 단위로 제한하고, 스크롤 인디케이터는 콘텐츠 형태에 따라 선택적으로 붙인다.** 높이 상한은 고정 px가 아니라 `vh`로 계산한다(Dialog `70vh`, Bottom Sheet `80vh` — 둘 다 독립적으로 이 규칙에 수렴했다). 구분선+스크롤바로 구성된 스크롤 인디케이터(`Show Scroll` Boolean)는 길이를 예측할 수 없는 자유 텍스트 콘텐츠(Dialog, `Bottom Sheet / Informational`)에만 붙이고, 리스트·캐러셀처럼 형태 자체가 이미 스크롤 가능함을 암시하는 콘텐츠(`Bottom Sheet / Interactive`, `Item Card / Recommendation` 등의 캐러셀)에는 붙이지 않는다.
10. **탭형 선택 UI는 역할에 따라 컴포넌트가 분리된다.** `Tab Group`은 같은 데이터를 다른 관점으로 재구성해 보여줄 때(실사용 예: 장바구니의 일반구매/자주산상품/찜한상품 — 사용자 개인의 구매·관심 이력 전환), `Category Tab`/`Chip`은 서로 다른 카테고리·옵션 자체를 나열해 고를 때 쓴다. 겉보기엔 둘 다 "여러 개 중 하나를 고르는 가로 UI"라 헷갈리기 쉽지만, "같은 데이터를 다른 렌즈로 보는가" 대 "서로 다른 대상을 고르는가"로 구분된다.
11. **자유 텍스트가 들어가는 슬롯은 넘침 처리를 반드시 정한다.** 한 줄로 보여줄 슬롯(제목·라벨·메뉴명 등)이 자유 텍스트(사용자 입력·카탈로그 데이터)라면 기본값은 말줄임(`textTruncation: ENDING`, 실사용례: `List`의 `history` 타입, `Option Chips / Thumbnail`)이다. 여러 줄을 허용할 슬롯(설명·본문)은 자동 줄바꿈을 기본으로 하고, 가능하면 최대 줄 수까지 정해 그 이상은 라인클램프한다. 디자인 시스템이 직접 통제하는 고정 짧은 문구(Label/Badge류)는 대상이 아니다.

---

## 5. 아직 없는 것 / 후속 과제

**컴포넌트**
- **Checkbox/Radio 진짜 인터랙티브 버전** — `Icon / Checkbox`/`Icon / Radio` 둘 다 "체크됨/선택됨" 모양만 있는 장식용 글리프라 Unchecked/미선택 상태를 표현 못 함(§2.15 참고) — 기존 체크 아이콘을 재사용해 Unchecked variant를 추가하는 방식으로 신규 제작 필요
- PRD §11.2가 예상했던 컴포넌트 중 여전히 전무한 것: Divider(독립 컴포넌트, 현재는 ad hoc 라인만 존재) · Accordion · Anchor Navigation · Carousel + Indicator · Ratio Visualization · Empty State · Skeleton · Promotion Badge(Rocket/Status Badge와 별개) · Ad Container · Evidence Chip · Reason Label
- **Dialog Error/Warning 헤더 아이콘이 고아 컴포넌트 참조 중**: Dialog의 `Status=Error`/`Warning` variant 헤더 아이콘 인스턴스가 정식 `Icon / Status / Error`·`Icon / Status / Warning`(둘 다 실사용 0곳인 별개 아이콘)이 아니라, 이름조차 없는 고아 컴포넌트("x-01", `17:1119`, 부모 없음)를 참조하고 있다. 정식 `Icon / Status` 계열로 재연결이 필요하나 이번 작업 범위 밖이라 지금 고치지 않는다.

**Foundation**
- Elevation 예외(Toast/Bottom Navigation) 공식 원칙화 여부
- Spacing "12"(4배수도 예외 5종도 아닌 값) 정체 재확인

---

## 6. Screen Composition Patterns

실제 화면 5개("UI" 섹션, `6294:10390`)를 실측해 도출한 화면 구성 패턴이다. 완전히 하나로 통일되진 않고 화면 성격에 따라 3가지로 갈린다.

**공통 규칙**
- 모든 화면은 `Status Bar`(44px 고정)로 시작합니다.
- 화면 최하단에는 `Action Bar`(구매 CTA류, 상세페이지·장바구니) 또는 `Bottom Navigation`(탭 루트 화면, Category)이 스크롤 본문과 분리된 고정 요소로 붙습니다.
- 이미지는 항상 풀블리드(화면 폭 100%, 마진 없음)이고, 텍스트·컨트롤 콘텐츠는 16px 마진을 씁니다(§1.3 Spacing 원칙과 일치).

**패턴 1 — 목록형 화면**(상품목록_그리드 타입, 상품목록_리스트 타입)
`App Bar` 2개 + `App Bar Small`(필터) 헤더 뭉치 → `divider`(1px) → 본문. 그리드는 2열(`Item Card / Grid`), 리스트는 1열(`Item Card / List`) — 헤더 구조는 완전히 동일하고 본문 컴포넌트만 바뀝니다. 행/아이템 사이는 전부 1px divider.

**패턴 2 — 스크롤형 화면**(상품 상세페이지, 장바구니)
헤더 바로 뒤에 divider 없이 첫 콘텐츠 블록이 바로 시작합니다(목록형과 다른 점). `spacing → 콘텐츠 블록 → spacing → divider(8px) → spacing → 다음 블록` 리듬이 반복됩니다 — **1px divider는 같은 섹션 안의 아이템 구분, 8px divider는 서로 다른 섹션(상품정보/결제정보/추천상품/리뷰) 구분**으로 두께 자체가 위계를 나타냅니다. 가로 캐러셀(추천상품)은 항상 `Section Header` 바로 아래 간격 없이 붙습니다.

**패턴 3 — 사이드바+콘텐츠 화면**(Category)
상단 `Section Header + Quick Badge Row` → divider → 좌측 `Category Tab` 세로 목록 + 우측 콘텐츠 그리드 좌우 분할.

---

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-04 | 최초 초안 작성. Foundation(Color/Typography/Spacing/Grid/Radius/Elevation/Icon) 실측값 전량 반영, Component 15개 체크리스트 명세, Naming Convention 확정, Design Principles 정정본 반영 |
| v0.2 | 2026-09-04 | 사용자와 Foundation 섹션 리뷰(대화 기반) 반영: (1) "확정 금액=파랑" 원칙 폐기, 실제(Error/Gray) 반영 (2) Success 색상 실사용 0건·Toast가 파랑 대체 사용 중임을 문서화 (3) Commercial 미사용 semantic 3개 삭제(22→19개) (4) 로켓 배송 배지 4색 `Rocket` Primitive 패밀리로 정식 등록 (5) 상품명 15px 미바인딩 + 외부 라이브러리 원격 스타일 참조 문제 해결 (6) Spacing 1,380곳 실제 Variable 바인딩 완료 (7) Radius 5단계 토큰(`none`/`small`/`medium`/`large`/`full`) 신규 확정 및 1,346곳 바인딩, `Icon Button`이 실제로는 pill(`full`)이었음을 정정. Foundation 섹션 리뷰 완료 |
| v0.3 | 2026-09-04 | §2 Component에 15개 항목 전체 Description/When to use/Variants·States/Content rules 4개 필드 추가(Figma 재조회·실사용 인스턴스 콘텐츠 기반). 조사 중 재확인된 사실 반영: `Thumbnail`에 `xsmall` 사이즈 신규 추가됨, `Radio` 컴포넌트가 삭제되어 더 이상 존재하지 않음(Radio/Checkbox 상태를 "부분"에서 "0/2"로 정정) |
| v0.4 | 2026-09-04 | §2.1 Badge 사용자 피드백 반영: (1) `Rocket Badge` radius를 pill→`small`(4)로 정정, `images 1`/`images 2` 블렌드 모드 틴팅 구조를 의도된 바인딩 예외로 명시 (2) `Status Badge`를 `Review / Card` 4개 variant에 실제 인스턴스로 연결(기존 raw 프레임 대체, Figma 실행) (3) `Cart / Countdown Badge`를 `Cart / Countdown Banner`로 개명하고 Badge→Banner로 재분류, `Order Deadline`과의 역할 차이(상품 단위 vs 장바구니 단위) 상호 문서화(Figma 실행) |
| v0.5 | 2026-09-04 | §2.2 Banner를 "정의 작성 전"으로 되돌림(사용자가 Notification Banner의 기존 설명이 Banner 정의에 안 맞는다고 판단, 임의 정의 보류). §2.3 Button을 CTA/Icon Button/Text Button 3개 컴포넌트로 재구성(Text Button 신규 반영). `Secondary`/`Line type gray` 실사용 전수 조사 결과 반영: Secondary는 "2-Button 조합의 차선책" 또는 "단독 저강조 행동" 두 맥락, Line type gray는 현재 세트 내 실사용 0곳이며 실제 회색 아웃라인 버튼("재입고 알림 신청")은 세트에서 분리된 미아 컴포넌트(`6009:9856`)를 참조 중임을 새로 발견 |
| v0.6 | 2026-09-04 | 미아 컴포넌트(`6009:9856`) 재연결 완료 — 인스턴스를 정식 `Button` 세트의 `Type=Line type gray, Size=Medium, Status=Default` variant로 swap, 그 variant 자체의 텍스트를 실제 프로덕션 문구("재입고 알림 신청")로 갱신, 미아 컴포넌트 삭제. `Line type gray`의 실제 사용 맥락(낮은 강조의 단독 대기 행동)을 §2.3에 반영 |
| v0.7 | 2026-09-04 | §2.5 Dialog에 스크롤 UI 패턴 반영 — `Dialog` COMPONENT_SET에 `Show Scroll`(Boolean, 기본값 false) 속성 신규 추가, 7개 variant 전부에 `divider-top`/`divider-bottom`/`scroll-track`/`scroll-thumb` 4요소 추가(구분선→`Background/Divider`, scroll-thumb→`Gray/700` 바인딩, scroll-track은 의도적 미바인딩 예외로 문서화, `Button=X` variant는 `divider-bottom` 제외 3요소만 적용). 사용자 제공 스크롤 발생 기준(뷰포트 70%/Body 최대 426px)을 개발 구현 규칙으로 문서화(Figma 실행) |
| v0.8 | 2026-09-04 | §2.5 Dialog 스크롤 기준을 고정 px(426px)에서 상대 단위(`max-height: 70vh`에서 고정 영역 134px을 뺀 값)로 수정 — 디바이스별 뷰포트 차이 대응, 426px는 800px 기준 참고 계산값으로만 남김. `scroll-thumb` 색상을 `Gray/700`→`Gray/400`으로 변경(Figma 실행, 7개 variant 전부) |
| v0.9 | 2026-09-04 | §2.6 Heading 사용자 피드백 2건 반영: (1) `App Bar`의 "화면 상단 고정" 서술이 전 Type 공통이 아님을 실사용 인스턴스 전수 조사로 확인·정정 — `Title`/`Category`/`Search`만 진짜 최상단 고정, `Filter`는 최상단 App Bar 아래 2번째 줄, `Filter Compact`는 스크롤 콘텐츠 내부 섹션 전용, `Index`는 Bottom Sheet 내부 요소(Figma 변경 없음, 문서만 정정). `Filter`/`Filter Compact`/`Index` 재분류 여부는 §5 후속 과제로 등록 (2) `Section Header`의 `State`/`Show Action` 의미 중복을 해소 — `Show Action` Boolean이 실제로 우측 "전체보기" 버튼의 `visible`을 제어하도록 재배선, 실사용 0곳이던 `text + button` variant 삭제(Figma 실행). `State` 축은 값 1개만 남았으나 Figma API 제약으로 완전 제거는 못 함. 기존 `Show Action=true` 인스턴스 7곳은 이번 수정으로 버튼이 새로 나타남(의도된 동작) |
| v0.10 | 2026-09-04 | §2.7을 Message Box/Toast 두 항목으로 명확히 분리 — Message Box는 아직 미착수임을 확인(❌로 정정, 체크리스트 14/15→13/15), Toast는 별개의 기존 컴포넌트로 재확인해 "매핑 미확인" 캐비어트 제거(Figma 변경 없음). Toast 동작 정의 신규 확정(사용자 결정, AskUserQuestion) — 위치: 화면 최하단 16px 고정(Bottom Navigation 유무 무관, 겹쳐서 뜸), 노출 시간: 3초. §5의 두 미결 항목(design-language.md 갱신, Message Box→Toast 매핑 확인)도 해결됨으로 정정 |
| v0.11 | 2026-09-04 | §2.8 Item Card 피드백 2건 반영: (1) `Item Card / Grid Small`이 실제로는 카테고리/검색 그리드가 아니라 장바구니·상품 상세 하단의 횡스크롤 추가구매 유도 카드 전용임을 실사용 15곳 전수 조사로 확인, `Item Card / Recommendation`으로 개명(Figma 실행). 조사 중 실제 카테고리/검색 2열 그리드를 담당하는, 지금까지 design.md에 전혀 없던 별도 컴포넌트 `Item Card / Grid`(160px, variant 없음, 실사용 8곳)를 새로 발견해 §2.8·요약 표에 신규 문서화 (2) `Price Block`에 `Show Unit Price`(Boolean, 기본값 true) 속성 신규 추가 — 그람/밀리리터 단위 환산이 불가능한 상품은 단위가 줄을 끌 수 있게 함(Figma 실행, 실사용 46곳 전부 기존엔 이 토글 자체가 없었음) |
| v0.12 | 2026-09-04 | §2.8 Review / Card의 `Status`(OneMonthUse/Repurchase) variant 축을 `Show Status Badge`(Boolean, 기본값 true)로 대체(Figma 실행) — 실사용 9곳 전수 조사 결과 전부 `Type=Photo, Status=OneMonthUse` 하나만 사용 중이었고 "4개 조합 전부 실사용"이라던 과거 서술은 오기재였음을 확인, 실사용 0곳인 2개 variant 삭제. 내부 `Status Badge` 인스턴스가 이미 자기 `Type` 프로퍼티를 가지므로 특정 신뢰 신호로 바꾸려면 그 인스턴스를 직접 override하면 되는 구조(테스트로 정상 동작 확인). `Status` 축은 값 1개만 남았으나 Section Header 때와 동일한 Figma API 제약으로 완전 제거는 못 함. Price Block 취소선 누락 버그 수정(정가 텍스트 `textDecoration: NONE`→`STRIKETHROUGH`, design.md는 이미 정확히 서술돼 있어 문서 변경 없음) |
| v0.13 | 2026-09-04 | §2.8 Review / Summary 2건 반영(Figma 실행): (1) "실사용 0곳"이라던 이전 평가가 오류였음을 사용자가 지적해 확인 — 상품 상세페이지에 실제 사용 중이었으나 컴포넌트 인스턴스 정리 과정에서 진짜 INSTANCE가 아니라 마스터를 그대로 복제한 raw 프레임으로 남아있었던 것을 발견, 실제 인스턴스로 복원(콘텐츠가 placeholder와 동일해 손실 없음) — 실사용 0곳→1곳으로 정정 (2) 카테고리 종속적 문제(향/거품/보습력/세정력이 모든 카테고리에 안 맞음) 해결을 위해 `Show Row 1`~`Show Row 4`(Boolean, 기본값 true) 4개 신규 추가, 3개 variant 전부 바인딩 — 불필요한 속성 행은 끄고 남은 행의 텍스트는 인스턴스에서 직접 override하는 방식(텍스트 전체를 프로퍼티화하진 않음, 테스트로 레이아웃 자연 축소 확인) |
| v0.14 | 2026-09-04 | §2.10 List 피드백 반영: `history` 타입(검색 기록)엔 8자 내외 규칙이 적용되지 않음을 실제 인스턴스("삼성전자 615L 2도어 냉장고", 18자)로 확인, 8자 규칙은 `Default`/`Image` 타입 전용으로 한정하고 `history`는 말줄임 처리로 확정 — 5개 variant 전부의 텍스트 노드에 `textTruncation: ENDING` 적용(Figma 실행, 기존 `DISABLED`). Category Menu Item은 사용자가 공유한 실제 화면으로 When to use(3열 그리드·바텀시트 내부)는 일치함을 재확인했으나, 그 9개 인스턴스 전부 여전히 placeholder "카테고리" 그대로라 Content rules는 실측 불가 상태 유지 — 실제 카테고리명 예시 확보 전까지 `Category Tab`의 실측 규칙을 잠정 준용하도록 권고만 추가 |
| v0.15 | 2026-09-05 | §2.12 Bottom Sheet 개편(Figma 실행): (1) `Sheet / Confirmation`→`Bottom Sheet / Informational`, `Sheet / Selector`→`Bottom Sheet / Interactive`로 개명 — Figma 파일에 이미 붙어 있던 "Informational/Interactive Bottom Sheet" 섹션 제목과 일치시킴, 내부 variant 축(Status×Button+Show Icons / Type=Item·Filter)은 그대로 유지 (2) 양쪽에 Dialog(§2.5)와 동일한 `Show Scroll`(Boolean, 기본값 false) 패턴 신규 추가 — Informational은 6개 variant 전부 버튼 영역이 있어 예외 없이 divider-top/bottom+scrollbar 4요소 적용, Interactive는 `Type=Filter`만 4요소·`Type=Item`은 하단 버튼 영역이 없어 Dialog `Button=X`와 동일 원칙으로 divider-bottom 생략한 3요소만 적용(각 컴포넌트 테스트 인스턴스로 토글 검증 후 삭제, `defaultValue` false 유지 확인) (3) 화면 높이 80% 상한을 Dialog의 70vh 규칙과 동일한 상대 단위 방식(`max-height: 80vh`)으로 문서화 — Figma 프레임 자체는 변경하지 않음, 파일에 이미 있던 360×640 참고 프레임을 계산 예시로 인용 (4) Content rules 갱신: Informational=순수 텍스트 전달 전용, Interactive=장바구니 담기 교차판매·업셀 확인 또는 필터 선택 화면. `Discription` 오탈자는 이번 범위 밖이라 수정하지 않고 그대로 표기 |
| v0.16 | 2026-09-05 | §2.12 후속 수정(Figma 실행): 사용자 요청으로 `Bottom Sheet / Interactive`(`Type=Filter`/`Type=Item` 둘 다)에서 스크롤바 인디케이터(`scroll-track`+`scroll-thumb`)만 삭제 — 구분선(`divider-top`/`divider-bottom`)과 `Show Scroll` Boolean 자체는 그대로 유지. `Bottom Sheet / Informational`의 스크롤바는 대상이 아니라 변경 없음. 테스트 인스턴스로 삭제 후에도 구분선 토글이 정상 작동함을 확인 후 삭제 |
| v0.17 | 2026-09-05 | §2.12 재정정(Figma 실행): 사용자가 v0.16에서 `Show Scroll` Boolean이 "아직 지워지지 않았다"고 재요청 — 확인 결과 스크롤바뿐 아니라 **`Show Scroll` 기능 자체를 완전히 제거**해달라는 뜻이었음(AskUserQuestion으로 재확정). `Bottom Sheet / Interactive`의 남은 구분선(`divider-top`×2, `divider-bottom`×1)을 삭제하고 COMPONENT_SET에서 `deleteComponentProperty('Show Scroll#6684:0')` 실행(정상 삭제, `Type` variant 축은 그대로). Filter/Item 두 variant 모두 header/body(/button)만 남은 원래 구조로 완전히 되돌아감. `Bottom Sheet / Informational`의 `Show Scroll`은 대상이 아니라 변경 없음 |
| v0.18 | 2026-09-05 | §2.12 문서 보강(Figma 변경 없음, 문서만): 사용자가 "같은 바텀시트인데 왜 어떤 건 스크롤 핸들이 있고 어떤 건 없냐"고 질문 — Informational(자유 텍스트라 길이 예측 불가)과 Interactive(리스트/캐러셀 형태 자체가 스크롤을 암시)의 콘텐츠 성격 차이를 "스크롤 동작" 서브섹션에 정식 근거 문장으로 추가. 기존 경위(추가했다 제거한 이력)는 이 근거의 배경 설명으로 재배치 |
| v0.19 | 2026-09-05 | §2.13 Tab Description/When to use 보강 — 사용자가 기존 서술("카테고리별 전환")이 부실하다고 지적, Explore 서브에이전트로 실사용 전수 조사. `Tab Group`은 실사용 4곳 전부 장바구니 화면(`★ 장바구니`) 한 패턴뿐이고 전부 `Type=3 Tab`(라벨: "일반구매(6)"/"자주산상품"/"찜한상품(40)") — 카테고리 나열이 아니라 사용자 개인의 구매·관심 이력을 관점별로 전환하는 용도임을 확인해 Description/When to use 재작성. `2 Tab`/`Swipe`는 실사용 0곳으로 확인돼 "라이브러리에만 존재" 캐비어트 추가, 기존 "탭 4개 이상이면 Swipe" 임계값 서술은 실사용 근거가 없는 추측이었음을 확인해 삭제. `Tab Item`은 실사용 23곳 전부 Tab Group 마스터 내부 자식으로만 존재함을 확인해 "Tab Group 내부 전용" 서술이 정확했음을 재확인(변경 없음). Figma 변경 없음, design.md 문서만 |
| v0.20 | 2026-09-05 | §2 Component(§2.1~2.15) 전체를 공개 디자인 시스템 사이트(당근마켓 Seed Design, 지마켓 GDS) 문서 스타일로 재구성 — 사용자가 두 사이트의 문체·구조·내용을 참고해달라고 요청, WebFetch로 두 사이트를 분석해 Anatomy(구조 설명)·Do/Don't 사용 가이드·`Property\|Values\|Default\|비고` 형식의 Variants 표·Content 목록·유사 컴포넌트 비교 노트 패턴을 도출. 순수 리포매팅 작업으로 **기존 실사용 조사·감사 이력(✅ 정정, ⚠️ 확인 필요, node ID, 날짜, 실사용 N곳 등)은 전부 그대로 유지**(사용자 확정 사항) — 이 문서는 공개용 마케팅 페이지가 아니라 내부 작업 기록이라 근거 추적성이 더 중요하다고 판단. §1 Foundation, §2 요약 체크리스트 표, §3 Naming Convention, §4 Design Principles, §5 후속 과제는 대상에서 제외하고 그대로 둠. 재작성 전/후로 모든 node ID(`` `[0-9]+:[0-9]+` `` 패턴)와 "실사용 N곳" 카운트를 diff해 정보 손실 없음을 확인 |
| v0.21 | 2026-09-05 | 당근/G마켓 벤치마킹 후속 반영(사용자가 3개 제안 중 2개만 승인, "확정 주체 표기"는 개인 프로젝트라 불필요하다고 판단해 제외): (1) 최상위 컴포넌트 37개에 `Figma: node-id` 참조 라인 추가(Figma `findAllWithCriteria`로 실시간 확인한 정확한 ID만 사용, 미확인 이름은 임의 부여하지 않음) (2) 형제 컴포넌트가 진짜 병렬 비교 가능한 6개 섹션(Badge/Button/Chip/Item Card 3종/List/Bottom Sheet)의 "비교" 산문을 표로 전환 — Tab Group/Tab Item, Bottom Navigation/Item처럼 상위-하위 포함 관계인 곳은 표로 강제하지 않고 산문 그대로 유지. 순수 리포매팅 — 기존 사실·node ID·실사용 카운트 손실 없음(전체 37개 ID 존재 확인) |
| v0.22 | 2026-09-05 | §2.15 Radio/Checkbox 재정정(Figma 실행 없음, 실측 확인) — 사용자가 공유한 Figma 링크(`node-id=6020-13544`)로 "Checkbox는 처음부터 만들어진 적이 없다"던 이전 서술이 틀렸음을 확인. `Icon / Checkbox`(`6020:13544`)가 실재하며, `Item Card / Cart` 마스터(Stock=Available/OutOfStock 각각 `Status=Default`/`Disabled`로 내장)와 `Cart / Selection Toolbar`, 장바구니 화면 실제 인스턴스 4곳에서 실사용 중임을 확인 — "실사용 0곳"이 아니었다. 다만 `Icon / Radio`와 동일한 구조적 한계 확인: 두 variant(Default/Disabled) 전부 내부가 단일 VECTOR이고 둘 다 "체크됨" 모양이 고정이라 Unchecked(빈 박스) variant 자체가 없어 실제 체크/언체크 토글은 여전히 불가능 — "진짜 인터랙티브 컴포넌트는 0/2"라는 결론 자체는 유지, "Checkbox가 아예 없다"는 서술만 정정. §2 요약 체크리스트 표(15번 행)도 함께 갱신 |
| v0.23 | 2026-09-05 | §3 Naming Convention 3건 보강(Figma 변경 없음, 문서만) — 사용자가 당근/G마켓 대비 우리 네이밍 규칙의 체계성을 물어, 두 사이트 모두 네이밍 컨벤션을 공개 문서로 두지 않음을 WebFetch/WebSearch로 확인한 뒤 우리 §3 자체를 감사해 발견한 3개 공백을 모두 반영: (1) Boolean 프로퍼티 접두사 규칙(`Show {명사}`)을 명문화 — 기존에 만들어진 모든 Boolean(Show Icons/Scroll/Unit Price/Status Badge/Row 1~4/Leading·Trailing Icon)이 이미 이 패턴을 따르고 있었으나 규칙으로 적어둔 적은 없었음 (2) Foundation 토큰 이름은 카테고리마다 표기 관례가 다르다는 관찰 사실을 신규 문서화(Semantic Color=Title Case+슬래시, Typography=소문자+슬래시) — 통일된 규칙으로 지어내지 않고 현재 상태 그대로 기록, §5 후속 과제로 통일 여부 논의 등록 (3) 새 Property·컴포넌트 이름 추가 직전 철자 재확인 권장 규칙 추가 — `Discription` 오탈자 전례(§2.12)를 근거로 제시. §5의 Checkbox 항목도 v0.22 재정정 내용에 맞춰 함께 갱신 |
| v0.24 | 2026-09-05 | §4 Design Principles에 이번 세션 리뷰로 확인된 신규 원칙 2개 추가(8개→10개, Figma 변경 없음, 문서만): (1) 모달형 오버레이(Dialog `70vh`/Bottom Sheet `80vh`)는 높이를 뷰포트 상대 단위로 제한하고, 스크롤 인디케이터는 자유 텍스트 콘텐츠(Dialog, Bottom Sheet/Informational)에만 붙이고 리스트·캐러셀처럼 형태 자체가 스크롤을 암시하는 콘텐츠(Bottom Sheet/Interactive)엔 붙이지 않는다는 원칙 — §2.5·§2.12 리뷰에서 두 컴포넌트가 독립적으로 같은 규칙에 수렴한 것을 근거로 원칙 승격 (2) 탭형 선택 UI는 "같은 데이터를 다른 관점으로 재구성"(Tab Group)과 "서로 다른 대상을 나열해 선택"(Category Tab/Chip)으로 역할이 분리된다는 원칙 — §2.13 Tab 실사용 조사에서 확인. 사용자가 "여태까지 대화한 내용을 기반으로 업데이트해달라"고 요청해 이번 세션에서 실제로 발견·확정된 사실만 반영, 새로 지어내지 않음 |
| v0.25 | 2026-09-05 | 문서 정합성 오류 정정(Figma 변경 없음, 문서만) — 사용자에게 후속 과제를 리스트업하다가 §5 "Section Header의 State/Show Action 의미 중복 가능성 검토"가 실제로는 2026-09-04에 이미 해결됐는데(§2.6에 "✅ 해결됨" 명시) §5 목록과 §2 요약 체크리스트 표(6번 행)에서 지워지지 않고 남아있던 걸 발견. §5 항목에 취소선+해결 근거를 추가하고, 요약 표 6번 행의 옛 문구("의미 중복 가능성 있어 후속 검토 필요")를 §2.6과 일치하는 해결 상태로 갱신 |
| v0.26 | 2026-09-05 | Figma 실행, design.md 문서 변경은 §5만: 아이콘 컴포넌트 재분류 + variant 분리. `Icon`/`Components` 두 페이지로 나눈 아이콘 6종 중 실사용 근거로 `Icon / Utility`(24곳, Chip 마스터 내장)·`Icon / Checkbox`(10곳)·`Icon / Radio`(§2.15 체크리스트 일관성)를 `Components` 페이지로 재이동, `Icon / Status`(실사용 0곳 — Dialog가 실제로는 고아 컴포넌트 "x-01"을 참조 중임을 발견)·`Icon / General`·`Icon / Placeholder Square`는 `Icon` 페이지 잔류로 확정. 이어서 Assets 패널에서 클릭 없이 모든 아이콘이 보이도록 5개 COMPONENT_SET(Radio/Status/Checkbox/General/Utility)의 variant 14개를 전부 독립 컴포넌트로 분리(`Icon / {세트명} / {값}` 네이밍) — 실사용 137곳 이상 걸린 상태에서 위험도 낮은 순으로 단계적 실행, 매 단계 스크린샷 검증, 문제 0건. §5에 Dialog "x-01" 고아 컴포넌트 재연결 필요 항목 신규 추가 |
| v0.27 | 2026-09-05 | §1 Foundation + §2 Component 문체 통일(Figma 변경 없음, 순수 리포매팅): 사용자가 지마켓 GDS를 참고해 문체를 통일해달라고 요청 — Explore 조사 결과 불일치가 섹션이 아니라 필드 종류 단위였음을 확인(Description 문장은 이미 100% 존댓말, 바로 아래 사용가이드 Do/Don't·Content 불릿·Variants 비고는 평어)해 §1+§2 전체(§3 Naming Convention/§4 Design Principles/§5 후속과제/변경이력은 장르가 달라 제외, 요약 체크리스트 표도 제외)의 평어 문장 종결을 존댓말로 전환. "바텀시트"/"다이얼로그"/"체크박스"(§2.15 Description 내)처럼 한글화된 컴포넌트 이름도 백틱 영문(`Bottom Sheet`/`Dialog`/`Icon / Checkbox`)으로 정규화. 재작성 전/후로 node ID 50개(diff 완전 일치)·실사용 75건·✅ 60건·⚠️ 19건·날짜 103건 전부 동일함을 확인해 정보 손실 0건 검증, §3 이후 라인 번호도 정확히 그대로 유지됨을 확인 |
| v0.28 | 2026-09-05 | §1~§5 전체에서 과거 이력 서술 제거(Figma 변경 없음, 순수 삭제·정리): 사용자가 공개 디자인 시스템 문서엔 "해결된 이슈"/"바로잡은 사실" 같은 과거 작업 이력이 없다고 지적 — decisions.md를 이력 저장소로 일원화하기로 하고, Explore로 전체 문서(§1~변경이력)를 감사해 "언제/어떻게 지금 상태에 도달했는지" narrate하는 Type A 서술(약 48건, decisions.md 표본 16건 대조 결과 전부 이미 기록돼 있음 확인)은 삭제, "지금도 확정 안 됐다"는 Type B 현재 캐비어트(약 30건)는 날짜·감사 프레이밍만 벗기고 유지, 한 문장에 둘이 섞인 Ambiguous 약 26건은 수술적으로 분리. §2 요약 체크리스트 표의 ✅/❌ 상태 열과 각 컴포넌트의 ✅ Do/❌ Don't 불릿, 변경 이력 표는 원래 성격이 달라 전부 그대로 유지(diff로 완전 동일함 확인). §5의 이미 해결된 3개 취소선 항목은 §2.6/데이터에 이미 반영돼 있어 목록에서 완전히 삭제. 재작성 전/후 diff로 node ID 50→49개(삭제된 Review/Summary 관련 1개는 순수 조사 경위용이라 decisions.md에 이미 기록된 사실, 손실 아님), 실사용 언급 75→60건(narrate성 언급만 감소, 현재 사실은 보존), Do/Don't 불릿 49개 완전 동일, 변경 이력 표 byte 단위로 완전 동일함을 확인 |
| v0.29 | 2026-09-05 | 새 섹션 `## 0. 이 문서 사용 원칙` 신규 추가(Figma 변경 없음, 문서만) — 3-1(새 컴포지션/새 화면 제작) 논의에서 사용자가 승인한 프로세스 규칙 5개(컴포넌트 우선/토큰 전용/컴포지션 우선/모호함 표면화/사후 검증)를 명문화. §1~§5 내용 자체는 변경 없음 |
| v0.30 | 2026-09-05 | 남은 별표 논의 항목 전부를 한 번에 배치 실행(Figma 실행 + 문서 반영): (1) `Message Box` 컴포넌트 신규 생성(`6735:3455`, `Status`=Error/Warning, 정보 아이콘+텍스트, 닫기 버튼 없음) — §2.7 문서화, §2 체크리스트 14/15로 갱신 (2) 신규 Semantic 색상 토큰 4개 추가(기존 Primitive alias) — `Feedback/Error Background`→`Error/50`, `Feedback/Error Text`→`Error/600`, `Feedback/Warning Background`→`Warning/50`, `Feedback/Warning Text`→`Warning/600`, Message Box에 바인딩 (3) Dialog/Bottom Sheet dim 오버레이=`Background/Overlay` 50% 불투명도, `Feedback/Success`=파랑 렌더링을 공식 사용 규칙으로 §1.1에 명문화 (4) §1.4 Grid에 4컬럼(76px)과 `Item Card/Grid` 160px의 산술적 정합성 추가 (5) §2.9 Label에 Label/Chip/Rocket Badge/Status Badge/Spec Row 비교표 신규 추가 (6) §3에 Variant vs Boolean 선택 기준 추가 (7) §4에 자유 텍스트 넘침 처리 기본 원칙 추가(8개→11개) (8) §5에서 해결된 3항목(Message Box, App Bar Type 재검토, 토큰 표기 통일) 제거 — App Bar Type은 "현행 유지", 토큰 표기는 "카테고리별 현행 유지"로 결론 (9) 신규 `## 6. Screen Composition Patterns` 섹션 추가(목록형/스크롤형/사이드바형 3패턴 + 공통 규칙, 실제 화면 5개 실측 근거) — Category 화면 `Status Bar`를 32px→44px로 통일(Figma 실행) |
