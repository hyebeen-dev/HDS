# Text Input 컴포넌트 통합 작업 기록

**작업일** 2026-09-06
**대상 Figma 파일** HDS_2609 (fileKey `y8OcE4JLKi7ADIPCVFJQej`), 노드 `6800:8767`("Text Input", `① Components` 페이지)
**작업 성격** Figma 실행(색상·텍스트·radius 재바인딩, property/value 리네임, 컴포넌트 신규 생성 1개, 아이콘 인스턴스 교체) + `design-system/design.md`/`status.md`/`decisions.md` 문서 반영.
**계기**: 사용자가 다른 Figma 파일에서 만들어둔 Text Input 컴포넌트를 이 파일에 복사(`6800:8767`)해왔고, `diagnosis/external-comparison-airbnb-2026-09-06.md` 감사에서 Text Input이 최우선 문서화 공백으로 지목된 직후였다. 계획 파일: `~/.claude/plans/figma-file-text-valiant-raven.md`.

---

## 1. 구조 확인

`get_metadata` 조회 결과 `6800:8767`은 이미 정식 `COMPONENT_SET`(26개 실제 `COMPONENT` 자식)이었다 — Figma 파일 간 복사 시 흔히 "낱개 COMPONENT가 프레임에 담긴 것뿐"인 경우가 있어 우려했으나, 이번엔 그렇지 않아 "Combine as Variants" 작업이 불필요했다.

## 2. Property / Value 이름 정리

`design.md §3` 규칙("상태 축은 항상 `Status`", Boolean은 `Show {명사}` 접두사)과 원본 이름이 충돌해 교환 매핑했다.

| 원본 | 원본 값 | → 최종 | 최종 값 |
|---|---|---|---|
| `Status`(콘텐츠 채움) | Empty/Filled/Done | `Type` | Empty/Filled/Done |
| `State`(인터랙션/검증) | Active/Default/Disabled/Invalid | `Status` | Active/Default/Disabled/Invalid |
| `Helpertext` | False/1 line/2 line | `Helper Text` | None/OneLine/TwoLine |
| `Required` | True/False | `Show Required Mark` | True/False |
| `Counter` | True/False | `Show Counter` | True/False |
| `Masking` | False/True/Label | `Masking` | None/Masked/MaskedWithLabel |
| `Button` | True/False | `Show Button` | True/False |

`editComponentProperty`로 키를 리네임하면 26개 자식 컴포넌트의 이름(variant 값 조합)이 자동으로 따라 바뀐다는 것을 확인했다 — 손으로 개별 레이어를 고칠 필요가 없었다. Value 문자열(Helper Text/Masking)은 각 자식의 `name`을 파싱해 세그먼트 단위로 치환하는 방식으로 26개 전부 일괄 처리했다.

## 3. 색상 재바인딩 (fill 164 + stroke 31 = 195곳)

`get_variable_defs`로 확인한 원본의 외부(원본 파일) 참조와 로컬 대응 토큰:

| 원본 참조 | hex | → 로컬 토큰 |
|---|---|---|
| `color/light/gray/90` | #34373D | `Color/Primitive/Gray/900` |
| `color/light/gray/50` | #969CA6 | `Color/Primitive/Gray/500` |
| `color/light/gray/30` | #CDD0D5 | `Color/Primitive/Gray/300` |
| `color/light/gray/10` | #EEEFF1 | `Color/Primitive/Gray/100`(정확 일치) |
| `color/light/gray/5` | #FAFAFA | `Color/Primitive/Gray/50`(#f4f6f6, 정확 일치 없어 최근접 스냅) |
| `color/light/primary/50` | #106DEF | `Color/Semantic/State/Primary` |
| `color/light/error/40` | #E23636 | `Color/Semantic/Feedback/Error` |
| `color/light/error/50` | #C91D1D | `Color/Primitive/Error/500`(정확 일치) |

매칭은 변수 **이름이 아니라 실제 렌더링된 hex**로 수행했다(이름이 같아도 값이 다른 경우가 있었기 때문 — 아래 4번 참고). 스크립트로 195곳을 일괄 처리했고, 최초 패스에서 인증 버튼("인증하기") 내부 인스턴스 노드 1곳(`I6800:9104;2168:8257`)이 `#fafafa`로 남아있는 것을 재조회로 발견해 별도로 마저 처리했다. 최종적으로 `get_variable_defs` 재조회 결과 외부 참조 0건.

## 4. 텍스트 스타일 재바인딩 (86곳)

