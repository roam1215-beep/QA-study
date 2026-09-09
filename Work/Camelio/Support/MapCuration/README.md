# Camelio Map Curation Support

> QA가 주 업무다.
> 이 폴더는 Cocos Creator에서 적/장애물 개별 배치 노가다를 줄이기 위한 **On-demand 기획/맵 제작 보조 자산**이다.
> 일반 QA Boot에서는 읽지 않는다.

## 1. 목적

대화로 맵 의도를 설계하고, Curation-compatible JSON을 만들어 Cocos Creator Editor에 바로 넣는다.

기본 Flow:

```text
자연어 요구
→ Grid/ASCII 목업
→ QA Owner 수정/확정
→ Curation-compatible JSON 생성
→ Cocos Creator [JSON] Import
→ Editor 배치 확인
→ Play 검증
```

JSON부터 바로 만들기보다 배치 의도가 중요한 경우 목업을 먼저 제시한다.

## 2. 데이터 경로 구분

### Curation JSON
웹 Curation의 `선택 내보내기` 및 Cocos Creator의 `JSON` 입력에서 사용하는 형식.
이 Support의 기본 출력 Target이다.

### Cocos Ex / Im CSV
Cocos Creator Editor 내부 Wave Export/Import 형식.
Editor 저장 상태/기본값을 분석하는 참고 자료이며 Curation JSON과 동일 Schema라고 가정하지 않는다.

## 3. 현재 검증된 moverTurn 입력

턴 이동 장애물 궤도는 Curation JSON에서 문자열이 아니라 **row/col Object Array**로 전달한다.

```json
{
  "type": "box",
  "row": 3,
  "col": 4,
  "length": 1,
  "length2": 1,
  "kind": "moverTurn",
  "styleId": "moverTurn",
  "moveMode": "linear",
  "moveLoop": true,
  "waypoints": [
    { "row": 3, "col": 4 },
    { "row": 4, "col": 6 },
    { "row": 6, "col": 7 }
  ]
}
```

검증 결과:
- Cocos `JSON` Import 성공
- 궤도 생성 성공
- Play 성공
- 1턴 진행 시 다음 waypoint 이동 성공
- 전체 Loop 1회전은 아직 미검증

Cocos CSV Export에서는 waypoint가 `3:4|4:6|6:7` 같은 문자열로 직렬화될 수 있으므로 두 표현을 혼동하지 않는다.

## 4. 실행 원칙

- 몬스터가 하나도 없는 맵은 Play 검증용으로 사용하지 않는다.
- JSON Import 성공과 실제 Play 성공을 별도로 확인한다.
- 확인하지 않은 특수 파라미터를 임의로 채우지 않는다.
- 새 Component/파라미터가 실제 Import + Play까지 성공하면 그때 재사용 자산으로 승격한다.
- 좌표를 QA Owner에게 일일이 요구하지 않는다. 자연어 요구를 목업/좌표로 변환한다.

## 5. 목업 예

```text
     C0 C1 C2 C3 C4 C5 C6 C7 C8

R3             M
R4         ·       ·
R5      M             M
R6
R7         ·       ·
R8             M
R10            S
```

- `M` = moverTurn
- `S` = Slime Start

예시 피드백:
- 오른쪽 장애물 한 칸 아래
- 상단 방패형 3마리 가로 배치
- 좌우 비대칭
- 중앙 공간 넓게
- 동일 궤도에 시작점만 분산

## 6. 재사용 Pattern

실제 Cocos Import + Play까지 성공한 조형만 `patterns/`에 보존한다.

현재:
- `patterns/ferriswheel_moverturn.json`
  - moverTurn 4개
  - 공통 8-point Loop
  - 시작점 분산
  - JSON Import / Play / 1턴 이동 검증 완료

Pattern은 완성 맵 정답이 아니라 재사용 가능한 조형/기믹 부품이다.
