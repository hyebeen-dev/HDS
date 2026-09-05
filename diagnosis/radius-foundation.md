# Radius Foundation — 작업 기록

**작업일** 2026-09-04
**대상 Figma 파일** HDS_2609 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)

---

## 배경

`design.md` 초안 리뷰 중 사용자가 "Radius 공식 토큰 정의를 대화하면서 진행"을 요청. 기존엔 공식 토큰 없이 컴포넌트마다 개별 실측값만 존재하는 상태였다.

## 대화로 확정한 체계

사용자와 실측값을 하나씩 확인하며 5단계로 확정: **`none`(0) / `small`(4) / `medium`(8) / `large`(16) / `full`(pill·원형)**. 필요해지면 이후 `large` 상위(예: `xlarge`)를 계속 추가하는 open-ended 구조로 합의.

### 확정 과정에서 발견한 것
- **`Icon Button`이 8px 계열이 아니라 실제로는 완전 pill(radius 50)이었음** — 기존에 Button과 같은 8px 계열로 잘못 알고 있었던 것을 실측으로 정정.
- **`large`(16)의 실사용을 처음엔 놓쳤다가 사용자 확인으로 재발견**: `Dialog`, `Sheet / Confirmation`, `Sheet / Selector` **3개 컴포넌트 전부**의 `header` 프레임이 동일하게 상단 두 모서리만 16, 하단은 0(전형적인 바텀시트/다이얼로그 모달 형태).
- **`full` 계열이 컴포넌트마다 제각각 다른 "충분히 큰 값"으로 구현돼 있었음**: Chip=200, Radio=100, Icon Button=50, Search=100, Sheet 드래그 핸들=20, Item Card/Cart 아이콘=80, Search 내부 아이콘 래퍼=16(16×16 정사각형이라 사실상 원형). 전부 시각적 의도는 동일(완전 pill/원형)하므로 하나의 공유 토큰(999, 업계 관행대로 충분히 큰 고정값)으로 통일.

## 실행 내용

### 1. `Radius` Variable Collection 신규 생성 (5개 FLOAT 변수)

| 토큰 | 값 |
|---|---|
| `none` | 0 |
| `small` | 4 |
| `medium` | 8 |
| `large` | 16 |
| `full` | 999(모든 실사용 케이스보다 충분히 큰 값 — 실제 렌더링은 각 요소 최단변 절반에서 자동 클램프되므로 시각적 차이 없음) |

### 2. 전체 41개 COMPONENT_SET 전수 바인딩

Figma Plugin API에는 범용 `cornerRadius` bindable field가 없어 `topLeftRadius`/`topRightRadius`/`bottomLeftRadius`/`bottomRightRadius` 4개를 개별 바인딩(균일한 radius도 4개 전부 동일 변수로 바인딩).

- **0이 아닌 값**(4/8/16/20/50/80/100/200) 전수 스캔 후 바인딩: **1,238곳**
- **`none`(0)**: 무관한 요소(텍스트박스, 아이콘 래퍼 등)에도 기본값으로 광범위하게 깔려 있어 전체 스캔 대신, 실제로 의도적으로 "각짐"인 컴포넌트 최상위 컨테이너만 선별 바인딩 — `Category Tab`, `Item Card / Grid Small`, `Item Card / Cart`, `Action Bar`, `Dialog`, `Sheet / Confirmation`, `Sheet / Selector`, `Thumbnail`(XLarge variant만): **108곳**
- **합계 1,346곳, 에러 0건**

### 검증
- Chip/Button/Dialog/Search 스크린샷으로 시각적 변화 없음 확인(pill·모서리 형태 그대로 유지)
- 0이 아닌 값 기준 전체 재스캔으로 미바인딩 매치 0건 확인

## 최종 매핑표

| 토큰 | 값 | 해당 컴포넌트 |
|---|---|---|
| `none` | 0 | Category Tab, Item Card(Grid Small/Cart), Action Bar, Dialog 본체, Sheet 본체, Thumbnail XLarge |
| `small` | 4 | Button Small/XSmall |
| `medium` | 8 | Button Large/Medium, Thumbnail Small/Medium/Large |
| `large` | 16 | Dialog·Sheet Confirmation·Sheet Selector의 `header` 상단 모서리 |
| `full` | pill/원형 | Chip, Radio, Icon Button, Search, Sheet 드래그 핸들, Item Card/Cart 아이콘, Search 내부 아이콘 래퍼 |

## 다음 단계 후보
- `design-system/design.md` §1.5 Radius 섹션을 이 5단계 토큰 표로 갱신(design.md 전체 리뷰 완료 후 일괄 반영 예정)
- `design-language.md`의 Button radius 서술(Large 8/Medium 4)도 이 기록 기준으로 정정 필요(이미 Large=Medium=8, 즉 `medium` 토�큰 하나로 통합됨을 반영)

## 변경 이력

| 일자 | 내용 |
|---|---|
| 2026-09-04 | 최초 작성. 사용자와 대화로 5단계 Radius 체계 확정, `Radius` Variable Collection 신규 생성, 컴포넌트 라이브러리 전체 1,346곳 바인딩 |
