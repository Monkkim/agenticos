# 🧭 Claude 최신 트렌드 브리프 (2026-04-21)

> **작성 맥락**: 데이븐포트 콘텐츠 소재 발굴을 위한 스냅샷. ICP 페르소나 "AI 흐름에 따라붙고 싶은 30~40대 실무자"를 염두에 두고 "지금 뭐부터 써야 하는가" 관점으로 정리.

---

## 1. 한 줄 요약

Claude는 2026-04 기준 **모델 세대 교체(Opus 4.7)** + **Claude Code의 OS화(Skills/Hooks/MCP Tool Search)** + **Agent SDK의 프로덕션화** 세 축이 동시에 뛰고 있음. 실무자 입장에서 가장 먼저 체감되는 변화는 **Opus 4.7 + MCP Tool Search + Skill 생태계**.

## 2. 타임라인 (모델 릴리스)

| 날짜 | 모델 | 포인트 |
|---|---|---|
| 2025-11-24 | Opus 4.5 | "Infinite Chats" — 컨텍스트 한도 에러 제거 |
| 2026-02-05 | Opus 4.6 | 에이전트 팀, PowerPoint 통합 |
| 2026-02-17 | Sonnet 4.6 | 토큰 절감 + 에이전틱 검색 성능 개선, 1M 컨텍스트 |
| 2026-04-16 | **Opus 4.7** ← 최신 | 에이전틱 코딩 "step-change" 개선, 새 토크나이저(동일 텍스트에 토큰 최대 +35%) |
| (진행 중) | Haiku 4.5 | 가장 빠르고 저렴. 코딩·컴퓨터 유즈·에이전트에서 Sonnet 4 수준 |

## 3. 모델 라인업 — 어느 걸 언제?

| 작업 | 추천 모델 | 이유 |
|---|---|---|
| 장기 에이전트·복잡 코딩 | Opus 4.7 | 최신. 지시 준수·비전·장시간 에이전트 안정성 |
| 일반 코딩·문서 처리 | Sonnet 4.6 | 속도·비용·1M 컨텍스트 균형 |
| 대량 일괄 처리·빠른 조회 | Haiku 4.5 | 저렴. 컴퓨터 유즈·에이전트 기본 성능 |

**실전 팁**: Opus 4.7은 토크나이저 변경으로 동일 문장에 최대 +35% 토큰을 쓸 수 있어, 예산 계산 시 이전 대비 **소폭 상향해야 함**.

## 4. Claude Code — "OS화"의 5가지 축

1. **Skill** — `~/.claude/skills/` 또는 프로젝트 로컬 `skills/`에 `SKILL.md` 한 장만 두면 자동 발동. SDK·빌드 스텝 필요 없음. **영상에서 본 Agentic OS 패턴의 기반.**
2. **Subagent** — `.claude/agents/*.md`. 새 컨텍스트 창에서 독립 실행, 결과만 메인에 반환. Explore/Plan 같은 빌트인 또는 커스텀.
3. **Hook** — 특정 이벤트(커밋 전, 툴 호출 후, 특정 파일 편집 시)에 셸 명령 강제 실행. 보안·자동 린팅·알림에 사용.
4. **MCP** — 외부 서비스(Telegram, Discord, 웹훅 등) 연동. 채널을 통해 외부 이벤트가 Claude 세션에 푸시됨.
5. **MCP Tool Search** — 2026 신기능. MCP 서버를 **레이지 로딩** → 컨텍스트 사용 최대 95% 감소. 여러 MCP 서버를 부담 없이 동시 연결 가능.

### 구조적 3계층 정리

| 계층 | 역할 | 예 |
|---|---|---|
| 대화 계층 | 사용자 ↔ Claude | 일반 대화 |
| 위임 계층 | 서브에이전트 | Explore / Plan / 커스텀 |
| 확장 계층 | MCP · Hook · Skill | 외부 서비스 · 자동 실행 · 도메인 지식 |

## 5. Agent SDK — "프로덕션 레벨"로 간다

