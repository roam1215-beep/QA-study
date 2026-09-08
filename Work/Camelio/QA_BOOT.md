# CAMELIO QA BOOT

> 목적: 새 ChatGPT 세션이 과거 대화를 기억하지 못해도
> 정확한 Source를 다시 읽고 까멜리오 QA 업무를 연속적으로 재개하게 한다.
>
> 이 문서는 게임 사양 저장소가 아니다.

## 0. CRITICAL SAFETY

### Monday.com = READ-ONLY

까멜리오 프로젝트에서 ChatGPT는 Monday.com에 절대 Write하지 않는다.

허용:
Board / Group / Item / Subitem / 상태 / 일정 / 담당 /
Comment / Update / Asset 조회와 첨부파일 Read.

금지:
Item 생성·수정·삭제, 상태/담당/일정/우선순위 변경,
Comment/Update 작성, 파일 업로드·교체 등 모든 Monday 데이터 변경.

### Git = REVIEW BEFORE WRITE

Git Read는 QA Context 복원과 Source 조회를 위해 사용할 수 있다.

Git Create / Update / Delete / Commit은 반드시 다음 순서를 따른다.

1. ChatGPT가 변경 초안 또는 Patch를 제시한다.
2. QA Owner가 최소 1회 내용을 검수한다.
3. QA Owner가 명시적으로 반영을 승인한다.
4. 승인된 범위만 ChatGPT가 Git에 반영하고 Commit한다.

QA Owner 승인 없이 Git 내용을 임의로 생성·수정·삭제하지 않는다.
검수 이후 내용이 변경되면 변경된 범위도 다시 검수 대상으로 본다.


## 1. Core Boot

1. `QA_BOOT.md`를 읽는다.
2. `QA_CURRENT.md`를 읽는다.
3. `QA_ACTIVE.md`를 읽는다.
4. `QA_ACTIVE.md`의 Current Sprint / Focus / Live Source Pointer를 확인한다.

Core Boot 단계에서 모든 Feature / BTS / TC / 기획 내용을 미리 로드하지 않는다.

과거 대화와 모델 기억은 검색 방향을 잡기 위한 참고로만 사용할 수 있다.
현재 상태나 Expected를 확정하는 근거로 사용하지 않는다.


## 2. Live Retrieval Rule

까멜리오의 현재 상태, 과거 QA 이력, Coverage, 변경 영향,
관련 Issue, Test Case 또는 Expected에 대한 판단이 필요한 경우
모델 기억이나 과거 대화만으로 답하지 않는다.

관련 Source가 존재하면 현재 업무 범위에 필요한 Source를 먼저 LIVE 조회한다.

주요 Source:
- Monday Sprint / Feature / Parent / QA Item 및 관련 Update
- Monday 기획서 Item과 최신 Design Asset
- Monday QA / 버그 보드의 관련 BTS
- Google Drive의 현재 Test Case / BVT / Sprint Test
- Git Test Asset의 기존 Coverage / Open / Recheck Context

Git Test Asset과 모델 기억은 현재 사실을 대신하지 않는다.
둘의 역할은 필요한 Live Source와 과거 QA Context를 빠르게 찾는 것이다.

Live Source와 기억이 충돌하면 Live Source를 우선한다.

필요한 Source를 신뢰성 있게 조회하지 못한 경우
현재 상태나 Expected를 추측해서 완성하지 않고
HOLD / QUESTION / REVIEW_BLOCKED 등으로 구분한다.


## 3. Task Retrieval

사용자가 특정 기능이나 업무를 요청하면 해당 업무를 Retrieval Target으로 잡는다.

예:
- "메인 로비 어떻게 됐더라?"
- "오늘 룬, 재능카드, 프로필 QA 할 거야."
- "이 변경 영향 어디까지야?"
- "이 기능 TC 보강하자."

이 경우 필요한 범위에서 다음 경로를 따라 현재 Context를 복원한다.

1. Git Test Asset Index에서 관련 Feature Asset 존재 여부 확인
2. 관련 Monday Sprint / Feature / Parent / QA Item 확인
3. QA Item의 Update / 답변 / 관련 이력 확인
4. 연결된 Monday 기획서 Item 확인
5. 해당 기획 Item의 최신 Design Asset 확인
6. 알려진 Issue ID 및 현재 Feature 관련 BTS 조회
7. Google Drive의 관련 Test Case / BVT / Sprint Test 확인
8. Git Test Asset의 Coverage / Open / Recheck와 결합
9. 그 결과를 기준으로 Review / Risk / QUESTION / Coverage / TC 작업 수행

모든 Source를 항상 전부 읽는 것이 목적은 아니다.

현재 판단에 필요한 관련 Source 종류를 빠뜨리지 않으면서,
업무와 무관한 Context를 불필요하게 로드하지 않는 것을 원칙으로 한다.


## 4. Stable Sources

### Sprint / Work
- Monday Board: `스프린트`
- Board ID: `18426754095`

### Design
- Monday Board: `기획서`
- Board ID: `18413127958`

개별 기능 기획은 가능한 경우:
현재 Feature / Sprint 일감
→ 연결된 기획서 Item
→ 최신 Design Asset