`getStyledTextSegments`로 각 텍스트 노드의 실제 폰트 스펙(family/style/size/lineHeight)을 읽어, 로컬 스타일 목록과 스펙으로 매칭했다(이름 매칭 아님). 원본의 `text/body/small`(15px/Regular/140%)은 이름만 보면 로컬 `text/body/small`(14px)과 헷갈리지만 실제로는 로컬 `text/body/compact`(15px/Regular/140%)와 일치하는 "false friend" 사례였다.

적용 결과: `text/label/xsmall-medium` 49곳, `text/body/compact` 22곳, `text/underline/xsmall` 13곳, `text/heading/xxsmall` 2곳. 86개 전부 단일 세그먼트로 정확히 매칭됐고 미해결(unresolved) 0건.

**15px Bold 스타일**: 처음엔 로컬에 없는 값이라 신규 스타일(`body/compact-bold`) 생성을 검토했으나, 로컬 스타일 전체 목록을 실측 조회한 결과 `text/heading/xxsmall`(Pretendard Bold 15px 150%)이 스펙까지 정확히 동일함을 발견 — 사용자에게 확인 후 새로 만들지 않고 재사용하기로 전환했다.

## 5. Radius 재바인딩 (68곳)

`text box`(입력창, radius 8) → `radius/medium`, 아이콘 원형 wrapper(radius 16)·커서(radius 10) → `radius/full`로 전부 바인딩. 4모서리(`topLeftRadius` 등) 개별 바인딩 방식 사용.

## 6. guide text 아이콘 정식화

guide text(도움말) 아이콘이 raw 도형(Ellipse+Vector 또는 Ellipse+Rectangle×2)으로 11곳에 중복돼 있었다. 구조로 성공/에러 톤을 구분(`Ellipse,Icon`=성공, `Ellipse,Rectangle 4383,Rectangle 4384`=에러)해 각각 인스턴스로 교체했다.

- 에러 톤(6곳): 기존 `Icon / Status / Error`(`6020:13581`) 재사용.
- 성공 톤(5곳): 원본에 이미 있던 파란 체크 글리프를 그대로 승격해 신규 `Icon / Status / Success`(`6810:8342`, 16×16, `Icon` 페이지) 컴포넌트로 생성 — description에 용도 기록.

Dialog·`Item Card / Cart`가 고아 컴포넌트("x-01")를 잘못 참조 중인 기존 미해결 이슈(`design.md §5`)는 사용자 확정대로 이번 범위에 포함하지 않았다.

## 7. 컴포넌트 정리

- 이름을 "Text Fields" → `Text Input`으로 변경.
- `① Components` 페이지 기존 컴포넌트들 옆으로 위치 재배치(기존 최우측 컴포넌트 기준 +160px).

## 8. Masking=MaskedWithLabel 프로토타입

원본에 `Masking=Label` 조합이 1개(`Type=Empty`) 있었으나 입력값이 없어 실제 렌더링을 특정할 수 없었다. 사용자 확인 결과 "`Masked`와 동일하게 점으로 가리되 옆에 추가 인디케이터가 붙는다"로 방향은 확정됐고, 정확한 형태는 프로토타입(점 6개 + 회색 pill "라벨" 태그)을 만들어 스크린샷으로 보여준 뒤 잠정 승인받았다. 최종 비주얼(태그 문구/아이콘/위치)은 `design.md §5` 후속 과제로 남겼다.

## 9. 의도적으로 하지 않은 것

- **Spacing Variable 바인딩** — itemSpacing(2, 4)과 padding 값 자체는 이미 기존 토큰과 일치해 시각적 문제는 없으나, 이번 범위에서 Variable로 바인딩하지는 않았다(`design.md §5` 후속 과제).
- **Icon/Status 고아 컴포넌트 이슈 확장 해결** — Dialog·Item Card/Cart의 기존 문제는 사용자가 이번 범위에서 제외하기로 확정.
- **Size Medium/Small 추가** — 복사본에 Large만 있어 이번엔 Large만 정식화(open-ended 구조로 유지).

## 10. 후속 수정 — Show Required Mark/Show Counter/Show Button을 진짜 BOOLEAN으로 재구성

사용자가 "properties가 variant 밖에 없는데 성격상 boolean도 있어야 할 것 같다"고 지적해 확인한 결과, 실제로 이 세 속성은 이름만 `Show {명사}` 규칙을 따르고 있었고 구현은 전부 VARIANT 축(True/False별로 별도 COMPONENT)이었다 — `Button`의 `Show Leading Icon`/`Show Trailing Icon`(진짜 BOOLEAN)과 다른 방식.

