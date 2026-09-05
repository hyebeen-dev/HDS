# Project Audit — 작업 이력

> 이 문서는 `diagnosis/01`, `27`(구 파일명)를 시간순으로 합친 이력 기록이다. 원본 내용은 그대로 보존했다. `01`은 프로젝트 최초 시점의 진단이며, 이후 지적된 문제 상당수가 `27` 시점 이전에 해결됐다 — `01`을 읽을 때는 그 점을 감안한다.
> **지금 유효한 최신 상태(어떤 이슈가 아직 미해결인지)는 `design-system/status.md`를 참고.**

---

## 2026-09-01 — Figma 디자인 시스템 진단 리포트 (최초 진단, 구 diagnosis/01)

# Figma 디자인 시스템 진단 리포트 — Design System_0901

**문서 버전** v0.1
**작성일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**성격** 읽기 전용 진단 (이 조사 과정에서 Figma는 수정하지 않음)

---

## Context

이 문서는 `../prd-v0.1.md`(핵심 문제 P1/P2/P3 정의) 및 `../00-project-brief.md`(프로젝트 전체 프로세스)를 기준으로, Figma 파일의 실제 작업 상태를 진단한 결과다. `claude/02-design-system-scope-v0.1.md`는 프로젝트 폴더에 존재하지 않아, PRD 11장(Design System Requirements 예상 항목)을 Scope의 대체 기준으로 사용했다.

색상값·spacing·typography 등은 실제 Figma에서 관측된 값만 인용했으며, 신규 값을 제안하지 않았다.

**조사 범위의 한계**: `get_metadata`(nodeId 미지정)가 상위 페이지 목록을 "0:1 Cover" 하나만 보고하는 버그성 동작을 보였다. `1:9`(💠 Components 페이지)는 존재가 확인됐지만 목록에는 잡히지 않았다. 즉 이 파일에 Cover, 💠 Components 외 다른 페이지(예: 별도 Foundation 페이지)가 있는지는 완전히 배제하지 못했다 — 다만 💠 Components 페이지 내부를 전수 순회한 결과 Foundation을 별도로 정리한 프레임(색상 팔레트, Spacing 스케일, Typography 스펙시트 등)은 발견되지 않았다. 또한 `search_design_system`은 로컬 파일의 미publish 변수/컴포넌트를 찾지 못했다(전부 빈 배열 반환) — 라이브러리로 publish된 자산 검색용으로 보이며, 이 파일은 아직 라이브러리로 publish되지 않은 것으로 판단된다. 따라서 아래 진단은 `get_metadata` 전수 트리 + 8개 대표 컴포넌트 프레임에 대한 `get_variable_defs` 샘플링을 근거로 한다.

---

## A. 현재 Figma 구조 요약

- 파일에서 확인된 페이지: **Cover**(0:1, 표지 전용), **💠 Components**(1:9, 실질적 작업 페이지)
- 💠 Components 페이지 최상단에 디자이너 본인이 남긴 **"to make" 메모**(6008:7302)가 존재 — Tier 1/2/3으로 미착수 컴포넌트를 스스로 정리해둔 상태:
  - Tier 1(미착수): Section Header, App Bar Sticky, Action Bar, (Item Card 내) Price Block · Spec Row · Rating Display
  - Tier 2(미착수): Ad Container, Reason Label, Ratio Display, Confidence Indicator, Review Summary, Review Card, Evidence Chip, Anchor Navigation, Option Selector, Accordion, Chip/Filter
  - Tier 3(미착수): Ad Container(중복 기재), Empty State, Skeleton, Carousel + Indicator, Icon Button
- 착수/부분 완성된 컴포넌트: Button, App Bar(+검색창+칩+Tab 서브그룹), Badge(로켓배송 전용), Bottom Sheet, Item Card(grid_small/grid/list 3종 밀도 + cart), Dialog, Toast, Icon(일부), Review 목업(review_main/review/AI review)
- 페이지 내부에 **쿠팡 실제 화면 스크린샷 3장**(product_detail, product_list view, product_list grid)이 컴포넌트들과 같은 캔버스에 나란히 배치되어 있음 — 참고자료와 작업 산출물이 분리되어 있지 않음
- Figma 파일에 연결된 라이브러리는 Material 3 / Simple Design System / Apple 플랫폼 UI 킷 등 **기본 제공 커뮤니티 라이브러리뿐**이며, 이 프로젝트 전용으로 publish된 라이브러리는 없음
- Variables: **Primitive 컬러 스케일**(Color/Primitive/Gray·Primary·Secondary·Error·Warning, 각 50~900/1000 단계)과 **Text Style**(`text/body/{size}-{weight}`, `text/heading/large`)만 확인됨. **Semantic 컬러, Spacing, Radius, Elevation, Grid/Layout 토큰은 샘플링된 8개 컴포넌트 어디에서도 발견되지 않음**

## B. 잘 되어 있는 부분

- **Primitive 컬러 스케일 자체의 구조**는 일관됨 — Gray/Primary/Secondary/Error/Warning 각각 50~900(or 1000) 단계로 정리되어 있고, 컴포넌트들이 실제로 하드코딩이 아닌 변수를 바인딩해서 쓰고 있음(값이 아니라 변수명으로 조회됨)
- **Typography 스타일 네이밍 규칙**(`text/{category}/{size}-{weight}`)이 대체로 일관되게 지켜짐 (예: `text/body/xsmall`, `text/body/xsmall-medium`, `text/body/xsmall-bold`)
- Button, Bottom Sheet, Dialog, Toast는 **Property=Value 형태의 정식 Variant 속성**을 사용 중이며, `Status`(Default/Error/Warning), `Button`(1/2 Button) 같은 축이 Bottom Sheet와 Dialog에서 **재사용**되고 있어 최소한의 시스템적 일관성이 보임
- Item Card를 grid_small / grid / list 세 밀도로 나눠 설계하려는 시도(FR-L5 "정보 밀도가 다른 카드 변형" 요구사항과 방향 일치)는 있음
- 디자이너 스스로 "to make" 메모로 미완성 범위를 인지하고 관리 중 — 진단 정확도에 도움이 되는 신호

## C. 문제가 있는 부분

