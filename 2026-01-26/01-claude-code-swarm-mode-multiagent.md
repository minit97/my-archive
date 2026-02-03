# Claude Code Swarm Mode - 숨겨진 멀티 에이전트 팀 모드

> **URL**: https://news.hada.io/topic?id=26100, https://github.com/mikekelly/claude-sneakpeek  
> **유형**: 기능 공개 + GitHub 레포지토리  
> **Stars**: 520 ⭐ | **Forks**: 44  
> **라이선스**: MIT

---

## 요약

Claude Code 내부에 숨겨져 있던 **Swarm 모드**가 공개됨. 단일 AI 코더가 아닌 **팀 리드 역할의 AI**와 상호작용하며, 팀 리드가 계획을 수립하고 **전문화된 워커 에이전트**들이 병렬로 실행하여 구현을 담당. `claude-sneakpeek` 도구로 이 기능을 미리 체험 가능.

> "더 이상 단일 AI 코더와 대화하지 않고, 팀 리드 역할의 AI와 상호작용"

---

## Swarm 모드 작동 방식

```
┌─────────────────────────────────────────────────────────────┐
│  사용자                                                      │
│       │                                                     │
│       ▼                                                     │
│  팀 리드 (Team Lead)                                        │
│  - 직접 코드 작성 X                                          │
│  - 계획 수립 + 작업 분배 + 결과 종합                         │
│       │                                                     │
│       │ 계획 승인 → Delegation Mode 전환                     │
│       │                                                     │
│  ┌────┴────────────────────────────────────────┐           │
│  │              │              │              │            │
│  ▼              ▼              ▼              ▼            │
│  Worker 1      Worker 2      Worker 3      Worker N        │
│  (코드 작성)   (분석)        (수정)        (테스트)         │
│       │              │              │              │        │
│       └──────────────┴──────────────┴──────────────┘        │
│                           │                                 │
│                           ▼                                 │
│                   결과 집계 → 최종 응답                      │
└─────────────────────────────────────────────────────────────┘
```

### 핵심 특징

| 기능 | 설명 |
|------|------|
| **Swarm mode** | 네이티브 멀티 에이전트 오케스트레이션 (`TeammateTool`) |
| **Delegate mode** | Task tool이 백그라운드 에이전트 생성 |
| **Team coordination** | 팀원 간 메시징 + 작업 소유권 관리 |

---

## claude-sneakpeek 설치

### Quick Install

```bash
# 설치
npx @realmikekelly/claude-sneakpeek quick --name claudesp

# PATH 추가 (macOS/Linux)
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc

# 실행
claudesp
```

### 주요 명령어

```bash
# 설치
npx @realmikekelly/claude-sneakpeek quick --name claudesp

# 업데이트
npx @realmikekelly/claude-sneakpeek update claudesp

# 제거
npx @realmikekelly/claude-sneakpeek remove claudesp
```

### 설치 위치

```
~/.claude-sneakpeek/claudesp/
├── npm/           ← 패치된 Claude Code
├── config/        ← 격리된 설정, 세션, MCP 서버
└── variant.json

~/.local/bin/claudesp   ← 래퍼 스크립트
```

> **중요**: 기존 Claude Code와 완전히 분리된 환경으로 실행 (설정, 세션, MCP 서버, 인증 모두 별도)

---

## 지원 프로바이더

| 프로바이더 | 지원 |
|-----------|------|
| **Z.ai** | ✅ |
| **MiniMax** | ✅ |
| **OpenRouter** | ✅ |
| **로컬 모델** | ✅ (cc-mirror 통해) |

---

## 커뮤니티 사용 사례

### 9명 에이전트 팀 구성 사례

레거시 Java → C# .NET 10 포팅 프로젝트에서 Opus 인스턴스로 관리:

