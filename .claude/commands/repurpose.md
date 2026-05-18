---
description: 유튜브 영상 1편 → 인스타·쓰레드·숏폼 훅 3개(+옵션 뉴스레터). 본체 스킬 호출.
argument-hint: <YOUTUBE_URL> [--newsletter]
---

# /repurpose

`/repurpose <YOUTUBE_URL>` — 유튜브 영상 1편을 인스타·쓰레드·숏폼 훅 3개로 변환합니다.

옵션:
- `/repurpose <URL> --newsletter` — 뉴스레터까지 함께 생성 (script-to-newsletter 12블록 패턴 호출)

---

인자: $ARGUMENTS

---

## 절차

이 슬래시 커맨드는 본체 스킬을 호출하는 얇은 트리거입니다.

1. **`skills/execution/repurpose/SKILL.md`를 Read하여 그 절차를 따릅니다.**
2. `$ARGUMENTS`에서 URL을 파싱해 입력으로 사용합니다.
3. `--newsletter`가 포함되면 뉴스레터 옵션 ON. 없으면 OFF (인스타·쓰레드·숏폼훅만).
4. URL이 없으면 사용자에게 한 번 안내: "유튜브 URL을 함께 보내주세요."
5. URL은 있는데 트랜스크립트가 없으면 안내: "트랜스크립트를 paste 해주세요. (유튜브 자막 펼치기 → 전체 복사)"

## 출력

- `output/repurpose/<날짜>-<topic>/instagram.md`
- `output/repurpose/<날짜>-<topic>/threads.md`
- `output/repurpose/<날짜>-<topic>/shorts-hooks.md`
- (옵션) `output/newsletter/<날짜>-<topic>/newsletter.md`

## 의존성 (본체 스킬이 자동 처리)

- `brand-context/` 5종(voice / icp / positioning / samples / assets)
- `context/learnings.md`의 `execution/repurpose` 섹션
- `skills/execution/repurpose/references/platform-formats.md`
- `skills/execution/repurpose/references/voice-rules-checklist.md`
- (옵션) `skills/execution/script-to-newsletter/references/newsletter-pattern.md`
