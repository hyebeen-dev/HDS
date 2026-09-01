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
