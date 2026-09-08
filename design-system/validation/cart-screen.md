# UI 검증 — 신규 "장바구니" 화면 (커머스 도메인 내부 검증)

**목적**: design.md가 새 화면을 실제로 만들기에 충분한지 검증. 예쁜 화면을 만드는 게 목표가 아니라, 어디서 판단이 막히는지를 드러내는 것이 목표였다.

**Figma**: https://www.figma.com/design/y8OcE4JLKi7ADIPCVFJQej/HDS_2609?node-id=6748-3437 (배치: "① Components" 페이지 "UI" 섹션)

**조건**: design.md → Foundation/Variables → Components → 기존 상품목록/카테고리/상품상세 화면 순으로 우선 판단. 기존 장바구니 화면(`6223:4991`)은 참고 목록에서 의도적으로 제외됨 — 다만 이전 세션(Screen Composition Patterns 작업)에서 이미 그 구조를 조회한 적이 있어 완전한 블라인드 테스트는 아니었음을 투명하게 밝힌다.

## 작업 전 판단

- **design.md만으로 바로 결정 가능**: 헤더/본문 구조(`## 6. Screen Composition Patterns` 패턴 2 — 헤더 뒤 divider 없이 콘텐츠 시작, 1px=섹션 내부/8px=섹션 간 구분, `Action Bar` 최하단 고정), 색상 규칙(가격 강조=Error/Gray, 파랑=CTA 전용), Radius/Spacing 토큰.
- **기존 컴포넌트로 재사용 가능**: `App Bar`(Type=Title), `Cart / Selection Toolbar`, `Item Card / Cart`(Stock=Available — 체크박스+상품명+옵션텍스트+이미지+Label+Spec Row+Order Deadline+Price Block+Quantity Stepper를 이미 전부 내장), `Action Bar`(Layout=1 Button).
- **design.md에 정의되지 않아 추론이 필요**: `Action Bar`/`Quantity Stepper`는 design.md에 전용 섹션이 없어(반복 언급만 있음) Figma에서 직접 실제 Variants(`Layout`=1 Button/2 Button/1 Button_line type, `Status`=Default/MinReached/MaxReached)를 확인해야 했음. 결제 요약("선택 상품 금액/배송비/총 결제 예정 금액") 블록은 컴포넌트가 없어 raw 텍스트+토큰으로 구성. 체크박스는 `Default`/`Disabled` 둘 다 "체크됨" 모양뿐이라 선택 해제 상태를 표현할 방법이 없어 전부 선택된 상태로만 표시.
- **새 Component가 필요한 부분**: 없음 — 전부 기존 컴포넌트 재사용으로 해결됨.

## 제작 후 자체 검토

1. **design.md 규칙을 얼마나 직접적으로 적용했는가?** 상당히 직접적이었다. 헤더/본문 구조는 §6 패턴 2를 그대로 따랐고, §1.1 색상 규칙(가격=Error/Gray, 파랑=CTA 전용)을 어긴 곳이 없다. Radius/Spacing도 기존 토큰만 사용.
2. **design.md에 없는 내용을 임의로 결정한 부분?** 결제 요약 블록의 레이아웃(라벨-값 좌우 정렬, 패딩 16, 행간 8, total 행 bold), "배송비=무료" 표기(0원 vs 무료 규정 없음), 체크박스를 전부 "체크됨" 모양으로 통일(대안 없음).
3. **기존 Component를 충분히 재사용했는가?** 그렇다. 새로 만든 컴포넌트 0개. `App Bar`, `Cart / Selection Toolbar`, `Item Card / Cart`, `Action Bar` 4개 컴포넌트 인스턴스만으로 요청 항목 대부분 해결 — `Item Card / Cart` 하나가 카드 레벨 요구사항 전부를 이미 내장.
4. **기존 화면들과 시각적으로 일관적인가?** 그렇다. Status Bar 44px, App Bar 구조, 16px 마진, divider 두께 관례(1px/8px)가 동일.
5. **design.md만으로는 판단하기 어려웠던 부분?** `Action Bar`/`Quantity Stepper` 전용 섹션 부재. `Item Card / Cart`의 실제 내부 구조(체크박스+옵션 텍스트+Label+Spec Row+Order Deadline+Price Block+Quantity Stepper 조합)가 design.md 서술("상품명 패턴은 Recommendation과 동일")과 실제로 달랐음. 결제 요약 블록은 아예 컴포넌트가 없어 판단 근거 전무.
6. **앞으로 도움이 될 규칙?** `Action Bar`/`Quantity Stepper`의 정식 §2 섹션(Variants 표) 추가. "결제 요약(Payment Summary)" raw-text 블록의 표준 레이아웃 패턴을 §6에 명문화. 체크박스 Unchecked variant 부재를 재사용 가이드에 명시.

## 잘 정의되어 있는 부분
Screen Composition Patterns(헤더/divider 규칙), 색상 사용 규칙, Radius/Spacing 토큰, `Item Card / Cart`/`Price Block`/`Spec Row` 등 카드 레벨 컴포넌트.

## 부족한 부분
- `Action Bar`·`Quantity Stepper`의 공식 문서화 부재.
- 결제 요약 블록을 위한 컴포넌트·레이아웃 규칙 부재.
- 체크박스 Unchecked 상태 부재(기존에 알려진 이슈).
- `Item Card / Cart`의 개별 삭제 아이콘이 고아 컴포넌트(`x-01`)를 참조 중 — Dialog와 같은 결함이 두 번째로 발견됨(design.md §5에 Dialog 건만 등록돼 있었음, Item Card/Cart도 같은 결함).
- design.md의 Item Card/Cart 콘텐츠 서술("상품명 패턴은 Recommendation과 동일")이 실제 구조(옵션이 별도 텍스트 줄)와 어긋남.

