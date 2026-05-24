> [!abstract]+ 📋 Knowledge Card
> **유형**: How-To · Pattern
> **도메인**: ai-os / second-brain
> **출처**: YouTube · Ben (AI 에이전시 운영자) · 2026-05-24 — https://www.youtube.com/watch?v=zElKhlFkqU4
> **요약**: 클로드 기반 AI 운영체제(Second Brain)를 5개 Skill로 구축·운영·최적화하는 가이드. 폴더+CLAUDE.md를 기반으로 시작하고, 실시간 컨텍스트 자동 수집 → 정기 hygiene → 팀 공유 → MCP로 자율 운영까지 단계적으로 확장.

# AI 운영체제(Second Brain) 5-Skill 구축 가이드

## 왜 Second Brain인가
- **모든 AI 도구(Claude Code, Codex, Co-work 등)에 persistent 컨텍스트/메모리 주입** → 매 채팅마다 복붙할 필요 없음, 항상 최신 비즈니스 컨텍스트 자동 pull
- **컨텍스트는 시간에 따라 compound** — 6개월 뒤 AI는 오늘 시작한 AI보다 훨씬 강력
- **팀 공유 시 모든 구성원의 AI가 같은 전략·맥락에 정렬** → "AI는 다 동의해주는 문제"(institutional vs individual AI) 해결
- 폴더 + connector + MCP + skills/routines/loops 조합으로 **AI가 일의 주 인터페이스가 됨**

## Obsidian의 역할
- Obsidian = **로컬 폴더의 시각화 오버레이** (실제 데이터는 그냥 .md 폴더)
- 무료, Mac/Win 다운로드 → "Open folder as vault"로 폴더 지정
- PLN 테마 추천 (Settings → Appearance → Themes → Manage)
- 그래프 뷰는 컨텍스트 커질 때 진가 발휘

---

## 5개 Skill 개요

### 1. **OS Setup Skill** — 초기 구축
- `/os-setup` 슬래시 커맨드로 실행
- 사용자에게 묻는 것:
  1. **Vault 타입**: solopreneur/professional vs business/team (초기 폴더 구조 결정)
  2. **12개 섹션 brain dump** (about you, company, market, ...) — WhisperFlow 같은 음성 받아쓰기로 비구조적으로 쏟아내기 권장. "맥주 한 팩 + 피자 시켜놓고 몇 시간 앉아서"
- 자동 생성 결과:
  - 폴더 구조: `context/`, `daily/`, `project/`, `intelligence/`, `resource/`, `skills/` (+ business: `department/`, `team/`, `onboarding/`)
  - **루트 CLAUDE.md** + 각 서브폴더별 CLAUDE.md (Karpathy 권장 패턴)
  - 12개 답변을 구조화해 폴더 배치
- **마인드셋**: 첫날 완벽 추구 X, 단순하게 시작. "30~40개 문서로 시작 → 6주 뒤 수백 → 지금 수천"

### 2. **OS Operator Skill** — 실시간 자동 업데이트
- `/os-operator` → scheduled task 자동 생성
- 동작:
  - connector(Fireflies, Slack, Circle, Calendar, Email, Analytics 등)에서 매일 최신 데이터 pull
  - `daily/` 폴더에 **일일 컨텍스트 브리프** 작성 (오늘 캘린더, 우선순위, 팀별 태스크, 미팅 transcript)
  - 관련 기존 파일 업데이트 (전략 변경 시 strategy doc 갱신), 비관련 파일 삭제 제안
  - 기본 hygiene: 중복 탐지·병합, 큰 파일 요약, wiki 링크 보강
  - 긴급 항목은 Slack DM으로 escalate 가능
- **제약**: scheduled task는 Claude Desktop + 노트북이 켜져 있어야 동작. 자율 실행하려면 **Routines + MCP** 필요(skill 5)

### 3. **OS Optimizer Skill** — 컨텍스트 bloat 방지
- 컨텍스트 커지면 → 토큰 낭비, AI 느려짐, 관련 없는 컨텍스트 retrieval, 중복/충돌
- 주기적으로 실행 권장: **주 1회 또는 격주**
- 적용 프레임워크들:
  - Anthropic best practices (architecture, CLAUDE.md, Dream framework, manage memory)
  - **Caveman compression method**
  - **Chroma context rot method**
  - **Karpathy N & M wiki**
