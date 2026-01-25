# Agent Skills for Context Engineering - 컨텍스트 엔지니어링 스킬 컬렉션

> **URL**: https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering  
> **유형**: GitHub 레포지토리 (스킬 컬렉션)  
> **작성자**: Muratcan Koylan  
> **라이선스**: MIT  
> **Stars**: 7.7k ⭐ | **Forks**: 608

---

## 요약

**컨텍스트 엔지니어링** 원칙에 초점을 맞춘 에이전트 스킬의 종합 컬렉션. 프로덕션급 AI 에이전트 시스템 구축을 위한 스킬들로, 프롬프트 엔지니어링과 달리 **모델의 컨텍스트 윈도우 전체를 관리**하는 방법론을 다룬다. Claude Code, Cursor, Codex 등 모든 에이전트 플랫폼에서 사용 가능.

> "Context engineering is the discipline of managing the language model's context window."

---

## 컨텍스트 엔지니어링이란?

```
┌─────────────────────────────────────────────────────────────┐
│  프롬프트 엔지니어링 vs 컨텍스트 엔지니어링                   │
├─────────────────────────────────────────────────────────────┤
│  Prompt Engineering                                         │
│  → 효과적인 지시문 작성에 집중                               │
│                                                             │
│  Context Engineering                                        │
│  → 모델에 들어가는 모든 정보의 전체적 관리                   │
│    - System prompts                                         │
│    - Tool definitions                                       │
│    - Retrieved documents                                    │
│    - Message history                                        │
│    - Tool outputs                                           │
└─────────────────────────────────────────────────────────────┘
```

### 핵심 문제

컨텍스트 윈도우는 **토큰 용량**이 아닌 **어텐션 메커니즘**에 의해 제한됨:

- **Lost-in-the-middle**: 중간에 있는 정보를 잊어버림
- **U-shaped attention**: 처음과 끝에 집중
- **Attention scarcity**: 어텐션 자원 부족

> **목표**: 원하는 결과의 가능성을 최대화하는 **가장 작은 고신호 토큰 집합** 찾기

---

## 스킬 개요

### 1️⃣ 기초 스킬 (Foundational)

| 스킬 | 설명 |
|------|------|
| **context-fundamentals** | 컨텍스트 윈도우 기본 원리 |
| **context-degradation** | 성능 저하 진단 및 수정 |
| **context-compression** | 컨텍스트 압축 기법 |
| **context-optimization** | 토큰 비용 최적화, KV-cache |

### 2️⃣ 아키텍처 스킬 (Architecture)

| 스킬 | 설명 |
|------|------|
| **multi-agent-patterns** | 멀티에이전트 시스템 설계 |
| **memory-systems** | 에이전트 메모리, 지식 그래프 |
| **tool-design** | 에이전트 도구 설계, MCP 도구 |
| **filesystem-context** | 파일 기반 컨텍스트 관리 |
| **hosted-agents** | 백그라운드/호스팅 에이전트 |

### 3️⃣ 평가 스킬 (Evaluation)

| 스킬 | 설명 |
|------|------|
| **evaluation** | 에이전트 성능 평가, 테스트 프레임워크 |
| **advanced-evaluation** | LLM-as-Judge, 바이어스 완화 |

### 4️⃣ 개발 스킬 (Development)

| 스킬 | 설명 |
|------|------|
| **project-development** | LLM 프로젝트 시작, 배치 파이프라인 |
| **bdi-mental-states** | BDI 아키텍처, 인지 에이전트 |

---

## Claude Code 설치

### 옵션 A: UI로 설치

```
1. Claude Code 열기
2. /plugins 입력
3. context-engineering-marketplace 검색
4. 원하는 플러그인 선택 후 Install now
```

### 옵션 B: 명령어로 설치

```bash
/plugin install context-engineering-fundamentals@context-engineering-marketplace
/plugin install agent-architecture@context-engineering-marketplace
/plugin install agent-evaluation@context-engineering-marketplace
/plugin install agent-development@context-engineering-marketplace
/plugin install cognitive-architecture@context-engineering-marketplace
```

---

## 플러그인 구성

| 플러그인 | 포함 스킬 |
|----------|-----------|
| **context-engineering-fundamentals** | context-fundamentals, context-degradation, context-compression, context-optimization |
| **agent-architecture** | multi-agent-patterns, memory-systems, tool-design, filesystem-context, hosted-agents |
| **agent-evaluation** | evaluation, advanced-evaluation |
| **agent-development** | project-development |
| **cognitive-architecture** | bdi-mental-states |

---

## 스킬 트리거

| 스킬 | 트리거 키워드 |
|------|---------------|
| context-fundamentals | "understand context", "explain context windows" |
| context-degradation | "diagnose context problems", "fix lost-in-middle" |
| context-compression | "compress context", "reduce token usage" |
| context-optimization | "optimize context", "implement KV-cache" |
| multi-agent-patterns | "design multi-agent system", "implement supervisor pattern" |
| memory-systems | "implement agent memory", "build knowledge graph" |
| tool-design | "design agent tools", "implement MCP tools" |
| filesystem-context | "offload context to files", "agent scratch pad" |
| hosted-agents | "build background agent", "sandboxed execution" |
| evaluation | "evaluate agent performance", "build test framework" |
| advanced-evaluation | "implement LLM-as-judge", "mitigate bias" |
| project-development | "start LLM project", "design batch pipeline" |
| bdi-mental-states | "model agent mental states", "build cognitive agent" |

---

## 예제 프로젝트

### 1️⃣ Digital Brain Skill

**창업자/크리에이터를 위한 개인 운영 체제**

