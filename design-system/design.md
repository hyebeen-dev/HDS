# Design System 명세서

Foundation과 Component의 값과 사용 규칙을 정리한 디자인 시스템 명세입니다.

- 기준 버전: v0.72
- Figma 파일: Design System_0901
- 관련 문서: [status.md](./status.md) (진행 현황), [design-language.md](./design-language.md) (시각 스타일 관찰)

---

## 0. 이 문서 사용 원칙

이 문서는 참고 기록이 아니라 새 화면을 만들 때 실제로 지키는 규칙입니다. §1~§6의 내용을 어떻게 적용할지 판단이 필요할 때는 아래 6개 원칙을 순서대로 확인합니다.

1. **컴포넌트 우선**: 새 화면 요소가 필요하면 먼저 §2에 정의된 컴포넌트로 해결되는지 확인합니다. 없는 것이 확실할 때만 새로 만듭니다.
2. **토큰 전용**: 색상, 여백, 모서리 반경 등은 §1 Foundation에 정의된 토큰만 사용합니다. hex나 px 값을 직접 쓰지 않습니다. 필요한 값에 맞는 토큰이 없으면 새 값을 임의로 만들지 않고 먼저 확인을 구합니다.
3. **컴포지션 우선**: 새 화면은 기존에 확인된 실제 화면 패턴 중 목적이 가장 가까운 것을 뼈대로 삼습니다. 그 화면의 목적상 다르게 가야 하는 부분만 벗어나고, 벗어난 이유를 남깁니다.
4. **모호함 표면화**: 이 문서에 없는 상황을 만나면 임의로 결정하지 않고 먼저 확인을 구합니다. 정의되지 않은 컴포넌트 조합이나 새로운 콘텐츠 종류가 여기에 해당합니다.
5. **사후 검증**: 화면을 다 만든 뒤, 사용한 컴포넌트의 Variants·Content 규칙과 적용한 컴포지션 패턴에 맞는지 다시 대조합니다.
6. **컴포넌트 목적 우선**: 특정 참고 화면이 어떤 컴포넌트를 예외적으로 쓰고 있어도 그 예외를 새 화면에 그대로 옮기지 않습니다. 타이틀을 보이지 않게 숨긴 화면이 그런 예입니다. 컴포넌트 자체의 Content와 사용 가이드 규칙을 먼저 따르고, 특정 화면의 관찰은 그다음 참고용으로만 씁니다.

---

## 1. Foundation

### 1.1 Color

**Primitive**

8개 색상 패밀리와 도메인 전용 패밀리 5개로 구성됩니다. 각 색상 패밀리는 팔레트 단계별 값을 갖습니다. Primitive는 직접 쓰지 않고 아래 Semantic 토큰을 통해 참조합니다. 도메인 전용 패밀리만 그 자체로 참조합니다.

| 패밀리 | 용도 |
|---|---|
| Gray | 배경, 구분선, 보조 텍스트 |
| Blue | 행동(CTA)과 링크. 가격 강조에는 사용하지 않음 |
| SkyBlue | Blue보다 채도가 낮은 하늘색 보조 톤 |
| Point | 강한 강조(레드 오렌지 계열) |
| Red | 위험, 할인, 최종 판매가 강조 |
| Orange | 경고 표기 |
| Green | 배송 안내 텍스트(`Text/Delivery`)에 사용. 성공 상태 표시에는 쓰지 않음 |
| Static | 순수 흰색(`0`)과 검정(`1000`) |
| Rocket | 배송 배지 4종 전용. `Rocket/Seller/Background`·`Text`, `Rocket/Fresh/Background`·`Text`, `Rocket/Global/Background`·`Text`, `Rocket/Tomorrow/Background`·`Text` 8개. 도메인 특화 개념이라 Semantic으로 올리지 않음 |
| QuickBadge | `Quick Badge Row` 컴포넌트의 배지 5종 배경 전용. `QuickBadge/WowCard·RocketFresh·RLux·GoldBox·CoupangEats/Background` 5개 |
| Reward | `Spec Row`의 적립 아이콘 전용(`Reward/Icon` 1개) |
| AI | `AI Review`의 반짝임 아이콘 전용(`AI/Sparkle` 1개) |
| Rating | `Rating Display` 별 아이콘 전용(`Rating/Star` 1개) |

**Semantic**

31개 토큰입니다. 모두 Primitive를 참조하며 값을 직접 넣은 곳은 없습니다.

| 토큰 | 값 | Primitive | 용도 |
|---|---|---|---|
| `Text/Primary` | `#34373d` | Gray/900 | 기본 텍스트 |
| `Text/Description` | `#7b818e` | Gray/600 | 보조 텍스트 |
| `Text/On Color` | `#ffffff` | Static/0 | 색 배경 위 텍스트 |
| `Text/Accent` | `#106def` | Blue/500 | 링크, 강조 텍스트 |
| `Background/Background` | `#f4f6f6` | Gray/50 | 페이지 배경 |
| `Background/Divider` | `#e3e5e8` | Gray/200 | 컴포넌트 내부 구분선 |
| `Background/Overlay` | `#000000` | 직접 값 | Dialog와 Bottom Sheet의 dim 오버레이 |
| `State/Primary` | `#106def` | Blue/500 | CTA 버튼 배경 |
| `State/Primary Pressed` | `#0d57bf` | Blue/600 | CTA 버튼 눌림 |
| `State/Secondary` | `#e7f0fd` | Blue/50 | 보조 버튼 배경 |
| `State/Secondary Pressed` | `#cfe2fc` | Blue/100 | 보조 버튼 눌림 |
| `State/Disabled` | `#cdd0d5` | Gray/300 | 비활성 상태 |
| `Feedback/Success` | `#1dc948` | Green/500 | 정의만 존재(성공 상태는 파랑으로 표시) |
| `Feedback/Error` | `#e23636` | Red/400 | 상태 아이콘 |
| `Feedback/Warning` | `#ffb33d` | Orange/400 | 상태 아이콘 |
| `Feedback/Info` | `#969ca6` | Gray/500 | 상태 아이콘 |
| `Feedback/Error Background` | `#fce9e9` | Red/50 | `Message Box` 배경 |
| `Feedback/Error Text` | `#c91d1d` | Red/500 | `Message Box` 텍스트 |
| `Feedback/Warning Background` | `#fff3e0` | Orange/50 | `Message Box` 배경 |
| `Feedback/Warning Text` | `#a35d00` | Orange/700 | `Message Box` 텍스트 |
| `Emphasis/Primary` | `#f2460d` | Point/500 | — |
| `Emphasis/Secondary` | `#34373d` | Gray/900 | — |
| `Emphasis/Ambient` | `#7b818e` | Gray/600 | — |
| `Rating/Star` | `#ff9c5c` | Rating/Star | `Rating Display` 별 아이콘 전용 |
| `Text/Price` | `#e23636` | Red/400 | 조건부 가격 강조(`Price Block` 판매가, 단위가) |
| `Background/Price` | `#e23636` | Red/400 | 할인율 배지 배경 |
| `Text/Urgent` | `#e23636` | Red/400 | `Order Deadline`, 재고 경고 |
| `Text/Delivery` | `#169c16` | Green/600 | 배송 달성, 도착 안내 |
| `Background/Benefit` | `#d4ebf7` | SkyBlue/200 | 무료배송, 무료반품 배지 배경 |
| `Border/Primary` | `#408af2` | Blue/400 | `Button`과 `Icon Button`의 Secondary 타입 기본 테두리 |
| `Border/Price` | `#e23636` | Red/400 | `Label` "특가 진행중" 아웃라인 테두리 |

**사용 규칙**

- 가격 강조에는 Red와 Gray 계열을 씁니다. `Price Block`의 최종가는 할인 적용 시 `Red/400`(빨강), 미적용 시 `Gray/900`(기본 텍스트 색)입니다. 장바구니 화면의 "총 결제 예상 금액"도 빨강으로 렌더링됩니다.
- Blue(파랑)는 CTA 버튼 배경과 링크 텍스트 같은 행동 용도로만 사용합니다. 가격 숫자 강조에는 쓰지 않습니다.
- Red(빨강)는 할인율, 위험, 긴급 신호와 위의 최종가 강조에 씁니다.
- `Text/Price`와 `Text/Urgent`는 값이 같지만 역할이 달라 별도 토큰으로 둡니다.
- 성공 상태는 파랑(`Blue/400`)으로 표시합니다. `Toast`의 `Status=Success` 아이콘이 기준이며, 신규 컴포넌트도 같은 기준을 따릅니다. `Feedback/Success`(Green) 토큰은 정의만 유지합니다.
- 배경색 채움은 이 시스템에서 가장 강한 강조 수단입니다. CTA 버튼과 할인율 배지 외에는 배경색을 채우지 않습니다.
- 프로모션 전용 색상은 정의되어 있지 않습니다. 필요해지면 그때 정의합니다.
- 로켓 배송 배지 4색은 `Rocket` Primitive 패밀리로 관리합니다. `Rocket Badge` 컴포넌트의 4개 variant가 모두 여기에 바인딩되어 있습니다.
- Dialog와 Bottom Sheet의 dim 오버레이는 `Background/Overlay`(`#000000`)를 50% 불투명도로 적용합니다. 화면 전체를 덮는 스크림 레이어는 컴포넌트에 포함되지 않고 구현 시 추가되는 영역입니다. 따라서 이 값은 `Toast`의 위치·노출 시간 규칙과 같은 개발 구현 규칙입니다.
- `Feedback/Error Background`·`Error Text`, `Feedback/Warning Background`·`Warning Text`는 `Message Box`(§2.7) 전용 배경·텍스트 쌍입니다. 옅은 배경 위에 진한 텍스트를 얹는 카드형 안내 배너에 씁니다. 아이콘 단색용인 `Feedback/Error`, `Feedback/Warning`과는 용도가 다릅니다.

### 1.2 Typography

Pretendard 단일 패밀리입니다. 6개 역할 카테고리로 나뉩니다. 크기와 굵기가 같아도 역할이 다르면 별도 토큰으로 분리합니다. 예를 들어 14px Regular는 Chip에서 `label`, Tab에서 `navigation`, Toast에서 `body` 스타일입니다.

| 카테고리 | 스타일 수 | 용도 |
|---|---|---|
| `display` | 0개(예약) | 실사용 사례가 없어 값을 정하지 않고 예약 |
| `heading` | 7개 | 제목, 헤더 |
| `body` | 15개 | 본문, 가격, 설명 등 일반 텍스트 |
| `label` | 3개 | 배지, 라벨류 짧은 텍스트 |
| `navigation` | 3개 | 탭, 내비게이션 텍스트 |
| `underline` | 1개 | 밑줄 텍스트(링크 등) |

**전체 스타일 표**

모든 스타일의 글꼴은 Pretendard입니다.

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
| `body/small` | Regular | 14 | 140%(예외) | 0 |
| `body/compact-medium` | Medium | 15 | 150% | 0 |
| `body/compact` | Regular | 15 | 140%(예외) | 0 |
| `body/xsmall-medium` | Medium | 13 | 150% | 0 |
| `body/xsmall` | Regular | 13 | 150% | 0 |
| `body/xxsmall` | Regular | 12 | 150% | 0 |
| `body/xxxsmall` | Medium | 11 | 150% | 0 |
| `label/small` | Regular | 14 | 140%(예외) | 0 |
| `label/xsmall-bold` | Bold | 13 | 150% | 0 |
| `label/xsmall-medium` | Medium | 13 | 150% | 0 |
| `navigation/medium` | Medium | 16 | 150% | 0 |
| `navigation/small-bold` | Bold | 14 | 150% | 0 |
| `navigation/small` | Regular | 14 | 140%(예외) | 0 |
| `underline/xsmall` | Regular | 13 | 150% | 0 |

**규칙**: Line height는 150%가 기본입니다. `body/small`, `body/compact`, `label/small`, `navigation/small` 4개만 140%로 예외를 둡니다. 네 스타일 모두 14px 또는 15px Regular 계열입니다.

### 1.3 Spacing

16개 값입니다. 4의 배수를 기본 원칙으로 합니다. 컴포넌트 특성상 불가피한 경우에는 2px 단위 예외를 명시적으로 허용합니다.

| 구분 | 값 |
|---|---|
| 4의 배수(권장) | 4, 8, 12, 16, 20, 24, 32, 40, 48, 56, 64 |
| 예외(2px 단위, 버튼·칩류에 사용) | 2, 6, 10, 11, 13 |

**반복되는 관계**: 화면 좌우 마진 16px과 카드 내부 요소 gap 8px이 가장 많이 쓰입니다. 섹션 사이는 8px 두꺼운 구분선과 16px 여백의 조합으로 구분합니다. 컴포넌트 라이브러리 전체의 padding과 gap은 Variable에 바인딩되어 있어 숫자를 직접 넣은 곳이 없습니다.

**주의: 텍스트를 세로로 촘촘히 쌓을 때**

이름과 역할 라벨처럼 짧은 한 줄 텍스트를 세로로 쌓는 경우입니다. 텍스트 박스는 이미 150% 또는 140% 줄간격만큼의 여백을 포함합니다. 예를 들어 14px 텍스트의 박스 높이는 21px입니다. 여기에 spacing 토큰값을 그대로 `itemSpacing`으로 더하면 체감 여백이 토큰값보다 훨씬 크게 느껴집니다. 이런 상황에서는 표준값(8, 16)보다 작은 예외값 `2`를 먼저 고려하고 실제 렌더링 결과를 확인합니다.

### 1.4 Grid

모바일 단일 브레이크포인트만 정의합니다. 모바일 우선 범위라 멀티 브레이크포인트는 도입하지 않았습니다.

| 항목 | 값 |
|---|---|
| 컨테이너 폭 | 360px |
| 마진 | 16px |
| 거터 | 8px |
| 컬럼 수 | 4 |

**미정의**: 캐러셀 영역(3열)의 규칙은 아직 정의되지 않았습니다.

**4컬럼과 `Item Card / Grid` 160px의 관계**

