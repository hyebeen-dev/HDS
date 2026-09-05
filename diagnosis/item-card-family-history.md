# Item Card Family — 작업 이력

> 이 문서는 `diagnosis/03`, `04`, `05`, `06`, `24`(구 파일명)를 시간순으로 합친 이력 기록이다. 원본 내용은 그대로 보존했다.
> **지금 유효한 최신 상태는 `design-system/status.md`를 참고.**

---

## 2026-09-01 — Item Card: Price Block / Rating Display / Spec Row 분리 (구 diagnosis/03)

# Figma 작업 완료 기록 — Item Card: Price Block / Rating Display / Spec Row 분리

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`
**관련 진단** `01-figma-design-system-diagnosis-v0.1.md`

---

## 실행 내용

### 1. Price Block 컴포넌트 신규 생성 — 완료, 24개 카드 전체 적용
grid_small(8) · grid(8) · list(8) 전체 24개 Item Card Variant의 raw "price" 프레임을 실제 프로덕션 레이어를 그대로 clone → `figma.createComponentFromNode`로 변환해 만든 **Price Block** 컴포넌트 인스턴스로 교체했다. `Show Discount`(Boolean) 컴포넌트 속성으로 정가·할인율 배지의 표시 여부를 제어하며, 값을 하나도 새로 만들지 않고 기존 레이어를 그대로 재사용했다.

- 카드별 `Sale=Y/N` 값에 따라 `Show Discount`를 true/false로 설정
- 재바인딩 후 전 카드 재조회 결과: **Price Block 인스턴스 24개, 잔여 raw price 프레임 0개**
- 스크린샷으로 재바인딩 전후 시각적 차이 없음을 확인

### 2. Rating Display 컴포넌트 신규 생성 — 완료 (컴포넌트만, 카드 적용은 보류)
`Review=Filled`(4.83 / (9,999+)) / `Review=Empty`(- / (0)) 2-Variant 컴포넌트 세트로 생성. 두 상태 모두 기존에 실제로 그려져 있던 텍스트 레이어를 그대로 clone해서 만들었다(새 텍스트를 타이핑하지 않음). `Filled` variant는 추가로 `Rating`/`Review Count` TEXT 속성을 노출해, Figma 데스크톱 앱처럼 폰트가 설치된 환경에서 임의의 평점 값을 넣을 수 있게 해뒀다.

### 3. Spec Row 컴포넌트 신규 생성 — 완료 (컴포넌트만, 카드 적용은 보류)
`Type=Rocket/Free/Today/None` 4-Variant + `Show ETA`/`ETA Text`/`Show Reward`/`Reward Text` 속성으로 생성. 실행 중 발견한 것: 기존 카드들은 "무료배송"을 카드마다 다른 스타일(알약 배지 vs 일반 텍스트)로 표현하고 있었음 — 사용자 확인 후 **알약 배지 스타일로 통일**했다. "오늘출발"도 같은 배지 스타일로 새로 만들되, 텍스트 자체는 기존에 실제로 그려져 있던 "오늘출발" 텍스트 레이어를 그대로 clone해서 재사용했다(새로 타이핑하지 않음).

## 이번에 다루지 못한 것 — 실행 환경 한계

**이 세션의 Figma 실행 환경(`use_figma`)에 `Pretendard` 폰트가 설치되어 있지 않다.** 이 파일의 모든 텍스트가 Pretendard를 쓰기 때문에, **텍스트 내용을 새로 입력하거나 기존 텍스트 값을 변경하는 조작은 이 세션에서 전부 불가능**했다(`figma.loadFontAsync`가 "Pretendard 폰트가 존재하지 않는다"는 에러로 실패). 기존에 이미 그려진 텍스트 레이어를 통째로 clone하는 것은 가능했지만(Type/Review Variant를 이렇게 만들었다), 카드마다 다른 배송 예정 문구("모레(수) 도착 예정" vs "내일(화) 도착 보장" 등)를 인스턴스별로 정확히 재현하려면 텍스트 속성값(`setProperties`)을 바꿔야 하는데, 이게 막혀 있었다.

이 때문에:
- **Rating Display, Spec Row 컴포넌트는 만들었지만, 24개 카드에는 아직 적용하지 않았다.** 사용자 확인 후 이번 범위를 Price Block으로 한정했다.
- 실제 카드 적용은 **Pretendard 폰트가 설치된 환경(Figma 데스크톱/웹 앱)에서 진행해야 한다.** 각 카드가 어떤 Type/Review/ETA Text/Reward Text 값을 가져야 하는지는 이번 조사 과정에서 24개 카드 전부 파악해뒀다 (아래 표).

### 카드별 Spec Row/Rating Display 적용값 참고표

| 카드 | Review | Spec Row Type | Show ETA | ETA Text | Show Reward |
|---|---|---|---|---|---|
| grid_small sale=Y,express=Y,review=Y | Filled | Rocket | false | — | false |
| grid_small sale=Y,express=Y,review=N | Empty | Rocket | false | — | false |
| grid_small sale=Y,express=N,review=Y | Filled | None | false | — | false |
| grid_small sale=Y,express=N,review=N | Empty | None | false | — | false |
| grid_small sale=N,express=Y,review=Y | Filled | Rocket | false | — | false |
| grid_small sale=N,express=Y,review=N | Empty | Rocket | false | — | false |
| grid_small sale=N,express=N,review=Y | Filled | None | true | 모레(수) 도착 예정 | false |
| grid_small sale=N,express=N,review=N | Empty | None | true | 모레(수) 도착 예정 | false |
| grid sale=Y,free=Y,review=Y (×2 중복) | Filled | Free | true | 모레(수) 도착 예정 | false |
| grid sale=Y,free=Y,review=N | Empty | Free | true | 모레(수) 도착 예정 | false |
| grid sale=Y,free=N,review=N | Empty | None | true | 모레(수) 도착 예정 | false (+로켓배지는 별도 확인 필요) |
| grid sale=N,free=N,review=Y (×2 중복) | Filled | Free | true | 모레(수) 도착 예정 | false |
| grid sale=N,free=N,review=N | Empty | Free | true | 모레(수) 도착 예정 | false |
| list sale=Y,free=Y,badge=Y,review=Y | Filled | Rocket | true | 내일(화) 도착 보장 | true |
| list sale=Y,free=Y,badge=Y,review=N | Empty | Rocket | true | 내일(화) 도착 보장 | true |
| list sale=Y,free=Y,badge=N,review=Y | Filled | Free+Today 조합* | — | — | true |
| list sale=Y,free=Y,badge=N,review=N | Empty | Free+Today 조합* | — | — | true |
| list sale=Y,free=N,badge=Y,review=Y | Filled | None | false | — | true |
| list sale=N,free=N,badge=N,review=Y | Filled | Today | false | — | false |
| list sale=N,free=N,badge=Y,review=Y | Filled | Rocket | true | 내일(화) 도착 보장 | true |
| list sale=N,free=N,badge=Y,review=N | Empty | Rocket | true | 내일(화) 도착 보장 | true |

`*` 표시된 2건(`list badge=N, free=Y`)은 원본이 "무료배송・오늘출발"을 한 줄에 같이 쓰고 있어 Spec Row 하나로 그대로 표현이 안 된다 — 실제 적용 시 판단이 필요.

**grid density는 원본 파일 자체에 pre-existing 결함이 있었다**: `sale=Y,free=Y,review=Y`와 `sale=N,free=N,review=Y`는 각각 노드가 2개씩 중복 존재하고, `sale=N,free=Y` 조합은 아예 없다(8개 중 6개 조합만 실존). 이번 작업에서 이 결함은 건드리지 않았다 — Variant 매트릭스 정리는 별도 작업으로 남겨둔다.

## 검증 결과

- Price Block 인스턴스 24개 확인, 잔여 raw price 프레임 0개
- 3개 밀도(grid_small/grid/list) 전체 스크린샷 — 교체 전후 시각적 차이 없음
- Cart Item은 계획대로 손대지 않음

## 다음 단계 후보

1. Pretendard 폰트가 설치된 환경에서 위 참고표대로 Rating Display/Spec Row를 24개 카드에 적용
2. grid density의 중복/누락 Variant 조합 정리
3. Tier 1 미착수 컴포넌트(Section Header, App Bar Sticky, Action Bar) 착수

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Price Block/Rating Display/Spec Row 컴포넌트 신규 생성. Price Block은 24개 카드 전체 적용 완료. Rating Display/Spec Row는 폰트 제약으로 컴포넌트 생성까지만 완료 |

---

## 2026-09-01 — Item Card를 Boolean/Text 속성 기반 단일 컴포넌트로 통합 (구 diagnosis/04)

# Figma 작업 완료 기록 — Item Card를 Boolean/Text 속성 기반 단일 컴포넌트로 통합

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`

