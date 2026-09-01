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
