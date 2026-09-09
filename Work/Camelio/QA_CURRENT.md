# 까멜리오 QA CURRENT

> 까멜리오 QA에서 현재 사용하는 리뷰/TC 작성 방식의 운영 기준.
> 규칙 자체보다 실제 결함 탐지, 실행성, 재사용 가치를 우선한다.

## 1. 기본 역할

- QA Owner가 최종 판단한다.
- 최신 전용 기획서와 명시적 확정 내용을 우선한다.
- 원문에 없는 사양을 임의로 완성하지 않는다.
- Expected가 결정되지 않으면 `QUESTION / HOLD`.
- AI는 반복 노동과 구조화 비용을 줄이고, QA Owner는 실제 플레이/탐색/판단에 집중한다.

## 2. 기본 리뷰 Flow

기획 원문
→ 핵심 Flow / Rule / State
→ Risk
→ 필요한 문의
→ Coverage 설계
→ TC / CL / BVT Draft
→ QA Owner 검수
→ 기획 답변 또는 변경 발생 시 영향 범위만 Delta 수정

가능한 한 Feature Review부터 Test Asset Draft까지 한 흐름으로 처리한다.

## 3. QA 문의 기준

문의한다:
- 답이 없으면 핵심 Action의 Expected를 결정할 수 없음
- 보상/재화/상태 전이 등 고위험 결과가 서로 다른 해석으로 갈림
- 최신 문서끼리 직접 충돌하며 현재 Scope에 영향

기본적으로 문의하지 않는다:
- 문서에 '논의/예시/제안'이 있다는 이유만으로 질문 생성
- Scope 밖 미정 항목을 전부 확인
- 구현 내부 세부(API/DB)를 기획 질문으로 올림
- 빌드에서 쉽게 확인 가능한 단순 UX 차이를 사전 확정하려 함

## 4. QA Quality Lens

기능마다 아래 축을 `필요 / N/A / 이번 Sprint 제외`로 판단한다. 모든 축을 TC로 만들 필요는 없다.

- **Flow / State** — 진입, 핵심 Action, 상태 전이, 종료/복귀
- **Rule / Data** — 조건, 값, 재화/보상, 전투 수치, 확률/횟수
- **Boundary / Failure** — 경계값, 부족/초과, 실패/불가능
- **Persistence** — 재진입, 스테이지 전환, 재접속, 초기화
- **Interaction** — 관련 시스템 결합/중첩/복합 상태
- **Presentation / Cleanup** — HUD/UI 피드백, 연출, 잔존/정리
- **Regression** — 관련 BTS, 동일 defect class, 변경 영향

High Risk인데 어떤 축에서도 다루지 않았다면 Coverage 누락 후보로 본다.

## 5. 검증 깊이

- **L0 Functional** — 기능 존재/Trigger/기본 동작
- **L1 Rule/Data** — 값/대상/상태가 기획과 일치
- **L2 Numerical/Boundary** — 실제 계산/경계/수치 결과
- **L3 Interaction** — 시스템 간 결합/중첩/복합 조건

기본 방향:
- High: 최소 L2, 핵심 상호작용은 L3 후보
- Medium: L1~L2
- Low: L0~L1

모든 콘텐츠 ID를 개별 Full TC로 확장하는 것이 목적은 아니다.

## 6. TC 작성 기준

TC는 검증 자산이며, **사양을 모르는 사람도 TC만 보고 실행하고 Pass/Fail을 판단할 수 있는 수준**을 지향한다.

- 한 행은 의미 있는 검증 단위로 작성한다.
- 기획 항목 수와 TC 수를 1:1 대응시키지 않는다.
- 같은 Action의 불필요한 반복을 피한다.
- 사전 조건에는 실행에 필요한 상태와 비교 기준점만 둔다.
- 테스트 내용은 실제 조작 또는 확인 대상을 명확하게 쓴다.
  - 상황에 따라 `터치 / 홀드 / 드래그 / 진입` 같은 조작어도,
    `데미지 확인 / 상세 정보 확인 / 출력 확인` 같은 관찰 표현도 사용할 수 있다.
