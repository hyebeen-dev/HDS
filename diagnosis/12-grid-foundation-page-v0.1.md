# Figma 작업 완료 기록 — Grid 문서화 페이지 신설

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**참고 레퍼런스** Wanted Design System — Grid (https://www.figma.com/design/ag3TPH0TszRibzs01gwCmf, node 15625:57936)

---

## 배경

Color·Typography에 이어 Wanted의 Grid 페이지를 참고했다. Wanted는 Mobile(2컬럼)·Tablet(3컬럼)·Desktop(992/1100/1440 브레이크포인트) 멀티 브레이크포인트 체계였지만, 우리 PRD(`prd-v0.1.md` 1.3)는 "모바일 웹(Mobile-first)" 단일 플랫폼으로 범위를 명시하고 있어 **모바일 그리드만** 정의했다(사용자 확정).

## 실행 내용

새 Figma 페이지 **"📐 Grid"** 를 만들고 시각 스와치(Wanted 스타일의 핑크 컬럼 오버레이) + 스펙 표를 배치했다.

| 항목 | 값 | 근거 |
|---|---|---|
| Container | 360px | 파일 내 실제 화면 레퍼런스(product_list, product_detail)가 전부 360px |
| Margin | 16px (좌우) | Item Card list 밀도 실제 폭 328px = 360 − 16×2 (역산). 기존 Spacing 토큰 `16`과 일치 |
| Gutter | 8px | 사용자 확정 (기존 Spacing 토큰 `8`과 연결) |
| Columns | 4컬럼 | 사용자 확정 |
| Column Width | 76px | (360 − 16×2 − 8×3) ÷ 4 = 76 (계산값) |

## 검증

- 페이지 스크린샷으로 4컬럼 오버레이와 스펙 표가 계산값과 일치하는지 확인 (76px × 4 + 8px × 3 + 16px × 2 = 360px)

## 다음 단계 후보

1. Pretendard 설치 후 Color·Typography·Grid 3개 페이지 텍스트를 Pretendard로 일괄 교체
2. 실제 Product List 화면 제작 시 이 Grid 스펙에 맞춰 카드 배치 검증

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Grid 문서화 페이지 신설 — 모바일 단일 브레이크포인트(360/16/8/4컬럼) |
