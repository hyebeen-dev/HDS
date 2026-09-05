# Typography Foundation — 작업 이력

> 이 문서는 `diagnosis/07`, `08`, `11`, `29`, `30`(구 파일명)를 시간순으로 합친 이력 기록이다. 원본 내용은 그대로 보존했다. **07의 역할 정의는 실제 Text Style이 아니라 별도 Variable(사이즈만)에 대한 것이었고 29에서 이 사실이 밝혀졌다. 11이 만든 Typography 페이지 구조는 29→30에서 두 번 재구성됐다 — 지금 유효한 건 30의 6카테고리 구조뿐이다.**
> **지금 유효한 최신 상태는 `design-system/status.md`를 참고.**

---

## 2026-09-01 — Typography Variables 값 정합성 + 역할(Role) 설명 (구 diagnosis/07, ⚠️ 이후 29에서 "잘못된 대상"이었음이 밝혀짐)

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

---

## 2026-09-01 — 레거시 Text Style 중복 정리 (구 diagnosis/08)

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

---

## 2026-09-01 — Typography 문서화 페이지 신설 (구 diagnosis/11, ⚠️ 페이지 구조는 이후 29→30에서 재구성됨)

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

---

## 2026-09-02 — 텍스트 스타일 체계 전면 정리 (용도 정의 + 미등록 값 토큰화) (구 diagnosis/29)

# Figma 작업 완료 기록 — 텍스트 스타일 체계 전면 정리 (용도 정의 + 미등록 값 토큰화)