컬럼 폭은 `(360 − 마진 16×2 − 거터 8×3) ÷ 4 = 76px`입니다. 2열 그리드 화면의 `Item Card / Grid`(160px)는 이 4컬럼 중 2컬럼을 내부 거터(8px)로 묶어 하나의 카드 폭으로 쓴 것입니다. 계산은 `76×2 + 8 = 160`입니다. 4컬럼 그리드와 2열 카드 배치는 서로 다른 규칙이 아닙니다. 같은 4컬럼 그리드를 카드 하나당 2컬럼씩 묶어 쓴 결과입니다.

### 1.5 Radius

공식 토큰 5단계입니다. `Radius` Variable Collection으로 Figma에 등록되어 있고 컴포넌트 라이브러리 전체에 바인딩되어 있습니다. 필요해지면 `large` 위로 `xlarge` 등을 추가할 수 있는 구조입니다.

| 토큰 | 값 | 해당 컴포넌트 |
|---|---|---|
| `none` | 0 | Category Tab, `Item Card / Recommendation`, `Item Card / Cart`, Action Bar, Dialog 본체, Bottom Sheet 본체, Thumbnail Xlarge |
| `small` | 4 | Button Small/XSmall |
| `medium` | 8 | Button Large/Medium, Thumbnail Small/Medium/Large |
| `large` | 16 | Dialog·`Bottom Sheet / Informational`·`Bottom Sheet / Interactive`의 `header` 상단 두 모서리(하단은 `none`) |
| `full` | 999. 렌더링 시 요소 크기에 맞춰 pill 또는 원으로 표시 | Chip, Radio, Icon Button, Search, Bottom Sheet 드래그 핸들, Item Card/Cart 아이콘 |

**규칙**: 작고 인터랙티브한 요소일수록 더 둥급니다. 순서는 `full`(Chip, Radio, Icon Button 등), `medium`(Button Large/Medium), `small`(Button Small/XSmall), `none`(Card, Action Bar 등)입니다. `large`는 이 축과 별개로 모달형 오버레이(Dialog, Bottom Sheet)의 상단 모서리에만 씁니다.

### 1.6 Elevation

기본 원칙은 그림자를 쓰지 않고 구분선과 여백으로 깊이를 표현하는 것입니다. `shadow` 이펙트 스타일(DROP_SHADOW, radius 10, 검정 5%)이 정의되어 있지만 원칙상 사용을 지양합니다.

**예외**: `Toast`에는 BACKGROUND_BLUR, `Bottom Navigation`에는 DROP_SHADOW가 적용되어 있습니다. `Toast`와 `Bottom Navigation`은 화면 위에 고정되는 요소라 예외로 둡니다.

**카드형 컨테이너에 대응 컴포넌트가 없는 경우**: 배경을 채우거나 그림자를 넣지 않습니다. 1px 선(`Background/Divider`)과 `radius/medium`(8)으로 경계만 표현합니다. 지표 카드나 차트 컨테이너 같은 새 카드형 UI에도 "그림자 대신 선" 원칙을 동일하게 적용합니다.

### 1.7 Icon

| 항목 | 값 |
|---|---|
| 기본 크기 | 16×16 |
| 선 두께 | 1.5px(가는 outline) |
| 스타일 구분 | 내비게이션·컨트롤 아이콘은 outline, 정보성 배지 아이콘(로켓 배송 등)은 filled |
| 기본형 | 아이콘 단독보다 텍스트와 짝을 이루는 구성 |

---

## 2. Component

체크리스트 15개 항목 중 14개를 완료했습니다. Radio/Checkbox만 미완료입니다. 완료 기준은 컴포넌트가 만들어져 있는지이며 실사용 여부와는 무관합니다. 체크리스트 밖에서 Text Input, Text Area, Seller Row 3개를 추가했습니다.

| # | 체크리스트 항목 | 상태 | 실제 컴포넌트 | Variant / Property 구조 |
|---|---|---|---|---|
| 1 | Badge | ✅ | `Rocket Badge`, `Status Badge` | `Rocket Badge`: `Type`(seller/fresh/global/tomorrow), radius `small`(4) · `Status Badge`: `Type`(Repurchase/OneMonthUse), `Review / Card`에 인스턴스로 연결 |
| 2 | Banner | ⚠️ 정의 작성 전 | `Cart / Notification Banner`, `Cart / Countdown Banner` | 컴포넌트는 있으나 Banner의 정의가 아직 합의되지 않음 |
| 3 | Button | ✅ | `Button`(CTA), `Icon Button`, `Text Button` | `Button`: `Type`(Primary/Secondary/Line type gray) × `Size`(Large/Medium/Small/XSmall) × `Status`(Default/Pressed/Disabled) + `Show Leading Icon`/`Show Trailing Icon`(Boolean) · `Icon Button`: `Type`(Secondary/Line type gray) × `Size`(Medium/Small) × `Status`(Default/Pressed) · `Text Button`: `Type`(Default/accent) × `Size`(Default/Large) |
| 4 | Chip | ✅ | `Chip`, `Option Chips / Size`, `Option Chips / Thumbnail` | `Chip`: `Status`(Default/Selected/Pressed) × `Dropdown`(Boolean) × `rocket`(배송 타입) · Option Chips: `Status`(Default/low_stock/sold_out) + `Selected`(Boolean) |
| 5 | Dialog | ✅ | `Dialog` | `Status`(Default/Error/Warning) × `Button`(1 Button/2 Button/X) + `Show Scroll`(Boolean, 본문 초과 시 스크롤 UI 미리보기) |
| 6 | Heading | ✅ | `App Bar`, `App Bar Small`, `Section Header`, Cart 전용 4종(Notification Banner/Countdown Banner/Address Row/Selection Toolbar) | App Bar: `Type`(Title/Filter/Filter Compact/Search/Category/Index) · Section Header: `State`(text 단일 값) + `Show Action`(Boolean, 우측 액션 버튼 `visible` 제어) |
| 7 | Message Box | ✅ | `Message Box` | `Status`(Error/Warning). `Toast`(스낵바)와는 별개 컴포넌트 |
| 8 | Item Card | ✅ | `Item Card / Grid`, `Item Card / Recommendation`, `Item Card / Cart`, `Price Block`, `Spec Row`, `Rating Display`, `Order Deadline`, `Review / Card`, `Review / Summary` | Grid: `Show Label`(Boolean, 내장 `Label` 인스턴스 `visible` 제어) + 내장 `Price Block` `Discount`(Wow/General/None) 노출(`isExposedInstance`) · Recommendation: `Type`(Default/Button) × `Discount`(Wow/General/None) · Cart: `Stock`(Available/OutOfStock) + 내장 `Price Block` `Discount`(Wow/General/None) 노출 · Price Block: `Discount`(Wow/General/None) + `Show Unit Price`(Boolean) · Review/Card: `Type`(Photo/Only Text) + `Show Status Badge`(Boolean) · Review/Summary: `status`(Default/none photo/no rating) + `Show Row 1~4`(Boolean) |
| 9 | Label | ✅ | `Label` | `Type`(Promotion/Return/Shipping/RepeatPurchase) |
| 10 | List | ✅ | `List`, `Category Tab`, `Category Menu Item` | List: `type`(Default/Image/history) × `Status`(Default/pressed) · Category Tab: `Status`(Default/Selected) |
| 11 | Navigation | ✅ | `Bottom Navigation`, `Bottom Navigation Item` | Bottom Navigation: `Platform`(iOS/Android) · Item: `Icon`(Home/Category/Search/Mypage/Cart) × `State`(On/Off/Pressed) |
| 12 | Bottom Sheet | ✅ | `Bottom Sheet / Informational`, `Bottom Sheet / Interactive` | Informational: `Status`(Default/Error/Warning) × `Button`(1/2 Button) + `Show Icons`(Boolean) + `Show Scroll`(Boolean, 기본값 false, 구분선+스크롤바 4요소) · Interactive: `Type`(Item/Filter), `Show Scroll` 없음 |
| 13 | Tab | ✅ | `Tab Group`, `Tab Item` | Group: `Type`(2 Tab/3 Tab/Swipe) · Item: `Status`(Default/Selected/Pressed) |
| 14 | Thumbnail | ✅ | `Thumbnail` | `Size`(xsmall/small/medium/large/Xlarge) |
| 15 | Radio / Checkbox | ❌ 미완료(토글 가능한 컴포넌트 기준 0/2) | `Icon / Radio`, `Icon / Checkbox`(둘 다 장식용 글리프, 체크/언체크 토글 불가) | `Icon / Checkbox`는 Default/Disabled 모두 체크된 모양만 있고 Unchecked variant가 없어 토글 컴포넌트로 쓸 수 없음. `Item Card / Cart`, 장바구니 화면, `Cart / Selection Toolbar`에서 사용 중 |
| 16 | Text Input | ✅ | `Text Input` | `Content`(Empty/Filled) × `Size`(Large) × `Status`(Default/Active/Disabled/Invalid) × `Helper Text`(None/OneLine/TwoLine) × `Input Type`(Default/Masking/Number) + `Show Required Mark`/`Show Counter`/`Show Button`(Boolean) |
| 17 | Text Area | ✅ | `Text Area` | `Content`(Empty/Filled) × `Status`(Default/Active/Disabled/Invalid) + `Show Counter`(Boolean) |
| 18 | Seller Row | ✅ | `Seller Row` | Variant 없는 단일 컴포넌트(아바타+판매자명+Chevron+브랜드 태그) |

### 2.1 Badge

**비교**

| 항목 | Rocket Badge | Status Badge |
|---|---|---|
| 용도 | 배송 유형 식별(정보성) | 리뷰 작성자의 구매 이력 신뢰 신호 |
| 형태 | 살짝 둥근 사각형(`small`, 4) | outline(파란 테두리+텍스트, 배경 없음) |
| 상태 변화 | 없음(정보 표시 전용) | 없음(정보 표시 전용) |

**Rocket Badge**

상품의 배송 유형(로켓배송 계열)을 한눈에 식별하게 하는 배지입니다. 아이콘과 텍스트 조합으로 배송 신뢰도를 전달합니다.

**사용 가이드**
- ✅ Do: 상품 카드, 상세 등에서 이 상품이 어떤 로켓배송 유형인지 표시할 때 사용합니다.
- ❌ Don't: 배송 유형이 없는 상품에는 노출하지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Seller(판매자로켓, 주황) / Fresh(로켓프레쉬, 초록) / Global(로켓직구, 보라) / Tomorrow(로켓내일, 파랑) | — | 상태(Pressed 등) 없음. 정보 표시 전용 |

Radius는 `small`(4)입니다. pill(`full`)이 아니라 살짝 둥근 사각형이며 Chip과 다른 형태입니다. 배경과 텍스트 색은 `Rocket` Primitive 변수에 바인딩되어 있습니다.

**Anatomy(아이콘 구조, 의도된 예외)**: `Seller`/`Fresh`/`Global` 3종의 아이콘은 내부가 2개 레이어로 구성됩니다. 파란 로켓 이미지(`images 2`, blendMode `MULTIPLY`) 위에 단색 레이어(`images 1`, blendMode `COLOR`)를 얹어 타입별 색으로 틴트하는 방식입니다. `images 1`의 색상은 변수에 바인딩하지 않고 raw hex로 유지합니다. 블렌드 모드 틴팅 기법을 위한 의도된 설계이며 변수 바인딩의 예외입니다. `Tomorrow`만 별도 벡터 아이콘이라 이 구조가 아닙니다.

**Content**
- "판매자로켓"/"로켓프레쉬"/"로켓직구"/"로켓내일" 4개 고정 문구만 사용(자유 입력 아님, Type 값과 1:1 대응)
- 전부 5자 고정

**Status Badge**

리뷰 등에서 작성자의 구매 이력 신뢰 신호를 표시하는 outline 배지입니다. 파란 테두리와 텍스트로 구성되며 배경은 없습니다.

**사용 가이드**
- ✅ Do: Review Card 등에서 리뷰 작성자가 재구매했는지, 한 달 이상 사용했는지를 짧게 표시할 때 사용합니다.
- ❌ Don't: 근거 데이터가 없으면 노출하지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Repurchase(재구매) / OneMonthUse(한달사용) | — | 둘 다 동일한 outline 스타일, 상태 변화 없음. 신뢰 신호 종류가 늘어나면(예: 장기사용, 대량구매) 값을 추가하는 방식으로 확장 |

**Content**
- "재구매"/"한달사용" 2개 고정 문구(3~4자)
- 신뢰 신호가 실제로 검증된 경우에만 표시

### 2.2 Banner

(미정) Banner의 정의가 아직 합의되지 않았습니다. Figma에는 `Cart / Notification Banner`, `Cart / Countdown Banner` 두 컴포넌트가 있습니다. 이 디자인 시스템에서 Banner가 무엇인지를 먼저 합의한 뒤 이 절을 채웁니다.

### 2.3 Button

Button은 역할이 다른 3개의 독립 컴포넌트로 구성됩니다. 셋을 같은 축으로 혼동하지 않습니다.

**비교**

| 항목 | Button (CTA) | Icon Button | Text Button |
|---|---|---|---|
| 용도 | 화면의 핵심 행동 | 라벨 없는 보조 액션 | 가장 낮은 강조의 내비게이션 |
| 텍스트 | 있음(라벨) | 없음(아이콘만) | 있음(텍스트+화살표) |
| 강조 수준 | 높음(Primary~Line type gray 3단계) | 중간 | 가장 낮음 |
| 사용 맥락 | "구매하기" 등 CTA | 리뷰 "도움이 돼요" | "전체보기 〉" |

**Button (CTA)**

구매, 상세 확인 같은 사용자의 핵심 행동을 유도하는 기본 CTA 컴포넌트입니다. Primary/Secondary/Line type gray 3종 시각 위계와 4단계 크기를 조합해 행동의 중요도를 구분합니다.

