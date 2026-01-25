# AI 코딩 에이전트 프로젝트 구성 가이드

> **작성일**: 2026-01-22  
> **목적**: 12개 문서 분석 후 최적의 프로젝트 구성 도출  
> **대상**: Claude Code, Cursor, 기타 AI 코딩 에이전트 사용자

---

## 요약

AI 코딩 에이전트를 효과적으로 활용하려면 **3계층 자동화 구조**(Skills, Hooks, Subagents)를 기반으로, **선택적 도구 활성화**와 **TDD 기반 Skill 검증**을 적용해야 합니다. 이 가이드는 실제 해커톤 우승자와 10개월 이상 실무 사용 경험을 바탕으로 한 권장사항입니다.

---

## 1. 디렉토리 구조 - 어떻게 구성해야 하나?

### 권장 구조

```
프로젝트/
├── .claude/                    # Claude Code용 (또는 .cursor/)
│   ├── skills/                 # 워크플로우 정의
│   │   ├── tdd-workflow/
│   │   │   └── SKILL.md
│   │   ├── coding-standards.md
│   │   └── security-review/
│   │       └── SKILL.md
│   ├── commands/               # 슬래시 명령어
│   │   ├── tdd.md
│   │   ├── plan.md
│   │   └── debug.md
│   ├── agents/                 # 서브에이전트 정의
│   │   ├── planner.md
│   │   ├── code-reviewer.md
│   │   └── security-reviewer.md
│   └── rules/                  # 항상 따라야 할 규칙
│       ├── security.md
│       └── testing.md
├── CLAUDE.md                   # 프로젝트별 컨텍스트
├── AGENTS.md                   # 스킬 목록 (OpenSkills용)
└── docs/
    └── plans/                  # 설계 문서
        └── YYYY-MM-DD-topic.md
```

### 출처 & 근거

| 디렉토리 | 출처 | 근거 |
|----------|------|------|
| `skills/` | [08-everything-claude-code] | "Skills는 광범위한 지침" |
| `commands/` | [08-everything-claude-code] | "Commands는 구체적인 실행 진입점" |
| `agents/` | [07-hackathon-winner-guide] | "Layer 3: 누가 할지 정의" |
| `rules/` | [08-everything-claude-code] | "항상 따라야 할 가이드라인" |
| `docs/plans/` | [06-superpowers-for-cursor] | "설계 승인 후 저장" |
| `AGENTS.md` | [09-openskills] | "스킬 목록 동기화" |

---

## 2. 3계층 자동화 - 무엇을 어디에 넣어야 하나?

```
┌─────────────────────────────────────────────────────────────┐
│  Layer 1: Skills & Commands                                 │
│  → "무엇을" 할지 (명시적 호출)                               │
├─────────────────────────────────────────────────────────────┤
│  Layer 2: Hooks                                             │
│  → "언제" 할지 (이벤트 기반 자동 반응)                       │
├─────────────────────────────────────────────────────────────┤
│  Layer 3: Subagents                                         │
│  → "누가" 할지 (역할 분리 및 위임)                           │
└─────────────────────────────────────────────────────────────┘
```

**출처**: [07-claude-code-hackathon-winner-guide.md] - "10개월간 Claude Code를 매일 사용하며 정립한 3계층 자동화 시스템"

### Layer 1: Skills vs Commands 구분

| 구분 | 용도 | 예시 |
|------|------|------|
| **Skills** | 광범위한 지침, 도메인 지식 | TDD 방법론, 코딩 표준, 보안 체크리스트 |
| **Commands** | 구체적 실행 진입점, 슬래시 명령 | `/tdd`, `/plan`, `/debug` |

**핵심**: Commands는 Skills를 **호출**한다. `/tdd` 명령은 `tdd-workflow/SKILL.md`를 로드.

**출처**: [08-everything-claude-code] - "Skills와 Commands의 핵심적 차이는 범위다"

### Layer 2: Hooks - 언제 사용?

| Hook | 트리거 | 용도 | 예시 |
|------|--------|------|------|
| `PreToolUse` | 도구 실행 전 | 차단, 경고 | dev 서버를 tmux 없이 실행 시 차단 |
| `PostToolUse` | 도구 실행 후 | 자동 체크 | TS 파일 수정 시 Prettier + console.log 경고 |
| `SessionStart` | 세션 시작 시 | 컨텍스트 로드 | 이전 세션 상태 복구 |
| `Stop` | 응답 완료 시 | 최종 감사 | 수정된 파일들의 console.log 최종 체크 |

**출처**: [08-everything-claude-code] - "Hooks 타입별 트리거 시점"

### Layer 3: Subagents - 언제 위임?

| 에이전트 | 위임 시점 | 권한 |
|----------|----------|------|
| `planner` | 복잡한 기능, 리팩토링 전 | Read, Grep, Glob |
| `code-reviewer` | 코드 작성 후 | Read, Grep, Glob, Bash |
| `security-reviewer` | 커밋 전 | Read, Grep |
| `tdd-guide` | 새 기능, 버그 수정 시 | Read, Edit, Bash |

