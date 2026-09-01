# Figma 작업 완료 기록 — Typography 문서화 페이지 신설

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**참고 레퍼런스** Wanted Design System — Typography (https://www.figma.com/design/ag3TPH0TszRibzs01gwCmf, node 15625:54522)

---

## 배경

Color 페이지에 이어, Wanted 디자인 시스템의 Typography 페이지를 참고해 우리 파일에도 타이포그래피 스펙 문서 페이지를 만들었다. Wanted는 Display 1~3·Title 1~3·Heading 1~2·Headline 1~2·Body 1~2·Label 1~2·Caption 1~2까지 19단계로 Size/Line Height/Letter Spacing/3종 미리보기를 갖춘 정교한 체계였다. 우리는 `Text` Variable 컬렉션의 16개 항목(실사용 9개, 미사용/예약 7개)만 갖고 있어, 없는 단계를 지어내지 않고 **우리가 가진 16개 그대로** 표로 정리했다(사용자 확정).

## 실행 내용

새 Figma 페이지 **"🔤 Typography"** 를 만들고 표를 배치했다. 컬럼: `Name` / `Size` / `Line Height` / `Preview`.

- **Line Height**: 실사용 9개 항목(Heading/xlarge·large·medium·small, Body/large·medium·small·xsmall, Detail/xsmall)은 대응하는 레거시 Text Style에서 확인한 실제 줄간격 값(150%, Body/small만 140%)을 그대로 표기
- 미사용 7개(Display 3종, Navigation, Detail/large·medium·small)는 대응 값이 없어 **"— (미사용)"** 으로 정직하게 표기, 임의로 지어내지 않음
- **Preview**: 각 행을 실제 크기·줄간격으로 렌더링. "상품을 빠르게 비교하고 판단하세요 Aa 123" 문구로 통일(한글·영문·숫자 동시 확인 가능)

## 폰트 관련

이 실행 환경에 Pretendard가 아직 없어서 두 가지 대체 폰트를 썼다: 표의 라벨/헤더는 **Inter**, 실제 크기를 보여줘야 하는 **Preview 컬럼은 Noto Sans KR**을 사용했다(Inter는 한글 글리프가 없어 한글 미리보기가 불가능해 별도로 확인 후 선택). 페이지 상단 설명에 이 사실을 명시했다. Pretendard 설치 후에는 Preview 컬럼 텍스트를 Pretendard로 교체하면 실제 프로덕션과 동일하게 보인다.

## 검증

- 표 스크린샷으로 16행 전체가 올바른 크기·줄간격·프리뷰로 렌더링되는지 확인
- 실사용 항목의 Line Height 값이 레거시 Text Style과 일치하는지 확인 (예: Body/small = 14px/140%, 나머지는 150%)

## 추가 작업 — "적용 컴포넌트" 컬럼 (v0.2)

사용자 요청으로 각 크기가 실제 어느 컴포넌트에 쓰이는지 조사해 5번째 컬럼으로 추가했다. 💠 Components 페이지 전체를 스캔해 텍스트 스타일별 실제 사용 노드의 최근접 컴포넌트 이름을 전부 수집한 뒤 정리했다.

| Text Variable | 실제 사용 컴포넌트 |
|---|---|
| Body/large (19px) | Item Card, Price Block(판매가), Cart Item, Chip |
| Body/medium (17px) | Button(Large 라벨), Bottom Sheet, Dialog, Chip, App Bar |
| Body/small (14px) | App Bar, Chip, Tab, Bottom Sheet, Dialog, Toast, Button(Medium 라벨) |
| Body/xsmall (13px) | Chip, Badge, Spec Row, Rating Display, Item Card, Price Block(정가·단위가·할인배지), Cart Item, Info Label |
| Heading/large (26px) | Review 섹션 타이틀 |
| Detail/xsmall (12px) | Order Deadline, Review, Cart Item |
| 나머지 10개 | 미사용 (정직하게 표기, 임의 작성 없음) |

## 다음 단계 후보

1. Pretendard macOS 설치 후 Color·Typography 두 페이지의 텍스트를 Pretendard로 일괄 교체
2. Spacing Foundation도 동일한 시각 문서화 페이지로 확장

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Typography 문서화 페이지 신설, 16개 항목 Name/Size/Line Height/Preview 표로 정리 |
| v0.2 | 2026-09-01 | 실제 컴포넌트 사용처를 조사해 "적용 컴포넌트" 컬럼 추가 |
