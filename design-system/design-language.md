# Design Language

> **문서 성격**: 이 문서는 "진단"이 아니라 **현재 UI에 실제로 존재하는 시각 스타일을 관찰해 정리한 살아있는 참조 문서**다. 원본은 `diagnosis/28-design-language-v0.1.md`(2026-09-02, 최초 추출 작업 기록)이며, `design-system/` 문서 재정리(2026-09-03)로 이 위치로 옮겨왔다. 새 컴포넌트가 쌓이며 관찰되는 패턴이 바뀌면 버전 파일을 새로 만들지 않고 이 문서를 직접 갱신한다. 임의로 새 디자인 원칙을 추가하지 않는다 — 항상 Figma에서 실제로 확인한 내용만 반영한다.

---

**최초 작성일** 2026-09-02
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**작업 성격** 100% read-only 시각 스타일 관찰. 새 디자인 원칙을 만들지 않고, 지금 만들어진 UI에서 이미 반복되는 패턴만 추출. Figma 수정 없음.

**조사 대상**: Foundation 4종(Color/Typography/Grid, Spacing은 루트 페이지 목록에 없어 컴포넌트 실측값으로 대체 확인), 주요 컴포넌트(Button/Item Card 4형제/Price Block/Spec Row/Chip/Cart Item/Action Bar/Review Card/App Bar/Section Header), 실제 UI 4화면(상품목록 그리드·리스트, 장바구니, 상품 상세페이지)

---

## 1. 전체 UI 관찰

Foundation·컴포넌트·실제 화면을 통틀어 가장 먼저 눈에 띄는 반복 패턴:

- **정보가 "칩/배지 형태의 원색 강조"와 "본문의 무채색 텍스트"로 명확히 분리**돼 있다. 색이 들어간 요소(할인율 배지, 로켓 배지, CTA 버튼)는 전부 pill/rounded 형태의 독립된 시각 단위이고, 순수 정보 텍스트(상품명, 배송 설명, 옵션)는 전부 무채색이다.
- **강조에 크기보다 색+weight를 우선 사용**한다 — 최종 판매가(19px Bold, 할인 적용 시 Error 빨강/미적용 시 Gray 900)와 할인율(14px Bold, 흰 글씨+빨강 배경)은 크기 차이가 크지 않은데도 색과 배경으로 위계가 분명히 갈린다.
- **가격 관련 "최종 판매가"는 할인 적용 시 Error 빨강, 미적용 시 Gray 900(거의 검정), "할인율"은 항상 Error 빨강**으로 역할이 고정돼 있다(`Price Block` 실측 확인, `design-system/decisions.md` 2026-09-04). Primary 파랑은 가격 강조에는 쓰이지 않고 **CTA 버튼·링크 등 "행동" 용도로만** 배정된다.
- 그림자(elevation)는 원칙적으로 미사용 — 정의된 `shadow` 이펙트 스타일(1개, DROP_SHADOW radius 10 opacity 5%)이 있지만 대부분 어디에도 바인딩되지 않았다. 깊이 표현은 대부분 **얇은 divider 선**으로 처리되며, `Toast`(BACKGROUND_BLUR 3건)·`Bottom Navigation`(DROP_SHADOW 2건)만 확인된 예외다(2026-09-04 감사).

---

## 2. Visual Style 분석

### Color


실제 Semantic 토큰 19개를 Primitive까지 전부 역추적한 결과(Commercial 3개는 2026-09-04 실사용 0건 확인 후 삭제):

| 역할 | 실제 값 | 근거 |
|---|---|---|
| Text/Primary | `#020b18`(거의 검정) | Primary/1000 |
| Text/Description | `#7b818e`(중간 회색) | Gray/600 |
| Text/Accent | `#106def`(파랑) | Primary/500 |
| Background | `#f4f6f6`(연회색) | Gray/50 |
| Divider | `#e3e5e8` | Gray/200 |
| State/Primary(CTA) | `#106def` | Primary/500 |
| Feedback/Error | `#e23636` | Error/400 |
| Feedback/Success | `#1dc948` | Success/500 |
| Emphasis/Primary | `#f2460d`(주황빨강) | Point/500 |