**사용 가이드 — Type별 실제 사용 맥락**
- ✅ Do (`Primary`): 화면당 가장 중요한 단일 행동에 씁니다. 예: "구매하기", "총 1개 상품 구매하기"
- ✅ Do (`Secondary`): 두 가지 맥락으로만 씁니다. 첫째는 2-Button 레이아웃에서 Primary와 짝을 이루는 차순위 행동입니다. "바로구매"+"장바구니 담기" 조합의 "장바구니 담기"가 그 예입니다. 둘째는 Action Bar 안에서 단독으로 쓰이는 낮은 강조의 1개 행동입니다. "상품 정보 더보기"가 여기에 해당합니다.
- ✅ Do (`Line type gray`): 진행 중인 다른 행동이 없을 때의 낮은 강조 단독 대기 행동에 씁니다. `Item Card / Cart` 품절 상태의 "재입고 알림 신청"이 그 예입니다.
- ✅ Do: 리스트나 카드 내부처럼 좁은 공간에는 `Small`/`XSmall`을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Primary / Secondary / Line type gray | Primary | 사용 맥락은 위 사용 가이드 참고 |
| Size | Large(48) / Medium(40) / Small(32) / XSmall(28) | Large | Radius는 Large·Medium `medium`(8), Small·XSmall `small`(4) |
| Status | Default / Pressed / Disabled | Default | Medium은 Default만 존재 |
| Show Leading Icon | Boolean | true | — |
| Show Trailing Icon | Boolean | true | — |

**Content**
- 라벨은 4~9자의 완료형 동사구. "~하기"로 끝나는 행동 지시형(예: "구매하기", "장바구니 담기", "총 1개 상품 구매하기", "상품 정보 더보기")
- 아이콘은 라벨 보완용으로만 사용, 아이콘 단독 사용 없음

**Icon Button**

라벨 텍스트 없이 아이콘 하나로 동작을 표현하는 보조 액션 버튼입니다.

**사용 가이드**
- ✅ Do: 텍스트 라벨을 붙일 공간이 부족하거나, 반복 노출되는 짧은 보조 액션(리뷰 "도움이 돼요" 등)에 사용합니다.
- ❌ Don't: 화면의 주 행동(CTA)에는 쓰지 않습니다. 그것은 `Button (CTA)`의 역할입니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Secondary / Line type gray | Secondary | Primary형 없음 |
| Size | Medium(40) / Small(32) | Medium | Radius는 `full`(pill). Button(CTA)의 `medium`(8) 계열과 다른 형태이므로 혼동 주의 |
| Status | Default / Pressed | Default | — |
| Show Leading/Trailing Icon | Boolean | — | — |

**Content**
- 텍스트 라벨 없음. 아이콘 하나로 의미 전달
- 현재 용례는 리뷰 "도움이 돼요"(공감) 하나
- (미정) 다른 용도에 적용할 때의 아이콘 선택 기준

**Text Button**

배경과 테두리 없이 텍스트(+화살표)만으로 표현하는 가장 낮은 강조의 링크형 버튼입니다.

**사용 가이드**
- ✅ Do: 섹션 헤더 옆 "더보기"처럼, 리스트나 카드 다음 단계로 이동하는 보조 내비게이션에 사용합니다.
- ❌ Don't: 행동을 완료시키는 CTA에는 쓰지 않습니다. 그것은 `Button (CTA)`의 역할입니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Default(회색 텍스트) / accent(파란 텍스트+Bold) | Default | — |
| Size | Default / Large | Default | — |

**Content**
- 라벨은 "전체보기 〉" 고정. 명사+"보기"+화살표 아이콘 패턴
- (미정) 다른 문구 변형의 허용 여부

### 2.4 Chip

**비교**

| 항목 | Chip | Option Chips |
|---|---|---|
| 용도 | 목록/검색 필터링 | 상품 옵션(용량·색상 등) 선택 |
| 선택 방식 | 다중 선택 | 단일 선택(옵션 하나) |
| 선택 표현 | `Status=Selected` 값 자체 | `Selected`(Boolean)+파랑 2px 테두리 오버레이 |
| 형태 | pill | pill |

**Chip**

목록이나 검색 결과를 정렬·필터링하기 위한 pill 형태의 선택형 태그입니다.

**사용 가이드**
- ✅ Do: 카테고리, 속성, 정렬 기준 등 다중 선택이 가능한 필터 UI에 사용합니다.
- ❌ Don't: 상품 옵션 선택에는 쓰지 않습니다. `Option Chips`를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / Selected / Pressed | Default | 선택 표현은 `Status=Selected` 값 자체로 함. 토글 동작: `Selected` 상태를 한 번 더 누르면 `Default`로 돌아감(선택 해제). (미정) Option Chips의 Boolean 방식과 다른 것이 의도된 차이인지 |
| Dropdown | Boolean | — | 하위 옵션 펼침 화살표 |
| rocket | False / tomorrow / fresh / global | False | 배송 아이콘+라벨 포함 특수 칩 |
| Show Icons | Boolean | — | — |

**Content**
- 라벨은 1~8자의 짧은 명사·형용사구. 초성 단독("ㄱ","ㄴ"), 브랜드명("도브"), 속성 키워드("무료배송","향이 좋아요") 등(Button과 달리 행동 동사형 아님)
- "전체"는 필터 리셋용 고정 관용구

**Option Chips / Size, Option Chips / Thumbnail**

상품 옵션(용량·색상 등)을 선택하는 pill 형태 컴포넌트입니다. 텍스트만 있는 `/ Size`와 이미지가 포함된 `/ Thumbnail` 두 하위 패밀리로 나뉩니다. 상품 유형이 늘면 `Option Chips / {유형}` 패턴으로 계속 추가할 수 있습니다.

**사용 가이드**
- ✅ Do: 상품 상세나 리스트에서 옵션을 하나만 선택해야 하는 UI에 사용합니다.
- ❌ Don't: 필터링 목적이면 쓰지 않습니다. `Chip`을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / low_stock(재고부족·빨간 "N개 남음") / sold_out(품절·회색 비활성) | Default | "N개 남음"은 항상 빨강. (미정) 노출 임계값 |
| Selected | Boolean | — | 파랑 2px 테두리 오버레이. Chip과 다른 메커니즘 |

**Content**
- `/ Size`는 "240"처럼 단위 없는 짧은 숫자. (미정) 단위 표기 규칙
- `/ Thumbnail`은 "7CA 웜톤 브라운"처럼 옵션 코드+속성명 조합, 길면 말줄임(...) 처리

### 2.5 Dialog

**Dialog**

화면 위에 모달로 뜨는 확인창입니다. 제목과 본문 설명, 버튼 1~2개로 구성됩니다. 사용자의 확인이나 선택이 필요할 때 화면 흐름을 멈추고 개입합니다.

**Anatomy**: `header`(제목+Status별 아이콘) → `body`(설명) → `button`(1~2개, `Button=X`는 버튼 영역 자체가 없음)

**사용 가이드**
- ✅ Do: 정보 확인(반품 정책 등)이나 명확한 예/아니오 결정이 필요할 때 사용합니다.
- ❌ Don't: 가볍게 스쳐가는 알림에는 쓰지 않습니다. `Toast`(§2.7)를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / Error / Warning | Default | 헤더에 상태 아이콘만 추가, 레이아웃 동일 |
| Button | 1 Button / 2 Button / X | 1 Button | `X`는 버튼 영역 자체가 없음(구분선·스크롤바도 3요소만 적용, 아래 참고) |
| Show Scroll | Boolean | false | 본문 초과 시 스크롤 UI 미리보기(아래 "스크롤 동작" 참고) |

헤더 상단 모서리만 `large`(16), 본문은 `none`(0)입니다.

**Content**
- 제목은 1줄의 간결한 명사형(예: "반품 안내")
- 본문은 여러 줄 허용(예: 정책 설명문+최대 3개 불릿)
- (미정) 버튼 라벨 규칙. 현재 사례가 1건뿐

**스크롤 동작**

본문(Body) 내용이 길어지면 `Dialog`는 뷰포트 전체를 채우지 않고 고정 크기 안에서 본문만 스크롤됩니다. Figma는 콘텐츠량에 따라 스크롤 여부를 동적으로 계산하지 못합니다. 그래서 아래 발생 기준은 개발 구현 규칙으로 문서화합니다. Figma에서는 `Show Scroll` Boolean으로 두 상태(스크롤 없음/스크롤 발생 중)를 미리 볼 수 있습니다.

*스크롤 발생 기준 — 상대 단위 기준(고정 px 하드코딩 금지)*

디바이스마다 뷰포트 높이가 다르므로 트리거 조건은 고정 px가 아니라 상대 단위로 계산합니다.

| 항목 | 값 |
|---|---|
| `Dialog` 전체 높이 | `max-height: 70vh` |
| 고정 영역(스크롤 안 됨) | Header 56px + Button 78px = 134px (`Button=X` variant는 Header 56px만 고정, 버튼 영역 자체가 없음) |
| Body 높이 | `max-height: calc(70vh - 134px)`(`Button=X`는 `calc(70vh - 56px)`) |
| 스크롤 트리거 | Body 콘텐츠 실제 높이가 위 `max-height`를 초과하면 `overflow-y: auto`로 자동 스크롤 |

> 참고: 뷰포트 800px 기준으로 환산하면 Body `max-height`는 약 426px입니다. 계산 예시일 뿐이며 트리거 조건으로 하드코딩하지 않습니다.

*스크롤 UI 4요소* (`Show Scroll=true`일 때 노출, 7개 variant 모두 적용)
- **Header/Body 구분선**(`divider-top`): 스크롤 영역의 시작을 시각적으로 분리합니다. `Background/Divider` 토큰에 바인딩되어 있습니다.
- **Body/Button 구분선**(`divider-bottom`): 아래에 더 많은 콘텐츠가 있음을 암시하고 버튼 영역이 고정임을 표현합니다. `Background/Divider` 토큰에 바인딩되어 있습니다. `Button=X` variant는 하단 버튼 영역 자체가 없어 이 구분선을 넣지 않습니다(3요소만 적용).
- **스크롤바**(`scroll-track`+`scroll-thumb`): 전체 대비 현재 위치를 표시하는 얇은(4px) 인디케이터입니다. `scroll-thumb`은 `Gray/400`에 바인딩되어 있습니다. `scroll-track`은 의도적으로 바인딩하지 않습니다. 반투명 검정(6% opacity) 표현 기법이 토큰 색상값과 맞지 않아, Rocket Badge의 블렌드 틴팅과 같은 원칙으로 예외 처리합니다.
- **Body clipping**: `body` 프레임은 `clipsContent:true`라 넘치는 콘텐츠가 자동으로 잘립니다.

### 2.6 Heading

Heading은 `App Bar`, `App Bar Small`, `Section Header`, Cart 전용 컴포넌트 4종을 포함합니다.

**비교**: `App Bar`는 화면 상단형 바 패밀리입니다. Type별 실제 위치는 서로 다르며 아래에서 설명합니다. `App Bar Small`은 그 아래 붙는 정렬·보기전환 보조 툴바입니다. `Section Header`는 화면 내부 콘텐츠 그룹의 소제목입니다. Cart 전용 4종은 장바구니 화면에서만 쓰는 단일 목적 바입니다.

**App Bar**

여러 상황에서 쓰이는 상단형 바 컴포넌트 패밀리입니다. 모든 Type이 화면 최상단 고정인 것은 아닙니다. Type별 실제 배치는 아래와 같습니다.

**사용 가이드 — Type별 실제 배치**
- `Title`/`Category`/`Search`: ✅ 최상단 고정입니다. Status Bar 바로 아래, 화면 프레임의 첫 자식으로 배치합니다. `Search`는 확인된 사례가 1곳뿐이라 참고용입니다.
- `Filter`: ❌ 최상단이 아닙니다. 최상단의 `Category` App Bar 바로 아래에 2번째 줄 필터 바로 쌓습니다. 순서는 Status Bar → Category → Filter → App Bar Small → 콘텐츠입니다. 독립적으로 화면 맨 위에 오지 않습니다.
- `Filter Compact`: ❌ 최상단이 아닙니다. 상품 상세페이지처럼 긴 스크롤 콘텐츠의 중간(예: 리뷰 섹션 부근)에 그 섹션 전용 필터 행으로 배치합니다. 화면 상단과 무관합니다.
- `Index`: ❌ 최상단이 아닙니다. `Bottom Sheet / Interactive` 내부의 자모 인덱스 점프 스트립으로 씁니다. 화면 레벨 요소가 아니라 모달 안의 보조 UI입니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Title / Filter / Filter Compact / Search / Category / Index | Title | 단일 축. `Filter`/`Filter Compact`/`Index`는 배치 맥락이 `Title`/`Category`/`Search`와 다름(2번째 줄 필터 / 섹션 내부 필터 / 모달 내부 요소). 별도 컴포넌트로 분리하지 않고 같은 축에 유지 |

**Content**
- `Title` 제목은 "장바구니"처럼 2~4자 명사형
- `Category`는 카테고리명 그대로("바디워시", "커피/차")
- `Filter` 칩은 2~6자 조건어("전체","로켓내일","4점 이상")
- `Search` 플레이스홀더는 "쿠팡에서 검색하세요" 형태(규칙으로 확정된 문구는 아님)
- `Search` 타입 내부에는 별도 컴포넌트 `Search`가 중첩되어 있으며 `Status`(Default/typing/typing_long/done) 4개 variant를 가짐
  - `Default`: 플레이스홀더만 있는 빈 상태
  - `typing`/`typing_long`: 입력 중(커서 깜빡임 표시)
  - `done`: 검색을 마치고 결과를 보여주는 화면에 사용(커서 없이 검색어만 남은 상태)

**App Bar Small**

목록형 콘텐츠 상단에서 정렬 기준과 보기 방식(리스트/그리드)을 전환하는 보조 툴바입니다.

**사용 가이드**
- ✅ Do: App Bar 아래, 정렬·보기전환이 필요한 콘텐츠 섹션 상단에 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | List / Grid | List | "현재 보기 방식"이 아니라 "전환 가능한 다른 보기"를 가리킴. 그리드 화면의 인스턴스는 `Type=List` 아이콘을 사용. (미정) 리스트 화면에서 `Type=Grid`를 쓰는지 여부. 정렬 드롭다운은 공통 |

**Content**
- 정렬 기준 "추천순"처럼 2~4자 명사형 + 드롭다운 화살표

**Section Header**

화면 내 콘텐츠를 의미 단위로 구분하고, 필요하면 부가 액션(전체보기 등)을 제공하는 소제목입니다.

