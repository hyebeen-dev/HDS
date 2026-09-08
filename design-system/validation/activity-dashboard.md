# UI 검증 — "개인 활동 대시보드" 화면 (비커머스 도메인 2차 검증: 데이터/분석형 UI)

**목적**: design.md가 새로운 도메인과 새로운 정보 구조(지표·차트·상태가 섞인 대시보드)에서도 일관된 디자인 판단을 내릴 수 있는지 검증. 예쁜 화면이 아니라 판단이 막히는 지점을 정확히 드러내는 것이 목표.

**Figma**: 메인 화면 https://www.figma.com/design/y8OcE4JLKi7ADIPCVFJQej/HDS_2609?node-id=6789-603 / Empty 상태 데모 https://www.figma.com/design/y8OcE4JLKi7ADIPCVFJQej/HDS_2609?node-id=6793-7787 ("Ⓝ Note" 페이지)

**조건**: 특정 서비스 대시보드를 참고·복제하지 않음. design.md → Foundation/Variables → 기존 Components → 기존 화면 실사용 방식 → 최소 추론 순으로 판단.

## 1. 잘 적용된 규칙
- **Tab Group을 기간 선택(전체/이번 주/이번 달)에 재사용** — §4 원칙 10("Tab Group은 같은 데이터를 다른 관점으로 재구성해 보여줄 때")이 정의한 목적과 "같은 활동 데이터를 기간별로 다르게 본다"가 정확히 일치. 커머스 밖에서도 가장 자연스럽게 맞아떨어진 재사용 사례.
- `App Bar`(Type=Title, 아이콘+짧은 타이틀), `Section Header`(Show Action 유무로 "전체보기" 노출 제어), `Action Bar`/`Button` 계열, `Thumbnail`(xsmall, 리스트 아이콘), `Icon/Chevron Right`(상세 이동 어포던스) — 전부 그대로 재사용.
- 색상: 새 색 0개. 가격 강조 규칙(Error/Gray, 파랑은 행동 전용)은 이번 화면에 해당 사항이 없었지만, **`Feedback/Success`=파랑 렌더링 규칙(v0.30에서 확정)을 실제로 적용**해 "완료" 상태 표시를 초록이 아니라 파랑으로 처리.
- Typography: `heading/large`(화면 제목), `heading/small`(지표 숫자), `body/*`(본문·타임스탬프), `label/xsmall-medium`(상태 pill) — 전부 기존 스케일에서 선택, 새 크기/weight 없음.
- Spacing: 16(화면 마진)/8(카드 내부, 지표 카드 사이 gutter) 그대로 사용.
- Surface/Elevation: §1.6 "그림자 대신 선으로 깊이 표현" 원칙을 지표 카드·차트 카드에 그대로 일반화 — 배경 채움이나 그림자 없이 1px stroke(`Background/Divider`)+`radius/medium`만으로 카드 경계를 표현.

## 2. 판단이 어려웠던 부분 (design.md에 규칙 없음, 추론)
- **차트 막대 색상**: design.md에 데이터 시각화 색상 규칙이 전혀 없음. `Primary`(파랑, 행동 전용)를 쓰면 기존 색상 규칙 위반이라 배제하고, 대신 `Price Block`의 "할인 미적용 시 최종가=`Gray/900`" 관례(중립적이지만 중요한 강조)를 일반화해 차트 막대에 `Gray/900`을 사용 — **근거 있는 추론이지 확정된 답은 아니다.**
- **지표(KPI) 숫자의 강조 수준**: "큰 숫자+작은 라벨" 조합 자체가 design.md에 선례가 없어, `heading/small`(Bold)+`body/xsmall`(Description 색상) 조합으로 최소 추론. Rating Display의 "숫자+보조텍스트" 시각적 위계를 참고했지만 명시적 근거는 아니다.
- **상태 pill의 배경색**: `완료`/`진행중`/`실패` 텍스트 색상은 Feedback 토큰으로 해결했지만, pill의 배경(`Gray/100`)은 design.md 어디에도 근거가 없어 가장 중성적인 회색을 임의 선택.
- **활동 리스트 행 구조**: `List`(§2.10) 컴포넌트를 확인했으나 실제 구조가 Thumbnail+텍스트 한 줄뿐이라 타임스탬프·상태 pill·chevron을 못 담는다 — `List`를 그대로 쓰지 않고 `Thumbnail`+`Icon/Chevron Right`+raw 텍스트를 조합해 별도 행을 구성(컴포넌트 조합이지 신규 컴포넌트는 아님).

## 3. 재사용한 Component
`App Bar`(Type=Title), `Tab Group`(Type=3 Tab), `Section Header`(Show Action True/False), `Thumbnail`(Size=xsmall), `Icon/Chevron Right`. 5개, 전부 텍스트/프로퍼티 override만 하고 새 Variant는 추가하지 않음.

