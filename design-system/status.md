# Design System — 현재 상태

> **문서 성격**: 이 문서는 "지금 디자인 시스템이 어디까지 왔는가"를 보여주는 **유일한 최신 현황 문서**다. 버전 파일을 새로 만들지 않고 이 문서를 직접 갱신한다. 구조가 크게 바뀔 때만 맨 아래 변경 이력에 한 줄을 추가한다. 각 항목의 근거는 `diagnosis/` 폴더의 원본 작업 기록을 가리키며, 세부 경위(왜 이렇게 됐는지, 실행 중 겪은 문제 등)는 원본에서 확인한다.
>
> **최종 갱신** 2026-09-05 (Figma 재조회로 확인)
> **대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)

---

## 1. Foundation 현황

| 영역 | 상태 | 근거 |
|---|---|---|
| Color · Primitive | ✅ 완료 | Gray/Primary/Secondary/Point/Error/Warning/Success/Static 8개 패밀리, `diagnosis/color-foundation-history.md` |
| Color · Semantic | ✅ 완료 (19개 변수) | Text/Background/State/Feedback/Emphasis, `diagnosis/color-foundation-history.md`. **Commercial(3개) 2026-09-04 삭제 — 실사용 0건 확인 후 제거, design.md 리뷰 중 발견** |
| Color · Rocket(로켓 배송 배지 4색) | ✅ 완료(2026-09-04) | 신규 Primitive 패밀리 `Color/Primitive/Rocket/{Seller,Fresh,Global,Tomorrow}/{Background,Text}` 8개 변수 등록, `Rocket Badge` 컴포넌트 4 variant 전량 바인딩. Foundation 문서의 잘못된 라벨(Success/Error/Warning/Info)도 실제 의미(Seller/Fresh/Global/Tomorrow)로 정정 — design.md 리뷰 중 발견·해결 |
| Color Foundation 페이지("🎨 Color") | ✅ 완료 | Semantic 22 + Primitive 78 스와치, `diagnosis/color-foundation-history.md` |
| Typography · Text Style | ✅ 완료 (6카테고리 체계) | `display`(예약)/`heading`/`body`/`label`/`navigation`/`underline` — 기존 18개 스타일 role 재정의 + 신규 11개 스펙 확정. **신규 11개는 아직 Figma에 생성 안 됨**(폰트 블로커, 사용자 수동 생성 대기) | `diagnosis/typography-foundation-history.md` |
| Typography Foundation 페이지("ㄴ Typography") | ✅ 완료 (6섹션 표) | `diagnosis/typography-foundation-history.md`(최신 섹션) 기준 최신. 이전 구조는 전부 이 버전으로 대체됨(같은 파일 안에 이력으로 보존) |
| Spacing Variables | ✅ 완료 (16개 값, 역할 설명 포함) | 4배수 11개 + 예외 5개(2,6,10,11,13), `diagnosis/spacing-foundation-history.md`. **실제 바인딩 완료(2026-09-04)** — 컴포넌트 라이브러리 전체(41개 세트) padding/gap 1,380곳을 Variable에 바인딩, 값만 우연히 일치하던 상태 해소 |
| Spacing Foundation 페이지("📏 Spacing") | ✅ 완료 | 4배수 권장 원칙 + 실측 주석 4건, `diagnosis/spacing-foundation-history.md` |
| Grid | ✅ 완료 (모바일 단일 브레이크포인트) | 360 컨테이너 / 16 마진 / 8 거터 / 4컬럼, `diagnosis/grid-foundation.md`. **캐러셀 영역(3열)은 문서에 별도 명시 안 됨** — `diagnosis/project-audit-history.md` |
| Radius | ✅ 완료(2026-09-04) | 5단계 토큰(`none`0/`small`4/`medium`8/`large`16/`full`999) 신규 정의, 컴포넌트 라이브러리 전체 1,346곳 바인딩. `diagnosis/radius-foundation.md` |
| Border/Divider | ⚠️ 부분 — 실사용 패턴은 일관됨(1px 행 구분 / 8px 섹션 구분), 토큰·네이밍 정리는 안 됨 | `diagnosis/project-audit-history.md`, `design-system/design-language.md` |
| Elevation | ❌ 미착수(의도적) | `shadow` 이펙트 스타일 정의는 있으나 실사용 0건 — 이 디자인은 그림자 대신 선으로 깊이 표현 | `design-system/design-language.md` |
| Icon | ⚠️ 부분 | Chevron Right/Status/Utility/Checkbox 존재·재사용 중, **Icon Button 완료**(사용자 재제작본, Medium/Small 14 variant, 실사용 0건) | `diagnosis/button-history.md`, `diagnosis/project-audit-history.md` |
| Image | ❌ 미착수 | 비율 규칙·깨짐 대응 미정의 | `diagnosis/project-audit-history.md` |

## 2. Component 현황

### Component Checklist (사용자 확정 15개 항목, 2026-09-03 — 기존 Tier 1/2/3 체계 전면 교체, **14/15 완료**, 2026-09-05 Message Box 신규 제작 완료 반영)

