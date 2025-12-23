# my-app-devlog-diary

## 프로젝트 소개

VS Code에서 Claude Code를 활용하여 **dev-log.md → PRD.md** 자동 생성 워크플로우를 구현한 프로젝트입니다.

## 폴더 구조

```
my-app-devlog-diary/
├── docs/
│   ├── dev-log.md        # 매일 작성하는 개발 로그
│   ├── prd-template.md   # PRD 템플릿
│   └── PRD.md            # 자동 생성되는 PRD 문서
├── prompts/
│   └── prd_from_devlog.md # Claude Code 지시문
├── CLAUDE.md              # 프로젝트 공통 규칙
└── README.md
```

## 사용 방법

### 1. 매일 개발 로그 작성
`docs/dev-log.md`에 아래 형식으로 기록합니다:
- ✔ 오늘 한 일
- ❌ 문제 / 막힌 점
- 💡 결정 (왜 이렇게 했는지)
- 🔜 다음 할 일

### 2. PRD 자동 생성
Claude Code에 다음과 같이 요청합니다:
> `docs/dev-log.md`와 `docs/prd-template.md`를 읽고 `prompts/prd_from_devlog.md` 규칙대로 `docs/PRD.md`를 만들어줘.

### 3. GitHub에 푸시
```bash
git add .
git commit -m "docs: update PRD (날짜)"
git push
```

## 매일 운영 루틴

1. `docs/dev-log.md`에 오늘 기록 (5줄이면 충분)
2. Claude Code에게 "PRD 갱신" 요청
3. `git add . && git commit -m "docs: update PRD" && git push`

## 키워드

#GitHub #DevLog #PRD #VSCode #ClaudeCode #자동화