**사용 가이드**
- ✅ Do: 한 화면에 여러 정보 그룹이 있을 때 그룹 경계를 표시하는 데 사용합니다. 상품 상세의 "리뷰", "함께 사면 좋은 상품"이 그 예입니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| State | text (값 1개만 남음) | text | Figma가 COMPONENT_SET의 마지막 variant 축 삭제를 허용하지 않아 값 1개짜리 축으로 유지. 기능상 문제 없음 |
| Show Action | Boolean | true | 우측 액션 버튼(`Text Button`, "전체보기 〉")의 `visible`을 직접 제어 |

**Content**
- (미정) 제목 텍스트 규칙. 현재 플레이스홀더 "Title"만 존재
- 액션 텍스트는 "더보기 〉" 형태

**Cart 전용 컴포넌트 4종** (Variant 없는 단일 컴포넌트, 장바구니 화면 전용)

| 컴포넌트 | Description | When to use | Content rules |
|---|---|---|---|
| Cart / Notification Banner | 혜택 알림 신청 유도 배너 | 장바구니 상단, 미신청 사용자에게 | "장바구니 혜택이 생기면 바로 알려드릴게요" + "알림받기" |
| Cart / Countdown Banner | 할인 종료 카운트다운 배너 | 장바구니 전체 단위 시간제한 할인 안내 | "할인 종료 HH:MM:SS" + "남은 상품이 있어요" |
| Cart / Address Row | 배송지 표시+변경 진입점 | 배송지 확인·변경 필요 지점 | "이름 (행정구역)" + "변경 〉" |
| Cart / Selection Toolbar | 전체선택/선택삭제 툴바 | 다중 선택·일괄 삭제 리스트 상단 | "전체선택" / "선택삭제"(각 4자 고정) |

### 2.7 Message Box

**Message Box**

화면 안에 고정으로 노출되는 안내·유의사항 배너입니다. 닫기 버튼이 없고, 사용자 조작과 무관하게 화면에 계속 남아 있습니다. 화면 최하단에 잠깐 떴다 사라지는 `Toast`(아래 참고)와는 별개 컴포넌트입니다.

**사용 가이드**
- ✅ Do: 결제 실패, 재고 임박처럼 사용자가 화면을 벗어나기 전까지 계속 인지해야 하는 안내·유의사항에 사용합니다.
- ❌ Don't: 잠깐 보여주고 사라져도 되는 조작 결과 피드백에는 쓰지 않습니다. `Toast`를 씁니다.
- ❌ Don't: 닫기 버튼을 추가하지 않습니다. 닫을 수 있어야 하는 안내에는 `Dialog`(§2.5)나 `Toast`를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Error / Warning | Error | Error=`Feedback/Error Background`+`Error Text`(연핑크 배경+진빨강), Warning=`Feedback/Warning Background`+`Warning Text`(연노랑 배경+진골드). 아이콘 원도 각각 같은 Text 토큰에 바인딩 |

**Content**
- 안내 문구는 한 줄로 끝나지 않을 수 있어 자동 줄바꿈 허용(§4 긴 텍스트 처리 원칙 참고)
- 아이콘은 원형 배경 위의 "i"(정보) 글리프. `Icon / Status`(경고 느낌표 아이콘)와는 별개인 Message Box 전용 아이콘

---

**Toast**

화면 하단에 잠깐 떴다 사라지는 짧은 상태 메시지(스낵바)입니다. 사용자 조작 결과를 방해 없이 알려줍니다.

**사용 가이드**
- ✅ Do: 즉각적인 피드백은 필요하지만 화면 흐름을 막을 필요는 없을 때 사용합니다(예: "찜 목록에 추가됨").
- ❌ Don't: 확인이 필요한 결정에는 쓰지 않습니다. `Dialog`(§2.5)를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / Success / Error | Default | 성공 상태는 파랑(`Blue/400`)으로 표시 |
| Button | Boolean | — | 우측 텍스트 링크 |
| Line | 1 / 2 | — | — |
| Icon | Boolean | — | — |

**Content**
- 메시지 문구는 두 줄까지 사용. 3줄 이상 노출은 지양
- (미정) 버튼 링크의 언어 컨벤션. 현재 "View"/"바로가기"로 국영문 혼재

**동작 정의**

아래는 개발 구현 규칙입니다.

| 항목 | 값 |
|---|---|
| 위치 | 화면 최하단에서 16px 고정. `Bottom Navigation`(87px 높이) 유무와 무관하게 항상 이 위치. Bottom Navigation이 있는 화면에서도 그 위로 피하지 않고 겹쳐서 표시 |
| 노출 시간 | 3초 |
| 근거 | Toast 자체 폭 328px = 360px 화면폭 − 좌우 16px(Spacing `16` 토큰과 일치). `Action Bar`(고정 CTA 바)도 16px 하단 padding을 쓰고 있어 "화면 최하단 여백 16px" 관례와 일관됨 |

### 2.8 Item Card

Item Card는 상품 카드 3종과 그 하위 컴포넌트, 그리고 Review Card(`Review / Card`, `Review / Summary`)를 포함합니다.

**비교**

카드 3종의 비교입니다. 이름만 보면 Grid와 Recommendation을 혼동하기 쉬우니 아래 Recommendation 항목을 함께 참고합니다.

| 항목 | Item Card / Grid | Item Card / Recommendation | Item Card / Cart |
|---|---|---|---|
| 카드 폭 | 160px | 130px | — |
| 배치 방식 | 360px 화면폭에 꽉 차는 2열 그리드 | 460px 폭 컨테이너의 횡스크롤 캐러셀(카드가 잘려서 노출) | 장바구니 전용 확장형(수량 조절·삭제 포함) |
| 사용 위치 | "상품목록_그리드 타입" 화면 2열 그리드 | 장바구니 "다시 구매하세요", 상세 "같이 둘러볼만한 상품" | 장바구니 화면 |

`Price Block`, `Spec Row`, `Rating Display`는 이 카드들 내부에서 재사용되는 하위 컴포넌트입니다. 카운트다운은 단위에 따라 역할이 나뉩니다. `Order Deadline`은 상품 단위, `Cart / Countdown Banner`(§2.6)는 장바구니 단위입니다.

**Item Card / Grid**

목록에서 상품을 표현하는 카드 컴포넌트(grid 밀도)입니다. 이미지, Price Block, Rating Display, Spec Row를 내부에 포함합니다. 카드 폭은 160px입니다.

**사용 가이드**
- ✅ Do: 카테고리 목록, 검색 결과 등 화면 폭(360px)에 꽉 차는 2열 그리드에서 사용합니다(횡스크롤 아님). "상품목록_그리드 타입" 화면이 그 예이며 App Bar+필터 칩+정렬 드롭다운+2열 그리드로 구성됩니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| — | Variant 없는 단일 컴포넌트 | — | Type 등 별도 축은 없음 |
| Show Label | Boolean | true | 카드 내장 `Label`(§2.9) 인스턴스의 `visible`을 직접 제어. 기본값은 `Type=Promotion`("특가 진행중"). 다른 Type으로 바꾸려면 detach 없이 인스턴스를 직접 스왑(이 속성은 노출된 서브 프로퍼티가 아니라 단순 on/off) |
| Discount(노출됨) | Wow / General / None | Wow | 자체 variant가 아니라 내장 `Price Block`(§2.8) 인스턴스에 `isExposedInstance=true`를 설정해 노출한 것. Item Card 인스턴스를 Figma에서 선택하면 `Price Block`의 `Discount`를 바로 편집 가능. `item info`가 auto-layout(HUG)이라 `Wow`/`General`(60px)↔`None`(40px) 높이 차이가 카드 전체 높이에 자동 반영 |

**Content**
- (미정) 상세 콘텐츠 패턴. 다른 Item Card 계열과 같은 상품명·가격 표기 규칙을 따르는지 검증 전
- `Label`은 `item info` 스택 맨 위(상품명보다 위)에 위치. 이미지 위에 배지를 오버레이로 얹지 않고, Rocket Badge와 Status Badge처럼 텍스트 스택 안에 인라인으로 두는 관례를 따름
- `item info`가 `itemSpacing=4`의 auto-layout이라 `Show Label=false`일 때 카드가 30px(라벨 26 + gap 4) 줄어들고 gap도 함께 접힘

**Item Card / Recommendation**

이미지, 상품명, 가격, 배송, 평점을 한 번에 스캔할 수 있게 묶은 상품 카드입니다. 카드 폭은 130px입니다.

**사용 가이드**
- ✅ Do: 장바구니와 상품 상세페이지 하단의 횡스크롤 추가구매 유도 캐러셀에 사용합니다. 장바구니 "다시 구매하세요", 상품 상세 "같이 둘러볼만한 상품" 섹션의 가로 스크롤 행이 여기에 해당합니다. 460px 폭 컨테이너가 360px 화면보다 넓어 카드가 잘려서 노출됩니다.
- ❌ Don't: 카테고리 목록이나 검색 결과에는 쓰지 않습니다. 그 역할은 위 `Item Card / Grid`가 담당합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Default / Button | Default | 하단 CTA 노출 여부 |
| Discount | Wow / General / None | Wow | 총 2×3=6 variant. `Wow`는 와우 할인(빨간 배지), `General`은 일반 할인(배경 없는 텍스트 %), `None`은 할인 없음. `Price Block`(§2.8 상세 참고)과 동일한 분류. 각 variant는 대응하는 `Price Block` `Discount` variant를 내부에 포함 |

**Content**
- 상품명은 "브랜드명+제품명+옵션"을 쉼표로 이어 쓰는 패턴("도브 화이트피치 리밸런싱 바디워시, 1kg, 2개")
- 상품명 텍스트 색상은 `Semantic Text/Primary`(=`Gray/900`). `Item Card / Grid`, `Recommendation`, `Cart`의 모든 마스터에 공통
- 가격은 천단위 콤마+"원", 할인율은 정수 %
- 배송 배지는 Rocket Badge, 평점은 Rating Display 재사용
- 상품명 텍스트는 두 조건을 함께 지킵니다(§4 원칙 11 참고).
  - 글자 수·줄 수와 무관하게 항상 2줄 분량의 `FIXED` 높이(40px) 박스를 차지합니다. 1줄 상품명과 2줄 상품명이 나란히 놓여도 카드 전체 높이와 가격 등 하위 요소의 위치가 같아집니다.
  - 그 박스 안에서 `textAlignVertical: TOP`으로 앵커링합니다. 상품명의 시작 줄 위치가 항상 같아집니다.
  - 둘 중 하나만 지키면 어긋남이 남습니다. 박스가 `HUG`면 카드 높이가 텍스트 길이에 따라 줄어들고, `TOP` 없이 `CENTER`면 박스 높이는 같아도 텍스트 시작선이 내려갑니다.

**Item Card / Cart**

장바구니에 담긴 상품 1건을 보여주는 확장형 카드입니다. 수량 조절, 삭제, 재고 상태를 포함합니다.

**사용 가이드**
- ✅ Do: 장바구니 화면 전용으로 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Stock | Available / OutOfStock | Available | `Available`은 체크박스+Quantity Stepper. `OutOfStock`은 체크박스 비활성+"재입고 알림 신청" 버튼+"일시품절" 라벨 |
| Discount(노출됨) | Wow / General / None | Wow(`Available`) / None(`OutOfStock`) | 자체 variant가 아니라 내장 `Price Block`(§2.8) 인스턴스를 `isExposedInstance=true`로 노출한 것(`Item Card / Grid`와 동일 기법). 두 `Stock` variant 각각의 `Price Block` 인스턴스에 독립적으로 적용. `OutOfStock`의 기본값은 `None`(품절 상품은 할인 노출 안 함)이며 필요하면 변경 가능 |

**Content**
- 상품명 패턴은 `Item Card / Recommendation`과 동일
- "일시품절" 고정 문구. (미정) 문구 변형 여부

**Price Block**

정가, 할인율, 최종가, 단위가를 하나로 묶어 보여주는 가격 표시 전용 컴포넌트입니다. 이 그룹에서 가장 널리 재사용됩니다.

**사용 가이드**
- ✅ Do: 가격이 노출되는 모든 곳에서 재사용합니다. 화면마다 가격 UI를 새로 조합하지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Discount | Wow / General / None | Wow | 할인 종류는 2가지. `Wow`는 와우(WOW) 할인으로 할인율을 빨간 배경 pill로 강조(배경 `Semantic Background/Price`, 텍스트 `Semantic Text/On Color`)하고 최종가·단위가도 `Semantic Text/Price`(빨강). `General`은 일반 할인으로 할인율을 배경 없이 텍스트로만 표시하고 최종가·단위가는 무채색. 정가 취소선(`Gray/600`)은 둘 다 공통. `None`은 할인이 없어 최종가·단위가 전부 `Semantic Text/Primary`(=`Gray/900`). 파랑(Blue)은 어느 상태에서도 쓰지 않음 |
| Show Unit Price | Boolean | true | — |

**Content**
- 가격 천단위 콤마+"원", 할인율 정수 %, 단위가 "(100ml당 715원)" 형식
- 그람, 밀리리터 등으로 단위 환산이 불가능한 상품은 `Show Unit Price=false`로 단위가 줄 자체를 숨김

**`Discount` 프로퍼티를 상위 Item Card에 노출하는 방법**
- `Item Card / Grid`(variant 없는 단일 컴포넌트)와 `Item Card / Cart`(Stock만 variant 축)에는 자체 `Discount` 프로퍼티가 없습니다. 내장된 `Price Block` 인스턴스에 `isExposedInstance=true`를 설정해, Item Card 인스턴스를 선택하면 `Price Block`의 `Discount`를 바로 편집할 수 있게 노출합니다. 두 컴포넌트 모두 `Price Block`을 담는 조상 프레임이 전부 auto-layout(HUG)입니다. 그래서 `Discount` 전환 시 높이 차이(Wow/General 60px ↔ None 40px)가 레이아웃에 자동 반영됩니다.
- `Item Card / Recommendation`은 자체 `Discount` variant 축을 갖습니다(§2.8). `Type`×`Discount` 2×3=6종으로 Price Block과 동일한 3분류를 따릅니다.

