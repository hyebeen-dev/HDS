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