**중복**
- 동일한 색상값이 **두 개의 서로 다른 네이밍 체계**로 이중 정의되어 있음: `Color/Primitive/Gray/600 = #7b818e` vs `color/light/gray/60 = #7B818E`(동일값), `Color/Primitive/Primary/500` vs `color/light/primary/50`, `Color/Primitive/Error/400` vs `color/light/error/40`, `Color/Primitive/Static/0` vs `white`(네임스페이스 없음) 등. 값은 같지만 이름 체계(대소문자, 슬래시 depth, "light" 테마 접두어 유무)가 달라 **컴포넌트마다 서로 다른 변수를 참조**하고 있을 가능성이 높음 — 하나만 바꿔도 나머지 절반은 갱신되지 않는 구조
- Ad Container가 "to make" 메모에 Tier 2와 Tier 3 양쪽에 중복 기재되어 있어, 디자이너의 계획 자체에도 혼선이 있는 것으로 보임

**불필요한 Variant / 완성도 불균형**
- Button은 Large 사이즈에서 Primary/Secondary/Line type/Line type Gray × Default/Pressed/Disabled 조합이 상당수 존재하지만, Medium 사이즈는 "Line type gray, Default"류 2개뿐 — 사이즈 간 매트릭스가 불균형하게 미완성
- Cart 아이템(`Property 1=Default/Variant3/Variant4/Variant3`)은 **Figma 기본 속성명("Property 1")과 자동 생성 값명("Variant3"가 두 번 중복 사용됨)** 을 그대로 남겨둔 상태 — 실질적으로 이름 없는 placeholder 컴포넌트

**잘못된 Property 구조**
- Item Card의 Boolean성 속성(`sale`, `express shipping`/`free shipping`, `review`, `badge`)이 실제 Boolean 타입이 아니라 **"Y"/"N" 문자열 Variant**로 구현됨. 반면 Toast는 같은 성격의 속성(`Button`, `Icon`)을 **"True"/"False"**로 표기 — 같은 파일 안에서 Boolean 표현 규칙이 컴포넌트마다 다름
- App Bar의 Chip 컴포넌트에서 `rocket` 속성이 `False`(불리언 값)와 `tomorrow`/`fresh`/`global`(문자열 값)을 **한 속성 축에 혼재**시키고 있음 — 하나의 Variant 축에 서로 다른 성격의 값 타입이 섞여 있는 구조적 오류
- Item Card 밀도별로 배송 관련 속성명이 `express shipping`(grid_small)과 `free shipping`(grid, list)으로 **동일 개념에 다른 이름**을 사용

**이름 규칙 문제**
- Cart 아이템, Icon 일부(`Property 1=Default`가 두 번 중복), Review 목업(`Property 1=Default`) 등 다수 컴포넌트가 Figma 기본값(`Property 1`, `Variant2`, `Variant3`, `Default`)을 그대로 남겨 의미 있는 이름이 없음
- App Bar의 상태값 중 `status4`(status=status4)처럼 이름을 붙이다 만 placeholder가 남아 있음
- Typography에 `xxsmall`이라는 값이 `text/body/xxsmall`이 아니라 네임스페이스 없이 단독으로 존재 — 나머지 텍스트 스타일과 명명 규칙이 어긋남

**재사용성이 낮은 구조**
- PRD가 명시적으로 요구하는 Price Block(FR-C4, FR-C5, UR-G2)이 **독립 컴포넌트로 분리되어 있지 않고** "to make" 메모에 Item Card의 괄호 항목으로만 존재 — 현재는 각 Item Card 밀도별 마스터 컴포넌트 안에 가격 표현이 개별적으로 박혀 있을 가능성이 높고, 이는 List/Detail이 동일한 가격 구성 규칙을 공유해야 한다는 FR-C5, DG-3 요구사항과 정면으로 배치됨
- Spec Row, Rating Display도 동일하게 독립 컴포넌트가 아님 — 세 요소 모두 "재사용 가능한 서브 컴포넌트"가 아니라 Item Card 내부에 종속된 상태로 추정됨

**Foundation과 Component가 혼재된 부분**
- 💠 Components 페이지 안에 쿠팡 실제 화면 캡처 이미지(참고자료)가 컴포넌트 마스터들과 같은 캔버스, 같은 depth에 배치되어 있어 "이것이 우리 시스템의 컴포넌트인지, 참고 스크린샷인지" 구분이 페이지 구조만으로는 안 됨
- Foundation(Primitive 컬러/Typography)을 한눈에 확인할 수 있는 전용 프레임(팔레트 스와치, 타입 스펙시트)이 없고, 각 컴포넌트에 개별적으로 바인딩된 변수를 통해서만 간접적으로 확인 가능 — Foundation이 "문서"가 아니라 "부산물"로만 존재

## D. Design System Scope(PRD 11장 기준) 대비 분류

### D.1 Foundation

| 항목 | 현재 상태 | 분류 |
|---|---|---|
| Color (Primitive) | Gray/Primary/Secondary/Error/Warning 스케일 존재, 단 이중 네이밍 중복 | **수정 필요** (통합) |
| Color (Semantic) | 전혀 없음 | **신규 필요** |
| Typography | `text/{category}/{size-weight}` 스타일 존재, 일부 네이밍 이탈(`xxsmall`) | **수정 필요** |
| Spacing | 샘플링 범위 내 미발견 | **신규 필요** (또는 확인 불가 — 재조사 필요) |
| Layout / Grid | 미발견 | **신규 필요** |
| Radius | 미발견 | **신규 필요** |
| Border | 미발견 | **신규 필요** |
| Elevation | 미발견 | **신규 필요** |
| Icon | 최소 개수만 존재(placeholder 다수) | **수정 필요** |
| Image | 규격 정의 프레임 없음, 참고 스크린샷만 존재 | **신규 필요** |

### D.2 Components

| 항목 | 현재 상태 | 분류 |
|---|---|---|
| Button | Large 위주로 상당히 완성, Medium 불균형 | **수정 필요** |
| App Bar / Search / Tab | 기본 구조 존재 | **수정 필요** (상태값 placeholder 정리) |
| Badge | 로켓배송 전용, 범용 Badge/Label/Tag 없음 | **수정 필요** + **신규 필요**(범용) |
| Chip | Filter/rocket 혼재 구조 | **수정 필요** |
| Bottom Sheet / Dialog / Toast | Property 구조 양호 | **현재 구조 유지** |
| Item Card (Product Card) | 3밀도 존재하나 Price Block/Spec Row/Rating 미분리, 네이밍 불일치 | **수정 필요** |
| Cart Item | 이름 없는 placeholder, **PRD 범위 밖(장바구니는 제외 화면)** | **폐기 고려** (스코프 밖 작업) |
| Review 목업 | 컨텐츠 방향은 있으나 Variant화 안 됨 | **수정 필요** |
| Icon 컴포넌트 | placeholder 다수 | **수정 필요** |
| Divider, Accordion, Anchor Navigation, Carousel+Indicator, Rating Display(독립), Ratio Visualization, Empty State, Skeleton, Section Header, App Bar Sticky, Action Bar, Price Block, Spec Row, Promotion Badge, Ad Container, Review Summary, Evidence Chip, Reason Label, Option Selector | 전혀 없음(디자이너 본인 "to make" 목록과 일치) | **신규 필요** |

