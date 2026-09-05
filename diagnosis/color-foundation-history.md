# Color Foundation — 작업 이력

> 이 문서는 `diagnosis/02`, `10`, `18`, `19`(구 파일명)를 시간순으로 합친 이력 기록이다. 원본 내용은 그대로 보존했고, 병합 시 구분선과 출처 표기만 추가했다.
> **지금 유효한 최신 상태는 `design-system/status.md`를 참고.**

---

## 2026-09-01 — Primitive 정리 + Semantic Color 레이어 확장 (구 diagnosis/02)

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

---

## 2026-09-01 — Color Foundation 문서화 페이지 신설 (구 diagnosis/10)

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

---

## 2026-09-01 — Color · Semantic 페이지에 Delivery Badge(로켓 배지) 섹션 추가 (구 diagnosis/18)

# Figma 작업 완료 기록 — Color · Semantic 페이지에 Delivery Badge(로켓 배지) 섹션 추가

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**대상 노드** Color · Semantic 프레임 (`6092:3`)

---

## 배경

쿠팡 로켓 배지 시리즈(판매자로켓/로켓프레쉬/로켓직구/로켓내일) 색상을 Color Foundation 페이지에 추가해달라는 요청. 기존 "Commercial" Role은 광고 배지 하나짜리 색상군(Background/Text/Border 3토큰)이라 성격이 다른 4개 색상 계열을 담기에 맞지 않아, 별도 "Delivery Badge" Role 섹션을 신설했다.

## 실행 전 조사

실제 로켓 배지 컴포넌트(`roket_badge` 세트)의 4개 variant에서 배경/텍스트 색을 직접 조회한 결과:

| 배지 | 배경 | 텍스트 | 토큰 상태 |
|---|---|---|---|
| 판매자로켓 (Seller) | #FFE6DD | #EC6234 | 미토큰화 (컴포넌트에 하드코딩) |
| 로켓프레쉬 (Fresh) | #F0FFE3 | #56B801 | 미토큰화 |
| 로켓직구 (Global) | #EEE0FF | #9747FF | 미토큰화 |
| 로켓내일 (Tomorrow) | #DEF9FF | #269AD9 | `Color/Primitive/Secondary/600` 참조 (기존 Variable) |

4개 중 1개만 기존 Primitive를 참조하고 있었고, 나머지 3개는 이런 색상 계열(주황/초록/보라) 자체가 Primitive 팔레트에 없는 상태였다. 사용자가 "새 Primitive 색상군을 정식 생성"이 아니라 "실제 값 그대로 문서화만" 하는 쪽으로 확정 — 토큰 체계 확장은 이번 범위에서 제외.

## 실행 내용

Color · Semantic 프레임 맨 아래에 기존 Role 섹션들(Emphasis/Commercial/Text/Background/State/Feedback)과 동일한 레이아웃 패턴(Role > Tokens > Style)으로 "Delivery Badge" 섹션을 추가했다:

- 각 배지의 **실제 컴포넌트를 그대로 clone**해 스와치로 사용 — 배경색·텍스트색·아이콘까지 실제 렌더링 그대로 보여줌 (별도로 사각형 스와치를 만들지 않고, 실물 그대로 문서화)
- 스와치 아래에 hex 값(배경/텍스트)과 토큰화 상태를 라벨로 표기

## 실행 중 참고 사항

