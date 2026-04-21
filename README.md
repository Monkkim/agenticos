# Agentic OS

Claude Code 기반 개인 브랜드/비즈니스 자동화 운영 체제.

> 영상 "How Smart People Are Using Claude Code Skills to Automate Anything"의 아키텍처를 한국어 환경에 맞게 재구성한 로컬 구현체.

## 구성

- **brand-context/** — 브랜드의 정체성 (voice / ICP / positioning / samples / assets)
- **context/** — 에이전트의 기억 (정체성, 사용자 선호, 장·단기 메모리, 학습 로그)
- **skills/** — 분류별 skill 모음 (foundation / execution / strategy / creative / ops)
- **projects/** — skill이 만들어낸 산출물 저장소

## 빠른 시작

```bash
cd "~/My Workspace/AgenticOS"
claude   # Claude Code 실행
```

프롬프트에서:

```
start here
```

→ `skills/foundation/start-here`가 브랜드 인터뷰를 진행하고 `brand-context/`를 채워준다.

## 일상 워크플로우

| 상황 | 하는 말 |
|---|---|
| 브랜드 정보 업데이트 | "브랜드 컨텍스트 갱신" → foundation 재실행 |
| 새 콘텐츠 제작 | "X에 대한 [뉴스레터/스크립트/카피] 써줘" → execution skill 자동 선택 |
| 세션 종료 | "세션 종료" / "wrap up" → wrap-up이 피드백 수집 + 커밋 |
| 새 skill 추가 | "새 skill 만들어줘: X" → skill-creator가 중복 체크 후 생성 |

## 원칙

1. **격리** — `~/.claude/`를 건드리지 않는다. 이 폴더가 Agentic OS의 전부다.
2. **공유 컨텍스트** — 모든 skill이 같은 `brand-context/`를 읽는다.
3. **학습 루프** — 세션 끝의 피드백이 skill을 점진적으로 개선한다.
4. **YAGNI** — 필요한 skill만 그때그때 추가한다.

## 상세 설계 문서

`/Users/user/.claude/plans/how-smart-people-are-purring-pretzel.md`