---

## 배경

사용자가 기존에 "할인 여부·무료배송 여부·리뷰 여부"별로 상황을 나눠 만든 Item Card 8종(밀도당) 작업을, 재작업 대상으로 지정했다. 재확인 결과 grid_small·grid·list 각각의 8개는 진짜 Figma Variant Set(COMPONENT_SET)이었지만, 속성이 전부 "Y/N" 문자열이 name에 박힌 형태였고 Boolean/Text 같은 실제 컴포넌트 속성 시스템은 쓰이지 않고 있었다. 이번 작업은 기존 레이어·콘텐츠는 그대로 재사용(clone)하면서, 8개 나열 구조를 Price Block/Rating Display/Spec Row 인스턴스를 품은 **단일 컴포넌트 + Boolean/Text/Variant 속성** 구조로 바꾼다.

## 실행 내용

### 1. Item Card / Grid Small, / Grid, / List — 신규 단일 컴포넌트 3개
밀도별로 가장 요소가 풍부한 기존 카드를 clone해서 기준으로 삼고, raw price/review/spec 콘텐츠를 이미 만들어둔 Price Block · Rating Display · Spec Row 인스턴스로 교체했다.

### 2. 속성 노출 — `isExposedInstance`
Price Block/Rating Display/Spec Row 세 인스턴스에 `isExposedInstance = true`를 설정했다. 이는 Figma의 "Expose properties from nested instance" 기능과 동일한 메커니즘으로, **Item Card 인스턴스를 선택하면 속성 패널에 세 하위 컴포넌트의 속성이 그대로 노출**된다 (`Show Discount`, `Spec Row Type/Show ETA/ETA Text/Show Reward/Reward Text`, `Rating Display Review/Rating/Review Count`). 이 방식은 부모 레벨에 새 property를 추가하고 값을 참조로 연결하는 방식(`componentPropertyReferences`)과는 다른 메커니즘이며, 실제로 `componentPropertyReferences`를 이용한 중첩 속성 바인딩은 API에서 지원하지 않는다는 것을 실험으로 확인했다(`'visible' | 'characters' | 'mainComponent'` 세 가지 노드 필드만 바인딩 가능, 임의의 하위 컴포넌트 속성 별칭 연결은 불가).

