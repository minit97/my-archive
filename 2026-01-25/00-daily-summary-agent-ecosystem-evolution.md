# 2026-01-25 AI 에이전트 생태계 진화 총정리

> **총 요약 문서 수**: 6개  
> **주제**: Subagents, Tasks, Skills 표준화, 터미널 DX 개선

---

## 📊 Stars 기준 순위

| 순위 | 도구 | Stars | 카테고리 |
|------|------|-------|----------|
| 1 | **Hugging Face Skills** | 1k | AI/ML 스킬 |
| 2 | **Google Stitch Skills** | 684 | 디자인→코드 |
| 3 | **Plugins for Claude Natives** | 425 | Claude Code 플러그인 |
| 4 | **Claude Chill** | 350 | 터미널 최적화 |
| 5 | **Stitch MCP** | 153 | MCP 서버 |

---

## 🗂️ 카테고리별 정리

### 1️⃣ 제품 업데이트 (Cursor & Claude Code)

| # | 문서 | 도구 | 핵심 기능 |
|---|------|------|-----------|
| 03 | `03-cursor-2-4-subagents-skills-release.md` | **Cursor 2.4** | Subagents, Skills, Image Generation |
| 04 | `04-claude-code-todos-to-tasks-upgrade.md` | **Claude Code Tasks** | Todos→Tasks 업그레이드 |

**핵심 인사이트**:
```
Cursor 2.4                          Claude Code
├── Subagents (병렬 전문 에이전트)    ├── Tasks (의존성 기반 태스크)
├── Skills (SKILL.md)               ├── 멀티 세션 협업
├── Image Generation                └── 서브에이전트 간 공유
└── Cursor Blame (Enterprise)
```

- **Subagents**: 병렬 실행, 독립 컨텍스트, 전문화된 태스크 분업
- **Tasks**: `~/.claude/tasks/`에 영속 저장, `blocked by #1, #7` 의존성 지원
- **SKILL.md 수렴**: Cursor, Claude Code 모두 같은 포맷 채택

---

### 2️⃣ 터미널 DX 개선

| # | 문서 | 도구 | 핵심 기능 |
|---|------|------|-----------|
| 01 | `01-claude-chill-pty-proxy-terminal-optimizer.md` | **Claude Chill** | PTY 프록시, 차분 렌더링 |

**핵심 인사이트**:
```
문제: Claude Code가 5000줄 원자적 업데이트 전송 → 랙, 깜빡임
        ↓
해결: VT100 에뮬레이터로 화면 상태 추적 → 변경점만 렌더링
        ↓
추가: Ctrl+6 룩백 모드로 전체 히스토리 확인
```

- **Rust 100%**: 빠르고 안정적인 구현
- **자동 룩백**: 15초 idle 후 자동 히스토리 덤프

---

### 3️⃣ AI/ML 스킬 생태계

| # | 문서 | 도구 | 핵심 기능 |
|---|------|------|-----------|
| 02 | `02-huggingface-skills-ai-ml-agent-skills.md` | **Hugging Face Skills** | 모델 훈련, 데이터셋, 평가 스킬 |

**핵심 인사이트**:
```
8개 스킬 제공:
├── hugging-face-model-trainer (SFT, DPO, GRPO)
├── hugging-face-datasets
├── hugging-face-evaluation
├── hugging-face-jobs
├── hugging-face-trackio
├── hugging-face-cli
├── hugging-face-paper-publisher
└── hugging-face-tool-builder
```

- **크로스 플랫폼**: Claude Code, Codex, Gemini CLI 모두 호환
- **공식 HF 스킬**: Hugging Face 생태계와 직접 연동

---

### 4️⃣ 디자인→코드 자동화

| # | 문서 | 도구 | 핵심 기능 |
|---|------|------|-----------|
| 05 | `05-google-stitch-skills-mcp-ecosystem.md` | **Stitch Skills + MCP** | 디자인 시스템 → React 컴포넌트 |

**핵심 인사이트**:
```
Google Stitch 생태계:
┌─────────────────────────────────────────────────────┐
│ 디자인 시스템 (Stitch)                               │
│       ↓                                             │
│ MCP 서버 (stitch-mcp)                               │
│       ↓                                             │
│ AI 에이전트 (Claude Code, Cursor, Gemini CLI)       │
│       ↓                                             │
│ Agent Skills (stitch-skills)                        │
│       ├── DESIGN.md 자동 생성                       │
│       └── React 컴포넌트 변환                       │
└─────────────────────────────────────────────────────┘
```