**문서 버전** v0.1
**작업일** 2026-09-02
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`

---

## 배경

"소숫점 line-height" 조사 도중 사용자가 "② Component" 페이지 콘텐츠는 실제로 원티드 파일에서 가져온 게 맞다고 확인해주면서 그 조사는 정리됐다. 이후 사용자가 초점을 바꿔 요청: 원티드 얘기는 접어두고 **우리 디자인 시스템 자체의 텍스트 체계**를 정리하자 — (1) 정의되지 않은 텍스트 스타일이 실사용되고 있는 부분을 찾고, (2) 스타일별 용도를 정의하자는 요청.

## 조사 결과

- 로컬 Text Style **18개 전부 `description`(용도)이 비어있었다.** diagnosis/07에서 역할을 정의했던 건 별도의 "Text" **Variable**(사이즈 숫자만 있는 컬렉션)이었고, 실제 폰트/굵기/줄간격을 포함한 Text Style 객체 자체엔 용도 설명이 없었다.
- 이전에 "16px Bold가 새 토큰 필요"라고 봤던 건 착오였다 — `text/body/medium-bold`(16px Bold 150%)가 이미 있고 66곳에서 정상 바인딩돼 쓰이고 있었다.
- "ㄴ Components" 페이지(파일 내 유일한 실제 제품 콘텐츠 페이지, 839개 텍스트 노드) 전수 스캔 결과, 실제 문제는 두 갈래였다.

### A. 이미 있는 스타일인데 안 묶여있음 (재연결만 필요, 신규 토큰 불필요)
| 스펙 | 매칭 스타일 | 안 묶인 개수 |
|---|---|---|
| 14px Regular 140% | `text/body/small` | 47곳 |
| 11px Medium 150% | `text/detail/small` | 30곳 |
| 13px Regular 150% | `text/body/xsmall` | 15곳 (원티드 원격 스타일 참조 중) |

### B. 우리 스케일 어디에도 없던 진짜 갭
| 스펙 | 실사용처 | 개수 | 처리 |
|---|---|---|---|
| 15px Bold 150% | Section Header·Cart/Address Row·Cart/Selection Toolbar 제목 | 9 | 신규 토큰 |
| 15px Medium 150% | Quantity Stepper 숫자 | 7 | 신규 토큰 |
| 15px Regular 140% | 장바구니 결제금액 breakdown | 7 | 신규 토큰 |
| 15px Medium 140% | 상품명 1건 | 1 | 리듬 불일치 — 150%로 통일할지 확인 필요 |
| 17px Medium 150% | 카테고리 탭 | 3 | 16px로 통일 (사용자 결정) |
| 17px Bold 150% | "18,680원" 등 | 2 | 16px로 통일 (사용자 결정) |

## 사용자 결정 사항
- 17px 그룹: 정식 토큰 만들지 않고 **가까운 기존 16px로 통일**(빈도 낮고 시각 차이 미미)
- 15px 그룹: 정식 토큰화 확정, 이름은 `small-plus` 대신 **`text/body/compact`** 계열

## 실행 내용

### 1. 기존 18개 Text Style에 description(용도) 채움 — 자동 실행 완료
`TextStyle.description`은 메타데이터라 Pretendard 폰트 로드 없이 설정 가능함을 확인, 실사용 데이터 기반으로 18개 전체에 일괄 적용. 사용례가 없는 6개(`large-medium`, `large`(Regular), `medium`(Regular), `heading/small`, `heading/medium`, `heading/xlarge`)는 "미사용·예약된 값"으로 정직하게 표기(지어내지 않음).

### 2. Typography Foundation 페이지 전면 재구성 — 자동 실행 완료
기존 표는 diagnosis/07의 "Text" Variable(사이즈만, weight 구분 없음) 기준이라 실제 Text Style과 어긋나 있었다(예: 표엔 "Body/medium=17px"인데 실제 `text/body/medium`은 16px, "Detail/small"은 "미사용"인데 실제 30곳 사용 중). 사용자 확정 후 실제 18개 스타일 + 신규 3개(`compact` 계열, 아직 미생성) 기준으로 **Name/Size/Weight/Line Height/Preview/용도** 6컬럼, 21행으로 전면 재구성.

- 크기 내림차순(30→11px), 같은 크기 내 Bold→Medium→Regular 순 정렬
- 신규 `compact` 계열 3행은 용도 앞에 "(스타일 생성 대기)" 표기
- 라벨/값은 Inter, Preview는 Noto Sans KR 사용(Pretendard 로드 블로커로 기존과 동일한 대체 방식) — 스타일 확인 후 프로덕션에서는 Preview도 실제 Pretendard로 교체 필요

## 처리하지 못한 것 (사용자 수동 작업 필요, 전부 Pretendard 폰트 블로커)

### ① 신규 Text Style 3개 생성
| 이름 | 사이즈 | Weight | Line Height | Letter Spacing |
|---|---|---|---|---|
| `text/body/compact` | 15px | Regular | 140% | 0 |
| `text/body/compact-medium` | 15px | Medium | 150% | 0 |
| `text/body/compact-bold` | 15px | Bold | 150% | 0 |

### ② 재바인딩 대상 노드 (스타일 생성 후 진행)

**A그룹 — 기존 스타일로 재연결**
- `text/body/small`로: 47곳 — `I6005:6726;6005:3364`, `I6005:6750;6005:3364`, `I6005:6774;6005:3364`, `6051:648`, `6253:1682`, `6264:1400`, `6264:1415`, `6053:677`, `I6294:11580;6053:677`, `I6059:874;6059:644`, `I6059:907;6059:644`, `6067:667`, `6067:794`, `6013:12494`, `6013:12501`, `6013:12508`, `6013:12515`, `6013:12619`, `6013:12666`, `6289:1860`, `6289:1909`, `6018:12878`, `I6249:6832;6053:677`, `I6249:7213;6053:677`, `I6249:7240;6053:677`, `I6249:7241;6053:677`, `I6223:5384;6067:667`, `I6223:5386;6067:667`, `I6249:5972;6051:648`, `I6249:5974;6051:648`, `I6249:5976;6051:648`, `6249:9238`, `6249:9249`, `I6249:9129;6051:648`, `I6249:9131;6051:648`, `I6249:9133;6051:648`, `6249:9368`, `6249:9375`, `6249:9382`, `6249:9389`, `6249:9410`, `I6294:10124;6013:12619`, `I6294:10180;6013:12619`, `I6294:10234;6013:12619`, `I6223:4454;6059:644`, `I6223:4488;6059:644`, `I6223:4528;6059:644`
- `text/detail/small`로: 30곳 — `I6005:6726;6050:446;6036:451`, `I6005:6750;6050:446;6036:451`, `I6005:6774;6050:446;6036:451`, `I6051:649;6036:451`, `I6253:1683;6036:451`, `I6264:1401;6065:639`, `I6264:1416;6065:639`, `I6053:678;6036:451`, `I6294:11580;6053:678;6065:639`, `I6059:874;6059:645;6036:451`, `I6059:907;6059:645;6065:639`, `I6067:724;6036:451`, `I6067:816;6065:639`, `6036:451`, `6065:639`, `I6249:6832;6053:678;6036:451`, `I6249:7213;6053:678;6036:451`, `I6249:7240;6053:678;6036:451`, `I6249:7241;6053:678;6036:451`, `I6223:5384;6067:724;6036:451`, `I6223:5386;6067:724;6036:451`, `I6249:5972;6051:649;6036:451`, `I6249:5974;6051:649;6036:451`, `I6249:5976;6051:649;6036:451`, `I6249:9129;6051:649;6036:451`, `I6249:9131;6051:649;6036:451`, `I6249:9133;6051:649;6036:451`, `I6223:4454;6059:645;6036:451`, `I6223:4488;6059:645;6036:451`, `I6223:4528;6059:645;6036:451`
- `text/body/xsmall`로 (원티드 원격 스타일 참조 중 — ⚠️ 이후 30에서 이 15곳은 전부 링크 텍스트임이 확인되어 `text/underline/xsmall`로 재배정됨): 15곳 — `6314:12438`, `6314:12445`, `I6322:12914;6314:12438`, `I6328:1550;2102:6322`, `6328:1558`, `I6322:12908;6314:12445`, `6328:1569`, `I6328:1571;2102:6322`, `I6249:6066;6328:1558`, `I6249:6198;6328:1571;2102:6322`, `I6249:5970;6322:12914;6314:12438`, `I6249:9127;6322:12914;6314:12438`, `I6249:9309;6322:12914;6314:12438`, `I6249:9350;6322:12914;6314:12438`, `I6294:9921;2102:6322`

**B그룹 — 신규 `compact` 계열로 연결**
- `text/body/compact-bold`로: 9곳 — `6324:1544`, `6328:1568`, `6328:1575`, `I6249:6198;6328:1575`, `I6249:5970;6324:1544`, `6249:6105`, `I6249:9127;6324:1544`, `I6249:9309;6324:1544`, `I6249:9350;6324:1544`
- `text/body/compact-medium`로: 7곳 — `I6067:732;6067:626`, `I6223:5384;6067:732;6067:626`, `I6223:5386;6067:732;6067:626`, `6249:6130`, `6067:626`, `6294:11439`, `6294:11450`
- `text/body/compact`로: 7곳 — `6249:6107`, `6249:6108`, `6249:6110`, `6249:6111`, `6249:6113`, `6249:6114`, `6249:6120`
- 상품명 1건(`6249:8216`, 현재 15px Medium **140%**) — 다른 Medium들과 리듬이 안 맞음(150%가 표준). **150%로 통일해서 `compact-medium`에 연결할지 사용자 확인 필요.** 임의로 정하지 않음.

**C그룹 — 17px→16px 통일 (리사이즈 후 재연결)**
- `text/body/medium-medium`(16px)로: 3곳 — `6223:4064`, `I6249:6826;6223:4064`, `I6223:4296;6223:4064` (현재 17px Medium)
- `text/body/medium-bold`(16px)로: 2곳 — `6249:6131`(현재 17px Bold, 미바인딩), `6249:9409`("AI 리뷰 요약" — 이미 `text/body/medium-bold`에 바인딩된 채 17px로 수동 오버라이드된 상태라 오버라이드만 제거하면 됨)

### ③ Typography 페이지 헤더 설명 텍스트 오기재
`6108:1140` 노드가 Color 페이지 설명("Semantic 토큰이 참조하는 원시 컬러 스케일입니다…")을 그대로 복붙한 채 남아있음 — Pretendard라 자동 수정 불가, 사용자가 직접 "텍스트 사이즈·굵기·줄간격을 정의합니다…" 류의 설명으로 교체 필요.

## 검증
- `getLocalTextStylesAsync()` 재조회로 18개 전부 description 반영 확인
- Typography 페이지 스크린샷으로 21행 표(Name/Size/Weight/Line Height/Preview/용도)가 정상 렌더링됨을 확인
- 사용자가 신규 스타일 생성 + 재바인딩 완료 알려주면 전수 스캔 재실행해 UNBOUND/UNKNOWN_ID 잔여 건수 0 수렴 확인 예정

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-03 | 18개 Text Style description 채움, Typography 페이지 전면 재구성(21행), 재바인딩 대상 92+23+5곳 전수 파악 |

---

## 2026-09-03 — 텍스트 스타일 6대 카테고리 체계화 (구 diagnosis/30, ⚠️ 지금 유효한 최종 구조)

# Figma 작업 완료 기록 — 텍스트 스타일 6대 카테고리 체계화

**문서 버전** v0.1
**작업일** 2026-09-03
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`