**조사 중 발견한 별개 버그**: `componentPropertyDefinitions`를 읽으려 하면 `"Component set has existing errors"` 에러가 났다. 원인은 27개 자식 중 2개(`6800:9105`, `6802:10160`)가 이름·내용 모두 완전히 동일한 진짜 중복이었기 때문 — `6802:10160`을 삭제해 해소(26개로 복원). 정확한 발생 경위는 확인하지 못했으나, 지난 세션 대량 rename 스크립트 중 하나에서 우연히 생긴 것으로 추정된다.

**병합 절차**: 남은 26개를 `(Type, Status, Helper Text, Masking)` 4축 조합으로 그룹핑한 결과 11개 그룹(6개는 2~5개 멤버, 5개는 단일 멤버). 멤버가 여럿인 그룹은 Show Required Mark/Show Counter/Show Button 값만 다른 형제였다 — 각 그룹에서 레이어가 가장 많이 존재하는 멤버를 생존시키고 나머지를 삭제(26→11개). 유일하게 "Empty/Default/None/None" 그룹은 req+counter를 가진 멤버와 button을 가진 멤버가 서로 달라(3속성을 동시에 가진 멤버가 원래 없었음), button 구조가 더 복잡해 button 보유 멤버(`6800:9088`)를 생존시키고 그 위에 `*`/`0/30` 레이어를 다른 멤버(`6800:8826`)에서 복제해 보강했다.

**Boolean 정의**: 병합 후 모든 자식 이름에서 `Show Required Mark=`/`Show Counter=`/`Show Button=` 세그먼트를 제거하자 해당 축들이 componentPropertyDefinitions에서 자동으로 사라짐을 확인(이름 기반 파생이라는 Figma의 특성). `addComponentProperty`로 동일한 이름의 진짜 BOOLEAN 3개(`defaultValue: false`)를 새로 정의하고, 각 레이어(`*` TEXT, `0/30` TEXT, `Button` INSTANCE)의 `componentPropertyReferences.visible`을 연결했다.

**레이아웃 버그 발견·수정**: 테스트 인스턴스로 `Show Button=false`를 확인한 결과 입력창이 328px 전체 폭으로 늘어나지 않고 244px로 남아 오른쪽에 빈 공간이 생겼다. 원인은 버튼을 감싸는 행(`Frame 1430106331`)의 `primaryAxisSizingMode`가 `AUTO`(hug)라, 버튼이 숨겨지면 행 자체가 남은 콘텐츠 폭(244)으로 쪼그라들었기 때문. 행을 `FIXED` 328px로 고정하고, 안쪽 `Frame 1430106330`과 `text box`를 `layoutSizingHorizontal: FILL`로 바꿔 버튼이 없을 때 자연스럽게 전체 폭을 채우도록 수정. 두 마스터(`6800:9088`, `6800:9096`) 모두 적용, 테스트 인스턴스로 true/false 양쪽 다 재확인 후 테스트 인스턴스는 삭제.

**Status/Masking 기본값 정리 시도**: 병합 과정에서 COMPONENT_SET의 `Status`/`Masking` VARIANT 속성 `defaultValue`가 각각 `Active`/`MaskedWithLabel`로 바뀌어 있었다(원치 않는 값이 첫 자식으로 남은 결과로 추정). `editComponentProperty`로 `Default`/`None`으로 되돌리려 했으나 Figma API가 VARIANT 속성의 `defaultValue`는 직접 변경할 수 없다는 제약(`Cannot change defaultValue of a variant property`)을 확인 — 자식 순서 재배치가 필요한 작업이라 이번 범위에서는 보류(영향은 Assets 패널에서 처음 보이는 미리보기 조합이 바뀌는 정도로 경미).

## 11. 후속 수정 — 트레일링 버튼을 로컬 Button 컴포넌트로 재연결

사용자가 "Show Button에 쓰이는 버튼은 Button 컴포넌트랑 연결되어야 한다"고 지적해 확인한 결과, 트레일링 버튼("인증하기") 인스턴스 2곳(`6800:9095`, `6800:9104`)이 우리 로컬 `Button`(`2167:15918`)이 아니라 **원본 복사본의 REMOTE 컴포넌트**(`getMainComponentAsync().remote === true`, 소속 세트 `6800:8728`, `Type`/`Size`/`State`/`Layout` 축 — 우리 Button과 다른 구조, 우리 파일 어느 페이지에도 실체가 없는 진짜 원격 참조)를 참조하고 있었다. 이전에 겪은 "원격 스타일 참조"(Quantity Stepper) 문제가 컴포넌트 단위로 재발한 것.

