# CAMELIO QA BOOT

> 목적: 새 ChatGPT 세션이 과거 대화를 기억하지 못해도 **정확한 Source를 다시 읽고** 까멜리오 QA 업무를 재개하게 한다.
> 이 문서는 게임 사양 저장소가 아니다.

## 0. CRITICAL SAFETY

### Monday.com = READ-ONLY

까멜리오 프로젝트에서 ChatGPT는 Monday.com에 **절대 Write하지 않는다.**

허용: Board / Group / Item / Subitem / 상태 / 일정 / 담당 / Comment / Asset 조회와 첨부파일 Read.

금지: Item 생성·수정·삭제, 상태/담당/일정/우선순위 변경, Comment/Update 작성, 파일 업로드·교체 등 모든 Monday 데이터 변경.

## 1. Boot Order

1. `QA_BOOT.md`를 읽는다.
2. `QA_CURRENT.md`를 읽는다.
3. `QA_ACTIVE.md`를 읽는다.
4. `QA_ACTIVE.md`의 Current Sprint와 Source Pointer를 확인한다.
5. Monday **스프린트 보드**를 LIVE 조회해 현재 Sprint Scope / 일정 / 상·하위 일감을 확인한다.
6. Monday **QA / 버그 보드**를 LIVE 조회해 현재 BTS 상태와 요청 기능의 관련 이력을 확인한다.
7. Google Drive의 **현재 QA Test Spreadsheet**를 LIVE 조회해 기존 TC / BVT / Sprint Test 상태를 확인한다.
8. 기능 리뷰가 필요하면 현재 Sprint 일감의 연결을 따라 Monday **기획서 보드**의 최신 기획 Asset을 특정한다.
9. 실제 Source를 읽은 뒤에만 Review / Risk / QUESTION / Coverage / TC 작업을 시작한다.

과거 대화와 모델 기억은 참고만 한다. LIVE Source 또는 Git의 명시 규칙과 충돌하면 사용하지 않는다.

## 2. Stable Sources

### Sprint / Work
- Monday Board: `스프린트`
- Board ID: `18426754095`

### Design
- Monday Board: `기획서`
- Board ID: `18413127958`
- 개별 기능은 현재 Sprint 일감 → 연결된 기획 일감 → 최신 File Asset 순으로 특정한다.

### BTS
- Monday Board: `QA / 버그`
- Board ID: `18427010413`
- 웹 BTS는 사람용 View다. QA 판단용 데이터는 Monday 원본을 우선한다.

### Test Assets
- Google Drive / Google Sheets
- 정확한 현재 Spreadsheet는 `QA_ACTIVE.md`의 ID를 사용한다.
- 파일명이 비슷하다는 이유로 다른 Spreadsheet를 임의 선택하지 않는다.

## 3. Source Resolution Contract

1. `QA_ACTIVE.md`에 명시된 ID/Pointer가 있으면 그것을 우선 사용한다.
2. 정확한 Pointer가 있는 Source를 이름 검색 결과로 임의 대체하지 않는다.
3. Current Sprint는 `QA_ACTIVE.md`와 Monday LIVE 데이터를 교차 확인한다.
4. 기능 기획서는 **현재 Sprint 일감 → 연결된 기획 일감 → 최신 Subitem/File Asset** 순서로 찾는다.
5. 전역 검색에서 찾은 비슷한 문서를 자동으로 Primary Source로 사용하지 않는다.
6. 최신 Source가 둘 이상이거나 연결이 불명확하면 임의 선택하지 않고 HOLD한다.
7. Source를 **찾은 것**과 내용을 **읽은 것**을 구분한다.

## 4. Source Read State

- `LOCATED` — Source/파일 위치만 확인
- `FILE_READ` — 실제 파일 접근 성공
- `CONTENT_READ` — 필요한 텍스트/셀/표/시각 내용을 실제로 읽음
- `VERIFIED` — 이번 QA 판단에 필요한 범위를 검토 완료
- `STALE` — 최신 여부를 확인하지 못함
- `REVIEW_BLOCKED` — 현재 경로로 신뢰성 있게 내용을 읽을 수 없음

`LOCATED` 또는 `FILE_READ`를 `CONTENT_READ`로 표현하지 않는다.

## 5. Document Reading Gate

기획 Asset 사용 전에 포맷/구조를 확인한다.

- **XLSX**: Sheet → 사용 Range → Section/Table/Mechanic 단위
- **HTML**: Heading hierarchy → Section → Table/Data 관계
- **CSV/JSON**: Schema → Row/Object group → 관련 데이터 범위
- **Native PPTX**: Slide → Text/Table/Image 관계
- **Flattened / image-only PPTX**: Visual content를 실제로 안정적으로 읽었을 때만 `CONTENT_READ`

파일 다운로드 성공만으로 기획 내용을 이해했다고 주장하지 않는다.

직접 Read가 실패하면 복잡한 다단계 변환/OCR을 계속 시도하지 않는다. 대체 경로는 1회까지만 시도하고, 신뢰성 있게 읽을 수 없으면 `REVIEW_BLOCKED`로 표시한다.

## 6. Evidence / Expected Gate

Expected 근거로 사용할 수 있는 것:
1. 최신 기능 전용 기획서/명세
2. 명시적으로 확정된 기획 답변
3. QA Owner가 확정한 판단

단독으로 Expected 근거가 될 수 없는 것:
- 현재 구현 상태
- BTS Actual
- 기존 TC
- 일반적인 게임 UX
- ChatGPT 기억
- AI 추론

근거가 없으면 `QUESTION` 또는 `HOLD`.

BTS History는 Risk/Regression 근거로는 사용할 수 있지만 새로운 게임 사양을 만드는 근거로 사용하지 않는다.

## 7. Live Check / Stop Rule

중요한 QA 작업 전 최소 확인:
- Monday Sprint: 현재 Sprint/일감 조회 성공
- BTS: 요청 Issue 또는 관련 Issue 조회 성공
- Test Asset: 현재 Spreadsheet 조회 성공
- Design Review 시: 정확한 기획 Asset 특정 + 필요한 내용 Read 성공

조회하지 못한 Source를 LIVE라고 주장하지 않는다.
존재하지 않는 Issue/TC/기획 Rule은 만들어내지 않는다.
정확한 Source나 Expected를 결정할 수 없으면 **STOP → HOLD/QUESTION**.

## 8. Boot Handshake

```text
CAMELIO QA BOOT
Sprint: <current> / LIVE CHECK
Monday Scope: PASS|FAIL
BTS: PASS|FAIL
QA Test Asset: PASS|FAIL
Design Source: 필요 시 조회
Blocked: <none or reason>
```

Handshake 자체가 목적이 아니다. PASS 후 바로 실제 QA 업무로 이동한다.

## 9. Git Scope

Git에는 부팅 규칙, 현재 QA 운영 기준, 현재 Sprint/Live Source Pointer만 우선 유지한다.

Git에 넣지 않는다:
- Monday 전체 Dump
- BTS 전체 Dump
- TC 전체
- 기획서 복제본
- ChatGPT 대화 로그
- 미확정 사양을 사실처럼 정리한 문서

Git이 두 번째 Monday/Drive가 되지 않게 한다.
