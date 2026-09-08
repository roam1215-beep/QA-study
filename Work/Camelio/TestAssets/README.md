# Camelio Test Assets

> Test Asset은 완료 보고서가 아니라 **다음 QA가 Coverage를 0부터 다시 만들지 않게 하는 작업 기억**이다.

## 역할
- 무엇을 실제로 확인했는지
- 무엇을 확인하지 못했고 왜 못 했는지
- 과거 어떤 문제를 Regression으로 봐야 하는지
- 다음 QA에서 무엇을 우선 볼지
- 상세 Test Case가 어디에 있는지

## 운영 원칙
- 기획서, Monday, BTS, Test Case 내용을 통째로 복제하지 않는다.
- 단순 화면 요소를 모두 정식 TC로 만들지 않는다. Asset checklist로 충분하면 그대로 둔다.
- 반복 실행 가치가 높은 Flow / State / Boundary / 수치 / Regression만 Google Drive Test Case로 승격한다.
- 미확인 사항은 숨기지 않고 `QUESTION`, `HOLD`, `DEFERRED`로 남긴다.
- 새 이슈/기획 변경이 생기면 영향받는 부분만 Delta Update한다.
- 문서 완성도보다 **다음 QA 비용 절감과 Coverage Hole 방지**를 우선한다.
