# Claude Code Agent Teams - 멀티 에이전트 팀 오케스트레이션

> **URL**: https://code.claude.com/docs/en/agent-teams  
> **유형**: 공식 문서 (Anthropic)  
> **상태**: 실험적 (Experimental, 기본 비활성화)

---

## 요약

Claude Code에 새로운 기능인 **Agent Teams**가 실험적으로 도입됐습니다. 기존에는 하나의 에이전트가 순차적으로 작업을 처리했지만, 이제는 **리드 에이전트가 여러 팀원 에이전트에게 작업을 분배**해 병렬로 리서치, 디버깅, 개발을 수행할 수 있습니다. 실제 개발 팀처럼 역할을 나누고 협업하는 구조로, 복잡한 작업을 효율적으로 처리할 수 있게 됩니다.

> "Coordinate multiple Claude Code instances working together as a team"

---

## 활성화 방법

Agent Teams는 기본 비활성화. `settings.json`에서 활성화:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

---

## 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│  사용자                                                      │
│       │                                                     │
│       ▼                                                     │
│  Team Lead (메인 Claude Code 세션)                           │
│  - 팀 생성, 작업 분배, 결과 종합                             │
│       │                                                     │
│  ┌────┴──────────────────────────────────┐                  │
│  │           │           │           │   │                  │
│  ▼           ▼           ▼           ▼   ▼                  │
│  Teammate 1  Teammate 2  Teammate 3  ... Teammate N         │
│  (독립 세션)  (독립 세션)  (독립 세션)     (독립 세션)         │
│       │           │           │           │                  │
│       └───────────┴───────────┴───────────┘                  │
│                       │                                     │
│              Shared Task List + Mailbox                      │
└─────────────────────────────────────────────────────────────┘
```

### 핵심 컴포넌트

| 컴포넌트 | 역할 |
|----------|------|
| **Team Lead** | 팀 생성, 팀원 스폰, 작업 조율 |
| **Teammates** | 각자 독립 Claude Code 인스턴스로 작업 수행 |
| **Task List** | 팀원들이 클레임하고 완료하는 공유 작업 목록 |
| **Mailbox** | 에이전트 간 메시징 시스템 |

### 저장 위치

```
~/.claude/teams/{team-name}/config.json    ← 팀 설정
~/.claude/tasks/{team-name}/               ← 태스크 리스트
```

---

## Subagents vs Agent Teams

| 항목 | Subagents | Agent Teams |
|------|-----------|-------------|
| **컨텍스트** | 자체 윈도우, 결과를 호출자에게 반환 | 자체 윈도우, 완전 독립 |
| **소통** | 메인 에이전트에게만 보고 | **팀원끼리 직접 메시징** |
| **조율** | 메인 에이전트가 모든 작업 관리 | **공유 태스크 리스트 + 자율 조율** |
| **적합한 경우** | 결과만 필요한 집중 작업 | **토론과 협업이 필요한 복잡한 작업** |
| **토큰 비용** | 낮음 (결과 요약 후 반환) | **높음** (각 팀원이 별도 인스턴스) |

> **핵심 차이**: Subagents는 보고만, Agent Teams는 **서로 소통하고 도전하고 협업**

---

## 최적 사용 사례

| 사용 사례 | 설명 |
|-----------|------|
| **리서치 & 리뷰** | 여러 팀원이 문제의 다른 측면을 동시 조사 |
| **새 모듈/기능** | 각 팀원이 별도 파트 담당 |
| **경쟁 가설 디버깅** | 서로 다른 이론을 병렬 테스트 후 수렴 |
| **크로스 레이어** | 프론트엔드, 백엔드, 테스트를 각각 담당 |

### 사용하지 않을 때

- 순차 작업, 같은 파일 수정, 의존성 많은 작업 → 단일 세션 또는 Subagents 추천

---

## 디스플레이 모드

| 모드 | 설명 | 요구사항 |
|------|------|----------|
| **In-process** | 메인 터미널에서 모든 팀원 실행 | 없음 (어떤 터미널이든) |
| **Split panes** | 각 팀원이 별도 패인 | tmux 또는 iTerm2 |

```json
// settings.json
{
  "teammateMode": "in-process"  // 또는 "tmux", "auto"
}
```

```bash
# 단일 세션 강제 설정
claude --teammate-mode in-process
```

### 키보드 단축키 (In-process 모드)

| 키 | 기능 |
|----|------|
| **Shift+Up/Down** | 팀원 선택 |
| **Enter** | 팀원 세션 보기 |
| **Escape** | 팀원 현재 턴 중단 |
| **Ctrl+T** | 태스크 리스트 토글 |
| **Shift+Tab** | Delegate 모드 전환 |

---

## 팀 시작하기

```
I'm designing a CLI tool that helps developers track TODO comments across
their codebase. Create an agent team to explore this from different angles:
one teammate on UX, one on technical architecture, one playing devil's advocate.
```

Claude가 자동으로:
1. 공유 태스크 리스트 생성
2. 각 관점별 팀원 스폰
3. 문제 탐색
4. 결과 종합
5. 팀 정리

---

## 핵심 제어 기능

### 1. Delegate Mode

리드가 직접 구현하지 않고 **조율에만 집중**하도록 제한:

```
Shift+Tab → Delegate 모드 전환
```

리드의 도구: 스폰, 메시징, 셧다운, 태스크 관리만 가능

### 2. Plan Approval

팀원이 구현 전에 계획을 먼저 세우도록 요구:

```
Spawn an architect teammate to refactor the authentication module.
Require plan approval before they make any changes.
```

```
계획 수립 (Read-only) → 리드 승인/거부 → 승인 시 구현 시작
```

### 3. 태스크 관리

| 상태 | 설명 |
|------|------|
| **Pending** | 대기 중 |
| **In Progress** | 진행 중 |
| **Completed** | 완료 |

- **리드 할당**: 특정 팀원에게 태스크 지정
- **자율 클레임**: 팀원이 다음 미할당 태스크 자동 선택
- **의존성**: 선행 태스크 완료 전까지 차단
- **파일 잠금**: 동시 클레임 경쟁 조건 방지

### 4. 팀원에게 직접 대화

각 팀원은 독립된 Claude Code 세션 → 직접 지시, 질문, 방향 전환 가능

---

## 실전 사용 예시

### 병렬 코드 리뷰

```
Create an agent team to review PR #142. Spawn three reviewers:
- One focused on security implications
- One checking performance impact  
- One validating test coverage
Have them each review and report findings.
```

### 경쟁 가설 디버깅

```
Users report the app exits after one message instead of staying connected.
Spawn 5 agent teammates to investigate different hypotheses. Have them talk to
each other to try to disprove each other's theories, like a scientific debate.
Update the findings doc with whatever consensus emerges.
```

> **핵심**: 단일 에이전트는 첫 번째 그럴듯한 설명에 고착 (앵커링). 여러 독립 조사자가 서로 반박하면, 살아남는 이론이 **실제 원인일 가능성이 높음**

---

## 소통 구조

```
┌─────────────────────────────────────────────────────────────┐
│  자동 메시지 전달: 팀원 → 수신자 (리드 폴링 불필요)          │
│  유휴 알림: 팀원 완료 시 리드에게 자동 통지                   │
│  공유 태스크 리스트: 모든 에이전트가 상태 확인 + 작업 클레임  │
├─────────────────────────────────────────────────────────────┤
│  메시징 방식:                                                │
│  - message: 특정 팀원 1명에게 전송                           │
│  - broadcast: 전체 팀원에게 전송 (비용 주의)                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 베스트 프랙티스