**핵심 이점**:
1. 각 Subagent는 **제한된 도구/MCP 권한**만 가짐
2. 메인 Claude의 **컨텍스트 윈도우 보존**

**출처**: [07-hackathon-winner-guide] - "역할별 전문 에이전트에게 위임"

---

## 3. 컨텍스트 경제학 - 얼마나 켜야 하나?

```
┌─────────────────────────────────────────────────────────────┐
│  "20-30개 설정, 10개 미만 활성화, 80개 미만 도구"            │
└─────────────────────────────────────────────────────────────┘
```

**출처**: [07-hackathon-winner-guide] & [08-everything-claude-code]

### 문제

| 스펙상 컨텍스트 | 실제 사용 가능 | 손실 |
|----------------|---------------|------|
| 200k 토큰 | **70k 토큰** | 130k |

**원인**: 각 MCP/플러그인 활성화 시 도구 정의가 컨텍스트를 점유

### 해법

| 항목 | 등록 | 실제 활성화 |
|------|------|------------|
| MCP | 14-20개 | **5-6개** |
| 플러그인 | 다수 | **4-5개** |

**프로젝트별 `disabledMcpServers`로 불필요한 것 비활성화**

**출처**: [08-everything-claude-code] - "너무 많은 MCP 활성화 → 200k 컨텍스트가 70k로 축소"

### 권장 MCP 선택

| 프로젝트 유형 | 권장 MCP |
|--------------|----------|
| 웹 개발 | github, vercel, supabase |
| 백엔드 | github, supabase, clickhouse |
| 리서치 | firecrawl, memory, sequential-thinking |
| 문서 작업 | memory, context7 |

---

## 4. Skill 작성 - TDD로 검증해야 하나?

```
┌─────────────────────────────────────────────────────────────┐
│  Skill 없이 에이전트가 실패하는 것을 먼저 보지 않으면,        │
│  그 Skill이 올바른 것을 가르치는지 알 수 없다.               │
└─────────────────────────────────────────────────────────────┘
```

**출처**: [12-tdd-for-skills-writing-guide] - obra/superpowers

### TDD 사이클 적용

| 단계 | 코드 TDD | Skill TDD |
|------|---------|-----------|
| 🔴 RED | 테스트 실패 | Skill 없이 에이전트 실패 관찰 |
| 🟢 GREEN | 최소 코드로 통과 | 그 실패만 해결하는 Skill 작성 |
| 🔵 REFACTOR | 코드 정리 | 새 변명 찾아서 막기 |

### SKILL.md 구조

```yaml
---
name: skill-name-with-hyphens
description: Use when [트리거 조건만, 워크플로우 요약 금지]
---
```

**치명적 실수**: description에 워크플로우를 요약하면 Claude가 전체 Skill을 읽지 않고 description만 따름

**출처**: [12-tdd-for-skills-writing] - "Description 작성의 함정"

---

## 5. 워크플로우 - 어떤 순서로?

### 새 기능 개발

```
brainstorming (설계)
    ↓ 설계 승인
writing-plans (계획)
    ↓ 계획 승인
executing-plans ←→ code-review
    ↓ 각 태스크
tdd + verification
    ↓ 버그 발생 시
systematic-debugging + root-cause-tracing
    ↓ 모든 태스크 완료
finishing-branch
```

**출처**: [06-superpowers-for-cursor] - "워크플로우 흐름"

### 4대 철칙

```
┌─────────────────────────────────────────────────────────────┐
│  1. Evidence over Claims: 검증 없이 완료 주장 금지           │
│  2. Test-First: 테스트 전에 코드 작성 금지                   │
│  3. Root Cause First: 원인 파악 전 수정 금지                 │
│  4. YAGNI: 필요한 것만 구현                                  │
└─────────────────────────────────────────────────────────────┘
```

**출처**: [06-superpowers-for-cursor] - "핵심 원칙 (4대 철칙)"

---

## 6. 병렬화 - 어떻게 분리?

### 3가지 병렬화 기법

| 기법 | 분리 단위 | 사용 시나리오 |
|------|----------|--------------|
| `/fork` | 대화 세션 | 백엔드 + 프론트엔드 동시 개발 |
| **Git Worktrees** | 파일 시스템 | 완전히 독립된 브랜치 작업 |
| **Subagents** | 역할과 권한 | 보안 리뷰 + 코드 리뷰 병렬 |

**핵심 원리**:
```
"먼저 나누고, 그 다음 동시에"
```

**출처**: [07-hackathon-winner-guide] - "분리가 병렬성을 만든다"

### Git Worktrees 사용법

```bash
# 완전히 독립적인 디렉토리 생성
git worktree add ../feature-branch feature-branch

# 각 Worktree에서 별도 Claude 인스턴스 실행
# → 충돌 불가능 상태를 구조적으로 보장
```

**출처**: [07-hackathon-winner-guide] & [06-superpowers-for-cursor]

---

## 7. 도구 선택 - 무엇을 쓸까?

### Claude Code vs Cursor

