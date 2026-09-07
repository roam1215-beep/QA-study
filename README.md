# QA OS

> **업무가 메인, 학습은 업무에서 발견한 약점을 개선하기 위한 성장 루프.**
> 채팅은 작업대, 원본 시스템은 사실의 근거, Git은 세션을 넘어 이어지는 최소 정본이다.

이 저장소는 두 축을 함께 관리한다.

1. **Work** — 실제 QA 업무를 안정적으로 수행하기 위한 운영 기준과 부팅 정보
2. **Study** — 업무에서 드러난 QA/기술 역량의 약점을 학습하고 다시 업무에 환류하는 공간

Git을 Monday/기획서/BTS/TC의 복제 저장소로 만들지 않는다. 최신 사실은 원본 시스템에서 다시 읽고, Git에는 **AI가 무엇을 어디서 어떻게 읽어야 하는지**와 **검증된 QA 운영 기준**만 남긴다.

## Repository Map

```text
Work/
└─ Camelio/
   ├─ QA_BOOT.md      # 새 세션 부팅 / Source 접근 / 안전 규칙
   ├─ QA_CURRENT.md   # 현재 QA 리뷰·Coverage·TC 기준
   └─ QA_ACTIVE.md    # 현재 Sprint와 Live Source Pointer

Tracker/             # 일별 활동 로그
Diary/               # 회고
Note/                # 학습 기록
Quiz/                # 인출/복습
Library/             # 학습 원문·정리
capability/          # 현재 역량 / 채용 자료
```

## Core Principles

- **Source first** — 최신 업무 사실은 Monday / 기획 Source / Google Drive 등 원본에서 확인한다.
- **AI memory is convenience, not truth** — 대화/프로젝트 메모리는 편의를 위한 보조 수단이며, LIVE Source나 Git 정본과 충돌하면 버린다.
- **Git is a boot layer, not a mirror** — Sprint 일감, BTS 전체, TC 전체, 기획서 전체를 Git에 복제하지 않는다.
- **Evidence before Expected** — 원문 근거가 없는 Expected는 만들지 않는다. 필요하면 QUESTION/HOLD로 둔다.
- **Work → Study → Work** — 실제 업무에서 반복적으로 드러난 약점을 공부하고, 다음 업무에서 개선 여부를 검증한다.
- **과기록 금지** — 기록을 정리하느라 QA/학습 시간이 줄어들면 본말전도다.

## Camelio QA

까멜리오 업무를 시작하는 새 ChatGPT 세션은 먼저 아래 문서를 읽는다.

1. `Work/Camelio/QA_BOOT.md`
2. `Work/Camelio/QA_CURRENT.md`
3. `Work/Camelio/QA_ACTIVE.md`

그 다음 `QA_ACTIVE.md`가 가리키는 LIVE Source를 실제 조회한 뒤 업무를 시작한다.

**중요:** 까멜리오 프로젝트에서 Monday.com은 ChatGPT에게 절대 **READ-ONLY**다. 조회 외의 생성/수정/삭제/댓글/상태 변경 등 Write는 하지 않는다.

---

## Study OS

학습은 별도 목적이 아니라 QA 역량을 높이기 위한 보조 축이다.

- **소스 = 디지털·공식 우선** — 공식 문서, 정식 전자 자료, 검증 가능한 원문을 우선한다.
- **AI는 가속기, 검증은 소스** — AI가 설명·문제 출제·정리를 도울 수 있지만 사실 판단은 검증 가능한 Source로 돌아간다.
- **인출은 손·머리로** — 읽고 끄덕이는 것으로 끝내지 않고 퀴즈, 직접 실행, 결과 확인으로 회수한다.
- **기록은 가볍게, 정본은 하나만** — 날짜 Note는 로그이고, 현재 상태/다음 목표는 필요한 정본에만 유지한다.

### 기존 학습 경로

```yaml
Tracker/년/월/일.md       매일 한 활동 목록
Diary/년/월/일.md         그날의 회고
Note/년/월/일_트랙.md     그날 배운 내용 상세
Quiz/년/월/파일.md        주간/다일치/월간 인출
Library/                  교재·학습 자료
capability/               현재 스펙·능력 + 채용 자료
```

### 주간 트리거

- 그 주 Note를 다시 읽고 반추한다.
- 약점 위주로 짧은 종합 퀴즈를 돌린다.
- 업무에서 반복적으로 막힌 부분이 있으면 다음 학습 후보로 올린다.