- **Primary color(파랑) 사용 방식**: CTA 버튼 배경, 링크 텍스트("더보기", 카테고리 드롭다운) 등 **"지금 눌러야 하는 행동"에만** 쓰인다. 최종 판매가 등 가격 강조에는 쓰이지 않는다 — `Price Block` 실측 결과 최종가는 할인 적용 시 Error 빨강, 미적용 시 Gray 900이다(`design-system/decisions.md` 2026-09-04). 화면 전체에서 파랑이 차지하는 면적은 작지만 반드시 행동 지점에만 등장.
- **Neutral/Gray 사용 방식**: 배경(Gray/50), 보조 텍스트(Gray/600), 구분선(Gray/200) — 3단계로 명확히 분리돼 있고 서로 다른 회색을 혼용하지 않는다.
- **Text hierarchy**: Primary(거의 검정, 상품명·핵심 정보) → Description(회색, 보조 설명) → On Color(흰색, 색 배경 위 텍스트) → Accent(파랑, 링크·CTA) 4단계.
- **Background hierarchy**: Background(연회색, 페이지 배경) vs 컴포넌트 자체는 대부분 흰색(Static/0) — 흰 카드가 연회색 배경 위에 얹히는 구조. 별도의 "카드 표면색" 토큰은 없고 흰색을 그대로 사용.
- **Border color**: Divider(연회색, `#e3e5e8`)와 Line type 버튼 테두리(Primary/500, `#106def`)로 두 종류뿐 — 장식용 테두리가 따로 없다.
- **Disabled color**: `State/Disabled` = Gray/300(`#cdd0d5`) 하나로 통일.
- **Feedback color**: Error=빨강(`#e23636`), Success=초록(`#1dc948`), Warning=주황(`#ebb447`) — 표준적인 3색 배정.
- **Accent color 사용 빈도**: 낮음~중간. 한 화면에 파랑이 2~4곳(CTA, 링크) 이상 등장하지 않는다 — 강조를 아끼는 방식.
- **로켓 배지 4색(주황/초록/보라/파랑)**: 2026-09-04 `Color/Primitive/Rocket/{Seller,Fresh,Global,Tomorrow}/{Background,Text}` 8개 변수로 정식 등록 완료, `Rocket Badge` 컴포넌트 4 variant 전량 바인딩됨(더 이상 하드코딩 아님). Semantic 레이어로는 승격하지 않음 — 배송 유형이라는 도메인 특화 개념이라 Primitive로 충분하다고 판단(`design-system/decisions.md`, `design-system/design.md` §1.1). 원색이지만 배지 영역 안에서만 국한돼 화면 전체 톤을 해치지 않는다는 관찰은 유효.

**색이 실제로 의미를 전달하는 방식**: 빨강=할인율·긴급·최종 판매가(할인 시), 회색=중립 정보 및 최종 판매가(무할인 시), 파랑=행동(CTA·링크), 로켓 배지 원색=배송 유형 식별. **색상이 절대 장식으로 쓰이지 않고 전부 정보 역할에 종속**돼 있다.

### Typography

- **Font family**: Pretendard(실제 UI 콘텐츠 전부) — 정의된 텍스트 스타일 전부 Pretendard. 2026-09-04 재정리 이후 `display`(예약, 미생성)/`heading`(7)/`body`(15)/`label`(3)/`navigation`(3)/`underline`(1) 6개 역할 카테고리, 총 29개 스타일 체계로 확정(`design-system/design.md` §1.2).
- **Font size hierarchy**(실측): 30/26/22/18(heading) → 19/17/14/13(body) → 12/11(xxsmall/detail). Heading은 전부 Bold, Body는 Regular/Medium/Bold 3중.
- **Line height**: 대부분 150% 고정, `text/body/small`(14px Regular)만 140% — **일관되게 지켜지는 규칙**.
- **Letter spacing**: 전부 0, `text/body/large-bold`(19px)만 -0.5px — 가장 큰 금액 텍스트에만 자간을 살짝 좁혀 밀도감을 준 예외.
- **가격 표현**: 실측 확인 — 최종 판매가(예: "14,300원")는 **19px Bold**(`text/body/large-bold`)로 본문 텍스트 중 가장 크고 진하다. 할인율("29%")은 14px Bold, 정가(취소선)는 더 작은 회색. **같은 "숫자"라도 확정 금액이 가장 크고, 나머지는 보조로 축소**되는 명확한 위계.
- **평점/리뷰 수**: "4.83"은 13px Bold, "(9,999+)"/"리뷰 9,999+"는 13px Regular — 같은 크기(13px)에서 **weight로만** 위계를 나눔(크기 위계가 아니라 weight 위계).
- **✅ 해결됨(2026-09-04)**: 상품명("도브 화이트피치...")은 **15px Medium**으로, 이제 정식 `text/body/compact-medium` 스타일(15px Medium, `body` 카테고리)에 바인딩돼 있다. 같은 조사에서 `Quantity Stepper` 숫자·할인 안내 텍스트 등 9곳이 이름은 로컬 스타일과 같지만 외부 팀 라이브러리의 원격(remote) 스타일을 참조하던 문제도 함께 발견돼 로컬 스타일로 재연결됐다(총 12곳, 전체 재스캔으로 미바인딩·원격 참조 0건 확인). `design-system/design.md` §1.2, `design-system/decisions.md` 2026-09-04 참고.

### Spacing

Spacing Foundation 페이지 자체는 이번 조사에서 페이지 목록에 잡히지 않았으나(알려진 페이지 나열 불완전 이슈), 실제 컴포넌트 padding/gap을 다수 실측한 결과:

