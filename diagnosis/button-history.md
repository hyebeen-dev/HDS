# Button — 작업 이력

> 이 문서는 `diagnosis/14`, `17`, `20`, `22`(구 파일명)를 시간순으로 합친 이력 기록이다. 원본 내용은 그대로 보존했다.
> **지금 유효한 최신 상태는 `design-system/status.md`를 참고.**

---

## 업데이트 — Small/XSmall padding·폰트 크기를 승인 스펙대로 마저 적용 (2026-09-04)

전체 Design System 감사(`diagnosis/design-system-audit-2026-09-04.md`)에서, Small/XSmall이 세션 초반 승인받은 스펙(Small: 좌우8·상하6·13px / XSmall: 좌우8·상하5·12px)과 다르게 실제로는 둘 다 상하 padding=11, 폰트가 Small 14px·XSmall 13px로 남아있음을 재확인했다. 당시엔 Pretendard 폰트 로드 블로커로 미적용 상태였으나, 이번 세션에서 `loadFontAsync`가 정상 동작함을 확인해(2026-09-04) 마저 적용했다.

### 실행 내용
Small 7개 variant(`Type=Primary/Secondary/Line type gray`, 각 State 조합)의 `paddingTop`/`paddingBottom`을 11→6으로, 텍스트 `fontSize`를 14→13으로 변경. XSmall 7개 variant도 동일 패턴으로 `paddingTop`/`paddingBottom`을 11→5, `fontSize`를 13→12로 변경. 총 14개 노드.

### 검증
- 실행 전 14개 노드 전부 실사용 인스턴스 0곳 확인 후 진행
- 전체 Button-large 세트 스크린샷으로 Large/Medium/Small/XSmall 4개 사이즈가 세로 여백·글자 크기로 시각적으로 구분됨을 확인

## 업데이트 — Icon Button 사용자 재제작본으로 교체, Leading/Trailing Boolean 추가 (2026-09-03)

사용자가 직접 Icon Button을 다시 만들었다며(Medium/Small 2사이즈, 각 7개 상태 — Type=Primary/Secondary/Line type gray × State=Default/Pressed/Disabled 패턴, CTA Large와 동일 커버리지) Leading/Trailing 아이콘을 Boolean으로 켜고 끌 수 있게 해달라고 요청.

### 실행 전 확인
- 색상을 CTA Large의 배색과 대조해 `Property 1=Default`로 뭉뚱그려 있던 14개(Medium 7 + Small 7)의 실제 Type/State를 전부 확인(추측 없이 값 비교로 판별)
- 사용자 확인 후: 기존에 만들어뒀던 구 Icon Button(Large/Medium/Small/XSmall 16개, 실사용 0건) 삭제하고 이번 재제작본으로 교체. Button-medium/Button-small 두 세트는 CTA처럼 하나의 `Icon Button` COMPONENT_SET(Type×Size×State)으로 합침

### 실행 내용
1. 구 Icon Button(`6457:7400`) 삭제
2. 14개 variant 이름 정리(`Type=Primary, Size=Medium, State=Default` 등)
3. 각 variant에 남아있던 "button" 텍스트 placeholder 삭제(CTA에서 clone해올 때 지우지 않고 남아있던 것 — 섹션 제목이 "Icon button"이라 아이콘 전용이 맞다고 판단해 제거)
4. `Button-small`의 7개를 `Button-medium` 세트로 병합, 세트 이름을 `Icon Button`으로 변경
5. `Show Leading Icon`/`Show Trailing Icon`(Boolean, 기본값 둘 다 true) 신규 추가, 14개 variant × 2아이콘 = 28개 인스턴스 전부 연결
6. 테스트 인스턴스로 Leading=false/Trailing=true 조합이 실제로 한쪽만 보이는지 스크린샷으로 직접 확인(마스터만 보고 판단하지 않음 — 직전 CTA 작업에서 겪은 실수를 반복하지 않기 위해)