- 이 페이지의 기존 라벨들은 전부 Pretendard로 작성돼 있어 새 텍스트를 같은 폰트로 타이핑할 수 없었다. 배지 이름 자체는 clone한 실제 컴포넌트에 이미 포함된 텍스트를 그대로 재사용해 문제없었고, 새로 작성해야 했던 라벨(hex/토큰 상태)은 Noto Sans KR로 작성했다 — 페이지 전체 폰트와는 다르지만 기존 Grid/Spacing/Typography 페이지에서 써온 것과 동일한 임시방편이다.
- 조사 중 **"Success" Primitive 컬러 패밀리(50~1000)가 전부 동일한 값(#e9fce9)으로 등록돼 있는 것**을 발견했다 — 스케일이 아직 정식으로 채워지지 않은 상태로 보인다. 이번 작업 범위 밖이라 손대지 않았고, 별도로 확인이 필요해 보여 기록만 남긴다.

## 검증

- 스크린샷으로 Delivery Badge 섹션이 기존 Role 섹션들과 시각적으로 일관된 레이아웃(제목 + 4개 스와치 가로 배열)으로 추가됐는지 확인
- 4개 배지 모두 실제 아이콘·텍스트·배경색이 정상적으로 렌더링됨을 확인

## 다음 단계 후보

1. Orange/Green/Purple 계열을 정식 Primitive로 승격할지 여부 결정 (지금은 문서화만 된 상태)
2. Success Primitive 컬러 패밀리의 실제 스케일 값 채우기 (별도 확인 필요)

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Color · Semantic 페이지에 Delivery Badge Role 섹션 신설 (실제 배지 4종 문서화) |

---

## 2026-09-01 — Success Primitive 컬러 hex 라벨 오류 수정 (구 diagnosis/19)

# Figma 작업 완료 기록 — Success Primitive 컬러 hex 라벨 오류 수정

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**대상 노드** Color · Primitive 페이지, Success 컬러군 (`6095:284`)

---

## 배경

직전 작업(diagnosis/18)에서 Success Primitive 컬러군의 hex 라벨이 전부 `#e9fce9`로 동일하게 표시되는 것을 발견해 기록만 남겼었다. 사용자가 "잘못 건드려서 같은 컬러값이 복사된 것 같다"며 수정을 요청했다.

## 원인 조사

Variable(`Color/Primitive/Success/50~1000`) 자체의 실제 값을 직접 조회한 결과, **10단계 전부 서로 다른 정상적인 초록색 그라데이션 값**이 이미 들어있었다. 스와치 Rectangle들도 각각 올바른 Variable에 정확히 바인딩돼 있어 실제로는 정상 렌더링되고 있었다. 문제는 **hex 텍스트 라벨만** — 50 단계의 라벨("#e9fce9")이 나머지 9개 단계에 그대로 복사돼 있었다(스와치 색상과 라벨 텍스트가 불일치).

## 실행 내용

100~1000 단계의 hex 라벨 9개를, 각 라벨과 같은 그룹에 있는 Rectangle의 실제 바인딩된 색상값을 읽어 정확한 hex로 재계산 후 교체했다.

| 단계 | 이전(전부 동일) | 수정 후(실제 값 기준) |
|---|---|---|
| 50 | #e9fce9 | #e9fce9 (원래 맞았음, 미수정) |
| 100 | #e9fce9 | #bcf5bc |
| 200 | #e9fce9 | #8fef8f |
| 300 | #e9fce9 | #63e963 |
| 400 | #e9fce9 | #36e236 |
| 500 | #e9fce9 | #1dc948 |
| 600 | #e9fce9 | #169c16 |
| 700 | #e9fce9 | #107010 |
| 800 | #e9fce9 | #0a430a |
| 900 | #e9fce9 | #062d06 |
| 1000 | #e9fce9 | #031603 |

## 실행 중 참고 사항

라벨 텍스트가 Pretendard였고 이 실행 환경에서 Pretendard를 로드할 수 없어 `.characters`를 직접 수정하지 못했다. 기존 라벨을 삭제하고 같은 위치·크기·색상으로 Inter 폰트의 새 텍스트 노드를 만들어 교체하는 방식으로 우회했다(스와치 자체나 Variable은 건드리지 않음).

## 검증

- 스크린샷으로 50→1000까지 연한 초록에서 진한 초록으로 이어지는 정상적인 그라데이션과, 각 스와치 색상에 맞는 hex 라벨이 표시되는지 확인

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Success Primitive 컬러군 hex 라벨 9개 수정 (스와치·Variable 자체는 원래 정상이었음) |

---

## 업데이트 — Commercial 미사용 변수 삭제 + 로켓 배송 배지 4색 정식 등록 (2026-09-04, `design.md` 리뷰 중)

`design.md` 초안을 사용자가 검토하며 발견한 것 — Foundation Color 섹션 참고.

### Commercial(프로모션) 색상 삭제
`Color/Semantic/Commercial/Background`·`Text`·`Border` 3개 변수 전량 실사용 조회 결과 0건 확인 후 삭제. Foundation 문서의 대응 스와치(Rectangle+라벨, "Style" 프레임 3개)도 함께 제거. Semantic 변수는 22개 → 19개.

### 로켓 배송 배지 4색 정식 Variable 등록
그동안 `Rocket Badge`(구 `roket_badge`) 컴포넌트의 seller/fresh/global/tomorrow 4색이 컴포넌트에 하드코딩된 채 미등록 상태였다(`tomorrow`의 텍스트 색상만 예외적으로 `Secondary/600`에 바인딩돼 있었음). 사용자 확정("배지당 배경+텍스트 2개씩, `Rocket` 패밀리")에 따라 신규 Primitive 8개 등록:

| Type | Background | Text |
|---|---|---|
| Seller(판매자로켓) | `#FEF2E6` | `#EC6234` |
| Fresh(로켓프레쉬) | `#F0FFE3` | `#56B801` |
| Global(로켓직구) | `#EEE0FF` | `#9747FF` |
| Tomorrow(로켓내일) | `#DEF9FF` | `#269AD9` |

`Rocket Badge` 4 variant의 컴포넌트 배경·아이콘·텍스트 fill을 전량 이 변수들에 바인딩(아이콘 색상이 텍스트와 미세하게 다르던 것도 텍스트 변수로 통일). 실사용 49곳, 스크린샷으로 색상 변화 없음 확인.

또한 Foundation 페이지에 이미 존재하던 문서화 프레임(`Role` → `Badge`, `6373:32752`)이 이 4색을 **`Success`/`Error`/`Warning`/`Info`로 완전히 잘못 라벨링**하고 있던 것을 발견 — 실제 배송 유형과 무관한 이름이었다. `Seller`/`Fresh`/`Global`/`Tomorrow`로 정정하고 스와치를 신규 변수에 바인딩.

## 변경 이력(추가)

| 일자 | 내용 |
|---|---|
| 2026-09-04 | Commercial 미사용 변수 3개 삭제, 로켓 배송 배지 4색 `Rocket` Primitive 패밀리로 정식 등록(8개 변수) + 컴포넌트·문서 바인딩 정정 |