**Spec Row**

배송 유형과 적립 혜택을 한 줄로 보여주는 아이콘+텍스트 로우입니다.

**사용 가이드**
- ✅ Do: 상품 카드나 상세에서 배송 조건과 적립 혜택을 안내할 때 사용합니다.
- ✅ Do: `CouponApplied`(쿠폰 할인 적용됨)는 `Item Card / Cart`에서만 사용합니다. 장바구니에 담긴 뒤에야 쿠폰이 실제로 적용되는 흐름이기 때문입니다. 아직 담기지 않은 상품을 보여주는 `Recommendation`/`Grid`에는 쓰지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Free / Tomorrow / Fresh / Seller / Shipping / Return / LowStock / CouponApplied | — | — |
| Show ETA | Boolean | — | Show Reward와 독립적으로 조합 가능 |
| Show Reward | Boolean | — | — |
| ETA Text / Reward Text | TEXT | — | — |

**Content**
- ETA "내일 도착"/"내일 새벽 도착"("~도착" 어미)
- Reward "최대 770원 적립"("최대 N원 적립" 형식)
- LowStock: "재고 N개 남음" 형식, 아이콘 없이 순수 텍스트, `Semantic Text/Urgent`(=`Red/400`), 폰트 크기 13px. 다른 Type의 12px보다 크며 긴급 역할을 강조하기 위한 의도적 차이
- CouponApplied: "쿠폰 할인 적용됨" 형식, `Order Deadline`의 `Type=Default`와 동일 스펙(아이콘 16×16, `itemSpacing=2`, 텍스트 12px). 아이콘은 `Icon / Placeholder Square`를 임시로 사용 중
- CouponApplied의 텍스트 색은 `Semantic Text/Price`(=`Red/400`). 긴급이 아니라 조건부 가격 역할이라 `Text/Urgent`가 아닌 `Text/Price` 사용
- `Order Deadline`(§2.8) 텍스트는 `Text/Urgent`, Spec Row의 배송 도착 텍스트("내일 도착" 등)는 `Text/Delivery`에 바인딩

**Rating Display**

별 아이콘, 평점 숫자, 리뷰 수를 한 세트로 보여주는 평가 요약 컴포넌트입니다.

**사용 가이드**
- ✅ Do: 평점 정보가 필요한 모든 곳에서 공통으로 재사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Review | Filled / Empty | Filled | `Filled`는 주황 별+평점+리뷰수. `Empty`는 회색 별+"−"+"(0)", 데이터 없음을 0이 아닌 "−"로 명시 구분 |
| Rating / Review Count | TEXT | — | — |

**Content**
- 평점은 소수 둘째 자리("4.83")
- 리뷰 수 9,999건 초과는 "(9,999+)" 상한 표기, 이하는 실수 그대로
- (미정) 상한 표기의 정확한 임계값 규칙. "(28,915)"처럼 상한을 넘는 표기 사례가 있음
- 별 아이콘 색상은 `Semantic Color/Rating/Star`(`#ff9c5c`)에 바인딩

**Order Deadline**

오늘 특정 시각까지 주문하면 언제 도착하는지 알려주는 카운트다운형 안내입니다. 상품 하나에 붙는 짧은 인라인 텍스트입니다.

**사용 가이드**
- ✅ Do: 로켓 배송처럼 주문 마감이 배송일에 영향을 주는 상품에서 구매를 서두르게 유도할 때 사용합니다.
- ❌ Don't: 장바구니 전체 단위 안내(체크박스·펼침 필요)에는 쓰지 않습니다. `Cart / Countdown Banner`(§2.6)를 씁니다. 둘 다 빨간 카운트다운을 쓰지만 단위가 다릅니다(상품 단위 vs 장바구니 단위).

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Default / WithDiscount | Default | `Default`는 시계+"00:42:25 내 주문 시". `WithDiscount`는 앞에 "할인 · " 접두 |

**Content**
- "시:분:초 내 주문 시" 고정 형식
- 색상은 항상 빨강

**Review / Card**

개별 리뷰 1건을 보여주는 카드입니다. 평점, 태그, 작성자, 본문, 이미지, "도움이 돼요" 버튼을 포함합니다.

**사용 가이드**
- ✅ Do: 상품 상세의 리뷰 목록에 사용합니다.
- ✅ Do: 커머스 밖에서도 재사용할 수 있습니다. 영화 상세 "관람평" 섹션에서는 attribute 페어("피부타입/향만족도" 등)를 "관람 형태/스포일러/추천 여부/재관람 의사"로, Status Badge "한달사용"을 "실관람"으로 override해 그대로 재사용합니다. "도움이 돼요" 버튼은 문구를 바꿀 필요가 없습니다. `product-name` 줄(리뷰 대상명)은 문맥상 불필요하면 `visible=false`로 숨깁니다. 별도 Boolean 없이 인스턴스 수준에서 숨길 수 있고 auto-layout이 빈 자리를 접습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Photo / Only Text | Photo | — |
| Show Status Badge | Boolean | true | 내부 `Status Badge`가 자기 `Type`(OneMonthUse/Repurchase) 프로퍼티를 가짐. 신뢰 신호 종류를 바꾸려면 Review/Card의 variant가 아니라 내부 `Status Badge` 인스턴스를 선택해 그 `Type`을 override. Review/Card 자체의 `Status` 축은 값 1개(`OneMonthUse`)만 남은 중복 축이며 Figma 제약으로 제거 불가 |

**Content**
- 상단 태그는 "라벨 값" 페어를 `|`로 구분해 나열
- 본문 2~3문장, 자동 줄바꿈
- 작성자 "윤**"(성+마스킹), 날짜 "2026.09.01"(점 구분)

**Review / Summary**

상품 전체 리뷰를 요약하는 위젯입니다. 평균 평점, 총수, 세부 만족도 막대그래프를 포함합니다.

**사용 가이드**
- ✅ Do: 리뷰 섹션 최상단, 개별 리뷰 목록 진입 전 요약 지점에 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| status | Default / none photo / no rating | Default | `no rating`은 Rating Display의 Empty와 같은 표기 관례. (미정) `none photo`의 구분 기준 |
| Show Row 1~4 | Boolean | true | 카테고리 유연성 확보용. 4개 속성 행(향 만족도/거품/보습력/세정력)이 모든 카테고리에 맞지는 않으므로 맞지 않는 행은 Boolean으로 끔. 남은 행의 라벨·정성평가·퍼센트 텍스트는 인스턴스에서 직접 override. 텍스트 레이어라 프로퍼티 노출 없이도 항상 override 가능하며, 프로퍼티가 과도하게 늘지 않도록 `Spec Row`처럼 텍스트까지 프로퍼티화하지는 않음 |

**Content**
- 세부 항목명(향 만족도/거품/보습력/세정력) + 정성 평가 문구("아주 만족해요") + 퍼센트 막대
- 커머스 밖 재사용: 영화 상세 "관람평" 섹션에서는 4개 속성 행 라벨을 "연출/스토리/연기/영상미"로 override. `photo_review`(리뷰 사진)는 `visible=false`로 숨김. Boolean 노출 없이 인스턴스 수준에서 숨길 수 있음

**행 레이아웃**: 4개 행은 `HORIZONTAL` auto-layout(`itemSpacing=12`)입니다. 왼쪽 라벨+정성평가 텍스트 그룹은 161px `FIXED`, 막대는 `FILL`, 퍼센트 텍스트는 36px `FIXED`에 `textAlignHorizontal=RIGHT`입니다. 그래서 4행의 퍼센트 숫자가 모두 같은 위치(X=292)에 정렬됩니다.

### 2.9 Label

**비교**

`Label`, `Chip`, `Rocket Badge`, `Status Badge`, `Spec Row`는 모두 짧은 태그성 정보를 보여준다는 점에서 겹쳐 보입니다. 전용 도메인과 전용 슬롯이 이미 있는지로 구분합니다.

| 항목 | Label | Chip | Rocket Badge | Status Badge | Spec Row |
|---|---|---|---|---|---|
| 도메인 | 범용(신규 태그는 기본적으로 여기) | 필터링·옵션 선택(인터랙티브) | 배송 전용(고정 4종) | 리뷰 신뢰 신호 전용(고정, 확장 시 값 추가) | 배송+적립 혜택 전용(`Item Card` 내장) |
| 형태 | 독립 pill/outline 태그 | 필터 칩(선택 상태 있음) | 살짝 둥근 사각형(`small`, 4) | outline 배지(배경 없음) | 아이콘+텍스트 한 줄(배지 아님) |
| 상호작용 | 없음 | 있음(Default/Selected/Pressed) | 없음 | 없음 | 없음 |
| 신규 태그를 여기 추가? | ✅ 기본값 | 필터링 목적일 때만 | ❌ 배송은 전용 컴포넌트가 있음 | ❌ 리뷰 신뢰 신호는 전용 컴포넌트가 있음 | ❌ 구조가 고정 로우라 확장 대상 아님 |

`Label`과 `Spec Row`가 둘 다 "배송/반품" 어휘(`Shipping`/`Return`)를 쓰는 것은 우연이며 서로 다른 슬롯입니다. `Spec Row`는 `Item Card` 안에 고정으로 들어가는 배송+적립 전용 로우입니다. `Label`은 독립적으로 붙는 낱개 태그입니다.

**Label**

상품 관련 짧은 안내 문구를 표시하는 범용 라벨입니다. 프로모션, 배송정책, 구매빈도 등 여러 목적으로 씁니다.

**사용 가이드**
- ✅ Do: 상품 카드나 상세에서 "특가 진행 중", "무료배송/반품 여부", "누적 구매 횟수" 등 부가 정보를 한 줄로 붙일 때 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Promotion(특가진행중·빨강 outline) / Return(무료반품·`SkyBlue/200` 배경) / Shipping(무료배송·`SkyBlue/200` 배경) / RepeatPurchase(N회 구매·회색 배경) | — | — |

**Content**
- 4~6자 단문
- "무료OO"(무료반품/무료배송)와 "N회 구매"(카운트+명사) 두 패턴. 새 Type을 추가할 때 이 중 하나를 따르는 것을 권장(강제 규칙 아님)
- `Shipping`/`Return`(무료배송/무료반품)의 텍스트는 `Semantic Text/Primary`(`Gray/900`), 배경은 둘 다 `Semantic Background/Benefit`(`SkyBlue/200`)에 바인딩. Primitive를 직접 참조하지 않음
- `Promotion`(특가진행중)의 텍스트는 `Red/400`, `RepeatPurchase`의 텍스트는 `Semantic Text/Primary`

### 2.10 List

**비교**

셋 다 카테고리 탐색과 관련되지만 레이아웃 형태가 다릅니다.

| 항목 | List | Category Tab | Category Menu Item |
|---|---|---|---|
| 형태 | 세로 리스트 row | 사이드바형 탭 아이템 | 이미지+라벨 그리드 아이템 |
| 선택 표현 | `Status`(Default/pressed) | 텍스트 색·굵기(배경 채움 아님) | — (variant 없음) |
| 사용 맥락 | 카테고리 전체보기, 검색 자동완성·최근 검색어 | 좌측 고정 사이드바 카테고리 내비게이션 | `Bottom Sheet` 하위 카테고리 3열 그리드 |

**List**

텍스트 기반 선택 항목을 세로로 나열하는 범용 리스트 행(row) 컴포넌트입니다.

**사용 가이드**
- ✅ Do: 상품 카테고리 전체보기, 검색 자동완성, 최근 검색어처럼 텍스트 위주의 세로 목록에 사용합니다.
- ❌ Don't: 이미지 중심 그리드에는 `Category Menu Item`, 탭형 선택에는 `Category Tab`을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| type | Default / Image(좌측 썸네일) / history(시계 아이콘+삭제 X) | Default | — |
| Status | Default / pressed(배경 회색) | Default | — |

**Content**
- `Default`/`Image` 타입은 "냉동 블루베리"처럼 짧은 명사구(8자 내외)
- `history` 타입(검색 기록)에는 이 글자 수 규칙을 적용하지 않음. 검색 기록은 사용자가 입력한 자유 텍스트라 길이가 일정하지 않음(예: "삼성전자 615L 2도어 냉장고", 18자)
- 텍스트가 폭을 넘으면 말줄임(ellipsis) 처리

**Category Tab**

카테고리 사이드바에서 카테고리 1개를 선택하는 탭 아이템입니다.

**사용 가이드**
- ✅ Do: 좌측 고정 사이드바형 카테고리 내비게이션(`Bottom Sheet` 카테고리 선택 등)에 사용합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default(회색 배경+회색 텍스트) / Selected(흰 배경+파란 굵은 텍스트) | Default | 배경 채움이 아니라 텍스트 색·굵기로 선택 표현(다른 Chip류와 다른 패턴) |

**Content**
- "식품"(2음절)부터 "패션의류/잡화"(슬래시 결합)까지 카테고리명 원문 그대로, 별도 축약 규칙 없음

**Category Menu Item**

이미지 썸네일과 라벨로 구성된 그리드형 카테고리/메뉴 항목입니다.

**사용 가이드**
- ✅ Do: `Bottom Sheet` 카테고리 화면의 하위 카테고리 그리드처럼 이미지와 함께 나열해야 할 때 사용합니다. 화면에서는 3열 그리드로 배치합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| — | Variant 없음(단일 컴포넌트) | — | 이미지 자리는 현재 빈 placeholder |

**Content**
- (미정) 글자 수·말줄임 규칙. 현재 인스턴스는 모두 placeholder "카테고리"이며 실제 카테고리명 사례가 없음
- 잠정적으로 같은 카테고리명을 표시하는 `Category Tab`의 규칙("식품"~"패션의류/잡화", 원문 그대로·별도 축약 없음)을 준용 권장

### 2.11 Navigation

**Bottom Navigation**

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

**비교**

`Show Scroll` 유무가 다른 설계 근거는 아래 "스크롤 동작"에서 설명합니다.