### 검증
- `componentPropertyDefinitions` 에러 없음 확인
- 전체 4개 페이지, 1028곳 인스턴스 재스캔 — 깨진 곳 0건
- 실제 인스턴스 스크린샷으로 Boolean 토글 정상 작동 확인 후 테스트 인스턴스 삭제

## 변경 이력 (추가)

| 일자 | 내용 |
|---|---|
| 2026-09-03 | 사용자가 재제작한 Icon Button(Medium/Small, 14 variant)으로 구버전 교체, 하나의 세트로 통합, Leading/Trailing Icon Boolean 추가 |

## 업데이트 — CTA 개명 + 아이콘 토글 버그 수정 + Icon Button 신규 분리 (2026-09-03)

사용자 리포트: (1) 직전에 만든 통합 버튼은 "텍스트 버튼"(CTA)이고 "아이콘 전용 버튼"(Icon Button)은 별개 컴포넌트로 분리 필요, (2) 복사한 인스턴스에서 Leading/Trailing Icon 토글이 실제로 안 먹힘.

### 버그 원인 진단 (전수 재조회)
- **Small/XSmall 6개 variant, 아이콘 12개**: `componentPropertyReferences`가 `{}`(빈 값) — Medium을 clone할 때 속성 연결이 유실됨
- **Large 7개 variant**: 애초에 아이콘 레이어 자체가 없었음(Primary/Default 1곳에만 trailing 아이콘 존재)
- ⚠️ **더 심각한 발견**: 아이콘 속성을 Large에 새로 연결한 시점부터, **실제 화면에 이미 배치된 CTA 인스턴스 17곳(Primary/Secondary, Large)이 전부 "보이지 않는 아이콘 placeholder"를 표시하는 상태로 바뀌어 있었다.** 마스터 컴포넌트 자체의 `.visible=false` 설정과, 실제 인스턴스가 갖는 `componentProperties`의 resolved 값은 **서로 다른 메커니즘**이라는 걸 이번에 확인했다 — 마스터를 고쳐도 이미 배치된 인스턴스의 resolved 값은 안 바뀐다. `getScreenshot`으로 마스터만 확인하고 "고쳐졌다"고 판단했던 게 잘못이었음(실제 인스턴스를 별도로 스크린샷 확인했어야 함).

### 실행 내용
1. **이름 변경**: `Button` → `CTA`
2. **Small/XSmall 아이콘 재연결**: 12개 아이콘 인스턴스에 `componentPropertyReferences` 복구
3. **Large 7개 variant에 아이콘 신규 추가**: Medium의 placeholder 아이콘을 clone해 각 variant 좌우에 삽입, Type별 색상 적용(Primary=흰색, Secondary=파랑, Line type gray=회색 — 전부 Medium/기존 실사용 색상에서 그대로 재사용, 새로 만들지 않음). 기존 Primary/Large/Default의 trailing 아이콘도 회색→흰색으로 색상 수정(원래 잘못된 회색이었음)
4. **기본값 통일**: `Show Leading Icon`/`Show Trailing Icon` 둘 다 `true`로 통일(사용자 확정: "기본값 둘 다 켜져있고 필요 없으면 끄는 형태")
5. **실제 인스턴스 회귀 수정**: 전체 4페이지에서 CTA 실사용 인스턴스 41곳을 전수 조회, 그중 **Large 크기 17곳**이 새로 생긴 아이콘 속성 때문에 원래 없던 아이콘이 보이는 상태였던 것을 확인 → 17곳 전부 `Show Leading/Trailing Icon = false`로 개별 override 설정해 원래 모습으로 복구. Medium 크기 인스턴스들은 원래부터 있던 속성이라 각자의 실제 값(Secondary는 trailing만 true, Line type gray는 둘 다 false)이 그대로 보존되어 있어 손대지 않음
6. **떠돌이 테스트 인스턴스 삭제**: 페이지에 고아 상태로 남아있던 Small 버튼 테스트 인스턴스 1개 발견해 삭제(실제 콘텐츠 아님)