| 컴포넌트 | 좌우 padding | 상하 padding | 내부 gap |
|---|---|---|---|
| Button(Large) | 20 | 11(예외) | 10(예외) |
| Button(Medium) | 8(예외) | 9(예외) | 10(예외) |
| Chip | 10(예외) | 6(예외) | 4(예외) |
| Action Bar | 16 | 16 | 0 |
| Item Card 내부 | 0 | 0 | 8 |
| Review Card 내부 | — | — | 16 |

**반복되는 relationship**: 화면 좌우 마진은 항상 **16px**(Grid Foundation과 일치), 카드류 내부 요소 간 gap은 **8px**, 섹션 사이는 **8px 두꺼운 divider + 16px spacing 조합**(이전 감사에서도 확인된 패턴). **버튼류(Button/Chip)만 4의 배수를 벗어난 예외 padding을 반복적으로 사용** — 이건 "불일치"라기보다 `diagnosis/spacing-foundation-history.md`에서 사용자가 이미 "컴포넌트 특성상 불가피한 경우 2px 단위 예외 허용"이라 명문화해둔 규칙과 정확히 일치한다. **일관되게 지켜지는 것**: 마진 16 / 카드 내부 8. **예외로 허용된 것**: 버튼·칩의 세로 padding.

### Shape

- **Border radius**: 2026-09-04 공식 5단계 토큰(`none`=0/`small`=4/`medium`=8/`large`=16/`full`=999, 코너 클램핑으로 pill/원 렌더링)으로 확정, 컴포넌트 라이브러리 전체 1,346곳 바인딩(`design-system/design.md` §1.5). Button Large·Medium=`medium`(8), Button Small·XSmall=`small`(4), Chip=`full`(완전 pill), Card/Action Bar=`none`(0). **"인터랙션 가능한 작은 요소일수록 더 둥글다"**는 경향(`full` > `medium` > `small` > `none`).
- **Border thickness**: 전부 1px 고정.
- **Divider**: 1px(행 구분)과 8px(섹션 구분, 사실상 두꺼운 여백형 구분) 두 종류만 반복.
- **Card shape**: 각진 사각형(`none`, radius 0), 이미지만 별도 radius를 가질 가능성 있으나 이번 조사에서 Item Card 최상위 컨테이너 자체는 radius 0으로 확인.
- **Button shape**: Large·Medium=`medium`(8), Small·XSmall=`small`(4) — 완전 각짐도 완전 pill도 아닌 중간.
- **Icon Button shape**: 기존에 Button과 같은 `medium`(8) 계열로 알려져 있었으나 실측 결과 `full`(pill, radius 50)로 정정됨(`design-system/design.md` §1.5).
- **Input shape**: 검색창은 App Bar 스크린샷상 pill에 가까운 둥근 형태로 관찰(정확한 radius 미실측 — **정의되지 않음**으로 표시).
- **Chip/Badge shape**: Chip은 완전 pill(`full`). Rocket Badge는 pill이 아니라 `small`(4, 살짝 둥근 사각형)임이 실측으로 확인됨(Chip과 다른 형태).
- **Bottom Sheet shape**: `Bottom Sheet / Informational`(구 `Sheet / Confirmation`), `Bottom Sheet / Interactive`(구 `Sheet / Selector`, 2026-09-05 개명 반영) 모두 헤더 상단 두 모서리는 `large`(16), 본체는 `none`(0)으로 확정됨(하단 모서리는 `none`).
- **모달형 오버레이 높이 상한**: Dialog는 `max-height: 70vh`, Bottom Sheet(Informational/Interactive 공통)는 `max-height: 80vh` — 서로 다른 두 컴포넌트가 독립적으로 "고정 px가 아니라 뷰포트 상대 단위(`vh`)로 높이를 제한한다"는 같은 규칙에 수렴했다(`design-system/design.md` §2.5, §2.12).

### Elevation / Surface

- **Shadow 사용 여부**: 원칙적으로 미사용 — `shadow`라는 이펙트 스타일이 정의는 돼 있지만(DROP_SHADOW radius 10, 검정 5%) 대부분의 노드에는 바인딩되지 않았다. 다만 **확인된 예외 2건**이 있다(2026-09-04 감사 확인): `Toast`는 BACKGROUND_BLUR 3건, `Bottom Navigation`은 DROP_SHADOW 2건을 실제 사용 중이다. 둘 다 화면 위에 떠 있는 고정 오버레이 성격의 컴포넌트로, 의도된 예외로 잠정 분류된다(`design-system/design.md` §1.6).
- **Surface hierarchy**: 흰 카드 vs 연회색 배경, 2단계뿐. 카드 위에 또 다른 카드가 얹히는 3단계 이상의 surface는 관찰되지 않음.
- **선호 방식**: **Border/Divider를 압도적으로 선호**, Shadow는 정의만 되고 대부분 미사용 — 단 Toast(blur)/Bottom Navigation(shadow) 두 고정 오버레이 컴포넌트는 예외. 깊이감보다 "선으로 나누는" 평면적 스타일이 기본이다.