| 항목 | Bottom Sheet / Informational | Bottom Sheet / Interactive |
|---|---|---|
| 목적 | 순수 텍스트 전달 전용 | 교차판매/업셀 확인(`Type=Item`) 또는 전체 화면급 필터 선택(`Type=Filter`) |
| Variant 축 | `Status`×`Button`+`Show Icons` | `Type`(Item/Filter) |
| Show Scroll | 있음(6개 variant 전부, 구분선+스크롤바 4요소) | 없음(리스트·캐러셀 형태가 스크롤을 암시) |

**Bottom Sheet / Informational** — 순수 텍스트 전달 전용

화면 하단에서 올라오는 `Bottom Sheet` 형태의 확인창입니다. `Dialog`와 구조는 비슷하지만 하단에서 진입하고 드래그 핸들이 있습니다. 제목과 본문 설명 외에 이미지, 목록, 복합 위젯은 다루지 않습니다.

**사용 가이드**
- ✅ Do: `Dialog`와 같은 확인·안내 목적이면서, 설명이 길거나 한 손 조작에 더 적합한 하단 진입이 필요할 때 사용합니다.
- ❌ Don't: 목록 선택이나 필터링, 교차판매 유도처럼 상호작용이 필요하면 쓰지 않습니다. `Bottom Sheet / Interactive`를 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default / Error / Warning | Default | — |
| Button | 1 Button / 2 Button | 1 Button | — |
| Show Icons | Boolean | — | — |
| Show Scroll | Boolean | false | Dialog(§2.5)와 동일 패턴(아래 "스크롤 동작" 참고) |
| Title text / Discription | TEXT | — | `Discription`은 Figma 프로퍼티명의 오탈자(정상 철자 Description)를 그대로 표기한 것 |

헤더 상단은 `large`(16), 드래그 핸들은 `full`(pill)입니다.

**Content**
- 순수 텍스트 전달 전용. 제목+본문 설명 외 이미지·목록·복합 위젯 없음
- (미정) 카피 규칙. 현재 사례는 더미 텍스트("제목을 입력해주세요" 등)뿐

**Bottom Sheet / Interactive** — 교차판매/업셀 확인, 전체 화면급 필터 선택

화면 하단에서 올라오는 `Bottom Sheet` 형태의 선택/목록 UI입니다. `Type=Item`은 장바구니 담기 완료 후 "다른 고객이 함께 구매한 상품"을 보여주는 교차판매·업셀 확인입니다. `Type=Filter`는 전체 화면급 필터 옵션 선택입니다.

**사용 가이드**
- ✅ Do: 장바구니에 담긴 직후 추가구매를 유도해야 할 때(`Type=Item`), 검색/필터 조건을 화면 전체 규모로 선택해야 할 때(`Type=Filter`) 사용합니다.
- ❌ Don't: 단순 확인·안내에는 쓰지 않습니다. `Bottom Sheet / Informational`을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | Item / Filter | Item | 둘 다 헤더 상단 `large`(16) |

`Show Scroll`은 없습니다. 리스트·캐러셀 형태 자체가 스크롤을 암시해 별도 인디케이터가 필요하지 않습니다(근거는 아래 "스크롤 동작" 참고).

**Content**
- 완료 안내 "상품을 ~했어요"(완료형 어미), 아래 교차판매성 추가 상품 리스트 동반
- 필터 옵션 라벨 2~5자("전체","로켓","무료배송")
- 검색 placeholder "~를 검색하세요"
- 위 내용은 컴포넌트 내 예시 기준이며 확정 규칙은 아님

**높이 제한**

`Bottom Sheet`(Informational/Interactive 공통)는 화면 전체를 채우지 않고 화면 높이의 최대 80%까지만 차지합니다. `Dialog`의 70vh 규칙(§2.5)과 같은 이유로 Figma 프레임에는 제약을 걸지 않고, 아래 내용을 개발 구현 규칙으로 문서화합니다.

*높이 기준 — 상대 단위 기준(고정 px 하드코딩 금지, Dialog와 동일 원칙)*

| 항목 | 값 |
|---|---|
| `Bottom Sheet` 전체 높이 | `max-height: 80vh` |
| 스크롤 트리거 | 콘텐츠 실제 높이가 `80vh`를 초과하면 body 영역에 `overflow-y: auto` 적용 |

> 참고: 표준 모바일 아트보드(360×800px) 기준으로 환산하면 80vh는 약 640px입니다. 계산 예시일 뿐이며 트리거 조건으로 하드코딩하지 않습니다.

**스크롤 동작(Informational 전용)**

`Dialog`(§2.5)의 스크롤 UI 패턴을 `Bottom Sheet / Informational`에만 적용합니다. `Bottom Sheet / Interactive`에는 `Show Scroll` 자체가 없습니다.

*근거 — 왜 Informational만 스크롤 인디케이터가 필요한가*: 두 컴포넌트는 콘텐츠 성격이 근본적으로 다릅니다.
- Informational은 길이를 예측할 수 없는 자유 텍스트입니다. 제목+본문 설명(`Discription`)은 글자 수 제한이 없어 짧을 수도, 화면을 넘길 만큼 길 수도 있습니다. 텍스트만 봐서는 지금 보이는 것이 전부인지 아래에 더 있는지 사용자가 판단할 수 없습니다. 그래서 구분선과 스크롤바로 스크롤 경계를 명시적으로 알려줍니다.
- Interactive는 콘텐츠 형태 자체가 스크롤 가능함을 암시합니다. `Type=Filter`는 필터 칩이나 카테고리 목록처럼 리스트가 스크롤되는 것이 당연한 패턴입니다. `Type=Item`은 가로 스크롤 캐러셀이라 카드가 화면 끝에서 잘려 보이는 것 자체가 "더 있다"는 신호입니다. `Item Card / Recommendation`(§2.8) 같은 다른 캐러셀도 별도 스크롤바 없이 같은 방식으로 스크롤을 암시합니다.
- 따라서 `Show Scroll`은 콘텐츠 길이가 가변적인 순수 텍스트 컴포넌트에만 필요합니다. 리스트·캐러셀처럼 형태 자체가 스크롤을 암시하는 컴포넌트에는 필요하지 않습니다.

Figma는 콘텐츠량에 따라 스크롤 여부를 동적으로 계산하지 못합니다. 그래서 Informational은 `Show Scroll` Boolean으로 두 상태(스크롤 없음/스크롤 발생 중)만 미리 봅니다.

*스크롤 UI 요소* (`Show Scroll=true`일 때 노출, `Bottom Sheet / Informational` 6개 variant 전부)
- **상단 구분선**(`divider-top`): 헤더(드래그 핸들)와 body의 경계입니다. `Background/Divider` 토큰에 바인딩되어 있습니다.
- **하단 구분선**(`divider-bottom`): body와 하단 버튼 영역의 경계입니다. `Background/Divider` 토큰에 바인딩되어 있습니다. 6개 variant 전부 버튼 영역이 있어 예외 없이 적용합니다(`Dialog`의 `Button=X` 같은 생략 케이스 없음).
- **스크롤바**(`scroll-track`+`scroll-thumb`): `scroll-thumb`은 `Gray/400`에 바인딩되어 있습니다. `scroll-track`은 `Dialog`와 같은 원칙으로 의도적으로 바인딩하지 않습니다. 반투명 검정(6% opacity) 표현 기법이 토큰 색상값과 맞지 않기 때문입니다.
- **Body clipping**: body 프레임에 `clipsContent:true`가 적용되어 있습니다.

`Bottom Sheet / Interactive`의 `Type=Filter`는 고정 408px+`overflow-clip` body를 가집니다. 내부 콘텐츠가 724px이라 실제로 스크롤되는 구조이며 `Show Scroll`과는 무관합니다.

### 2.13 Tab

**비교**: `Tab Group`은 같은 데이터를 다른 관점으로 재구성해 보여주는 컨테이너입니다. `Tab Item`은 그 안의 개별 탭 버튼이며 독립적으로 배치하지 않습니다. 상품 카테고리 자체를 나열하는 용도가 아니라는 점에서 `Category Tab`(§2.10)과 역할이 다릅니다.

**Tab Group**

같은 종류의 콘텐츠 목록을 서로 다른 관점(필터 기준)으로 전환해서 보여주는 가로 탭 컨테이너입니다. 별개의 카테고리를 나열하는 것이 아니라 같은 데이터를 다른 기준으로 재구성해 보여줍니다. 장바구니 화면에서 지금 보이는 상품 목록의 출처를 전환하는 것이 그 예입니다.

**사용 가이드**
- ✅ Do: 한 화면 안에서 같은 성격의 리스트를 여러 관점으로 오갈 때 사용합니다. 장바구니 화면은 `Type=3 Tab`으로 "일반구매(6)"(Status=Selected, 기본 활성 탭)/"자주산상품"/"찜한상품(40)"을 전환합니다. 일반 구매 흐름 상품, 자주 구매한 상품, 찜한 상품이라는 사용자 개인의 구매·관심 이력을 관점별로 보여줍니다.
- ❌ Don't: 상품 카테고리(식품/뷰티 등)를 나열하는 용도로 쓰지 않습니다. 카테고리 나열에는 `Category Tab`(§2.10)을 씁니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Type | 2 Tab / 3 Tab / Swipe | — | 현재 사용 사례는 `3 Tab`뿐. `2 Tab`과 `Swipe`는 라이브러리에 정의만 있음. `Swipe`는 가로 스크롤 다수 탭 구조 |

**Content**
- 라벨 패턴의 근거는 `3 Tab`뿐. "일반구매(6)"/"찜한상품(40)"처럼 `라벨(숫자)` 형태로 항목 수를 병기하거나 "자주산상품"처럼 숫자 없이 사용. 라벨 길이 4~7자 내외
- (미정) `2 Tab`과 `Swipe`의 사용 기준

**Tab Item**

Tab Group을 구성하는 개별 탭 버튼(라벨+선택 여부)입니다.

**사용 가이드**
- ✅ Do: Tab Group 내부 전용으로 사용합니다. Tab Group의 마스터 variant 내부 자식으로만 두고 독립적으로 배치하지 않습니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default(회색) / Selected(파란 굵은 텍스트+밑줄) / Pressed(회색 배경 하이라이트) | Default | — |

**Content**
- "일반구매(6)"처럼 `라벨(숫자)` 형태로 항목 수 병기. 의미 있는 경우에만 쓰며 "자주산상품"처럼 숫자가 없는 라벨도 있음
- 라벨 길이 4~7자 내외

### 2.14 Thumbnail

**Thumbnail**

범용 이미지 자리 표시자 컴포넌트입니다. 실제 이미지가 들어갈 정사각형 영역을 크기별로 표준화합니다.

**사용 가이드**
- ✅ Do: 상품 이미지, 카테고리 아이콘 등 정사각형 이미지가 필요한 모든 곳에 사용합니다.
- ❌ Don't: Item Card처럼 이미지가 내장된 복합 컴포넌트에는 별도로 끼워 넣지 않습니다.
- ❌ Don't: 세로형(포스터 등) 비율의 이미지에는 사용하지 않습니다. 전 사이즈(xsmall~Xlarge)가 정사각형 전용이라 다른 비율에는 대응하지 못합니다. 이런 경우 Foundation 토큰(Radius, Color)만으로 별도 프레임을 구성합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Size | xsmall / small / medium / large / Xlarge | — | 전 사이즈 공통으로 회색 배경+이미지 placeholder 아이콘. (미정) 사이즈별 정확한 픽셀값 |

**Content**
- 텍스트 콘텐츠 없음(순수 이미지 컨테이너)
- 이미지 없을 때 항상 동일한 회색+아이콘 placeholder

### 2.15 Radio / Checkbox

토글 가능한 인터랙티브 Radio·Checkbox 컴포넌트는 아직 없습니다. `Icon / Radio`와 `Icon / Checkbox`는 선택된 모양만 있는 장식용 글리프입니다.

**Icon / Radio**

장식용 아이콘 글리프입니다. `Status=Default`(파란 원)/`Disabled`(회색 원) 두 상태뿐이며, "선택됨/선택 안 됨"을 토글하는 상호작용 상태가 아닙니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default(파란 원) / Disabled(회색 원) | Default | 아이콘 색상 변형일 뿐 "선택됨/선택 안 됨" 상호작용 상태가 아님 |

**Content**
- 텍스트 없음(순수 아이콘)

**Icon / Checkbox**

`Icon / Radio`와 같은 구조적 한계를 가진 장식용 글리프입니다. 두 variant(`Status=Default`/`Disabled`) 모두 내부 구조가 단일 VECTOR 하나이고, 둘 다 "체크됨(✓)" 모양이 고정으로 그려져 있습니다. "선택 안 됨(빈 박스)"을 표현하는 variant가 없어 체크/언체크 토글을 구현할 수 없습니다.

**사용 가이드**
- `Item Card / Cart`(§2.8) 마스터에 내장되어 있습니다. `Stock=Available`은 `Status=Default`(파란 체크)를, `Stock=OutOfStock`은 `Status=Disabled`(회색 체크)를 씁니다. 장바구니 화면의 상품 리스트에서는 `Status=Default`로 쓰입니다. `Cart / Selection Toolbar`(§2.6)에는 `Status=Disabled`로 내장되어 있습니다.
- ❌ Don't: 이 글리프만으로는 "선택 해제(빈 박스)" 상태를 표현할 수 없습니다. 체크/언체크를 토글해야 하는 다중 선택 UI에는 아직 쓸 수 없으며 신규 제작이 필요합니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Status | Default(파란 체크) / Disabled(회색 체크) | Default | 둘 다 "체크됨" 모양 고정. Unchecked(빈 박스) variant 없음 |

**Content**
- 텍스트 없음(순수 아이콘)

**신규 착수 필요**

토글 가능한 Radio/Checkbox는 0/2입니다. Checkbox를 새로 만들 때는 기존 체크 아이콘 비주얼을 재사용하면서 Unchecked 상태를 추가하는 방식으로 접근할 수 있습니다.

### 2.16 Text Input

체크리스트 밖에서 추가한 컴포넌트입니다.

레이블, placeholder, 값 입력 영역, 도움말(guide text), 글자수 카운터를 하나로 묶은 텍스트 입력 필드입니다. 로그인, 회원가입, 검색, 쿠폰코드 등 사용자가 직접 텍스트를 입력해야 하는 모든 화면에서 재사용합니다.

