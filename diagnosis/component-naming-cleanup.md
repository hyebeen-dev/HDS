# Figma 작업 완료 기록 — 구조적 네이밍 중복 정리 (1단계: 기능적 위험 요소)

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`

---

## 배경

사용자가 "컴포넌트를 만들면서 네이밍을 체계적으로 못했고, 중복되는 이름도 많다"고 우려했다. 💠 Components 페이지(`1:9`) 전체를 read-only로 조사해 이름이 겹치는 컴포넌트 세트/프레임을 모두 찾고, 각 후보의 실사용 인스턴스 개수까지 확인한 뒤 두 종류로 분류했다.

- **A. 진짜 병합 대상**: 같은 이름 + 실질적으로 같은 역할인데 파일이 쪼개진 경우
- **B. 이름만 같고 실제론 다른 것**: 병합하면 오히려 정보 손실 — 이름만 구분

사용자가 "구조적 중복은 지금 바로, 표기 통일·전체 배치 정리는 체크리스트 완료 후 한 번에"로 시점을 확정해, 이번 세션에서는 A/B 케이스만 처리했다.

## 실행 내용

### A. Button("Buttopns") 세트 병합

기존 정식 Button 세트(`2167:15918`, 12 variants — 이번 세션 내내 확장해온 것)와는 별도로, "Line type gray, Size=Medium, State=Default" variant 2개가 완전히 동일한 이름으로 들어있는 leftover 세트(`6009:9822`)가 존재했다. 조사 결과 **정식 세트는 실사용 인스턴스가 0개**였고, **leftover 세트의 2개 variant는 각각 실제 목업(product_detail, Cart Item)에서 4개 인스턴스로 사용 중**이었다 — 즉 화면은 정식 세트가 아니라 leftover를 참조하고 있었다.

- 실사용 텍스트에 override가 없는 쪽(`6009:9856`, "재입고 알림 신청" — Cart Item 품절 상태에서 사용)을 정식 세트로 이동(`appendChild`). 이 노드에 연결된 인스턴스는 ID가 그대로라 별도 조치 없이 정상 동작.
- override가 있는 쪽(`6009:10882` → 리뷰 카드의 "도움이 돼요" 버튼 2개)은 `instance.swapComponent()`로 정식 variant(`6009:9856`)로 재연결 — override-preserving swap이라 텍스트("도움이 돼요")가 폰트 로드 없이 그대로 유지됨을 확인.
- 이동 과정에서 `Type=Line type gray`(소문자 g)가 기존 `Line type Gray`(대문자 G)와 별개 축 값으로 잡히는 걸 발견해 즉시 대소문자 통일(캐스팅 자체가 아니라 이번 병합이 새로 만든 충돌이라 바로 수정) — Type 축이 다시 깔끔한 5개 값(Primary/Secondary/Line type/Line type Gray/Text)으로 정리됨.
- 실사용이 없어진 `6009:10882`, leftover 세트(`6009:9822`)를 삭제. Figma가 마지막 variant 삭제 시 빈 COMPONENT_SET을 자동 삭제하는 것을 확인.
- 정식 세트 이름을 오타("Buttopns") → "Button"으로 수정.
- **GRID 레이아웃 이슈**: 정식 세트가 `layoutMode: 'GRID'`(4×4)라 병합된 variant가 자동으로 기존 "Primary/Large/Pressed"와 같은 셀(row0,col1)에 배치돼 겹쳐 보이는 문제 발견 → `setGridChildPosition(3, 2)`로 비어있는 셀(Line type Gray 행 옆)로 재배치해 해결.

### B. 이름만 같고 실제론 다른 컴포넌트 — 이름 구분

스크린샷으로 실제 내용을 확인한 뒤 아래와 같이 구분했다(병합하지 않음):

| 기존 이름 | 노드 | 실제 내용 | 새 이름 |
|---|---|---|---|
| icon | `6008:9076` | filter·star (실사용 11) | `Icon / Utility` |
| icon | `6020:13582` | 에러·경고 아이콘 | `Icon / Status` |
| icon | `6020:13544` | 체크박스 checked/unchecked | `Icon / Checkbox` |
| Sheets | `4043:2686` | Status=Default/Error/Warning 확인형 시트 | `Sheet / Confirmation` |
| Sheets | `6005:6973` | type=item/filter 선택형 시트 | `Sheet / Selector` |
| review | `6013:12529` | 평점 요약 위젯(Review_main) | `Review / Summary` |
| review | `6013:12578` | 리뷰 카드(photo/only text) | `Review / Card` |

또한 "dialog/ toast"라는 이름의 프레임이 3개 있었는데, 그중 2개는 실제로 dialog/toast와 무관한 내용(복제 후 이름을 안 바꾼 컨테이너)이었다:

- `6011:11358` — 실제 dialog+toast 컴포넌트, 이름 유지
- `6020:13372` — 실제로는 icon 세트 3개를 담고 있었음 → `Icon`으로 변경
- `6013:12073` — 실제로는 review + ai_review 세트를 담고 있었음 → `Review`로 변경

## 검증

- Button 세트 전체 스크린샷 — 13개 variant(신규 Medium 포함)가 겹침 없이 정상 렌더링, 새로 합친 "재입고 알림 신청" variant가 Line type Gray 행 옆에 올바르게 배치됨을 확인
- Cart Item(품절 상태) 스크린샷 — "재입고 알림 신청" 버튼이 병합 후에도 정상 표시됨을 확인
- Review 프레임 스크린샷 — "도움이 돼요" 버튼 2개가 swap 이후에도 텍스트 그대로 유지됨을 확인
- 삭제 전 항상 실사용 인스턴스 0개인지 재확인 후 삭제 진행

## 실행 중 발견한 것

- 이 Button 세트는 `layoutMode: 'GRID'`(Figma의 신규 그리드 오토레이아웃)를 사용 중이라, variant를 옮기거나 추가할 때 `x`/`y`를 직접 지정해도 무시되고 그리드 엔진이 재배치한다. 앞으로 이 세트에 variant를 추가할 땐 `setGridChildPosition(row, col)`으로 명시적으로 빈 셀을 지정해야 한다.
- Pretendard 폰트가 걸린 텍스트 노드라도, **`.characters`를 직접 쓰지 않고** `appendChild`(단순 재배치)나 `swapComponent`(override-preserving 스왑)만 쓰면 폰트 로드 없이도 안전하게 처리된다는 것을 확인했다 — 지난 세션들에서 겪은 폰트 블로커는 텍스트 내용을 직접 새로 쓰거나 스타일을 재할당할 때만 발생하는 것으로 재확인.

## 다음 단계 후보 (범위 밖 — 체크리스트 완료 후 한 번에)

1. ~~대소문자/오타 표기 통일 (`status=` vs `Status=`, `type=` vs `Type=` 등 나머지 케이스)~~ — **해결(2026-09-04)**, 아래 업데이트 참고
2. ~~`Property 1=Default` 같은 Figma 기본값 이름을 의미있는 이름으로 교체 (Icon / Status, Icon / Checkbox 세트 등)~~ — **해결(2026-09-04)**, 예시로 든 두 세트 정확히 포함해서 처리됨
3. `Review=Review3` 같은 placeholder 값 이름 교체 — 여전히 범위 밖(Review/Card의 한글 Variant 값 `한달사용`/`재구매`은 별도 감사 항목으로 남아있음, `design-system-audit-2026-09-04.md` P2)
4. 페이지 전체 배치(스크린 정리) — 여전히 범위 밖

## 업데이트 — Property 이름·표기 표준화 (2026-09-04)

Component Checklist가 사실상 마무리 단계(14/15)에 도달해, 위 1·2번 후속 과제를 전체 Design System 감사(`diagnosis/design-system-audit-2026-09-04.md`)의 일환으로 실행했다.

- **상태/선택 계열**(`status`/`Status`/`State`/`Property 1`로 흩어져 있던 20개 세트) → 전부 `Status`(Title Case)로 통일. Button-large/Icon Button의 `State`, radio의 `Property 1` 포함 약 90개 노드.
- **콘텐츠·글리프 종류**(상태가 아닌 것) → `Type`으로 재분류: `Icon / Utility`(filter/star/default), `Order Deadline`(Default/WithDiscount). `Sheet / Selector`·`roket_badge`의 소문자 `type`도 `Type`으로 통일.
- **`Icon / Status`**(예시로 들었던 바로 그 세트) — 실제로 깨져 있었음(2개 variant 모두 "Property 1=Default"). 빨간 원=Error/주황 삼각형=Warning으로 확인 후 이름 부여, 에러 해소.
- **`Icon / Checkbox`**(역시 예시로 들었던 세트) — 4개 variant 중 절반이 실제로는 라디오 아이콘이었음을 스크린샷으로 확인, `Icon / Checkbox`/`Icon / Radio`로 분리.
- **`Quantity Stepper`** — `Property 1`은 아니었지만 같은 유형(의미 불명 값)이라 함께 처리. −/+ 버튼의 실제 활성화 여부를 직접 조회해 `Default`/`MinReached`/`MaxReached`로 명명(예시값 1/5/999 자체는 힌트일 뿐 진짜 구분 기준이 아니었음).

전체 42개 세트 재스캔으로 에러 0건 확인. 범위 제외: `Section Header`의 `State`(별도 Boolean과 의미 중복 가능성, 새 발견으로만 기록), `ai_review`(값 1개), `menu`(하단내비 15개 값 구조 재설계는 별도 P2).

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Button 세트 병합(Medium variant 통합, 내부 중복 제거, GRID 배치 수정), icon/Sheets/review 세트 및 컨테이너 프레임 이름 구분 |
| — | 2026-09-04 | Property 이름 표준화(Status/Type 재분류), `Icon / Status` 버그 수정, `Icon / Checkbox`→`Icon / Checkbox`+`Icon / Radio` 분리, `Quantity Stepper` 실제 의미 확인 후 명명 |