완료 기준(사용자 확정): 컴포넌트가 만들어져 있으면 완료(실사용 인스턴스 개수 무관). 근거: `diagnosis/component-checklist.md`(2026-09-03 갱신).

- ✅ Badge — roket_badge, status-badge, Cart/Countdown Badge
- ✅ Banner — Cart/Notification Banner
- ✅ Button — Button-large(CTA) + Icon Button
- ✅ Chip — Filter Chips + Option Chips(Size/Thumbnail)
- ✅ Dialog
- ✅ Heading(=App Bar, Section Header, Cart Utility)
- ✅ Message Box — `Message Box`(`6735:3455`, `Status`=Error/Warning), 2026-09-05 신규 제작. `Toast`는 별개의 기존 컴포넌트 — `design-system/design.md` §2.7
- ✅ Item Card(, Review Card)
- ✅ Label — Info Label
- ✅ List — `list`(`6527:40035`, `type=Default/Image/history × status=Default/pressed`), 실사용 7곳 확인. 2026-09-03 체크리스트 작성 시점엔 발견 못했던 컴포넌트 — 2026-09-04 감사 과정에서 확인해 정정(`diagnosis/design-system-audit-2026-09-04.md`). **2026-09-04: `history` 타입엔 8자 규칙이 안 맞음을 확인(실제 텍스트 18자), 5개 variant 전부 `textTruncation: ENDING` 적용해 말줄임 처리로 전환**(`design-system/design.md` §2.10). Category Menu Item은 실제 화면으로 배치 맥락은 재확인했으나 여전히 실제 카테고리명 콘텐츠 사례 없음(placeholder만 존재)
- ✅ Navigation — `bottom_navigation`(`Platform=iOS`/`Android`, 실사용 2곳) 존재 확인, 2026-09-03 체크리스트 작성 시점엔 누락됨 — 정정(`diagnosis/list-component.md`)
- ✅ Thumbnail — `thumbnail`(`Size=small/medium/large/Xlarge`), 실사용 10곳(`diagnosis/list-component.md`, 2026-09-04). variant 값 대소문자 표기 불일치(`Xlarge`만 대문자 X) 남아있음, 표기 통일 시점에 정리
- ❌ Radio/Checkbox — 진짜 토글 가능한 인터랙티브 컴포넌트 기준 0/2 유지. `Icon / Radio`(`6573:3439`, 실사용 0곳)와 **`Icon / Checkbox`(`6020:13544`, 2026-09-05 재확인 — "Checkbox 아예 없음"이라던 이전 서술은 오류, `Item Card / Cart` 내장+장바구니 4곳+`Cart / Selection Toolbar`에서 실사용 중)** 둘 다 존재하지만, 둘 다 Default/Disabled(또는 Selected) 상태만 있고 실제 선택 해제(빈 박스/빈 원)를 표현하는 variant가 없는 장식용 글리프라 항목 전체는 미완료 유지
- ✅ Bottom Sheet — `Bottom Sheet / Informational`(구 Sheet/Confirmation) + `Bottom Sheet / Interactive`(구 Sheet/Selector), 2026-09-05 개명. `Show Scroll`(Boolean)은 Informational에만 있음 — Interactive는 한 차례 추가했다가 완전히 되돌려 원래 구조(header/body/button)로 복원
- ✅ Tab

