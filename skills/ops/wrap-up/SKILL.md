---
name: wrap-up
description: Use this skill when the user signals end of session ("세션 종료", "wrap up", "close session", "오늘 작업 끝"). Reviews the session's deliverables, collects skill-by-skill feedback from the user, appends to context/learnings.md, updates affected SKILL.md files with recurring lessons, writes today's context/memory/YYYY-MM-DD.md, and commits+pushes to git.
---

# wrap-up

세션을 **의미 있게 종료**하는 skill. 오늘 배운 것을 내일의 자신에게 전달한다.

## 언제 실행되는가

- 사용자가 "세션 종료" / "wrap up" / "close session" / "오늘은 여기까지" 발화
- 세션 막바지에 사용자가 직접 호출
- **자동 트리거 금지** — 사용자 명시적 의사가 있을 때만.

## 실행 절차

### 1단계 — 오늘 세션 스냅샷 수집

- `context/memory/$(date +%F).md` Read
- 이번 세션에서 수정된 파일 목록 확보 — `git status` + `git diff --stat`
- 이번 세션에서 호출된 skill 추정 (사용자에게 확인)

### 2단계 — 산출물 리뷰

사용자에게:
> "오늘 만든 것을 정리해 보여드립니다. 각 항목에 대해 피드백을 짧게 주세요 (좋았음 / 아쉬웠음 / 이런 점만 고치면 완벽)."

산출물 하나씩 짧게 보여주고, 각각에 대해 **한 줄 피드백**을 받는다.

### 3단계 — Skill별 피드백 수집

각 호출된 skill에 대해 AskUserQuestion:
> "`<skill-name>` skill 품질은? (만족 / 보통 / 불만족 — 불만족이면 한 줄 코멘트)"

`보통` 이상: 별 조치 없음
`불만족`: 사용자의 한 줄 코멘트를 받아서 `learnings.md` + 해당 SKILL.md에 반영 대상으로 표시

### 4단계 — learnings.md 업데이트

피드백이 있는 skill 섹션에 다음 포맷으로 append:

```markdown
- YYYY-MM-DD: <한 줄 요약>
  - 맥락: <언제 발생>
  - 교훈: <앞으로 어떻게>
```

### 5단계 — SKILL.md 승격 판단

`learnings.md`의 각 skill 섹션을 다시 읽어서 **같은 교훈이 3회 이상 반복**되면:
1. 해당 SKILL.md의 "중요 원칙" 섹션에 공식 규칙으로 승격 제안
2. 사용자 승인 받으면 Edit으로 반영
3. `learnings.md`에는 `[승격됨 YYYY-MM-DD]` 태그 추가 (삭제하지 않음, 이력 보존)

### 6단계 — 오늘 메모리 파일 마감

`context/memory/$(date +%F).md`를 Edit으로 완성:

```markdown
# YYYY-MM-DD 세션

## 시작 시점
- 어제 이어받은 컨텍스트: ...
- 오늘 목표: ...

## 주요 산출물
- [파일명] 한 줄 설명
- ...

## 학습 (learnings.md로 복사됨)
- ...

## 미해결/다음 세션으로
- ...
```

### 7단계 — 민감 정보 검사

커밋 전 다음 패턴 검사 (간단한 grep):
- `API_KEY`, `SECRET`, `TOKEN`, `PASSWORD` 대문자
- `sk-`, `pk-`, `ghp_` 같은 키 접두어
- 이메일 주소가 예상 밖 위치에 등장

발견되면 **커밋 중단**하고 사용자에게 알림.

### 8단계 — Git 커밋 & 푸시

```bash
cd "/Users/user/My Workspace/AgenticOS"
git add -A
git commit -m "session($(date +%F)): <오늘 핵심 산출물 한 줄>"
git push origin main
```

커밋 메시지 컨벤션:
- `session(YYYY-MM-DD): 오늘 핵심 산출물`
- 여러 카테고리면 첫 줄 요약 + 본문에 불릿

### 9단계 — 종료 메시지

```
✓ 세션 요약 저장: context/memory/YYYY-MM-DD.md
✓ 학습 N건 기록
✓ Git 커밋 완료: <커밋 해시 앞 7자>
다음 세션 시작 시 heartbeat이 오늘 내용을 이어받습니다.
```

## 과거 학습 읽기

`context/learnings.md`의 `ops/wrap-up` 섹션 Read 후 실행.

## Idempotency

- 같은 날 wrap-up이 두 번 실행되면 memory 파일에 새 섹션 "## 추가 작업 (HH:MM)"으로 append
- git commit은 변경사항이 없으면 skip

## 출력 파일

- `context/memory/$(date +%F).md` (덮어쓰기/보강)
- `context/learnings.md` (append)
- 영향받은 `skills/**/SKILL.md` (승격된 규칙)
- git 저장소 (새 커밋)
