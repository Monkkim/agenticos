# Agentic OS

**개인 브랜드/비즈니스 자동화를 위한 Claude Code 기반 운영 체제.**
모든 skill이 공유 Brand Context를 참조하고, 피드백이 누적되어 점점 똑똑해지며, 세션 시작/종료 시 자가 유지된다.

---

## 🧭 프로젝트 지도

```
brand-context/   # 브랜드 스냅샷 (voice, icp, positioning, samples, assets)
context/         # 동적 기억 (soul, user, memory, learnings, memory/YYYY-MM-DD.md)
skills/          # 카테고리별 skill (foundation / execution / strategy / ops)
projects/        # skill 산출물 (YYYY-MM-DD-<topic>/)
```

- **foundation/** — brand-context 최초 구축용 (start-here, brand-voice-extraction, icp-builder, positioning-builder)
- **ops/** — 시스템 자가 유지 (heartbeat, wrap-up, skill-creator)
- **execution/** · **strategy/** · **creative/** — 실제 산출물을 만드는 skill (필요 시 1개씩 추가)

## 🔑 핵심 작동 원리 (YOU MUST)

1. **모든 실행 Skill은 시작 전에 `brand-context/` 파일을 Read 한다.** 최소 `voice.md` + `icp.md` + `positioning.md`. 건너뛰면 일반적인 AI 산출물이 되어 버린다.
2. **세션 시작 시** `skills/ops/heartbeat/SKILL.md`를 참조하여 시스템 정합성을 체크한다. `CLAUDE.md`가 로드된 순간 heartbeat 절차를 수행한다.
3. **새 Skill을 만들기 전에** `skills/ops/skill-creator/SKILL.md`를 반드시 호출한다. 중복/의존성을 피하기 위함이다.
4. **세션 종료 시** 사용자가 "close session" / "wrap up" / "세션 종료" 같은 의사를 밝히면 `skills/ops/wrap-up/SKILL.md`를 호출한다.
5. **Skill 산출물은 `projects/YYYY-MM-DD-<topic>/`에 저장**한다. 날짜는 `date +%F`로.

## 📁 주요 경로 (자주 참조)

- `brand-context/voice.md` — 모든 글쓰기 skill의 1차 참조
- `brand-context/icp.md` — 타깃 오디언스 프로파일
- `context/learnings.md` — skill별 피드백 장기 기억 (wrap-up이 append)
- `context/memory/YYYY-MM-DD.md` — 세션별 단기 기억 (heartbeat가 load)
- `skills/foundation/_question-banks/<domain>.md` — start-here가 로드하는 도메인별 인터뷰 질문지

## 🧪 자기 검증 규칙

- Skill 실행 후 해당 산출물이 `brand-context/voice.md`의 "금지어" / "선호 표현"을 반영했는지 스스로 체크한다.
- 파일을 수정한 경우 `ls -la <path>`로 실제 반영 여부 확인.
- Git 커밋 전 `git diff --stat`으로 변경 범위 자체 검증.

## ⚠️ Caveats (코드만으로는 알 수 없는 것)

- 이 저장소는 **글로벌 `~/.claude/`와 완전히 독립**이다. `~/.claude/skills/`, `~/.claude/settings.json` 등을 수정하지 않는다.
- 사용자는 한국어로 작업한다. 모든 산출물/인터뷰 질문은 **한국어 기본**.
- 글로벌 `oh-my-claudecode`(OMC) 규칙을 상속하므로 에이전트 라우팅/모델 선택 규칙을 여기서 재선언하지 않는다.
- `context/memory/` 하위 파일은 민감 정보가 있을 수 있으니 git 커밋 전 wrap-up 검사 통과해야 한다.

## 🛠 자주 쓰는 명령

```bash
# 브랜드 컨텍스트 초기화 (최초 1회)
# → skills/foundation/start-here/SKILL.md를 따라간다

# 세션 종료 & 커밋
# → "세션 종료" 발화 → skills/ops/wrap-up 실행

# 새 skill 만들기 전
# → skills/ops/skill-creator 먼저 호출
```

## 📚 용어 정의

- **Brand Context** — `brand-context/` 아래 5개 파일로 구성된 브랜드 정체성 스냅샷 (정적, 분기 단위 갱신)
- **Context** — `context/` 아래 에이전트가 누적해서 유지하는 동적 상태 (세션별 갱신)
- **Skill 체인** — 한 skill의 산출물이 다른 skill의 입력이 되는 연결 구조 (예: trending-research → copywriting)
- **Foundation** — brand-context를 만들어내는 메타-skill (최초 1회 실행)
- **Ops** — 시스템 스스로를 유지하는 skill (heartbeat, wrap-up, skill-creator)

---

자세한 사용법은 `README.md`. 설계 근거는 `/Users/user/.claude/plans/how-smart-people-are-purring-pretzel.md`.
