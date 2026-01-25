# Ralph - 자율 AI 에이전트 루프 (PRD 완료까지)

> **URL**: https://github.com/snarktank/ralph  
> **유형**: GitHub 레포지토리 (도구/스크립트)  
> **작성자**: Ryan Carson (snarktank)  
> **라이선스**: MIT  
> **Stars**: 7k ⭐ | **Forks**: 835

---

## 요약

**PRD(제품 요구사항 문서)의 모든 항목이 완료될 때까지 AI 코딩 도구를 반복 실행**하는 자율 에이전트 루프. Geoffrey Huntley의 Ralph 패턴을 실제 구현한 도구로, Amp 또는 Claude Code CLI를 지원한다. 매 반복마다 새로운 AI 인스턴스가 클린 컨텍스트로 시작하며, Git 히스토리, `progress.txt`, `prd.json`을 통해 메모리가 유지된다.

> "Each iteration is a fresh instance with clean context."

---

## 핵심 개념

```
┌─────────────────────────────────────────────────────────────┐
│  PRD 기반 자율 실행                                          │
│     prd.json의 모든 스토리가 passes: true 될 때까지 반복      │
├─────────────────────────────────────────────────────────────┤
│  매 반복 = 새로운 컨텍스트                                   │
│     클린 AI 인스턴스 스폰 → 컨텍스트 오염 방지               │
├─────────────────────────────────────────────────────────────┤
│  메모리 유지 채널                                            │
│     Git History + progress.txt + prd.json                   │
├─────────────────────────────────────────────────────────────┤
│  피드백 루프 필수                                            │
│     Typecheck + Tests + CI → 품질 보장                      │
└─────────────────────────────────────────────────────────────┘
```

---

## 워크플로우

```
┌─────────────────────────────────────────────────────────────┐
│  1. PRD 생성                                                │
│     "Load the prd skill and create a PRD for [feature]"     │
│     → tasks/prd-[feature-name].md                           │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  2. PRD → JSON 변환                                         │
│     "Load the ralph skill and convert prd.md to prd.json"   │
│     → prd.json (user stories 구조화)                        │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  3. Ralph 실행                                              │
│     ./scripts/ralph/ralph.sh [max_iterations]               │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  4. Ralph 루프 (자동 반복)                                   │
│     ┌───────────────────────────────────────────────────┐   │
│     │ a. Feature branch 생성                            │   │
│     │ b. passes: false인 최우선 스토리 선택             │   │
│     │ c. 해당 스토리 구현                               │   │
│     │ d. 품질 체크 (typecheck, tests)                   │   │
│     │ e. 체크 통과 시 커밋                              │   │
│     │ f. prd.json 업데이트 (passes: true)               │   │
│     │ g. progress.txt에 학습 내용 추가                  │   │
│     │ h. 모든 스토리 완료 또는 max 도달까지 반복        │   │
│     └───────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## 요구사항

- **AI 코딩 도구** (둘 중 하나):
  - Amp CLI (기본)
  - Claude Code (`npm install -g @anthropic-ai/claude-code`)
- **jq**: `brew install jq` (macOS)
- **Git 레포지토리**: 프로젝트가 git으로 초기화되어 있어야 함

---

## 설치

### 옵션 1: 프로젝트에 복사

```bash
# 프로젝트 루트에서
mkdir -p scripts/ralph
cp /path/to/ralph/ralph.sh scripts/ralph/

# AI 도구에 맞는 프롬프트 템플릿 복사:
cp /path/to/ralph/prompt.md scripts/ralph/prompt.md    # Amp용
# 또는
cp /path/to/ralph/CLAUDE.md scripts/ralph/CLAUDE.md    # Claude Code용

chmod +x scripts/ralph/ralph.sh
```

### 옵션 2: 글로벌 스킬 설치

```bash
# Amp용
cp -r skills/prd ~/.config/amp/skills/
cp -r skills/ralph ~/.config/amp/skills/

