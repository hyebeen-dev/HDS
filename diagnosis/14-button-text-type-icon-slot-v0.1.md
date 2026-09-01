# Figma 작업 완료 기록 — Button: Text 타입 + 아이콘 슬롯 추가

**문서 버전** v0.1
**작업일** 2026-09-01
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**참고 레퍼런스** Wanted Design System — Button (node 16222:137705), 사용자 제공 쿠팡 상품 상세 캡처
**관련 계획** `/Users/hyebeen/.claude/plans/hazy-scribbling-creek.md`

---

## 배경

Button 컴포넌트에 텍스트 전용 타입과 아이콘 슬롯이 전혀 없었다. Wanted의 Button 구조(variant/color/size/icon 축)를 참고하고, 사용자가 제공한 실제 쿠팡 화면에서 텍스트 버튼 실사용 사례("샤워메이트 >", "할인받기 >", "절약 금액 기준")를 확인한 결과, 아이콘이 있을 때는 **텍스트 뒤(트레일링) 화살표** 패턴이 공통적이었다.

## 실행 내용

### 1. Icon/Chevron Right 컴포넌트 신규
순수 벡터(SVG)로 만든 16×16 화살표 아이콘. 폰트 의존 없이 제작 가능해 이 실행 환경의 Pretendard 미설치 제약과 무관하게 완성했다.

### 2. Button — Type=Text 추가 (Large, Default/Pressed)
- 배경·테두리 없음, 텍스트 색상 `Color/Semantic/Text/Accent`(Default) / `Primary/600`(Pressed)
- `Show Icon`(Boolean) + `Icon`(INSTANCE_SWAP) 속성으로 트레일링 아이콘 표시 여부 제어
- 라벨 텍스트는 이 환경에 Pretendard가 없어 새로 타이핑하지 못해, 기존 버튼에 실제로 있던 텍스트("바로구매", "총 1개 상품 구매하기")를 그대로 clone해서 재사용했다 — 실제 문구("할인받기" 등)로 교체는 Pretendard 설치 후 진행 필요

### 3. Primary/Large/Default에 아이콘 슬롯 시범 적용
같은 `Show Icon`/`Icon` 속성을 기존 Primary/Large/Default에도 추가(기본값 Show Icon=false로 기존 모습 유지). 토글 시 흰색 화살표가 텍스트 뒤에 나타나는 것을 인스턴스로 확인했다.

## 실행 중 발견·수정한 문제

1. **Button 세트에 이미 있던 중복 variant**: `Type=Primary, Size=Large, State=Default, Layout=1 Button`이 서로 다른 ID(`2167:15919`, `2183:7700`)로 두 번 등록되어 있었다. 이 중복이 `componentPropertyDefinitions` 조회와 `setProperties()` 호출을 모두 막고 있었다. 두 노드 모두 실사용 인스턴스가 0개임을 확인한 뒤, 아이콘 슬롯을 이미 추가한 쪽만 남기고 중복을 삭제했다.
2. **컴포넌트(variant) 자체에는 `addComponentProperty`를 직접 호출할 수 없음**: Figma Plugin API 제약으로, Variant Set에 속한 개별 컴포넌트가 아니라 **Set 자체**에 속성을 추가해야 한다는 것을 확인했다.
3. **속성 중복 생성**: 위 두 문제를 해결하는 과정에서 `Show Icon`/`Icon` 속성이 두 벌(`...#24`/`...#25`와 `...2#48`/`...2#62`) 생겼다. 실제 아이콘 레이어에 연결된 쪽만 남기고 미사용 쪽을 삭제, 이름도 "2" 접미사 없이 정리했다.
4. **아이콘 색상 미적용**: 아이콘을 흰색으로 재색상하려던 첫 시도가 실제로는 반영되지 않아, 파란 배경 위에 파란 아이콘이 겹쳐 안 보이는 상태였다. 재확인 후 직접 흰색(Static/0)으로 바인딩해 해결했다.

## 검증

- Text 타입(Default/Pressed) 스크린샷 — 배경·테두리 없이 텍스트+트레일링 화살표 정상 렌더링
- Primary/Large/Default 테스트 인스턴스로 `Show Icon` false→true 토글 — 기본은 아이콘 없음, 토글 시 흰색 화살표가 텍스트 뒤에 정확히 나타남을 확인
- Button 전체 프레임 재스크린샷 — 기존 10개 변형이 시각적으로 그대로 유지됨을 확인

## 다음 단계 후보

1. Pretendard 설치 후 Text 타입 라벨을 실제 문구("할인받기", "절약 금액 기준" 등)로 교체
2. Medium/Small 사이즈의 Text 타입, 그리고 Secondary/Line type 등 나머지 타입에도 아이콘 슬롯 확장
3. 쿠팡 접근이 막혀 있었으므로, 추후 접근 가능해지면 실제 버튼 패턴을 더 조사해 보강

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-01 | Icon/Chevron Right 신규, Button에 Type=Text(Large) 추가, Primary/Large/Default에 아이콘 슬롯 시범 적용, 기존 중복 variant 정리 |
