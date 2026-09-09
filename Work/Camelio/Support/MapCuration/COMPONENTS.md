# Map Curation Components

> 현재 세션에서 확인한 Component 사전.
> 모든 항목이 Curation JSON Import + Play까지 검증된 것은 아니다.
> 새 세션에서는 필요한 타입만 읽고 사용한다.

## Status

- `VERIFIED_PLAY` — Curation JSON Import + Play 동작 확인
- `VERIFIED_JSON` — Curation JSON/기존 추출물에서 구조 확인
- `OBSERVED` — Editor/진단 Export에서 존재/식별 확인, Curation 직접 생성은 추가 검증 필요
- `EXCLUDED` — 현재 범위 제외

## Enemy

| 표시/용도 | ID | 상태 |
|---|---|---|
| normal | `enemy_normal` | VERIFIED_JSON |
| heavy | `enemy_heavy` | VERIFIED_JSON |
| defense | `enemy_defense` | VERIFIED_JSON |
| shield | `enemy_shield` | VERIFIED_JSON |
| splitter | `enemy_splitter` | OBSERVED |
| armored | `enemy_armored` | OBSERVED |
| diamond | `enemy_diamond` | OBSERVED |
| spider | `enemy_spider` | OBSERVED |
| egg | `enemy_egg` | OBSERVED |
| split | `enemy_split` | OBSERVED |
| mole | `enemy_mole` | OBSERVED |
| diver | `enemy_diver` | OBSERVED |
| webspider | `enemy_webspider` | OBSERVED |
| normal_S | `enemy_normal_S` | OBSERVED |
| webegg | `enemy_webegg` | OBSERVED |
| slow | `enemy_slow` | OBSERVED |
| trampoline | `enemy_trampoline` | OBSERVED |
| bonus | `enemy_bonus` | OBSERVED |
| armored_100 | `enemy_armored_100` | OBSERVED |
| diamond_100 | `enemy_diamond_100` | OBSERVED |

## Boss

| 표시 | ID | 상태 |
|---|---|---|
| B:01 | `boss_01` | OBSERVED |
| B:02~04 | - | EXCLUDED · 현재 미구현 |

## Obstacle

| 용도 | JSON 식별 | 상태 |
|---|---|---|
| 세로 일자 | `type: v` | OBSERVED |
| 가로 일자 | `type: h` | VERIFIED_JSON |
| 사각 | `type: box` | VERIFIED_JSON |
| 폭탄 | `kind: bomb` | OBSERVED |
| 트램펄린 | `kind: trampoline` | OBSERVED |
| 용수철 | `kind: spring` | OBSERVED |
| 거미줄 | `kind: web` | OBSERVED |
| 생성/소멸 | `kind: blink` | OBSERVED |
| 일방통행 | `kind: oneWay` | OBSERVED |
| 체력 장애물 | `kind: breakable` | OBSERVED |
| 실시간 이동 | `kind: moverReal` | OBSERVED |
| 턴 이동 | `kind: moverTurn` | VERIFIED_PLAY |

### moverTurn verified fields

```text
type: box
kind: moverTurn
styleId: moverTurn
moveMode: linear
moveLoop: true
waypoints: [{row, col}, ...]
```

## Current scope note

- Skill은 현재 Map Curation Support 범위에서 제외.
- 특수 장애물의 세부 파라미터(속도, blink 주기, Hits 등)는 실제 성공 사례가 생길 때 추가한다.
