# 🎵 VCS — Vibe Coding System

바이브코딩의 3대 문제를 해결하는 Claude Code 스킬 시스템

## 문제 → 해결

| 문제 | 원인 | 해결 |
|------|------|------|
| 모호함 | "만들어줘" 한마디로 시작 | **vibe-planner** — 기획 에이전트팀이 디테일을 끝까지 파고듦 |
| 품질 부재 | 한번에 다 만들고 기도하기 | **vibe-dev** — 빌드 블록 단위로 나눠서 매번 검수 |
| 유지보수 불가 | 뭘 어떻게 만들었는지 모름 | **vibe-docs** — 모든 과정을 기록하고 재사용 가능한 문서화 |

## 시스템 구조

```
vibe-coding-system/
├── ARCHITECTURE.md          ← 전체 설계도
├── vibe-planner/            ← 기획 에이전트팀 (독립 팀 1)
│   ├── SKILL.md
│   └── references/
│       ├── question-bank.md   도메인별 질문 은행
│       ├── roles.md           기획팀장/기획자/마케터 역할
│       └── spec-template.md   기획서 템플릿
├── vibe-reviewer/           ← 검수 에이전트팀 (독립 팀 2)
│   ├── SKILL.md
│   └── references/
│       ├── review-dimensions.md  검수 차원별 기준
│       └── common-gaps.md        자주 빠지는 항목
├── vibe-dev/                ← 개발 에이전트팀
│   ├── SKILL.md
│   └── references/
│       ├── review-checklist.md   코드 리뷰 체크리스트
│       ├── tech-stacks.md        기술 스택 가이드
│       └── error-patterns.md     에러 패턴 사전
├── vibe-docs/               ← 문서화/유지보수 시스템
│   └── SKILL.md
└── vibe-orchestrator/       ← 통합 오케스트레이터
    └── SKILL.md
```

## 워크플로우

```
사용자: "~만들어줘"
         ↓
[vibe-planner] 기획팀 가동
  ├── 기획팀장: 프로젝트 범위 설정
  ├── 기획자: 세부 기능 질문 수집
  └── 마케터: 시장 조사 (선택)
         ↓
  → 기획서 초안 출력
         ↓
[vibe-reviewer] 검수팀 가동 (독립)
  ├── 기술 검수관
  ├── UX 검수관
  └── 비즈니스 검수관
         ↓
  → 기획서 + 검수 리포트 동시 제시
         ↓
  사용자 승인
         ↓
[vibe-dev] 개발팀 가동
  ├── 빌드 블록 분해
  ├── 블록별 개발 + 중간 검수
  └── 에러 분석 + 수정
         ↓
[vibe-docs] 문서화
  ├── 프로젝트 레시피
  ├── 코드 맵
  └── 유지보수 가이드
```

## 설치 방법

### Claude Code

```bash
git clone https://github.com/ian6oss/vibe-coding-system.git

# 프로젝트의 .claude/skills/ 에 복사
mkdir -p your-project/.claude/skills
cp -r vibe-coding-system/vibe-* your-project/.claude/skills/
```

### OpenAI Codex (GPT)

```bash
git clone https://github.com/ian6oss/vibe-coding-system.git

# AGENTS.md를 프로젝트 루트에 복사
cp vibe-coding-system/codex/AGENTS.md your-project/AGENTS.md
```

## 사용법

자연어로 사용 (Claude Code / Codex 공통):

```
"vcs 시작"               → 전체 파이프라인 시작
"쇼핑몰 만들어줘"        → 기획부터 시작
"이 기획서 검토해줘"     → 검수팀 가동
"개발 시작해줘"          → 개발팀 가동
"문서화해줘"             → 문서화 시스템 가동
"vcs 이어가기"           → 이전 세션 이어가기
"어디까지 했어?"         → 현재 상태 확인
```

### 모드 선택

| 모드 | 설명 | 용도 |
|------|------|------|
| 정식 | 기획→검수→개발→문서화 풀 | 대규모 프로젝트 |
| 속성 | 핵심만 빠르게 | 중규모 프로젝트 |
| 초속성 | 3문답 후 바로 개발 | 간단한 프로젝트 |

## 핵심 설계: 기획팀 ↔ 검수팀 독립 구조

```
┌─────────────┐        ┌─────────────┐
│   기획팀      │        │   검수팀      │
│ (planner)    │        │ (reviewer)   │
│              │        │              │
│  만드는 데    │        │  부수는 데    │
│  집중         │        │  집중         │
└──────┬───────┘        └──────┬───────┘
       │                        │
       └───────┬────────────────┘
               ↓
         사용자에게 동시 제시
               ↓
          사용자 결정
```

- 자기가 만든 것의 빈 곳은 자기가 못 본다 (확인 편향 방지)
- 의견 충돌 시 사용자가 최종 판단 (투명한 의사결정)

## 라이선스

MIT