### Icon Button 신규 생성
CTA와 별도의 COMPONENT_SET `Icon Button` 신설. CTA가 실제로 갖춘 16개 조합을 그대로 미러링(Large: Primary 3state+Secondary 2state+Line type gray 2state / Medium·Small·XSmall: 3type×Default). 각 CTA variant를 clone → 텍스트 삭제, 아이콘 하나만 남겨 "Icon"으로 이름 변경 후 신규 `Icon`(INSTANCE_SWAP) 속성에 연결 → 정사각형으로 리사이즈(Large 48×48/Medium 40×40/Small 32×32/XSmall 28×28, padding은 아이콘 16×16 기준 중앙 정렬 계산: 16/12/8/6). 색상·모서리 반경은 CTA에서 그대로 상속(재발명 없음).

**실행 중 이슈**: `figma.combineAsVariants()`로 세트를 만든 뒤 `layoutMode='GRID'`를 적용했더니 컬럼 폭이 각 variant의 실제 너비(48/40/32/28px 제각각)를 반영하지 못하고 자동 사이징이 깨졌다(세트 전체 크기가 48×1528으로 잘못 계산됨). GRID 엔진 재시도 대신 `layoutMode='NONE'` + 수동 x/y 좌표로 전환해 해결.

### 검증
- `componentPropertyDefinitions` 재조회로 CTA/Icon Button 둘 다 에러 없음 확인
- 전체 4개 페이지, 1015곳 인스턴스 전수 스캔 — mainComponent 깨진 곳 0건
- CTA 실사용 41곳 전부 개별 확인, Large 17곳 회귀 수정 완료, Medium 24곳은 원래 값 그대로 보존 확인
- Icon Button 16개 variant 스크린샷으로 정사각형·아이콘 중앙정렬·타입별 색상 확인

## 다음 단계 후보
- Small/XSmall 폰트(13px/12px)는 여전히 미적용(Pretendard 블로커, 이전 항목과 동일)
- Icon Button 16개 variant는 전부 실사용 0건 — 실제 화면에 적용해 검증 필요
- Icon Button의 기본 아이콘도 `Icon/Placeholder Square`(플레이스홀더)라 실제 사용 시 원하는 아이콘으로 교체 필요

## 변경 이력 (추가)

| 일자 | 내용 |
|---|---|
| 2026-09-03 | `Button`→`CTA` 개명, Leading/Trailing Icon 토글 버그 수정(Small/XSmall 재연결 + Large 신규 추가), 실사용 인스턴스 17곳 회귀 수정, `Icon Button` 신규 분리(16 variant) |

## 업데이트 — Button_large + Button_medium 통합, Type×Size×State×Icon 체계로 재구조화 (2026-09-03)

이 프로젝트에서 여러 번 "미결정"으로 남아있던 Button 세트 통합을 실행했다. 사용자가 준 기준: Type(Primary/Secondary(구 line_type)/Line type gray), Size(Large 48/Medium 40/Small 32/XSmall 28), State(Default/Pressed/Disabled), Icon(Boolean).

### 실행 전 재조사로 발견한 것 (기억이 아니라 재조회한 값)
- `Button_large`: Type 값이 대소문자 혼용으로 `Primary`/`Line type`/`line_type_gray`/`line_type` 4갈래로 쪼개져 있었음. 실사용은 Default 상태뿐(Pressed/Disabled는 전부 0곳).
- `Button_medium`: State 축이 아예 없었음(Default만). Type이 `Variant3`/`line_type`/`line_type_gray`/`Variant4`로 미정리. 색상 대조로 `Variant3`=Primary(파란 채움), `line_type`=Secondary(파란 테두리, 10곳 실사용), `line_type_gray`=Line type gray(4곳), `Variant4`=파란 테두리이지만 라벨이 다른("장바구니 담기") 노드였음.