### Iconography

- **Icon size**: 16×16(Chevron Right 등 대부분), Rating 아이콘도 16×16 관�023. 32×32 크기의 터치 영역(예: 상품문의 리스트의 chevron 아이콘 wrapper)도 관찰되나 내부 그래픽 자체는 16×16 유지.
- **Stroke 느낌**: Chevron 벡터 strokeWeight 1.5px, 5×10 크기의 작은 화살표 — **가는 선(thin outline) 스타일**.
- **Icon과 Text의 관계**: 아이콘이 텍스트보다 항상 작거나 비슷한 시각적 무게 — 아이콘이 텍스트를 압도하지 않음. 로켓 배지처럼 "아이콘+텍스트가 한 세트"인 경우가 많다(단독 아이콘보다 라벨 동반이 기본형).
- **Filled/Outline**: 로켓 배지 아이콘은 filled(원색 채움), Chevron·UI 컨트롤 아이콘은 outline(stroke만) — **정보성 아이콘=filled, 내비게이션/컨트롤 아이콘=outline**으로 갈리는 경향.
- **Icon Button 형태**: 별도의 "아이콘 전용 버튼" 컴포넌트(`Icon Button`)가 정식으로 존재한다(`Type`(Secondary/Line type gray) × `Size`(Medium/Small) × `Status`(Default/Pressed)). Radius는 `full`(pill) — Button(CTA)의 `medium`(8) 계열과 다름. 실사용은 리뷰 "도움이 돼요" 단일 용례뿐(`design-system/design.md` §2.3).
- **Checkbox/Radio 글리프**: `Icon / Checkbox`·`Icon / Radio` 둘 다 위 "정보성 아이콘=filled" 경향을 그대로 따르는 사례다(원색 채움 체크/원 아이콘). 다만 2026-09-05 재확인 결과 두 컴포넌트 모두 `Status=Default`/`Disabled`(또는 원 색상) variant만 있고 "선택 안 됨(빈 박스/빈 원)"을 표현하는 variant 자체가 없다 — 즉 시각 언어로서는 filled 원칙에 맞지만, 실제 체크/언체크를 토글하는 인터랙티브 컴포넌트는 아니다. `Icon / Checkbox`는 그럼에도 `Item Card / Cart`·`Cart / Selection Toolbar`·장바구니 화면에서 실사용 중(`design-system/design.md` §2.15).

---

## 3. Information Hierarchy 분석

| 요소 | 크기 | Weight | Color | Spacing/Position | Background |
|---|---|---|---|---|---|
| 최종 판매가 | 19px(가장 큼) | Bold | 할인 적용 시 Error 빨강 / 미적용 시 Gray 900 | 할인율 배지 바로 옆 | 없음 |
| 할인율 | 14px | Bold | 흰색 | 판매가 왼쪽, 독립 배지 | **Error 빨강(유일하게 배경색을 쓰는 정보)** |
| 정가(취소선) | 작음(14px대 추정) | Regular | 회색+취소선 | 판매가 위 | 없음 |
| 상품명 | 15px(`text/body/compact-medium`) | Medium | 검정 | 가격 위 | 없음 |
| 배송 정보(로켓) | 13px대 | Regular/Bold | 로켓 배지=원색, 설명 텍스트=회색 | 가격 아래 | 배지만 원색 |
| 평점 | 13px | Bold(숫자)/Regular(부가) | 검정+주황 별 아이콘 | 배송정보 아래 | 없음 |
| 리뷰 수 | 13px | Regular | 회색 | 평점 옆, divider로 분리 | 없음 |
| 상태(품절 등) | (Info Label로 처리) | — | — | — | — |
| CTA | 16px대 | Bold | 흰색 텍스트 | 화면 폭 꽉 채움 | **Primary 파랑(면 전체 배경)** |
| 보조 설명(옵션 등) | 13px | Regular | 회색 | 본문 하단 | 없음 |
| 메타데이터(날짜 등) | 13~14px | Regular | 회색 | 최하단 | 없음 |

**패턴 요약**: 이 디자인은 **"배경색을 가진 요소 = 가장 강한 강조"**라는 규칙을 쓴다 — 화면 전체에서 배경색이 채워지는 곳은 딱 두 곳뿐이다: **할인율 배지(빨강)**와 **CTA 버튼(파랑)**. 그 외 모든 위계는 크기·weight·색(배경 없이 텍스트 색만)으로 처리된다. 즉 강조 수단의 우선순위는 **Background-fill > Color > Size > Weight** 순으로 강도가 세지고, "정말 중요한 것"에만 background-fill을 아껴 쓴다.

---

## 4. Component의 시각적 문법

