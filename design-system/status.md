# Design System 현황

디자인 시스템의 완성도와 검증 결과를 정리한 현황 문서입니다.

- 기준일: 2026-10-06
- 기준 문서: design.md v0.72
- Figma 파일: Design System_0901

---

## 1. 요약

- Foundation: 11개 영역 중 7개 완료, 3개 부분 완료, 1개 미착수
- Component: 체크리스트 15개 중 14개 완료, 체크리스트 밖 추가 컴포넌트 3개
- 화면 검증: 11개 화면(커머스 6개, 비커머스 5개)
- 핵심 결과: 11개 화면을 모두 신규 컴포넌트 없이 제작, 주문 내역 화면 제작에 2분 42초 소요

---

## 2. Foundation

| 영역 | 상태 | 내용 |
|---|---|---|
| Color · Primitive | 완료 | 8개 색상 패밀리와 도메인 전용 패밀리(`Rocket`, `QuickBadge`, `Reward`, `AI`, `Rating`) |
| Color · Semantic | 완료 | 31개 토큰, 전량 Primitive 참조 |
| Color · Rocket | 완료 | 로켓 배송 배지 4색을 변수 8개로 등록, `Rocket Badge` 4개 variant 전량 바인딩 |
| Typography | 완료 | Pretendard 단일 글꼴, 6개 카테고리 29개 스타일 |
| Spacing | 완료 | 16개 값, 컴포넌트 라이브러리 전체 padding과 gap 1,380곳 바인딩 |
| Grid | 완료 | 모바일 단일 브레이크포인트(컨테이너 360, 마진 16, 거터 8, 4컬럼) |
| Radius | 완료 | 5단계 토큰, 컴포넌트 라이브러리 전체 1,346곳 바인딩 |
| Border/Divider | 부분 완료 | 1px 행 구분과 8px 섹션 구분 패턴, 색상 규칙 확정. 독립 Divider 컴포넌트 없음 |
| Elevation | 부분 완료 | 그림자 대신 구분선과 여백으로 깊이를 표현하는 원칙 확정. `Toast`와 `Bottom Navigation`만 예외 |
| Icon | 부분 완료 | 기본 크기 16×16, 선 두께 1.5px. 미디어 액션 아이콘과 쿠폰 아이콘 없음 |
| Image | 미착수 | 비율 규칙과 깨짐 대응 아직 정의되지 않음 |

---

## 3. Component

완료 기준은 컴포넌트가 만들어져 있는지 여부입니다. 16~18번은 원래 체크리스트에 없던 추가 항목입니다.

| # | 항목 | 상태 | 포함 컴포넌트 |
|---|---|---|---|
| 1 | Badge | 완료 | `Rocket Badge`, `Status Badge` |
| 2 | Banner | 완료(정의 작성 전) | `Cart / Notification Banner`, `Cart / Countdown Banner` |
| 3 | Button | 완료 | `Button`, `Icon Button`, `Text Button` |
| 4 | Chip | 완료 | `Chip`, `Option Chips / Size`, `Option Chips / Thumbnail` |
| 5 | Dialog | 완료 | `Dialog` |
| 6 | Heading | 완료 | `App Bar`, `App Bar Small`, `Section Header`, `Cart / Address Row`, `Cart / Selection Toolbar` |
| 7 | Message Box | 완료 | `Message Box`, `Toast` |
| 8 | Item Card | 완료 | `Item Card / Grid`, `Item Card / Recommendation`, `Item Card / Cart`, `Price Block`, `Spec Row`, `Rating Display`, `Order Deadline`, `Review / Card`, `Review / Summary` |
| 9 | Label | 완료 | `Label` |
| 10 | List | 완료 | `List`, `Category Tab`, `Category Menu Item` |
| 11 | Navigation | 완료 | `Bottom Navigation`, `Bottom Navigation Item` |
| 12 | Bottom Sheet | 완료 | `Bottom Sheet / Informational`, `Bottom Sheet / Interactive` |
| 13 | Tab | 완료 | `Tab Group`, `Tab Item` |
| 14 | Thumbnail | 완료 | `Thumbnail` |
| 15 | Radio / Checkbox | 미완료 | `Icon / Radio`, `Icon / Checkbox` |
| 16 | Text Input (추가) | 완료 | `Text Input`, `Guide Row` |
| 17 | Text Area (추가) | 완료 | `Text Area` |
| 18 | Seller Row (추가) | 완료 | `Seller Row` |

각 컴포넌트의 상세 명세는 design.md §2를 참고하세요.

---

## 4. 화면 검증

design.md를 근거로 화면을 제작해 시스템이 실제로 쓰이는지 확인한 기록입니다.

