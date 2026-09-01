# Figma 작업 완료 기록 — 미착수 컴포넌트 체크리스트 구역 신설

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)

---

## 배경

기존 "to make" 메모(파일 내 원본 디자이너 메모)는 프로젝트 초기에 작성되어 이미 완료된 항목(Price Block, Spec Row, Rating Display 등)과 미완료 항목이 구분 없이 섞여 있었다. PRD 기반 Tier 1 점검(직전 대화)에서 확인한 최신 상태를 반영해, **아직 만들지 않은 컴포넌트만** 체크박스 형태로 추적할 수 있는 구역을 💠 Components 페이지에 새로 만들었다.

## 실행 내용

"📋 Component Checklist — 미착수" 구역을 기존 "to make" 메모 바로 아래에 배치. Tier 1/2/3 3개 블록, 총 18개 항목. 각 항목은 체크박스 + 컴포넌트 이름 + PRD 요구사항 ID(근거)로 구성했다.

- **Tier 1 (P0 필수, 3개)**: Section Header, App Bar Sticky, Action Bar
- **Tier 2 (있으면 좋음, 11개)**: Ad Container, Reason Label, Ratio Display, Confidence Indicator, Review Summary, Review Card(정식화 필요 — 현재 raw 목업만 존재), Evidence Chip, Anchor Navigation, Option Selector, Accordion, Chip/Filter(App Bar 내 chip이 유사 기능 수행 중이라 분리 검토 필요로 별도 표기)
- **Tier 3 (여유 시, 4개)**: Empty State, Skeleton, Carousel + Indicator, Icon Button

## 실행 중 발견한 것

Figma 파일에 저장된 페이지 이름이 API 조회 시 "💠 Components"가 아니라 "ㄴ Components"로 반환되는 인코딩 이슈를 발견했다(이모지가 다른 문자로 치환됨). 페이지를 이름으로 찾는 스크립트가 실패해, 이후 페이지 ID(`1:9`)로 직접 접근하도록 수정했다 — 향후 이 페이지를 다룰 때도 이름 매칭 대신 ID 사용을 권장한다.

## 검증

스크린샷으로 18개 항목 전체가 체크박스·이름·PRD 근거와 함께 올바르게 렌더링되는지 확인.

## 다음 단계 후보

각 컴포넌트를 완성할 때마다 이 체크리스트의 체크박스를 채우거나 항목을 제거해 최신 상태로 유지.

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | 미착수 컴포넌트 체크리스트 구역 신설 (Tier 1/2/3, 18개 항목) |