- **Button**: radius는 크기 티어별로 묶임(Large·Medium=`medium` 8 / Small·XSmall=`small` 4), 좌우 padding은 20(Large)/8(Medium)로 크게 차이나지만 상하 padding은 둘 다 4의 배수를 벗어난 예외값(11/9) — **"버튼 높이는 4의 배수 규칙보다 시각적 균형을 우선한다"**는 걸 보여주는 반복 패턴.
- **Chip**: radius `full`(완전 pill) + 얇은 1px 테두리(Default) → 채움(Selected로 추정) 전환 방식. Button과 달리 **테두리 기반**이 기본 상태.
- **Card(Item Card/Review Card 등)**: 이미지 → 텍스트 정보 → (구분 요소) → 메타데이터 순서가 반복된다. 내부 gap은 공통적으로 8~16px. 카드 자체에 테두리/그림자가 없고 페이지 배경과의 대비(흰 카드 vs 연회색 배경)만으로 경계를 표현.
- **여러 컴포넌트에 공통되는 문법**: (1) 색이 있는 요소는 항상 pill/rounded, 정보 텍스트는 항상 각짐 (2) 상태 전환은 배경색보다 **텍스트/테두리 색 변화**로 표현되는 경우가 많음(Button Pressed는 톤다운, Line type은 테두리만 채움) (3) 강조 색상(파랑/빨강)은 절대 큰 면적을 차지하지 않고 배지·버튼 등 "작은 단위"에만 국한.
- **모달형 오버레이(Dialog/Bottom Sheet)의 높이·스크롤 처리**: 둘 다 전체 높이를 고정 px가 아니라 뷰포트 상대 단위(`vh`)로 제한한다(Dialog `70vh`, Bottom Sheet `80vh`) — 서로 다른 시점에 독립적으로 같은 규칙에 수렴했다. 스크롤 인디케이터(구분선+스크롤바, `Show Scroll` Boolean)는 길이가 예측 불가능한 자유 텍스트 콘텐츠(Dialog, Bottom Sheet Informational)에만 붙고, 리스트·캐러셀처럼 형태 자체가 스크롤을 암시하는 콘텐츠(Bottom Sheet Interactive)에는 붙지 않는다(`design-system/design.md` §2.5, §2.12, §4 원칙 9).
- **탭형 선택 UI의 역할 분리**: `Tab Group`(장바구니의 일반구매/자주산상품/찜한상품처럼 같은 데이터를 관점별로 재구성)과 `Category Tab`/`Chip`(서로 다른 카테고리·옵션을 나열해 선택)은 겉보기엔 비슷한 가로 선택 UI지만 역할이 명확히 다르다(`design-system/design.md` §2.13, §4 원칙 10).

---

## 5. Commerce UI 특유의 스타일

| 정보 | 표현 방식 |
|---|---|
| 가격(최종) | 19px Bold, 할인 적용 시 Error 빨강 / 미적용 시 Gray 900 |
| 할인율 | 14px Bold 흰 글씨 + Error 빨강 배경 pill |
| 할인 전 가격 | 작은 회색 + 취소선 |
| 배송 정보 | 회색 텍스트, 로켓 관련이면 원색 배지 동반 |
| 로켓 관련 | `Rocket Badge` 컴포넌트(주황/초록/보라/파랑 4색, `Color/Primitive/Rocket` 패밀리로 토큰화 완료) + 아이콘+텍스트 세트, radius `small`(4) |
| 평점 | 주황 별 아이콘 + 검정 Bold 숫자 + 회색 부가정보 |
| 리뷰 수 | 회색 Regular, 평점과 divider로 구분되지만 같은 행에 배치 |
| 적립 혜택 | Info Row 패턴(라벨+내용, `diagnosis/info-row-component.md`) — 컴포넌트는 있으나 실제 화면에서 raw로 재구현된 경우 발견(`diagnosis/project-audit-history.md`) |
| 품절 | Info Label 컴포넌트(Label=SoldOut)로 정의는 돼 있음, 실사용 화면 노출은 이번 조사에서 미확인 |
| 프로모션 | Commercial 색상 그룹(Warning 계열) 별도 존재 — 일반 정보와 확실히 다른 색 체계 |
| CTA | Primary 파랑 풀블리드 버튼, 항상 화면 폭 꽉 채움 |
| 상품 옵션 | Quantity Stepper(수량 조절) + 안내 문구("2개 사면 2,300원 절약") 조합 |

**정보 과다 시 복잡도를 낮추는 방식**: (1) 리뷰 상세 지표(향 만족도 등)는 **막대바+퍼센트**로 축약 (2) 긴 리뷰 본문은 처음부터 몇 줄만(Review Card 구조상 body가 고정 영역) (3) 추천 상품군은 **가로 스크롤 캐러셀**로 세로 스크롤 길이를 줄임 (4) 결제 관련 다수 항목(상품가/할인/배송비/예상금액)은 **얇은 divider로만 구분된 단순 리스트**로 나열하고 마지막 합계만 Bold로 강조.

---

# Design Language

