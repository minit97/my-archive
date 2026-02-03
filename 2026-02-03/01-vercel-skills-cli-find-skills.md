# Vercel Skills CLI - AI가 스스로 도구를 찾는 시대

> **URL**: https://github.com/vercel-labs/skills/blob/main/skills/find-skills/SKILL.md  
> **유형**: GitHub (SKILL.md)  
> **작성자**: Vercel Labs  
> **생태계**: https://skills.sh/

---

## 요약

Vercel이 Skills CLI에 **AI가 스스로 필요한 도구를 탐색하는 기능**을 추가했습니다. `npx skills find` 명령어를 통해 사용자가 자연어로 의도를 말하면 AI가 방대한 스킬 생태계에서 적합한 Skill을 **스스로 탐색하고 제안**합니다.

> "Skill도 이제 AI가 찾아쓰는 시대"

---

## 왜 중요한가?

```
┌─────────────────────────────────────────────────────────────┐
│  현재                                                        │
│  사용자: "어떤 스킬이 있지?" → AI가 검색 → 제안              │
├─────────────────────────────────────────────────────────────┤
│  미래 (자가 진화형 에이전트)                                 │
│  AI: "이 작업에 내 능력이 부족하네"                          │
│       ↓                                                     │
│  AI: "npx skills find ..." 실행                             │
│       ↓                                                     │
│  AI: 적합한 스킬 발견 → 설치 → 학습 → 적용                  │
│       ↓                                                     │
│  AI: 작업 완료 (스스로 능력 확장)                            │
└─────────────────────────────────────────────────────────────┘
```

**커뮤니티의 힘**: 개발자들이 만든 수많은 스킬이 AI 에이전트의 능력 확장 도구가 됨

---

## find-skills 스킬이란?

### 활성화 시점

사용자가 다음과 같이 말할 때 자동 활성화:

| 패턴 | 예시 |
|------|------|
| **"how do I do X"** | "how do I make my React app faster?" |
| **"find a skill for X"** | "find a skill for PR reviews" |
| **"is there a skill for X"** | "is there a skill for changelog?" |
| **"can you do X"** | "can you help with testing?" |
| **능력 확장 요청** | "I wish I had help with design" |

---

## Skills CLI 핵심 명령어

```bash
# 스킬 검색 (인터랙티브 또는 키워드)
npx skills find [query]

# 스킬 설치
npx skills add <package>

# 업데이트 확인
npx skills check

# 모든 스킬 업데이트
npx skills update
```

### 스킬 브라우징

**https://skills.sh/** 에서 전체 스킬 생태계 탐색 가능

---

## 사용 워크플로우

### Step 1: 사용자 의도 파악

```
사용자 질문 분석:
├── 도메인: React, testing, design, deployment 등
├── 특정 작업: 테스트 작성, 애니메이션, PR 리뷰 등
└── 기존 스킬 존재 가능성 판단
```

### Step 2: 스킬 검색

```bash
# 예시
npx skills find react performance
npx skills find pr review
npx skills find changelog
```

### Step 3: 결과 제시

```
Install with npx skills add <owner/repo@skill>

vercel-labs/agent-skills@vercel-react-best-practices
└ https://skills.sh/vercel-labs/agent-skills/vercel-react-best-practices
```

### Step 4: 설치

```bash
# 글로벌 설치 + 확인 생략
npx skills add <owner/repo@skill> -g -y
```

---

## 일반적인 스킬 카테고리

| 카테고리 | 검색 키워드 예시 |
|----------|-----------------|
| **Web Development** | react, nextjs, typescript, css, tailwind |
| **Testing** | testing, jest, playwright, e2e |
| **DevOps** | deploy, docker, kubernetes, ci-cd |
| **Documentation** | docs, readme, changelog, api-docs |
| **Code Quality** | review, lint, refactor, best-practices |
| **Design** | ui, ux, design-system, accessibility |
| **Productivity** | workflow, automation, git |

---

## 검색 팁

| 팁 | 설명 |
|----|------|
| **구체적 키워드** | "testing" 보다 "react testing"이 효과적 |
| **대안 용어 시도** | "deploy" 안되면 "deployment", "ci-cd" 시도 |
| **인기 소스 확인** | `vercel-labs/agent-skills`, `ComposioHQ/awesome-claude-skills` |

---

## 스킬이 없을 때

```
스킬 검색 결과 없음
       ↓
1. 결과 없음 안내
2. 일반 능력으로 직접 도움 제안
3. 커스텀 스킬 생성 안내: npx skills init my-xyz-skill
```

---

## 자가 진화형 에이전트로의 발전

### 현재 단계

```
사용자 → "X 할 수 있어?" → AI 검색 → 스킬 제안 → 사용자가 설치
```

### 미래 예상

```
AI 작업 중
    ↓
"이 작업에 필요한 능력이 부족하다"
    ↓
npx skills find [필요한 기능]
    ↓
적합한 스킬 발견
    ↓
npx skills add <skill> -g -y (자동 설치)
    ↓
새 스킬로 능력 확장
    ↓
작업 완료
```

### 핵심 의의

| 관점 | 의미 |
|------|------|
| **AI 자율성** | 과제 수행 중 능력 부족 시 스스로 해결책 탐색 |
| **학습 능력** | 새로운 스킬을 발견하고 적용하는 자가 학습 |
| **생태계 활용** | 커뮤니티가 만든 스킬이 AI의 확장 모듈이 됨 |
| **워크플로우 통합** | 에이전트 작업 흐름에 자연스럽게 결합 |

---

## 핵심 포인트

1. **커뮤니티의 힘**: 개발자들이 만든 스킬 → AI 에이전트의 능력 확장 도구
2. **자연어 검색**: `npx skills find [query]`로 의도 기반 탐색
3. **글로벌 생태계**: skills.sh에서 전체 스킬 브라우징
4. **자가 진화 가능성**: AI가 능력 부족 시 스스로 스킬 탐색 → 설치 → 적용
5. **인기 소스**: `vercel-labs/agent-skills`, `ComposioHQ/awesome-claude-skills`
6. **커스텀 스킬**: `npx skills init`으로 자체 스킬 생성 가능

---

## 참고 자료

- [find-skills SKILL.md](https://github.com/vercel-labs/skills/blob/main/skills/find-skills/SKILL.md)
- [skills.sh](https://skills.sh/) - 스킬 생태계 브라우저
- [Vercel Labs Skills](https://github.com/vercel-labs/skills)