---

## 배경

diagnosis/29에서 18개 Text Style의 용도를 채운 직후, 사용자가 카테고리 체계 자체를 `body`/`heading`/`detail` 3개에서 **display/heading/body/label/navigation/underline** 6개로 재편해달라고 요청. "ㄴ Components" 페이지 전체를 (사이즈/굵기/줄간격) 스펙별로 재스캔하면서 이번엔 각 텍스트 노드의 **가장 가까운 컴포넌트/인스턴스 조상**과 `textDecoration` 값까지 함께 수집해 실제 역할을 파악했다.

## 핵심 발견

같은 시각 스펙이 여러 역할로 동시에 쓰이는 경우가 다수 발견됐다(예: 16px Bold 150%는 Button 라벨과 Dialog 제목에 동시 사용, 14px Regular 140%는 Chip·Tab·Toast에 동시 사용). 사용자는 "카테고리별로 완전히 분리된 진짜 의미적 토큰"을 만드는 방식을 선택 — 시각적으로 동일해도 역할이 다르면 별개 스타일로 분리한다.

## 사용자 결정 사항
- Display: 실사용 없음 → 빈 채로 예약만 (임의 값 발명 금지)
- Underline: 실제 밑줄 걸린 건 "리뷰 9,999+" 1건뿐이지만, 더보기·전체보기·변경·선택삭제·알림받기·리뷰 전체보기 등 **링크 역할 텍스트 전부**를 대상에 포함
- 제목성 텍스트(Section Header, Sheet/Confirmation 제목, Cart Address Row/Selection Toolbar 제목, App Bar 타이틀) → heading으로 이동
- Label 범위: Chip, Badge(roket_badge, Cart/Countdown Badge), Spec Row (Rating Display·Order Deadline은 제외, body 유지)
- 공유 스펙은 카테고리별 완전 분리(중복이더라도 독립 토큰)

