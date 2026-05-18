---
name: repurpose
description: Use this skill when 사용자가 "/repurpose" 슬래시 커맨드 또는 "유튜브 영상 콘텐츠로 풀어줘", "이 영상 인스타·쓰레드용으로 만들어줘", "숏폼 훅 뽑아줘" 같은 요청을 할 때. 유튜브 영상 1편(URL+트랜스크립트)을 입력으로 받아 인스타 캡션·쓰레드 멀티포스트·숏폼 훅 3개(+옵션 뉴스레터)를 데이븐포트(투쏠) 보이스로 일관되게 생성한다.
---

# repurpose

유튜브 영상 1편 → **인스타 캡션 / 쓰레드 멀티포스트 / 숏폼 훅 3개**를 한 번에 생성. 뉴스레터는 옵션으로 `script-to-newsletter`의 12블록 패턴을 호출. 모든 출력은 `brand-context/` 5종 + voice 규칙(SNS 마침표 생략·금지어)을 일관 적용.

---

## 언제 실행되는가

- 사용자가 `/repurpose <URL>` 슬래시 커맨드 발화
- "유튜브 영상 다 풀어줘" / "이 영상 콘텐츠로 만들어줘" / "인스타·쓰레드 캡션 뽑아줘" / "숏폼 훅 만들어줘" 발화
- `script-to-newsletter` 사용 후 "다른 플랫폼은?" 후속 요청

---

## 실행 절차

### 0단계 — 의존성 Read (반드시 병렬)

다음을 한 번에 읽는다:

- `brand-context/voice.md`
- `brand-context/icp.md`
- `brand-context/positioning.md`
- `brand-context/samples.md`
- `brand-context/assets.md`
- `skills/execution/repurpose/references/platform-formats.md`
- `skills/execution/repurpose/references/voice-rules-checklist.md`
- `context/learnings.md`의 `execution/repurpose` 섹션

뉴스레터 옵션 ON일 때 추가:
- `skills/execution/script-to-newsletter/SKILL.md`
- `skills/execution/script-to-newsletter/references/newsletter-pattern.md`

### 1단계 — 입력 수집

확보 우선순위:
1. 사용자가 URL + 트랜스크립트를 같이 paste → 그대로 사용
2. URL만 있으면 → "트랜스크립트를 paste 해주세요. (유튜브 자막 펼치기 → 전체 복사)" 1회 안내
3. 추가 입력(옵션):
   - topic 슬러그 힌트 (없으면 본문에서 자동 추출)
   - 뉴스레터 포함 여부 (`--newsletter` 또는 "뉴스레터도" 키워드)
   - 영상 게시 시점 (게시 직후 / 하루 후 등 — CTA 톤 조절용)

### 2단계 — 핵심 추출

트랜스크립트에서 식별:

- **단일 핵심 인사이트** 1개 — 인스타 첫 줄 / 쓰레드 1번 게시물의 훅 재료
- **보강 포인트 3~5개** — 단계 / 비교 / 사례
- **구체 수치·도구명** — 인사이트의 증거 (모호한 "많은" 대신 "1만 명" / "Claude Code" 등)
- **본인 적용점** — `icp.md`의 "준호" 페르소나 기준으로 "이 영상이 준호의 어느 Pain을 풀어주는가"
- **CTA 1개** — `assets.md`의 허용 CTA 목록에서 선택

### 3단계 — 플랫폼별 본문 생성

`references/platform-formats.md`의 규칙을 그대로 따른다.

**3-1. 인스타 캡션** → `output/repurpose/<날짜>-<topic>/instagram.md`
- 첫 125자 안에 훅 (도구명·구체 결과 포함)
- 2~3문단, 한 단락 2~5줄
- 호라이즌프레스 스타일 번호 단문 리스트(2~10개) 적극 활용
- **본문 마지막 문장 마침표 생략** ← voice.md 규칙
- CTA 1개
- 해시태그 3~5개를 `## 해시태그` 별도 섹션 (첫 댓글 붙임용)

**3-2. 쓰레드 멀티포스트** → `output/repurpose/<날짜>-<topic>/threads.md`
- 1번 게시물: 500자 이내 훅 — 단독으로도 흥미 유발
- 2~5번: 한 게시물당 1 아이디어
- 게시물 사이 `---` 구분 (실제 게시 시엔 새 게시물로 따로 올림)
- **각 게시물 본문 마지막 마침표 생략**
- 마지막 게시물에 CTA + 다음 채널 안내 한 줄

**3-3. 숏폼 훅 3개** → `output/repurpose/<날짜>-<topic>/shorts-hooks.md`
- 각 훅 10단어 이내 (한국어 6~10어절)
- 도구명을 첫 줄(첫 1~3어절 안)에 노출
- 시청자가 자기 모습을 그릴 수 있는 형태 (`[도구] + [내가 ~한다]`)
- 금지 시작어: `안녕하세요` / `오늘은` / `여러분` / `지금부터`
- 3개 훅이 서로 다른 각도(결과 / 대조 / 도구) 다룸