### 컴포넌트 상세 현황(체크리스트 항목의 세부 근거)
- Price Block, Spec Row, Rating Display — Item Card에서 독립 컴포넌트로 분리 완료(`diagnosis/item-card-family-history.md`), 단 **상품 상세페이지에서는 아직 raw로 재구현되어 인스턴스 미적용**(`diagnosis/project-audit-history.md`). **Price Block은 2026-09-04에 `Show Unit Price`(Boolean) 신규 추가** — 그람/밀리리터 단위 환산 불가능한 상품은 단위가 줄을 끌 수 있음(`design-system/design.md` §2.8, `design-system/decisions.md`)
- **Item Card / Grid Small → `Item Card / Recommendation`으로 개명(2026-09-04)** — 실사용 15곳 전수 조사 결과 카테고리/검색 그리드가 아니라 장바구니·상품 상세 하단의 횡스크롤 추가구매 유도 카드 전용이었음이 확인됨(Type×Discount 2×2 매트릭스는 유지). 같은 조사로 실제 카테고리/검색 2열 그리드를 담당하는 별도 컴포넌트 **`Item Card / Grid`(160px, 실사용 8곳)를 신규 발견·문서화** — 지금까지 완전히 미문서 상태였음. `design-system/design.md` §2.8 참고
- Item Card / Cart — Stock=Available/OutOfStock, 이름 통일 완료(`diagnosis/item-card-family-history.md`, `diagnosis/project-audit-history.md`)
- Order Deadline, Quantity Stepper, Info Label — 신규 생성 완료(`diagnosis/item-card-family-history.md`)
- **Review / Card — 2026-09-04: `Status`(OneMonthUse/Repurchase) variant 축을 `Show Status Badge`(Boolean)로 대체.** 실사용 9곳 전수 조사 결과 전부 `Type=Photo, Status=OneMonthUse` 하나만 사용 중이었음("4개 조합 전부 실사용"이라던 과거 서술 오기재). 특정 신뢰 신호로 바꾸려면 내부 `Status Badge` 인스턴스를 직접 override. `Status` 축은 API 제약으로 완전 제거는 못 함. Price Block 정가 취소선 누락 버그도 함께 수정(`textDecoration` NONE→STRIKETHROUGH) — `design-system/design.md` §2.8, `design-system/decisions.md`
- **Review / Summary — 2026-09-04: "실사용 0곳" 평가가 오류였음을 확인.** 상품 상세페이지에 실제 사용 중이었으나 컴포넌트 정리 과정에서 진짜 인스턴스가 아니라 마스터를 복제한 raw 프레임으로 남아있던 것을 사용자 지적으로 발견, 실제 인스턴스로 복원(실사용 0곳→1곳). 카테고리 유연성 확보를 위해 `Show Row 1~4`(Boolean) 4개 신규 추가 — `design-system/design.md` §2.8, `design-system/decisions.md`
- Info Row(Type=Text/Expandable/Link) — 컴포넌트 완료(`diagnosis/info-row-component.md`), **실제 결제/적립 정보·상품 부가정보 화면에는 아직 미적용(raw로 남음)**(`diagnosis/project-audit-history.md`, 미해결)
- Button — **통합 완료, `CTA`로 개명**. 단일 세트 `CTA`(`Type`: Primary/Secondary/Line type gray × `Size`: Large/Medium/Small/XSmall × `State`: Default, Medium은 Default만, Small/XSmall은 Default+Pressed(Primary만 Disabled 추가), Large는 기존 Pressed/Disabled 유지) + `Leading Icon`/`Trailing Icon` Boolean(둘 다 기본값 true). Small(32px)/XSmall(28px) padding·폰트 크기(13px/12px) 2026-09-04에 승인 스펙대로 마저 적용 완료(`diagnosis/design-system-audit-2026-09-04.md`). "2 Button" 레이아웃은 `Button / 2 Button Layout (legacy)`로 별도 분리(`diagnosis/button-history.md`)
- **Icon Button** — CTA와 별도 컴포넌트, **사용자가 직접 재제작**(기존 Claude 제작본은 삭제·교체). Type×Size×State 축(Medium/Small 2사이즈, CTA Large와 동일 7-state 커버리지), `Show Leading Icon`/`Show Trailing Icon` Boolean으로 좌우 아이콘 개별 토글. 아이콘 자체는 플레이스홀더, Large/XSmall 사이즈·실사용은 아직 없음(`diagnosis/button-history.md`)
- App Bar — Search/Filter/Filter Compact/Category/Index/Title 6 variant로 정리(Section Header·Cart 계열 분리 후), App Bar Small 별도 유지. **2026-09-04: 전 Type이 "최상단 고정"이 아님을 실사용 조사로 확인** — Title/Category/Search만 진짜 최상단, Filter는 2번째 줄, Filter Compact는 스크롤 콘텐츠 내부, Index는 Bottom Sheet 내부(Figma 변경 없음, design.md 문서만 정정). Filter/Filter Compact/Index를 계속 같은 Type 축에 둘지는 미결 — `design-system/design.md` §5
- **Section Header — 2026-09-04: `State`/`Show Action` 의미 중복 해소.** `Show Action`(Boolean)이 죽은 속성이었던 것을 우측 "전체보기" 버튼의 `visible`에 실제로 바인딩, 실사용 0곳이던 `text + button` variant 삭제. `State` 축은 값 1개만 남았지만 Figma API 제약으로 완전 제거는 못 함. 기존 `Show Action=true` 인스턴스 7곳은 버튼이 새로 노출됨(의도된 변화) — `design-system/design.md` §2.6, `design-system/decisions.md`
- Cart 전용 컴포넌트 4종(Notification Banner/Countdown Badge/Address Row/Selection Toolbar) — App Bar에서 분리, "Cart-only Components" 그룹으로 정리
- Tab, Sheet(Confirmation/Selector로 명칭 분리), Dialog, Toast — 기존 완성, 정상 재사용 중(`diagnosis/component-naming-cleanup.md`, `diagnosis/project-audit-history.md`). **Dialog는 2026-09-04에 `Show Scroll`(Boolean) 속성 신규 추가** — 본문(Body) 콘텐츠 초과 시 스크롤 UI(상하단 구분선+스크롤바)를 7개 variant 전부에서 미리보기 가능, 기본값 false(스크롤 없음). 스크롤바 thumb 색상은 `Gray/400`(최초 `Gray/700`에서 정정). 스크롤 발생 기준은 고정 px가 아니라 상대 단위(`max-height: 70vh`에서 고정 영역을 뺀 값)로 문서화. 상세는 `design-system/design.md` §2.5, `design-system/decisions.md` 참고. **Bottom Sheet는 2026-09-05에 `Sheet / Confirmation`→`Bottom Sheet / Informational`, `Sheet / Selector`→`Bottom Sheet / Interactive`로 개명, 양쪽에 Dialog와 동일한 `Show Scroll`(Boolean, 기본값 false) 패턴 추가, 화면 높이 80% 상한을 Dialog의 70vh와 동일한 상대 단위 방식으로 문서화. 같은 날 사용자 요청으로 `Bottom Sheet / Interactive`의 `Show Scroll`은 스크롤바만 먼저 제거했다가 곧이어 Boolean·구분선까지 전부 삭제해 원래 구조로 완전히 되돌림 — 현재 `Show Scroll`은 `Bottom Sheet / Informational`(구분선+스크롤바 4요소)에만 존재** — 상세는 `design-system/design.md` §2.12, `design-system/decisions.md` 참고
- Chip — **Filter Chips**(`chip`, 기존 14 variant, Status=Default/Selected 등)와 **Option Chips**로 용도 분리 완료. Option Chips는 상품 유형별로 늘어날 예정이라 `Option Chips / {유형}` 네이밍 패밀리로 관리 — 현재 **`Option Chips / Size`**(텍스트만, 예: "240ml")와 **`Option Chips / Thumbnail`**(이미지+텍스트) 2종. 둘 다 `status`(Default/low_stock, Size만 sold_out 추가) 축 + `Selected`(Boolean, Primary/500 2px 테두리) 지원, 생성 시 있던 중복 variant 이름 오류도 함께 해결(`diagnosis/chip-components.md`). Filter Chips와 Option Chips의 "선택됨" 표현 방식이 다른 점(Filter=Variant, Option=Boolean+테두리)은 의도된 차이인지 확인 필요