## Visual Character
연회색 배경 위에 흰 카드가 얹히는 평면적(flat) 구조. 그림자·깊이 표현은 원칙적으로 없고 얇은 선(divider)만으로 영역을 나눈다(단 Toast/Bottom Navigation은 예외). 색은 거의 무채색(검정/회색)이 기본이고, 파랑(행동)과 빨강(할인·최종가) 두 가지 강조색만 아주 좁은 영역(배지, 버튼, 가격 숫자)에 집중적으로 쓰인다. 전체적으로 "정보를 나열하되, 결정적인 지점만 색으로 짚어주는" 인상.

## Color Language
Neutral(회색 3단계: 배경/구분선/보조텍스트)을 기반으로, Primary 파랑은 "CTA·링크 등 행동"에만 배정되고 가격 강조에는 쓰이지 않는다. Error 빨강은 "할인율·긴급성" 그리고 **할인 적용 시 최종 판매가**에 배정되며, 최종 판매가는 할인 미적용 시 Gray 900(거의 검정)이다(`Price Block` 실측, `design-system/decisions.md` 2026-09-04). 배경색이 채워지는 요소는 화면 전체에서 CTA 버튼과 할인율 배지 두 종류뿐 — 나머지 위계는 텍스트 색만으로 처리한다. 로켓 배송 배지 4색은 2026-09-04 `Rocket` Primitive 색상 패밀리로 정식 토큰화됐고, 여전히 좁은 배지 영역에 갇혀 있다.

## Typography Language
Pretendard 단일 패밀리, 6개 역할 카테고리(`display`(예약)/`heading`/`body`/`label`/`navigation`/`underline`)에 총 29개 스타일. 대부분 line-height 150% 고정. 가격·평점처럼 Commerce에서 중요한 숫자는 크기보다 **weight(Bold)** 로 우선 강조되고, 그중에서도 "최종 판매가"만 스케일에서 가장 큰 body 사이즈(19px)를 차지한다. 상품명의 15px Medium은 2026-09-04 `text/body/compact-medium` 스타일로 정식 바인딩 완료.

## Spacing Language
화면 좌우 마진 16px, 카드 내부 요소 gap 8px가 가장 강하게 반복된다. 섹션 간은 8px 두꺼운 구분선 + 16px 여백 조합. 4의 배수 원칙이 레이아웃 레벨에서는 잘 지켜지고, 버튼·칩처럼 작은 인터랙션 요소의 세로 padding에서만 예외(2px 단위)가 허용되는 이미 문서화된 규칙과 일치.

## Shape Language
2026-09-04 공식 5단계 Radius 토큰(`none`=0/`small`=4/`medium`=8/`large`=16/`full`=999)으로 확정. 버튼은 Large·Medium이 `medium`(8), Small·XSmall이 `small`(4), Chip·Icon Button은 완전한 pill(`full`), Card·Action Bar는 완전히 각진 사각(`none`) — "작고 눌리는 요소일수록 둥글다"는 경향이 뚜렷하다. `large`(16)는 이 축과 별개로 Dialog/Bottom Sheet 헤더 상단 모서리 전용. Border는 항상 1px. Elevation은 원칙적으로 미사용(Toast/Bottom Navigation 예외), Border/Divider가 깊이 표현을 전담한다.

## Icon Language
16×16 기준, 가는 outline(strokeWeight 1.5) 위주. 내비게이션/컨트롤 아이콘은 outline, 배송 배지 같은 정보성 아이콘은 filled 원색 — 역할에 따라 스타일이 갈린다. 아이콘 단독보다 텍스트와 짝을 이루는 구성이 기본형이다. 전용 `Icon Button` 컴포넌트가 정식으로 존재하며(radius `full`), 실사용은 리뷰 "도움이 돼요" 단일 용례뿐이다. `Icon / Checkbox`·`Icon / Radio`도 filled 원칙을 따르는 사례지만, 둘 다 "선택 안 됨" variant가 없어 시각 언어로만 완성돼 있고 아직 진짜 토글 컴포넌트는 아니다.

## Information Hierarchy
강조 강도는 **배경색 채움 > 텍스트 색 > 크기 > weight** 순으로 세진다. 배경색을 쓰는 요소는 CTA와 할인율 배지뿐이라는 점에서, 이 디자인은 강조 수단을 극도로 아껴 쓰고 "정말 눌러야 하거나 정말 중요한 숫자"에만 최상위 시각 강조를 배정한다.

## Interaction Language
Button은 Pressed 시 색 톤다운(배경 자체를 바꾸지 않고 어둡게), Line type은 테두리만 채워진 상태로 존재. Chip은 테두리 있음→채움 방식의 Selected 전환으로 추정(정확한 Selected 색은 이번 조사에서 미실측 — 정의되지 않음). 상태 전환이 대체로 "색의 진하기"로 표현되고 형태 자체는 거의 안 바뀐다.

## Commerce Language
최종 판매가는 할인 적용 시 빨강/미적용 시 거의 검정, 할인율은 빨강 배경, 배송 배지는 원색 아이콘+텍스트 세트(Rocket 토큰화 완료), 리뷰 상세지표는 막대바+퍼센트로 축약. 정보 밀도가 높아지는 지점(리뷰, 추천상품, 결제 breakdown)은 전부 "얇은 divider로 나눈 단순 리스트" 또는 "가로 스크롤 캐러셀"로 시각적 복잡도를 낮춘다.