### ⚠️ 실행 중 발생한 실수와 수정
`Variant4`를 `line_type`과 "같은 타입의 중복"으로 판단해 `swapComponent()`로 병합 후 삭제했는데, **실제로는 인스턴스에 텍스트 오버라이드가 없고 마스터 자체의 고유 텍스트("장바구니 담기")였다** — Option Chips 때 겪은 것과 같은 유형의 착오("이름이 같으면 병합 가능"이 아니라 "실제 콘텐츠까지 같아야 병합 가능")를 또 반복함. `swapComponent()` 직후 텍스트가 "재입고 알림 신청"(line_type의 원래 텍스트)으로 바뀐 것을 확인하고 즉시 수정:
- 파일 내 다른 곳에서 실제 "장바구니 담기" 텍스트(Large, 16px Bold)를 찾아 clone
- 영향받은 2개 인스턴스(`6294:11642`, `6264:1419`)는 **인스턴스 내부를 직접 편집할 수 없어(Figma 제약) `detachInstance()`로 분리한 뒤** 잘못된 텍스트를 삭제하고 clone한 텍스트로 교체
- **알려진 트레이드오프**: 이 2곳은 이제 Button 컴포넌트와의 링크가 끊긴 detached frame이다. 텍스트는 맞지만 폰트 크기가 16px(원래 14px)로, 폰트 블로커 때문에 줄이지 못했다. Pretendard 사용 가능한 환경에서 사용자가 직접 재교체(인스턴스로 다시 연결 + 14px로 조정) 권장

### 실행 내용
1. **Type/Size/State 이름 정리**: 대소문자 통일, Medium의 `Variant3`/`line_type`/`line_type_gray`를 `Type=Primary/Secondary/Line type gray, Size=Medium, State=Default`로 정리
2. **"2 Button" 레이아웃 분리**: `Type=Primary, Size=Large, State=Default, Layout=2 Button` variant는 4번째 속성 키(Layout)가 있어 나머지 variant와 축이 안 맞아 세트 에러를 유발 — Action Bar와 개념이 겹치기도 해서, 세트 밖으로 꺼내 독립 컴포넌트 `Button / 2 Button Layout (legacy — Primary Large Default only)`로 분리(실사용 3곳 그대로 유지, 컴포넌트 ID 안 바뀌어 영향 없음)
3. **두 세트 병합**: `Button_medium`의 3개 variant를 `Button_large`(→ `Button`으로 개명)의 GRID에 새 컬럼으로 편입, 빈 `Button_medium` 세트는 자동 삭제됨
4. **Icon 속성 통합**: 기존 Large의 단일 `Show Icon`/`Icon`(trailing 전용)을 삭제하고, Medium에 있던 `Show Leading Icon`/`Leading Icon`/`Show Trailing Icon`/`Trailing Icon`으로 세트 전체 통일. Large의 기존 trailing 아이콘 인스턴스를 새 `Trailing Icon` 속성에 재연결(리와이어링 과정에서 아이콘이 기본으로 보이는 회귀가 있었으나 `visible=false`로 즉시 수정)
5. **Small(32px)/XSmall(28px) 신규 생성**: Medium의 3개(Primary/Secondary/Line type gray)를 clone해 사용자 승인 스펙 적용
   - Small: 좌우 8 / 상하 6 / 목표 폰트 13px Bold
   - XSmall: 좌우 8 / 상하 5 / 목표 폰트 12px Bold
   - **폰트 크기는 적용 못함(Pretendard 블로커)** — 현재 두 사이즈 다 Medium에서 물려받은 14px Bold 그대로. 좌우/상하 padding과 높이(32px/28px)는 정확히 적용됨. 폰트만 나중에 사용자가 13px/12px로 교체 필요(교체 후 상하 padding을 정확히 6/5로 다시 확인하는 게 안전 — 현재 padding은 13px/12px 기준으로 계산된 값이라 14px 폰트와는 높이가 딱 안 맞을 수 있음)

### 검증
- `componentPropertyDefinitions` 재조회로 에러 없음 확인 — 최종 축: `Type`(Primary/Secondary/Line type gray) × `Size`(Large/Medium/Small/XSmall) × `State`(Default/Pressed/Disabled, Small·XSmall·Medium은 아직 Default만) + `Show/Leading/Trailing Icon`
- 파일 전체 4개 페이지(① Component, Actions, Feedback, Contents) 전수 스캔 — 총 616+27+148+194곳의 인스턴스 중 **mainComponent 깨진 곳 0건**
- Type×Size 16개 조합 스크린샷으로 렌더링 확인, 기존 실사용 버튼들 시각적으로 그대로인지 확인

