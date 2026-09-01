# Figma 작업 완료 기록 — Item Card: Price Block / Rating Display / Spec Row 분리

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`
**관련 진단** `01-figma-design-system-diagnosis-v0.1.md`

---

## 실행 내용

### 1. Price Block 컴포넌트 신규 생성 — 완료, 24개 카드 전체 적용
grid_small(8) · grid(8) · list(8) 전체 24개 Item Card Variant의 raw "price" 프레임을 실제 프로덕션 레이어를 그대로 clone → `figma.createComponentFromNode`로 변환해 만든 **Price Block** 컴포넌트 인스턴스로 교체했다. `Show Discount`(Boolean) 컴포넌트 속성으로 정가·할인율 배지의 표시 여부를 제어하며, 값을 하나도 새로 만들지 않고 기존 레이어를 그대로 재사용했다.

- 카드별 `Sale=Y/N` 값에 따라 `Show Discount`를 true/false로 설정
- 재바인딩 후 전 카드 재조회 결과: **Price Block 인스턴스 24개, 잔여 raw price 프레임 0개**
- 스크린샷으로 재바인딩 전후 시각적 차이 없음을 확인

### 2. Rating Display 컴포넌트 신규 생성 — 완료 (컴포넌트만, 카드 적용은 보류)
`Review=Filled`(4.83 / (9,999+)) / `Review=Empty`(- / (0)) 2-Variant 컴포넌트 세트로 생성. 두 상태 모두 기존에 실제로 그려져 있던 텍스트 레이어를 그대로 clone해서 만들었다(새 텍스트를 타이핑하지 않음). `Filled` variant는 추가로 `Rating`/`Review Count` TEXT 속성을 노출해, Figma 데스크톱 앱처럼 폰트가 설치된 환경에서 임의의 평점 값을 넣을 수 있게 해뒀다.

### 3. Spec Row 컴포넌트 신규 생성 — 완료 (컴포넌트만, 카드 적용은 보류)
`Type=Rocket/Free/Today/None` 4-Variant + `Show ETA`/`ETA Text`/`Show Reward`/`Reward Text` 속성으로 생성. 실행 중 발견한 것: 기존 카드들은 "무료배송"을 카드마다 다른 스타일(알약 배지 vs 일반 텍스트)로 표현하고 있었음 — 사용자 확인 후 **알약 배지 스타일로 통일**했다. "오늘출발"도 같은 배지 스타일로 새로 만들되, 텍스트 자체는 기존에 실제로 그려져 있던 "오늘출발" 텍스트 레이어를 그대로 clone해서 재사용했다(새로 타이핑하지 않음).

## 이번에 다루지 못한 것 — 실행 환경 한계

**이 세션의 Figma 실행 환경(`use_figma`)에 `Pretendard` 폰트가 설치되어 있지 않다.** 이 파일의 모든 텍스트가 Pretendard를 쓰기 때문에, **텍스트 내용을 새로 입력하거나 기존 텍스트 값을 변경하는 조작은 이 세션에서 전부 불가능**했다(`figma.loadFontAsync`가 "Pretendard 폰트가 존재하지 않는다"는 에러로 실패). 기존에 이미 그려진 텍스트 레이어를 통째로 clone하는 것은 가능했지만(Type/Review Variant를 이렇게 만들었다), 카드마다 다른 배송 예정 문구("모레(수) 도착 예정" vs "내일(화) 도착 보장" 등)를 인스턴스별로 정확히 재현하려면 텍스트 속성값(`setProperties`)을 바꿔야 하는데, 이게 막혀 있었다.

이 때문에:
- **Rating Display, Spec Row 컴포넌트는 만들었지만, 24개 카드에는 아직 적용하지 않았다.** 사용자 확인 후 이번 범위를 Price Block으로 한정했다.
- 실제 카드 적용은 **Pretendard 폰트가 설치된 환경(Figma 데스크톱/웹 앱)에서 진행해야 한다.** 각 카드가 어떤 Type/Review/ETA Text/Reward Text 값을 가져야 하는지는 이번 조사 과정에서 24개 카드 전부 파악해뒀다 (아래 표).

### 카드별 Spec Row/Rating Display 적용값 참고표

| 카드 | Review | Spec Row Type | Show ETA | ETA Text | Show Reward |
|---|---|---|---|---|---|
| grid_small sale=Y,express=Y,review=Y | Filled | Rocket | false | — | false |
| grid_small sale=Y,express=Y,review=N | Empty | Rocket | false | — | false |
| grid_small sale=Y,express=N,review=Y | Filled | None | false | — | false |
| grid_small sale=Y,express=N,review=N | Empty | None | false | — | false |
| grid_small sale=N,express=Y,review=Y | Filled | Rocket | false | — | false |
| grid_small sale=N,express=Y,review=N | Empty | Rocket | false | — | false |
| grid_small sale=N,express=N,review=Y | Filled | None | true | 모레(수) 도착 예정 | false |
| grid_small sale=N,express=N,review=N | Empty | None | true | 모레(수) 도착 예정 | false |
| grid sale=Y,free=Y,review=Y (×2 중복) | Filled | Free | true | 모레(수) 도착 예정 | false |
| grid sale=Y,free=Y,review=N | Empty | Free | true | 모레(수) 도착 예정 | false |
| grid sale=Y,free=N,review=N | Empty | None | true | 모레(수) 도착 예정 | false (+로켓배지는 별도 확인 필요) |
| grid sale=N,free=N,review=Y (×2 중복) | Filled | Free | true | 모레(수) 도착 예정 | false |
| grid sale=N,free=N,review=N | Empty | Free | true | 모레(수) 도착 예정 | false |
| list sale=Y,free=Y,badge=Y,review=Y | Filled | Rocket | true | 내일(화) 도착 보장 | true |
| list sale=Y,free=Y,badge=Y,review=N | Empty | Rocket | true | 내일(화) 도착 보장 | true |
| list sale=Y,free=Y,badge=N,review=Y | Filled | Free+Today 조합* | — | — | true |
| list sale=Y,free=Y,badge=N,review=N | Empty | Free+Today 조합* | — | — | true |
| list sale=Y,free=N,badge=Y,review=Y | Filled | None | false | — | true |
| list sale=N,free=N,badge=N,review=Y | Filled | Today | false | — | false |
| list sale=N,free=N,badge=Y,review=Y | Filled | Rocket | true | 내일(화) 도착 보장 | true |
| list sale=N,free=N,badge=Y,review=N | Empty | Rocket | true | 내일(화) 도착 보장 | true |

`*` 표시된 2건(`list badge=N, free=Y`)은 원본이 "무료배송・오늘출발"을 한 줄에 같이 쓰고 있어 Spec Row 하나로 그대로 표현이 안 된다 — 실제 적용 시 판단이 필요.

**grid density는 원본 파일 자체에 pre-existing 결함이 있었다**: `sale=Y,free=Y,review=Y`와 `sale=N,free=N,review=Y`는 각각 노드가 2개씩 중복 존재하고, `sale=N,free=Y` 조합은 아예 없다(8개 중 6개 조합만 실존). 이번 작업에서 이 결함은 건드리지 않았다 — Variant 매트릭스 정리는 별도 작업으로 남겨둔다.

## 검증 결과

- Price Block 인스턴스 24개 확인, 잔여 raw price 프레임 0개
- 3개 밀도(grid_small/grid/list) 전체 스크린샷 — 교체 전후 시각적 차이 없음
- Cart Item은 계획대로 손대지 않음

## 다음 단계 후보

1. Pretendard 폰트가 설치된 환경에서 위 참고표대로 Rating Display/Spec Row를 24개 카드에 적용
2. grid density의 중복/누락 Variant 조합 정리
3. Tier 1 미착수 컴포넌트(Section Header, App Bar Sticky, Action Bar) 착수

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Price Block/Rating Display/Spec Row 컴포넌트 신규 생성. Price Block은 24개 카드 전체 적용 완료. Rating Display/Spec Row는 폰트 제약으로 컴포넌트 생성까지만 완료 |