## E. PRD P1/P2/P3 기준으로 해결하지 못하는 부분

- **P1(강조의 위계)**: Semantic Color·Emphasis 토큰이 전무하므로 UR-E1~E6이 전부 미충족. 상업적 정보 구분 체계(UR-E5)의 대상인 Ad Container조차 없어 "제품 정보와 다른 표현 체계"를 만들 재료 자체가 없음
- **P2(정보 묶음/배치)**: Spacing 토큰 부재로 UR-G1(그룹 경계 표현), UR-G2(판단 단위 간 근접도) 구현 불가. Price Block/Spec Row가 독립 컴포넌트가 아니므로 FR-C4/C5/C6, UR-G3(Page Anatomy), UR-G4(List/Detail 동일 상대 위치)를 검증할 수 없음. FR-D3/D4(정보 그룹 이동 장치)에 해당하는 Anchor Navigation, Section Header, Sticky Action Bar가 전부 미착수 — Product Detail의 핵심 문제(이미지 위 진입점↔실제 정보 간 거리)를 해결할 컴포넌트가 아직 없음
- **P3(판단 근거)**: Review Summary, Evidence Chip, Confidence Indicator, Ratio Visualization이 전부 미착수. 현재 Review 목업은 신뢰 신호(UR-D3), 저신뢰 상태(UR-D4, E-10 케이스) 표현이 구조적으로 반영될 수 없는 placeholder 단계
- **시스템 전반(UR-S1~S5)**: 토큰 커버리지가 Color/Typography에 국한되어 "모든 시각 속성이 토큰으로 설명 가능"(UR-S1, DG-2)이라는 목표와 거리가 큼. 컴포넌트 사용 조건/금지 조건 문서(UR-S3)는 Figma 상에서 확인되지 않음(별도 문서 도구 사용 여부는 이번 조사 범위 밖)

## F. 특별 평가 항목

| 항목 | 평가 |
|---|---|
| **Emphasis Level** | 미존재. 3단계 강조(UR-E1) 개념 자체가 Figma 어디에도 토큰/문서로 표현되어 있지 않음. P1의 최대 병목 |
| **Semantic Color** | 미존재. Primitive만 있고, 그마저 이중 네이밍으로 중복. Semantic 레이어 신규 설계 + Primitive 통합이 동시에 필요 |
| **Spacing / Information Grouping** | Spacing 토큰 미발견. Page Anatomy 문서 없음. 그룹 경계를 표현할 시각 장치(구분선/헤더) 대상인 Divider·Section Header도 미착수 |
| **Product Card** | Item Card로 착수는 됐으나 밀도별 속성명 불일치, Boolean 표현 방식 불일치, 서브 컴포넌트(Price Block 등) 미분리로 구조적 완성도가 낮음. PRD 범위 밖인 Cart Item에 리소스가 일부 사용됨 |
| **Price Block** | 독립 컴포넌트 없음. FR-C4/C5(List·Detail 동일 구성 규칙)를 검증할 수 있는 최소 단위가 아직 없음. P2의 핵심 병목 |
| **Commercial Content** | Ad Container, Reason Label 전부 미착수. 현재 광고/프로모션을 제품 정보와 구분할 시각 체계가 전혀 없어 UR-E5, FR-C3가 완전히 미해결 |
| **Review / Evidence** | Review Summary(비율+모수+신뢰상태), Evidence Chip, Confidence Indicator 모두 미착수. 현재 review 목업은 Variant 구조가 없는 단일 placeholder로, UR-D1~D8 요구사항을 담을 그릇이 아직 아님 |
| **Content Format** | 텍스트 축약(UR-G5), 접힘/펼침(UR-D7, Accordion 미착수), 데이터 없음 상태(UR-D8, Empty State 미착수), 로딩 중 레이아웃 이동 방지(E-17, Skeleton 미착수) — 전부 미해결 |

---

## 권장 착수 순서 (참고용)

이 진단을 실제 작업으로 옮길 때 참고할 수 있는 순서 제안이다. 값(색상/spacing 등)은 포함하지 않으며, 순서와 범위만 제안한다.

1. Primitive 컬러 이중 네이밍 정리 → 단일 체계로 통합
2. Semantic Color 레이어 신규 설계 (강조 위계·상업 콘텐츠 구분 포함)
3. Price Block / Spec Row / Rating Display를 Item Card에서 독립 컴포넌트로 분리
4. Tier 1 미착수 컴포넌트(Section Header, App Bar Sticky, Action Bar) 착수
5. Tier 2~3 순서로 나머지 컴포넌트 진행

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | 최초 진단. Figma 읽기 전용 조사 기반. `claude/02-design-system-scope-v0.1.md` 부재로 PRD 11장을 대체 기준으로 사용 |

---

## 2026-09-02 — 디자인 시스템 종합 감사 (사용자 제작 UI 4종 기반, 구 diagnosis/27)

# Figma 작업 완료 기록 — 디자인 시스템 종합 감사 (사용자 제작 UI 4종 기반)

**문서 버전** v0.1
**작업일** 2026-09-02
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**대상 UI 노드** "UI" 섹션 (`6294:10390`) — 상품목록_그리드(`6249:6823`), 상품목록_리스트(`6223:4293`), 장바구니(`6223:4991`), 상품 상세페이지(`6249:8189`)
**작업 성격** 100% read-only 분석. Figma 수정 없음.

> 참고: 요청서에 언급된 `claude/01-prd-v0.1.md`, `claude/02-design-system-scope-v0.1.md`는 이 프로젝트에 존재하지 않는다(이전 세션에서 확인됨). 대신 실제 존재하는 `/Users/hyebeen/Desktop/HDS/prd-v0.1.md`(PRD, §11에 Foundation/Component/Pattern 예상 목록 포함)와 `00-project-brief.md`를 "Scope 문서"로 대체 사용했다.