- 결과 예시: 1,700 files audited → 34 problems found → 32 fixed → health score 46 → 94
- 주요 작업:
  - CLAUDE.md 및 index 파일을 routing 효율 + 토큰 사용 기준으로 최적화
  - 중복 병합, stale 컨텍스트·unreachable 파일 정리, 충돌 해결
  - 폴더 구조 재조직 제안
  - 깨진 wiki 링크, 포맷, 태그/front matter 보강
- **참고**: Anthropic이 곧 비슷한 "Dream feature" 출시 예정(현재는 managed agents 전용)

### 4. **Team OS Skill** — 팀 공유 + 권한
- 시도해본 옵션 비교:

| 방식 | 문제 |
|---|---|
| **GitHub repo** | 실시간 아님, 매번 수동 commit/push 필요 |
| **Cloud-based (Notion/GDrive)** | MCP 거쳐야 함 → 정확도↓ 속도↓ 토큰↑. 작은 setup엔 OK |
| **Obsidian Sync (native)** | 실시간 아님, 권한 설정 없음 |
| **Obsidian Relay 플러그인** ✅ | 실시간 멀티 폴더 sync. 단 read/write 권한 없음 |
| **Ben AI Relay (Relay 위에 권한 레이어)** ✅✅ | Relay 기반 + role-based 권한 (member/owner) |

- 절차: Relay 플러그인 설치 → `/team-os` 실행 → Relay 교체 → 폴더별 팀원 + role 매핑
- 추천: **본인 혼자 몇 주 써본 다음** 팀 롤아웃 (복잡도 증가)

### 5. **OS MCP Skill** — 완전 자율 운영
- 목적: Operator/Optimizer가 **Claude Routines**(클라우드 실행, 노트북 꺼져 있어도 동작, event-triggered 가능)에서 돌도록
- 핵심: 로컬 second brain을 **MCP 서버로 노출**
- 절차:
  1. Relay 설치 (server 역할)
  2. `/os-mcp` 실행 → Relay 계정 + API token 발급 → vault MCP URL 생성
  3. Customize → Connectors → Add custom → MCP URL 등록 → Relay 이메일/pw로 연결
  4. Cloud Code → Routines → Remote → 프롬프트 + schedule + 필요 connector(Fireflies, Calendar, brain MCP) 지정
- Event trigger 예: "Fireflies 미팅 끝날 때마다 transcript 처리"

---

## 우리 시스템(agentic OS)과의 비교 — 적용 가능 인사이트

| 영상 | 우리 agentic OS |
|---|---|
| `context/` | `brand-context/` + `context/` |
| `daily/` | `context/memory/YYYY-MM-DD.md` (Rule 9로 작업 단위 append) |
| `resource/` | (없음 — 추가 가능) |
| `skills/` | `skills/` (foundation/execution/strategy/ops) |
| `intelligence/` | (없음 — 미팅 transcript·경쟁사 리서치용으로 추가 가능) |
| 폴더별 CLAUDE.md | 루트 CLAUDE.md만 (서브 추가 검토) |
| OS Operator 자동 업데이트 | wrap-up이 수동 트리거. **scheduled task로 자동화 검토** |
| OS Optimizer hygiene | (없음 — Caveman compression / context rot 도입 검토) |
| OS MCP + Routines | (Claude Code on the web으로 부분 대체. Obsidian Vault는 별도 repo로 분리 중) |

## Key Takeaways
1. **시작은 단순하게, 완벽 X**. 30~40개 문서로 시작해도 6개월 뒤 수천 개로 자연 성장.
2. **폴더 + CLAUDE.md가 본질**. Obsidian은 시각화, MCP는 외부 노출용 - 데이터는 그냥 .md 파일.
3. **CLAUDE.md 최적화가 토큰 비용의 핵심**. 매 채팅마다 pull되므로 routing 효율이 곧 비용.
4. **컨텍스트 compound**: 일찍 시작할수록 6개월 후 AI 격차가 커짐.
5. **자율화 단계**: 수동 → scheduled task(노트북 열려있어야) → Routines + MCP(완전 자율).
6. **클라우드 기반(Notion/GDrive)보다 로컬 폴더가 정확/빠름/저렴** — MCP 레이어 비용 회피.
7. **팀 공유는 Relay + 권한 레이어**가 현재 최선. GitHub은 실시간 아님, Obsidian Sync는 권한 없음.