**검증**: 테스트 인스턴스에서 Price Block의 `Show Discount`, Rating Display의 `Review`, Spec Row의 `Type`을 실제로 토글해 렌더링에 정확히 반영되는 것을 스크린샷으로 확인했다.

### 3. 기존 24개 개별 카드 컴포넌트 삭제
grid_small(8) · grid(8) · list(8) = 24개를 전부 삭제했다. 삭제 과정에서 빈 COMPONENT_SET wrapper(`Item_grid`/`Item_list`)가 남는 것을 확인해 함께 제거했고, 대신 각 밀도의 상위 프레임에 새 컴포넌트의 인스턴스 2개씩(기본 상태 / `Show Discount=false` 상태)을 배치해 사용 예시로 남겼다. Cart 변형(4개)은 이번 범위가 아니므로 손대지 않았다.

## 알아둘 점 — 원본과 달라진 부분

**list 밀도의 정보 순서가 살짝 바뀌었다.** 원본은 "배송 배지+도착일 → 리뷰 → 적립" 순서였는데, Spec Row 컴포넌트가 적립 행을 배송 배지+도착일과 하나의 단위로 묶어서 갖고 있어 실제로는 "배송 배지+도착일+적립 → 리뷰" 순서가 됐다. 내용은 전부 동일하고 값도 바뀌지 않았지만, **정보가 노출되는 상대적 위치가 바뀐 것**이라 별도로 알려드린다. (오히려 "구매 조건"(배송+적립)과 "평가"(리뷰)를 각각 하나의 그룹으로 묶는 방향이라 PRD의 정보 그룹핑 원칙(UR-G2)과는 더 가깝다고 판단했지만, 최종 판단은 사용자 몫이라 별도로 되돌리고 싶다면 알려달라.)