---

## 요약 (먼저 결론)

사용자가 만든 4개 화면은 **컴포넌트를 실제로 조립해봤다는 사실 자체가 이미 큰 진전**이다. Action Bar·Review Card·Quantity Stepper·Chip·roket_badge·Icon 계열은 여러 화면에 걸쳐 정확히 재사용되고 있다. 그러나 화면을 만드는 과정에서 **① 이미 있는 컴포넌트(Price Block, Spec Row, Rating Display, Info Row)를 인스턴스로 꽂지 않고 raw 레이어로 새로 그린 곳이 다수 발견됐고, ② Button이 두 개의 분리된 세트(`Button_large`/`button_medium`)로 쪼개진 채 하나는 실제로 깨져 있으며, ③ Item Card 3형제(Grid Small/Grid/List) 중 Grid Small만 Type×Discount 속성을 갖고 나머지 둘은 단일 컴포넌트로 남아있다.** 이 세 가지가 이번 감사의 핵심이다.

---

## 1. Foundation 분석

### Color
- **실사용**: Primitive 8개 패밀리(Gray/Primary/Secondary/Point/Error/Warning/Success/Static, 총 100개 변수) + Semantic 8개 역할(Text/Background/State/Feedback/Emphasis/Commercial 등)이 신규 UI 전반에서 실제로 참조되고 있다. 예: 결제금액 강조는 Primary 계열, 할인율 배지는 Error 계열, 로켓 배지는 각 배송타입별 원색(주황/초록/보라/파랑)을 그대로 사용.
- **패턴**: 강조가 필요한 숫자(19,130원, 18,680원, 14,300원)는 전부 Primary 계열 파란색+Bold, 보조 정보(라벨, 옵션 설명)는 Text/Description 회색 — 강조 위계 규칙이 화면 전반에 일관되게 지켜지고 있다. **이미 Design Token으로 잘 자리잡은 부분.**
- **아직 체계가 없는 값**: "Delivery Badge"(로켓내일/프레쉬/직구/판매자로켓) 4색은 이전 감사(diagnosis/18)에서 이미 "미토큰화"로 문서화된 상태 그대로다. 신규 UI에서도 그 상태 그대로 재사용되고 있어 추가로 발견된 문제는 아니지만, 실사용 빈도가 높아졌으므로(그리드/리스트/상세 페이지 전부에 등장) 우선순위가 올라갔다고 볼 수 있다.

### Typography
- **실사용**: 18개 정의된 텍스트 스타일 중 다수가 실제로 바인딩되어 쓰이고 있다(예: "카드 할인" 라벨 14px Medium, "총 수량" 13px Medium 모두 실제 스타일에 바인딩 확인).
- **서로 다르게 쓰이는 값(발견)**:
  - **15px Bold/Medium**이 장바구니의 "총 결제금액" 헤더와 "총 4,000원 할인" 텍스트에 쓰이고 있는데, 18개 정의된 스타일 목록(12/13/14/17/19/18/22/26/30/11px) 어디에도 **15px는 없다.** 텍스트 자체는 `textStyleId`가 바인딩된 상태로 나오는데, 이는 (a) 사용자가 Figma에서 직접 15px짜리 스타일을 새로 만들었거나 (b) 다른 원인으로 바인딩이 발생한 것일 수 있다 — **직접 Figma Text Styles 패널에서 15px 스타일이 실제로 존재하는지 확인이 필요하다.**
  - **16px Bold**가 Review Card 내부의 별점 숫자("5")와 Button 라벨 등 여러 곳에서 산발적으로 쓰이는데, 이 값도 정의된 18개 스케일에 없다. 별점 숫자처럼 반복적으로 등장하는 값이라 새 토큰 후보로 적합하다.
  - 11px(`text/detail/small`)은 이전 세션에서 사용자가 직접 추가 완료했고, 신규 UI에서 아직 육안으로 활용 사례를 재확인하지 못했다(Price Block의 "100ml당" 표기 등에 쓰일 자리이나 raw 텍스트라 스타일 미바인딩 상태로 추정).
- **이미 Token으로 좋은 값**: 14px Medium(라벨), 13px Medium(보조 라벨), 19px Bold(핵심 금액), 17px Bold(부금액) — 실제 반복 사용 빈도가 높아 그대로 유지.
- **아직 체계가 없는 값**: 15px, 16px 두 개 — §4(Design Token 분석)에서 통합/신규 여부를 다시 판단한다.

> **후속 처리**: 15px/16px/17px 이슈는 이후 `diagnosis/29`, `diagnosis/30`(`typography-foundation-history.md`에 통합)에서 완전히 해결됐다 — 6개 카테고리(display/heading/body/label/navigation/underline) 체계로 재편.

### Spacing
- **실사용**: "spacing"이라는 빈 프레임을 스페이서로 반복 사용하는 패턴이 화면 전반에 나타난다. 실측한 높이값은 4/8/16/24/40px — **전부 기존 16개 Spacing 토큰(4,8,12,16,20,24,32,40,48,56,64 + 예외 2,6,10,11,13) 안에 정확히 들어맞는다.** 임의의 새 간격값을 쓰지 않았다는 점에서 Spacing Foundation(diagnosis/13)이 실제로 잘 지켜지고 있다.
- **패턴**: 섹션 사이(예: 상품정보→결제정보) 간격은 8px "divider" 프레임(두께 자체가 간격 역할까지 겸함) + 앞뒤 16px spacing 조합, 행 사이(예: Cart Item 사이)는 얇은 1px divider. **간격 자체는 토큰을 벗어나지 않지만, "spacing 프레임을 수동으로 끼워넣는" 방식 자체가 반복 작업이며 auto-layout의 `itemSpacing`으로 대체 가능한 경우가 많다** — Pattern 정리 후보(§7).

### Layout / Grid
- **실사용**: 그리드형 목록(Grid Small 2열, Grid 2열)은 기존 Grid Foundation(360폭/16마진/8거터, diagnosis/12)과 정확히 일치. 다만 **추천 영역(장바구니의 "다시 구매하세요", 상세페이지의 "함께 구매하면 좋아요")은 3열**(Item Card / Grid Small 130px×3 + divider)로, 목록 화면의 2열 Grid와 다른 컬럼 수를 쓴다 — 이는 임의 불일치가 아니라 **가로 스크롤 캐러셀 영역이라 별도 그리드 규칙이 필요한 정당한 차이**로 보이며(§5에서 재論), Grid Foundation에 "캐러셀/추천 영역은 3열 기준"이라는 문서화가 없다는 점만 보완하면 된다.