순으로 특정한다.

기획서 Item에 새 Asset이 갱신될 수 있으므로
과거 파일의 내용만으로 최신 Expected를 확정하지 않는다.

### BTS
- Monday Board: `QA / 버그`
- Board ID: `18427010413`

Feature Asset에 저장된 Issue 번호는 중요한 과거 Regression Pointer다.
현재 상태와 상세 내용은 BTS에서 LIVE 조회한다.

필요한 경우 저장된 Issue 번호뿐 아니라
현재 Feature와 관련된 신규 BTS도 함께 검색한다.

### Git Test Assets
- Repository: `roam1215-beep/QA-study`
- Path: `Work/Camelio/TestAssets/`

역할:
Feature별 QA Baseline / Coverage / Open / Recheck Context를
세션을 넘어 경량으로 유지한다.

Test Asset은 기획서, BTS, Monday Update 또는 Test Case를 복제하지 않는다.
Test Asset 자체를 새로운 Expected의 근거로 사용하지 않는다.

### Test Case / Execution Docs
- Google Drive / Google Sheets
- 현재 Spreadsheet Pointer는 `QA_ACTIVE.md`를 사용한다.

상세 Test Case / BVT / Sprint Test의 정본은 Google Drive에 둔다.
파일명이 비슷하다는 이유로 다른 Spreadsheet를 임의 선택하지 않는다.


## 5. Source Resolution Contract

1. `QA_ACTIVE.md`에 명시된 ID / Pointer가 있으면 우선 사용한다.
2. 정확한 Pointer가 있는 Source를 이름 검색 결과로 임의 대체하지 않는다.
3. Current Sprint / 업무 상태는 Monday LIVE 데이터를 확인한다.
4. 기능 기획은 연결된 Monday 기획서 Item과 최신 Asset을 우선한다.
5. 전역 검색에서 찾은 비슷한 문서를 자동으로 Primary Source로 사용하지 않는다.
6. 최신 Source가 둘 이상이거나 연결이 불명확하면 임의 선택하지 않는다.
7. Source를 찾은 것과 실제 내용을 읽은 것을 구분한다.


## 6. Source Read State

- `LOCATED` — Source / 파일 위치만 확인
- `FILE_READ` — 실제 파일 접근 성공
- `CONTENT_READ` — 필요한 내용 실제 Read
- `VERIFIED` — 이번 판단에 필요한 범위 검토 완료
- `STALE` — 최신 여부 확인 실패
- `REVIEW_BLOCKED` — 현재 경로로 신뢰성 있게 검토 불가

`LOCATED` 또는 `FILE_READ`를 `CONTENT_READ`로 표현하지 않는다.


## 7. Document Reading Gate

기획 Asset 사용 전에 포맷 / 구조를 확인한다.

- XLSX: Sheet → 사용 Range → Section / Table / Mechanic
- HTML: Heading hierarchy → Section → Table / Data 관계
- CSV / JSON: Schema → Row / Object group → 관련 데이터
- Native PPTX: Slide → Text / Table / Image 관계
- Flattened / image-only PPTX: 시각 내용을 안정적으로 읽었을 때만 `CONTENT_READ`

파일 다운로드 성공만으로 기획 내용을 이해했다고 주장하지 않는다.

직접 Read가 실패하면 불필요한 변환이나 OCR을 반복하지 않는다.
신뢰성 있게 읽을 수 없으면 `REVIEW_BLOCKED`로 표시한다.


## 8. Evidence / Expected Gate

Expected 근거로 사용할 수 있는 것:

1. 최신 기능 전용 기획서 / 명세
2. 명시적으로 확정된 기획 답변
3. QA Owner가 확정한 판단

단독으로 Expected 근거가 될 수 없는 것:

- 현재 구현 상태
- BTS Actual
- 기존 Test Case
- Git Test Asset
- 일반적인 게임 UX
- ChatGPT 기억
- AI 추론

근거가 없으면 QUESTION 또는 HOLD.

BTS History와 Test Asset은 Risk / Regression / 과거 Coverage 근거로는 사용할 수 있지만
새로운 게임 사양을 만드는 근거로 사용하지 않는다.


## 9. Stop Rule

조회하지 못한 Source를 LIVE라고 주장하지 않는다.

존재하지 않는 Issue / TC / 기획 Rule을 만들어내지 않는다.

현재 판단에 필요한 Source나 Expected를 결정할 수 없으면
추측으로 계속 진행하지 않고 STOP → HOLD / QUESTION / REVIEW_BLOCKED 처리한다.


## 10. Git Scope

Git에는 다음을 유지한다.

- Boot / Source 접근 규칙
- 현재 QA 운영 기준
- 현재 Sprint / Live Pointer
- Feature별 경량 Test Asset
- 반복적으로 유효성이 확인된 QA Context

Git에 넣지 않는다.

- Monday 전체 Dump
- BTS 전체 Dump
- Test Case 전체
- 기획서 복제본
- ChatGPT 대화 로그
- 미확정 사양을 사실처럼 정리한 문서

Git이 두 번째 Monday / Drive / BTS가 되지 않게 한다.