**수정**: 두 인스턴스를 로컬 `Button`의 `Type=Primary, Size=Medium, Status=Disabled`(`6474:33403`)/`Status=Default`(`6474:33380`)로 `swapComponent()` — remote 쪽도 `Type=Primary`였어서 의미는 그대로 보존.

**swap 중 발견한 부작용 2건**:
1. 우리 `Button`은 기본적으로 Leading/Trailing 아이콘 슬롯이 켜져 있어(placeholder 점선 사각형), swap 직후 텍스트 양옆에 빈 아이콘 박스가 나타났다 — `Show Leading Icon`/`Show Trailing Icon`을 인스턴스에서 false로 꺼서 정리(폭 144→92px).
2. `swapComponent()` 직후 두 인스턴스 중 하나(`6800:9104`)의 `visible`이 `false`로 바뀌어 있었다 — `componentPropertyReferences.visible`(`Show Button` 바인딩) 자체는 끊기지 않았지만, swap이 그 프로퍼티의 "현재 해석된 값"을 초기화하는 부작용이 있는 것으로 보인다. 두 인스턴스 모두 `visible = true`로 재설정해 바로잡았다. **향후 컴포넌트 swap을 할 때는 swap 전/후로 `componentPropertyReferences`가 걸린 레이어의 `visible` 상태를 항상 재확인해야 한다.**

텍스트("인증하기")는 swap 전후로 정상 보존됐다. `Show Button` on/off 리플로우(지난 세션에 고친 328px 전체 폭 확장)도 재검증 결과 회귀 없음을 확인.

## 12. 후속 수정 — `Type`을 `Content`로 개명

사용자가 `Type`(Empty/Filled/Done)과 `Status`(Default/Active/Disabled/Invalid)가 서로 겹쳐 보인다고 지적했다. 확인 결과 두 축의 **값 자체는 겹치지 않았다**(8개 값 전부 서로 다름) — 문제는 둘 다 "필드의 상태"처럼 읽혀서 `Type`이라는 이름이 실제 의미와 안 맞았던 것.

원인 추적: 원본 복사본의 두 축(`Status`=콘텐츠 채움 Empty/Filled/Done, `State`=인터랙션 Active/Default/Disabled/Invalid)을 "상태 축은 항상 `Status`" 규칙에 맞춰 교환할 때, 인터랙션 축은 `Status`로 옮기고 콘텐츠 채움 축은 남은 이름인 `Type`을 그냥 붙였다. 이 시스템에서 `Type`은 원래 Button의 Primary/Secondary처럼 "서로 다른 종류(kind)"를 뜻해왔는데, Empty/Filled/Done은 종류가 아니라 콘텐츠 채움 미리보기 상태라 `Type`이라는 이름과 실제 뜻이 어긋났다.

**수정**: `editComponentProperty`로 `Type`→`Content` 리네임(값은 그대로, 11개 자식 이름 자동 갱신, 에러 없음 확인). `design.md §3`에 "콘텐츠 축이 상태처럼 읽히면 `Type` 대신 더 구체적인 이름을 쓴다"는 예외 규칙을 각주로 추가했다.

## 13. 후속 수정 — 트레일링 버튼 활성화 여부: `Content=Done` 삭제로 정리

사용자가 트레일링 버튼의 활성/비활성도 Boolean으로 가져가고 싶다고 요청했다. 확인 결과 `Content=Done`(11개 변형 중 `6800:9096` 단 1곳)이 그 역할을 몰래 하고 있었다 — 화면상 `Content=Empty`(다른 버튼 있는 변형 `6800:9088`)와 완전히 동일하고, 유일한 차이는 내부 `Button` 인스턴스가 `Status=Default`(활성)냐 `Status=Disabled`(비활성)냐뿐이었다.

**검토한 접근과 기각 사유**: 처음엔 `Button Enabled` 같은 새 BOOLEAN을 만들어 중첩 Button의 Status를 스위칭하는 걸 고려했으나, 이 프로젝트가 2026-09-01 Item Card 작업에서 이미 확인한 제약 — Figma `componentPropertyReferences`는 `visible`/`characters`/`mainComponent` 세 필드만 지원 — 이 여기도 그대로 적용돼, Boolean 하나로 중첩 컴포넌트의 Variant 선택 자체를 바꾸는 건 애초에 불가능했다. (두 개의 Button 인스턴스를 겹쳐놓고 각각의 `visible`을 반대로 바인딩하는 우회법도 검토했으나, Figma가 "반대(NOT)" 바인딩을 지원하지 않아 실질적으로 하나의 깔끔한 Boolean으로 귀결되지 않는다는 결론.)

