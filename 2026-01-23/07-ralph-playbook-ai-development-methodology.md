# The Ralph Playbook - AI 개발 방법론 완전 가이드

> **URL**: https://github.com/ghuntley/how-to-ralph-wiggum  
> **유형**: GitHub 레포지토리 (방법론 가이드)  
> **작성자**: Geoffrey Huntley (@ghuntley)  
> **Stars**: 954 ⭐ | **Forks**: 93

---

## 요약

"Ralph Wiggum 기법"을 체계적으로 정리한 **AI 개발 방법론 플레이북**. AI 코딩 에이전트를 활용해 **패스트푸드 직원 임금보다 낮은 비용**으로 소프트웨어를 개발하는 방법론이다. 3단계 페이즈, 2개 프롬프트, 1개 루프로 구성.

> "The AI development methodology that reduces software costs to less than a fast food worker's wage."

---

## 핵심 개념: 3단계, 2개 프롬프트, 1개 루프

```
┌─────────────────────────────────────────────────────────────┐
│  Phase 1: Define Requirements (LLM 대화)                    │
│     아이디어 → JTBD(Jobs to Be Done) → specs/*.md           │
├─────────────────────────────────────────────────────────────┤
│  Phase 2: Create Plan (Ralph Loop - Plan Mode)              │
│     specs/*.md 분석 → IMPLEMENTATION_PLAN.md 생성            │
├─────────────────────────────────────────────────────────────┤
│  Phase 3: Build (Ralph Loop - Build Mode)                   │
│     계획 실행 → 코드 구현 → 무한 반복                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 워크플로우 다이어그램

```
       ┌──────────────┐
       │   아이디어    │
       └──────┬───────┘
              │
              ▼