## Core Design Principles

1. **강조는 배경색 채움을 최상위 수단으로 아껴 쓴다** — 화면 전체에서 배경이 채워지는 요소는 CTA와 할인율 배지뿐이다(Product Detail 실측, `2229:9383`/Error 배지 확인).
2. **파랑=행동(CTA·링크), 빨강=할인율·긴급·최종 판매가(할인 시)로 색의 의미가 고정돼 있다** — `Price Block` 실측 결과 최종가는 할인 적용 시 Error 빨강, 미적용 시 Gray 900이며 파랑은 가격 강조에 쓰이지 않는다. Primary 파랑은 CTA 버튼 배경·링크 텍스트 전용(`design-system/decisions.md` 2026-09-04).
3. **인터랙션 가능한 작은 요소일수록 더 둥글다** — `full`(Chip/Icon Button 등, 999) > `medium`(Button Large/Medium, 8) > `small`(Button Small/XSmall, 4) > `none`(Card/Action Bar, 0)의 공식 5단계 Radius 토큰(`design-system/design.md` §1.5).
4. **깊이는 원칙적으로 그림자가 아니라 선으로 표현한다** — 정의된 shadow 이펙트 스타일은 대부분 미사용이나, `Toast`(BACKGROUND_BLUR 3건)와 `Bottom Navigation`(DROP_SHADOW 2건)은 확인된 예외(고정 오버레이 컴포넌트, 2026-09-04 감사 확인).
5. **Commerce 숫자는 크기보다 weight로 위계를 나눈다** — 평점 "4.83"과 "(9,999+)"가 같은 13px에서 Bold/Regular로만 구분됨(실측).
6. **여백은 16(화면 마진)/8(카드 내부) 두 값이 대부분을 지배하고, 버튼·칩류만 예외를 허용한다** — 이미 `diagnosis/spacing-foundation-history.md`에서 문서화된 4배수 규칙과 정확히 일치.
7. **정보 밀도가 높아지면 리스트+divider 또는 가로 캐러셀로 낮춘다**, 새 레이아웃 패턴을 만들지 않는다(결제 breakdown·추천상품 실측 확인).
8. **아이콘은 역할에 따라 outline(컨트롤)과 filled(정보성 배지)로 나뉜다** — Chevron 계열 vs 로켓 배지 비교로 확인.
9. **모달형 오버레이의 높이는 뷰포트 상대 단위로 제한하고, 스크롤 인디케이터는 콘텐츠 형태에 따라 선택적으로 붙는다** — Dialog(`70vh`)와 Bottom Sheet(`80vh`)가 독립적으로 같은 규칙에 수렴했고, 자유 텍스트 콘텐츠에만 구분선+스크롤바가 붙는다(`design-system/design.md` §2.5, §2.12, §4).
10. **탭형 선택 UI는 역할에 따라 컴포넌트가 분리된다** — `Tab Group`은 같은 데이터를 관점별로 재구성(장바구니 실측), `Category Tab`/`Chip`은 서로 다른 대상을 나열해 선택한다(`design-system/design.md` §2.13, §4).

---

## 최종 정리

### 1. 현재 디자인에서 가장 강하게 드러나는 스타일 5가지
1. Flat + Border 기반 구조 (그림자는 원칙적으로 미사용, Toast/Bottom Navigation만 예외, divider가 깊이 표현 전담)
2. 강조색 2종의 엄격한 역할 분리 — 파랑=행동(CTA·링크), 빨강=할인율·긴급·최종 판매가(할인 시)
3. 배경색 채움을 "가장 강한 강조 수단"으로 극도로 아껴 씀(CTA·할인배지 외 전무)
4. 공식 5단계 Radius 토큰(`none`/`small`/`medium`/`large`/`full`)에 따라 요소 크기·인터랙션 성격에 비례하는 일관된 shape 규칙(`full` > `medium` > `small` > `none`)
5. 16px 화면 마진 / 8px 카드 내부 gap이 지배적인 spacing 언어

### 2. 아직 스타일이 명확하게 정의되지 않은 부분
1. Input(검색창)의 정확한 radius 값 — 실측 못함
2. Chip Selected 상태의 정확한 색상값 — 이번 조사에서 미확인

> 과거 이 목록에 있던 "상품명 15px 미바인딩", "Bottom Sheet radius 미실측", "로켓 배송 배지 Semantic Token화 여부"는 2026-09-04 세션에서 모두 해결돼 목록에서 제거함(상세는 위 각 절 및 `design-system/design.md`, `design-system/decisions.md` 참고).

