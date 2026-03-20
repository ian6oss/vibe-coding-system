# VCS — Vibe Coding System

바이브코딩의 3대 문제를 해결하는 AI 코딩 어시스턴트 시스템.
Claude Code와 OpenAI Codex 모두 지원합니다.

## 이게 뭔가요?

AI에게 "쇼핑몰 만들어줘"라고 하면, AI가 바로 코딩을 시작합니다.
그런데 결과물이 엉망인 이유는 **코딩 실력이 아니라 기획이 없어서**입니다.

VCS는 이 문제를 해결합니다:

| 문제 | 원인 | VCS가 하는 일 |
|------|------|------|
| 모호함 | "만들어줘" 한마디로 시작 | 기획팀이 질문으로 디테일을 끝까지 파고듦 |
| 품질 부재 | 한번에 다 만들고 기도하기 | 작은 단위로 나눠서 매번 검수 |
| 유지보수 불가 | 뭘 어떻게 만들었는지 모름 | 모든 과정을 문서로 기록 |

## 빠른 시작

### OpenAI Codex (GPT) 사용자

**필요한 것:**
- Node.js 18+ 설치 ([nodejs.org](https://nodejs.org))
- OpenAI API 키 또는 ChatGPT Pro 구독

```bash
# 1. Codex CLI 설치 (처음 한 번만)
npm install -g @openai/codex

# 2. API 키 설정 (처음 한 번만)
export OPENAI_API_KEY="sk-..."

# 3. 프로젝트 폴더 만들기
mkdir my-project && cd my-project

# 4. VCS 설치 (이것만 하면 됨!)
git clone https://github.com/ian6oss/vibe-coding-system.git
cp vibe-coding-system/codex/AGENTS.md .
rm -rf vibe-coding-system

# 5. 실행
codex
```

Codex가 열리면:
> "vcs 시작, 쇼핑몰 만들어줘"

### Claude Code 사용자

**필요한 것:**
- Node.js 18+ 설치 ([nodejs.org](https://nodejs.org))
- Anthropic API 키 또는 Claude Pro/Max 구독

```bash
# 1. Claude Code 설치 (처음 한 번만)
npm install -g @anthropic-ai/claude-code

# 2. 프로젝트 폴더 만들기
mkdir my-project && cd my-project

# 3. VCS 설치
git clone https://github.com/ian6oss/vibe-coding-system.git
mkdir -p .claude/skills
cp -r vibe-coding-system/vibe-* .claude/skills/
rm -rf vibe-coding-system

# 4. 실행
claude
```

Claude Code가 열리면:
> "vcs 시작, 블로그 만들어줘"

---

## 사용법

### 기본 명령어

| 이렇게 말하면 | 이렇게 됩니다 |
|---|---|
| "vcs 시작" | 전체 파이프라인 시작 (모드 선택) |
| "~만들어줘" | 기획부터 시작 |
| "검수해줘" / "검토해줘" | 검수팀이 기획서 검토 |
| "개발 시작해줘" | 개발팀이 코딩 시작 |
| "문서화해줘" | 프로젝트 문서 자동 생성 |
| "vcs 이어가기" | 이전에 하던 작업 이어가기 |
| "어디까지 했어?" | 현재 진행 상태 확인 |

### 모드 선택

처음 시작할 때 프로젝트 규모에 맞는 모드를 고릅니다:

| 모드 | 소요 시간 | 추천 상황 |
|------|------|------|
| **정식** | 기획 30분+ | 쇼핑몰, SaaS 등 큰 프로젝트 |
| **속성** | 기획 10분 | 블로그, 포트폴리오 등 중간 프로젝트 |
| **초속성** | 기획 3분 | 랜딩페이지, 간단한 도구 |

### 전체 흐름

```
"vcs 시작, 쇼핑몰 만들어줘"
         |
    [1. 기획] AI가 질문을 던짐
         |    "어떤 상품? 결제는? 회원가입은?"
         |    -> 기획서 자동 생성
         |
    [2. 검수] 독립 검수팀이 기획서 검토
         |    "보안이 빠졌어요", "모바일 대응이 없어요"
         |    -> 검수 리포트 + 수정
         |
    [3. 개발] 작은 단위로 나눠서 개발
         |    블록1: 레이아웃 -> 블록2: 로그인 -> 블록3: 상품 목록 -> ...
         |    매 블록마다 검수 + 미리보기
         |
    [4. 문서화] 자동으로 문서 생성
              -> 프로젝트 레시피 (다음에 재사용 가능)
              -> 코드 맵 (어떤 파일이 뭘 하는지)
              -> 유지보수 가이드 (실행/배포 방법)
```

### 이전 대화 이어가기

작업 중간에 대화가 끊겨도 걱정 없습니다.
다음에 다시 시작할 때:

> "vcs 이어가기"

그러면 AI가 마지막 상태를 불러와서 이어서 진행합니다.

---

## 시스템 구조

```
vibe-coding-system/
|
|-- codex/
|   |-- AGENTS.md              <- Codex(GPT)용 (이 파일 하나로 끝)
|
|-- vibe-planner/              <- 기획 에이전트팀
|   |-- SKILL.md
|   |-- references/
|       |-- question-bank.md     도메인별 질문 은행
|       |-- roles.md             기획팀장/기획자/마케터 역할
|       |-- spec-template.md     기획서 템플릿
|
|-- vibe-reviewer/             <- 검수 에이전트팀 (기획팀과 독립)
|   |-- SKILL.md
|   |-- references/
|       |-- review-dimensions.md 검수 기준
|       |-- common-gaps.md       자주 빠지는 항목
|
|-- vibe-dev/                  <- 개발 에이전트팀
|   |-- SKILL.md
|   |-- references/
|       |-- review-checklist.md  코드 리뷰 체크리스트
|       |-- tech-stacks.md       기술 스택 가이드
|       |-- error-patterns.md    에러 패턴 사전
|
|-- vibe-docs/                 <- 문서화 시스템
|   |-- SKILL.md
|
|-- vibe-orchestrator/         <- 전체 흐름 제어
|   |-- SKILL.md
|
|-- ARCHITECTURE.md            <- 전체 설계도
|-- README.md                  <- 이 파일
```

### Claude Code vs Codex 차이

| | Claude Code | Codex (GPT) |
|---|---|---|
| 설치 파일 | `.claude/skills/` 폴더 (5개 스킬) | `AGENTS.md` 파일 1개 |
| AI 모델 | Claude (Anthropic) | GPT (OpenAI) |
| 구독 | Anthropic API 또는 Claude Pro/Max | OpenAI API 또는 ChatGPT Pro |
| 기능 | 동일 | 동일 |

---

## 핵심 설계: 왜 기획팀과 검수팀이 따로?

```
+-----------+          +-----------+
|  기획팀    |          |  검수팀    |
| (planner) |          | (reviewer)|
|           |          |           |
| 만드는 데  |          | 부수는 데  |
| 집중       |          | 집중       |
+-----+-----+          +-----+-----+
      |                       |
      +----------+------------+
                 |
          사용자에게 동시 제시
                 |
            사용자가 결정
```

자기가 만든 것의 빈 곳은 자기가 못 봅니다.
그래서 만드는 팀과 검수하는 팀을 분리했습니다.
의견이 다르면 사용자가 최종 판단합니다.

---

## FAQ

### Q: 코딩을 전혀 몰라도 쓸 수 있나요?
**A:** 네. VCS의 핵심이 바로 그겁니다. "쇼핑몰 만들어줘"라고만 하면 AI가 질문을 던지고, 답변하면 기획서를 만들고, 검수하고, 개발하고, 문서까지 만들어줍니다.

### Q: 무료인가요?
**A:** VCS 자체는 무료(MIT 라이선스)입니다. 단, AI 도구(Claude Code 또는 Codex) 사용료는 별도입니다.

### Q: Claude Code와 Codex 중 뭘 써야 하나요?
**A:** 둘 다 됩니다. 이미 쓰고 있는 걸로 하세요. 처음이면 Codex(ChatGPT Pro 구독이 있으면)가 시작하기 쉽습니다.

### Q: 이전에 하던 작업을 이어갈 수 있나요?
**A:** 네. "vcs 이어가기"라고 하면 마지막 상태를 불러와서 이어서 진행합니다.

### Q: 기획 없이 바로 코딩할 수 있나요?
**A:** 네. "초속성 모드"를 선택하면 질문 3개만 하고 바로 개발합니다. 또는 개별 스킬(vibe-dev)을 직접 호출할 수도 있습니다.

### Q: 어떤 종류의 프로젝트를 만들 수 있나요?
**A:** 웹사이트, 웹앱, 쇼핑몰, SaaS, 블로그, 포트폴리오, 모바일 앱 등 거의 모든 소프트웨어 프로젝트에 사용할 수 있습니다.

---

## 업데이트

최신 버전을 받으려면:

```bash
cd vibe-coding-system
git pull
```

그리고 다시 프로젝트에 복사하면 됩니다.

## 라이선스

MIT — 누구나 자유롭게 사용, 수정, 배포할 수 있습니다.