- **SDK 제공**: Python / TypeScript 양쪽. Claude Code와 동일한 툴·에이전트 루프·컨텍스트 관리.
- **지속 세션**: session ID 반환 → 이어받기 가능. 파일 읽기·명령 실행·히스토리 유지.
- **상태 저장 권장**: 1턴 이상 에이전트는 Postgres/Redis/오브젝트 스토리지에 "대화 로그를 진실의 원천"으로 보관. SDK 세션 자체는 **일시적(ephemeral)** 로 취급.
- **샌드박스**: 프로세스 격리·네트워크 제어·일시적 파일시스템 필수. Kubernetes/ECS/Nomad 어디든 붙음.
- **Claude Managed Agents (Public Beta)** — Anthropic이 관리하는 에이전트 하니스. 컨테이너 설정·세션 실행 API 제공. 헤더 `managed-agents-2026-04-01` 필요. **자체 인프라 없이 에이전트 운영 가능**이 핵심.

## 6. 기타 신규 기능

- **Computer Use** — Cowork · Claude Code에 내장. Pro/Max 요금제에서 Dispatch 기능 개선.
- **ant CLI** — Claude API용 명령줄 클라이언트. YAML로 API 리소스 버전 관리.

---

## 7. 데이븐포트 관점 — 콘텐츠 소재 3개 (추천)

> 금지어(과한 후킹·딸깍) 회피 + 구체 수치 + 1인칭 실전 관점.

### 📌 소재 1 — "Opus 4.7 토크나이저 바뀐 얘기, 비용 체감 테스트"
- **훅**: "같은 프롬프트가 갑자기 35% 더 비싸졌다" — 실제 수치 비교
- **구조**: 번호 단문 7~10개. `4.6 vs 4.7` 같은 작업 토큰 소비 비교표.
- **왜 좋음**: ICP가 가장 먼저 궁금해할 **실비용 변화**. 어그로 아닌 실측.

### 📌 소재 2 — "MCP Tool Search로 컨텍스트 95% 줄여봤다"
- **훅**: "MCP 서버 10개 붙여도 컨텍스트 안 터짐"
- **구조**: Before(무거운 MCP 붙였을 때 컨텍스트 포화) → 설정 변경 → After 수치.
- **왜 좋음**: 시그니처 주제("Claude Code 실전")와 완전 일치. 한국어권에 아직 드문 구체 튜토리얼.

### 📌 소재 3 — "Claude Managed Agents — 자체 서버 없이 에이전트 돌리기"
- **훅**: "n8n 대신 Managed Agents로 옮겨봤다" (n8n → Claude Code 변천사 서사와 연결)
- **구조**: 헤더 설정 → 컨테이너 구성 → 첫 에이전트 실행까지 단계별.
- **왜 좋음**: 사용자 스토리("작년까지 n8n") 자연스럽게 재활용. 해외 경쟁자(Nick Saraev)는 아직 한국어로 다룬 콘텐츠 없음.

---

## 8. 참고 출처

- [Anthropic Release Notes (2026-04)](https://releasebot.io/updates/anthropic/claude)
- [Claude Models Overview](https://platform.claude.com/docs/en/about-claude/models/overview)
- [Agent SDK Overview](https://platform.claude.com/docs/en/agent-sdk/overview)
- [Claude Code Skills Docs](https://code.claude.com/docs/en/skills)
- [Opus 4.7 Pricing 분석](https://www.finout.io/blog/claude-opus-4.7-pricing-the-real-cost-story-behind-the-unchanged-price-tag)
- [Claude Model Lineup 2026 비교](https://blog.imseankim.com/claude-model-lineup-2026-opus-sonnet-haiku-comparison/)
- [Claude Code Full Stack 해설](https://alexop.dev/posts/understanding-claude-code-full-stack/)
- [My Claude Code Setup 2026 — okhlopkov](https://okhlopkov.com/claude-code-setup-mcp-hooks-skills-2026/)
- [Claude Managed Agents 가이드](https://blog.laozhang.ai/en/posts/claude-managed-agents)
- [Claude Agent SDK Production Patterns](https://www.digitalapplied.com/blog/claude-agent-sdk-production-patterns-guide)

---

## 9. 다음 액션

- [ ] 소재 1~3 중 하나 선택 → `execution/copywriting` skill(다음 세션에 구축)로 본 카피 생성
- [ ] Opus 4.7 토큰 소비 직접 테스트 → 수치 확보 후 소재 1 보강
- [ ] `strategy/trending-research` skill을 이번 리서치 흐름을 템플릿으로 정식 등록 (사용자 지정 다음 TODO)
