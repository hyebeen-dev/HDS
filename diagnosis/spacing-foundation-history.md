# Spacing Foundation — 작업 이력

> 이 문서는 `diagnosis/09`, `13`(구 파일명)를 시간순으로 합친 이력 기록이다. 원본 내용은 그대로 보존했다.
> **지금 유효한 최신 상태는 `design-system/status.md`를 참고.**

---

## 2026-09-01 — Spacing Variables 값 보강 + 역할(Role) 설명 (구 diagnosis/09)

# Figma 작업 완료 기록 — Spacing Variables 값 보강 + 역할(Role) 설명

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**참고** Gmarket Design System Spacing 페이지(https://gds.gmarket.co.kr/foundation/spacing)의 "값+역할" 문서화 방향을 참고했으나, 토큰 이름은 사용자 확정에 따라 티셔츠 사이즈(xxxs~xxxl)가 아닌 기존 숫자 이름을 유지했다.

---

## 배경

Spacing Variable 컬렉션(4/8/12/16/20/24/32/40/48/56/64, 11개)이 있었지만 Typography와 마찬가지로 파일 전체 사용 0회였다. 실제로 이번 세션에서 만든 컴포넌트(Button, Item Card, Price Block, Spec Row, Rating Display, Cart Item, Quantity Stepper, Order Deadline)와 기존 컴포넌트(App Bar, Badge, Toast, Chip)의 실제 padding·gap 값을 조사한 결과, 스케일에 없는 값(2, 6, 10, 11, 13)이 하드코딩 상태로 쓰이고 있었다.

## 실행 내용

- **신규 추가**: 2, 6, 10, 11, 13 — 전부 실제 컴포넌트에서 확인된 값
- **역할 설명 16개 전체 작성**: 실사용 확인된 7개(2, 4, 6, 8, 10, 11, 13, 16, 20 — 정정: 총 9개)는 실제 사용처를 근거로, 미사용 7개(12, 24, 32, 40, 48, 56, 64)는 "미사용·예약된 값"으로 정직하게 표기

## 조사 근거 (실사용 확인)

| 값 | 확인된 사용처 |
|---|---|
| 2 | Rating Display, Order Deadline (아이콘-텍스트 간격) |
| 4 | Spec Row, Item Card item info (내부 요소 간격) |
| 6 | Chip 상하 padding |
| 8 | Item Card, Cart Item 최상위 블록 간격 |
| 10 | Button 아이콘-라벨 간격, Toast 내부 간격, Chip 좌우 padding |
| 11 | Button 상하 padding |
| 13 | Badge 아이콘-텍스트 간격 |
| 16 | Toast 내부 여백(4방향) |
| 20 | Button 좌우 padding, App Bar 여백/간격 |

## 범위에서 제외한 것

토큰 이름을 Gmarket처럼 티셔츠 사이즈(xxxs~xxxl)로 바꾸는 건 이번에 하지 않았다(사용자 확정 — 숫자 이름 유지). 8-Point Grid 개념 설명이나 Page/Template 단위의 상위 레이아웃 가이드(Gmarket 페이지의 "Page", "Template" 섹션에 해당)는 Figma Variables 자체가 표현하는 범위를 넘어서므로 다루지 않았다.

## 검증

- Spacing 컬렉션 재조회 결과 16개 항목의 이름·값·description이 계획대로 반영됨
- 이 컬렉션을 참조하는 노드가 원래 없었으므로(사용 0회) 화면에 미치는 시각적 영향 없음 — 실제 컴포넌트들은 여전히 하드코딩된 값을 쓰고 있어, 토큰 재바인딩은 별도 작업으로 남음

## 다음 단계 후보

- 실제 컴포넌트(Button, Item Card 등)의 padding/gap을 하드코딩 값 대신 이 Spacing 토큰에 바인딩 (PRD UR-S1 "모든 시각 속성은 토큰으로 정의" 요구사항 충족)

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | 실제 컴포넌트 사용처 조사 후 Spacing 값 5개 신규 추가, 전체 16개 역할 설명 작성 |

---

## 2026-09-01 — Spacing 문서화 페이지 신설 (기준 + 실제 컴포넌트 주석) (구 diagnosis/13)

# Figma 작업 완료 기록 — Spacing 문서화 페이지 신설 (기준 + 실제 컴포넌트 주석)

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**참고 레퍼런스** Wanted Design System — Spacing (사용자 제공 스크린샷)

---

## 배경

Color·Typography·Grid에 이어 Wanted의 Spacing 페이지를 참고해 "📏 Spacing" 페이지를 만들었다. Wanted는 (1) 간격 스케일을 룰러로 시각화하고 "4배수 권장" 원칙을 명시하는 "기준" 섹션, (2) 실제 UI에 정확한 padding/gap 값을 원형 배지로 주석 처리한 "훑어보기" 섹션으로 구성되어 있었다. 사용자가 "4배수 권장" 원칙을 우리 시스템에도 도입하기로 확정했다.

## 실행 내용

### 기준
16개 Spacing 값(2~64)을 막대 룰러로 시각화. **4의 배수 11개는 초록**, **예외 5개(2,6,10,11,13)는 주황**으로 구분. "예측 가능한 디자인 규칙 및 개발자와의 원활한 소통을 위해, 4배수 기준으로 간격을 구성하는 것을 권장합니다. 컴포넌트 특성상 불가피한 경우 2px 단위의 예외를 허용합니다." 라는 원칙을 명시.

### 적용 예시
실제 컴포넌트 4개를 clone해 정확한 위치에 원형 숫자 배지로 실측값을 주석 처리했다(값은 이번 세션에서 직접 조사, 임의 작성 없음).

| 컴포넌트 | 측정 | 값 | 구분 |
|---|---|---|---|
| Button (Large, Primary) | 좌우 padding | 20px | 4배수 |
| Button | 상하 padding | 11px | 예외 |
| Price Block | 할인배지↔판매가 간격 | 4px | 4배수 |
| Spec Row | 배송행↔적립행 간격 | 4px | 4배수 |
| Spec Row | 로켓배지 내부 아이콘↔텍스트 | 2px | 예외 |
| Toast | 내부 여백(4방향) | 16px | 4배수 |

## 검증

- 기준 섹션 스크린샷 — 16개 막대의 색상·라벨이 4배수/예외 분류와 정확히 일치
- 적용 예시 스크린샷 — 4개 컴포넌트 전부 실제 레이어를 clone한 것이며, 배지 위치·숫자가 조사한 실측값과 일치
- 페이지 전체 스크린샷으로 레이아웃 이상 없음 확인

## 다음 단계 후보

1. Pretendard 설치 후 Color·Typography·Grid·Spacing 4개 페이지 텍스트를 Pretendard로 일괄 교체
2. "4배수 권장" 원칙에 따라, 향후 신규 컴포넌트 제작 시 가급적 4배수 값을 우선 사용하고 예외는 의도적으로만 허용

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Spacing 문서화 페이지 신설 — 기준(4배수 룰러) + 실제 컴포넌트 4개 주석 예시 |

---

## 업데이트 — 컴포넌트 padding/gap 실제 Variable 바인딩 (2026-09-04, `design.md` 리뷰 중)

`design.md` 초안 검토 중 사용자가 "알려진 이슈"(padding/gap 숫자값은 Spacing 원칙과 일치하지만 실제 Variable에 바인딩돼 있지 않고 리터럴 값으로만 우연히 맞는 상태) 처리를 요청.

### 실행 내용
`① Component` 페이지의 41개 COMPONENT_SET 전체를 대상으로, 16개 Spacing 값(2/4/6/8/10/11/12/13/16/20/24/32/40/48/56/64) 중 하나와 정확히 일치하는 `paddingLeft/Right/Top/Bottom`·`itemSpacing`·`counterAxisSpacing`을 전수 스캔해 `setBoundVariable()`로 바인딩:
- 1차(각 variant 최상위 컨테이너): 420곳
- 2차(각 컴포넌트 내부 중첩 프레임까지 재귀 스캔): 960곳
- **총 1,380곳, 에러 0건**

### 제외한 것
재스캔 결과 남은 160곳은 전부 각 COMPONENT_SET **자체의 variant 갤러리 배치용 레이아웃**(Figma가 여러 variant를 나란히 보여주기 위해 쓰는 padding=20/itemSpacing=20 등, 실제 UI가 아님) — 의도적으로 바인딩 대상에서 제외.

### 검증
- Button/Chip/Item Card/Dialog 등 주요 컴포넌트 스크린샷으로 시각적 변화 없음(값은 그대로, 바인딩만 추가) 확인
- 전체 재스캔으로 실제 UI 요소 중 미바인딩 매치 0건 확인

## 변경 이력(추가)

| 일자 | 내용 |
|---|---|
| 2026-09-04 | 컴포넌트 라이브러리 전체 padding/gap 1,380곳을 Spacing Variable에 바인딩(41개 세트, 에러 0건) |