## 실행 내용

### 1. 이름만 변경 (자동 실행, 생성 불필요) — 완료
- `text/detail/small` → **`text/body/xxxsmall`** (역할 100% body라 이름만 이동)
- `text/body/xsmall-bold` → **`text/label/xsmall-bold`** (역할 100% label이라 이름만 이동)

### 2. 역할이 축소된 기존 6개 스타일 description 갱신 — 완료
`text/body/medium-bold`, `text/body/medium-medium`, `text/body/small-bold`, `text/body/small`, `text/body/xsmall-medium`, `text/body/xsmall` — 새로 분리되는 역할(Dialog 제목, App Bar 타이틀, Tab 라벨, Chip 라벨, Spec Row/Info Label, 링크 텍스트)을 설명에서 제외하고 "~로 분리" 메모 추가.

### 3. Typography Foundation 페이지 전면 재구성 — 완료
6개 카테고리 섹션(Display/Heading/Body/Label/Navigation/Underline)으로 재구성. 카테고리별 소제목 + 설명 추가. 아직 생성되지 않은 11개 신규 스타일은 이름을 주황색으로 표시하고 용도 앞에 "(스타일 생성 대기)" 표기.

## 처리하지 못한 것 (사용자 수동 작업 필요, 전부 Pretendard 폰트 블로커)

### ① 신규 Text Style 11개 생성

