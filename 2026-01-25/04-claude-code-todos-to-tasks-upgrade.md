# Claude Code - Todos에서 Tasks로 업그레이드

> **URL**: https://x.com/trq212/status/2014480496013803643  
> **유형**: 제품 업데이트 공지 (Anthropic 공식)  
> **발행일**: 2026년 1월  
> **관련**: Claude Code, Opus 4.5

---

## 요약

Anthropic이 Claude Code의 **Todos를 Tasks로 업그레이드**. Tasks는 복잡한 프로젝트 추적, 멀티 세션/서브에이전트 간 협업을 위한 새로운 프리미티브. 모델 성능 향상(Opus 4.5)으로 단순 작업에는 TodoWrite가 불필요해졌고, 장기 프로젝트에는 더 강력한 추상화가 필요해짐.

> "Tasks are a new primitive that help Claude Code track and complete more complicated projects and collaborate on them across multiple sessions or subagents."

---

## 변경 배경

### 1. 모델 능력 향상 (Opus 4.5)

```
이전 모델: TodoWrite Tool 필요 (상태 추적용)
         ↓
Opus 4.5: 자율 실행 시간 ↑, 상태 추적 능력 ↑
         ↓
결과: 작은 작업에는 TodoWrite 불필요
```

### 2. 장기 프로젝트 요구 증가

| 기존 Todos 한계 | Tasks로 해결 |
|-----------------|--------------|
| 단순 체크리스트 | **의존성(Dependencies)** 지원 |
| 단일 세션 | **멀티 세션** 협업 |
| 상태 공유 불가 | **서브에이전트 간** 공유 |
| 휘발성 | **파일 시스템** 저장 |

---

## Todos vs Tasks 비교

| 항목 | Todos (기존) | Tasks (신규) |
|------|-------------|--------------|
| **저장** | 메모리 | `~/.claude/tasks/` |
| **의존성** | ❌ | ✅ `blocked by #1, #7` |
| **멀티 세션** | ❌ | ✅ 브로드캐스트 |
| **서브에이전트** | ❌ | ✅ 협업 가능 |
| **영속성** | 세션 종료 시 소멸 | 파일로 유지 |

---

## Tasks 작동 방식

### 스크린샷 예시

```
Tasks (4 done, 6 open) · ctrl+t to hide tasks
□ #1  Buy fresh pasta and marinara sauce
✓ #2  Buy chicken breasts and spice rub
□ #3  Buy salad greens, croutons, and Caesar dressing
✓ #4  Buy sourdough bread and butter
✓ #5  Buy lemons and olive oil
✓ #6  Marinate chicken for grilling
□ #7  Make garlic bread
□ #8  Prepare Caesar salad > blocked by #3
□ #9  Grill marinated chicken
□ #10 Host dinner party > blocked by #1, #7, #8, #9
```

### 핵심 기능

| 기능 | 설명 |
|------|------|
| **의존성** | `blocked by #3` → #3 완료 전까지 대기 |
| **진행 상태** | `4 done, 6 open` 실시간 추적 |
| **토글** | `ctrl+t`로 Tasks 패널 숨기기/보기 |
| **취소선** | 완료된 태스크 시각적 표시 |

---

## 사용 방법

### 1. Tasks 생성

```
"이 프로젝트를 Tasks로 분해해줘"
"서브에이전트 시작 전에 Tasks 만들어줘"
```

### 2. 멀티 세션 협업

```bash
# 환경변수로 Task List 지정
CLAUDE_CODE_TASK_LIST_ID=groceries claude

# -p 플래그와 함께
CLAUDE_CODE_TASK_LIST_ID=my-project claude -p "Continue task #7"
```

### 3. AgentSDK에서도 동작

```javascript
// AgentSDK에서 Task List 공유
const agent = new Agent({
  taskListId: "my-project"
});
```

---

## 저장 위치 및 확장성

```
~/.claude/tasks/
├── groceries.json        ← Task List 파일
├── my-project.json
└── ...
```

**확장 가능**: 이 파일을 기반으로 **커스텀 유틸리티** 구축 가능

---

## 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│  Project                                                    │
│       │                                                     │
│  Task List (~/.claude/tasks/project.json)                   │
│       │                                                     │
│  ┌────┴────────────────────────────────────────┐           │
│  │                                              │           │
│  ▼                                              ▼           │
│  Session 1                                 Session 2        │
│  (Main Agent)                              (Subagent)       │
│       │                                         │           │
│       └─────────── 브로드캐스트 ────────────────┘           │
│                                                             │
│  Task #1 업데이트 → 모든 세션에 즉시 반영                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 영감 및 참고

| 프로젝트 | 기여 |
|----------|------|
| **Beads by Steve Yegge** | Tasks 개념 영감 |
| **커뮤니티 피드백** | 장기 프로젝트 협업 요구 |

---

## 주요 사용 사례

### 1. 서브에이전트 스핀업

```
"이 기능 구현을 위해 서브에이전트 3개 생성하고 Tasks로 조율해줘"
```

### 2. 장기 프로젝트

```
"이 리팩토링 프로젝트를 Tasks로 나누고 의존성 설정해줘"
```

### 3. 팀 협업 (여러 세션)

```bash
# 터미널 1
CLAUDE_CODE_TASK_LIST_ID=feature-x claude

# 터미널 2 (같은 Task List 공유)
CLAUDE_CODE_TASK_LIST_ID=feature-x claude
```

---

## 핵심 포인트

1. **Todos → Tasks 진화**: 단순 체크리스트에서 의존성 기반 프로젝트 관리로
2. **모델 향상 반영**: Opus 4.5의 자율성/상태 추적 능력 활용
3. **멀티 세션 협업**: 브로드캐스트로 Task 상태 실시간 동기화
4. **서브에이전트 지원**: 여러 에이전트가 같은 Task List 공유
5. **파일 기반 저장**: `~/.claude/tasks/`에 영속적 저장
6. **확장 가능**: 커스텀 유틸리티 구축 가능
7. **환경변수 설정**: `CLAUDE_CODE_TASK_LIST_ID`로 세션 간 공유

---

## 참고 자료

- [X 게시물 원문](https://x.com/trq212/status/2014480496013803643)
- [Beads by Steve Yegge](https://github.com/steveyegge/beads) (영감을 준 프로젝트)