- `기능 확인 / 정상 동작 확인`처럼 무엇을 수행하거나 관찰하는지 알 수 없는 표현은 피한다.
- 기대 결과는 사양 전체를 복사하지 않고 해당 TC의 Pass/Fail을 판정할 수 있는 관찰 가능한 결과를 적는다.
- 대분류/중분류/사전 조건에 이미 있는 정보를 테스트 내용에 불필요하게 반복하지 않는다.
- 비교가 필요한 TC는 기준점이 어디인지 명시한다.
- 실제 관찰/검증 방법이 없는 값을 실행 가능한 것처럼 쓰지 않는다. 검증 수단이 결정되지 않으면 `HOLD`.
- 우선순위/State Transition도 가능하면 실제 Flow와 결과로 검증한다.
- 길어질 때 무조건 축약하지 않는다. 다른 열로 옮길 정보인지, 불필요한 반복인지, 별도 검증으로 나눌지 판단한다.
- 위 기준은 문장 템플릿이 아니다. 실행성과 판정성을 해치지 않는 범위에서 기능 특성에 맞게 작성한다.

### TC 셀프리뷰
1. 사양을 모르는 사람이 이 행만 보고 무엇을 해야 하는지 알 수 있는가?
2. 외부 문서를 다시 찾지 않고 무엇이 Pass인지 판단할 수 있는가?
3. 비교가 필요하다면 기준점이 명확한가?
4. 관찰할 수 없는 값을 '확인'한다고 써서 실행 가능한 척하고 있지 않은가?

## 7. TC Review / Execution Format

검수 Draft:
```text
대분류 | 중분류 | 사전 조건 | 테스트 내용 | 기대 결과
```

Google Sheet 실행 자산:
```text
No. | 대분류 | 중분류 | 사전 조건 | 테스트 내용 | 기대 결과 | 결과 | 이슈번호 | 비고
```

- 같은 대분류가 연속되면 첫 행 이후 비워도 된다.
- 셀 내부 강제 줄바꿈은 기본 사용하지 않는다.
- 기존 검증된 Sheet의 Summary / Formula / Validation / Conditional Format / Merge / Layout을 재사용한다.
- 짧은 No. 체계를 사용한다.

## 8. Practical Test Asset

우선순위:
1. 실제 결함 탐지에 도움이 되는 Coverage
2. 테스트 시 바로 사용할 수 있는 실행성
3. Regression 재사용 가치
4. 짧고 판정 가능한 Expected

TC는 테스트 행동을 제한하는 절차서가 아니라 필수 Coverage를 놓치지 않기 위한 Backbone이다.

QA Owner는 TC 밖에서도 의심, Edge Case, 복합 조건을 자유롭게 탐색한다.
반복 가치가 확인되면 이후 TC / CL / Git Coverage에 승격한다.

## 9. Test Asset Type

- `BVT` — 빌드 기본 생존 및 핵심 진입
- `TC_[기능]` — 수치/상태/경계/상호작용 등 반복 실행 가치가 높은 검증
- `CL_[기능]` — 빠르게 훑는 UI/Flow/단순 기능 Checklist

모든 기능을 TC로 만들지 않는다.

## 10. Sprint Test Spreadsheet

Google Drive QA Test Spreadsheet는 실제 QA 실행 Workspace다.

기본 구성 예:
- 요약
- 빌드 스펙
- BVT
- TC_[기능]
- CL_[기능]

Sprint/Milestone이 바뀌면 필요한 기존 TC/CL만 취사 선택하고 현재 변경점/Risk에 맞게 보강한다.
정확한 Spreadsheet Pointer는 `QA_ACTIVE.md`에서 관리한다.

## 11. 변경 반영 원칙

- 기획 답변/변경이 발생하면 영향받는 TC만 수정/추가/삭제한다.
- 이미 확정된 TC 전체를 다시 생성하지 않는다.
- 한 기능에서 한 번 나온 피드백만으로 Core 규칙을 늘리지 않는다.
- 서로 다른 기능에서 반복되거나 QA Owner가 명시적으로 일반화한 기준만 CURRENT에 승격한다.
- 이 문서는 짧게 유지하고 기능별 사양/개발 상태/대화 이력은 넣지 않는다.

## 12. AI-assisted QA Orchestration

주요 활용:
- Git QA Context 복원
- Monday / Design / BTS Live Source 조회
- 기획 구조/Risk 정리
- TC / CL Draft
- Google Sheet 실행 자산 생성/수정
- 실행 결과 정리
- QA Update / BTS Draft 보조

중요 산출물과 의사결정은 QA Owner 검수를 거친다.
다른 AI는 동일 Context의 정본으로 사용하지 않고, 명확한 이점이 있을 때만 보조적으로 사용한다.
