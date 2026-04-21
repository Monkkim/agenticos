---
name: icp-builder
description: Use this skill to build or refresh the Ideal Customer Profile document at brand-context/icp.md. Invoked by start-here during onboarding, or directly when the user says "ICP 갱신" / "타깃 오디언스 다시 정리". Produces a persona-centric document with pain points, desired outcomes, decision triggers, turn-offs, and voice-of-customer quotes.
---

# icp-builder

`brand-context/icp.md`를 **페르소나 기반**으로 작성하는 skill.

## 언제 실행되는가

- `foundation/start-here`가 Stage 2에서 호출
- 사용자가 "ICP 갱신" / "타깃 오디언스 다시 정리" 발화
- `content/icp.md`에 새로 알게 된 고객 인사이트가 있을 때

## 입력

- `start-here`에서 수집한 인터뷰 답변
- 또는 사용자가 직접 입력하는 답변
- (선택) 실제 고객 댓글·후기·DM — voice-of-customer 섹션에 인용

## 실행 절차

### 0단계 — 과거 학습 확인
`context/learnings.md`의 `foundation/icp-builder` 섹션 Read.

### 1단계 — 페르소나 인터뷰

한 번에 한 질문씩 AskUserQuestion으로:

1. **주 페르소나 한 명** — "머릿속에 떠오르는 대표 고객 한 명이 있나요? 실제 인물이 있다면 그 사람을 기준으로."
2. **나이대/직군** — 선택지 제공 (20대 학생 / 20대 직장인 / 30대 실무자 / 30대 관리자 / 40대+ / 창업자 / 프리랜서 / 기타)
3. **하루 일과** — "그 사람이 내 콘텐츠를 만나는 순간은 언제? (출근길 / 점심시간 / 저녁 / 주말 등)"
4. **Pain** — "그 사람이 해결하고 싶은 가장 큰 문제는?"
5. **현재 시도 중인 대안** — "그 사람이 지금 이 문제를 어떻게 풀고 있나? (다른 서비스 / 유튜버 / 책 / 포기 중)"
6. **Desired Outcome** — "이 문제가 풀리면 그 사람의 일상이 어떻게 바뀌나?"
7. **결정 트리거** — "그 사람이 '이거다' 하고 반응하는 순간은? (특정 문구 / 상황 / 증거)"
8. **반감 포인트** — "그 사람이 '이건 아니야' 하고 바로 이탈하는 말/스타일은?"
9. **주 채널** — 선택지 제공 (유튜브 / 인스타 / X / 링크드인 / 뉴스레터 / 오프라인)
10. **실제 발화** — "최근에 그 사람 비슷한 사람한테서 받은 DM/댓글/후기가 있나? 있으면 원문 그대로."

### 2단계 — icp.md 작성

답변을 정리해서 `brand-context/icp.md`를 Write. 원칙:

- **이름은 가상으로 만든다.** (예: "30대 SaaS 마케터 '지은'")
- **Pain과 Desired Outcome은 1인칭으로 서술** (예: "나는 X를 할 때마다 Y가 힘들다")
- **실제 발화가 있으면 원문 그대로 인용** — 가장 중요한 voice-of-customer 자료.
- **Turn-offs는 브랜드가 즉시 피해야 할 신호.** 모든 execution skill의 금지 체크리스트로 쓰인다.

### 3단계 — 검증

사용자에게 정리본을 보여주고:
> "이 사람을 상상하며 다음 글을 쓰면 되겠는지?"

승인 받으면 🟡 플레이스홀더 태그 제거.

### 4단계 — 학습 로깅

독특한 인사이트(예: "타깃이 예상보다 어리다", "Pain이 기술이 아니라 감정")를 `learnings.md`에 append.

## 출력 파일

- `brand-context/icp.md` (덮어쓰기)

## 의존성

- `context/learnings.md` 참조
- (선택) `brand-context/voice.md` — 페르소나 어투와 voice의 일관성 검증