## 검증 결과

- 새 컴포넌트 3개(Item Card / Grid Small, / Grid, / List) 스크린샷 — 원본과 시각적으로 동일
- 속성 토글 테스트(Show Discount off / Review Empty / Spec Row Type None) 전부 정상 반영 확인
- 기존 24개 개별 컴포넌트 및 빈 wrapper 3개 삭제 완료, 페이지 전체 스크린샷으로 레이아웃 이상 없음 확인
- Cart 영역 미변경 확인

## 다음 단계 후보

1. Pretendard 폰트가 설치된 환경에서 각 카드가 실제로 어떤 ETA/Reward 문구를 가져야 하는지(이전 기록 `03-item-card-component-extraction-v0.1.md`의 참고표 활용) 세부 인스턴스를 채워 넣기
2. Tier 1 미착수 컴포넌트(Section Header, App Bar Sticky, Action Bar) 착수

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Item Card 8-variant×3밀도(24개) → 단일 컴포넌트 3개(Boolean/Text/Variant 속성 기반)로 통합 완료 |

---

## 2026-09-01 — Price Block 가격 색상 버그 수정 (구 diagnosis/05)

# Figma 버그 수정 기록 — Price Block 가격 색상

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)

---

## 문제

Price Block의 판매가·단위가 텍스트가 `Show Discount` 값과 무관하게 항상 `Color/Primitive/Error/400`으로 표시되고 있었다. 올바른 동작은: discount 적용 시 Error/400, 미적용 시 `Color/Primitive/Gray/900`.

## 원인

Price Block이 단일 컴포넌트 + `Show Discount`(Boolean) 속성으로 만들어져 있었는데, Figma의 Boolean 컴포넌트 속성은 `componentPropertyReferences`를 통해 레이어의 `visible`만 제어할 수 있고 **fill(색상)은 바인딩할 수 없다**(Plugin API 확인: 유효한 키는 `visible` | `characters` | `mainComponent` 셋뿐). 그래서 정가·할인 배지의 표시/숨김은 정상 동작했지만, 판매가·단위가 색상은 애초에 조건부로 만들 수 없는 구조였다.

## 수정

Price Block을 `Discount=Applied` / `Discount=None` 2-Variant 컴포넌트 세트로 재구성했다 (Spec Row/Rating Display와 동일한 패턴).

- `Discount=Applied`: 정가(취소선)+할인율 배지 표시, 판매가·단위가 = Error/400 (`#e23636`)
- `Discount=None`: 정가·배지 없음, 판매가·단위가 = Gray/900 (`#34373d`)