- **2개 스킬**: `design-md` (문서화), `react-components` (코드 변환)
- **MCP 프록시**: 토큰 자동 갱신, 디버그 로깅

---

### 5️⃣ Claude Code 플러그인

| # | 문서 | 도구 | 핵심 기능 |
|---|------|------|-----------|
| 06 | `06-claude-code-plugins-power-users.md` | **Plugins for Claude Natives** | 9개 생산성 플러그인 |

**핵심 인사이트**:

| 플러그인 | 핵심 기능 |
|---------|----------|
| **agent-council** | 여러 AI 모델 합의 도출 |
| **clarify** | 요구사항 → 명확한 스펙 |
| **dev** | 커뮤니티 스캔 + 기술 의사결정 |
| **youtube-digest** | 유튜브 요약 + 한글 번역 + 퀴즈 |
| **kakaotalk** | macOS 카카오톡 연동 |
| **session-wrap** | 세션 종료 요약 + 히스토리 분석 |

---

## 🎯 핵심 선택 가이드

### 용도별 추천

| 용도 | 추천 도구 |
|------|-----------|
| **터미널 렌더링 최적화** | Claude Chill |
| **AI/ML 워크플로우** | Hugging Face Skills |
| **디자인→코드** | Stitch Skills + MCP |
| **Claude Code 확장** | Plugins for Claude Natives |
| **복잡한 프로젝트 관리** | Claude Code Tasks |
| **병렬 태스크 분업** | Cursor 2.4 Subagents |

### 기능 비교

| 기능 | Cursor 2.4 | Claude Code |
|------|------------|-------------|
| **Subagents** | ✅ 기본 제공 | ✅ 수동 정의 |
| **Skills** | ✅ `SKILL.md` | ✅ `SKILL.md` |
| **Tasks** | ❓ 미확인 | ✅ 의존성 지원 |
| **Image Gen** | ✅ 내장 | ❌ |
| **AI Blame** | ✅ Enterprise | ❌ |

---

## 📈 오늘의 핵심 학습

### 1. Subagents & Tasks = 병렬 + 의존성

```
Subagents: "누가" 할 것인가 (병렬 실행)
Tasks: "무엇을" 할 것인가 (의존성 기반)

조합하면:
┌─────────────────────────────────────────────────────┐
│ Task List (의존성 그래프)                            │
│       ↓                                             │
│ Subagent 1 → Task #1, #2                           │
│ Subagent 2 → Task #3, #4 (blocked by #1)           │
│ Subagent 3 → Task #5 (blocked by #3, #4)           │
└─────────────────────────────────────────────────────┘
```

### 2. SKILL.md = 범용 표준

```
Cursor 2.4       ← SKILL.md → Claude Code
     ↑                              ↑
     └─────── 같은 포맷 ─────────────┘
     
Hugging Face Skills, Stitch Skills 모두 채택
```

### 3. 터미널 DX도 중요

```
Claude Code 대규모 출력 문제
→ Claude Chill로 차분 렌더링
→ 룩백 모드로 히스토리 확인
→ 작업 효율 대폭 개선
```

### 4. 멀티 AI 협업 시대

```
agent-council: GPT + Gemini + Codex → 합의 도출
dev 플러그인: 커뮤니티 + AI → 기술 의사결정
```

---

## 🔗 문서 목록

| # | 파일명 | 주제 |
|---|--------|------|
| 01 | `01-claude-chill-pty-proxy-terminal-optimizer.md` | Claude Chill 터미널 최적화 |
| 02 | `02-huggingface-skills-ai-ml-agent-skills.md` | Hugging Face AI/ML 스킬 |
| 03 | `03-cursor-2-4-subagents-skills-release.md` | Cursor 2.4 릴리스 |
| 04 | `04-claude-code-todos-to-tasks-upgrade.md` | Claude Code Tasks 업그레이드 |
| 05 | `05-google-stitch-skills-mcp-ecosystem.md` | Google Stitch Skills & MCP |
| 06 | `06-claude-code-plugins-power-users.md` | Claude Code 플러그인 모음 |

---

> **총 Stars 합계**: ~2.6k+ ⭐  
> **핵심 키워드**: Subagents, Tasks, SKILL.md, 터미널 DX, 멀티 AI 협업