### 3. 앞으로 새 Component를 만들 때 반드시 유지해야 할 규칙
- 배경색 채움은 CTA/긴급 신호 등 "정말 중요한 것"에만 — 장식적으로 배경색을 쓰지 않는다.
- 파랑은 행동(CTA·링크)에만, 빨강은 할인율·위험 신호 및 할인 시 최종가 강조에 쓴다는 색-의미 매핑을 그대로 따른다. 파랑을 가격 강조에 쓰지 않는다.
- radius는 공식 5단계 토큰(`none`/`small`/`medium`/`large`/`full`) 중 요소 크기·인터랙션 성격에 맞는 값을 쓴다(작고 누르는 것=더 둥글게).
- 그림자를 쓰지 않고 divider/여백으로 경계를 표현하는 것이 기본이다(고정 오버레이류만 예외 검토 가능).
- 화면 마진 16 / 카드 내부 gap 8을 기본값으로 삼는다.
- 모달형 오버레이(Dialog/Bottom Sheet류)를 새로 만들 때는 높이를 고정 px가 아니라 `vh` 상대 단위로 제한하고, 자유 텍스트 콘텐츠에만 스크롤 인디케이터(구분선+스크롤바)를 붙인다 — 리스트·캐러셀 형태 콘텐츠에는 붙이지 않는다.
- 가로로 여러 옵션을 나열하는 선택 UI를 새로 만들 때는 "같은 데이터를 관점별로 재구성하는가"(Tab Group류) "서로 다른 대상을 나열해 고르는가"(Category Tab/Chip류)를 먼저 구분하고 그에 맞는 컴포넌트를 재사용한다.

### 4. 반대로 아직 결정하지 말아야 할 부분
- Input의 정확한 radius 값 — 아직 실측/합의된 값이 없으므로 새로 지어내지 않는다.
- Chip Selected 색상 — 정의되지 않은 상태이므로 다음 컴포넌트 설계 시 추측하지 말고 확인 후 진행.

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-02 | 기존 UI/컴포넌트 기반 Design Language 추출 (read-only) |
| v0.2 | 2026-09-04 | design.md 리뷰 세션(2026-09-04)에서 확정된 사실 반영해 전면 갱신: (1) "확정 금액=파랑" 원칙 폐기 — `Price Block` 실측대로 할인 시 Error 빨강/미적용 시 Gray 900, 파랑은 CTA·링크 등 행동 전용으로 정정(전체 UI 관찰·Color 섹션·Core Design Principles·새 Component 규칙 등 관련 서술 전부 수정) (2) Commercial 색상 그룹(3개 변수) 삭제 반영, Semantic 토큰 수 22→19개로 정정 (3) 로켓 배지 4색이 `Rocket` Primitive 패밀리(8개 변수)로 정식 토큰화됐음을 반영, "미토큰화" 서술 제거 (4) 상품명 15px Medium이 `text/body/compact-medium`에 정식 바인딩됐음을 반영, "정의되지 않음" 서술 제거 (5) Typography 스케일을 "18단계(11~30px)"에서 "6개 카테고리·29개 스타일" 체계로 정정 (6) Radius를 컴포넌트별 개별 값 서술에서 공식 5단계 토큰(`none`/`small`/`medium`/`large`/`full`) 체계로 전면 재기술, Button Large/Medium이 실제로는 같은 `medium`(8) 티어임을 반영, Icon Button=`full`(기존 "컴포넌트 없음" 서술 제거), Bottom Sheet 헤더 상단 모서리=`large`(16) 확정 반영 (7) Shadow "실사용 0건" 서술을 Toast(blur)·Bottom Navigation(shadow) 확인된 예외 2건 명시로 정정. "최종 정리" 절의 미정의 목록에서 위 해결된 항목들 제거 |
| v0.3 | 2026-09-05 | design.md §4가 8개→10개 원칙으로 갱신된 것을 동기화: (1) Shape 절의 `Sheet / Confirmation`/`Sheet / Selector`를 `Bottom Sheet / Informational`/`Bottom Sheet / Interactive`(2026-09-05 개명)로 정정하고, Dialog `70vh`/Bottom Sheet `80vh`가 독립적으로 vh 상대 단위 높이 규칙에 수렴했다는 관찰 추가 (2) Iconography·Icon Language 절에 `Icon / Checkbox`·`Icon / Radio`가 filled 원칙을 따르는 사례이자 "선택 안 됨" variant가 없어 아직 진짜 토글 컴포넌트는 아니라는 사실 추가(design.md §2.15 재확인 근거) (3) "Component의 시각적 문법" 절에 모달 오버레이 높이·스크롤 패턴, 탭형 선택 UI 역할 분리(Tab Group vs Category Tab/Chip) 관찰 2건 추가 (4) Core Design Principles에 9·10번 신규 추가(모달 오버레이 vh 높이+선택적 스크롤 인디케이터, 탭 역할 분리) (5) "새 Component를 만들 때 반드시 유지해야 할 규칙"에 대응 규칙 2줄 추가. design.md/decisions.md/status.md는 참조만 하고 수정하지 않음 |