```
digital-brain-skill/
├── SKILL.md              # 메인 스킬 파일
├── modules/
│   ├── identity/         # 아이덴티티 모듈
│   ├── content/          # 콘텐츠 관리
│   ├── knowledge/        # 지식 베이스
│   ├── network/          # 네트워크 관리
│   ├── operations/       # 운영
│   └── agents/           # 에이전트
└── scripts/              # 자동화 스크립트
    ├── weekly_review
    ├── content_ideas
    ├── stale_contacts
    └── idea_to_draft
```

**적용 스킬**: context-fundamentals, context-optimization, memory-systems, tool-design, multi-agent-patterns, evaluation, project-development

**핵심 패턴**:
- **Progressive Disclosure**: 3단계 로딩 (SKILL.md → MODULE.md → 데이터 파일)
- **Module Isolation**: 6개 독립 모듈
- **Append-Only Memory**: JSONL 파일, 스키마-퍼스트

### 2️⃣ X-to-Book System

**X 계정 모니터링 → 일일 합성 책 생성**

```
다중 에이전트 시스템:
- X 계정 모니터링 에이전트
- 콘텐츠 합성 에이전트
- 책 생성 에이전트
```

**적용 스킬**: multi-agent-patterns, memory-systems, context-optimization, tool-design, evaluation

### 3️⃣ LLM-as-Judge Skills

**프로덕션급 LLM 평가 도구 (TypeScript)**

```typescript
// 평가 기능
- Direct Scoring: 가중 기준 + 루브릭 평가
- Pairwise Comparison: 위치 바이어스 완화 비교
- Rubric Generation: 도메인별 평가 기준 생성
- EvaluatorAgent: 모든 평가 기능 통합
```

**테스트**: 19개 통과

### 4️⃣ Book SFT Pipeline

**소형 모델(8B)을 특정 작가 스타일로 학습**

```
- 지능형 세그멘테이션: 2-tier 청킹
- 프롬프트 다양성: 15+ 템플릿
- Tinker 통합: LoRA 학습 ($2 총 비용)
- 검증: Pangram에서 70% 인간 점수
```

**사례 연구**: Gertrude Stein 스타일

---

## 컨텍스트 엔지니어링 핵심 원칙

### 1️⃣ 어텐션 예산 관리

```
┌─────────────────────────────────────────────────────────────┐
│  200k 토큰 컨텍스트 ≠ 200k 토큰 활용 가능                    │
│                                                             │
│  실제 활용:                                                  │
│  - 처음 ~20% → 높은 어텐션                                   │
│  - 중간 ~60% → 낮은 어텐션 (lost-in-middle)                 │
│  - 마지막 ~20% → 높은 어텐션                                 │
└─────────────────────────────────────────────────────────────┘
```

### 2️⃣ 컨텍스트 압축 전략

| 전략 | 설명 |
|------|------|
| **요약** | 긴 대화/문서를 핵심만 요약 |
| **청킹** | 관련 정보만 선택적 로드 |
| **Progressive Disclosure** | 단계별 정보 공개 |
| **Append-Only** | 이전 컨텍스트 보존하며 추가 |

### 3️⃣ 멀티에이전트 패턴

```
Supervisor Pattern:
┌─────────────┐
│  Supervisor │
└──────┬──────┘
       │
   ┌───┴───┬───────┐
   ▼       ▼       ▼
┌─────┐ ┌─────┐ ┌─────┐
│Agent│ │Agent│ │Agent│
│  A  │ │  B  │ │  C  │
└─────┘ └─────┘ └─────┘
```

---

## 스킬 구조

```
skill-name/
├── SKILL.md              # 필수: 지침 + 메타데이터
├── scripts/              # 선택: 개념 시연 코드
└── references/           # 선택: 추가 문서 및 참조
```

### SKILL.md 가이드라인

- **500줄 미만** 권장 (최적 성능)
- 명확하고 실행 가능한 지침
- 트레이드오프 및 잠재적 문제 문서화
- 작동하는 예제 포함

---

## Cursor & IDE 사용

`.rules` 파일에 스킬 내용 복사 또는 프로젝트별 Skills 폴더 생성:

```
project/
├── .cursor/
│   └── rules/
│       └── context-engineering.md
└── ...
```

---

## 유사 컬렉션 비교

| 컬렉션 | Stars | 특징 |
|--------|-------|------|
| **Claude Code Router** | 26.3k | 멀티 프로바이더 라우팅 |
| **Awesome Claude Skills** | 24.4k | 범용 스킬 큐레이션 |
| **Agent Skills for Context Engineering** | 7.7k | 컨텍스트 엔지니어링 전문 |
| **Ralph (snarktank)** | 7k | PRD 기반 자율 루프 |

---

## 핵심 포인트

1. **컨텍스트 엔지니어링**: 프롬프트 엔지니어링을 넘어 전체 컨텍스트 관리
2. **어텐션 메커니즘 이해**: Lost-in-middle, U-shaped attention
3. **5개 플러그인**: 기초, 아키텍처, 평가, 개발, 인지
4. **실전 예제**: Digital Brain, X-to-Book, LLM-as-Judge
5. **플랫폼 무관**: Claude Code, Cursor, Codex 모두 지원
6. **500줄 규칙**: 스킬 파일 최적 성능을 위한 가이드라인

---

## 참고 자료

- [GitHub 레포지토리](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)
- [Digital Brain Example](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering/tree/main/examples/digital-brain-skill)
- [LLM-as-Judge Skills](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering/tree/main/examples/llm-as-judge-skills)
- [Book SFT Pipeline](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering/tree/main/examples/book-sft-pipeline)
- [CONTRIBUTING.md](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering/blob/main/CONTRIBUTING.md)