## 개선 제안
1. `Action Bar`, `Quantity Stepper` 전용 섹션을 §2에 추가.
2. "결제/가격 요약 블록"을 정식 컴포넌트 또는 최소한 §6에 raw-layout 패턴으로 등록.
3. `Item Card / Cart`의 Content 서술을 실제 구조에 맞게 정정.
4. `x-01` 고아 컴포넌트를 정식 아이콘으로 재연결(Dialog + Item Card/Cart 두 곳 모두).
5. Checkbox 컴포넌트에 진짜 Unchecked variant 추가(§5 기존 백로그 우선순위를 올릴 근거가 하나 더 생김).

---

## 피드백 반영 (2026-09-05)

사용자가 완성된 화면(원본 삭제 후 현재는 `6764:3606` "🧪 장바구니 (design.md 검증)", "Ⓝ Note" 페이지)을 검토하고 3개 피드백을 줬다.

### 1. cart item 컴포넌트 양 옆 여백 16px이 지켜지지 않았다
**진단**: 화면 최상위 프레임에 padding 없이 `Item Card / Cart` 인스턴스를 `layoutSizingHorizontal=FILL`로 붙여서, 카드 폭이 328px(마스터 기본값)이 아니라 화면 전체 폭 360px로 늘어나 있었다. 좌우 16px 여백이 통째로 사라진 상태였다.
**반영**: 아이템 카드+구분선을 감싸는 별도 `item-list` 오토레이아웃 컨테이너를 새로 만들어 `paddingLeft/Right=16`을 적용, 그 안에서 카드가 FILL되면 자연히 328px(360−32)이 되도록 수정(Figma 실행).

### 2. cart item 사이 divider가 item - spacing(16px) - divider - spacing(16px) - item 패턴이 아니었다
**진단**: 실제 화면(`node-id=6764-3802`, ★ 장바구니)을 확인한 결과 아이템 사이 간격은 "16px 여백 + 1px divider + 16px 여백"으로 구성돼 있는데, 내 화면은 divider를 아이템에 바로 붙여서(간격 0) 넣어 시각적으로 답답해 보였다. 또한 divider 폭(328px)이 360px 컨테이너 안에서 좌측 정렬(`counterAxisAlignItems=MIN`)돼 있어 좌우 여백이 비대칭이었던 것도 함께 발견.
**반영**: 위 `item-list` 컨테이너의 `itemSpacing=16`으로 설정 — 컨테이너 안에 `[아이템, divider, 아이템, divider, 아이템]` 순서로 넣으면 모든 인접 요소 사이에 자동으로 16px 간격이 생겨 "아이템-16-divider-16-아이템" 리듬이 정확히 재현됨. divider도 FILL 폭이라 좌우 정렬 문제도 함께 해소(Figma 실행).
**추가 발견**: 헤더→콘텐츠 경계 divider(360px, 풀블리드)와 같은 섹션 내부 아이템 구분 divider(328px, 마진 인셋)는 폭이 다르다는 게 실제 화면에 이미 있던 규칙인데, `## 6. Screen Composition Patterns`엔 "1px divider=섹션 내부 구분"이라고만 적혀 있고 이 폭 차이(풀블리드 vs 인셋)는 명시돼 있지 않았다.

### 3. 필수 요소는 다 들어갔지만 사용성·비즈니스 측면의 깊은 고민이 부족(ex. 추천 상품 노출)
**진단**: design.md 규칙을 어긴 건 아니었지만, 실제 커머스 장바구니의 핵심 목적(추가 구매 유도) 중 하나가 빠져 있었다. 공교롭게도 실제 "★ 장바구니" 화면에는 이미 `Item Card / Recommendation` 캐러셀("다시 구매하세요")이 있는데, 이번 검증에서는 사용자가 요청한 필수 항목 체크리스트만 그대로 옮기고 이 패턴은 가져오지 않았다.
**반영**: `Section Header`("다시 담아보세요") + `Item Card / Recommendation`(Discount=None) 3장 가로 캐러셀을 아이템 리스트와 결제 요약 사이에 신규 추가(Figma 실행) — 실제 화면과 동일한 위치·구조.

### 이번 피드백에서 얻은 인사이트
1. **오토레이아웃 FILL은 부모 padding이 없으면 여백을 통째로 삼킨다** — `layoutSizingHorizontal=FILL`을 쓸 때 부모 컨테이너에 padding이 있는지 항상 먼저 확인해야 한다. 토큰(16px 마진) 자체는 문서화돼 있었지만, 그걸 오토레이아웃 구조로 정확히 구현했는지는 스크린샷으로 실측해야 알 수 있었다.
2. **"1px divider=섹션 내부 구분"이라는 §6 서술은 폭(풀블리드 vs 인셋)까지는 규정하지 않는다** — 실측 필요.
3. **design.md 준수와 좋은 제품 판단은 별개다** — 토큰·컴포넌트 규칙을 다 지켜도 비즈니스 목적(추가 구매 유도)을 놓칠 수 있다. 이미 존재하는 컴포넌트(`Item Card / Recommendation`)로 해결 가능한 문제였는데도, "요청받은 체크리스트"에만 집중하느라 놓쳤다.

### design.md에 반영한 것 / 안 한 것
- **반영**: 인사이트 2 → §6 "패턴 2" 서술에 divider 폭 규칙(풀블리드 360 vs 인셋 328) 추가.
- **미반영**: 인사이트 1(오토레이아웃 FILL+padding 체크)과 인사이트 3(제품 판단 관점)은 Figma 구현 기법/작업 습관에 가까워 design.md(디자인 규칙 문서)보다는 이 파일과 향후 작업 방식에 반영하는 게 맞다고 판단 — design.md는 건드리지 않았다.
