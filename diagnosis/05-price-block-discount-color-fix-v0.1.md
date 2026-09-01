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