## 다음 단계 후보
- Small/XSmall의 실제 폰트(13px/12px Bold)를 Pretendard 사용 가능 환경에서 적용
- detached된 2개 "장바구니 담기" 버튼(Item Card Grid Small 등)을 Button 컴포넌트 인스턴스로 재연결
- `Button / 2 Button Layout` — Action Bar와 역할이 겹치는지 검토, 필요 없으면 실사용 3곳을 Action Bar로 이전 후 폐기 검토
- Pressed/Disabled 상태와 Icon 적용을 Medium/Small/XSmall까지 확장하는 건 실사용처가 생기면 진행(이번 범위 아님)

## 변경 이력 (신규)

| 일자 | 내용 |
|---|---|
| 2026-09-03 | `Button_large`+`Button_medium`을 `Button` 하나로 통합(Type×Size×State×Icon), Small/XSmall 신규 추가(폰트 크기 제외), 2 Button 레이아웃 독립 컴포넌트로 분리, 병합 과정 중 발생한 텍스트 유실 사고 수정 |

---

## 2026-09-01 — Button: Text 타입 + 아이콘 슬롯 추가 (구 diagnosis/14)

# Figma 작업 완료 기록 — Button: Text 타입 + 아이콘 슬롯 추가

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**참고 레퍼런스** Wanted Design System — Button (node 16222:137705), 사용자 제공 쿠팡 상품 상세 캡처
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`

---

## 배경

Button 컴포넌트에 텍스트 전용 타입과 아이콘 슬롯이 전혀 없었다. Wanted의 Button 구조(variant/color/size/icon 축)를 참고하고, 사용자가 제공한 실제 쿠팡 화면에서 텍스트 버튼 실사용 사례("샤워메이트 >", "할인받기 >", "절약 금액 기준")를 확인한 결과, 아이콘이 있을 때는 **텍스트 뒤(트레일링) 화살표** 패턴이 공통적이었다.

## 실행 내용

### 1. Icon/Chevron Right 컴포넌트 신규
순수 벡터(SVG)로 만든 16×16 화살표 아이콘. 폰트 의존 없이 제작 가능해 이 실행 환경의 Pretendard 미설치 제약과 무관하게 완성했다.

### 2. Button — Type=Text 추가 (Large, Default/Pressed)
- 배경·테두리 없음, 텍스트 색상 `Color/Semantic/Text/Accent`(Default) / `Primary/600`(Pressed)
- `Show Icon`(Boolean) + `Icon`(INSTANCE_SWAP) 속성으로 트레일링 아이콘 표시 여부 제어
- 라벨 텍스트는 이 환경에 Pretendard가 없어 새로 타이핑하지 못해, 기존 버튼에 실제로 있던 텍스트("바로구매", "총 1개 상품 구매하기")를 그대로 clone해서 재사용했다 — 실제 문구("할인받기" 등)로 교체는 Pretendard 설치 후 진행 필요

### 3. Primary/Large/Default에 아이콘 슬롯 시범 적용
같은 `Show Icon`/`Icon` 속성을 기존 Primary/Large/Default에도 추가(기본값 Show Icon=false로 기존 모습 유지). 토글 시 흰색 화살표가 텍스트 뒤에 나타나는 것을 인스턴스로 확인했다.

## 실행 중 발견·수정한 문제

1. **Button 세트에 이미 있던 중복 variant**: `Type=Primary, Size=Large, State=Default, Layout=1 Button`이 서로 다른 ID(`2167:15919`, `2183:7700`)로 두 번 등록되어 있었다. 이 중복이 `componentPropertyDefinitions` 조회와 `setProperties()` 호출을 모두 막고 있었다. 두 노드 모두 실사용 인스턴스가 0개임을 확인한 뒤, 아이콘 슬롯을 이미 추가한 쪽만 남기고 중복을 삭제했다.
2. **컴포넌트(variant) 자체에는 `addComponentProperty`를 직접 호출할 수 없음**: Figma Plugin API 제약으로, Variant Set에 속한 개별 컴포넌트가 아니라 **Set 자체**에 속성을 추가해야 한다는 것을 확인했다.
3. **속성 중복 생성**: 위 두 문제를 해결하는 과정에서 `Show Icon`/`Icon` 속성이 두 벌(`...#24`/`...#25`와 `...2#48`/`...2#62`) 생겼다. 실제 아이콘 레이어에 연결된 쪽만 남기고 미사용 쪽을 삭제, 이름도 "2" 접미사 없이 정리했다.
4. **아이콘 색상 미적용**: 아이콘을 흰색으로 재색상하려던 첫 시도가 실제로는 반영되지 않아, 파란 배경 위에 파란 아이콘이 겹쳐 안 보이는 상태였다. 재확인 후 직접 흰색(Static/0)으로 바인딩해 해결했다.

