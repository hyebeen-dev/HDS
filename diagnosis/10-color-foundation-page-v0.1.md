# Figma 작업 완료 기록 — Color Foundation 문서화 페이지 신설

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**참고 레퍼런스** Wanted Design System — Color · Semantic (https://www.figma.com/design/ag3TPH0TszRibzs01gwCmf, node 15625:32983)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`

---

## 배경

사용자가 Wanted 디자인 시스템의 Color 문서화 페이지를 참고해, 우리 파일에도 색상을 스와치로 시각 문서화한 페이지를 만들어달라고 요청했다. Wanted 페이지 구조(타이틀+설명 → Role별 스와치 그리드, 각 스와치는 정사각형 컬러칩 + 라벨)를 조사해 동일한 시각 레이아웃을 적용하되, 토큰 이름·그룹화는 Wanted 체계로 재매핑하지 않고 우리가 이미 구축한 체계를 그대로 사용했다.

## 실행 내용

새 Figma 페이지 **"🎨 Color"** 를 만들고 프레임 2개를 배치했다.

### Color · Semantic (22개 스와치)
Emphasis(3) / Commercial(3) / Text(4) / Background(3) / State(5) / Feedback(4) — 전부 실제 `Color/Semantic/*` Variable을 fill로 바인딩. 라벨은 하위 토큰 이름만 표기(Wanted 방식과 동일 — 시맨틱 토큰은 이름 자체가 정보이므로 hex 생략).

### Color · Primitive (78개 스와치)
Gray / Primary / Secondary / Point / Error / Warning / Success (각 50~1000, Secondary는 50 없이 100~1000) / Static(0, 1000) — 전부 실제 `Color/Primitive/*` Variable을 fill로 바인딩. 라벨은 스텝 번호 + hex 값 2줄로 표기.

## 실행 중 발견/수정한 문제

Primitive 프레임의 스와치 grid(`Tokens` 행, `layoutWrap=WRAP`)를 만들 때 `resize(1200, 80)`으로 너비만 고정하려다 높이도 80으로 같이 고정되면서, 스와치(56px 컬러칩 + 라벨 2줄 = 92px)의 hex 라벨 부분이 잘려 안 보이는 문제가 있었다. `counterAxisSizingMode`를 `AUTO`로 바꿔 높이가 내용에 맞게 늘어나도록 수정했다.

## 폰트 관련

이 실행 환경에 `Pretendard`가 아직 설치되어 있지 않아(사용자가 폰트 파일은 프로젝트 폴더에 추가했으나 macOS에 미설치 상태로 확인됨), 이 페이지의 모든 텍스트는 **Inter**로 생성했다. Pretendard를 macOS에 설치하고 Figma를 재시작한 뒤, 이 페이지의 텍스트 레이어를 Pretendard로 일괄 교체하면 된다(내용·구조는 그대로 유지).

## 검증

- Semantic 프레임 스크린샷 — 22개 스와치 전부 올바른 색상과 라벨로 렌더링 확인
- Primitive 프레임 스크린샷 — 78개 스와치 전부 올바른 색상·스텝·hex로 렌더링 확인 (hex 클리핑 버그 수정 후 재확인)
- `get_variable_defs`로 Semantic 프레임 전체를 재조회해 22개 전부 하드코딩이 아닌 실제 `Color/Semantic/*` Variable 참조임을 확인

## 다음 단계 후보

1. Pretendard 폰트 설치 후 이 페이지 텍스트를 Pretendard로 교체
2. Typography, Spacing Foundation도 동일한 스와치/스펙 문서화 페이지로 확장 (Variables 패널 대신 시각적으로 확인 가능하게)

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Color · Semantic(22개), Color · Primitive(78개) 스와치 문서화 페이지 신설, 전부 실제 Variable 바인딩 |