### Radius
- 신규 UI에서 카드/버튼 라운드값을 직접 다수 표본 조사하지는 못했으나(예산 제약), 기존 컴포넌트(Button cornerRadius 8/4, Chip 라운드/스퀘어 variant)가 재사용되고 있어 카드 이미지 라운드 등은 실제 컴포넌트를 통해 간접적으로 일관성이 유지되고 있을 가능성이 높다. **별도 Radius 토큰 컬렉션은 아직 없음** — Foundation §11.1 예상 목록에 있던 항목이나 아직 미착수.

### Border / Divider
- 두 종류로 명확히 구분되어 쓰이고 있다: **1px(행/카드 사이의 얇은 구분선)**, **8px(섹션 사이의 두꺼운 여백형 구분)**. 둘 다 Spacing 토큰(1은 별도 hairline, 8은 기존 토큰)과 일치하며 성격이 다른 구분선이 실제로 다른 굵기로 구현되고 있다는 점에서 **의도가 분명한 체계**로 판단된다. 다만 "divider"라는 이름 하나로 1px/8px 두 성격이 섞여 있어 레이어 이름만 봐서는 구분이 안 된다 — 네이밍 보완 후보.

### Elevation / Surface
- 신규 UI에서 그림자/elevation을 쓰는 사례를 발견하지 못했다(전부 flat, 흰 배경 + divider로 영역 구분). PRD에도 Elevation은 "고정 영역 처리"(Action Bar 등)를 위한 보조 표현으로 예상만 되어 있고(§11.1), 아직 실제로 필요해진 정황은 없다 — **지금 단계에서 정의하지 않아도 되는 항목.**

### Icon
- Icon/Chevron Right, Icon/Status, Icon/Checkbox, Icon/Utility가 신규 UI에서 실제로 재사용되고 있다(상품 상세의 브랜드샵 링크, 아코디언 화살표, 상품문의 리스트의 이동 화살표 등). **다만 같은 Chevron Right 아이콘이 "펼치기"(아래 방향 회전, Info Row Expandable)와 "이동하기"(오른쪽 방향, 상품문의 리스트)라는 서로 다른 의미로 재사용되고 있다** — 시각적으로는 같은 벡터라 재사용 자체는 합리적이지만, 의미가 다른 두 액션에 완전히 동일한 아이콘을 쓰는 게 사용자 인지 관점에서 맞는지는 확인이 필요하다(디자인 판단은 사용자 몫으로 남김).

### Image
- 상품 이미지, 리뷰 사진, 상세 이미지 전부 실제 비율(정사각형 130×130, 106×107, 79×79 등)로 쓰이고 있으나, 별도의 "Image" Foundation(비율 규칙, 깨짐 대응)은 아직 정의되지 않았다. PRD E-6(이미지 없음/규격 불일치)에 대한 처리도 신규 UI에서 확인되지 않는다 — Tier 3 "Empty State"와 연결지어 후속 필요.

### Motion / Content Format
- 이번 4개 화면은 정적 스크린이라 Motion 실사용 근거를 찾을 수 없었다. Content Format(문구 톤, 숫자 표기 규칙 등)은 "9,999+", "3,301 더보기" 같은 축약 표기가 반복되고 있어 패턴화 가능성이 있으나 현재는 개별 텍스트로 하드코딩. 우선순위 낮음(P3).

---

## 2. Component 분석

| UI 요소 | 분류 | 근거 |
|---|---|---|
| Button (Large) | 이미 Component로 존재 (`Button_large`, 10 variants) | Action Bar, Cart Item, Item Card에서 인스턴스로 재사용 확인 |
| Button (Medium) | Component는 존재하나 **구조가 깨짐** | `button_medium` 세트에 `Property 1=Variant4`가 **중복 등록**돼 `componentPropertyDefinitions` 조회 자체가 에러(§3에서 상세) |
| Item Card (Grid Small) | 이미 Component로 존재, 구조 양호 | Type×Discount 2×2 매트릭스, 신규 UI에서 정상 인스턴스로 재사용 |
| Item Card (Grid) | Component는 존재하나 **구조가 체계적이지 않음** | 단일 컴포넌트(속성 없음) — 신규 UI에서도 그대로 인스턴스 사용 중이지만 Discount=None 케이스를 표현할 방법이 없음 |
| Item Card (List) | Component는 존재하나 **구조가 체계적이지 않음** | 위와 동일 — 단일 컴포넌트, 속성 없음 |
| Price Block | **비슷한 UI가 다른 방식으로 만들어짐** | 컴포넌트 자체(Discount=Applied/None)는 잘 만들어져 있는데, 상품 상세페이지에서는 인스턴스가 아니라 raw 텍스트+프레임으로 재구현됨 |
| Spec Row | **비슷한 UI가 다른 방식으로 만들어짐** | 동일 — 상세페이지의 배송 정보 블록이 raw 구조. 단, 내부의 `roket_badge`만은 실제 인스턴스라 부분적으로만 재사용됨 |
| Rating Display | **비슷한 UI가 다른 방식으로 만들어짐** | 상세페이지 상단의 별점("4.83 (최근 6개월 4.92) · 리뷰 9,999+")이 raw 프레임. Item Card 내부에 들어간 것들은 실제 인스턴스로 정상 재사용 — 즉 **같은 컴포넌트가 문맥에 따라 인스턴스/raw로 갈림** |
| Info Row | Component는 존재하나 **UI에 재사용되지 않음** | 결제/적립 정보(`payment_information`)와 상품 부가정보(`product_info`)가 Info Row(Type=Text)와 완전히 동일한 라벨+내용 패턴인데 raw로 새로 만들어짐 |
| App Bar | 이미 Component로 존재, 활발히 재사용 | 다만 13개 variant까지 늘어났고 그 중 `type12`처럼 이름이 정리 안 된 신규 항목 발견(§3) |
| Chip | 이미 Component로 존재, 정상 재사용 | AI 리뷰 요약의 6개 chip, 필터 chip 모두 인스턴스 |
| Action Bar | 이미 Component로 존재, 모범적으로 재사용 | 장바구니/상세페이지 중간/상세페이지 하단 3곳에서 정확히 재사용. **다만 Layout 속성에 `Layout3`라는 미정리 이름이 새로 추가됨**(§3) |
| Review / Card | 이미 Component로 존재, 모범적으로 재사용 | 3개 인스턴스가 정확히 재사용됨(체크리스트 체크박스만 아직 미체크 — 문서 동기화 누락) |
| Review Summary | **UI에는 존재하지만 Component화되지 않음** | "향 만족도 80% / 거품 75%..." 바 형태 위젯이 raw로 구현. 단, 이는 애초에 Tier 2 미착수 컴포넌트라 "재사용 실패"가 아니라 "신규 컴포넌트 필요" 케이스 |
| 상품문의/판매자정보/배송정책 리스트 행 | **UI에는 존재하지만 Component화되지 않음** | 라벨+카운트+이동화살표 구조가 3번 반복되는데 컴포넌트가 없음. Info Row와 구조는 유사하나 트레일링 아이콘 의미가 "펼치기"가 아니라 "페이지 이동"이라 완전히 같은 컴포넌트로 묶기보다 별도 Variant/컴포넌트로 검토 필요 |
| 총 결제금액 breakdown | **UI에는 존재하지만 Component화되지 않음** | "총 상품가격/총 즉시할인/총 배송비/총 결제예상금액" 4행 + 강조 합계 — Info Row와 유사하지만 우측 정렬 숫자·강조 단계가 달라 Info Row 그대로 쓰기엔 안 맞음. 별도 패턴 후보 |
| Quantity Stepper | 이미 Component로 존재, 정상 재사용 | 상세페이지 수량 선택에 정확히 인스턴스로 사용 |
| Icon (Chevron Right, Status, Utility) | 이미 Component로 존재, 정상 재사용 | 여러 화면에서 재사용 확인 |
| "N개 사면 N원 절약" 툴팁 텍스트 | **Component로 만들 필요 없는 일회성 요소** | 상세페이지 1곳에만 등장, 반복 근거 없음 |
| "3,301 더보기" 리뷰 사진 오버레이 | **Component로 만들 필요 없는 일회성 요소로 보이나 재확인 필요** | 리뷰 사진이 4장을 넘을 때만 나타나는 상태이므로, Photo Review 컴포넌트 자체가 생기면 그 안의 State로 편입하는 게 맞고 독립 컴포넌트는 불필요 |

