# Everything Claude Code - 완전 가이드

> **경로**: `/study-github/everything-claude-code`  
> **원본**: [github.com/affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code)  
> **저자**: @affaanmustafa (Anthropic 해커톤 우승자)  
> **라이선스**: MIT

---

## 요약

Anthropic 해커톤 우승자가 **10개월 이상** Claude Code를 매일 사용하며 구축한 **프로덕션 레디 설정 모음**입니다. Agents, Skills, Hooks, Commands, Rules, MCP 설정이 모두 포함되어 있습니다.

---

## 전체 구조

```
everything-claude-code/
├── agents/           # 특화된 서브에이전트들
├── skills/           # 워크플로우 정의 및 도메인 지식
├── commands/         # 슬래시 명령어
├── rules/            # 항상 따라야 할 가이드라인
├── hooks/            # 이벤트 기반 자동화
├── contexts/         # 동적 시스템 프롬프트 주입
├── mcp-configs/      # MCP 서버 설정
├── examples/         # 예시 설정 및 세션
└── plugins/          # 플러그인 문서
```

---

## 1. Agents (서브에이전트)

### 사용 가능한 에이전트

| Agent | 목적 | 사용 시점 |
|-------|------|----------|
| `planner` | 구현 계획 수립 | 복잡한 기능, 리팩토링 |
| `architect` | 시스템 설계 | 아키텍처 결정 |
| `tdd-guide` | 테스트 주도 개발 | 새 기능, 버그 수정 |
| `code-reviewer` | 코드 리뷰 | 코드 작성 후 |
| `security-reviewer` | 보안 분석 | 커밋 전 |
| `build-error-resolver` | 빌드 에러 해결 | 빌드 실패 시 |
| `e2e-runner` | E2E 테스트 | 크리티컬 사용자 플로우 |
| `refactor-cleaner` | 죽은 코드 정리 | 코드 유지보수 |
| `doc-updater` | 문서 업데이트 | 문서 갱신 |

### 에이전트 정의 형식

```markdown
---
name: code-reviewer
description: 코드 품질, 보안, 유지보수성 리뷰
tools: Read, Grep, Glob, Bash
model: opus
---

You are a senior code reviewer...
```

### 병렬 에이전트 실행

```markdown
# GOOD: 병렬 실행
3개 에이전트를 동시에 실행:
1. Agent 1: auth.ts 보안 분석
2. Agent 2: 캐시 시스템 성능 리뷰
3. Agent 3: utils.ts 타입 체크

# BAD: 불필요한 순차 실행
에이전트 1 → 에이전트 2 → 에이전트 3
```

---

## 2. Skills (워크플로우 정의)

### 사용 가능한 스킬

| 스킬 | 용도 |
|------|------|
| `coding-standards.md` | 언어별 베스트 프랙티스 |
| `backend-patterns.md` | API, 데이터베이스, 캐싱 패턴 |
| `frontend-patterns.md` | React, Next.js 패턴 |
| `tdd-workflow/` | TDD 방법론 |
| `security-review/` | 보안 체크리스트 |
| `continuous-learning/` | 세션에서 패턴 자동 추출 |
| `strategic-compact/` | 수동 컴팩션 제안 |

### TDD 워크플로우 예시

```
RED → GREEN → REFACTOR → REPEAT

RED:      실패하는 테스트 작성
GREEN:    최소 코드로 통과
REFACTOR: 코드 개선, 테스트 유지
REPEAT:   다음 기능/시나리오
```

### 커버리지 요구사항

| 대상 | 최소 커버리지 |
|------|--------------|
| 모든 코드 | 80% |
| 금융 계산 | 100% |
| 인증 로직 | 100% |
| 보안 관련 | 100% |
| 핵심 비즈니스 로직 | 100% |

---

## 3. Commands (슬래시 명령어)

| 명령어 | 용도 | 호출 에이전트/스킬 |
|--------|------|-------------------|
| `/tdd` | TDD 워크플로우 | tdd-guide 에이전트 |
| `/plan` | 구현 계획 | planner 에이전트 |
| `/e2e` | E2E 테스트 생성 | e2e-runner 에이전트 |
| `/code-review` | 품질 리뷰 | code-reviewer 에이전트 |
| `/build-fix` | 빌드 에러 수정 | build-error-resolver |
| `/refactor-clean` | 죽은 코드 제거 | refactor-cleaner |
| `/learn` | 세션 중 패턴 추출 | continuous-learning 스킬 |
| `/test-coverage` | 커버리지 확인 | - |
| `/update-docs` | 문서 업데이트 | doc-updater 에이전트 |