| 원칙 | 설명 |
|------|------|
| **충분한 컨텍스트** | 스폰 프롬프트에 태스크별 세부 내용 포함 |
| **적절한 태스크 크기** | 너무 작으면 오버헤드, 너무 크면 낭비 → 팀원당 5-6개 태스크 |
| **팀원 완료 대기** | 리드가 직접 구현 시작하면 "Wait for teammates" 지시 |
| **리서치/리뷰부터** | 처음에는 코드 수정 없는 작업으로 시작 |
| **파일 충돌 방지** | 각 팀원이 다른 파일 세트 담당 |
| **모니터링** | 진행 상황 확인, 방향 수정, 결과 종합 |

---

## 팀 정리

```bash
# 팀원 셧다운
"Ask the researcher teammate to shut down"

# 팀 리소스 정리 (반드시 리드가 실행)
"Clean up the team"
```

> 항상 **리드가 정리** 실행. 팀원이 실행하면 리소스 불일치 위험

---

## 알려진 제한사항

| 제한 | 설명 |
|------|------|
| **세션 복원 불가** | `/resume`, `/rewind` 시 in-process 팀원 미복원 |
| **태스크 상태 지연** | 팀원이 완료 마킹 실패 시 의존 태스크 차단 |
| **셧다운 느림** | 현재 요청/도구 호출 완료 후 종료 |
| **세션당 1팀** | 새 팀 시작 전 현재 팀 정리 필요 |
| **중첩 팀 불가** | 팀원이 자체 팀 생성 불가 |
| **리드 고정** | 리더십 이전/팀원 승격 불가 |
| **권한 스폰 시 설정** | 전체 팀원 동일 권한, 개별 설정은 스폰 후만 |
| **Split pane 제한** | VS Code 통합 터미널, Windows Terminal, Ghostty 미지원 |

---

## 토큰 사용량 주의

```
Agent Teams = 팀원 수 × 독립 컨텍스트 윈도우
            = 단일 세션 대비 훨씬 높은 토큰 소모

추천:
├── 리서치/리뷰/새 기능 → Agent Teams (토큰 투자 가치 있음)
└── 루틴 작업 → 단일 세션이 비용 효율적
```

---

## 핵심 포인트

1. **실제 개발 팀 구조**: 리드(조율) + 팀원(구현)으로 역할 분리
2. **Subagents와 차별화**: 팀원끼리 직접 소통, 서로 도전, 자율 협업
3. **Delegate Mode**: 리드가 코드 작성 없이 오케스트레이션에만 집중
4. **Plan Approval**: 복잡한 작업에서 구현 전 계획 승인 요구 가능
5. **경쟁 가설 디버깅**: 앵커링 편향 방지를 위한 과학적 토론 구조
6. **토큰 주의**: 팀원 수에 비례해 비용 증가, 복잡한 작업에만 활용 권장
7. **아직 실험적**: 세션 복원, 태스크 조율 등에 알려진 제한 존재

---

## 참고 자료

- [Claude Code Agent Teams 공식 문서](https://code.claude.com/docs/en/agent-teams)
- [Subagents 문서](https://code.claude.com/docs/en/sub-agents)
- [Git Worktrees 병렬 세션](https://code.claude.com/docs/en/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees)