| 측면 | Claude Code | Cursor |
|------|-------------|--------|
| 환경 | 터미널 | IDE |
| Hooks | 네이티브 지원 | User Rules로 대체 |
| Subagents | 네이티브 지원 | 제한적 |
| MCP | 완전 지원 | 지원 |
| 진입 장벽 | 중간 | 낮음 |

### OpenSkills 사용 여부

**사용 권장 시**:
- 여러 에이전트(Claude Code, Cursor, Aider)를 함께 사용
- 팀에서 스킬을 공유
- Anthropic 마켓플레이스 스킬 활용

```bash
# 설치
npx openskills install anthropics/skills

# AGENTS.md에 동기화
npx openskills sync
```

**출처**: [09-openskills] - "하나의 CLI. 모든 에이전트. Claude Code와 동일한 포맷."

---

## 8. 보안 체크리스트 - 무엇을 확인?

커밋 전 **필수** 확인:

- [ ] 하드코딩된 시크릿 없음 (API 키, 비밀번호, 토큰)
- [ ] 모든 사용자 입력 검증됨
- [ ] SQL 인젝션 방지 (파라미터화된 쿼리)
- [ ] XSS 방지 (HTML 살균)
- [ ] CSRF 보호 활성화
- [ ] 인증/인가 검증됨
- [ ] 모든 엔드포인트에 레이트 리밋
- [ ] 에러 메시지에 민감 데이터 노출 없음

**출처**: [08-everything-claude-code] - "보안 체크리스트"

---

## 9. 최종 권장 설정

### 단계별 적용

| 단계 | 적용 내용 | 난이도 |
|------|----------|--------|
| **1단계** | `.cursorrules` 또는 `CLAUDE.md` 작성 | ⭐ |
| **2단계** | Skills 디렉토리 구성 (TDD, 보안) | ⭐⭐ |
| **3단계** | Commands 추가 (`/tdd`, `/debug`) | ⭐⭐ |
| **4단계** | Hooks 설정 (자동 포맷팅, console.log 경고) | ⭐⭐⭐ |
| **5단계** | Subagents 정의 | ⭐⭐⭐ |
| **6단계** | MCP 선택적 활성화 | ⭐⭐⭐ |

### 최소 시작 구성

```
프로젝트/
├── .claude/ (또는 .cursor/)
│   ├── skills/
│   │   └── tdd-workflow/
│   │       └── SKILL.md
│   └── commands/
│       └── tdd.md
├── CLAUDE.md
└── .cursorrules (Cursor용)
```

---

## 10. 자주 하는 실수

### 변명 vs 현실

| 변명 | 현실 | 출처 |
|------|------|------|
| "MCP 많이 켜면 강력해지겠지" | 컨텍스트 70k로 축소, 성능 저하 | [07-hackathon] |
| "Skill 없어도 잘 하겠지" | 테스트해보면 절반은 불필요한 내용 | [12-tdd-skills] |
| "테스트 나중에 할게" | 즉시 통과하는 테스트는 아무것도 증명 안 함 | [06-superpowers] |
| "이건 간단해서 설계 필요 없음" | 설계 없이 구현 → 대규모 수정 | [06-superpowers] |
| "버그니까 빨리 고쳐야지" | 근본 원인 파악 전 수정 → 3번 이상 실패 | [06-superpowers] |

---

## 요약 - 최적 선택

### 필수 적용

1. **3계층 구조** 이해: Skills(무엇) + Hooks(언제) + Subagents(누가)
2. **컨텍스트 경제학**: MCP 10개 미만 활성화
3. **TDD 강제**: 테스트 먼저, 예외 없음
4. **4대 철칙**: Evidence > Claims, Test-First, Root Cause First, YAGNI

### 도구별 권장

| 환경 | 권장 구성 |
|------|----------|
| **Claude Code** | 전체 3계층 (Skills + Hooks + Subagents) |
| **Cursor** | Skills + Commands + User Rules |
| **멀티 에이전트** | OpenSkills + `.agent/skills/` |

### 프로젝트 규모별

| 규모 | 권장 |
|------|------|
| **소규모** | CLAUDE.md + TDD skill |
| **중규모** | + Commands + 보안 skill |
| **대규모** | + Hooks + Subagents + MCP 선택적 활성화 |

---

## 출처 요약

| 문서 | 핵심 기여 |
|------|----------|
| [06-superpowers-for-cursor] | 워크플로우, 4대 철칙, TDD |
| [07-hackathon-winner-guide] | 3계층 구조, 컨텍스트 경제학, 병렬화 |
| [08-everything-claude-code] | 완전 설정, Hooks, MCP, 보안 |
| [09-openskills] | 멀티 에이전트, SKILL.md 포맷 |
| [12-tdd-for-skills-writing] | Skill TDD 검증 |

---

> **"자동화를 도구로 보면 '이 도구가 저 작업을 빠르게 해준다'에서 사고가 멈춘다. 자동화를 계층으로 보면 '이 작업 전체를 어떻게 구조화할 것인가'로 질문이 확장된다."**
> — [07-claude-code-hackathon-winner-guide]
