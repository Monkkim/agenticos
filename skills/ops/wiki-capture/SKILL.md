# Wiki Capture

## 목적
대화 중 위키-worthy 지식을 감지하고 `/Users/user/Documents/Obsidian Vault/wiki/`에 직접 저장한다.

---

## 자동 트리거 기준

**저장한다 (다음 중 하나라도 해당):**
- 새 개념·원리를 설명했을 때
- 구체적인 방법·절차를 다뤘을 때
- 도구·서비스에 대한 인사이트가 나왔을 때
- 나중에 반복 참조할 것 같은 내용일 때

**저장하지 않는다:**
- 단순 확인·지시 ("ㄱㄱ", "파일 저장해줘", "ㅇㅇ")
- `wiki/_master-index.md`를 확인했을 때 이미 있는 내용
- 일시적 작업 맥락 (git 커밋, 파일 수정 지시 등)

---

## 수동 트리거

사용자가 "정리해줘" / "컴파일해줘" 라고 말하면:
1. `raw/` 폴더 내 .md 파일 전체 스캔
2. 토픽 미분류 파일 확인
3. 아래 실행 절차대로 wiki로 컴파일

---

## 실행 절차

### 1. 중복 확인
`/Users/user/Documents/Obsidian Vault/wiki/_master-index.md` 를 Read.
동일/유사 주제 항목이 있으면 기존 파일 업데이트. 없으면 새 파일 생성.

### 2. 토픽 결정
기존 폴더(`wiki/<topic>/`) 중 내용에 맞는 것이 있으면 사용.
없으면 lowercase-with-hyphens 형식으로 신규 폴더 생성.
예: `llm-concepts`, `youtube-strategy`, `business-insights`, `personal-development`

### 3. 파일 생성/업데이트

경로: `/Users/user/Documents/Obsidian Vault/wiki/<topic>/<slug>.md`
파일명: 내용 핵심어 기반 lowercase-with-hyphens

**파일 구조 (반드시 이 순서):**

```
> [!abstract]+ 📋 Knowledge Card
> **유형**: <Concept | How-To | Insight | Tool | Pattern | Reference>
> **도메인**: <토픽/서브토픽>
> **출처**: <대화 세션 · YYYY-MM-DD | 문서명 | URL>
> **요약**: <한 줄 핵심 요약>

# <제목>

## <핵심 내용 섹션>
- bullet point 중심
- [[wiki links]] 적극 활용

## Key Takeaways
- 3~5개 핵심 포인트
```

**유형 태그 기준:**
- `Concept` — 개념·이론·원리
- `How-To` — 방법·절차·튜토리얼
- `Insight` — 인사이트·판단·관찰
- `Tool` — 도구·서비스·라이브러리
- `Pattern` — 설계 패턴·원칙
- `Reference` — 참조 자료·치트시트

### 4. _index.md 업데이트
`/Users/user/Documents/Obsidian Vault/wiki/<topic>/_index.md` 에 새 파일 항목 추가.
파일이 없으면 아래 형식으로 생성:

```markdown
# <Topic> Index

## Articles
- [[<slug>|<제목>]] — <한줄 설명>
```

### 5. _master-index.md 업데이트
`/Users/user/Documents/Obsidian Vault/wiki/_master-index.md` 에 토픽 항목 추가 또는 신규 파일 항목 추가.
