---
name: skill-creator
description: Use this skill BEFORE creating any new skill in the Agentic OS. Triggered when the user says "새 skill 만들어줘", "skill 추가", or when any agent is about to write a new SKILL.md. Scans all existing skill frontmatter for overlap/duplication, identifies dependencies on brand-context and context files, applies the standard SKILL.md template, and registers the new skill in CLAUDE.md and learnings.md.
---

# skill-creator

**새 skill을 만들기 전에 반드시 호출**되는 가드레일 skill. 중복 방지, 의존성 정리, 포맷 통일을 담당한다.

## 언제 실행되는가

- 사용자가 "새 skill 만들어줘: X" / "X에 대한 skill 추가" 발화
- 다른 skill이 내부에서 새 skill을 생성하려 할 때
- `oh-my-claudecode` 에이전트가 skill 생성을 제안할 때

## 실행 절차

### 1단계 — 의도 명확화

사용자에게 먼저 AskUserQuestion으로 확인:

1. **새 skill 이름(kebab-case)?**
2. **어느 카테고리?** (foundation / execution / strategy / creative / ops)
3. **무슨 일을 하는가?** (한 문장 — SKILL.md description 시드가 됨)
4. **입력/출력은?** (어떤 파일을 읽고, 어떤 파일을 쓰는가)
5. **언제 자동 발화되면 좋은가?** (사용자 발화 키워드 예시)

### 2단계 — 중복/의존성 스캔

모든 기존 skill의 frontmatter를 읽는다:

```
Glob: skills/**/SKILL.md
Read: 각 파일의 --- 프론트매터 블록
```

다음을 체크:
- **이름 중복** — 같은 name이 있으면 실패
- **description 유사도** — 새 skill의 의도와 키워드 겹치는 기존 skill 찾기
- **입출력 파일 충돌** — 같은 `brand-context/*.md`를 쓰려는 skill이 있는지

유사 skill이 있으면 사용자에게 보여주고:
- "기존 skill을 확장할지, 정말 새로 만들지 선택해주세요."

### 3단계 — 위치 확정

`skills/<category>/<name>/SKILL.md` 경로 확정.

기존에 같은 카테고리에 유사 skill이 몰려 있으면, 필요 시 서브카테고리 제안 (예: `execution/writing/copywriting`).

### 4단계 — 템플릿 적용

아래 표준 템플릿으로 `SKILL.md` 초안 작성:

```markdown
---
name: <skill-name>
description: Use this skill when <trigger>. <What it does>. <What it outputs>.
---

# <skill-name>

<한 문장 목적>

## 언제 실행되는가
- <트리거 조건 1>
- <트리거 조건 2>

## 실행 절차

### 0단계 — 과거 학습 확인
`context/learnings.md`의 `<category>/<name>` 섹션 Read.

### 1단계 — 입력 수집
...

### N단계 — 산출물 생성
...

### 마지막 단계 — 검증 & 로깅
...

## 입력
- <파일/사용자 발화>

## 출력
- <생성/수정되는 파일>

## 의존성
- `brand-context/voice.md` (또는 필요한 파일)
- `context/learnings.md`

## 중요 원칙
- ...
```

### 5단계 — 보조 파일 생성

필요 시:
- `skills/<category>/<name>/references/` — skill이 참조할 서브 지식
- `skills/<category>/<name>/templates/` — 산출물 템플릿

### 6단계 — 시스템 등록

1. `context/learnings.md`에 새 섹션 `## <category>/<name>` 추가 (비어 있는 상태)
2. `CLAUDE.md`의 "프로젝트 지도" 섹션 업데이트 (해당 카테고리에 새 skill 언급)
3. 변경사항 diff를 사용자에게 보여주고 승인 받기

### 7단계 — 사용자 검토 안내

```
✓ 새 skill 생성: skills/<category>/<name>/SKILL.md
  - description: <한 줄>
  - 의존 파일: <목록>
다음 단계: SKILL.md를 열어 실행 절차를 사용자 상황에 맞게 구체화하세요.
```

## 거부 조건

다음 경우 생성을 거부하고 사용자에게 알린다:
- 같은 이름 skill이 이미 존재
- description에 "general purpose" / "do everything" 같은 모호 표현
- 카테고리가 지정되지 않음
- brand-context를 안 읽겠다고 명시한 경우 (Agentic OS 원칙 위배)

## 과거 학습 읽기

`context/learnings.md`의 `ops/skill-creator` 섹션 Read 후 실행.

## 출력 파일

- 새로운 `skills/<category>/<name>/SKILL.md`
- 업데이트된 `context/learnings.md`
- 업데이트된 `CLAUDE.md` (사용자 승인 시)