# Claude Code용
cp -r skills/prd ~/.claude/skills/
cp -r skills/ralph ~/.claude/skills/
```

---

## 실행

```bash
# Amp 사용 (기본)
./scripts/ralph/ralph.sh [max_iterations]

# Claude Code 사용
./scripts/ralph/ralph.sh --tool claude [max_iterations]
```

기본값: 10회 반복

---

## 핵심 파일

| 파일 | 용도 |
|------|------|
| `ralph.sh` | AI 인스턴스를 스폰하는 Bash 루프 |
| `prompt.md` | Amp용 프롬프트 템플릿 |
| `CLAUDE.md` | Claude Code용 프롬프트 템플릿 |
| `prd.json` | 유저 스토리 + passes 상태 (태스크 목록) |
| `prd.json.example` | PRD 포맷 예제 |
| `progress.txt` | 향후 반복을 위한 학습 내용 (append-only) |
| `skills/prd/` | PRD 생성 스킬 |
| `skills/ralph/` | PRD → JSON 변환 스킬 |
| `AGENTS.md` | AI가 자동으로 읽는 학습 내용 |

---

## prd.json 구조

```json
{
  "projectName": "my-feature",
  "branchName": "feature/my-feature",
  "userStories": [
    {
      "id": "US-001",
      "title": "Add user login form",
      "acceptanceCriteria": [
        "Form has email and password fields",
        "Validation on submit"
      ],
      "passes": false
    },
    {
      "id": "US-002",
      "title": "Add authentication API",
      "acceptanceCriteria": [...],
      "passes": true
    }
  ]
}
```

---

## 중요 원칙

### 1️⃣ 매 반복 = 새로운 컨텍스트

```
반복 1: [새 AI 인스턴스] → 스토리 A 구현 → 커밋 → 종료
        ↓
반복 2: [새 AI 인스턴스] → Git 히스토리 읽기 → 스토리 B 구현
        ↓
반복 N: [새 AI 인스턴스] → 모든 스토리 완료 확인 → 종료
```

메모리는 오직:
- **Git history**: 이전 반복의 커밋들
- **progress.txt**: 학습 내용 및 컨텍스트
- **prd.json**: 어떤 스토리가 완료되었는지

### 2️⃣ 작은 태스크 필수

```
✅ 올바른 크기의 스토리:
- DB 컬럼 및 마이그레이션 추가
- 기존 페이지에 UI 컴포넌트 추가
- 서버 액션에 새 로직 업데이트
- 목록에 필터 드롭다운 추가

❌ 너무 큰 스토리 (분할 필요):
- "전체 대시보드 구축"
- "인증 추가"
- "API 리팩토링"
```

> 태스크가 너무 크면 LLM이 컨텍스트를 다 쓰기 전에 끝내지 못하고 품질 저하

### 3️⃣ AGENTS.md 업데이트 중요

```markdown
# AGENTS.md 추가 예시

## 발견된 패턴
- 이 코드베이스는 X를 Y에 사용함

## 주의사항
- W를 변경할 때 Z 업데이트 잊지 말 것