---

## 4. Rules (항상 따라야 할 규칙)

### 규칙 파일들

| 파일 | 내용 |
|------|------|
| `security.md` | 필수 보안 체크 |
| `coding-style.md` | 불변성, 파일 구조 |
| `testing.md` | TDD, 80% 커버리지 요구 |
| `git-workflow.md` | 커밋 형식, PR 프로세스 |
| `agents.md` | 에이전트 위임 시점 |
| `performance.md` | 모델 선택, 컨텍스트 관리 |
| `patterns.md` | 코딩 패턴 |
| `hooks.md` | 훅 사용법 |

### 보안 체크리스트

커밋 전 **필수** 확인:
- [ ] 하드코딩된 시크릿 없음 (API 키, 비밀번호, 토큰)
- [ ] 모든 사용자 입력 검증됨
- [ ] SQL 인젝션 방지 (파라미터화된 쿼리)
- [ ] XSS 방지 (HTML 살균)
- [ ] CSRF 보호 활성화
- [ ] 인증/인가 검증됨
- [ ] 모든 엔드포인트에 레이트 리밋
- [ ] 에러 메시지에 민감 데이터 노출 없음

---

## 5. Hooks (이벤트 기반 자동화)

### 훅 타입

| 훅 | 트리거 시점 | 용도 |
|----|-------------|------|
| `PreToolUse` | 도구 실행 전 | 차단, 경고 |
| `PostToolUse` | 도구 실행 후 | 자동 포맷팅, 체크 |
| `PreCompact` | 컨텍스트 컴팩션 전 | 상태 저장 |
| `SessionStart` | 세션 시작 시 | 이전 컨텍스트 로드 |
| `Stop` | Claude 응답 완료 시 | 최종 감사, 상태 저장 |

### 주요 훅 예시

#### 1. Dev 서버 tmux 강제

```json
{
  "matcher": "tool == \"Bash\" && tool_input.command matches \"npm run dev\"",
  "hooks": [{
    "type": "command",
    "command": "echo '[Hook] BLOCKED: Dev server must run in tmux' >&2; exit 1"
  }],
  "description": "tmux 없이 dev 서버 실행 차단"
}
```

#### 2. TS 파일 자동 포맷팅

```json
{
  "matcher": "tool == \"Edit\" && tool_input.file_path matches \"\\.(ts|tsx)$\"",
  "hooks": [{
    "type": "command",
    "command": "prettier --write \"$file_path\""
  }],
  "description": "JS/TS 파일 수정 후 자동 Prettier"
}
```

#### 3. console.log 경고

```json
{
  "matcher": "tool == \"Edit\" && tool_input.file_path matches \"\\.(ts|tsx|js|jsx)$\"",
  "hooks": [{
    "type": "command",
    "command": "grep -n 'console\\.log' \"$file_path\" && echo '[Hook] Remove console.log'"
  }],
  "description": "수정된 파일에 console.log 경고"
}
```

### 메모리 지속성 훅

```
SessionStart → 이전 컨텍스트 로드
PreCompact   → 컴팩션 전 상태 저장
Stop         → 세션 종료 시 상태 저장 + 패턴 추출
```

---

## 6. MCP 설정

### 사용 가능한 MCP 서버

| MCP | 용도 |
|-----|------|
| `github` | PR, 이슈, 레포 관리 |
| `supabase` | 데이터베이스 작업 |
| `vercel` | 배포 및 프로젝트 |
| `railway` | 배포 |
| `cloudflare-docs` | Cloudflare 문서 검색 |
| `cloudflare-workers-*` | Workers 빌드, 바인딩, 로그 |
| `clickhouse` | 분석 쿼리 |
| `firecrawl` | 웹 스크래핑 |
| `memory` | 세션 간 지속 메모리 |
| `sequential-thinking` | 체인 오브 쏘트 추론 |
| `context7` | 실시간 문서 조회 |

### 컨텍스트 경고

```
┌─────────────────────────────────────────────────────────────┐
│  "20-30개 MCP 설정, 10개 미만 활성화, 80개 미만 도구"         │
└─────────────────────────────────────────────────────────────┘
```

