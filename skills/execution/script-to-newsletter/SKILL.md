---
name: script-to-newsletter
description: Use this skill when 사용자가 "뉴스레터 만들어줘" / "스티비로 보낼 거" / "스크립트로 뉴스레터" 같은 요청을 하거나, 유튜브 대본·외부 인물 인터뷰 메모·본인 노트를 데이븐포트(투쏠)의 표준 뉴스레터로 변환해달라고 할 때. 입력된 스크립트를 12블록 골격에 따라 재구성하여 스티비 발송용 마크다운으로 산출한다.
---

# script-to-newsletter

스크립트(유튜브 대본 / 외부 인터뷰 메모 / 본인 인사이트 메모)를 입력으로 받아, **데이븐포트(투쏠)의 표준 뉴스레터 1편**을 작성한다. 출력은 **스티비 에디터에 그대로 페이스트할 수 있는 마크다운**.

---

## 언제 실행되는가

- 사용자가 "뉴스레터 만들어줘" / "스티비로 보낼 거" / "스크립트로 뉴스레터" 발화
- 사용자가 스크립트 파일 경로(`output/newsletter/YYYY-MM-DD-<topic>/script.md` 등)를 제시
- 사용자가 채팅에 스크립트 본문을 직접 paste

---

## 실행 절차

### 0단계 — 의존성 Read (반드시 병렬)

다음을 한 번에 읽는다:

- `brand-context/voice.md`
- `brand-context/icp.md`
- `brand-context/positioning.md`
- `brand-context/samples.md`
- `skills/execution/script-to-newsletter/references/newsletter-pattern.md`
- `context/learnings.md`의 `execution/script-to-newsletter` 섹션

### 1단계 — 입력 수집

스크립트 확보 우선순위:
1. 사용자가 파일 경로 제시 → Read
2. 사용자가 채팅에 paste → 그대로 사용
3. 둘 다 없으면 → "스크립트를 어떻게 전달하실 건가요?" 1회 확인

추가 입력 (있으면 사용, 없으면 OK):
- 출처 링크 (외부 인물의 영상/글이라면)
- 미리 정한 제목 힌트
- 이번 편 끝에 붙일 공지/CTA (새 영상, 강의, 보이스 에이전트 시연 등)

### 2단계 — 스크립트 파싱 및 핵심 추출

다음 요소를 식별:

- **인물/사례 주체** — 누가 말한 건지
- **출처** — 영상 / 인터뷰 / 글 / 본인 메모
- **핵심 인용 1~3개** — 큰따옴표로 묶을 만한 강한 문장
- **사례의 작은 섹션 5~8개** — 흐름을 쪼갤 단위
- **본인 적용점** — voice/icp 기반으로 데이븐포트 맥락에 연결되는 지점
- **모티프 매칭** — 자주 등장하는 모티프(n8n→Claude Code, 시스템, 속도 vs 본질, 결과 vs 도구) 중 어떤 것과 가장 잘 묶이는지

### 3단계 — 뉴스레터 본문 작성

`references/newsletter-pattern.md`의 12블록 골격을 따라 작성한다. 각 블록은 그 문서의 작성 규칙을 그대로 적용.

**제목은 3~5개 후보를 먼저 만들고 1번을 본문에 사용**. 나머지는 사용자가 골라 바꿀 수 있도록 산출물 상단에 같이 출력.

작성 중 계속 점검할 체크리스트:
- [ ] 존댓말 `~해요/~습니다` 혼용
- [ ] 단락 사이 빈 줄 충분
- [ ] 한 단락 2~5줄
- [ ] 핵심 문장은 한 줄로 분리
- [ ] `$%name%$님` 5~10회 등장
- [ ] **마침표 사용** (SNS 규칙과 다름)
- [ ] 친근 마커 `:)` / `ㅎㅎ` 적절히 (남발 ❌)
- [ ] 분량 1500~3500자

### 4단계 — voice/positioning 자체 검증