┌─────────────────────────────┐
│  Phase 1: Requirements      │
│  ────────────────────────   │
│  • JTBD 식별                │
│  • 토픽별 분리              │
│  • URL → 컨텍스트 로드      │
│  • specs/*.md 생성          │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  Phase 2: Planning          │
│  ────────────────────────   │
│  PROMPT_plan.md 사용        │
│  • 스펙 + 코드 분석         │
│  • IMPLEMENTATION_PLAN.md   │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  Phase 3: Building          │◄──────┐
│  ────────────────────────   │       │
│  PROMPT_build.md 사용       │       │
│  • 계획 실행                │       │ Ralph
│  • 코드 구현                │       │ Loop
│  • 완료까지 반복            │───────┘
└─────────────────────────────┘
```

---

## 두 가지 Ralph 모드

| 모드 | 사용 시점 | 프롬프트 초점 |
|------|-----------|---------------|
| **Plan** | 새 기능/대규모 리팩토링 시작 시 | 스펙 분석 → 계획 생성 |
| **Build** | 계획 실행, 버그 수정 시 | 계획에 따라 구현 |

---

## 핵심 원칙

### 1️⃣ JTBD (Jobs to Be Done) 기반

```
사용자가 "무엇을 완료하고 싶은가?"에 집중

예시:
❌ "사진 업로드 기능"
✅ "사용자가 공간을 캡처할 수 있다"
```

### 2️⃣ Spec 파일 구조

```
specs/
├── upload-photo.md       # 업로드 활동
├── extract-colors.md     # 색상 추출 활동
├── arrange-palette.md    # 팔레트 정렬 활동
└── export-palette.md     # 내보내기 활동
```

### 3️⃣ 서브에이전트 대규모 활용

```markdown
# 계획 프롬프트 예시
0b. Study `specs/*` with up to 250 parallel Sonnet subagents
2. Use up to 500 Sonnet subagents to compare `src/*` against `specs/*`
3. Use an Opus subagent (ultrathink) to analyze findings
```

---

## SLC (Simple, Lovable, Complete) 릴리스 전략

### MVP vs SLC

| 접근법 | 철학 |
|--------|------|
| **MVP** | 학습 최적화 → 사용자 희생 (깨진/불편한 경험) |
| **SLC** | 가치 제공하면서 학습 (범위 내 완전한 경험) |

### SLC 기준

```
┌─────────────────────────────────────────────────────────────┐
│  Simple   - 빠르게 출시할 수 있는 좁은 범위                  │
│  Lovable  - 사람들이 실제로 사용하고 싶어함                  │
│  Complete - 범위 내에서 작업을 완전히 완수                   │
└─────────────────────────────────────────────────────────────┘
```

### 릴리스 슬라이싱 예시

```
                  UPLOAD    →   EXTRACT    →   ARRANGE     →   SHARE

Palette Picker:   basic         auto                           export
                  ───────────────────────────────────────────────────
Mood Board:                     palette        manual
                  ───────────────────────────────────────────────────
Design Studio:    batch         AI themes      templates       embed
```

| 릴리스 | 포함 활동 | 가치 |
|--------|-----------|------|
| **Palette Picker** | 업로드 + 추출 + 내보내기 | Day 1부터 즉시 가치 |
| **Mood Board** | + 정렬 | 창의적 표현 추가 |
| **Design Studio** | + 배치, AI 테마, 템플릿 | 전문가 기능 |

---

## 파일 구조

```
project/
├── specs/                    # JTBD 활동별 스펙
│   ├── activity-1.md
│   └── activity-2.md
├── AUDIENCE_JTBD.md          # 대상 사용자 & JTBD 정의
├── IMPLEMENTATION_PLAN.md    # 구현 계획 (AI 생성)
├── PROMPT_plan.md            # 계획 모드 프롬프트
├── PROMPT_build.md           # 빌드 모드 프롬프트
└── src/
    └── lib/                  # 공유 유틸리티 & 컴포넌트
```

---

## 계획 프롬프트 템플릿 (SLC 버전)

```markdown
0a. Study @AUDIENCE_JTBD.md to understand who we're building for and their Jobs to Be Done.
0b. Study `specs/*` with up to 250 parallel Sonnet subagents to learn JTBD activities.
0c. Study @IMPLEMENTATION_PLAN.md (if present) to understand the plan so far.
0d. Study `src/lib/*` with up to 250 parallel Sonnet subagents to understand shared utilities & components.
0e. For reference, the application source code is in `src/*`.

1. Sequence the activities in `specs/*` into a user journey map for the audience in @AUDIENCE_JTBD.md.

2. Determine the next SLC release. Use up to 500 Sonnet subagents to compare `src/*` against `specs/*`. 
   Use an Opus subagent to analyze findings. Ultrathink.
   Prefer thin horizontal slices - the narrowest scope that still delivers real value.

3. Use an Opus subagent (ultrathink) to create/update @IMPLEMENTATION_PLAN.md
   - Begin with summary of recommended SLC release
   - List prioritized tasks for that scope
   - Note discoveries outside scope as future work

IMPORTANT: Plan only. Do NOT implement anything.
Do NOT assume functionality is missing; confirm with code search first.

ULTIMATE GOAL: Most valuable next release for the audience in @AUDIENCE_JTBD.md.
```

---

## 빌드 프롬프트 템플릿

```markdown
0a. Study @IMPLEMENTATION_PLAN.md to understand what to build.
0b. Study `src/lib/*` to understand shared utilities & components.

1. Implement the next highest priority item from @IMPLEMENTATION_PLAN.md.

2. After implementation:
   - Run tests
   - Update @IMPLEMENTATION_PLAN.md (mark complete, add discoveries)
   - Commit with descriptive message

3. LOOP: Return to step 1 until plan is complete.

IMPORTANT: 
- Treat `src/lib` as standard library - prefer consolidated implementations
- One task at a time, fully complete before moving on
- If blocked, note in plan and move to next item
```

---

## AUDIENCE_JTBD.md 구조

```markdown
# Audience & Jobs to Be Done

## Target Audience
- **Primary**: Interior designers looking for quick color inspiration
- **Secondary**: Clients reviewing design proposals

## Jobs to Be Done

### Designer JTBD
1. **Capture Space** - "I want to quickly capture a room's color essence"
2. **Explore Concepts** - "I want to experiment with color variations"
3. **Present to Client** - "I want to share professional-looking palettes"

### Client JTBD
1. **Review Options** - "I want to see design options clearly"
2. **Provide Feedback** - "I want to indicate preferences easily"

## Relationships
- Designer: capture → explore → present
- Client: review → feedback
- Connection: Designer presents TO Client, Client feedback TO Designer
```

---

## 카디널리티 (관계)

```
┌─────────────────────────────────────────────────────────────┐
│  1 Audience → Many JTBDs                                    │
│     "Designer" has "capture", "explore", "present"          │
├─────────────────────────────────────────────────────────────┤
│  1 JTBD → Many Activities                                   │
│     "capture" includes upload, measurements, room detection │
├─────────────────────────────────────────────────────────────┤
│  1 Activity → Multiple JTBDs (가능)                         │
│     "upload photo" serves both "capture" AND "inspiration"  │
└─────────────────────────────────────────────────────────────┘
```

---

## 기존 Ralph vs SLC Ralph 비교

### 기존 Ralph 접근법

```
1. Define Requirements: JTBD → specs/*.md
2. Create Tasks Plan: specs + code → IMPLEMENTATION_PLAN.md
3. Build: 전체 범위에 대해 빌드
```

→ **문제**: 기능 중심, SLC 릴리스 아님

### SLC Ralph 접근법

```
1. Define Audience & JTBDs → AUDIENCE_JTBD.md
2. Define Activities → specs/*.md
3. Plan SLC Release → IMPLEMENTATION_PLAN.md (슬라이스 추천 포함)
4. Build → 해당 슬라이스만 구현
5. Repeat → 다음 SLC 슬라이스
```

→ **결과**: 점진적으로 가치 있는 릴리스 생성

---

## 핵심 포인트

1. **3단계 퍼널**: Requirements → Plan → Build
2. **JTBD 중심**: 기능이 아닌 "완료해야 할 일" 중심 사고
3. **대규모 병렬화**: 250~500개 서브에이전트 동시 활용
4. **SLC 슬라이싱**: MVP 대신 "사용 가능한 최소 단위" 릴리스
5. **Thin Horizontal Slices**: 가장 좁은 범위의 가치 있는 슬라이스
6. **Ultrathink with Opus**: 복잡한 분석은 Opus 서브에이전트에 위임

---

## Ralph 기법의 핵심 철학

> "Ralph isn't just 'a loop that codes.' It's a funnel with 3 Phases, 2 Prompts, and 1 Loop."

```
┌─────────────────────────────────────────────────────────────┐
│  아이디어                                                    │
│       ↓                                                     │
│  JTBD로 분해                                                 │
│       ↓                                                     │
│  활동별 스펙                                                 │
│       ↓                                                     │
│  SLC 슬라이스 계획                                           │
│       ↓                                                     │
│  무한 루프 빌드                                              │
│       ↓                                                     │
│  가치 있는 릴리스 🚀                                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 참고 자료

- [The Ralph Playbook (원본)](https://github.com/ghuntley/how-to-ralph-wiggum)
- [Geoffrey Huntley의 원본 포스트](https://ghuntley.com/ralph/)
- [ralphcoin.org](https://ralphcoin.org)
- [Matt Pocock의 해설](https://twitter.com/mattpocockuk)
- [Ryan Carson의 해설](https://twitter.com/ryancarson)