**실제 조치**: `6800:9096`(`Content=Done`) 삭제, `6800:9088`(`Content=Empty`) 하나만 생존 — `Content`가 자연히 `["Empty","Filled"]` 2개 값으로 단순화됨(11→10개 변형, 에러 없음, 스크린샷으로 시각적 회귀 없음 확인). 트레일링 버튼의 활성/비활성은 Text Input 자체 속성이 아니라 **중첩된 `Button` 인스턴스를 직접 선택해 그 `Status`를 바꾸는 것**으로 처리하기로 하고, 이 사용법을 `design.md §2.16` 사용 가이드에 명시했다 — 이건 이미 오늘 로컬 Button으로 재연결한 덕분에 지금도 가능한 조작이라, 별도 구현이 필요 없었다.

## 14. 후속 시도(실패) — guide text 성공/에러 톤을 독립 Type으로 분리

사용자가 guide text 아이콘의 성공/에러 톤을 Boolean이 아닌 별도 Type(Success/Error)으로 분리하고 싶다고 요청했다. 당시 상태: 톤이 100% `Status`에 종속(Active+Helper Text≠None→`Icon / Status / Success`, Invalid→`Icon / Status / Error`, 실측 6개 인스턴스 — Invalid 계열 4곳, Active 계열 2곳).

**시도**: Text Input `COMPONENT_SET`에 `Guide Type`(INSTANCE_SWAP, preferredValues=[Success, Error], defaultValue=Success)을 신규 추가하고, 6개 기존 아이콘 인스턴스 전부의 `componentPropertyReferences.mainComponent`를 이 키로 바인딩(현재 값을 유지하려는 의도).

**발견한 제약**: 바인딩 직후 같은 스크립트 내 조회에서는 각 인스턴스의 `mainComponent`가 올바르게 유지된 것처럼 보였으나, 실제 스크린샷을 찍어보니 **전부 하나의 값(Error)으로 통일**돼 있었다. 이후 각 인스턴스에 직접 `.mainComponent = 올바른 마스터`를 재할당해 바로잡으려 했으나, 재조회 결과 이번엔 **전부 다른 값(Error)으로 다시 통일**돼 있었다 — 즉 여러 variant(컴포넌트)에 걸쳐 같은 컴포넌트 속성을 참조시키면, Figma가 variant별로 독립된 가로 값을 유지하지 못하고 마지막에 적용된 값 하나가 전체에 전파되는 것으로 보인다. `visible`(Boolean) 바인딩 때는 각 마스터가 자기 값을 유지했던 것과 대조적이다.

**복구**: `deleteComponentProperty`로 `Guide Type` 속성 자체를 삭제(모든 레이어의 바인딩이 함께 해제됨), 이어서 6개 아이콘 인스턴스 각각을 `componentPropertyReferences={}`로 초기화하고 부모 variant의 `Status`(Invalid/Active)에 따라 올바른 마스터(Error/Success)로 직접(바인딩 없이) 재할당. 스크린샷으로 원래 상태와 완전히 동일하게 복원됐음을 확인.

**결론**: 사용자와 논의 후, 이 톤을 독립 속성으로 분리하는 건 포기하고 현재의 `Status` 종속 구조를 유지하기로 함. `design.md §2.16`에 이 종속 관계와, 다른 조합이 필요하면 중첩 아이콘 인스턴스를 직접 교체하면 된다는 가이드를 추가했다. Variant 축으로 분리하는 대안도 검토했으나, 실제로 한 번도 함께 쓰인 적 없는 조합(예: Active+Error)까지 만들어야 하는 부담이 있어 채택하지 않았다.

## 15. 후속 수정 — `Masking`을 `Input Type`(Default/Masking/Number)으로 재구성

사용자가 입력값의 종류(기본 텍스트/비밀번호/숫자)도 variant로 고를 수 있으면 좋겠다고 제안했다. 확인 결과 기존 `Masking`(None/Masked/MaskedWithLabel)과 개념이 겹쳐, 별개 축을 추가하는 대신 `Masking`을 대체하기로 사용자와 합의했다.

**처리 내역**:
1. 아직 최종 비주얼이 확정 안 됐던 `MaskedWithLabel` 변형(`6800:9072`, 점+"라벨" pill 태그 프로토타입 포함) 삭제 — 같은 축 조합(Content=Empty/Status=Active/Helper Text=None)의 `Default`(구 `None`) 버전(`6800:8880`)이 이미 따로 있어 삭제해도 충돌 없음.
2. `editComponentProperty`로 `Masking` → `Input Type` 리네임(9개 자식 이름 자동 반영).
3. 9개 자식의 값 세그먼트를 `None`→`Default`, `Masked`→`Masking`으로 문자열 치환.
4. `Number` 값 추가 — 대표 변형(`6800:8880`)을 `clone()`해 이름만 `Input Type=Number`로 바꾼 새 컴포넌트(`6887:2`) 생성. 사용자 확인대로 Figma 상에서는 `Default`와 시각적으로 완전히 동일(실제 키보드 타입 차이는 구현 단계에서만 의미가 있음).