**사용 가이드**
- ✅ Do: 아이디, 비밀번호, 검색어, 쿠폰코드 등 자유 텍스트 입력이 필요한 모든 곳에 사용합니다.
- ✅ Do: 값 검증이 필요하면 `Status=Invalid`+`Helper Text`로 에러 문구를 표시합니다.
- ❌ Don't: 옵션 중 하나를 고르는 선택 UI에는 쓰지 않습니다. `Chip`/`Option Chips`(§2.4)를 씁니다.
- ❌ Don't: 검색 결과 목록 자체에는 쓰지 않습니다. 검색창(입력)은 Text Input, 결과 목록은 `List`(§2.10)입니다.
- 트레일링 버튼(`Show Button`)의 활성/비활성은 Text Input 자체 속성이 아닙니다. 중첩된 `Button` 인스턴스를 직접 선택해 그 인스턴스의 `Status`(Default/Pressed/Disabled)를 바꿔서 제어합니다.
- `Show Button`을 지원하는 변형은 `Content=Empty, Status=Default` 하나입니다. 이 마스터의 기본 상태는 버튼이 꺼져 있으며, 인스턴스에서 `Show Button`을 켜야 버튼이 나타납니다. `Status=Default`와 버튼 유무는 서로 무관한 축입니다.
- `TwoLine`의 각 줄 문구를 바꾸는 과정은 2단계입니다. 먼저 `Guide Row 1`/`Guide Row 2`로 톤(Success/Error)을 고릅니다. 그다음 중첩된 `Guide Row` 인스턴스를 직접 선택해 자체 `Text` 속성을 편집합니다.
- `Content=Empty`는 상태(Status)와 무관하게 고를 수 있습니다. `Disabled`/`Default`/`Active` 세 가지 모두 `Empty` 예시가 있습니다. `Empty`를 고른다고 자동으로 비활성처럼 보이지 않습니다.
- `Helper Text=OneLine`의 성공/에러 톤은 `Status`에 종속됩니다(Active→Success, Invalid→Error). 줄이 1개라 "일부만 통과" 개념이 성립하지 않기 때문입니다. 다른 조합이 필요하면 중첩 아이콘 인스턴스를 직접 교체합니다.
- guide text(도움말) 아이콘의 성공/에러 톤도 독립된 속성이 아니라 `Status`에 종속됩니다. `Status=Active`+`Helper Text≠None`이면 항상 `Icon / Status / Success`(파랑 체크), `Status=Invalid`면 항상 `Icon / Status / Error`(빨강 느낌표)입니다. Active 상태에서 에러 톤 안내처럼 다른 조합이 필요하면 중첩된 아이콘 인스턴스를 직접 선택해 `Icon / Status / Error`(또는 `Success`)로 바꿉니다.

**Anatomy**: `label`(라벨+`Show Required Mark` 별표+`Show Counter` 글자수) → `text box`(placeholder/입력값+커서+`Show Button` 트레일링 버튼) → `guide text group`(`Helper Text`, `Icon / Status / Success` 또는 `Icon / Status / Error` 인스턴스+안내문구)

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Content | Empty / Filled | Empty | 필드 안에 무엇이 들어 있는지를 나타내는 콘텐츠 채움 상태(정보 표시용). 실제 입력 여부에 따라 화면에서 자연히 전환. `Status`(인터랙션/검증 상태)와 구분하기 위해 `Type`이 아닌 `Content`라는 이름 사용(§3 참고) |
| Size | Large | Large | 현재 Large만 존재. 필요해지면 Medium/Small 추가 가능 |
| Status | Default / Active / Disabled / Invalid | Default | Active=포커스(파란 테두리+커서), Disabled=회색 배경, Invalid=빨간 테두리(`Feedback/Error`) |
| Helper Text | None / OneLine / TwoLine | None | 값에 따라 컴포넌트 높이가 66→90→112px로 변함(멀티라인 Variant, Boolean 아님). `TwoLine`의 두 줄은 각각 `Guide Row 1`/`Guide Row 2`(INSTANCE_SWAP, `Guide Row` 컴포넌트의 Success·Error 중 선택) 속성으로 톤을 독립적으로 선택. 아이콘과 텍스트 색이 함께 바뀌는 컴포넌트를 통째로 스왑하므로 아이콘·텍스트 색 불일치가 생기지 않음. 비밀번호 입력 규칙처럼 "일부 조건만 통과"하는 상황을 표현하기 위함(예: "8자 이상"은 Success, "특수문자 포함"은 Error). `OneLine`(줄 1개)은 `Status`에 종속(§2.16 사용 가이드 참고) |
| Show Required Mark | Boolean | false | 라벨 옆 빨간 `*` |
| Show Counter | Boolean | false | 라벨 행 우측 "N/30" 글자수 카운터 |
| Input Type | Default / Masking / Number | Default | 입력값의 종류. `Default`=일반 텍스트, `Masking`=입력값을 점(`●`)으로 가리는 비밀번호류 입력, `Number`=숫자 입력. `Number`는 Figma에서 `Default`와 시각적으로 동일. 키보드 타입 등 구현 단계의 차이만 있고 디자인 차이는 없음 |
| Show Button | Boolean | false | 트레일링 CTA형 버튼(예: "인증하기"). OTP/인증번호 입력 패턴 전용. 버튼은 `Button`의 `Type=Primary, Size=Medium` 인스턴스이며 아이콘 없음. 버튼을 끄면 입력창이 328px 전체 폭으로 확장 |

**Content**
- 라벨은 "아이디"/"비밀번호"처럼 2~5자 명사형
- placeholder는 입력 조건을 짧게 안내("영문, 숫자 조합 8자 이상")
- guide text는 성공(파랑 체크)/에러(빨강 느낌표) 톤으로 구분, `Icon / Status` 컴포넌트를 인스턴스로 재사용. 성공 상태는 파랑(`Blue/400`)으로 표시하는 규칙을 따름
- 카운터는 "N/30" 형식(현재값/최대값)
- 모서리는 입력창 `radius/medium`(8), 아이콘·커서 `radius/full`(999)

### 2.17 Text Area

체크리스트 밖에서 추가한 컴포넌트입니다.

`Text Input`(§2.16)과 같이 텍스트를 입력받는 목적이지만 형태가 다릅니다. `Text Input`은 테두리가 있는 짧은 한 줄 필드이고, Text Area는 테두리 없이 화면 폭 전체를 쓰는 멀티라인 입력 영역입니다. 리뷰 본문처럼 여러 문장을 길게 작성하는 용도입니다.

**사용 가이드**
- ✅ Do: 상품 리뷰 본문, 문의 내용처럼 여러 줄의 자유 텍스트를 입력받는 곳에 사용합니다.
- ✅ Do: 한 줄짜리 제목이나 요약 입력에는 이 컴포넌트 대신 `Text Input`을 사용합니다. 리뷰 작성 화면의 "한 줄 요약" 필드가 그 예입니다.
- ❌ Don't: 짧은 값 하나만 받는 곳(아이디, 비밀번호, 검색어 등)에는 쓰지 않습니다. `Text Input`(§2.16)을 씁니다.

**Anatomy**: `placeholder`/`value`(본문 텍스트, 비었을 때는 placeholder 스타일) → (`Status=Active`일 때만) 텍스트 앞 커서 → (`Show Counter`) 우측 정렬 글자수 카운터 → (`Status=Invalid`일 때) 하단에 `Guide Row`(§2.16 참고, Error 톤) 안내 문구

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| Content | Empty / Filled | Empty | `Text Input`과 동일한 개념. 필드 안에 무엇이 들어 있는지 |
| Status | Default / Active / Disabled / Invalid | Default | `Active`는 텍스트 앞에 커서 표시(포커스). `Disabled`는 `Gray/50` 배경 워시로만 표시. 테두리가 없는 컴포넌트라 `Text Input`처럼 테두리·배경을 함께 바꾸지 않고 은은한 배경 틴트 하나로 비활성을 표현. `Invalid`는 하단에 `Guide Row`(Error) 표시 |
| Show Counter | Boolean | false | 우측 정렬 글자수 카운터("N/500" 형식) 표시 여부 |

**Content**
- placeholder는 무엇을 써야 할지 안내하는 완전한 문장 형태("사용해 보신 후 느낀 점을 남겨주세요...")
- 카운터는 "N/500"처럼 현재값/최대값 형식(최대값은 실제 글자 수 제한에 맞게 화면마다 다를 수 있음)
- `Invalid`의 안내 문구는 `Text Input`과 마찬가지로 `Guide Row` 인스턴스를 직접 선택해 편집(§2.16 사용 가이드 참고). Text Area 수준의 속성으로는 문구를 노출하지 않음
- 마스터 컴포넌트의 높이는 콘텐츠에 맞춰 늘어나는 HUG 방식. 화면에 배치할 때는 그 화면에 맞는 고정 높이나 `FILL`로 인스턴스 크기를 조정
- `Show Counter`와 `Invalid`의 `Guide Row` 구조는 `Content=Filled, Status=Active`/`Invalid` 마스터 각각 하나에만 존재(Text Input과 같은 원칙으로, 모든 조합에 레이어를 추가하지 않음)

### 2.18 Seller Row

체크리스트 밖에서 추가한 컴포넌트입니다.

상품 상세페이지에서 판매자 정보를 보여주고, 행을 클릭하면 판매자 페이지로 이동하는 단일 목적의 클릭 가능한 행입니다.

**비교**: 상품 상세페이지에는 판매자정보, 상품문의, 배송정책 리스트 행이 있습니다. 판매자정보는 아바타 이미지를 구조적으로 포함합니다. 아바타가 없는 순수 텍스트 행인 상품문의·배송정책과 anatomy가 달라 별도 컴포넌트로 둡니다. `Cart / Address Row`(§2.6, "이름(행정구역)"+"변경 〉")와는 "행 클릭 → 이동"이라는 목적이 비슷합니다. 다만 `Cart / Address Row`는 장바구니 화면 전용 단일 목적 컴포넌트라 이름에 `Cart /` 접두사가 붙습니다. `Seller Row`는 상품 상세페이지 전반에서 쓰는 범용 컴포넌트라 접두사 없이 단독 이름을 씁니다.

**사용 가이드**
- ✅ Do: 상품 상세페이지에서 이 상품을 파는 판매자(브랜드) 정보를 요약해서 보여주고 판매자 페이지로 연결할 때 사용합니다.
- ❌ Don't: 판매자가 아닌 일반 안내/설정 행(상품문의, 배송정책 등)에는 쓰지 않습니다. 아바타 이미지가 구조적으로 포함되어 있어 아바타가 없는 행에는 맞지 않습니다. 이런 행에 대응하는 컴포넌트는 아직 없습니다(§5 참고).

**Anatomy**: 원형 아바타(36×36, 판매자 로고/썸네일) → 판매자명 텍스트 + `Icon/Chevron Right` 인스턴스(같은 줄, 클릭 유도) → 그 아래 브랜드 태그 텍스트(회색 톤, 보조 정보). 레이어 이름은 `avatar`/`info`/`seller name`/`brand tag`입니다.

**Variants**

| Property | Values | Default | 비고 |
|---|---|---|---|
| — | Variant 없는 단일 컴포넌트 | — | 현재 사례는 1곳. 아바타 없음 등 다른 상태는 필요해지면 검토 |

**Content**
- 예시는 "도브"(판매자명) + "브랜드샵"(브랜드 태그) 1건. (미정) 텍스트 길이·톤 규칙
- 클릭 시 판매자 페이지로 이동하는 인터랙션은 컴포넌트(정적 UI)에 표현되지 않음. 다른 컴포넌트와 마찬가지로 실제 동작은 코드 구현 단계에서 처리

**재사용 시 주의**: 이름 텍스트 노드는 `textAutoResize: NONE`+`layoutSizingHorizontal: FIXED`(고정 폭 25px)입니다. 더 긴 텍스트로 바꾸면 폭이 늘어나지 않아 텍스트가 줄바꿈되고 아래 브랜드 태그 줄과 겹칩니다. 이름을 바꿀 때는 그 텍스트 노드를 `textAutoResize: 'WIDTH_AND_HEIGHT'`+`layoutSizingHorizontal: 'HUG'`로 먼저 전환합니다.

---

## 3. Naming Convention

새 컴포넌트를 만들 때 이 규칙을 그대로 따릅니다.

- **컴포넌트 이름**: Title Case + 공백(예: `Section Header`, `Action Bar`, `Button`). snake_case, kebab-case, 소문자 단일 단어는 쓰지 않습니다.
- **패밀리(변형이 여러 세트로 나뉘는 경우)**: `이름 / 하위이름` 형식, 슬래시 앞뒤 공백 포함(예: `Option Chips / Size`, `Icon / Checkbox`).
- **Property 이름**:
  - 상태·선택 여부를 나타내는 축은 `Status`(Title Case) 하나로 통일합니다. Default/Selected/Pressed/Error/Warning 등 성격이 달라도 모두 이 키를 씁니다.
  - 콘텐츠·글리프 종류를 나타내는 축(상태가 아닌 것)은 `Type`을 씁니다. 다만 "필드에 콘텐츠가 얼마나 들어찼는지"처럼 값 자체가 상태처럼 읽히는 콘텐츠 축은 `Type`이 `Status`와 개념적으로 겹쳐 보일 수 있습니다. 이런 경우 `Type` 대신 `Content`처럼 더 구체적인 이름을 씁니다. 예를 들어 `Text Input`은 `Content`=Empty/Filled와 `Status`=Default/Active/Disabled/Invalid를 구분합니다.
  - 크기 축은 `Size`를 씁니다.
  - Boolean 축은 `Show {명사}` 접두사로 통일합니다. 예: `Show Icons`(Bottom Sheet), `Show Scroll`(Dialog/Bottom Sheet), `Show Unit Price`(Price Block), `Show Status Badge`(Review/Card), `Show Row 1`~`Show Row 4`(Review/Summary), `Show Leading Icon`/`Show Trailing Icon`(Button 계열). 반례는 없습니다.
  - Figma 기본값(`Property 1`, `Variant2` 등)을 그대로 남기지 않습니다. 반드시 의미 있는 이름으로 교체합니다.
