---
name: positioning-builder
description: Use this skill to create or refresh brand-context/positioning.md — the document that defines category, target, key benefit, evidence, and competitor differentiation. Invoked by start-here, or directly when the user says "포지셔닝 갱신" or needs a landing-page-quality one-liner about who they are.
---

# positioning-builder

`brand-context/positioning.md`를 작성하는 skill. 한 줄 포지셔닝 + 카테고리 + 타깃 + 편익 + 증거 + 경쟁자 차별점을 구조화한다.

## 언제 실행되는가

- `foundation/start-here`가 Stage 2에서 호출
- 사용자가 "포지셔닝 갱신" / "자기소개 한 줄 정리" 발화
- 새 제품/채널 런칭 직전

## 입력

- `start-here`의 인터뷰 답변
- `brand-context/icp.md` (타깃 섹션 참조용)
- (선택) 경쟁자 URL/이름 목록

## 실행 절차

### 0단계 — 과거 학습 + ICP 로드

- `context/learnings.md`의 `foundation/positioning-builder` 섹션 Read
- `brand-context/icp.md` Read — 타깃 정의와 일관성 유지

### 1단계 — 템플릿 기반 질문

**March 포지셔닝 포뮬러**를 사용:

> "**_(타깃)_**를 위한 **_(카테고리)_**로서, **_(경쟁 대안)_** 대신 선택할 수 있는 **_(핵심 편익)_**을 제공한다."

한 번에 하나씩 묻는다:

1. **카테고리** — "지금 활동을 한 단어로 설명하면? (예: '콘텐츠 크리에이터', 'B2B SaaS', '1인 에이전시')"
2. **타깃 재확인** — `icp.md`에서 뽑은 한 문장을 제시하고 "이거 맞나요?" 확인
3. **핵심 편익** — "타깃이 얻는 구체적 변화 하나. 추상어 금지. (X 대신 Y가 된다)"
4. **경쟁 대안** — "타깃이 나 말고 생각할 수 있는 다른 선택지 2~3개. 직접 경쟁자든 대안적 방법이든."
5. **차별점** — 경쟁 대안마다 "내가 그것과 다른 한 가지"
6. **증거** — "내 주장을 왜 믿어야 하나? (실적 / 경험 / 사례 / 리스트)"
7. **Not-For** — "명시적으로 타깃하지 않는 사람은? (맞지 않으면 서로 시간 낭비)"

### 2단계 — 한 줄 포지셔닝 작성

수집된 정보로 **3개 후보**를 만들어 보여준다:
- 공식형 (March 포뮬러 그대로)
- 친근형 (구어체)
- 도발형 (짧고 강한 대조)

사용자가 하나 선택 또는 혼합.

### 3단계 — positioning.md 작성

`brand-context/positioning.md` Write. 다음 섹션 모두 채움:
- 한 줄 포지셔닝 (최종 확정본)
- 카테고리
- 타깃 (icp.md 요약 1문장)
- 핵심 편익
- 증거
- 경쟁자 비교 표
- Not-For

### 4단계 — 검증

- voice.md의 톤으로 한 줄 포지셔닝을 읽었을 때 어색한지 역검증
- icp.md의 Pain 섹션과 핵심 편익이 실제로 연결되는지 체크

승인 받으면 🟡 플레이스홀더 태그 제거.

### 5단계 — 학습 로깅

"한 줄 포지셔닝을 몇 번 고쳤는지", "어떤 부분에서 막혔는지"를 `learnings.md`에 기록.

## 출력 파일

- `brand-context/positioning.md` (덮어쓰기)

## 의존성

- `brand-context/icp.md` 필수
- `brand-context/voice.md` 선호 (톤 일관성 검증)
- `context/learnings.md`
