# Figma 작업 완료 기록 — Review Card 네이밍 정리 + Status 속성 추가

**문서 버전** v0.1
**작업일** 2026-09-02
**대상 Figma 파일** Design System_0901 (fileKey `y8OcE4JLKi7ADIPCVFJQej`)
**대상 컴포넌트** Review / Card (`6013:12578`)

---

## 배경

사용자가 이미 작업해둔 Review Card(`Type=photo`/`Type=only text` 2 variant)의 내부 레이어가 대부분 Figma 자동 생성 이름이었고, 헤더의 리뷰 태그 배지("한달사용"/"재구매")가 Type 값에 종속된 고정 예시로만 존재했다. 사용자가 배지를 독립적인 `Status` 속성으로 분리하기로 확정.

## 실행 내용

### 1. 내부 레이어 네이밍 정리 (2개 variant, 총 43개 레이어)
`Frame 1430106611`→`header`, 두 번째 레벨 `user`→`info`, 아바타 `user`→`avatar`, `Price/ Review`→`meta`, 별점+배지 `review`→`rating-tag`, 배지 프레임→`status-badge`, 유저명+날짜 `review`→`user-meta`, 상품명 `text`→`product-name`, `Frame 1430106606`→`attributes`, 속성 행→`attribute-row`, 속성 항목→`attribute-item`, 구분선→`divider`, 본문(전체 문장이 레이어명이었음)→`body`, `photo_review`→`photo-group`, 버튼 인스턴스→`helpful-button`

### 2. Status 속성 추가 — Type × Status 2×2
- `type=photo`/`type=only text` → `Type=Photo`/`Type=Only Text` 대소문자 정리
- 기존 2개에 `Status=한달사용`(Photo)/`Status=재구매`(Only Text) 이름 보강
- 신규 2개(`Type=Photo, Status=재구매`, `Type=Only Text, Status=한달사용`) 생성 — 각각 기존 variant를 clone한 뒤 `status-badge` 프레임만 반대쪽 variant의 실제 배지로 교체(Pretendard 재입력 없이 실제 텍스트 재사용)

## 검증

- `componentPropertyDefinitions` 확인 — `Type`(Photo/Only Text), `Status`(한달사용/재구매) 2개 독립 속성으로 정상 등록
- 4개 variant 스크린샷으로 사진 유무 × 배지 값 조합이 모두 올바르게 렌더링됨을 확인

## 변경 이력

| 버전 | 일자 | 내용 |
|---|---|---|
| v0.1 | 2026-09-02 | Review Card 내부 레이어 네이밍 정리, Status(한달사용/재구매) 속성 추가로 2×2 매트릭스 완성 |
