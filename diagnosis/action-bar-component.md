# Figma 작업 완료 기록 — Action Bar 컴포넌트 신규 생성 (Tier 1)

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**신규 컴포넌트** Action Bar (`6217:1012`)
**참고 레퍼런스** Wanted Design System — Action Area (node 16215:35516)

---

## 배경

Component Checklist(`diagnosis/component-checklist.md`) Tier 1 항목 중 하나. PRD FR-D6("구매 행동(CTA)은 화면 어디에서나 접근 가능하며, 항상 최상위 강조 단계를 점유한다")의 근거 컴포넌트.

## 레퍼런스 조사

Wanted의 "Action Area"는 하단 고정 "Actions"(버튼 행)와 그 위에 선택적으로 붙는 "Extra"(요약 정보/체크박스/칩 등) 영역으로 구성되며, 버튼 자체는 Strong/Neutral/Compact/Cancel 등 자체 Variant 체계를 갖고 있었다.

우리 프로젝트는 이미 Button 컴포넌트에 Primary/Large의 1버튼·2버튼 레이아웃이 실사용 텍스트("총 1개 상품 구매하기", "장바구니 담기"+"바로구매")까지 갖춰진 상태였고, PRD DG-3("Product List와 Product Detail이 공통 컴포넌트를 최대한 공유")에 따라 Wanted의 버튼 하위 체계를 새로 만들지 않고 **기존 Button을 그대로 재사용**하는 방향으로 범위를 좁혔다. Wanted의 "Extra" 영역(가격 요약 등)은 이번 범위에서 제외 — 필요해지면 후속 확장.

## 실행 내용

`Action Bar` COMPONENT_SET 신설, `Layout` 속성(`1 Button` / `2 Button`, 기존 Button의 Layout 값과 동일한 네이밍) 2개 variant:

- **Layout=1 Button**: 기존 `Type=Primary, Size=Large, Layout=1 Button` 버튼("총 1개 상품 구매하기")을 그대로 clone해 내장
- **Layout=2 Button**: 기존 `Type=Primary, Size=Large, Layout=2 Button` 버튼("장바구니 담기" + "바로구매")을 그대로 clone해 내장

컨테이너: 360px 폭, 흰 배경(`Static/0`), 상단 1px 구분선(`Color/Semantic/Background/Divider`), 16px 여백(상하좌우) — 내부 버튼은 FILL로 채워 컨테이너 폭에 맞춰 늘어나도록 구성.

## 검증

- 두 variant 모두 스크린샷으로 버튼이 컨테이너 안에서 올바른 여백·구분선과 함께 렌더링됨을 확인
- Component Checklist 섹션의 "Action Bar" 체크박스를 완료 표시(초록색)로 변경

## 다음 단계 후보

1. Wanted의 "Extra" 패턴처럼 버튼 위에 가격 요약/약관 동의 체크박스를 얹는 옵션이 필요해지면 Boolean 슬롯으로 후속 확장
2. Product Detail 화면 조립 시 실제로 하단에 고정 배치해 FR-D6가 실제로 해결되는지 재검증

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Action Bar 컴포넌트 신규 생성 (1 Button/2 Button, 기존 Button 재사용), 체크리스트 반영 |