## 3. 알려진 이슈 / 백로그

`diagnosis/project-audit-history.md`(2026-09-02 종합 감사) 기준, 이후 해결된 항목은 취소선으로 표시.

**P0**
- ~~`button_medium`의 중복 variant 이름(`Variant4`×2)으로 속성 패널 에러~~ — 해결(`diagnosis/item-card-family-history.md`에서 이름 분리)
- Item Card Grid/List에 Discount 축 없음 — **미해결**
- Price Block/Spec Row/Rating Display가 상품 상세페이지에서 raw로 재구현됨 — **미해결**

**P1**
- Figma AI로 생성한 카테고리 화면(`6502:37457`, "① Component" 페이지)에 실사용 중인 진짜 Section Header(`6324:1546`, 11곳 사용)를 인스턴스로 안 쓰고 **COMPONENT_SET을 통째로 복제**해서 심음(`6502:37458`, 실사용 0) — Assets 패널 중복, 마스터 갱신 미반영. **미해결**
- 같은 화면의 카테고리 Sidebar 리스트(List 후보) 하단 4개 탭이 전부 "자동차용품"으로 중복 — 콘텐츠 버그. **미해결**(사용자가 직접 정리 예정)
- Info Row가 실제 화면(결제/적립 정보, 상품 부가정보)에 미적용 — **미해결**
- ~~Button 세트 통합 여부 결정 필요(`Button_large` vs `button_medium`)~~ — 해결(`diagnosis/button-history.md`, 2026-09-03 통합)
- ~~Small/XSmall 버튼의 실제 폰트 크기(13px/12px) 적용~~ — 해결(2026-09-04, padding 6px/5px도 함께 적용, `diagnosis/design-system-audit-2026-09-04.md`)
- Detached된 "장바구니 담기" 버튼 2곳을 Button 컴포넌트 인스턴스로 재연결 — **미해결**
- ~~"재입고 알림 신청" 버튼(Item Card/Cart OutOfStock)이 미아 컴포넌트(`6009:9856`, 부모 없음)를 참조 중~~ — 해결(2026-09-04). 정식 `Button` 세트의 `Type=Line type gray, Size=Medium, Status=Default` variant로 swap, 그 variant 텍스트를 실제 문구로 갱신, 미아 컴포넌트 삭제
- App Bar `type12`, Action Bar `Layout3` 등 미정리 이름 — 확인 필요(App Bar는 이후 재정리됨, Action Bar `Layout3`는 미확인)
- 카운트다운 배지가 2곳(장바구니 상단/Cart Item 내부)에서 다른 스타일로 raw 구현 — **미해결**

**P2**
- 결제금액 breakdown, 상품문의 리스트 행을 신규 컴포넌트로 정식화 — **미착수**
- ~~15px/16px/17px 텍스트 값의 정체 확인~~ — 해결(`diagnosis/typography-foundation-history.md`에서 6카테고리 체계로 정리, 단 신규 스타일 11개는 Figma 생성 대기)
- ~~상품명 15px Medium 미바인딩~~ — 해결(2026-09-04, design.md 리뷰 중). 로컬 `text/body/compact-medium`(15px Medium)에 정식 바인딩. 같은 조사에서 `Quantity Stepper` 숫자·"총 4,000원 할인" 텍스트가 이름은 같지만 **외부 팀 라이브러리의 원격(remote) 스타일**을 참조하고 있던 것도 함께 발견해 로컬 스타일로 재연결 — `diagnosis/typography-foundation-history.md`
- ~~Delivery Badge 4색의 Semantic Token 승격 여부~~ — 해결(2026-09-04, Primitive 레벨 `Rocket` 패밀리로 등록. Semantic 승격은 하지 않음 — 배송 유형이라는 도메인 특화 개념이라 Primitive에 머무는 것으로 충분하다고 판단)

