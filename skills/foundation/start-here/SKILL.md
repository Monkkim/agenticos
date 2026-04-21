---
name: start-here
description: Use this skill when the user invokes "start here", "브랜드 컨텍스트 초기화", or explicitly asks to onboard/set up the Agentic OS for their brand. This is the one-time orchestrator that conducts a two-stage interview (domain identification → domain-specific questions) and populates all five brand-context files (voice, icp, positioning, samples, assets) by sequentially delegating to brand-voice-extraction, icp-builder, and positioning-builder.
---

# start-here

Agentic OS의 **최초 1회 부트스트랩 오케스트레이터**. 사용자의 브랜드를 인터뷰해서 `brand-context/` 5개 파일을 채운다.

## 언제 실행되는가

- 사용자가 "start here" / "브랜드 컨텍스트 초기화" / "온보딩" 발화
- `brand-context/voice.md`가 플레이스홀더 상태임이 확인될 때

## 실행 절차

### 0단계 — 사전 점검

1. `/Users/user/My Workspace/AgenticOS/context/learnings.md`에서 `foundation/start-here` 섹션을 Read한다. 과거 피드백 확인.
2. `brand-context/voice.md`가 이미 실내용으로 채워져 있다면 사용자에게 재구축할지 확인.

### Stage 0 — 도메인 식별

사용자에게 AskUserQuestion으로 다음을 묻는다:

> "어떤 비즈니스/활동을 중심으로 Agentic OS를 세팅할까요?"

선택지:
- 유튜브/영상 콘텐츠
- 개인 브랜드 / 지식 창업 (뉴스레터·강의·SNS)
- SaaS / 프로덕트 마케팅
- 에이전시 / 프리랜서 서비스
- 이커머스
- 기타 / 아직 정의 안 됨

### Stage 1 — 도메인별 질문지 로드

선택된 도메인에 맞춰 질문지 뱅크를 Read:

| 선택 | 로드할 파일 |
|---|---|
| 유튜브 | `_question-banks/common.md` + `_question-banks/youtube.md` |
| 개인 브랜드 | `_question-banks/common.md` + `_question-banks/personal-brand.md` |
| SaaS | `_question-banks/common.md` + `_question-banks/saas.md` |
| 에이전시 | `_question-banks/common.md` + `_question-banks/agency.md` |
| 이커머스 | `_question-banks/common.md` + `_question-banks/ecommerce.md` |
| 기타 | `_question-banks/common.md`만 |

질문지에 따라 **한 번에 하나씩**, 가능하면 AskUserQuestion의 선택지 형태로 묻는다. 모든 답변을 메모리에 축적.

### Stage 2 — 하위 skill 순차 호출

수집된 답변을 인자/컨텍스트로 전달하며 세 skill을 순서대로 실행:

1. `foundation/brand-voice-extraction` → `brand-context/voice.md` 작성
2. `foundation/icp-builder` → `brand-context/icp.md` 작성
3. `foundation/positioning-builder` → `brand-context/positioning.md` 작성

각 하위 skill이 끝날 때마다 사용자에게 "이 파일 확인해주세요"라고 보여주고 승인 받는다.

### Stage 3 — samples & assets 수동 수집

`brand-context/samples.md`, `brand-context/assets.md`는 인터뷰로 완전히 채우기 어렵다. 대신:

- samples: "좋아하는 콘텐츠 레퍼런스 링크 2~3개와 내 과거 베스트 콘텐츠 링크 2~3개를 알려달라"고 요청 → 링크 자체를 파일에 기록
- assets: "공개 소셜 핸들·웹사이트·제품 URL을 알려달라"고 묻고 정리해서 기록

### Stage 4 — 검증 & 완료

1. `ls -la /Users/user/My Workspace/AgenticOS/brand-context/*.md`로 5개 파일 모두 실내용 확인.
2. 각 파일의 "상태" 태그를 🟡 플레이스홀더에서 제거.
3. `context/user.md`의 미입력 섹션을 이번 인터뷰에서 얻은 정보로 보강.
4. 사용자에게 "Brand Context 초기 설정 완료. 이제 execution skill을 호출하면 이 정보가 자동 반영됩니다"라고 알린다.
5. `context/learnings.md`의 `foundation/start-here` 섹션에 이번 세션 요약 한 줄 append.

## 중요 원칙

- **질문은 절대 쏟아붓지 말 것.** 한 번에 1개. 사용자가 피곤해 보이면 "여기까지 저장하고 다음에 이어갈까요?"
- **빈 답변을 억지로 채우지 말 것.** "모르겠다"고 하면 `(미입력 — 추후 갱신)` 마커를 남긴다.
- **공감보다 요약.** 사용자 답변을 그대로 복붙하지 말고, 요점을 한 줄로 정리해서 "이렇게 이해했는데 맞나요?"로 확인.

## 의존 skill

- `foundation/brand-voice-extraction`
- `foundation/icp-builder`
- `foundation/positioning-builder`

## 참조 파일

- `_question-banks/common.md` — 전 도메인 공통 질문
- `_question-banks/<domain>.md` — 도메인별 추가 질문