**요약**: 컴포넌트가 "없어서" 생긴 raw 구현은 소수(Review Summary, 결제금액 breakdown, 상품문의 리스트)이고, **대부분의 raw 구현은 이미 있는 컴포넌트(Price Block/Spec Row/Rating Display/Info Row)를 안 쓰고 새로 그린 경우**다. 이게 이번 감사에서 가장 먼저 손봐야 할 지점이다.

---

## 3. Component Property / Variant 구조 분석

### 문제 1 — Button이 두 개의 분리된 세트로 쪼개짐, 하나는 깨짐 (P0)
- `Button_large`(`2167:15918`, 10 variants): Type=Primary/Secondary/Line type/Line type Gray × State=Default/Pressed/Disabled, Size=Large 고정. **Text 타입(이전 diagnosis/14에서 추가했던 것)이 사라져 있다** — 사용자가 정리 과정에서 삭제했거나 별도 이동한 것으로 보임, 확인 필요.
- `button_medium`(`6192:2344`, 4 variants): `Property 1`이라는 미정리 속성명 아래 `Default`/`Variant3`/`Variant4`가 있는데, **`Variant4`가 두 개(`6294:10035`, `6249:6238`)로 중복 등록돼 `componentPropertyDefinitions` 조회 자체가 에러 상태다.** 지금 이 세트를 선택하면 Figma 속성 패널이 정상 작동하지 않을 가능성이 높다.
- **제안**: 두 세트를 하나의 `Button` 세트로 합치고 `Size=Large/Medium` 축으로 통합하는 것이 원래 설계 의도(diagnosis/17)였다. 지금처럼 완전히 분리된 두 세트로 굳어지면 "버튼 하나 바꿀 때 두 군데를 따로 관리"해야 하는 구조적 부담이 생긴다. 최소한 **`button_medium`의 중복 이름부터 해소**하고, 두 세트를 통합할지 분리 유지할지는 사용자 판단이 필요(둘 다 유효한 선택지 — 통합하면 관리 포인트가 줄고, 분리하면 Variant 조합 폭발을 피할 수 있음).

> **후속 처리**: `button_medium`의 중복 이름(`Variant4`×2)은 이후 `diagnosis/24`(`item-card-family-history.md`에 통합)에서 해소됐다. Button 세트 통합 여부는 여전히 미결정.

### 문제 2 — Item Card 3형제의 속성 구조 불일치 (P0)
- Grid Small만 `Type`(Default/Button) × `Discount`(Applied/None) 2×2 매트릭스를 가짐.
- Grid, List는 여전히 **속성이 하나도 없는 단일 컴포넌트**다. 즉 List/Grid 타입에서는 "할인 없는 상품"을 표현할 방법 자체가 컴포넌트 레벨에 없다(내부의 Price Block을 손으로 swap해야 함).
- **제안**: Grid Small에 적용한 것과 동일한 패턴(Discount 축)을 Grid/List에도 동일하게 적용해 3형제의 속성 구조를 맞추는 것을 최우선으로 제안. Button 축은 Grid Small에서 "리스트에 버튼이 필요한 특수 케이스"였을 뿐이므로 Grid/List에까지 강제할 필요는 없다(불필요한 Variant 증식 방지 원칙에 따름).

### 문제 3 — App Bar에 미정리 이름이 다시 쌓이고 있음 (P1)
- 지난 세션에서 `status=X` → `type=X`로 정리했던 App Bar(diagnosis 참고)에 사용자가 이후 3개 variant(`delete`, `type12`, `dropdown`)를 추가하면서 **`type12`라는 자동생성/미정리 이름이 다시 등장**했다. 정리 직후에도 같은 패턴이 재발한다는 것은 "새 variant 추가 시 이름 규칙을 지키자"는 규칙이 아직 습관화되지 않았다는 신호 — 컴포넌트를 새로 추가할 때마다 즉시 이름을 정리하는 루틴이 필요하다(체크리스트 완료 후 일괄 정리 방식보다, 추가 시점에 바로 정리하는 게 재발을 막는다).

### 문제 4 — Action Bar에도 미정리 Layout 값 추가 (P2)
- `Layout` 속성에 `1 Button`/`2 Button` 외 **`Layout3`**가 추가됨. 실제로 몇 번째 화면에서 어떤 용도로 쓰였는지 확인 후 의미있는 이름(예: `3 Button`, 또는 다른 레이아웃이면 그에 맞는 이름)으로 바꾸는 게 좋다.

