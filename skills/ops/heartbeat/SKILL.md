---
name: heartbeat
description: Use this skill at the start of every Agentic OS session (triggered by the SessionStart hook message, or when the user says "heartbeat" / "세션 점검"). Scans skills/ for newly added or removed skills, syncs the skill list in CLAUDE.md and learnings.md, loads yesterday's memory/YYYY-MM-DD.md, and surfaces any drift between declared structure and actual filesystem state.
---

# heartbeat

세션 시작 시 **시스템 정합성 체크**를 수행하는 skill. 시스템이 자기 자신을 안다.

## 언제 실행되는가

- SessionStart 훅이 발화하면 → `.claude/settings.local.json`의 메시지가 이 skill을 상기시킴
- 사용자가 "heartbeat" / "세션 점검" / "상태 체크" 발화
- 새로운 Claude Code 세션에서 CLAUDE.md를 읽은 직후 자동 실행 권장

## 실행 절차

### 1단계 — 현재 상태 수집 (모두 병렬 Read/Glob)

- `ls skills/foundation skills/execution skills/strategy skills/creative skills/ops` — 실제 존재하는 skill 목록
- `brand-context/` 5개 파일 존재 여부 + "🟡 플레이스홀더" 태그 유무
- `context/memory/`에서 가장 최근 파일 (어제 또는 그 이전 세션)
- `context/learnings.md`의 섹션 목록

### 2단계 — 정합성 비교

| 체크 항목 | 기대 상태 | 현재 상태 차이 시 조치 |
|---|---|---|
| `skills/*` 폴더 vs `CLAUDE.md` 지도 | 일치 | `CLAUDE.md` 업데이트 제안 (사용자 승인 후 적용) |
| `skills/*` 폴더 vs `learnings.md` 섹션 | 일치 | 누락 섹션 자동 추가, 고아 섹션은 사용자에게 삭제 확인 |
| `brand-context/*.md` | 5개 모두 실내용 | 플레이스홀더가 있으면 `foundation/start-here` 안내 |
| 어제 `memory/*.md` 존재 | 존재 | Read해서 "어제 진행사항" 요약을 세션 컨텍스트에 주입 |

### 3단계 — 단기 기억 주입

`context/memory/`에서 최신 파일을 읽어 다음 포맷으로 사용자에게 한 줄 브리핑:

> "지난 세션(YYYY-MM-DD)에는 [핵심 산출물]을 만들었고, [미완료 항목]이 남아있습니다."

만약 `memory/`가 비어 있으면 "첫 세션이네요, 어떤 작업부터 할까요?"

### 4단계 — 오늘 세션 기록 초기화

`context/memory/$(date +%F).md` 파일이 없다면 다음 템플릿으로 생성:

```markdown
# YYYY-MM-DD 세션

## 시작 시점
- 어제 이어받은 컨텍스트: (한 줄)
- 오늘 목표: (사용자 입력 대기)

## 주요 산출물
- (세션 중 업데이트)

## 미해결/다음 세션으로
- (세션 중 업데이트)
```

### 5단계 — 요약 출력

사용자에게 3줄 이내로:

```
✓ 시스템 상태: OK (또는 발견된 이슈 N개)
✓ 어제 이어받은 것: <요약>
→ 오늘 무엇부터?
```

## Idempotency 규칙

- 같은 세션에서 heartbeat이 두 번 돌아도 오늘 `memory/` 파일을 덮어쓰지 않는다 (존재 체크 후 skip).
- `learnings.md`에 이미 섹션이 있으면 중복 추가하지 않는다.
- `CLAUDE.md` 업데이트는 반드시 사용자 승인 필요.

## 과거 학습 읽기

`context/learnings.md`의 `ops/heartbeat` 섹션 Read 후 실행.

## 출력 파일

- `context/memory/$(date +%F).md` (없을 때만 신규 생성)
- `context/learnings.md` (섹션 구조 동기화)
- `CLAUDE.md` (사용자 승인 시 skill 목록 갱신)
