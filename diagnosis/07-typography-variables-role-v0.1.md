# Figma 작업 완료 기록 — Typography Variables 값 정합성 + 역할(Role) 설명

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`

---

## 배경

Figma Variables에 이미 "Text" 컬렉션(사이즈 FLOAT 값)이 있었지만 파일 전체에서 사용 횟수 0회였고, 실제로 적용 중인 레거시 Text Style과 숫자 값이 어긋나 있었다(특히 Heading 계열: Variables 32/24/19/40 vs 실제 적용 26/22/18/30). 사용자가 "기획자·개발자가 Variables 페이지에서 각 요소의 스펙과 쓰임을 알 수 있게" 해달라고 요청해, 실제 적용 값 기준으로 Variables를 맞추고 각 변수에 역할(용도) 설명을 채웠다.

## 실행 내용

- **값 수정**: Heading/xlarge(40→30), Heading/large(32→26), Heading/medium(24→22), Heading/small(19→18), Body/small(15→14) — 전부 실제 적용 중인 레거시 Text Style 값 기준
- **삭제**: Heading/xsmall(17) — 대응하는 레거시 스타일이 없는 값이었음(Body/medium과 우연히 같은 숫자였을 뿐)
- **신규 추가**: Detail/xsmall(12) — 레거시 `xxsmall` 스타일이 실제로 8회 쓰이고 있었는데 대응하는 Variables 항목이 없었음
- **역할 설명(description) 16개 전체 작성**: 실사용 중인 5개(Body/large·medium·small·xsmall, Heading/large, Detail/xsmall)는 이번 세션에서 직접 확인한 실제 사용처(Price Block, Button, App Bar, Bottom Sheet, Badge, 리뷰 섹션 등)를 근거로 작성. 미사용 9개(Display 3종, Heading 2종, Navigation 1종, Detail 3종)는 "미사용·예약된 값"이라고 정직하게 표기 — 임의로 역할을 지어내지 않음

## 범위에서 제외한 것

폰트 패밀리·굵기·줄간격·자간은 이번에 Variables화하지 않았다(사용자 확정 — 사이즈 중심으로 범위 한정). 완전한 "Text Style" 토큰(굵기까지 포함)이 필요하면 별도 작업으로 진행한다.

## 검증

- Variables 재조회 결과 16개 항목의 이름·값·description이 계획한 표와 정확히 일치
- 이 컬렉션을 참조하는 노드가 원래 없었으므로(사용 0회) 화면에 미치는 시각적 영향 없음

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Typography Variables 값을 실제 적용 값 기준으로 정정, Heading/xsmall 삭제, Detail/xsmall 신규 추가, 전체 역할 설명 작성 |
