# CAMELIO QA BOOT

> 목적: 새 세션에서도 최소 Context로 까멜리오 QA 업무를 복구한다.
> 이 문서는 게임 사양 저장소가 아니라 Source 접근/판단 규칙을 정의하는 Router다.

## 0. Critical Safety

### Monday.com = READ-ONLY
허용: Board / Group / Item / Subitem / 상태 / 일정 / 담당 / Update / Reply / Asset 조회.
금지: Item/상태/담당/일정/Comment/Update/파일 등 모든 Monday Write.

### Git = REVIEW BEFORE WRITE
Git Read는 자유롭게 사용한다.
Git Create / Update / Delete / Commit은 반드시:
1. ChatGPT가 변경 초안 또는 Patch 제시
2. QA Owner가 최소 1회 검수
3. QA Owner가 명시적으로 승인
4. 승인된 범위만 반영
순서를 따른다.

### Google Drive Test Asset = BOUNDED WRITE
`QA_ACTIVE.md`의 현재 QA Test Spreadsheet는 QA Owner가 TC / CL / BVT 작성·수정을 요청한 경우에만 Write한다.

- 정확한 Spreadsheet Pointer 사용
- Workbook 전체 재생성 금지
- 기존 검증된 Sheet Shell 우선 재사용
- 필요한 Sheet / Range만 수정
- Write 후 Formula / Summary / Validation / Conditional Format / Merge / Layout 재확인
- Source 없는 Expected 추가 금지

## 1. Core Boot

QA 업무 시작 또는 현재 상태/이력/Coverage/Expected 판단이 필요하면:

1. `QA_BOOT.md`
2. `QA_CURRENT.md`
3. `QA_ACTIVE.md`
4. 현재 업무에 필요한 Live Source

순으로 읽는다.

과거 대화/모델 기억은 검색 방향 참고용이며 현재 사실이나 Expected 확정 근거로 사용하지 않는다.

## 2. Live Retrieval Contract

현재 상태, 이력, Coverage, 변경 영향, 관련 Issue, Expected 판단은 관련 Live Source가 있으면 먼저 조회한다.

### 기본 Source
- Monday Sprint / Feature / QA Item 및 Update / Reply
- Monday 기획서 Item과 최신 Design Asset
- Monday QA / 버그 Board의 관련 BTS
- Git Test Asset의 기존 Coverage / Open / Recheck Context

### 필요할 때만
Google Drive의 TC / BVT / Sprint Test는 다음 경우에만 읽는다.
- QA Owner가 조회 요청
- TC/BVT/CL 작성·수정이 목적
- 실행 가능한 Test Asset 상태가 현재 판단에 필요
- 기본 Source만으로 Coverage 판단이 불충분

Live Source와 기억이 충돌하면 Live Source를 우선한다.

## 3. Progressive Retrieval

가장 좁고 신뢰도 높은 Pointer부터 시작한다.

1. `QA_ACTIVE.md` 또는 정확한 Last-known ID
2. Git Test Asset의 Semantic Anchor / Alias
3. Parent / Subitem / Linked Item
4. 제한된 Board / Source Search
5. 광역 검색

앞 단계에서 충분한 근거가 확보되면 더 넓게 검색하지 않는다.

이름 검색은 약칭/띄어쓰기/분류명 차이를 허용하되, 후보가 여러 개면 임의 선택하지 않고 QA Owner에게 Scope를 확인한다.

## 4. Stable Source Map

- Sprint Board: `18426754095`
- Design Board: `18413127958`
- BTS Board: `18427010413`
- Git Test Assets: `Work/Camelio/TestAssets/`
- Current QA Spreadsheet: `QA_ACTIVE.md` Pointer 사용

개별 기획은 현재 Feature/Sprint 일감 → 연결 기획서 Item → 최신 Design Asset 순으로 특정한다.

## 5. Source Read State

- `LOCATED` — 위치만 확인
- `FILE_READ` — 파일 접근 성공
- `CONTENT_READ` — 필요한 내용 실제 Read
- `VERIFIED` — 이번 판단 범위 검토 완료
- `STALE` — 최신 여부 확인 실패
- `REVIEW_BLOCKED` — 신뢰성 있게 검토 불가

파일을 찾거나 다운로드한 것만으로 내용을 읽었다고 표현하지 않는다.

## 6. Document Reading Gate

포맷에 맞게 필요한 범위만 읽는다.

- XLSX: Sheet → Range → Section/Table
- HTML: Heading → Section → Table/Data
- CSV/JSON: Schema → Object/Row group
- PPTX: Slide → Text/Table/Image 관계

직접 Read가 신뢰성 있게 되지 않으면 불필요한 변환/OCR을 반복하지 않고 `REVIEW_BLOCKED`로 처리한다.

## 7. Expected Evidence Gate

Expected 근거로 사용할 수 있는 것:
1. 최신 기능 전용 기획서/명세
2. 명시적으로 확정된 기획 답변
3. QA Owner 결정

단독 근거가 될 수 없는 것:
- 현재 구현 상태
- BTS Actual
- 기존 TC
- Git Test Asset
- 일반적 UX
- 모델 기억/추론

결정되지 않으면 `QUESTION` 또는 `HOLD`.

## 8. Stop Rule

조회하지 못한 Source를 LIVE라고 주장하지 않는다.
존재하지 않는 Issue / Rule / Expected를 만들지 않는다.
필요한 근거를 확보할 수 없으면 추측을 중단하고 `HOLD / QUESTION / REVIEW_BLOCKED`로 구분한다.

## 9. Git Scope

Git에는 다음만 경량 유지한다.
- Boot / Source 접근 규칙
- 현재 QA 운영 기준
- Current Sprint / Live Pointer
- Feature별 경량 Test Asset
- 반복적으로 유효성이 확인된 QA Context

Git에 넣지 않는다.
- Monday/BTS 전체 Dump
- TC 전체 복제
- 기획서 복제
- 대화 로그
- 미확정 사양을 사실처럼 정리한 내용

Git은 두 번째 Monday / Drive / BTS가 아니다.

## 10. On-demand Support

`Work/Camelio/Support/`는 QA 외 반복 작업을 줄이기 위한 보조 자산이다.

- 일반 QA Boot에서는 읽지 않는다.
- QA Owner가 관련 작업을 요청한 경우에만 필요한 Support 문서를 추가로 읽는다.
- Support는 QA Core 규칙이나 Live Source를 대체하지 않는다.