**P3**
- Divider 레이어 이름을 두께별로 구분(`divider-thin`/`divider-section`) — **미착수**
- Grid Foundation 문서에 "캐러셀 영역 3열" 규칙 보완 — **미착수**

**신규 텍스트 스타일 생성 대기 (2026-09-03, `diagnosis/typography-foundation-history.md` 기준)**
Pretendard 폰트를 이 실행 환경에서 로드할 수 없어 사용자가 Figma 앱에서 직접 생성해야 하는 11개 Text Style — `heading/xsmall`, `heading/xsmall-medium`, `heading/xxsmall`, `body/compact`, `body/compact-medium`, `label/small`, `label/xsmall-medium`, `navigation/small-bold`, `navigation/small`, `navigation/medium`, `underline/xsmall`. 정확한 스펙과 재바인딩 대상 노드는 `diagnosis/typography-foundation-history.md`(2026-09-03 섹션) 참고.

## 4. 관련 문서

- 전체 시각 스타일 관찰 → `design-system/design-language.md`(2026-09-04 v0.2로 전면 갱신 완료 — design.md 리뷰로 확정된 사실들과의 불일치 7건 해소. 전담 에이전트 `design-language-curator`(`.claude/agents/design-language-curator.md`) 신설, 앞으로 이 문서 갱신은 해당 에이전트가 담당)
- 짧은 의사결정·버그수정 로그 → `design-system/decisions.md`
- 각 작업의 상세 경위 → `diagnosis/` 폴더(세션별 원본 기록)

## 변경 이력

