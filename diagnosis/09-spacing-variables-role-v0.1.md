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