## 검증

- Text 타입(Default/Pressed) 스크린샷 — 배경·테두리 없이 텍스트+트레일링 화살표 정상 렌더링
- Primary/Large/Default 테스트 인스턴스로 `Show Icon` false→true 토글 — 기본은 아이콘 없음, 토글 시 흰색 화살표가 텍스트 뒤에 정확히 나타남을 확인
- Button 전체 프레임 재스크린샷 — 기존 10개 변형이 시각적으로 그대로 유지됨을 확인

## 다음 단계 후보

1. Pretendard 설치 후 Text 타입 라벨을 실제 문구("할인받기", "절약 금액 기준" 등)로 교체
2. Medium/Small 사이즈의 Text 타입, 그리고 Secondary/Line type 등 나머지 타입에도 아이콘 슬롯 확장
3. 쿠팡 접근이 막혀 있었으므로, 추후 접근 가능해지면 실제 버튼 패턴을 더 조사해 보강

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Icon/Chevron Right 신규, Button에 Type=Text(Large) 추가, Primary/Large/Default에 아이콘 슬롯 시범 적용, 기존 중복 variant 정리 |

---

## 2026-09-01 — Button Medium 사이즈 분리 및 아이콘 슬롯 추가 (구 diagnosis/17)

# Figma 작업 완료 기록 — Button Medium 사이즈 분리 및 아이콘 슬롯 추가

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**참고 레퍼런스** Wanted Design System — Button size/icon 섹션

---

## 배경

Large(48px)와 Medium(기존 38px, Line type Gray 하나만 존재) 사이즈가 명확히 분리돼 있지 않았다. 사용자가 두 사이즈의 목표 높이(Large 48px, Medium 40px)를 확정하고, Medium을 Large와 대칭이 맞는 완전한 타입 구성(Primary/Secondary/Line type/Line type Gray)으로 확장하기로 했다. 아이콘은 기존 Large와 동일하게 trailing 전용(Show Icon Boolean + Icon INSTANCE_SWAP)으로 통일.

## 실행 내용

1. **Line type Gray/Medium/Default 높이 수정**: padding 8px→9px, 38px→40px로 조정
2. **Primary/Secondary/Line type의 Medium 버전 신규 생성**: Line type Gray/Medium을 베이스로 clone 후 각 타입의 실제 Large 버전과 동일한 Variable(배경/테두리/텍스트 색)로 재바인딩 — 임의 색상값 발명 없이 기존 Large 타입의 색상 토큰을 그대로 재사용
3. **4개 Medium variant 전체에 아이콘 슬롯 추가**: 기존 Large에 쓰던 것과 동일한 `Show Icon`/`Icon` 속성을 재사용해 trailing 위치에 아이콘 인스턴스 추가, 각 variant의 텍스트 색과 동일한 색으로 아이콘도 자동 매칭
4. Button 세트가 GRID 레이아웃이라 `setGridChildPosition`으로 빈 셀(row2/col2,3, row3/col3)에 배치