| 이름 | 사이즈 | Weight | Line Height | 비고 |
|---|---|---|---|---|
| `text/heading/xsmall` | 16px | Bold | 150% | Dialog/Sheet Confirmation 제목, "AI 리뷰 요약" |
| `text/heading/xsmall-medium` | 16px | Medium | 150% | App Bar 타이틀("상품리뷰"/"장바구니") |
| `text/heading/xxsmall` | 15px | Bold | 150% | Section Header, Cart Address Row/Selection Toolbar 제목 |
| `text/body/compact-medium` | 15px | Medium | 150% | Quantity Stepper 숫자 |
| `text/body/compact` | 15px | Regular | 140% | 장바구니 결제금액 breakdown |
| `text/label/small` | 14px | Regular | 140% | Chip 라벨 |
| `text/label/xsmall-medium` | 13px | Medium | 150% | Spec Row, Info Label |
| `text/navigation/small-bold` | 14px | Bold | 150% | Tab 선택 라벨 |
| `text/navigation/small` | 14px | Regular | 140% | Tab 비선택 라벨 |
| `text/navigation/medium` | 16px | Medium | 150% | App Bar 카테고리 표시("바디워시" 등, 원래 17px에서 통일) |
| `text/underline/xsmall` | 13px | Regular | 150% | 링크 텍스트 전부(더보기 등), textDecoration=Underline |

(letterSpacing은 전부 0)

### ② 재바인딩 — 신규 스타일 생성 후 진행

**heading/xsmall** (16 Bold, 제목 역할): dialog 컨텍스트 16곳 + Sheet/Confirmation 컨텍스트 6곳 + "AI 리뷰 요약"(`6249:9409`, 현재 17px 오버라이드 — 16px로 되돌리며 이 스타일에 연결) — 정확한 22곳 노드 ID는 재바인딩 직전에 dialog/Sheet Confirmation 서브트리를 다시 조회해 확정 예정(이번엔 컨텍스트 카운트만 확인, 개별 ID는 미수집)

**heading/xsmall-medium** (16 Medium, App Bar 타이틀): `6008:8725`("상품리뷰"), `I6249:5957;6008:8725`("장바구니") — **주의**: 같은 16px Medium 스펙의 `I6294:9804;6223:4064`("상품 상세")는 실제로는 마스터 노드 `6223:4064`("커피/차" 카테고리 라벨)의 오버라이드라 **navigation/medium 쪽 역할**일 가능성이 높음 — heading이 아니라 navigation으로 재분류 필요, 재바인딩 시 재확인.

**heading/xxsmall** (15 Bold): `6324:1544`, `6328:1568`, `6328:1575`, `I6249:6198;6328:1575`, `I6249:5970;6324:1544`, `6249:6105`, `I6249:9127;6324:1544`, `I6249:9309;6324:1544`, `I6249:9350;6324:1544` (diagnosis/29의 "new_15B150_compactBold" 목록과 동일 — 대상 스타일만 `text/body/compact-bold`에서 `text/heading/xxsmall`로 변경)

**body/compact-medium / body/compact**: diagnosis/29의 `new_15M150_compactMedium`(7곳)/`new_15R140_compact`(7곳) 목록 그대로 유효.

**label/small** (Chip): Components 페이지에서 `chip` 컴포넌트 컨텍스트의 14px Regular 140% 텍스트 82곳 — 정확한 ID 목록은 재바인딩 직전 Chip 서브트리 재조회 필요(이번엔 개수만 확인).

**label/xsmall-medium** (Spec Row/Info Label): Spec Row 컨텍스트 22곳 + Info Label 컨텍스트 7곳 = 29곳(13px Medium), 정확한 ID는 재바인딩 시 확정.

**navigation/small-bold / navigation/small** (Tab): 14px Bold tab 컨텍스트 다수 + 14px Regular tab 컨텍스트 16곳, 정확한 ID는 재바인딩 시 확정.

**navigation/medium**: `6223:4064`("커피/차", 17px→16px 리사이즈 필요), `I6249:6826;6223:4064`("바디워시"), `I6223:4296;6223:4064`("바디워시"), `I6294:9804;6223:4064`("상품 상세" — 위 참고).

**underline/xsmall**: diagnosis/29의 "13px Regular 150% 오ュ픈 15곳" 목록 전체(더보기/전체보기/변경/선택삭제/알림받기/리뷰 전체보기) + `6249:8225`("리뷰 9,999+", 이미 실제 밑줄 적용됨) = 16곳. **diagnosis/29 수정**: 이 15곳은 원래 `text/body/xsmall`로 재연결 예정이었으나 전부 링크 텍스트로 확인되어 `text/underline/xsmall`로 변경.