### Boolean vs Variant 판단
- Spec Row의 `Show ETA`/`Show Reward`(Boolean) + `Type`(Variant: Rocket/Free/Today/None) 조합은 **적절한 예시**다 — 배송 유형은 서로 배타적 상태라 Variant가 맞고, ETA/적립 표시 여부는 단순 on/off라 Boolean이 맞다. 이 패턴을 Item Card Grid/List의 속성 설계에도 참고할 만하다.
- Info Row의 `Type=Text/Expandable/Link`도 세 형태가 구조적으로 다르므로(내용만 있음 vs 내용+아이콘 vs 내용+버튼) Variant가 적절하다. 단순 "아이콘 표시 여부"였다면 Boolean이 맞았겠지만, 실제로는 하위 구조 자체가 달라 Variant가 옳은 선택.

---

## 4. Design Token 분석

| 값 | 관찰 | 제안 |
|---|---|---|
| 15px 텍스트(총 결제금액 헤더, 총 4,000원 할인) | 정의된 18개 스타일에 없는데 `textStyleId`가 바인딩된 상태로 나타남 | **확인 필요** — Figma에서 15px 스타일이 실제로 새로 만들어졌는지 직접 확인 후, 실수면 기존 14/17px로 통합, 의도된 거면 정식 토큰(`text/body/large-medium` 계열 사이 값)으로 등록 |
| 16px 텍스트(별점 숫자 등) | 스케일에 없는 값이 여러 곳에서 반복 | **새 토큰으로 정의** 후보 — 반복 빈도가 있으므로 무시하기보다 스케일에 편입 검토 |
| Divider 1px vs 8px | 성격이 다른 두 구분선(행 vs 섹션)이 다른 굵기로 일관되게 쓰임 | **유지** — 이미 체계적. 다만 레이어 이름을 `divider-thin`/`divider-section`처럼 구분하면 더 명확 |
| Spacing 4/8/16/24/40px | 전부 기존 16개 토큰 범위 내 | **유지** — 새 토큰 불필요 |
| Delivery Badge 4색(로켓내일/프레쉬/직구/판매자) | 여전히 컴포넌트에 하드코딩, Primitive 승격 안 됨(diagnosis/18에서 이미 문서화된 이슈) | **Semantic Token으로 분리** 검토 시점이 됐다고 판단 — 신규 UI 전 화면에 반복 등장해 우선순위가 올라감 |
| Grid 컬럼 수(2열 목록 vs 3열 캐러셀) | 서로 다른 값이지만 용도(목록형 vs 가로 캐러셀)가 명확히 다름 | **통합 불필요** — Grid Foundation에 "캐러셀 영역은 3열"이라는 문서 한 줄만 보완 |

---

## 5. UI 간 일관성 분석

- **App Bar 높이(48px)와 app_bar_small(40px)**: 화면마다 정확히 같은 값으로 재사용되고 있다 — 통일 유지, 문제 없음.
- **Item Card List에는 "최대 770원 적립" 리워드 행이 있는데 Grid 카드에는 없다.** 같은 Item Card 패밀리인데 정보 밀도가 타입별로 다르다 — **왜 통일해야 하는가**: 사용자가 그리드→리스트로 뷰를 전환했을 때 같은 상품의 정보량이 달라지면 "어느 쪽이 정확한 정보인지" 혼란을 준다. **통일 판단**: Spec Row 컴포넌트 자체에 이미 `Show Reward` Boolean이 있으므로, 이건 컴포넌트 설계 문제가 아니라 **그리드 카드 인스턴스에서 Show Reward를 꺼둔 것뿐**일 가능성이 높다 — 의도적 밀도 조절이면 유지해도 되고, 실수면 Boolean만 켜면 되는 가벼운 이슈.
- **가격 강조 방식은 화면 전반에서 일관됨**(항상 파란색 굵게 + 할인율은 빨간 배지) — 통일돼 있어야 할 이유가 명확하고 실제로 지켜지고 있다.
- **카운트다운 표기가 두 곳에서 다른 형식**: 장바구니 상단 "할인 종료 09:10:28"과 Cart Item 내부 "00:42:25 내 주문 시"가 같은 "시간 제한" 개념인데 색상(주황 vs 파랑)과 문구 패턴이 다르다. **왜 통일해야 하는가**: 둘 다 "지금 안 사면 놓친다"는 긴급성 신호인데 표현이 다르면 사용자가 둘을 별개 정보로 오인할 수 있다(PRD P1 강조 위계 문제와 직결). 반대로 **통일 안 해도 되는 이유**도 있다 — 하나는 "장바구니 전체에 적용되는 할인 임박"이고 다른 하나는 "개별 상품 주문 마감"이라 서로 다른 스코프의 정보이므로 완전히 같은 스타일일 필요는 없을 수 있다. **판단은 사용자 몫으로 남기되, 최소한 이 두 카운트다운이 같은 컴포넌트 패밀리(가칭 Countdown Badge)로 묶여야 한다는 점은 분명하다** — 지금은 각각 raw 텍스트라 나중에 색상 하나 바꾸려면 두 곳을 따로 찾아야 한다.
- **Price Block/Spec Row/Rating Display가 목록 화면에서는 인스턴스, 상세 화면에서는 raw**: 같은 정보(가격/배송/평점)를 두 화면에서 시각적으로는 거의 동일하게 보여주면서 실제 구현은 다르다. **왜 통일해야 하는가**: 지금 당장은 똑같아 보여도, 나중에 Price Block 컴포넌트의 색상이나 간격을 하나만 바꾸면 목록 화면엔 반영되고 상세 화면엔 반영 안 되는 "보이지 않는 불일치"가 반드시 생긴다. 이게 이번 감사에서 가장 실질적인 위험이다.

---

## 6. Design System Scope와 비교

### A. 이미 충분히 구현된 부분
- Foundation 4종(Color/Typography/Grid/Spacing)이 실제 화면에서 정확히 참조되고 있다(15/16px 예외 제외).
- Button(Large)/Item Card(Grid Small)/Cart Item/Price Block/Spec Row/App Bar/Chip/Action Bar/Review Card — PRD §11.2에서 예상했던 "범용 + 커머스 특화" 컴포넌트 목록의 상당수가 실제로 만들어져 있고 대부분 재사용 사례도 있다.
- FR-D6(구매 CTA 어디서나 접근 가능)은 Action Bar가 3개 화면에서 정확히 재사용되며 실질적으로 충족되고 있다.