| 일자 | 내용 |
|---|---|
| 2026-09-03 | 최초 작성. `diagnosis/01`~`30` 전체 검토 + Figma 재조회로 Tier 1/2/3 체크리스트 최신 상태 확인 후 종합 |
| 2026-09-03 | `diagnosis/` 30개 파일 중 중복 있는 22개를 6개 이력 파일로 병합(색상/타이포그래피/스페이싱/Item Card/Button/프로젝트 감사) — 30개 → 13개로 정리. 이 문서의 diagnosis 링크를 병합 후 파일명으로 갱신 |
| 2026-09-03 | Chip을 Filter Chips/Option Chips로 분리(사용자 작업), Option Chips에 Selected Boolean 추가 완료. 신규 `diagnosis/chip-components.md` |
| 2026-09-03 | Option Chips에 이미지 포함 신규 세트(`Option Chips / Thumbnail`) 추가 + 기존 `size`를 `Option Chips / Size`로 이름 통일, 패밀리 네이밍 규칙(`Option Chips / {유형}`) 확립 |
| 2026-09-03 | Button 통합 완료 — `Button_large`+`Button_medium` → 단일 `Button`(Type×Size×State×Icon), Small/XSmall 신규 추가(폰트 크기 제외) |
| 2026-09-03 | `Button`→`CTA` 개명, 아이콘 토글 버그 수정(실사용 인스턴스 17곳 회귀 포함), `Icon Button` 신규 분리(16 variant) |
| 2026-09-03 | 사용자가 재제작한 Icon Button(Medium/Small, 14 variant)으로 교체, 하나의 세트로 통합, Leading/Trailing Icon Boolean 추가 |
| 2026-09-03 | Figma Component Checklist를 Tier 1/2/3(18개 항목) → 사용자 확정 플랫 15개 항목으로 전면 교체, 11/15 완료 판정(`diagnosis/component-checklist.md`) |
| 2026-09-04 | 사용자가 List 후보 컴포넌트(`Category Tab`, `Category Menu Item`, `Quick Badge Row`) 신규 생성 — 리뷰 후 네이밍 충돌(`menu`)·콘텐츠 중복("식품"×3)·표기 통일 4건 수정. `Navigation` 항목은 기존에 존재하던 `bottom_navigation`을 뒤늦게 확인해 ❌→✅ 정정(`diagnosis/list-component.md`), 체크리스트 12/15 |
| 2026-09-04 | 사용자가 Thumbnail/Radio·Checkbox/List/Navigation 완료를 보고 → 재조회 결과 `thumbnail`(4-size variant, 실사용 10곳) 신규 완료 확인해 ❌→✅ 반영, `radio`는 중복 이름 버그(`Property 1=Default`×2, 실제로는 둘 다 진짜 Default/Selected 상태였음)를 수정했으나 Checkbox 컴포넌트 부재로 Radio/Checkbox 항목은 미완료 유지, List는 컨테이너 미생성으로 미완료 유지 — 체크리스트 13/15, Figma 체크리스트 보드(`6144:769`)도 동기화 |
| 2026-09-04 | 전체 Design System 감사 실시 후 P0 4건 + P1 전 항목(Noto Sans KR, Button Small/XSmall 스펙, `Text` orphan 변수, Button Size 대소문자, List 체크리스트 정정, Property 이름 표준화) 완료. `list` 컴포넌트 재발견으로 체크리스트 14/15로 상향. 3개 "ㄴ ..." 하위 페이지의 컴포넌트 라이브러리 중복(19개)도 삭제. 상세는 `diagnosis/design-system-audit-2026-09-04.md` |
| 2026-09-04 | 감사 P2 4건 완료: 최상위 컴포넌트 이름 18개 Title Case 통일(그 과정에서 `icon`/`Icon Checkbox` 진짜 중복 발견해 병합), `Sheet / Confirmation` 죽은 Variant 축 제거, `Bottom Navigation Item` `Icon×State` 재구조화, `Review / Card` 영문 값 전환. 감사 P0~P2 전 항목 완료, 41개 세트 에러 0건 |
| 2026-09-04 | `design.md` 섹션별 리뷰 진행 — Foundation(Color/Typography/Spacing/Radius) 전 항목 처리, §2 Component 15개 항목 4필드(Description/When to use/Variants·States/Content rules) 작성, Badge(Rocket Badge 실측 정정+Status Badge를 Review Card에 연결+Countdown Badge→Banner 재분류), Banner 정의 보류, Button 3분류+미아 컴포넌트(`6009:9856`) 재연결, Chip 토글 동작 문서화, **Dialog에 `Show Scroll` Boolean 신규 추가**(7개 variant 전부에 구분선 2개+스크롤바 요소 추가, `Background/Divider`·`Gray/700` 토큰 바인딩, scroll-track은 의도적 미바인딩 예외). Radio 컴포넌트가 세션 중 삭제되어 체크리스트가 14/15→향후 재확인 필요 상태로 정정됨. 상세는 `design-system/design.md`(v0.7), `design-system/decisions.md` |
| 2026-09-04 | Dialog 스크롤 기준을 고정 px(426px)에서 상대 단위(`max-height: 70vh`에서 고정 영역을 뺀 값)로 수정, `scroll-thumb` 색상 `Gray/700`→`Gray/400` 변경(Figma 7개 variant 전부 재바인딩). 전담 에이전트 `design-language-curator` 신설 후 호출해 `design-language.md`를 v0.2로 전면 갱신(확정 금액 색상·Commercial 삭제·Rocket 토큰화·상품명 15px 바인딩·Typography 6카테고리 29스타일·Radius 5단계 체계·Shadow 예외 2건 등 design.md 리뷰로 확정된 사실과의 불일치 7건 해소) |
| 2026-09-04 | §2.6 Heading 피드백 반영 — App Bar "최상단 고정" 서술을 실사용 인스턴스 전수 조사로 Type별 정정(Title/Category/Search만 최상단, Filter/Filter Compact/Index는 아님, Figma 변경 없음). Section Header의 `State`/`Show Action` 의미 중복 해소(Figma 실행) — `Show Action` Boolean을 우측 버튼 `visible`에 정식 배선, 실사용 0곳이던 `text + button` variant 삭제, `State` 축은 API 제약으로 완전 제거는 못 함. design.md v0.9 |
| 2026-09-04 | Message Box와 Toast가 별개임을 확인 — Message Box 체크리스트 상태 ✅(추정)→❌ 정정(13/15), Toast는 기존 별도 컴포넌트로 명시. Toast 동작(위치=화면 최하단 16px 고정/Bottom Nav 유무 무관, 노출시간=3초)을 신규 확정(사용자 결정, Figma 변경 없이 design.md에 개발 구현 규칙으로 문서화). design.md v0.10 |
| 2026-09-04 | §2.8 Item Card 피드백 반영 — `Item Card / Grid Small`을 `Item Card / Recommendation`으로 개명(실사용 15곳 전수 조사로 카테고리/검색 그리드가 아니라 횡스크롤 추가구매 카드 전용임을 확인, Figma 실행). 조사 중 지금까지 미문서였던 실제 카테고리/검색 그리드 컴포넌트 `Item Card / Grid`(160px, 실사용 8곳)를 신규 발견해 문서화. `Price Block`에 `Show Unit Price`(Boolean) 신규 추가(Figma 실행, 실사용 46곳 전부 기존엔 토글 없이 항상 노출). design.md v0.11 |
| 2026-09-04 | Price Block 정가 취소선 누락 버그 수정(Figma 실행). `Review / Card`의 `Status` variant 축을 `Show Status Badge`(Boolean)로 대체(Figma 실행) — 실사용 9곳 전수 조사로 "4개 조합 전부 실사용" 서술이 오기재였음을 확인(전부 1개 조합만 사용), 내부 Status Badge 인스턴스 override로 신뢰 신호 종류 변경 가능. design.md v0.12 |
| 2026-09-04 | Review / Summary의 "실사용 0곳" 평가 오류를 사용자 지적으로 정정 — 상품 상세페이지의 raw 프레임을 실제 인스턴스로 복원(Figma 실행). 카테고리 유연성 확보를 위해 `Show Row 1~4`(Boolean) 4개 신규 추가, 3개 variant 전부 바인딩(Figma 실행). design.md v0.13 |
| 2026-09-04 | List `history` 타입의 8자 규칙을 폐기하고 말줄임 처리로 확정 — 5개 variant 전부 `textTruncation: ENDING` 적용(Figma 실행, 기존 DISABLED). Category Menu Item은 사용자 공유 화면으로 배치 맥락 재확인했으나 실제 콘텐츠 사례는 여전히 없음(문서만 보강). design.md v0.14 |
| 2026-09-05 | §2.12 Bottom Sheet 개명 + 기능 반영 — `Sheet / Confirmation`→`Bottom Sheet / Informational`, `Sheet / Selector`→`Bottom Sheet / Interactive`(Figma 실행, 내부 variant 축 불변). 양쪽에 `Show Scroll`(Boolean, 기본값 false) 신규 추가(Dialog 패턴 동일 적용, Figma 실행 — Informational 6개 variant 전부 4요소, Interactive는 Filter만 4요소·Item은 푸터 없어 3요소). 화면 높이 80% 상한을 상대 단위(`max-height: 80vh`)로 문서화(Dialog 70vh 규칙과 동일 원칙, Figma 프레임 변경 없음). Content rules 갱신: Informational=순수 텍스트 전달, Interactive=교차판매/업셀 확인 또는 필터 선택 화면. design.md v0.15 |
| 2026-09-05 | §2.12 후속 수정 — `Bottom Sheet / Interactive`(Filter/Item 둘 다)에서 스크롤바(scroll-track/thumb)만 삭제(Figma 실행, 사용자 요청), 구분선과 `Show Scroll` Boolean은 유지. Informational의 스크롤바는 변경 없음. design.md v0.16 |
| 2026-09-05 | §2.12 재정정 — 사용자가 Boolean이 아직 안 지워졌다고 재요청, `Bottom Sheet / Interactive`에서 `Show Scroll` 기능 전체(구분선 3개+Boolean 속성) 완전 삭제(Figma 실행, `deleteComponentProperty` 정상 성공) — Filter/Item 모두 header/body(/button)만 남은 원래 구조로 복원. Informational의 `Show Scroll`은 대상 아니라 변경 없음. design.md v0.17 |
| 2026-09-05 | §2.12 문서 보강(Figma 변경 없음) — Informational/Interactive의 Show Scroll 유무 차이에 대한 설계 근거(자유 텍스트 vs 리스트·캐러셀 형태의 암묵적 스크롤 암시)를 "스크롤 동작" 서브섹션에 정식 문장으로 추가. design.md v0.18 |
| 2026-09-05 | §2.13 Tab Description/When to use 재작성(Figma 변경 없음) — 실사용 전수 조사 결과 `Tab Group`은 4곳 전부 장바구니 화면 단일 패턴(전부 `Type=3 Tab`, "일반구매(6)/자주산상품/찜한상품(40)")으로 카테고리 나열이 아니라 사용자 이력 관점 전환 용도임을 확인, `2 Tab`/`Swipe`는 실사용 0곳(라이브러리 전용)이라는 캐비어트 추가하고 근거 없던 "탭 4개 이상→Swipe" 임계값 삭제. `Tab Item`은 실사용 23곳 전부 Tab Group 내부 자식뿐임을 재확인(변경 없음). design.md v0.19 |
| 2026-09-05 | §2 Component(§2.1~2.15) 전체를 당근마켓 Seed Design·지마켓 GDS 문서 스타일로 재구성(Figma 변경 없음, 순수 리포매팅) — Anatomy/Do·Don't 사용 가이드/`Property\|Values\|Default\|비고` Variants 표/유사 컴포넌트 비교 노트 패턴 적용, 기존 감사·조사 이력(✅ 정정, ⚠️ 확인 필요, node ID, 날짜, 실사용 N곳)은 사용자 확정에 따라 전부 유지. 재작성 전/후 node ID·실사용 카운트 diff로 정보 손실 없음 확인. §1/§2 요약 표/§3/§4/§5는 대상 제외. design.md v0.20 |
| 2026-09-05 | 당근/G마켓 벤치마킹 후속(Figma 변경 없음) — 사용자가 3개 제안 중 "확정 주체 표기"는 제외, 나머지 2개만 반영: 최상위 컴포넌트 37개에 `Figma: node-id` 참조 라인 추가(Figma 실시간 조회로 정확한 ID 확보), 형제 컴포넌트가 진짜 병렬 비교인 6개 섹션(Badge/Button/Chip/Item Card/List/Bottom Sheet)의 "비교" 산문을 표로 전환(포함 관계인 Tab/Navigation은 산문 유지). 37개 ID 전부 존재 확인, 기존 사실 손실 없음. design.md v0.21 |
| 2026-09-05 | §2.15 Checkbox 서술 오류 정정 — 사용자가 Figma 링크로 지적, `Icon / Checkbox`(`6020:13544`)가 실재하며 `Item Card / Cart` 내장+장바구니 화면 4곳+`Cart / Selection Toolbar`에서 실사용 중임을 확인("Checkbox 처음부터 없음"이 오류였음). 단 Default/Disabled 둘 다 "체크됨" 모양 고정이라 Unchecked variant가 없어 진짜 토글 컴포넌트는 여전히 0/2. design.md v0.22 |
| 2026-09-05 | §3 Naming Convention 3건 보강(Figma 변경 없음) — 당근/G마켓 둘 다 네이밍 컨벤션을 공개하지 않음을 확인 후 자체 감사로 발견한 공백 반영: Boolean 프로퍼티 접두사(`Show {명사}`) 규칙 명문화, Foundation 토큰이 카테고리마다 표기 관례가 다르다는 관찰 기록(통일 여부는 §5 후속 과제), 새 이름 추가 전 철자 재확인 권장 규칙 추가(`Discription` 오탈자 전례 근거). §5 Checkbox 항목도 v0.22 내용에 맞춰 갱신. design.md v0.23 |
| 2026-09-05 | §4 Design Principles에 신규 원칙 2개 추가(8개→10개, Figma 변경 없음) — 사용자 요청으로 이번 세션 전체를 리뷰해 컴포넌트 하나에 국한되지 않는 원칙급 패턴만 승격: (1) 모달형 오버레이(Dialog 70vh/Bottom Sheet 80vh)는 높이를 vh 상대 단위로 제한하고 스크롤 인디케이터는 자유 텍스트에만 붙인다 (2) 탭형 선택 UI는 "같은 데이터를 관점별 재구성"(Tab Group)과 "서로 다른 대상 나열"(Category Tab/Chip)으로 역할이 분리된다. design.md v0.24 |
| 2026-09-05 | 문서 정합성 오류 정정(Figma 변경 없음) — §5 "Section Header State/Show Action 검토" 항목이 2026-09-04에 이미 해결됐는데(§2.6엔 반영됨) §5 목록·§2 요약 표에서 안 지워졌던 걸 발견, 둘 다 §2.6과 일치하도록 정정(취소선+해결 근거 추가). design.md v0.25 |
| 2026-09-05 | Figma 파일 페이지 재구성(Figma 실행) — Assets 패널에서 개발자·기획자가 컴포넌트를 편하게 찾도록 `Components`/`Icon` 2개 페이지로 분리. 아이콘 글리프 컴포넌트 6개(`Icon / Status`/`Utility`/`General`/`Checkbox`/`Radio`/`Placeholder Square`)를 신규 `Icon` 페이지로 이동, 기존 `① Component` 페이지는 `Components`로 개명(원형 숫자 접두사는 사용자 확정으로 제외). 노드 ID는 그대로라 design.md 문서 변경 불필요 |
| 2026-09-05 | 아이콘 컴포넌트 재분류 + variant 분리(Figma 실행) — 실사용 근거로 `Icon / Utility`(24곳)·`Icon / Checkbox`(10곳)·`Icon / Radio`를 `Components` 페이지로 재이동, `Icon / Status`는 실사용 0곳+Dialog가 실제로는 고아 컴포넌트("x-01", `17:1119`) 참조 중임을 발견해 `Icon` 페이지 잔류로 확정. 이어서 5개 COMPONENT_SET의 variant 14개 전부를 독립 컴포넌트로 분리해 Assets 패널에서 클릭 없이 보이게 함 — 위험도 낮은 순 단계적 실행, 매 단계 스크린샷 검증, 문제 0건. design.md v0.26(§5에 Dialog 고아 컴포넌트 재연결 필요 항목 추가) |
| 2026-09-05 | design.md §1 Foundation+§2 Component 문체를 존댓말로 통일(Figma 변경 없음, 순수 리포매팅) — 지마켓 GDS 참고. 불일치가 섹션이 아니라 필드 종류 단위(Description은 이미 존댓말, Do/Don't·Content·비고는 평어)였음을 확인, §3/§4/§5/변경이력은 장르가 달라 제외. "바텀시트"/"다이얼로그"/"체크박스" 한글화 표기도 백틱 영문으로 정규화. node ID 50개·실사용 75건·✅ 60건·⚠️ 19건·날짜 103건 전부 diff 일치로 정보 손실 0건 검증. design.md v0.27 |
| 2026-09-05 | design.md §1~§5 전체에서 과거 이력 서술("✅ 정정", "해결된 이슈" 등) 제거, decisions.md를 유일한 이력 저장소로 일원화(Figma 변경 없음) — Type A(과거 서술, 약 48건 삭제·decisions.md 표본 대조로 손실 없음 확인)/Type B(현재 캐비어트, 약 30건 유지)/Ambiguous(약 26건 절 단위 분리)로 분류. ✅/❌ 상태 열·Do/Don't 불릿·변경이력 표는 원래 성격이 달라 전부 그대로 유지(diff 완전 동일 확인). §5의 이미 해결된 취소선 항목 3개도 목록에서 완전히 삭제. design.md v0.28 |
| 2026-09-05 | design.md에 `## 0. 이 문서 사용 원칙` 신규 추가(Figma 변경 없음) — 새 화면 제작 시 지킬 프로세스 규칙 5개(컴포넌트 우선/토큰 전용/컴포지션 우선/모호함 표면화/사후 검증) 명문화. design.md v0.29 |
| 2026-09-05 | 남은 별표 항목 전부 배치 실행 — `Message Box` 컴포넌트 신규 제작(Figma, `Status`=Error/Warning), 신규 Semantic 색상 토큰 4개(기존 Primitive alias), Category 화면 `Status Bar` 32→44px 통일(Figma), Overlay/Success 색상 사용 규칙 확정, Grid 정합성·Label/Chip/Badge 비교표·Variant/Boolean 기준·텍스트 넘침 원칙·`## 6. Screen Composition Patterns` 문서 반영. Component Checklist 13/15→14/15. design.md v0.30 |
