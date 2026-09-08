# 계정 레벨 베네핏

## Baseline
- QA 완료
- Recovery: 2026-09-08
- 레벨 상태 세팅이 필요한 심화 검증은 HOLD

## Source
- Work: `[프로필] 계정레벨배네핏보상` | last-known: `12861994506`
- QA: `[QA] 계정 레벨 베네핏 보상 기능 QA` | last-known: `12861865549`
- Design: `프로필 > 계정레벨배네핏UIUX` | last-known: `12840778597`
- Issues: SLM-0001, SLM-0002, SLM-0011, SLM-0012, SLM-0046, SLM-0047, SLM-0107, SLM-0108

## Coverage
- 스테이지 첫 클리어 → 계정 EXP 반영
- Lv1 수령 불가 / Lv2+ 보상 수령
- 모두받기 및 연속 입력 중복 지급 방지
- 레벨 배너 탐색 / 보상 상태 표시
- 보상 획득 연출 / 재화 HUD 반영

## Open
- HOLD: 임의 계정 레벨 / EXP 상태 세팅 수단 부재
  - 다중 레벨 상승 / leftover
  - 최대 레벨 / MAX / 이후 EXP
  - 다수 미수령 보상 누적 상태
  - 레벨 기반 UNLOCK 상태
  - 상태 Persistence / 재수령 경계

## Recheck
- 계정 레벨 / EXP 획득 구조
- 보상 수령 / 중복 지급 방지
- 보상 상태 및 획득 연출
- 프로필 EXP 영역 → 레벨 보상 팝업 진입
- 획득 가능 보상 연출 반영
- 레벨 상태 설정용 QA Tool 정상화