### ③ Spec Row의 13px Regular 150% 하위 텍스트(약 11곳) 재분류 미확정
Spec Row는 label로 분류했지만, 이 컴포넌트가 13px **Medium**(라벨)과 13px **Regular**(보조 텍스트로 추정) 두 굵기를 함께 쓰고 있어 Regular 쪽도 label로 옮길지 body에 남길지 실행 단계에서 노드별 텍스트 내용을 직접 확인 후 결정 필요 — 추정만으로 재바인딩하지 않음.

## 검증
- `getLocalTextStylesAsync()` 재조회로 18개 전부 description 반영 확인
- Typography 페이지 스크린샷으로 6개 카테고리 섹션 정상 렌더링 확인 (신규 11개는 주황색 "생성 대기" 표시)
- 사용자가 11개 스타일 생성 완료 알려주면 재바인딩 진행, 이후 전수 스캔으로 카테고리별 미분류 건수 0 수렴 확인 예정

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-03 | 6대 카테고리 체계 설계, 이름 변경 2건 완료, description 갱신 6건 완료, Typography 페이지 6섹션 재구성, 신규 스타일 11개 스펙 확정(생성은 사용자 수동) |

---

## 업데이트 — 15px Medium 텍스트 전수 리스트업 및 처리 (2026-09-04, `design.md` 리뷰 중)

`design.md` 초안 검토 중 사용자가 "15px Medium 텍스트가 어디 쓰이는지 리스트업하고 하나씩 처리"를 요청. `① Component` 페이지 전체(1,654개 TEXT 노드) 전수 조회 결과 15px Medium은 세 갈래로 나뉘어 있었다.

1. **완전 미바인딩(기존에 이미 알려졌던 문제)**: 상품명 "도브 화이트피치 리밸런싱 바디워시, 1kg, 1개" 3곳(`textStyleId` 없음, "★ 장바구니" 목업의 `product_info` 영역).
2. **신규 발견 — 이름은 같지만 외부 팀 라이브러리의 원격(remote) 스타일을 참조**: `Quantity Stepper` 숫자("1"/"5"/"999")와 "총 4,000원 할인" 텍스트(같은 장바구니 목업)가 `text/body/small-medium`이라는 이름의 스타일에 바인딩돼 있어 정상처럼 보였으나, `getStyleByIdAsync()`로 직접 조회한 결과 `remote: true` — 이 파일의 로컬 스타일이 아니라 외부 라이브러리 스타일이었다. 로컬 `text/body/small-medium`은 14px인데 이 원격 스타일은 15px로 값도 달라, 이름만 보고는 이 차이를 알아챌 수 없었다.
3. **정상**: `Icon Button`의 "도움이 돼요", `Button` placeholder 텍스트 — 로컬 `text/body/compact-medium`(15px Medium)에 정상 바인딩, 문제 없음.

### 처리
1·2번 그룹 총 12개 노드(상품명 3 + 원격 스타일 참조 9)를 전부 로컬 `text/body/compact-medium`(15px Medium, line-height 150%)에 재바인딩. 상품명은 기존에도 15px Medium이었고 line-height만 140%→150%로 정식 스타일값에 맞춰짐(사용자가 "compact-medium으로 변경해도 될 것 같다"고 직접 제안). 원격 스타일 참조 9개는 이미 시각적으로 15px/150%였던 값과 정확히 일치해 크기 변화 없이 참조만 로컬로 교체됨.

### 검증
- 전체 재스캔으로 15px Medium 텍스트 중 미바인딩·원격 스타일 참조 0건 확인
- "★ 장바구니" 목업 섹션 스크린샷으로 실제 화면 깨짐 없음 확인(가격 강조색이 파랑이 아니라 빨강인 것도 재확인 — Foundation Color (a) 항목과 일치)

## 변경 이력(추가)

| 일자 | 내용 |
|---|---|
| 2026-09-04 | 15px Medium 텍스트 전수 리스트업, 미바인딩 3곳 + 외부 라이브러리 원격 스타일 참조 9곳 발견 및 전부 로컬 `body/compact-medium`으로 재바인딩 |