### B. UI에는 존재하지만 Design System으로 정리되지 않은 부분
- 결제금액 breakdown(총 상품가격/즉시할인/배송비/예상금액), 카운트다운 배지, 상품문의류 리스트 행 — 전부 반복 등장하는 실제 패턴인데 아직 컴포넌트/체크리스트 어디에도 없음.
- Review Summary는 PRD FR-D8/D9(만족도 비율+모수) 요구사항이 명확히 있고 체크리스트에도 등록돼 있지만(Tier 2), 실제 UI에서는 그 요구사항을 raw로 먼저 구현해버린 상태 — 체크리스트와 실제 진행 상황이 어긋나 있다.

### C. Design System에는 정의되어 있지만 UI에서 제대로 활용되지 않은 부분
- **Info Row(Type=Text)**: 정의는 있는데 실제로 쓰여야 할 두 자리(결제/적립 정보, 상품 부가정보)에 쓰이지 않았다 — 이번 감사에서 가장 명확한 "활용 안 된" 사례.
- Price Block/Spec Row/Rating Display의 상세페이지 미활용도 같은 범주.

### D. 현재 UI에서 새롭게 발견되는 부분
- **결제금액 breakdown 패턴**(라벨-값 행 다수 + 강조 합계 1행) — PRD E-3/E-4(가격 구성 최소 단위, 조건부 금액)와 연결되는 신규 패턴, Foundation §11.3 Pattern 목록에 없던 개념.
- **카운트다운/시간 제한 배지** — PRD에 명시적 요구사항은 없었으나(가장 가까운 건 P1 강조 위계) 실제 커머스 UI에서 반복적으로 필요해진 요소.
- **탐색용 리스트 행(상품문의/판매자정보/배송정책)** — Anchor Navigation·Section Header와는 다른, "다른 페이지로 이동"하는 목적의 리스트 행 패턴. PRD 어디에도 명시되지 않았지만 실제 상세페이지 하단에 필수로 등장.

---

## 7. 우선순위 제안

### P0 — 반드시 먼저 정리해야 하는 구조적 문제
1. **`button_medium`의 중복 variant 이름(`Variant4` × 2) 해소** — 지금 상태로는 속성 패널 자체가 깨져 있어 더 이상의 작업이 위에 쌓일수록 되돌리기 어려워진다.
2. **Item Card Grid/List에 Discount 축 추가**(Grid Small과 동일 패턴) — 지금 상태로는 "할인 없는 상품"을 표준적으로 표현할 방법이 두 타입에 없다.
3. **Price Block/Spec Row/Rating Display를 상세페이지에서 실제 인스턴스로 교체** — 컴포넌트를 만든 의미 자체가 훼손되는 지점이라 가장 먼저 손대야 한다.

### P1 — 여러 화면의 일관성에 큰 영향을 주는 문제
4. Info Row를 결제/적립 정보 및 상품 부가정보 자리에 실제로 적용.
5. Button `Button_large`/`button_medium` 통합 여부 결정(사용자 판단 필요) 및 App Bar `type12` 등 신규 미정리 이름 정리.
6. 카운트다운 배지를 하나의 컴포넌트 패밀리로 정리(현재 2곳에서 다른 스타일로 raw 구현).

### P2 — Component / Token 체계를 개선하면 좋은 문제
7. 결제금액 breakdown, 상품문의 리스트 행을 신규 컴포넌트로 정식화.
8. 15px/16px 텍스트 값의 정체 확인 후 토큰 편입 또는 통합.
9. Delivery Badge 4색의 Semantic Token 승격 여부 결정.

### P3 — 나중에 정리해도 되는 세부 사항
10. Divider 레이어 이름을 두께별로 구분(`divider-thin`/`divider-section`).
11. Grid Foundation 문서에 "캐러셀 영역 3열" 규칙 한 줄 보완.
12. Content Format(축약 표기 규칙) 패턴화.

### 최종 정리

1. **가장 먼저 정리할 Foundation**: 없음(Foundation 자체는 신규 문제가 발견되지 않음, 15/16px 값 확인만 필요)
2. **가장 먼저 정리할 Component**: `button_medium`(중복 에러), Item Card Grid/List(Discount 축 누락)
3. **재설계가 필요한 Component**: Button 세트 구조(Large/Medium 통합 여부 결정)
4. **새롭게 만들어야 할 Component**: Review Summary(Tier 2, 이미 체크리스트에 있음), 결제금액 Breakdown, 카운트다운 배지, 탐색용 리스트 행
5. **새롭게 정의해야 할 Token**: 15px(확인 후)/16px 텍스트 스케일 편입 여부, Delivery Badge Semantic 승격 여부
6. **Pattern으로 정리해야 할 부분**: "라벨+값 반복 행"(Info Row가 일부 커버하나 우측정렬 숫자형·이동형은 별도 패턴 필요), 카운트다운/긴급성 표시
7. **굳이 Component화하지 않아도 되는 부분**: "N개 사면 N원 절약" 툴팁류 일회성 텍스트, 리뷰 사진 "더보기" 오버레이(Review Card 자체의 State로 흡수하면 충분)

---

## 현재 디자인 시스템 상태 한 문장 요약

**Foundation과 토큰 체계는 실사용 화면에서도 대체로 정확히 지켜지고 있지만, 이미 만든 컴포넌트를 화면 조립 시점에 다시 raw로 그리는 습관과 Button/Item Card 일부의 속성 구조 불일치가 "따로 노는 디자인 시스템"으로 굳어지기 직전 단계다.**

## 다음 작업에서 무엇부터 하면 되는지 (5단계 이내)

1. `button_medium` 중복 variant 이름부터 해소(안 그러면 계속 에러 상태로 작업이 쌓임)
2. Item Card Grid/List에 Discount 축 추가해 3형제 속성 구조 통일
3. 상세페이지의 Price Block/Spec Row/Rating Display를 raw → 실제 인스턴스로 교체
4. 결제/적립 정보·상품 부가정보 자리에 Info Row 실제 적용
5. Button 세트 통합 여부를 사용자가 결정한 뒤, App Bar/Action Bar에 새로 쌓인 미정리 이름(`type12`, `Layout3`) 정리

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-02 | 사용자 제작 UI 4종 기반 디자인 시스템 종합 감사 (read-only) |