- **Variant vs Boolean 선택 기준**: 값이 2개이고 차이가 "요소 하나의 visible on/off"뿐이면 Boolean(`Show {명사}`)을 씁니다. 값이 3개 이상이거나, 값이 바뀔 때 색상·아이콘·텍스트·레이아웃 등 여러 속성이 함께 바뀌면 Variant를 씁니다(예: Dialog `Status=Error/Warning`, Review/Card `Type=Photo/OnlyText`, Button `Size`, Dialog `Button=1 Button/2 Button/X`). 알려진 제약이 하나 있습니다. Variant를 Boolean으로 전환하면서 원래 축에 값이 1개만 남는 경우(Section Header `State`, Review/Card `Status`), Figma가 COMPONENT_SET의 마지막 속성 축 삭제를 허용하지 않아 빈 축이 남습니다. 이는 설계 원칙 위반이 아니라 도구의 제약입니다.
- **Variant 값**: 영문 식별자를 사용합니다. 한글 서술형 값은 쓰지 않습니다(예: `한달사용` ❌ → `OneMonthUse` ⭕). 화면에 표시되는 텍스트는 별도 텍스트 레이어로 분리합니다.
- **컴포넌트를 새로 만들거나 옮길 때**: 반드시 Instance로 참조합니다. 컴포넌트 원본(COMPONENT/COMPONENT_SET)을 다른 페이지에 복제하지 않습니다.
- **이름이 같거나 비슷해 보여도** 병합·삭제 전에 반드시 스크린샷이나 실제 콘텐츠 조회로 진짜 중복인지 확인합니다. 이름만 보고 판단하지 않습니다.
- **새 Property와 컴포넌트 이름은 추가 전에 철자를 확인합니다.** `Bottom Sheet / Informational`의 `Discription`(정상 철자 Description)은 오탈자가 Figma 프로퍼티명으로 굳어진 사례입니다(§2.12 참고).

**Foundation 토큰 이름 — 관찰된 현재 상태(통일된 규칙 아님)**

위 규칙은 컴포넌트와 프로퍼티 이름에 관한 것입니다. Foundation(§1) 토큰 이름은 카테고리마다 서로 다른 표기 관례를 씁니다. 하나의 규칙으로 묶지 않고 현재 상태 그대로 기록합니다.

| 카테고리 | 실제 패턴 | 예시 |
|---|---|---|
| Color · Semantic | Title Case + 슬래시(`카테고리/역할`) | `Background/Divider`, `Text/Primary`, `State/Primary Pressed` |
| Typography | 소문자 + 슬래시 + 하이픈(`카테고리/역할-수식어`) | `heading/xlarge`, `body/large-bold`, `label/xsmall-medium` |
| Radius | 소문자 단일 단어(계층 없음) | `none`, `small`, `medium`, `large`, `full` |
| Spacing | 숫자 그대로(이름 레이어 없음) | `4`, `8`, `16`, `20` |

**결정**: Semantic Color(Title Case)와 Typography(소문자)는 같은 토큰이지만 대소문자 표기 관례가 다릅니다. 강제로 통일하지 않고 카테고리별 현행 관례를 그대로 유지합니다.

---

## 4. Design Principles

디자인 원칙은 아래 11개입니다.

1. **강조는 배경색 채움을 최상위 수단으로 아껴 씁니다.** 화면 전체에서 배경이 채워지는 요소는 CTA와 할인율 배지뿐입니다.
2. **확정 금액=Red(빨강)/Gray(검정), 위험/할인=빨강으로 색의 의미가 고정되어 있습니다.** 파랑(Blue)은 CTA·링크 등 "행동" 전용이고, 가격 강조는 Red/Gray가 맡습니다.
3. **인터랙션 가능한 작은 요소일수록 더 둥급니다.** 순서는 `full`(Chip/Radio/Icon Button 등), `medium`(Button Large/Medium, 8), `small`(Button Small/XSmall, 4), `none`(Card/Action Bar, 0)입니다. 모달형 오버레이(Dialog, Bottom Sheet)의 상단 모서리는 별도로 `large`(16)입니다.
4. **깊이는 원칙적으로 그림자가 아니라 선으로 표현합니다.** 다만 `Toast`(blur)와 `Bottom Navigation`(shadow)에는 예외가 있습니다. 화면 위에 뜨는 고정 오버레이 성격의 컴포넌트에 한정된 예외로 잠정 분류합니다.
5. **커머스 숫자는 크기보다 weight로 위계를 나눕니다.**
6. **여백은 16(화면 마진)/8(카드 내부) 두 값이 대부분을 지배하고, 버튼·칩류만 예외를 허용합니다.**
7. **정보 밀도가 높아지면 리스트+구분선 또는 가로 캐러셀로 낮춥니다.** 새 레이아웃 패턴을 만들지 않습니다.
8. **아이콘은 역할에 따라 outline(컨트롤)과 filled(정보성 배지)로 나뉩니다.**
9. **모달형 오버레이의 높이는 뷰포트 상대 단위로 제한하고, 스크롤 인디케이터는 콘텐츠 형태에 따라 선택적으로 붙입니다.** 높이 상한은 고정 px가 아니라 `vh`로 계산합니다(Dialog `70vh`, Bottom Sheet `80vh`). 구분선+스크롤바로 구성된 스크롤 인디케이터(`Show Scroll` Boolean)는 길이를 예측할 수 없는 자유 텍스트 콘텐츠(Dialog, `Bottom Sheet / Informational`)에만 붙입니다. 리스트·캐러셀처럼 형태 자체가 스크롤 가능함을 암시하는 콘텐츠(`Bottom Sheet / Interactive`, `Item Card / Recommendation` 등의 캐러셀)에는 붙이지 않습니다.
10. **탭형 선택 UI는 역할에 따라 컴포넌트가 분리됩니다.** `Tab Group`은 같은 데이터를 다른 관점으로 재구성해 보여줄 때 씁니다. 장바구니의 일반구매/자주산상품/찜한상품처럼 사용자 개인의 구매·관심 이력을 전환하는 경우가 그 예입니다. `Category Tab`/`Chip`은 서로 다른 카테고리·옵션 자체를 나열해 고를 때 씁니다. 겉보기에는 둘 다 "여러 개 중 하나를 고르는 가로 UI"라 혼동하기 쉽습니다. "같은 데이터를 다른 렌즈로 보는가"와 "서로 다른 대상을 고르는가"로 구분합니다.
11. **자유 텍스트가 들어가는 슬롯은 넘침 처리를 반드시 정합니다.** 한 줄로 보여줄 슬롯(제목·라벨·메뉴명 등)이 자유 텍스트(사용자 입력·카탈로그 데이터)라면 기본값은 말줄임입니다(`textTruncation: ENDING`, 예: `List`의 `history` 타입, `Option Chips / Thumbnail`). 여러 줄을 허용할 슬롯(설명·본문)은 자동 줄바꿈을 기본으로 하고, 가능하면 최대 줄 수까지 정해 그 이상은 라인클램프합니다. 디자인 시스템이 직접 통제하는 고정 짧은 문구(Label/Badge류)는 대상이 아닙니다. 여러 인스턴스가 나란히 놓이는 카드형 컴포넌트에서, 줄바꿈 가능한 텍스트가 들어가는 슬롯은 실제 글자 수와 무관하게 항상 같은 면적을 차지해야 합니다. 아래 두 조건을 함께 충족해야 합니다(예: `Item Card / Recommendation` 상품명, §2.8).
    - 텍스트 박스의 세로 크기는 줄 수에 따라 변하는 `HUG`가 아니라, 예상 최대 줄 수(예: 2줄) 기준의 `FIXED` 높이로 고정합니다. `HUG`로 두면 텍스트가 짧을수록 박스가 작아지고, 그 아래의 가격·배송 정보가 위로 당겨져 옆 카드와 시작 높이가 달라집니다.
    - 그 고정 박스 안에서 텍스트의 `textAlignVertical`은 `CENTER`가 아니라 `TOP`으로 앵커링합니다. `CENTER`로 두면 박스 높이는 같아도 짧은 텍스트가 박스 중앙에 떠서 시작 줄이 아래로 내려갑니다.

---

## 5. 남은 과제

남은 과제와 알려진 결함은 [status.md](./status.md)의 '남은 과제'에서 관리합니다.

---

## 6. Screen Composition Patterns

실제 화면 5개에서 도출한 화면 구성 패턴입니다. 하나로 통일되지는 않고 화면 성격에 따라 3가지로 나뉩니다.

**공통 규칙**
- 모든 화면은 `Status Bar`(44px 고정)로 시작합니다.
- 화면 최하단에는 `Action Bar`(구매 CTA류, 상세페이지·장바구니) 또는 `Bottom Navigation`(탭 루트 화면, Category)이 스크롤 본문과 분리된 고정 요소로 붙습니다.
- 이미지는 항상 풀블리드(화면 폭 100%, 마진 없음)이고, 텍스트·컨트롤 콘텐츠는 16px 마진을 씁니다(§1.3 Spacing 원칙과 일치).
- 콘텐츠 블록 사이의 순수 여백(`spacing`, 구분선이 아닌 빈 간격)은 `Color/Primitive/Static/0`(흰색)을 씁니다. `Background/Background`(Gray/50)를 쓰면 옅은 회색이라 바로 옆 8px 구분선(`Gray/100`)과 명도차가 작아 여백과 구분선이 섞여 보입니다. 여백은 항상 흰색, 구분선만 회색이어야 두 요소가 명확히 구분됩니다.

**패턴 1 — 목록형 화면**(상품목록_그리드 타입, 상품목록_리스트 타입, 상품 검색 결과)

`App Bar` 2개 + `App Bar Small`(필터) 헤더 뭉치 → `divider`(1px) → 본문 순서입니다. 그리드는 2열(`Item Card / Grid`), 리스트는 1열(`Item Card / List`)입니다. 헤더 구조는 완전히 같고 본문 컴포넌트만 바뀝니다. 행과 아이템 사이는 모두 1px 구분선입니다. 2열 그리드도 예외 없이 행 사이에 328px 인셋 구분선을 둡니다.
- App Bar 2개의 Type 조합: 카테고리 브라우징 화면은 `Type=Category`+`Type=Filter`, 검색 결과 화면은 `Type=Search`+`Type=Filter`입니다. 첫 번째 App Bar만 화면 성격에 따라 바뀝니다. 헤더 구조(두 번째 App Bar=Filter, 그 아래 App Bar Small, 구분선 위치)는 같습니다.
- App Bar Small의 `Type`이 가리키는 것은 "현재 보기"가 아니라 "전환 가능한 다른 보기"입니다. 그리드 화면의 인스턴스는 `Type=List`를 씁니다. 그리드로 보고 있으므로 "리스트로 바꾸기" 아이콘을 보여주는 것입니다.

**패턴 2 — 스크롤형 화면**(상품 상세페이지, 장바구니)

헤더 바로 뒤에 구분선 없이 첫 콘텐츠 블록이 시작합니다(목록형과 다른 점). `spacing → 콘텐츠 블록 → spacing → divider(8px) → spacing → 다음 블록` 리듬이 반복됩니다. 1px 구분선은 같은 섹션 안의 아이템 구분, 8px 구분선은 서로 다른 섹션(상품정보/결제정보/추천상품/리뷰) 구분입니다. 두께 자체가 위계를 나타냅니다. 가로 캐러셀(추천상품)은 항상 `Section Header` 바로 아래에 간격 없이 붙습니다.
- 구분선 폭: 헤더→콘텐츠 경계처럼 화면을 가로지르는 구분선은 360px 풀블리드입니다. 같은 섹션 안에서 아이템끼리 구분하는 1px 구분선은 328px(화면폭−마진 32)로 좌우 16px 인셋을 유지합니다. 아이템 사이는 `아이템 → 16px → divider → 16px → 아이템` 리듬입니다. 구분선을 아이템에 바로 붙이지 않습니다.
- 구분선 색상은 두께에 따라 다른 Primitive를 직접 참조합니다. 어느 쪽도 `Color/Semantic/Background/Divider` 토큰을 거치지 않고 Primitive를 직접 바인딩합니다.
  - 1px 구분선(아이템 구분, 헤더 경계, 컴포넌트 내부 구분선 모두 포함) → `Color/Primitive/Gray/200`
  - 8px 구분선(섹션 간 구분) → `Color/Primitive/Gray/100`
  - `Background/Divider` Semantic 토큰(→`Gray/200`)은 Dialog·Bottom Sheet의 스크롤 UI 구분선처럼 컴포넌트 내부에서만 씁니다. 화면 레벨의 구분선은 Semantic을 거치지 않습니다. 두 사용처의 바인딩 경로가 다른 것은 예외가 아니라 설계입니다.

**패턴 3 — 사이드바+콘텐츠 화면**(Category)

상단 `Section Header + Quick Badge Row` → 구분선 → 좌측 `Category Tab` 세로 목록 + 우측 콘텐츠 그리드의 좌우 분할 구조입니다.

---

## 변경 이력

| 버전 | 날짜 | 요약 |
|---|---|---|
| v0.1–v0.14 | 2026-09-04 | 최초 작성, Foundation 실측값 반영, 컴포넌트 15개 명세 |
| v0.15–v0.33 | 2026-09-05 | 컴포넌트 문서 형식 정비, Message Box 추가, 사용 원칙·화면 구성 패턴 정의, 첫 화면 검증 |
| v0.34–v0.46 | 2026-09-06 | Text Input, Text Area 추가, Warning 색상 조정 |
| v0.47–v0.53 | 2026-09-08~09 | 구분선 색상 규칙, Item Card 색상 역할 정리, 가격·긴급·배송·혜택 토큰 추가 |
| v0.54–v0.57 | 2026-09-10 | Primitive 색상 이름 변경, 전체 색상 바인딩 감사 |
| v0.58–v0.68 | 2026-09-30 | Seller Row 추가, 검색 결과·앨범 상세 화면 검증, 기존 화면 보완 |
| v0.69–v0.71 | 2026-10-02~03 | 주간 캘린더, 스트리밍 홈, 주문 내역 화면 검증 |
| v0.72 | 2026-10-06 | 작업 이력 정리, 표기·문체 통일 |
