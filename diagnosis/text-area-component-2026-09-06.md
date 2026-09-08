# Text Area 컴포넌트 신규 등록

**작업일** 2026-09-06
**대상 Figma 파일** HDS_2609 (fileKey `y8OcE4JLKi7ADIPCVFJQej`), 신규 노드 `6998:6`("Text Area", `① Components` 페이지)
**작업 성격** Figma 실행(신규 컴포넌트 6개 variant 제작 + 기존 `Guide Row` 재사용) + `design-system/design.md`/`status.md`/`decisions.md` 문서 반영.
**계기**: 사용자가 상품 리뷰 작성처럼 많은 텍스트를 입력하는 화면에 대응할 컴포넌트가 필요하다고 요청, 실제 사용 예시 화면(`6801:4770`, "review-writing-screen")을 참고 자료로 제공.

---

## 1. 참고 화면 실측

`review-writing-screen`(`6801:4770`)은 App Bar("상품 품질 평가 남기기") + 한 줄 요약 입력 행(`title-input-row`, 이미 `Text Input`이 커버하는 영역) + **`review-textarea-container`**(`6801:4794`, 이번 작업 대상) + 리뷰 작성 유의사항 Section Header + 프로모션 배너로 구성된 화면이다.

`review-textarea-container` 실측 결과:
- 360px 전체 폭 프레임, **fills=[]/strokes=[]** — 배경·테두리 완전히 없음(borderless).
- placeholder 텍스트 하나만 존재: `Content=Empty` 상태, `body/small`(Pretendard Regular 14px, lineHeight 140%) 스타일, `Color/Semantic/Text/Description`(Gray/600, `#7b818e`) 색상.
- 좌우 위치는 화면 마진(x=16) 그대로 — 별도 내부 패딩 없음.
- 카운터, 커서, 에러 표시 등 다른 상태는 이 화면에 안 나타나 있었음(Empty+Default 딱 하나뿐).

## 2. 사용자 확인 사항

실행 전 두 가지를 확인:
1. **컴포넌트 구조**: `Text Input`의 새 변형이 아니라 **별개 컴포넌트 `Text Area`**로 결정 — 테두리 유무·화면 폭 전체 사용 여부가 근본적으로 달라 변형으로 묶으면 오히려 혼란스러움.
2. **상태 범위**: 참고 화면엔 Empty 하나뿐이지만, `Text Input`과 동일한 수준(Content×Status 전체 + Show Counter)으로 만들기로 확정.

## 3. 컴포넌트 설계 및 제작

**Variants**: `Content`(Empty/Filled) × `Status`(Default/Active/Disabled/Invalid) — 6개 조합(전체 8개 중 실제로 만든 건 6개, 자세한 목록은 design.md §2.17 참고) + `Show Counter`(Boolean, `Filled+Active` 마스터 하나에만 카운터 레이어 존재).

**상태별 시각 처리**:
- **Default**: placeholder만, 참고 화면 그대로.
- **Active**: placeholder/값 앞에 커서(caret) 추가 — `Text Input`의 caret 비주얼을 그대로 clone해 재사용, 스타일 일관성 확보.
- **Disabled**: `Text Input`은 테두리+배경을 함께 바꿔 비활성을 표현하지만, `Text Area`는 테두리 자체가 없어 그 방식을 못 씀. 대신 `Gray/50`(`Background/Background`) 배경 워시 하나만 추가 — 처음에 패딩(12px)+radius(8)를 함께 줬다가, 그러면 Default/Active 대비 텍스트 시작 위치가 오른쪽으로 밀려 정렬이 어긋나는 것을 스크린샷으로 확인하고 패딩·radius를 제거, 배경 워시만 남김(다른 Status와 완전히 동일한 정렬 유지).
- **Filled 예시**: 실제 상품 리뷰 톤의 더미 텍스트("배송도 빠르고 생각보다 품질이 좋아서...")로 채움, `Text/Primary` 색상.
- **Invalid**: 기존 `Guide Row`(§2.16에서 Text Input용으로 만든 아이콘+텍스트 결합 컴포넌트, Error variant)를 그대로 인스턴스로 재사용 — 새로 만들지 않고 기존 자산 재사용.
- **Show Counter**: "68/500" 형식의 우측 정렬 카운터 텍스트, `Gray/500` 색상, `Filled+Active` 마스터에만 실제로 존재하고 Boolean으로 visible 바인딩.

## 4. 실수 자체 교정 — 카운터를 별도 변형으로 만들려다 중단

처음에 "Show Counter 있는 예시"를 보여주려고 `Content=Filled, Status=Active`를 하나 더 복제해서 카운터를 추가하려 했으나, 이렇게 하면 **같은 축 조합(Content=Filled, Status=Active)에 이름만 같은 컴포넌트 2개**가 생겨 결합 시 충돌한다는 걸 바로 알아채고(지난 세션에 `Show Required Mark`/`Counter`/`Button`, `Content=Done` 때 반복했던 실수와 동일 패턴), 즉시 중단 — 복제본을 삭제하고 카운터 레이어만 기존 `Filled+Active` 마스터로 옮긴 뒤 Boolean으로 전환했다.

## 5. 검증

- `combineAsVariants`로 6개 컴포넌트를 `Text Area` 세트로 결합, `componentPropertyDefinitions` 에러 없음 확인.
- `Show Counter` Boolean이 `Filled+Active`의 카운터 레이어에 정상 바인딩됨을 속성 조회로 확인.
- `Guide Row`의 `Text` 속성으로 커스텀 안내문구("최소 10자 이상 작성해주세요.")를 설정한 직후 스크린샷에서 구값("guide text")이 보였으나, 속성·노드 상태를 직접 재조회한 결과 실제로는 이미 정상 반영돼 있었음 — 스크린샷 캐시 지연 패턴(이 파일에서 여러 번 확인된 것과 동일)으로 판단, 재스크린샷으로 최종 확인.
- 전체 6개 variant 그리드 스크린샷으로 회귀·정렬 문제 없음 확인, `① Components` 페이지의 다른 컴포넌트와 겹치지 않음을 좌표 비교로 확인.

## 6. 의도적으로 하지 않은 것 / 후속 과제

- 실제 화면에 배치할 때의 높이(참고 화면은 화면 남은 영역을 꽉 채우는 535px)는 마스터에 반영하지 않음 — 마스터는 콘텐츠에 맞춰 자동으로 늘어나는 HUG 방식이고, 실제 사용 시 인스턴스에서 고정 높이/`FILL`로 조정하도록 문서에 안내만 남김.
- `Invalid` 상태의 안내 문구는 `Text Input`과 동일하게 `Guide Row` 인스턴스를 직접 선택해 편집하는 방식 — Text Area 레벨 속성으로 문구를 노출하지 않음(Figma가 인스턴스 하위 레이어에 속성 참조를 못 건다는, 이미 확인된 제약과 동일한 이유).
- Content=Empty+Status=Invalid, Content=Filled+Status=Disabled 등 나머지 조합은 실사용 예시가 없어 이번에 만들지 않음(Text Input과 동일하게, 필요해지면 추가).

## 관련 문서

- `design-system/design.md` §2.17 Text Area
- `design-system/status.md` Component Checklist 17번째 항목
- `design-system/decisions.md` 2026-09-06 항목