**3-4. 뉴스레터** (옵션, 사용자가 `--newsletter` 또는 "뉴스레터도" 명시 시)
- `skills/execution/script-to-newsletter/references/newsletter-pattern.md`의 12블록 그대로 호출
- 출력 위치는 분리: `output/newsletter/<날짜>-<topic>/newsletter.md`
- **마침표 규칙은 SNS와 반대로 사용** (newsletter-pattern.md 규칙 준수)

### 4단계 — voice/positioning 자체 검증

`references/voice-rules-checklist.md`를 1행씩 통과시킨다. 위반 시 즉시 재작성.

핵심 항목 (전체 목록은 체크리스트 파일 참조):
- [ ] 인스타·쓰레드 본문 마지막 문장 마침표 없음
- [ ] 금지어 미포함 (`딸깍` / `월천만원` / `대박` / `혁신` / `게임체인저` / `누구나` / `여러분 모두` / `~해보시면 됩니다`)
- [ ] 후킹·어그로 첫 줄 아님
- [ ] 1인칭 ("저는" / "제가") 우세
- [ ] 숫자·도구명 구체
- [ ] 숏폼 훅 모두 10단어 이내, 도구명 첫 줄
- [ ] (뉴스레터 ON일 때) `$%name%$님` 5~10회 + 서명 고정

### 5단계 — 출력 저장

```
output/repurpose/$(date +%F)-<topic>/
├── instagram.md
├── threads.md
└── shorts-hooks.md
```

뉴스레터 옵션 ON일 때만 추가:
```
output/newsletter/$(date +%F)-<topic>/
└── newsletter.md
```

topic 슬러그: 본문 핵심을 5단어 내외 kebab-case (예: `claude-code-vs-n8n`). 사용자 힌트 우선.

### 6단계 — 사용자 검토 안내

3줄 이내 보고:
```
✓ /repurpose 완료 — output/repurpose/<경로>/ (instagram / threads / shorts-hooks)
✓ 뉴스레터 옵션: ON/OFF
→ 수정 요청: 톤·길이·CTA 알려주세요
```

### 마지막 단계 — 학습 로깅

사용자 수정 요청·피드백이 있었으면 `context/learnings.md`의 `execution/repurpose` 섹션에 append:

```
- YYYY-MM-DD: <한 줄 요약>
  - 맥락: <어떤 상황>
  - 교훈: <다음 번엔 어떻게>
```

---

## 입력

- 유튜브 URL (필수)
- 트랜스크립트 (필수, 사용자 paste)
- (선택) topic 슬러그 힌트 / 뉴스레터 포함 / 영상 게시 시점

## 출력

- `output/repurpose/$(date +%F)-<topic>/instagram.md`
- `output/repurpose/$(date +%F)-<topic>/threads.md`
- `output/repurpose/$(date +%F)-<topic>/shorts-hooks.md`
- (옵션) `output/newsletter/$(date +%F)-<topic>/newsletter.md`

## 의존성

- `brand-context/voice.md` — 톤·금지어·SNS 마침표 규칙
- `brand-context/icp.md` — "준호" 페르소나
- `brand-context/positioning.md` — 핵심 편익·Not-For
- `brand-context/samples.md` — 번호 단문 리스트 레퍼런스
- `brand-context/assets.md` — 핸들·CTA 허용·금지 표현
- `skills/execution/repurpose/references/platform-formats.md` — 플랫폼별 포맷 규칙
- `skills/execution/repurpose/references/voice-rules-checklist.md` — 자체 검증 체크리스트
- (옵션) `skills/execution/script-to-newsletter/references/newsletter-pattern.md` — 뉴스레터 12블록
- `context/learnings.md` — 과거 피드백

---

## 중요 원칙

- **마침표 규칙 분기**: 인스타·쓰레드는 본문 마지막 마침표 생략. 뉴스레터는 마침표 사용.
- **AI는 초안만**: 사용자가 손대기 쉬운 1차 초안. 통째로 게시하지 않는다는 전제.
- **출처 명시**: 외부 인물·사례 인용 시 인물명 + (가능하면) 링크.
- **숏폼 훅은 시청자 행동 가능 형태**: 멋진 표현보다 "내가 따라할 수 있겠다"는 감각.
- **뉴스레터 패턴 재사용**: 별도 패턴 만들지 않고 `script-to-newsletter`의 12블록 그대로. voice 규칙은 `repurpose`가 일괄 적용.
- **카드뉴스 미포함**: 본 skill은 텍스트 산출물만. 카드뉴스는 후속 별도 skill (`creative/card-news`)로 분리 예정.

---

## 과거 학습 읽기

`context/learnings.md`의 `execution/repurpose` 섹션을 0단계에서 Read.

---

## 다음 갱신 트리거

다음 중 하나가 발생하면 이 skill 또는 references/를 업데이트:

- 5편 이상 누적 → 사용자 수정 패턴 재추출
- 같은 수정 요청 3회 반복 → 공식 규칙 승격
- 쓰레드/인스타 정책(글자 수·해시태그 제한 등) 변경
- 새 플랫폼 추가 (X·링크드인 등) → 카테고리 분기 추가
