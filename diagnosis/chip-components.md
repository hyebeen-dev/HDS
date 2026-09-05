# Figma 작업 완료 기록 — Chip 컴포넌트 분리(Filter/Option) + Option Chips Selected 추가

**작업일** 2026-09-03
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**대상 컴포넌트** Filter Chips(`chip`, `2103:5367`), Option Chips(`Option Chips / Size`, `6373:23548`), Option Chips(`Option Chips / Thumbnail`, `6373:23662`)

---

## 배경

사용자가 기존 단일 Chip 컴포넌트를 용도에 따라 **Filter Chips**(기존 `chip` 세트, Status/Dropdown/Style/rocket 축을 가진 14 variant — 정렬·필터용)와 **Option Chips**(신규 `size` 세트 — 상품 옵션 선택용, 예: 용량 "240ml")로 분리했다(사용자 직접 작업). 이번 요청은 Option Chips에 `Selected`(Boolean) 속성을 추가해, 선택 시 `blue-500` 테두리가 표시되게 하는 것.

## 실행 전 발견한 문제 — Option Chips 세트가 에러 상태였음

`size` 세트의 `componentPropertyDefinitions` 조회 시 "Component set has existing errors" 발생. 원인은 3개 variant 중 2개가 **완전히 같은 이름**(`status=Default`)으로 등록돼 있었기 때문 — 이 프로젝트에서 여러 번 반복된 유형의 버그(`button_medium`의 `Variant4` 중복 등, `diagnosis/item-card-family-history.md` 참고).

스크린샷으로 실제 내용을 확인한 결과 이름만 같았을 뿐 서로 다른 콘텐츠였다:

| 노드 | 원래 이름 | 실제 내용 | 실사용 |
|---|---|---|---|
| `6373:23547` | `status=Default` | "240" + 빨간 "3개 남음"(재고 부족 안내) | 1곳(`6373:23549`) |
| `6373:23546` | `status=Default` | "240"만 | 0곳 |
| `6373:23545` | `status=sold_out` | "240" 회색(품절) | 0곳 |

**처리**: 사용자 확인 후 `6373:23547`을 `status=low_stock`으로 이름 변경(기존 `sold_out`과 동일한 소문자 스네이크 케이스 규칙, 실제 콘텐츠와 일치). `status` 축이 `Default`/`low_stock`/`sold_out` 3개 값으로 정리되며 에러 해소.

## Selected Boolean 구현 — 겪은 문제와 해결

### 1차 시도 실패: Boolean은 색상을 직접 못 켠다 (기존에 이미 확인된 제약)
Boolean 컴포넌트 속성의 `componentPropertyReferences`는 `visible`만 지원하므로, 파란 테두리 전용 오버레이 프레임(칩과 동일 크기, stroke만 파랑, fill 없음)을 자식으로 추가하고 그 `visible`을 `Selected`에 바인딩하는 방식을 택했다(이 프로젝트에서 이미 여러 번 쓴 "Boolean = 오버레이 표시/숨김" 패턴).

### 2차 문제 — 색상 토큰을 잘못 참조함
오버레이의 stroke 색상 Variable ID를 기존 칩의 회색 테두리가 참조하던 ID에서 그대로 복사해왔는데, 이는 실제로 파란색(`Primary/500`)이 아니라 **회색 테두리 색상 Variable**이었다. `Color/Primitive/Primary/500`(`#106def`)을 이름으로 직접 재조회해 올바른 Variable ID(`VariableID:2068:2531`)로 교체.

### 3차 문제 — 파란 오버레이가 색을 고쳐도 안 보임 (새로 발견한 Figma 렌더링 규칙)
색상을 바로잡았는데도 화면엔 여전히 기존 회색 테두리만 보였다. 원인을 진단하기 위해 오버레이 stroke를 일시적으로 8px로 키워 스크린샷을 찍어보니, **파란 테두리가 바깥쪽 일부만 살짝 보이고 안쪽은 여전히 가려져 있었다** — 이를 통해 **부모 프레임 자체의 stroke(자기 자신의 테두리 페인트)는 자식 레이어보다 항상 위에 그려진다**는 걸 확인했다(children 배치 순서와 무관하게, 부모 자체의 stroke는 최상단에 렌더링됨). 즉 부모가 자기 테두리를 그대로 갖고 있는 한, 그 안쪽에 아무리 자식 오버레이를 추가해도 절대 보이지 않는 구조였다.

**최종 해결**: 부모 프레임 자체의 stroke를 제거하고, 기존 회색 테두리도 별도의 자식 프레임(`default-border`, 항상 보임)으로 옮겼다. 최종 자식 순서는 `default-border`(맨 아래) → `text` → `selected-border`(맨 위, Selected일 때만 보임). 이제 둘 다 "자식"이라 일반적인 z-order 규칙(나중에 추가된 자식이 위)이 정상 적용되어, Selected=true일 때 파란 테두리가 회색 테두리 위에 정확히 겹쳐 보인다.