**결과**: `Input Type` variantOptions = `["Default","Masking","Number"]`, 총 10개 변형(9개 리네임 + 1개 신규), 에러 없음, 스크린샷으로 전체 회귀 없음 확인. `design.md §5`의 "MaskedWithLabel 라벨 태그 최종 비주얼 확정" 백로그 항목도 자연히 해소(대상 자체가 삭제됨).

## 16. 후속 수정 — 누락된 기본 조합 복원 + 유령 중복 3번째 재발

사용자가 `Number`를 Figma에서 직접 삭제한 뒤 두 가지를 지적했다: (1) `Content=Empty`를 고르면 마치 `Disabled`인 것처럼 보인다 (2) `Status=Default`를 고르면 항상 트레일링 버튼이 딸려온다.

**원인 확인**:
- `Content=Empty, Status=Active`(빈 칸에 포커스만 된 상태) 변형이 어느 시점엔가(아마 `Number` 삭제와 함께) 통째로 없어져 있었다 — 남은 `Empty` 예시는 `Disabled`/`Default` 뿐이라 실제로 "Empty=Disabled"처럼 보이는 상황이었다.
- `Status=Default`의 유일한 예시(`6800:9088`)가 트레일링 버튼("인증하기")을 항상 포함하고 있어, `Default` 상태 자체와 버튼 유무가 우연히 묶여 보였다.

**유령 중복 3번째 재발**: 같은 확인 과정에서 `6802:10613`(`6800:8867`의 완전한 복제)과 `6802:10627`(`6800:9026`의 완전한 복제)을 발견했다. 이번엔 내 스크립트 실행 중이 아니라 **사용자가 Figma에서 직접 속성을 조작하던 중**에 생긴 것으로 보인다 — 이 COMPONENT_SET이 어떤 방식으로 편집되든(스크립트든 수동이든) 자식을 통째로 복제하는 고질적인 문제가 있다는 뜻이다. 지금까지 3번(첫 번째는 이 문서 §후속수정 초반부 참고) 발생했고 정확한 트리거 조건은 여전히 특정하지 못했다. 두 중복 모두 삭제해 정리했다.

