# my-app-devlog-diary

## 프로젝트 소개

VS Code에서 Claude Code를 활용하여 **dev-log.md → PRD.md** 자동 생성 워크플로우를 구현한 프로젝트입니다.

매일 개발 로그를 작성하면, Claude Code가 이를 분석하여 PRD(제품 요구사항 문서)를 자동으로 생성/갱신합니다.

---

## 폴더 구조

```
my-app-devlog-diary/
├── docs/
│   ├── dev-log.md        # 매일 작성하는 개발 로그
│   ├── prd-template.md   # PRD 템플릿
│   └── PRD.md            # 자동 생성되는 PRD 문서
├── prompts/
│   └── prd_from_devlog.md # Claude Code 지시문 (PRD 생성 규칙)
├── CLAUDE.md              # 프로젝트 공통 규칙
└── README.md              # 이 파일
```

---

## 사용 방법

### Step 1. 개발 로그 작성

`docs/dev-log.md` 파일을 열고, 아래 형식으로 오늘의 기록을 추가합니다:

```markdown
## 2025-12-24
### ✔ 오늘 한 일
- 완료한 작업 1
- 완료한 작업 2

### ❌ 문제 / 막힌 점
- 겪은 문제나 어려움

### 💡 결정 (왜 이렇게 했는지)
- 내린 결정과 그 이유

### 🔜 다음 할 일
- 내일 할 작업
```

> 5줄 정도면 충분합니다. 매일 조금씩 기록하는 것이 중요합니다.

---

### Step 2. PRD 자동 생성 (앱 실행)

VS Code에서 Claude Code를 열고 다음 중 하나를 입력합니다:

#### 방법 A: 간단한 명령
```
앱을 실행해
```

#### 방법 B: 상세한 명령
```
docs/dev-log.md와 docs/prd-template.md를 읽고 prompts/prd_from_devlog.md 규칙대로 docs/PRD.md를 만들어줘. 파일로 저장까지 해줘.
```

#### 방법 C: 단일 작업 지시문 (전체 기능)
아래 프롬프트를 Claude Code에 붙여넣으면, PRD 갱신 + 변경 요약 + 커밋 메시지 추천까지 한 번에 실행됩니다:

```
[목표]
- docs/dev-log.md와 docs/prd-template.md를 기반으로 docs/PRD.md를 최신 상태로 생성/갱신한다.
- 변경 사항을 요약하고, 적절한 git commit 메시지(1개)를 추천한다.
- 파일 편집은 최소한으로, 결과는 재현 가능하게 만든다.

[작업 순서]
1) 다음 파일을 읽어라:
   - docs/dev-log.md
   - docs/prd-template.md
   - prompts/prd_from_devlog.md (규칙/매핑)
   - (있으면) docs/PRD.md (기존 PRD가 있으면 비교용)

2) prompts/prd_from_devlog.md의 규칙을 반드시 준수해서,
   - docs/prd-template.md의 {{...}} 빈칸을 채운 "완성형 PRD 전체 내용"을 작성하라.
   - 결과물을 docs/PRD.md에 저장하라.
   - 작성 언어: 한국어
   - 파일명/기능명/필드명은 원문 그대로 유지
   - 정보가 부족한 섹션은 "정보 부족"이라고 명시하고,
     dev-log에 다음에 무엇을 기록하면 좋을지 1줄 제안(구체적으로) 추가

3) docs/PRD.md를 저장한 뒤, 변경점을 요약하라:
   - "이번 업데이트로 PRD에 새로 반영된 내용 3~7개"를 불릿으로 정리
   - "정보 부족으로 남은 섹션"을 불릿으로 정리
   - "내일 dev-log에 적으면 좋은 항목 3개" 제안

4) 마지막으로 git 커밋 메시지 1개를 추천하라:
   - 형식: docs: ...
   - 예: docs: update PRD from dev-log (2025-12-24)

[주의]
- dev-log의 기존 기록은 절대 삭제/변형하지 마라.
- PRD는 '읽는 사람이 바로 실행할 수 있는' 수준으로 구체적으로 쓰되,
  어려운 기술 용어는 최대한 피하고 필요하면 쉬운 설명을 괄호로 붙여라.
```

---

### Step 3. GitHub에 푸시

터미널에서 아래 명령어를 실행합니다:

```bash
git add .
git commit -m "docs: update PRD from dev-log (2025-12-24)"
git push
```

---

## 매일 운영 루틴 (3단계)

| 순서 | 작업 | 소요 시간 |
|------|------|----------|
| 1 | `docs/dev-log.md`에 오늘 기록 추가 | 2분 |
| 2 | Claude Code에 "앱을 실행해" 입력 | 1분 |
| 3 | `git add . && git commit && git push` | 1분 |

---

## PRD 자동 매핑 규칙

dev-log의 각 섹션이 PRD의 어느 부분으로 변환되는지:

| dev-log 섹션 | PRD 섹션 |
|-------------|---------|
| ❌ 문제 / 막힌 점 | 1) 문제 정의 (Why) |
| 💡 결정 | 5) 데이터/연동 구조, 6) 범위 |
| ✔ 오늘 한 일 | 4) 핵심 기능, 7) 현재 상태 |
| 🔜 다음 할 일 | 6) 다음 단계, 7) 다음 할 일 |

> 정보가 부족한 섹션은 "정보 부족"으로 표시되고, 다음에 기록할 내용을 제안합니다.

---

## 파일별 역할

| 파일 | 역할 |
|-----|------|
| `docs/dev-log.md` | 매일 개발 진행 상황을 기록 (입력) |
| `docs/prd-template.md` | PRD 문서의 구조/틀 정의 |
| `prompts/prd_from_devlog.md` | Claude Code가 따를 PRD 생성 규칙 |
| `docs/PRD.md` | 자동 생성되는 PRD 문서 (출력) |
| `CLAUDE.md` | Claude Code 프로젝트 공통 설정 |

---

## 자주 묻는 질문 (FAQ)

### Q: dev-log에 뭘 써야 할지 모르겠어요
A: 아래 질문에 답하듯 적어보세요:
- 오늘 뭘 했나요? (✔ 오늘 한 일)
- 막힌 게 있었나요? (❌ 문제)
- 왜 그렇게 결정했나요? (💡 결정)
- 내일은 뭘 할 건가요? (🔜 다음 할 일)

### Q: PRD에 "정보 부족"이 많아요
A: 정상입니다! 개발 초기에는 정보가 부족한 게 당연합니다. 매일 로그를 쌓으면 PRD가 점점 완성됩니다.

### Q: 이전 dev-log 기록이 사라지나요?
A: 아니요. dev-log의 기존 기록은 절대 삭제되지 않습니다. 새 기록을 추가하기만 하면 됩니다.

---

## 키워드

#GitHub #DevLog #PRD #VSCode #ClaudeCode #자동화
