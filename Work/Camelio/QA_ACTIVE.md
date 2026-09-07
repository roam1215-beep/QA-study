# CAMELIO QA ACTIVE

> **현재 Sprint와 Live Source Pointer만** 유지한다.
> 일정/이슈/TC/기획 내용 전체를 복사하지 않는다. 상태 값은 새 세션에서 LIVE Source로 다시 확인한다.

## Current Sprint

- Sprint: **S2**
- Working scope selector: Monday `스프린트` 보드의 **9M. 2W** 그룹들
- Build theme: **D1 플레이 보완 / 이전 Sprint 잔여 정리**
- QA phase: **Pre-build preparation**
- Last boot setup update: **2026-09-07**

현재 일정/진행률/담당/상태는 이 파일보다 Monday LIVE 조회를 우선한다.

## Live Pointers

### Monday Sprint
- Board: `스프린트`
- Board ID: `18426754095`

### Monday BTS
- Board: `QA / 버그`
- Board ID: `18427010413`

### Monday Design
- Board: `기획서`
- Board ID: `18413127958`
- 개별 기획 Asset ID는 고정하지 않는다. 현재 Sprint 일감의 연결을 따라 매번 최신 Asset을 특정한다.

### QA Test Spreadsheet
- Title: `sprint 테스트 문서`
- Spreadsheet ID: `1g4WDbrlNxjbqnrVUmIjLj2JUPQmKCgvrrGBOlzVwXBs`
- Sprint가 바뀌어 테스트 문서가 교체되면 **이 Pointer를 먼저 갱신**한다.
- Pointer가 없거나 문서가 불명확하면 비슷한 파일을 임의 선택하지 말고 HOLD한다.

## Current QA Context

- S1 수정 확인/잔여 처리 이후 S2 사전 준비 단계.
- 실제 빌드 도착 전에 BVT + Sprint Test 준비가 필요.
- 기존 TC Coverage가 기능별로 불균일해 Sprint 작업과 함께 보강 필요.
- 룬/재능카드 등 전투 영향 시스템은 Functional 확인 대비 Numerical/Boundary/Interaction 검증 부채가 큼.
- 현재 파일럿 목표: Git Boot + Monday/Drive LIVE Source 기반으로 새 세션에서도 QA 맥락 복구 → 실제 S2 Feature Review/TC Draft까지 연결.

## Update Rule

이 파일은 다음 경우에만 짧게 수정한다.
- Sprint 변경
- Current QA Test Spreadsheet 변경
- Source Board/Pointer 변경
- 현재 QA Phase가 크게 변경

개별 일감 상태, BTS 숫자, TC 상세, 기획 Rule은 저장하지 않는다. 그것들은 LIVE Source에서 다시 읽는다.