## 실행 중 발견·수정한 버그

1. **Primary/Large/Default의 기존 아이콘 인스턴스가 내부적으로 깨져 있었음** — 벡터 자식 노드가 없는 빈 인스턴스였다(이전 세션에서 색상 수정 과정 중 유실된 것으로 추정). 새 인스턴스로 교체해 실제로 흰색 화살표가 렌더링되는 것을 테스트 인스턴스로 확인, 이번 기회에 함께 수정했다.
2. **아이콘 인스턴스의 `.visible = false`를 설정하면 그 인스턴스의 자식(벡터)이 사라지는 것처럼 조회되는 현상**을 여러 차례 재현했다 — 이 실행 환경의 간헐적 버그로 보인다. `visible`을 설정하기 *전에* 벡터를 찾아 색을 입히고, `componentPropertyReferences`·`visible`은 마지막에 설정하는 순서로 우회했다.

## 검증

- 각 신규 variant를 실제 인스턴스로 만들어 `Show Icon=true`로 토글 → 트레일링 화살표가 각 타입의 텍스트 색과 일치하는 색으로 정상 렌더링됨을 스크린샷으로 확인
- 모든 fill/stroke/text 색상이 실제로 올바른 Variable에 바인딩됐는지 `boundVariables`를 직접 조회해 재확인 (Primary=진한 파랑/흰 텍스트, Secondary=연한 파랑 배경/파란 텍스트, Line type=흰 배경·파란 테두리/파란 텍스트, Line type Gray=흰 배경·회색 테두리/회색 텍스트)
- Button 세트 전체 스크린샷으로 13개 → 16개 variant가 겹침 없이 정상 배치됨을 확인

## 알려진 제약 — 라벨 텍스트

신규 3개 Medium variant(Primary/Secondary/Line type)는 Pretendard 폰트가 이 실행 환경에서 로드되지 않아 텍스트 내용을 새로 입력하지 못했다. 현재 셋 다 베이스로 삼은 "재입고 알림 신청" 텍스트가 그대로 남아있다 — Large의 대응 문구("총 1개 상품 구매하기", "바로구매", "바로구매")로 교체하는 작업은 Pretendard 접근이 가능해지면 사용자가 직접 진행해야 한다.

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Medium 사이즈 40px로 통일, Primary/Secondary/Line type Medium 신규 생성, 4개 Medium variant 전체에 아이콘 슬롯 추가, Primary/Large 아이콘 버그 수정 |

---

## 2026-09-01 — 사용자 제작 button_medium에 Leading/Trailing 아이콘 Boolean 토글 추가 (구 diagnosis/20)

# Figma 작업 완료 기록 — 사용자 제작 button_medium에 Leading/Trailing 아이콘 Boolean 토글 추가

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**대상 노드** `button_medium` COMPONENT_SET (`6192:2344`, 사용자가 직접 제작 중인 별도 Medium 버튼)

---

## 배경

사용자가 기존에 만든 Button 세트와는 별도로, "button_medium"이라는 새 COMPONENT_SET을 직접 제작 중이었다. 각 variant(Default/Variant3) 안에 텍스트 양옆으로 점선 사각형 모양의 아이콘 자리(leading/trailing 각 1개, 총 4개)를 직접 그려 배치해뒀고, 이를 Boolean으로 켜고 끌 수 있게 만들어달라는 요청.

## 실행 내용

1. 사용자가 그린 점선 사각형 벡터(16×16) 하나를 그대로 clone해 별도 위치로 옮긴 뒤 재사용 가능한 컴포넌트 `Icon/Placeholder Square`로 승격 — 기존 파일에 있던 다른 아이콘("Icon/Chevron Right")과 동일한 구조(컴포넌트 프레임 + 내부 Vector 자식)로 만들어졌다.
2. `button_medium` 세트에 4개 속성 추가:
   - `Show Leading Icon`(Boolean, 기본 true) + `Leading Icon`(INSTANCE_SWAP, 기본값 = 새 Placeholder 컴포넌트)
   - `Show Trailing Icon`(Boolean, 기본 true) + `Trailing Icon`(INSTANCE_SWAP, 기본값 = 새 Placeholder 컴포넌트)