| 일자 | 화면 | 도메인 | 주요 결과 |
|---|---|---|---|
| 2026-09-05 | 장바구니 | 커머스 | 구분선 폭 규칙 추가 |
| 2026-09-05 | 영화 상세 | 비커머스 | `Review / Summary`와 `Review / Card`를 영화 리뷰에 재사용 |
| 2026-09-05 | 개인 활동 대시보드 | 비커머스 | 차트, 통계 카드, 정보 표시용 태그 공백 확인 |
| 2026-09-07 | 마이페이지 | 커머스 | 기존 컴포넌트와 토큰 조합만으로 제작 |
| 2026-09-08 | 상품 상세 페이지 | 커머스 | 기존 컴포넌트 인스턴스만으로 제작 |
| 2026-09-18 | 펫 커머스 상품 리스트 | 커머스 | 다른 커머스 도메인에 기존 컴포넌트 적용 |
| 2026-09-30 | 검색 결과 | 커머스 | 화면 구성 패턴 1건 보완 |
| 2026-09-30 | 음악 스트리밍 앨범 상세 | 비커머스 | `Seller Row` 재사용, 미디어 아이콘과 콘텐츠 카드 공백 확인 |
| 2026-10-02 | 개인 일정 관리 주간 캘린더 | 비커머스 | 시간 그리드와 일정 블록 패턴 공백 확인 |
| 2026-10-02 | 스트리밍 서비스 모바일 홈 | 비커머스 | `Thumbnail` 와이드 비율 공백 확인 |
| 2026-10-03 | 주문 내역 | 커머스 | 기존 컴포넌트 전량 재사용, 제작 시간 2분 42초 |

Foundation과 화면 구성 규칙은 비커머스 도메인에서도 그대로 적용됐습니다. 반면 컴포넌트는 대부분 커머스에 맞춰져 있어, 비커머스 화면에서는 차트, 콘텐츠 카드, 시간 그리드 같은 공백이 확인됐습니다. 이 공백은 5절의 남은 과제로 이어집니다.

---

## 5. 남은 과제

### 컴포넌트 공백

- Checkbox와 Radio의 인터랙티브 버전(선택 해제 상태 variant 필요)
- Banner의 정의 미확정
- 아직 없는 컴포넌트: Divider, Accordion, Anchor Navigation, Carousel과 Indicator, Ratio Visualization, Empty State, Skeleton, Promotion Badge, Ad Container, Evidence Chip, Reason Label
- 결제·적립 정보와 상품 부가정보를 표시하는 `Info Row`
- 결제금액 내역 컴포넌트
- 상품문의와 배송정책 리스트 행
- 미디어 액션 아이콘(재생, 일시정지, 셔플, 하트)
- 가격·배송·평점 없이 이미지와 제목만 담는 콘텐츠 카드
- 차트와 데이터 시각화(막대그래프 색상 원칙만 확립, 범례와 축 규칙 없음)
- 큰 숫자와 작은 라벨을 조합하는 통계 카드
- 인터랙션 없는 상태·태그 표시 컴포넌트
- 시간 그리드와 일정 블록 패턴
- `Thumbnail`의 와이드(16:9) variant
- `Status Bar` 컴포넌트화

### 기존 컴포넌트 결함

- `Price Block`, `Spec Row`, `Rating Display`, 일부 버튼이 화면에서 인스턴스가 아닌 직접 구현 상태
- 카운트다운 표시가 장바구니 상단과 상품 행 내부에서 서로 다른 스타일
- `Section Header` 컴포넌트 세트의 중복본 존재
- `Action Bar`의 미정리 variant 이름
- `Dialog` 헤더 아이콘과 `Item Card / Cart` 삭제 아이콘이 정식 아이콘이 아닌 컴포넌트를 참조
- `Spec Row`의 쿠폰 적용 variant가 임시 아이콘 사용
- `Chip`의 `Show Icons` Boolean이 레이어에 연결되지 않음
- `App Bar` 필터 타입의 칩 개수 고정
- `Label`의 프로모션 타입 색상이 마스터와 실제 화면에서 불일치
- `Icon Button`의 자리표시 텍스트 상시 노출, `Button`의 아이콘 Boolean 미동작
- `Text Input`의 `Status` 기본값이 Active로 설정됨
- `Text Input` 편집 시 variant가 중복 생성되는 현상의 재발 위험

### Foundation

- Elevation 예외 2건의 공식 원칙화 여부
- `Text Input`의 padding과 gap을 Spacing 변수에 바인딩
- 구분선 레이어 이름을 두께별로 구분
- Grid의 캐러셀 영역 3열 규칙 보완
- Image의 비율 규칙과 깨짐 대응 정의

---

## 6. 관련 문서

- design.md: Foundation·Component 명세와 원칙
- design-language.md: 시각 스타일 관찰

---

## 변경 이력

| 날짜 | 요약 |
|---|---|
| 2026-09-03 | 최초 작성, Component 체크리스트 15개 확정 |
| 2026-09-04 | 전체 감사, Radius·Spacing 변수 바인딩 |
| 2026-09-05 | Message Box 제작, 화면 구성 패턴 정의, 첫 화면 검증 |
| 2026-09-06 | Text Input, Text Area 추가 |
| 2026-09-10 | Primitive 색상 이름 변경, 색상 바인딩 감사 |
| 2026-09-30 | Seller Row 추가, 비커머스 검증 확대 |
| 2026-10-02~03 | 주간 캘린더, 스트리밍 홈, 주문 내역 검증 |
| 2026-10-06 | 현황판 형태로 문서 재구성 |