- [ ] voice.md 금지어 미포함 (`딸깍`, `월천만원`, `혁신`, `게임체인저`, `여러분 모두`, `~해보시면 됩니다`)
- [ ] 후킹/어그로 제목 아님
- [ ] icp.md의 Pain/Desired Outcome과 연결되는 메시지
- [ ] positioning.md의 Not-For 영역 침범 안 함
- [ ] 서명·클로징이 고정 포맷 그대로
- [ ] 외부 인용은 인물명 명시, 가능하면 출처 링크

### 5단계 — 출력 저장

- 저장 경로: `output/newsletter/$(date +%F)-<topic>/newsletter.md`
- topic 슬러그: 본문 핵심을 5단어 내외 kebab-case (예: `system-vs-rituals`)
- 동시에 채팅에도 본문 출력 — 사용자가 즉시 검토·복사 가능하도록

산출 구성:
```
## 제목 후보
1. <제목 A — 본문 사용>
2. <제목 B>
3. <제목 C>

---

## 본문 (스티비 페이스트용)

[제목 1번]

안녕하세요 $%name%$님 ...
...
$%name%$님의 AI 디렉터
- 투쏠 드림 -
```

### 6단계 — 사용자 검토 안내

3줄 이내로 보고:
```
✓ 뉴스레터 v0.1 작성 완료 — output/newsletter/<경로>/newsletter.md (<글자수>자)
✓ 제목 후보 N개 / 본문은 1번 사용
→ 수정 요청: 제목 번호 변경 / 톤·분량 조정 알려주세요
```

### 마지막 단계 — 학습 로깅

사용자 수정 요청·피드백이 있었으면 `context/learnings.md`의 `execution/script-to-newsletter` 섹션에 다음 포맷으로 append:

```
- YYYY-MM-DD: <한 줄 요약>
  - 맥락: <어떤 상황>
  - 교훈: <다음 번엔 어떻게>
```

---

## 입력

- 스크립트 (파일 경로 또는 채팅 paste)
- (선택) 출처 링크, 제목 힌트, 공지/CTA 메모

## 출력

- `output/newsletter/$(date +%F)-<topic>/newsletter.md` — 스티비 호환 마크다운
- 채팅 출력 (제목 후보 + 본문)

## 의존성

- `brand-context/voice.md` — 톤·금지어
- `brand-context/icp.md` — 독자 페르소나
- `brand-context/positioning.md` — Not-For·핵심 편익
- `brand-context/samples.md` — 베스트 콘텐츠 레퍼런스
- `skills/execution/script-to-newsletter/references/newsletter-pattern.md` — 12블록 골격
- `context/learnings.md` — 과거 피드백

---

## 중요 원칙

- **마침표 규칙 분기**: voice.md의 "SNS 본문 마침표 생략" 규칙은 **뉴스레터엔 적용 안 됨**. 뉴스레터에서는 마침표 사용. 단, 한 줄로 떨어진 강조 라인은 생략 가능.
- **`$%name%$` 토큰 보존**: 스티비 변수. 5~10회 자연스럽게 등장. AI가 임의로 "고객님" / "독자님"으로 치환 ❌.
- **서명 고정**: `$%name%$님의 AI 디렉터` 줄바꿈 `- 투쏠 드림 -` — 절대 변경 금지.
- **출처 명시**: 외부 인물·사례 인용 시 인물명 + (가능하면) 링크.
- **AI는 초안만**: Dan Koe 사례 그대로 — 본문을 통째로 AI가 쓴 느낌은 페르소나 위배. 산출물은 "사용자가 손대기 쉬운 초안"이라는 전제.
- **본문 글쓰기 책임은 사용자**: 이 skill은 골격 + 1차 초안만 제공. 사용자가 본인 손으로 다듬는 것이 정상 워크플로우.

---

## 과거 학습 읽기

`context/learnings.md`의 `execution/script-to-newsletter` 섹션을 0단계에서 Read.

---

## 다음 갱신 트리거

다음 중 하나가 발생하면 이 skill 또는 references/newsletter-pattern.md를 업데이트:

- 발송 뉴스레터 5편 누적 → 패턴 재추출
- 사용자가 같은 수정 요청을 3회 반복 → 공식 규칙으로 승격
- voice.md에 "뉴스레터 마침표" 예외가 명시되면 그쪽으로 위임
- 스티비 외 다른 발송 플랫폼이 추가되면 출력 포맷 분기 추가