```
┌─────────────────────────────────────────────────────────────┐
│  에이전트 구성 (9명 + 7단계 Kanban + Git Worktree)          │
├─────────────────────────────────────────────────────────────┤
│  Manager (Opus 4.5)      : Kanban 상태에 따라 에이전트 호출 │
│  Product Owner (Opus 4.5): 스코프 크리프 방지               │
│  Scrum Master (Opus 4.5) : 백로그 우선순위 + 티켓 배정      │
│  Architect (Sonnet 4.5)  : 설계 전담 (구현 X)               │
│  Archaeologist (Grok)    : 레거시 Java 디컴파일 읽기        │
│  CAB (Opus 4.5)          : 기능 거부 게이트키퍼             │
│  Dev Pair (Sonnet+Haiku) : AD-TDD 루프                     │
│  Librarian (Gemini 2.5)  : 문서 관리 + 회고 트리거          │
└─────────────────────────────────────────────────────────────┘
```

> "비용은 10배지만, 가장 높은 품질의 코드를 얻었다"

### 코디네이터 + 전문가 패턴

```
┌─────────────────────────────────────────────────────────────┐
│  코디네이터 (1명)                                           │
│       │                                                     │
│  ┌────┴────────────────────────────────┐                   │
│  │         │         │         │       │                   │
│  ▼         ▼         ▼         ▼       ▼                   │
│  Backend   Frontend  DB        DevOps  Security            │
│  전문가    전문가    전문가    전문가  전문가               │
└─────────────────────────────────────────────────────────────┘
```

**핵심 이점**: 인지 부하 감소 + 전체 진행 상황 추적

---

## 기존 Sub Agent와의 차이점

| 항목 | 기존 Sub Agent | Swarm Mode |
|------|---------------|------------|
| **추상화 단위** | 대화 단위 | **작업 단위** |
| **컨텍스트 모드** | 일반 | **위임 중심 컨텍스트** |
| **태스크 시스템** | 없음 | **팀 기반 태스크 + 메일박스** |
| **구현 방식** | 플러그인 가능 | **네이티브 통합** |

```
기존: 사용자가 계속 프롬프트를 던져야 함
Swarm: AI가 스스로 작업을 조율 (AI가 다른 AI를 관리)
```

---

## 주의사항 및 한계

### 아직 공개되지 않은 이유

```
모델이 충분히 준비되지 않았다고 Anthropic이 판단
→ 실험적 기능으로 분류
→ 신뢰성/계획 세부사항 부족
```

### 알려진 문제

| 문제 | 예시 |
|------|------|
| **과도한 코드 생성** | 리뷰 어려움, 10~100배 복잡한 코드 가능 |
| **비정상 행동** | nyc 설치 대신 bash로 Istanbul 재구현 시도 |
| **리뷰 필요** | YOLO식 접근은 개인 프로젝트에만 권장 |

### 대응 전략

```bash
# 1. Stack 형태로 변경사항 관리 (Graphite/Cursor)
# 2. 자동 리뷰 에이전트 연동
# 3. CLAUDE.md에서 swarm 사용 조건 지정
```

---

## 버전 정보

- **2.1.9 버전**: 메인 루프가 하위 에이전트를 오케스트레이션하는 방식 변경
- 로그 예시: `"FTSChunkManager agent가 아직 실행 중이지만 진행 중이니 기다리자"`

---

## 2026년 트렌드 전망

```
에이전트 오케스트레이터 = 주요 트렌드
├── 기존 소프트웨어 용어 사용 (팀 리드, 팀 멤버)
├── 이해도/수용성 향상
└── 핵심: 메시징 + 태스크 관리
```

---

## 핵심 포인트

1. **Swarm = 팀 리드 + 워커 에이전트**: 계획/분배/종합은 리드, 구현은 워커
2. **Delegation Mode**: 계획 승인 시 자동 전환, 병렬 작업 수행
3. **완전 격리**: 기존 Claude Code와 별도 환경으로 실행
4. **멀티 프로바이더**: Z.ai, MiniMax, OpenRouter, 로컬 모델 지원
5. **아직 실험적**: Anthropic이 공식 공개하지 않은 기능
6. **비용 vs 품질 트레이드오프**: 10배 비용 → 최고 품질 코드 가능

---

## 참고 자료

- [GeekNews 토픽](https://news.hada.io/topic?id=26100)
- [claude-sneakpeek GitHub](https://github.com/mikekelly/claude-sneakpeek)
- [Swarm 모드 데모 영상](https://x.com/NicerInPerson/status/2014989679796347375)
- [workforest.space](https://workforest.space) - Sub Agent 패턴 정리
