# 까멜리오 QA CURRENT

> 게임 사양 저장소가 아니라, 까멜리오 QA에서 현재 사용하는 QA 리뷰/TC 작성 방식의 운영 기준이다.
> 규칙 준수 자체보다 정확한 Coverage와 실제 검증 가치를 우선한다.

## 1. 기본 역할
- 사용자가 QA Owner이며 최종 판단한다.
- 최신 전용 기획서와 명시된 확정 내용을 우선한다.
- 원문에 없는 사양을 임의로 완성하지 않는다.
- Expected가 결정되지 않으면 QUESTION 또는 HOLD로 분리한다.
- 다른 모델은 기본 경유 단계가 아니다. 필요할 때 교차 검증/대량 비교 용도로만 사용한다.

## 2. Source 사용 기준
- 기능 리뷰 시 현재 기능의 **주 기획서**와 실제 참고 문서를 먼저 특정한다.
- 관련 없는 문서를 억지로 끌어오지 않는다.
- 구버전/신버전이 함께 존재하면 임의로 섞지 않고 최신 전용 문서와 명시된 확정 내용을 우선한다.
- 문서 간 직접 충돌이 현재 Expected에 영향을 줄 때만 QUESTION/HOLD로 올린다.
- BTS 이력은 Risk/Regression 근거로 사용하되 사양 원문을 대신하지 않는다.

## 3. 기본 리뷰 Flow
기획 원문 → 핵심 Flow/Rule/State → Risk → 필요한 문의 → TC Coverage → 확정 영역 TC Draft → QA Owner 검수 → 기획 답변 발생 시 영향 범위만 Delta Patch

가능한 한 Feature Review부터 TC Draft까지 한 번의 Pass에서 처리한다.

## 4. QA 문의 기준
문의한다:
- 답이 없으면 핵심 Action의 Expected Result를 결정할 수 없다.
- 보상/재화/상태 전이 등 고위험 결과가 서로 다른 해석으로 갈린다.
- 최신 문서끼리 직접 충돌하며 현재 Scope 검증에 영향을 준다.

기본적으로 문의하지 않는다:
- 문서에 '논의', '예시', '제안'이 있다는 이유만으로 질문을 만든다.
- 현재 Scope 밖의 미정 항목을 모두 확인한다.
- 구현 내부 세부(API 필드, DB 구조 등)를 기획 질문으로 올린다.
- 빌드를 보면 검증 가능한 단순 UX 차이를 사전에 모두 확정하려 한다.

## 5. QA Quality Lens
기능 리뷰 시 아래 축을 한 번씩 검토하고 `필요 / N/A / 이번 Sprint 제외`를 판단한다. 모든 축을 별도 TC로 만들 필요는 없다.

- **Flow / State** — 진입, 핵심 Action, 상태 전이, 종료/복귀
- **Rule / Data** — 조건, 값, 재화/보상, 전투 수치, 확률/횟수
- **Boundary / Failure** — 경계값, 부족/초과, 실패/불가능 상태
- **Persistence** — 화면 재진입, 스테이지 전환, 재접속, 초기화
- **Interaction** — 관련 시스템 결합, 중첩, 복합 상태
- **Presentation / Cleanup** — HUD/UI 피드백, 연출, 이펙트 잔존, 종료 후 정리
- **Regression** — 관련 과거 BTS, 동일 defect class, 변경 영향 영역

High Risk가 있는데 어떤 축에서도 다루지 않았다면 Coverage 누락 후보로 본다.

## 6. 검증 깊이
- **L0 Functional** — 기능 존재/Trigger/기본 동작
- **L1 Rule/Data** — 파라미터, 값, 적용 대상, 상태가 기획과 일치
- **L2 Numerical/Boundary** — 실제 계산, 경계, 수치 결과 검증
- **L3 Interaction** — 시스템 간 결합, 중첩, 복합 조건 검증

Risk 기준 기본 방향:
- High Risk: 최소 L2 검토, 핵심 상호작용은 L3 후보
- Medium Risk: L1~L2
- Low Risk: L0~L1

기능 특성에 따라 QA Owner가 조정한다. 모든 콘텐츠 ID를 개별 Full TC로 확장하는 것이 목적이 아니다.

## 7. TC 작성 기준
- 한 행은 실제 사용자/시스템의 의미 있는 Action 중심으로 작성한다.
- 기획서 항목 수와 TC 수를 1:1 대응시키지 않는다.
- 같은 Action의 불필요한 반복을 피한다.
- 사전 조건은 결과 분기나 경계 상태를 만들기 위해 필요한 경우만 사용한다.
- Expected Result는 짧고 관찰 가능하게 쓴다.
- `정상적으로`, `문제없이`, `~되는지 확인` 같은 표현을 피한다.
- 간결함을 이유로 핵심 Flow/State Transition/보상 정합성 Coverage를 삭제하지 않는다.
- Static UI라고 무조건 제외하지 않는다. 핵심 화면 구성은 대표 진입 Action에서 묶어서 확인할 수 있다.
- 구현 규칙 자체를 정식 TC로 만들기보다 실제 사용자 결과와 리스크를 검증한다.
- 동일 Action이라도 서로 다른 고위험 State Transition을 검증해야 하면 분리할 수 있다.

## 8. TC Review / Execution Format

QA Owner와 내용 검수 단계에서는 기본적으로:

```text
대분류 | 중분류 | 사전 조건 | 테스트 내용 | 기대 결과
```

5개 핵심 필드만 사용한다.

실제 Google Sheet 실행 자산에는 기존 Spreadsheet 구조에 따라:

```text
No. | 대분류 | 중분류 | 사전 조건 | 테스트 내용 | 기대 결과 | 결과 | 이슈번호 | 비고
```

형태로 구성할 수 있다.

- 같은 대분류가 연속되면 검수용 Draft에서는 첫 행 이후 대분류를 비워도 된다.
- 셀 내부 강제 줄바꿈은 기본적으로 사용하지 않는다.
- Sheet 작성 시 기존에 검증된 TC Sheet의 Summary / Formula / Validation / Conditional Format / Merge / Layout을 우선 재사용한다.

## 9. Golden Examples
GOOD
- 테스트 내용: `몬스터를 비치명 피해로 타격`
- 기대 결과: `실제로 감소한 HP를 기준으로 골드 동전 드랍 연출이 노출된다.`

GOOD
- 테스트 내용: `상단 HUD 재화를 획득`
- 기대 결과: `획득 수량이 보유량에 반영된다.`

GOOD
- 테스트 내용: `상위 보상 노드를 터치`
- 기대 결과: `선택한 노드까지의 미수령 보상이 지급된다.`

## 10. Anti-patterns
BAD
- `로비에 진입 → 프로필 정보가 출력된다.`
- `로비에 진입 → 재화 HUD가 출력된다.`
- `로비에 진입 → 좌·우 HUD가 출력된다.`

이유: 하나의 화면 진입 Action을 UI 요소별로 반복해 기획서 항목을 TC로 복제한다.

BAD
- `레이아웃 Z-Order가 기획 순서대로 출력된다.`

이유: 구현 구조 자체보다 가림, 터치 차단, 잘못된 표시 등 사용자 결과를 검증한다.

BAD
- `신규 별이 획득되고 게이지가 증가하며 보상 노드가 해금되고 신규 연출이 출력된다.`

이유: 하나의 Expected에 여러 검증 포인트를 과도하게 적재한다.

## 11. 변경 반영
- 기획 답변으로 일부 사양이 확정되면 영향받는 TC만 수정/추가/삭제한다.
- 이미 확정된 TC 전체를 다시 생성하지 않는다.
- 새 규칙은 한 기능에서 한 번 나온 피드백만으로 추가하지 않는다.
- 서로 다른 기능에서 반복되거나 QA Owner가 명시적으로 일반화한 규칙만 CURRENT에 승격한다.
- 이 문서는 짧게 유지한다. 기능별 사양, Q&A 역사, 개발 상태는 저장하지 않는다.

## 12. Practical Test Asset

QA Test Asset의 목적은 완벽하거나 형식적으로 우아한 TC를 만드는 것이 아니다.

우선순위:
1. 실제 결함 탐지에 도움이 되는 Coverage
2. 테스트 시 바로 사용할 수 있는 실행성
3. Regression 시 재사용 가능성
4. 짧고 관찰 가능한 Expected

TC는 QA의 행동 범위를 제한하는 절차서가 아니라
필수 Coverage를 놓치지 않기 위한 Backbone으로 사용한다.

QA Owner가 테스트 중 발견한 의심, Edge Case, 복합 조건은
기존 TC에 없더라도 자유롭게 탐색한다.

반복 가치가 확인되면 이후 TC / CL / Git Coverage에 승격할 수 있다.

## 13. Test Asset Type

- `BVT` — 빌드 기본 생존 및 핵심 진입 검증
- `TC_[기능]` — 수치, 상태, 경계, 상호작용 등 반복 실행 가치가 높은 검증
- `CL_[기능]` — 빠르게 훑을 수 있는 UI / Flow / 단순 기능 Checklist

모든 기능을 TC로 만들지 않는다.
Risk와 반복 실행 가치에 따라 TC / CL을 선택한다.

## 14. Sprint Test Spreadsheet

Google Drive의 QA Test Spreadsheet는 실제 QA 실행 Workspace다.

기본 구성 예:
- 요약
- 빌드 스펙
- BVT
- TC_[기능]
- CL_[기능]

Test Spreadsheet는 Sprint 또는 Milestone 단위로 분리할 수 있다.

이전 Spreadsheet는 현재 Sprint의 정본으로 계속 유지하지 않는다.
새 Sprint에서 필요한 기존 TC / CL만 참고하여 취사 선택하고,
현재 변경점과 Risk에 맞게 보강한다.

현재 사용 Spreadsheet의 정확한 Pointer는 `QA_ACTIVE.md`에서 관리한다.

## 15. AI-assisted QA Orchestration

QA Owner가 Scope / Expected / Issue 여부 / Release 관련 최종 판단을 담당한다.

AI는 반복 노동과 구조화 비용을 줄여
QA Owner가 실제 테스트, 의심, Risk 판단에 더 많은 시간을 사용할 수 있도록 보조한다.

현재 주요 활용 범위:
- Git QA Context 복원
- Monday / Design / BTS Live Source 조회
- 기획 구조 및 Risk 정리
- TC / CL Draft
- Google Sheet 실행 자산 생성 및 수정
- 실행 결과 정리
- QA Update / BTS Draft 보조

중요한 산출물과 의사결정은 QA Owner 검수를 거친다.

다른 AI 모델은 동일 Context를 별도로 유지하는 정본으로 사용하지 않는다.
복잡한 문서 해석, 코드 작업, 독립 Review 등 명확한 이점이 있을 때 보조적으로 사용할 수 있다.

향후 실제 업무에서 유효성이 확인되는 경우:
- BTS 등록 보조 / Bulk 처리
- 자동화 테스트
- 로그 기반 검증
- 기획 단계 QA Ideation / Risk Review

등을 점진적으로 추가할 수 있다.
