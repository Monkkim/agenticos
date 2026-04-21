# Learnings Log

Skill별 피드백이 누적되는 **학습 로그**. `ops/wrap-up`이 세션 끝마다 이 파일에 append하고, 중요한 학습은 해당 skill의 `SKILL.md`에 반영한다.

## 사용 규칙

1. 모든 skill은 실행 전 **자기 섹션을 Read**한다 — 과거 피드백을 무시하지 않기 위함.
2. `wrap-up`이 피드백을 기록할 때 **날짜 + 한 줄 요약 + (선택) 재현 예시** 구조를 지킨다.
3. 같은 학습이 3회 이상 반복되면 → 해당 skill의 `SKILL.md`에 공식 규칙으로 승격.
4. Skill이 삭제되면 해당 섹션도 같이 제거 (`ops/heartbeat`이 감지).

---

## foundation/start-here

- 2026-04-21: 최초 온보딩 완료. 도메인=개인 브랜드/지식 창업 선택, brand-context/ 5개 파일 v0.1 생성.
  - 맥락: 사용자가 여러 질문에 "아직 정리 안 됨"으로 답변(Pain 구체화·반직관 조언·시그니처 에피소드) → 억지 채움 대신 `(미입력)` 마커로 남기고 진행.
  - 교훈: 인터뷰 완결성보다 "즉시 사용 가능한 초안" 생성이 우선. 미입력 구간은 명시하고 후속 세션에 갱신.

## foundation/brand-voice-extraction

- 2026-04-21: 실제 글 샘플 없이 인터뷰 기반 v0.1 생성. 사용자가 샘플(호라이즌프레스 인스타) 공유 시점에 "SNS 본문 마침표 생략" 규칙 추가로 확보 → voice.md에 즉시 반영.
  - 교훈: 샘플 하나라도 확보되면 voice 정밀도가 크게 올라간다. 샘플 요청을 Stage 3가 아니라 Stage 1~2 사이로 앞당기면 효율 증가 가능.

## foundation/icp-builder

- 2026-04-21: Pain 구체화는 사용자 답변에 없었지만 "AI에 적응하고 싶은 30~40대"라는 정체성 답변 하나로 Pain/Desired를 합리적으로 확장 작성 가능. voice-of-customer 섹션은 실데이터 확보 전까지 템플릿만 남김.

## foundation/positioning-builder

- 2026-04-21: 사용자가 한 줄 포지셔닝 단일 확정 대신 "세 개 모두 보존" 선택. 공식형/친근형/도발형 3후보를 채널별로 변주 사용하는 전략 — 추후 사용 후 승자 정해도 OK.

## foundation/brand-voice-extraction

_(세션 피드백 없음)_

## foundation/icp-builder

_(세션 피드백 없음)_

## foundation/positioning-builder

_(세션 피드백 없음)_

## ops/heartbeat

_(세션 피드백 없음)_

## ops/wrap-up

- 2026-04-21: 첫 wrap-up 실행. heartbeat가 자동 발화되지 않아 오늘 memory 파일이 없던 상태 → wrap-up이 신규 생성.
  - 맥락: SessionStart 훅이 기대한 자동 heartbeat이 실제로 돌지 않은 사례.
  - 교훈: 다음 세션 시작 시 수동으로 heartbeat 확인 필요. 훅 설정/메시지 검증을 다음 세션의 첫 TODO에 추가 고려.

## ops/skill-creator

_(세션 피드백 없음)_