기존 노드 ID(`6036:452`)를 `Discount=Applied` variant로 그대로 재사용했기 때문에, 이미 Price Block을 품고 있던 Item Card / Grid Small·Grid·List 3개 마스터는 별도 수정 없이 계속 올바르게 동작한다. 이전에 "할인 없음" 상태로 토글해뒀던 예시 인스턴스 3개(밀도별 두 번째 카드)는 새 `Discount=None` variant를 쓰도록 재설정했다.

## 검증

- Price Block 컴포넌트 세트 스크린샷 — Applied는 빨강, None은 회색으로 정확히 구분됨
- Item Card 전체 페이지 스크린샷 — 밀도별 두 번째 카드(할인 없음 예시)가 회색 가격으로 정상 표시됨
- Cart 영역 등 나머지는 변경 없음

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Price Block의 Boolean 속성을 Variant로 전환해 discount 여부에 따른 가격 텍스트 색상 버그 수정 |

---

## 2026-09-01 — Cart Item을 Item Card와 같은 방식으로 정리 (구 diagnosis/06)

# Figma 작업 완료 기록 — Cart Item을 Item Card와 같은 방식으로 정리

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`

---

## 실행 내용

### 1. 신규 재사용 컴포넌트 3개
- **Order Deadline**: 시계 아이콘 + `Deadline Text`(TEXT 속성, 기본값 "00:42:25 내 주문 시")
- **Quantity Stepper**: `-`/숫자/`+` 스테퍼, `Quantity`(TEXT 속성)
- **Info Label**: `Label=MonthlyPurchase`("한달구매 2만+") / `DeliveryDate`("9/3(목) 도착예정") / `SoldOut`("일시품절") 3-Variant. 처음엔 TEXT 속성 하나로 설계했지만, 이 실행 환경에 Pretendard 폰트가 없어 인스턴스별 텍스트 override가 불가능해서 — Spec Row/Rating Display와 같은 방식으로 실제 존재하는 3개 문구를 그대로 clone한 Variant Set으로 재구성했다.

### 2. Cart Item — `Stock=Available` / `Stock=OutOfStock` 2-Variant
기존 4개(이름 미정리·중복 상태였던 "Property 1=Default/Variant3/Variant4/Variant3")를 조사한 결과, 실질적으로 재고 있음/품절 두 구조였다. 재고 있음 상태는 Info Label + Spec Row(Type=Rocket) + Order Deadline + Price Block(Discount=Applied) + Quantity Stepper로, 품절 상태는 Info Label(SoldOut) + Price Block(Discount=None) + 기존 Button 컴포넌트(재입고 알림 신청)로 구성했다. 각 인스턴스에 `isExposedInstance = true`를 설정해 Cart Item 인스턴스 하나에서 모든 하위 속성을 바로 조작할 수 있다.

### 3. 기존 4개 컴포넌트 삭제
`item`이라는 이름의 진짜 Figma COMPONENT_SET(4개 variant 보유)이었음을 확인하고 통째로 삭제. "Item: cart" 섹션에 새 컴포넌트 예시 인스턴스 3개 배치: **Available+할인**, **Available+할인없음**, **OutOfStock**.

## 검증

- 신규 컴포넌트 3개 + Cart Item 최종 스크린샷 — 원본과 시각적으로 동일
- `Stock` variant 전환, 중첩 Price Block의 `Discount` 속성 토글 — 실제 인스턴스로 테스트해 정상 반영 확인
- Item Card 전체 페이지 스크린샷 — grid_small/grid/list/cart 전체 레이아웃 이상 없음

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Cart Item 4-variant → Order Deadline/Quantity Stepper/Info Label 신규 컴포넌트 + Cart Item(Stock=Available/OutOfStock) 단일 컴포넌트로 통합 완료 |

---

## 2026-09-02 — Item Card / Grid Small에 Button Type 추가 (구 diagnosis/24)

# Figma 작업 완료 기록 — Item Card / Grid Small에 Button Type 추가

**문서 버전** v0.1
**작업일** 2026-09-02
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**대상 컴포넌트** Item Card / Grid Small (`6253:1706`)

---

## 배경

사용자가 Item Card / Grid Small에 하단 버튼이 추가된 새 type을 요청. 사용할 버튼(흰 배경/파란 500 테두리)은 사용자가 직접 `button_medium` 세트에 미리 추가해뒀다.

## 실행 중 발견·수정한 문제

`button_medium` 세트(`6192:2344`)에 사용자가 새로 추가한 variant(`6249:6238`, 흰 배경+파란 테두리)가 기존 variant와 이름이 완전히 동일(`Property 1=Variant3`)해 세트 전체가 `componentPropertyDefinitions` 조회 에러 상태였다. `Property 1=Variant4`로 이름을 구분해 해결(이전 세션에서 겪은 것과 동일한 유형의 중복 variant 이름 문제).

## 실행 내용

1. 기존 단일 컴포넌트였던 `Item Card / Grid Small`(`6051:644`)을 clone
2. clone의 "item info" 영역 하단에 `button_medium`의 새 variant(흰 배경/파란 테두리, "장바구니 담기") 인스턴스 추가 — item info가 이미 세로 auto-layout(gap 4px)이라 자연스럽게 하단에 붙음, 카드 전체 높이 294→338px로 자동 증가
3. 원본과 clone을 `Type=Default` / `Type=Button` 2개 variant로 묶어 COMPONENT_SET 전환

## 검증

- 두 variant 스크린샷으로 정상 렌더링 확인
- 기존에 이미 배치돼 있던 실사용 인스턴스(`6059:772`)가 세트 전환 후에도 깨지지 않고 `Type=Default`로 정상 유지됨을 확인 (컴포넌트 ID가 그대로 보존되므로 기존 인스턴스는 영향 없음)

## 다음 단계 후보

- `button_medium` 세트의 남은 네이밍 정리(`Property 1` → `Type`, `Default/Variant3/Variant4` → 의미있는 이름)는 이전에 합의한 대로 체크리스트 완료 후 한 번에 진행
- 새로 추가한 버튼 variant(`Variant4`)에는 현재 leading/trailing 아이콘 슬롯이 없음(다른 variant와 달리 아이콘 인스턴스가 없는 상태) — 필요하면 후속으로 추가 가능

## 추가 작업 — Discount 축 추가 (v0.2)

사용자가 "할인중이지 않은 경우도 추가해달라"고 요청. Item Card에 Price Block과 동일한 `Discount`(Applied/None) 축을 독립적으로 추가하기로 확정 — Type(Default/Button) × Discount(Applied/None) 2×2 매트릭스, 총 4개 variant로 확장했다.

- 기존 2개 variant를 각각 `Discount=Applied`로 이름 보강
- 각각을 clone해 `Discount=None` 2개 신규 생성, 내부 Price Block 인스턴스를 `swapComponent()`로 기존 Price Block 컴포넌트의 `Discount=None` variant로 교체(override 보존 방식)
- 신규 2개가 기존과 같은 좌표에 겹쳐 배치되는 문제 발견 → 2번째 행으로 재배치, 세트 프레임 자체가 `clipsContent=true`라 새 행이 잘려 보이지 않는 문제도 발견해 프레임 높이를 수동으로 확장

## 검증 (v0.2)
- 4개 variant 스크린샷으로 Discount=Applied(취소선+할인율 배지)와 Discount=None(단일 가격) 차이가 정상 렌더링됨을 확인
- 기존 실사용 인스턴스가 `Type=Default, Discount=Applied`로 그대로 유지됨을 확인

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-02 | Item Card / Grid Small에 Type=Button variant 추가, button_medium 중복 이름 버그 수정 |
| v0.2 | 2026-09-02 | Discount(Applied/None) 축 추가, Type×Discount 2×2 매트릭스로 확장 |
