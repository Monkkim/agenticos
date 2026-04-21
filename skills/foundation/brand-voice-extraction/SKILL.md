---
name: brand-voice-extraction
description: Use this skill when building or refreshing brand voice documentation in brand-context/voice.md. Called by start-here during initial onboarding, or directly when the user says "브랜드 보이스 갱신" or provides new sample writing they want analyzed for tone patterns. Produces a structured voice.md with a one-line tone summary, three adjective keywords, sentence style, preferred/forbidden expressions, and before/after examples.
---

# brand-voice-extraction

사용자의 **말투·어휘·문장 패턴**을 추출해 `brand-context/voice.md`로 정리하는 skill.

## 언제 실행되는가

- `foundation/start-here`가 Stage 2에서 호출
- 사용자가 "브랜드 보이스 갱신" / "voice.md 다시 써줘" 발화
- 과거 글 샘플을 붙여넣으며 "이 톤을 분석해줘"라고 할 때

## 입력

다음 중 최소 하나:
1. `start-here`에서 넘어온 인터뷰 답변
2. 사용자가 제공한 실제 글/콘텐츠 샘플 (링크 또는 원문 3개 이상)
3. `brand-context/samples.md`의 기존 내용

## 실행 절차

### 0단계 — 과거 학습 확인
`context/learnings.md`의 `foundation/brand-voice-extraction` 섹션 Read.

### 1단계 — 데이터 수집

샘플이 부족하면 사용자에게 요청:
> "대표적인 과거 글 3개만 붙여넣어 주세요. 길 필요 없고, 톤이 잘 드러나는 부분이면 됩니다."

샘플이 전혀 없으면 **인터뷰 기반 추정 모드**로 전환:
- "존댓말 / 반말 / 섞어 씀?"
- "공식적 / 친근함 / 장난스러움 중 어느 쪽?"
- "전문가 톤 / 친구 톤?"
- "자주 쓰는 추임새·감탄사?"
- "절대 쓰기 싫은 표현?"

각 질문은 가능하면 AskUserQuestion 선택지로.

### 2단계 — 분석

수집한 데이터에서 다음 패턴을 뽑는다:

| 분석 항목 | 방법 |
|---|---|
| 평균 문장 길이 | 샘플 문장 글자수 중앙값 |
| 종결 어미 빈도 | `~습니다` / `~해요` / `~다` / `~임` 카운트 |
| 인칭 선호 | 1인칭 (나·제가) vs 2인칭 (여러분·당신) 빈도 |
| 반복 어휘 | 3회 이상 등장 고유 표현 |
| 구두점 습관 | `—` / `…` / `!` / `?` 사용 패턴 |

### 3단계 — voice.md 생성

`brand-context/voice.md`를 Edit/Write로 완성. 구조:

1. **한 줄 톤 요약** — 10~20자. (예: "전문가 동료처럼 단정하고 구체적")
2. **3가지 키워드** — 형용사 3개 (예: `단정한` / `실용적인` / `따뜻한`)
3. **문장 스타일** — 길이·종결·인칭 구체 수치
4. **자주 쓰는 표현** — 3~7개 실제 예
5. **금지어 / 피해야 할 표현** — 3~7개
6. **샘플 Before→After** — AI 일반형 문장 1개를 추출된 voice로 교정한 실제 예

### 4단계 — 검증

- 사용자에게 "이대로 저장해도 될까요?" 확인
- 금지어가 혹시 본인이 실제로 쓰는 표현인지 역검증 (실수 방지)
- `voice.md` 첫 줄의 "🟡 플레이스홀더" 태그 제거

### 5단계 — 학습 로깅

이번에 뽑힌 독특한 패턴을 `context/learnings.md`의 `foundation/brand-voice-extraction`에 한 줄로 기록. (예: `2026-04-21: 사용자는 "결국" 부사를 과용하는 경향. 카피에서 주의.`)

## 출력 파일

- `brand-context/voice.md` (덮어쓰기)

## 의존성

- `context/learnings.md` 참조
- (선택) `brand-context/samples.md` 참조