## 4. 새롭게 필요한 UI
- **지표(Stat) 카드**: 대응 컴포넌트 없음 — Foundation 토큰(Typography/Spacing/Radius/Stroke)만으로 raw 구성.
- **차트/시각화**: 대응 컴포넌트·색상 규칙 전혀 없음 — 막대 7개짜리 최소 바 차트를 raw로 구성, 규칙 없이 만든 것이라 절대 "정답"으로 취급하면 안 됨.
- **비인터랙티브 상태 pill**: 영화 화면 검증 때 발견한 것과 동일한 공백(Label/Chip 둘 다 안 맞음)이 이번에도 그대로 재현됨 — raw pill로 재구성.
- **Empty 상태**: 컴포넌트·패턴 전무(§5에 이미 등록된 백로그) — 원형 아이콘 placeholder + 텍스트 2줄로 최소 구성, 완전히 새로 지어낸 것.
- **Loading/Skeleton 상태**: 이번 테스트에서 아예 만들지 않음 — 현재 Design System으로는 해결 불가로 판단(색상·형태 근거가 전혀 없어 만들면 순수 창작이 될 뿐). §5에 이미 등록된 백로그와 일치, 재확인만 함.
- **Error 상태**: 별도로 만들지 않음 — `Message Box`(Status=Error)가 이미 검증된 컴포넌트라 그대로 재사용 가능하다고 판단(안내 배너로서의 의미가 "차트를 불러오지 못했습니다" 같은 문구와 호환됨).

## 5. 디자인 언어 일관성
기존 상품 목록/카테고리/상품 상세/영화 상세 화면과 같은 서비스처럼 느껴진다. Status Bar 44px, 16px 마진, App Bar/Section Header 톤, Typography 위계가 전부 동일해 "같은 디자인 시스템에서 나온 화면"이라는 인상은 유지된다. 다만 지표 카드·차트의 "1px stroke 카드" 스타일은 이번에 처음 등장한 표현이라, 기존 화면 어디에도 선례가 없다는 점은 유의해야 한다(정합성이 있어 보이지만 사실은 이번에 새로 정한 규칙).

## 6. design.md의 부족한 부분 (이번 테스트에서 실제로 발견된 것만)
- **데이터 시각화(차트) 관련 규칙이 전무하다** — 색상·형태·축/범례 표기 전부 미정의. 커머스 화면에는 애초에 차트가 없어서 이 공백이 지금까지 드러난 적이 없었다.
- **KPI/통계 숫자 표시 패턴이 없다** — "큰 숫자+작은 라벨" 조합에 대한 타이포그래피·배치 규칙 없음.
- **비인터랙티브 상태/태그 표시 컴포넌트 부재**가 두 번째로 확인됨(영화 화면과 동일 공백, 재현성 있는 문제로 격상됨).
- **`List` 컴포넌트가 실제로는 매우 단순한 한 줄짜리라, 타임스탬프·상태·이동 액션이 있는 "리치 리스트 행" 요구를 충족 못 한다.**

## 최종 판단: A vs B

**A(특정 커머스 UI 설명 수준)에 더 가깝다.** 단, 예외를 명확히 인정해야 한다 — **Foundation(색상·타이포·spacing·radius)과 §0/§4/§6의 프로세스·구성 원칙은 B(일반적 디자인 언어)에 근접한 수준으로 잘 일반화된다.** 실제로 이번 화면에서 색상·타이포·spacing·화면구성 규칙은 단 하나도 어기거나 새로 만들지 않고 그대로 통했다. 문제는 **§2 Component의 절대다수가 상품/장바구니/리뷰 같은 커머스 콘텐츠에 종속돼 있다는 것**이다 — 세 번의 비커머스 검증(영화, 장바구니 재검증, 이번 대시보드)에서 재사용 가능했던 컴포넌트는 매번 App Bar/Section Header/Tab Group/Action Bar/Rating Display/Thumbnail/Message Box 같은 "구조·내비게이션" 계열뿐이었고, "콘텐츠를 실제로 담는" 컴포넌트(Item Card 계열, Price Block, Spec Row, Label, Chip, Rocket Badge, Status Badge, Review 계열)는 세 번 다 단 하나도 재사용되지 못했다. 문서 전체를 기준으로 보면 §2가 가장 크고 실질적인 섹션인데 그 대부분이 도메인 종속적이므로, "design.md가 일반적 디자인 언어를 정의한다"고 말하기엔 아직 이르다.

## 다음 검증 후보 (최대 3개)
1. **입력 중심 화면**(설정, 프로필 편집 등) — 텍스트 입력·토글·셀렉트 같은 입력 컨트롤이 design.md에 전무한 상태에서 폼 UI를 어떻게 구성하게 되는지 검증.
2. **알림/피드형 화면**(알림 센터, 활동 피드) — `Message Box`/`Toast` 외의 다양한 알림 패턴과 무한 스크롤·페이지네이션 같은 새로운 레이아웃 압박을 검증.
3. **멀티스텝 플로우/온보딩 화면**(설문, 단계별 입력) — 진행률 표시(progress indicator)와 여러 화면에 걸친 상태 유지가 필요한 상황에서 design.md의 판단력을 검증.
