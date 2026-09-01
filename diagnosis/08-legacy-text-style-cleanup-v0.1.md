# Figma 작업 완료 기록 — 레거시 Text Style 중복 정리

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)

---

## 배경

사용자가 Figma Styles 패널에서 `text/body/medium`, `Text/body/medium`, `Text/Body/Medium`처럼 대소문자만 다른 중복 Text Style을 직접 발견하고 정리를 요청했다. 이는 앞서 `01-figma-design-system-diagnosis-v0.1.md`에서도 지적했던 문제다.

## 조사 결과 (전체 파일 스캔)

로컬 Text Style 20개 중 3개가 실사용 0회인 중복/고아였고, 별도로 텍스트 노드 4개가 로컬 스타일 목록에 존재하지도 않는 "고아 스타일 ID"를 참조하고 있었다(내가 이번에 만든 Quantity Stepper 숫자 텍스트).

## 실행 내용

- **삭제** (실사용 0회 확인 후):
  - `Text/body/medium` (15px, Regular) — `text/body/medium`(17px)의 오기재 중복
  - `Text/Body/Medium` (15px, Regular) — 위와 동일한 값의 추가 중복
  - `xsmall-regular` (14px/140%, Regular) — `text/body/small`(14px/140%, Regular)과 스펙이 완전히 같은 중복, 이름만 규칙에서 벗어나 있었음
- **이름 정정**: `xxsmall`(11회 실사용) → `text/body/xxsmall` — 다른 스타일이 전부 `text/body/*` 또는 `text/heading/*` 규칙을 따르는데 이것만 최상위에 있었음. 실제 사용 중인 스타일이라 이름만 바꾸고(ID는 유지) 적용된 노드에는 영향 없음

## 처리하지 못한 것

Quantity Stepper 숫자("1") 4곳이 참조하는 고아 스타일(15px, Medium, 150%)을 값이 가장 가까운 `text/body/small-medium`(14px, Medium, 150%)으로 재바인딩하려 했으나, **이 실행 환경에 Pretendard 폰트가 설치되어 있지 않아 텍스트 스타일 재할당 자체가 차단됐다**(글자를 바꾸는 게 아니라 스타일을 바꾸는 것도 동일하게 막힘). 이 4곳은 여전히 고아 스타일을 참조 중이며, Pretendard 폰트가 있는 Figma 데스크톱/웹 앱에서 `text/body/small-medium`으로 수동 재할당이 필요하다.

## 검증

- `getLocalTextStylesAsync()` 재조회 결과 17개 스타일 전부 `text/body/*` / `text/heading/*` 규칙을 따름
- 삭제된 3개는 삭제 전 실사용 0회를 확인했으므로 화면상 시각적 변화 없음
- `xxsmall` 이름 변경은 ID를 유지했으므로 11곳의 기존 적용에 영향 없음

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | 미사용 중복 Text Style 3개 삭제, `xxsmall` 이름 정정. Quantity Stepper 고아 스타일 재바인딩은 폰트 제약으로 보류 |