**실측 기반 재구성**: `Content=Empty, Status=Active` 변형이 없어져 있어 실측 토큰을 다시 확인한 뒤 재구성했다.
- `Empty/Disabled`(`6800:8773`): 배경 `Gray/100`(#eeeff1) 채움, 테두리 `Gray/300`(#cdd0d5), 커서 없음.
- `Empty/Default`(`6800:9088`): 배경 없음, 테두리 `Gray/300`(#cdd0d5), 커서 없음.
- `Filled/Active`(`6800:8867`): 배경 없음, 테두리 `Gray/500`(#969ca6, 다른 상태보다 한 단계 진함), 커서 있음(`State/Primary` 파랑).

`6800:8773`(구조가 가장 단순한 Empty)를 clone해 Status를 Active로 바꾸고, 테두리를 `Gray/500`로, 배경을 투명으로 바꾼 뒤 `6800:8867`의 커서를 복제해 추가, placeholder 텍스트 위치를 커서와 겹치지 않게 조정(x=18)했다.

**`Status=Default`의 버튼 문제 처리**: 처음엔 "버튼 없는 버전"을 새 변형으로 만들려다가, 그러면 **같은 축 조합(Content=Empty, Status=Default, Helper Text=None, Input Type=Default)에 이름만 다른 두 컴포넌트가 생겨** 이번 세션 내내 반복해서 고쳐온 문제(Show Required Mark/Counter/Button 때, Content=Done 때와 동일한 유형)를 스스로 재생산하는 것임을 깨닫고 중단했다. 대신 **기존 마스터(`6800:9088`) 자체의 `Button` 인스턴스 `visible`을 `false`로 꺼서 "버튼 없는 게 기본 모습"이 되도록** 수정했다 — `Show Button` Boolean으로 필요할 때 켤 수 있는 능력은 그대로 유지된다. 텍스트박스 폭이 자동으로 328px(전체 폭)로 리플로우되는 것도 확인(지난 세션에 고친 auto-layout FILL 동작이 정상 작동).

**검증 이슈**: 전체 컴포넌트 세트를 한 번에 스크린샷 찍으면 새로 만든/수정한 변형 부근에서 텍스트가 겹쳐 보이는 현상이 또 나타났다(이전 `Number` 작업 때와 동일 패턴) — 그러나 `6800:9088`, `6915:4155` 각각을 개별로 스크린샷 찍으면 완전히 깨끗하게 나온다. **결론: 이 컴포넌트를 검증할 때는 전체 그룹 스크린샷이 아니라 개별 노드 스크린샷을 신뢰 기준으로 삼는다** — 그룹 스크린샷의 겹침은 캐시 지연으로 보이며 실제 파일 상태를 반영하지 않는다.

**결과**: 9개 변형, 에러 없음, 중복 이름 0건.

## 17. 후속 수정 — Helper Text 줄별 독립 Success/Error 속성 추가

사용자가 "OneLine=Success, TwoLine=Error로 나온다"고 지적했다. 확인 결과 톤이 줄 수 자체에 종속된 게 아니라, **`Helper Text=TwoLine` 조합이 파일 전체에 단 하나(`6800:8778`, Invalid+Masking)뿐**이라 생긴 커버리지 착시였다(§14의 guide-type 커플링과 같은 근본 원인 — 항목이 하나뿐이면 그 축을 고를 때마다 항상 그 하나로 귀결된다).

**"TwoLine 삭제" 검토**: 삭제 여부를 사용자에게 먼저 의견만 물어(실행 보류 요청) 논의한 결과, 이 컴포넌트가 **비밀번호 입력 규칙 안내**에 주로 쓰일 예정이고, 그런 맥락에서는 "일부 조건만 통과"(예: 8자 이상은 충족, 특수문자는 미충족)를 동시에 보여줘야 하는 경우가 흔하다는 점을 확인해 `TwoLine`은 유지하기로 결론지었다.

**요구사항 구체화**: 사용자가 "helper text 영역에서 줄마다 독립적으로 error/success를 설정할 수 있어야 한다"고 명확히 함 — 즉 TwoLine의 두 줄이 서로 다른 톤을 가질 수 있어야 한다.

**재확인**: 이 작업을 위해 `6800:8778`의 `guide text group` 구조를 다시 정밀하게 읽어본 결과, §16에서 "2번째 줄이 없어지고 텍스트가 '가나다'로 손상됐다"고 봤던 판단은 **오독이었다** — 실제로는 2개 guide 줄이 처음부터 온전히 존재했고, 텍스트도 이 파일 전체가 쓰는 제네릭 placeholder("guide text")였을 뿐 손상이 아니었다. (당시 얕은 depth로 조회했거나 특정 서브트리만 봤을 가능성 — 정확한 원인은 특정하지 못함. 교훈: 이 컴포넌트에 대해서는 "손상"을 단정하기 전에 항상 깊은 구조 조회로 재확인한다.)

**실제 구현**:
1. 두 guide 줄의 placeholder 텍스트를 실제 비밀번호 규칙 예시로 교체: 줄1="8자 이상 입력했어요"(Success로 교체), 줄2="특수문자가 포함되지 않았어요"(기존 Error 유지) — "일부만 통과" 상황을 실제로 보여주는 예시가 되도록 함.
2. `Guide Icon 1`(INSTANCE_SWAP, 줄1 아이콘에 1:1 바인딩), `Guide Icon 2`(INSTANCE_SWAP, 줄2 아이콘에 1:1 바인딩)를 신설.
3. **기술적으로 지난번(§14)과 다른 점**: 그때는 하나의 속성을 **서로 다른 여러 variant**의 아이콘에 바인딩해 값이 하나로 통일되는 문제를 겪었다. 이번엔 **같은 variant 안의 서로 다른 두 레이어**에 각각 전용 속성을 1:1로 바인딩하는 것이라 상황이 다르다 — 테스트 인스턴스로 `Guide Icon 1`만 Error로 바꿔본 결과, 테스트 인스턴스만 바뀌고 마스터의 실제 값(Success)은 그대로 유지됨을 확인해 정상 작동을 검증했다.

**결과**: 9개 변형 유지(구조 변경 없음, 속성만 추가), 에러 없음, 개별 스크린샷으로 확인. `Helper Text=OneLine`(4개 variant, 각 1줄)은 "부분 통과" 개념이 성립하지 않아 이번 범위에서 제외 — 여전히 `Status`에 종속된 채 유지.

## 18. 후속 수정 — 아이콘+텍스트 결합형 `Guide Row` 컴포넌트 신설

사용자가 "아이콘만 따로 고르지 말고 아이콘+텍스트를 조합으로 고를 수 있게 해달라"고 요청했다(참고 링크: `node-id=6800-8778`, 지난 절에서 작업한 `TwoLine` 변형).

**§17에서 만든 `Guide Icon 1/2`의 한계**: INSTANCE_SWAP으로 아이콘만 바뀌고, 옆 텍스트의 색은 그대로 남아 아이콘·텍스트 색이 어긋나는 문제가 있었다(테스트 인스턴스로 직접 확인 — 아이콘은 파랑으로 바뀌었는데 텍스트는 계속 빨강). 원인은 Figma `componentPropertyReferences`가 `visible`/`characters`/`mainComponent`만 지원하고 `fills`(색상)는 지원하지 않기 때문 — 아이콘과 텍스트를 각각 독립적으로 바인딩하는 한 색 불일치는 구조적으로 피할 수 없었다.

**해결책**: 아이콘+텍스트를 하나의 스왑 가능한 컴포넌트로 묶는 것 외엔 방법이 없다고 판단했다.
1. 기존 guide 줄(아이콘+텍스트, 색이 이미 올바르게 바인딩된 상태)을 그대로 복제해 `Icon` 페이지에 옮기고, `figma.createComponent()` + 자식 이동 방식(기존 `Icon / Status / Success` 만들 때와 동일 기법)으로 COMPONENT로 전환.
2. 텍스트를 제네릭 placeholder("guide text")로 리셋 후 `combineAsVariants`로 `Status`=Success/Error 2-variant 세트 `Guide Row`(`6955:7842`) 생성. `description`에 용도 기록.
3. `Text`(TEXT 타입) 속성을 신설해 두 variant의 텍스트에 바인딩 — 메시지 내용은 인스턴스별로 자유롭게 오버라이드 가능하게 함.
4. **검증**: 테스트 인스턴스에서 `Text` 속성으로 커스텀 문구를 설정한 뒤 `swapComponent()`로 Status를 Success→Error로 바꿔본 결과, 커스텀 텍스트는 그대로 유지되면서 아이콘·텍스트 색이 함께 빨강으로 바뀜을 확인 — 아이콘+텍스트 색 결합 문제가 완전히 해결됐다.
5. `TwoLine`(`6800:8778`)의 두 guide 줄의 기존 아이콘+텍스트 raw 구성을 각각 `Guide Row` 인스턴스로 교체(줄1=Success, 줄2=Error 기본값). 기존 `Guide Icon 1/2`, `Guide Text 1/2` 속성(§17에서 만든 것, 이제 대상 노드가 사라짐)은 삭제.
6. Text Input에 `Guide Row 1`/`Guide Row 2`(INSTANCE_SWAP, `mainComponent` 바인딩)를 새로 신설해 각 줄의 톤을 Text Input 레벨에서 고를 수 있게 함 — 테스트 인스턴스로 스왑 시 아이콘+텍스트 색이 함께 바뀌고 마스터(`6800:8778`)는 영향받지 않음을 재확인.

**남은 제약**: 문구(텍스트 내용) 자체는 Text Input 레벨 속성으로 노출하지 못했다 — `text.componentPropertyReferences = {characters: ...}`를 중첩 인스턴스 안의 텍스트에 걸려고 하면 `"Cannot set component property references on instance sublayer"` 에러가 난다. `isExposedInstance = true`로 우회를 시도했으나 이는 Figma UI에서 중첩 인스턴스를 더 쉽게 선택하게 해주는 기능일 뿐, `componentPropertyDefinitions`에 자동으로 새 속성을 만들어주지는 않았다. 결론: **톤 선택은 Text Input 레벨(`Guide Row 1/2`)에서, 문구 편집은 중첩된 `Guide Row` 인스턴스를 직접 선택해 그 자체의 `Text` 속성에서** 하는 2단계 워크플로로 확정했다 — 이미 이 프로젝트에서 받아들인 "중첩 인스턴스 직접 선택 후 자체 속성 편집" 패턴(트레일링 버튼의 Status 편집과 동일)과 일관된다.

**결과**: 9개 Text Input 변형 유지(구조 변경 없음), 신규 `Guide Row` 컴포넌트 1개(`Icon` 페이지), 에러 없음, 개별 스크린샷으로 확인.

## 관련 문서

- `design-system/design.md` §2.16 Text Input
- `design-system/status.md` Component Checklist 16번째 항목
- `design-system/decisions.md` 2026-09-06 항목 5건
- `diagnosis/external-comparison-airbnb-2026-09-06.md` (이 작업의 계기가 된 감사)