## 검증

- 3개 status(`Default`/`low_stock`/`sold_out`) × Selected(true/false) 총 6개 조합을 테스트 인스턴스로 만들어 스크린샷 확인 — 전부 정상: false는 기존 회색 테두리, true는 1px 파란(`Primary/500`) 테두리로 정확히 전환됨
- 기존 실사용 인스턴스(`6373:23549`, low_stock·Selected=false)가 이름 변경·구조 변경 후에도 시각적으로 그대로인지 확인
- 테스트 인스턴스 전체 삭제 완료

## 업데이트 — Selected 테두리 굵기 2px로 조정 (2026-09-03)

사용자 요청으로 `selected-border` 오버레이의 `strokeWeight`를 1px → 2px로 변경. 3개 status × Selected(true/false) 조합 재검증, 기존 실사용 인스턴스 영향 없음 확인 후 테스트 인스턴스 삭제.

## 업데이트 — 신규 Thumbnail 세트 정리 + Selected 추가, 두 세트 이름 통일 (2026-09-03)

Option Chips가 상품 유형별로 여러 종류로 늘어날 예정이라며, 사용자가 이미지 포함 신규 세트(`select`, `6373:23662`)를 추가하고 정리를 요청했다.

**발견한 문제**: 신규 세트도 3개 variant가 전부 `Property 1=Default`로 동일 등록돼 에러 상태였다. 스크린샷 확인 결과:

| 노드 | 실제 내용 |
|---|---|
| `6373:23660` | 신발 이미지 + "7CA 웜톤 브라운"(2줄 텍스트 예시), 재고 문구 없음 |
| `6373:23661` | 동일 + 빨간 "3개 남음"(재고 부족) |
| `6373:23659` | 동일 + "화이트"(1줄 텍스트 예시), 재고 문구 없음 |

`6373:23660`과 `6373:23659`는 실제 상태 차이 없이 예시 텍스트만 다른 진짜 중복이었다(둘 다 실사용 0건). `6373:23661`만 진짜 다른 상태(재고 부족).

**사용자 결정 및 처리**:
- 세트 이름: `select` → **`Option Chips / Thumbnail`**. 패밀리 확장에 대비해 기존 `size`도 **`Option Chips / Size`**로 함께 통일(프로젝트 기존 `/` 네이밍 관례와 일치, Assets 패널에서 "Option Chips" 폴더로 묶임)
- 중복 Default: "화이트"(`6373:23659`) 삭제, "7CA 웜톤 브라운"(`6373:23660`)을 `status=Default`로, `6373:23661`을 `status=low_stock`으로 이름 변경 — `Option Chips / Size`와 동일한 `status: Default/low_stock` 축 규칙
- `Selected`(Boolean) 추가 — 위에서 확립한 `default-border`+`selected-border` 자식 오버레이 패턴 그대로 재사용, `Primary/500` 2px(Size 세트와 통일)

**검증**: `Default`/`low_stock` × Selected(true/false) 4개 조합 테스트 인스턴스로 확인 — 전부 정상. 기존 실사용 인스턴스(`6373:23549`) 영향 없음 확인 후 테스트 인스턴스 삭제.

## 다음 단계 후보

- Filter Chips(`chip`, `2103:5367`)는 이미 자체 `Status=Selected` variant를 갖고 있어 이번 작업과 별개 — 두 Chip 계열의 "선택됨" 표현 방식이 서로 다르다는 점(Filter는 Variant 축, Option은 Boolean+파란 테두리)은 의도된 차이인지 사용자 확인 필요
- `Option Chips / Thumbnail`의 `sold_out`(품절) 상태는 아직 없음 — `Size` 세트처럼 필요해지면 추가
- Option Chips가 앞으로 상품 유형별로 더 늘어날 예정이라고 확인됨 — 신규 세트 추가 시 `Option Chips / {유형}` 네이밍과 `status: Default/low_stock(/sold_out)` + `Selected` Boolean 패턴을 그대로 따르면 됨

## 변경 이력

| 일자 | 내용 |
|---|---|
| 2026-09-03 | 최초 작성. Option Chips 중복 variant 이름 수정(`status=low_stock`), `Selected` Boolean 추가(파란 1px 테두리), 부모 프레임 stroke가 자식보다 항상 위에 렌더링되는 규칙 발견 및 우회 |
| 2026-09-03 | Selected 테두리 굵기 1px → 2px로 조정 |
| 2026-09-03 | 신규 Thumbnail 세트(`Option Chips / Thumbnail`) 정리 + Selected 추가, 기존 `size` 세트를 `Option Chips / Size`로 이름 통일 |
