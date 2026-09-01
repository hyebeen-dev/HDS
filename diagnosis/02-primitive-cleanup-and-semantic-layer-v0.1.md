# Figma 작업 완료 기록 — Primitive 정리 + Semantic Color 레이어 확장

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`
**관련 진단** `01-figma-design-system-diagnosis-v0.1.md`

---

## 착수 전 재조사로 바로잡은 점

`01-figma-design-system-diagnosis-v0.1.md`는 `get_variable_defs`로 특정 노드에 바인딩된 변수만 조회한 결과였고, 실제로 Figma Variables 전체를 조회하니 다음이 이미 존재했다:

- **Color/Semantic 컬렉션이 이미 존재**: Text, Background, State, Feedback 카테고리. 다만 파일 전체에서 사용 횟수 0회(완전 미적용)
- **Spacing 컬렉션(4~64), Text 사이즈 컬렉션도 이미 존재** — 역시 미적용 상태 (이번 작업 범위 밖, 별도 착수 필요)
- "이중 Primitive"로 보였던 `color/light/...`, `white`는 사실 **Variables 이전에 쓰던 레거시 Paint Style**이었고, 대응하는 `Color/Primitive/...` Variable이 이미 거의 모든 값에 대해 정확히 일치하게 존재했음

## 실행 내용

### 1. Color/Primitive/Success 신규 이식
레거시 Style `color/light/success/5~95`(11단계, 초록 계열)을 `Color/Primitive/Success/50~1000`으로 이식. 값은 레거시에 있던 것을 그대로 옮겼을 뿐 새로 만들지 않음.

### 2. Semantic Color 오류 수정 및 확장
- `Color/Semantic/Feedback/Success`가 파랑(Primary)에 잘못 연결되어 있던 것을 새로 만든 `Success/500`(#1dc948)로 수정
- `Color/Semantic/Text/Discount`(커머스 용어, PRD UR-S2 위반) 삭제 → 의미를 Emphasis로 일반화해 흡수
- **Emphasis 3단계 신규 추가** (PRD UR-E1, Open Question Q-6 반영)
  - `Emphasis/Primary` → Point/500 (#f2460d)
  - `Emphasis/Secondary` → Gray/900 (#34373d)
  - `Emphasis/Ambient` → Gray/600 (#7b818e)
- **Commercial 카테고리 신규 추가** (PRD UR-E5 — 상업 콘텐츠와 제품 정보 구분)
  - `Commercial/Background` → Warning/50 (#fcf6e8)
  - `Commercial/Text` → Warning/700 (#8a610f)
  - `Commercial/Border` → Warning/300 (#f0c775)

Color/Semantic 컬렉션: 17개 → **22개 변수**.

### 3. 레거시 Paint Style → Variable 재바인딩 및 삭제
💠 Components 페이지 전체(1,902개 노드)를 스캔해 레거시 Style을 참조하던 fill/stroke 113건을 대응하는 `Color/Primitive/...` Variable로 재바인딩:

| 레거시 Style | → Variable | 비고 |
|---|---|---|
| color/light/gray/95 | Gray/1000 | 정확히 일치 |
| color/light/gray/60 | Gray/600 | 정확히 일치 |
| color/light/gray/5 | Gray/50 | `#fafafa`→`#f4f6f6`, 미세한 차이를 사용자 확인 후 통합 |
| color/light/primary/50·20·40 | Primary/500·200·400 | 정확히 일치 |
| white | Static/0 | 정확히 일치 |
| color/light/warning/50 | Warning/500 | 정확히 일치 |
| color/light/error/40 | Error/400 | 정확히 일치 |

재바인딩 완료 후 참조가 남지 않은 레거시 Paint Style **77개 전체 삭제**. 파일에 Paint Style이 하나도 남지 않은 상태 확인.

### 4. Button 컴포넌트에 Semantic 적용
Button(Large)의 fill/stroke/text 바인딩 중, **값과 의미가 모두 정확히 일치하는 경우에만** 기존 `Color/Semantic/State/*`, `Text/On Color`, `Text/Description` 토큰으로 재바인딩 (17건). Line type 계열의 보더 색처럼 대응하는 Semantic 토큰이 없는 경우는 임의로 매핑하지 않고 Primitive 참조를 그대로 유지.

`get_screenshot`으로 재바인딩 전후 렌더링을 확인 — 시각적 변화 없음(값을 그대로 alias했기 때문).

## 검증 결과

- 💠 Components 페이지 전체에서 레거시 Paint Style 참조 **0건**
- 로컬 Paint Style **0개** (전량 삭제 확인)
- `Color/Semantic/Feedback/Success` = `#1dc948`(초록)로 정상화 확인
- `Color/Semantic` 컬렉션 22개 변수 전체 값 재확인 완료
- Button 스크린샷 확인 — 의도치 않은 시각적 변화 없음

## 이번에 다루지 않은 것 (다음 단계 후보)

- **Spacing / Typography 크기 Variables 적용**: 이미 만들어져 있으나 파일 전체에서 미적용 상태. 이번 범위(색상)에 포함되지 않음
- **레거시 Text Style 정리**: `text/body/...`, `xxsmall`, 이름이 동일한데 ID가 다른 중복 Text Style(`text/body/small-medium` 2개, `Text/body/medium` vs `Text/Body/Medium` vs `text/body/medium` 등 크기값 불일치) 발견됨 — 색상 작업과 별개 이슈라 이번엔 손대지 않음
- **Point/500 안에 남아있는 "remote library shadow" 바인딩**: 극소수 노드가 로컬이 아닌 동일 이름의 외부 라이브러리 변수를 참조 중인 것으로 보이는 흔적 발견(예: `VariableID:9cc2f59.../2226:127` 형태의 ID) — 규모가 작아 이번엔 정리하지 않음
- **Emphasis/Commercial의 실제 컴포넌트 적용**: Foundation(Variables)만 만들었고, Ad Container·Badge·Item Card 등 실제 컴포넌트에 적용하는 건 다음 단계

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Primitive 정리 + Semantic Color(Emphasis/Commercial) 신규 확장 완료. Button 재바인딩 완료 |