## 유용한 컨텍스트
- 설정 패널은 X 컴포넌트에 있음
```

AI 코딩 도구가 자동으로 `AGENTS.md`를 읽으므로, 미래 반복과 개발자 모두 혜택

### 4️⃣ 피드백 루프 필수

```
┌─────────────────────────────────────────────────────────────┐
│  Typecheck    →  타입 에러 검출                             │
│  Tests        →  동작 검증                                  │
│  CI           →  그린 상태 유지 필수 (깨진 코드 누적 방지)   │
└─────────────────────────────────────────────────────────────┘
```

### 5️⃣ UI 스토리 브라우저 검증

프론트엔드 스토리는 반드시 acceptance criteria에 포함:

```
"Verify in browser using dev-browser skill"
```

Ralph가 dev-browser 스킬로 페이지 이동, UI 상호작용, 변경 확인

### 6️⃣ 종료 조건

모든 스토리가 `passes: true`가 되면:

```
<promise>COMPLETE</promise>
```

출력 후 루프 종료

---

## 플로우차트

```
        ┌─────────────┐
        │  ralph.sh   │
        │   시작      │
        └──────┬──────┘
               │
               ▼
        ┌─────────────┐
        │ Feature     │
        │ Branch 생성 │
        └──────┬──────┘
               │
               ▼
    ┌──────────────────────┐
    │ passes:false 스토리  │◄────────────────┐
    │ 선택                 │                 │
    └──────────┬───────────┘                 │
               │                             │
               ▼                             │
    ┌──────────────────────┐                 │
    │ 새 AI 인스턴스 스폰  │                 │
    │ (클린 컨텍스트)      │                 │
    └──────────┬───────────┘                 │
               │                             │
               ▼                             │
    ┌──────────────────────┐                 │
    │ 스토리 구현          │                 │
    └──────────┬───────────┘                 │
               │                             │
               ▼                             │
    ┌──────────────────────┐    실패         │
    │ 품질 체크            │─────────────────┤
    │ (typecheck, tests)   │                 │
    └──────────┬───────────┘                 │
               │ 통과                        │
               ▼                             │
    ┌──────────────────────┐                 │
    │ 커밋 + prd.json      │                 │
    │ passes:true 업데이트 │                 │
    └──────────┬───────────┘                 │
               │                             │
               ▼                             │
    ┌──────────────────────┐    아니오       │
    │ 모든 스토리 완료?    │─────────────────┘
    └──────────┬───────────┘
               │ 예
               ▼
        ┌─────────────┐
        │  COMPLETE   │
        │   종료      │
        └─────────────┘
```

---

## 디버깅

```bash
# 어떤 스토리가 완료되었는지 확인
cat prd.json | jq '.userStories[] | {id, title, passes}'

# 이전 반복의 학습 내용 확인
cat progress.txt

# Git 히스토리 확인
git log --oneline -10
```

---

## Auto-Handoff 설정 (Amp)

`~/.config/amp/settings.json`에 추가:

```json
{
  "amp.experimental.autoHandoff": { "context": 90 }
}
```

컨텍스트가 90% 차면 자동 핸드오프 → 단일 컨텍스트 윈도우를 초과하는 큰 스토리 처리 가능

---

## 아카이빙

새 기능 시작 시 (다른 `branchName`) Ralph가 자동으로 이전 실행 아카이브:

```
archive/YYYY-MM-DD-feature-name/
├── prd.json
└── progress.txt
```

---

## 유사 도구 비교

| 도구 | 특징 | 차이점 |
|------|------|--------|
| **Ralph (snarktank)** | Bash 스크립트, PRD 기반 | 가장 심플, 직접 제어 |
| **Ralph Playbook** | 방법론 문서 | 개념/철학 중심 |
| **Oh My Claude Code** | 플러그인, 28 에이전트 | 멀티에이전트 오케스트레이션 |
| **Auto-Claude** | 데스크톱 앱 | GUI, 12개 병렬 터미널 |

---

## 핵심 포인트

1. **PRD 기반 자율 실행**: 모든 스토리 완료까지 반복
2. **클린 컨텍스트**: 매 반복 새 AI 인스턴스
3. **메모리 채널 3개**: Git + progress.txt + prd.json
4. **작은 태스크**: 한 컨텍스트 윈도우 내 완료 가능해야
5. **피드백 루프 필수**: Typecheck + Tests + CI
6. **AGENTS.md 업데이트**: 학습 내용 축적
7. **7k Stars**: 가장 인기 있는 Ralph 구현체

---

## 참고 자료

- [GitHub 레포지토리](https://github.com/snarktank/ralph)
- [Geoffrey Huntley의 원본 Ralph 글](https://ghuntley.com/ralph/)
- [Ryan Carson의 Ralph 사용법 상세 글](https://x.com/ryancarson/status/2008548371712135632)
- [Interactive Flowchart](https://github.com/snarktank/ralph/tree/main/flowchart)
