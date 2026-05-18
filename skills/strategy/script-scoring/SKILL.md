---
name: script-scoring
description: Use this skill when 사용자가 "스크립트 점수", "이 대본 어때", "촬영 전 검토", "이 영상 잘 될까" 같은 요청을 할 때. 새 유튜브 스크립트(촬영 전 대본)를 입력으로 받아 가장 최근 channel-audit과 brand-context를 기준으로 1~10점 채점하고 7점 미만 섹션의 즉시 리라이트 제안을 산출한다.
---

# script-scoring

촬영 전 스크립트를 4축(훅 / 주제 적합도 / 길이·호흡 / CTA)으로 1~10점 채점. 가장 최근 `youtube-audit` 결과가 있으면 그걸 기준으로, 없으면 voice + samples + 일반 벤치마크로 평가.

---

## 언제 실행되는가
- 사용자가 "스크립트 점수" / "채점" / "이 대본 어때" / "촬영 전 검토" 발화
- 사용자가 스크립트 파일 또는 paste 제공
- `youtube-audit` 직후 사용자가 새 스크립트를 가지고 옴

---

## 실행 절차

### 0단계 — 의존성 Read (병렬)
- `brand-context/voice.md` / `icp.md` / `positioning.md` / `samples.md`
- `context/learnings.md`의 `strategy/script-scoring` 섹션
- 가장 최근 `output/analysis/*/audit.md` (있으면) — 채널별 패턴 기준

audit이 없으면 사용자에게 1회 안내:
> "최근 channel-audit이 없습니다. 채널 데이터로 채점하려면 `youtube-audit` 먼저 돌리는 걸 권장해요. 진행할까요? (없이 진행해도 voice·samples 기준으로 채점은 가능)"

### 1단계 — 스크립트 입력 수집
- 파일 경로 → Read
- 채팅 paste → 그대로 사용

### 2단계 — 4축 채점 (각 1~10점)

**축 A — 훅 강도**
- 평가 대상: 첫 30초(혹은 첫 100자)
- 가산:
  - audit의 "이 채널만의 훅 공식"과 일치 → +2~3점
  - 도구명·구체 결과 명시 → +2점
  - 1인칭 동료 포지션 → +1점
- 감산:
  - 어그로/금지어 사용 → -3점

**축 B — 주제 적합도**
- 가산:
  - audit 우수 주제 카테고리에 포함 → +3점
  - positioning 시그니처 주제(보이스 에이전트 / Claude Code 실전) → +2점
  - icp "준호"의 Pain 직접 해결 → +2점
- 감산:
  - Not-For 영역 → -3점

**축 C — 길이 / 호흡**
- 가산:
  - audit sweet spot ± 20% 안 → +3점
  - 본론 도착까지의 시간(`hook → 본론` 전환) 적정 → +1~2점
  - 단락 호흡, 빈 줄 적정 → +1점
- 감산:
  - 너무 길거나 너무 짧음 → -2점

**축 D — CTA 명료도**
- 가산:
  - 단일 명확 CTA → +3점
  - assets.md 허용 CTA → +2점
- 감산:
  - 복수 CTA / 어그로 CTA → -2점

각 축 1~10점 → **총점 평균** + 4축별 점수 명시.

### 3단계 — 7점 미만 섹션 리라이트

각 7점 미만 축에 대해:
- 무엇이 약했는가 1줄
- 즉시 리라이트 제안 (정확한 한국어 문장 1~3개) — 사용자가 복사해 그대로 쓸 수 있는 수준

### 4단계 — 산출물 작성

`output/analysis/$(date +%F)-script-scoring/` 디렉토리에 1~2개 파일:

**score-report.md** (항상 생성)
```markdown
# 스크립트 채점 — YYYY-MM-DD

## 총점: X.X / 10
- 훅 강도: X / 10
- 주제 적합도: X / 10
- 길이 / 호흡: X / 10
- CTA 명료도: X / 10

## 채점 근거
(축별로 1~3줄 평가)

## 7점 미만 축
(있으면 나열, 없으면 "없음")

## 종합 의견 (3줄)
- 강점:
- 약점:
- 다음 액션:
```

**rewritten-sections.md** (7점 미만 축이 있을 때만 생성)
```markdown
# 리라이트 제안 — YYYY-MM-DD

## [축 X] 원본
> ...

## [축 X] 제안
> 제안 1
> 또는
> 제안 2
```

### 5단계 — 사용자 검토 안내
3줄 보고:
```
✓ 스크립트 채점 완료 — 총 X.X/10 (output/analysis/<날짜>-script-scoring/)
✓ 약점: <축> ← rewritten-sections.md에 즉시 리라이트 N개
→ 7점 이상으로 올리고 다시 들고 와주시면 재채점합니다
```

### 마지막 단계 — 학습 로깅
사용자 피드백 → `context/learnings.md`의 `strategy/script-scoring` 섹션 append.

---

## 입력
- 스크립트 (파일 경로 또는 paste)
- (자동) 가장 최근 audit.md (`output/analysis/*` 안에서 자동 탐색)

## 출력
- `output/analysis/$(date +%F)-script-scoring/score-report.md`
- (조건부) `output/analysis/$(date +%F)-script-scoring/rewritten-sections.md`

## 의존성
- `brand-context/voice.md` / `icp.md` / `positioning.md` / `samples.md`
- (선택) `output/analysis/<latest>/audit.md`
- `context/learnings.md`

## 중요 원칙
- **audit이 있으면 audit 우선**: 일반 베스트 프랙티스보다 이 채널 데이터.
- **점수는 절대값이 아닌 상대값**: "이 채널 평균 대비"로 해석.
- **즉시 리라이트는 사용자가 복사해서 쓸 수 있는 한국어 문장**으로 — 일반론 ❌.
- **재채점 환영**: 사용자가 수정 후 다시 들고 오면 같은 폴더에 v2 추가.

## 다음 갱신 트리거
- 5회 채점 누적 → 4축 가중치 사용자 피드백으로 조정
- 사용자가 "이 점수가 너무 후하다/박하다" 3회 → 채점 캘리브레이션
- audit 결과 포맷 변경 → 입력 파싱 갱신