- 너무 많은 MCP 활성화 → 200k 컨텍스트가 70k로 축소
- 프로젝트별 `disabledMcpServers`로 불필요한 것 비활성화

---

## 7. Contexts (동적 시스템 프롬프트)

| 컨텍스트 | 용도 |
|----------|------|
| `dev.md` | 개발 모드 |
| `review.md` | 코드 리뷰 모드 |
| `research.md` | 탐색/리서치 모드 |

---

## 8. 빠른 시작

### 1. 파일 복사

```bash
# 클론
git clone https://github.com/affaan-m/everything-claude-code.git

# 에이전트 복사
cp everything-claude-code/agents/*.md ~/.claude/agents/

# 규칙 복사
cp everything-claude-code/rules/*.md ~/.claude/rules/

# 명령어 복사
cp everything-claude-code/commands/*.md ~/.claude/commands/

# 스킬 복사
cp -r everything-claude-code/skills/* ~/.claude/skills/
```

### 2. 훅 추가

`hooks/hooks.json`의 내용을 `~/.claude/settings.json`에 추가

### 3. MCP 설정

`mcp-configs/mcp-servers.json`에서 필요한 서버를 `~/.claude.json`에 복사

⚠️ `YOUR_*_HERE` 플레이스홀더를 실제 API 키로 교체

---

## 9. 코드 리뷰 체크리스트

### 우선순위별 분류

#### CRITICAL (반드시 수정)
- 하드코딩된 자격 증명
- SQL 인젝션 위험
- XSS 취약점
- 입력 검증 누락
- 취약한 의존성
- 인증 우회

#### HIGH (수정 필요)
- 큰 함수 (>50 줄)
- 큰 파일 (>800 줄)
- 깊은 중첩 (>4 레벨)
- 에러 핸들링 누락
- console.log 남아있음
- 새 코드에 테스트 누락

#### MEDIUM (개선 권장)
- 비효율적 알고리즘
- React 불필요한 리렌더링
- 메모이제이션 누락
- 번들 사이즈 큼
- N+1 쿼리
- 접근성 이슈

### 승인 기준

| 상태 | 조건 |
|------|------|
| ✅ 승인 | CRITICAL/HIGH 이슈 없음 |
| ⚠️ 경고 | MEDIUM 이슈만 |
| ❌ 차단 | CRITICAL/HIGH 이슈 발견 |

---

## 10. 가이드 문서

| 가이드 | 내용 |
|--------|------|
| **Shorthand Guide** | 설정과 기초 (먼저 읽기) |
| **Longform Guide** | 고급 기법 (토큰 최적화, 메모리 지속성, 평가, 병렬화) |

### Longform Guide 주제

| 주제 | 내용 |
|------|------|
| 토큰 최적화 | 모델 선택, 시스템 프롬프트 슬리밍 |
| 메모리 지속성 | 세션 간 컨텍스트 자동 저장/로드 |
| 지속적 학습 | 세션에서 패턴 자동 추출 |
| 검증 루프 | 체크포인트 vs 연속 평가, pass@k |
| 병렬화 | Git worktrees, cascade 방식 |
| 서브에이전트 오케스트레이션 | 컨텍스트 문제, 반복 검색 패턴 |

---

## 핵심 포인트

1. **3계층 자동화**: Agents + Skills + Hooks
2. **TDD 강제**: 테스트 먼저, 80%+ 커버리지
3. **보안 우선**: 커밋 전 필수 체크리스트
4. **컨텍스트 경제학**: MCP 10개 미만 활성화
5. **병렬 실행**: 독립적 작업은 동시에
6. **메모리 지속성**: 세션 간 컨텍스트 유지

---

## 관련 문서

- [07-claude-code-hackathon-winner-guide.md](./07-claude-code-hackathon-winner-guide.md) - 해설 요약
- [06-superpowers-for-cursor-guide.md](./06-superpowers-for-cursor-guide.md) - Cursor용 Superpowers
- [05-ralph-claude-code-implementation.md](./05-ralph-claude-code-implementation.md) - Ralph 구현체

---

## 인용

> "자동화를 도구로 보면 '이 도구가 저 작업을 빠르게 해준다'에서 사고가 멈춘다. 자동화를 계층으로 보면 '이 작업 전체를 어떻게 구조화할 것인가'로 질문이 확장된다."

> "컨텍스트는 희소 자원이고, 희소 자원의 관리가 시스템 성능을 결정한다."