3. 기존 4개 점선 벡터(Default 변형의 leading/trailing, Variant3 변형의 leading/trailing)를 전부 삭제하고, 새 Placeholder 컴포넌트의 인스턴스로 교체 — 각 자리의 원래 색상(Default=회색, Variant3=흰색)을 그대로 유지하며 재색상 처리하고, 위 속성에 연결했다.

## 결정 사항

- **기본값 true 유지**: 다른 버튼들(Show Icon 기본 false)과 달리, 사용자가 이미 두 아이콘 자리를 항상 보이게 만들어둔 상태였기 때문에 현재 모습을 그대로 보존하기 위해 기본값을 true로 설정했다. 필요하면 false로 바꿀 수 있다.
- **leading/trailing을 독립적인 속성으로 분리**: 이전에 Wanted 레퍼런스에서 봤던 것처럼 없음/leading만/trailing만/둘 다 4가지 조합을 모두 표현할 수 있도록, 하나의 속성으로 묶지 않고 둘로 나눴다.

## 검증

- 테스트 인스턴스 4개(둘 다 표시/leading만/trailing만/둘 다 숨김)를 만들어 스크린샷으로 각 조합이 정확히 렌더링되는지 확인 후 삭제
- 기존 2개 variant의 기본 모습이 작업 전후로 시각적으로 동일한지 확인

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | button_medium에 Leading/Trailing 아이콘 Boolean 토글 추가, 점선 placeholder를 재사용 컴포넌트로 승격 |

---

## 2026-09-01 — button_medium 아이콘 토글 시 버튼 사이즈 자동 반영 (구 diagnosis/22)

# Figma 작업 완료 기록 — button_medium 아이콘 토글 시 버튼 사이즈 자동 반영

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**대상 노드** `button_medium` COMPONENT_SET (`6192:2344`)

---

## 배경

직전 작업(diagnosis/20)에서 Leading/Trailing 아이콘을 Boolean으로 켜고 끌 수 있게 만들었지만, 아이콘을 꺼도 버튼 폭이 줄어들지 않는 문제가 있었다. 원인은 아이콘 인스턴스가 20×20 "icon" 래퍼 FRAME(`layoutMode: NONE`, 고정 크기) 안에 들어있어서 — 인스턴스 자체는 숨겨져도 래퍼 프레임이 계속 20px 폭을 차지하고 있었다.

## 실행 내용

각 variant(Default/Variant3)에서 leading/trailing 아이콘 인스턴스를 래퍼 FRAME 밖으로 꺼내 **버튼의 직속 자식**으로 재배치하고, 래퍼 FRAME은 삭제했다. 아이콘 인스턴스 자체 크기는 16×16으로 통일. 버튼이 이미 HUG(내용에 맞춰 폭 자동 조정) 상태였기 때문에, 직속 자식의 가시성이 꺼지면 그 공간이 자동으로 사라진다.

## 검증

Default variant 기준 4가지 조합을 인스턴스로 만들어 실제 폭을 측정:

| Leading | Trailing | 폭 |
|---|---|---|
| ✅ | ✅ | 150px |
| ✅ | ❌ | 130px (trailing 아이콘+간격 20px 감소) |
| ❌ | ✅ | 130px |
| ❌ | ❌ | 110px |

스크린샷으로도 4가지 조합 전부 여백이 어색하게 남지 않고 자연스럽게 줄어드는 것을 확인했다. Variant3(Primary 스타일)에도 동일하게 적용, 기본 모습(두 아이콘 표시 상태)이 작업 전후 시각적으로 동일함을 확인했다.

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | button_medium의 아이콘 래퍼 FRAME 제거, 아이콘 인스턴스를 직속 자식으로 재배치해 Boolean 토글 시 버튼 사이즈가 자동으로 줄어들도록 수정 |
