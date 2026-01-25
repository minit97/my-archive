# Everything Claude Code - 해커톤 우승자의 AI 개발 팀 레시피 (Distilled)

> **URL**: https://roboco.io/posts/everything-claude-code-distilled/  
> **유형**: 기술 분석 블로그  
> **작성자**: 정도현 (로보코 수석 컨설턴트)  
> **발행일**: 2026-01-20

---

## 요약

Everything Claude Code는 Claude Code CLI를 **가상 개발 팀 환경**으로 변모시키는 설정 모음집이다. Anthropic x Forum Ventures 해커톤 우승자(Affaan Mustafa)가 10개월간 실무에서 다듬은 레시피로, Claude를 "시니어 엔지니어 + QA + 아키텍트"로 호출할 수 있게 해준다.

> "AI를 팀으로 받아들이려면, 도구보다 프로세스를 먼저 설계해야 한다."  
> — Affaan Mustafa

---

## 핵심 철학: 3가지 가치 제안

```
┌─────────────────────────────────────────────────────────────┐
│  1. 역할 분리                                               │
│     → 역할별 프롬프트, 툴 권한, 톤을 분리해 LLM 집중도 향상   │
├─────────────────────────────────────────────────────────────┤
│  2. 프로세스 표준화                                         │
│     → /plan, /tdd, /code-review 등 슬래시 명령으로 루틴 자동화│
├─────────────────────────────────────────────────────────────┤
│  3. 품질 가드레일                                           │
│     → 규칙·스킬·훅을 조합해 보안, 테스트, 스타일 강제        │
└─────────────────────────────────────────────────────────────┘
```

---

## 저장소 구조와 아키텍처

### 2.1 Agents (역할 특화 프롬프트)

| 에이전트 | 역할 |
|----------|------|
| `planner` | 기능 계획 수립 |
| `architect` | 시스템 설계 결정 |
| `code-reviewer` | 코드 품질 리뷰 |
| `security-reviewer` | 보안 취약점 분석 |
| `tdd-guide` | TDD 워크플로우 가이드 |
| `build-error-resolver` | 빌드 오류 해결 |
| `e2e-runner` | E2E 테스트 실행 |
| `refactor-cleaner` | 리팩토링 및 정리 |
| `doc-updater` | 문서 동기화 |

> **설계 핵심**: 각 에이전트에 **툴을 최소화**해 집중도를 높이고, 에이전트 간 역할 충돌 방지

### 2.2 Skills (업무 매뉴얼)

```
skills/
├── coding-standards.md      # 코딩 표준
├── backend-patterns.md      # API·DB·캐싱 베스트 프랙티스
├── frontend-patterns.md     # React/Next.js 지침
├── security-review/         # 보안 리뷰 체크리스트
├── tdd-workflow/            # RED→GREEN→REFACTOR + 80% 커버리지
└── clickhouse-io.md         # 특정 기술 스킬
```

- **전사 공통**: `~/.claude/skills`
- **프로젝트 전용**: `.claude/skills`

### 2.3 Commands (슬래시 명령)

| 명령 | 역할 |
|------|------|
| `/plan` | 기능 계획 수립 |
| `/tdd` | TDD 워크플로우 실행 |
| `/e2e` | E2E 테스트 실행 |
| `/code-review` | 코드 리뷰 |
| `/build-fix` | 빌드 오류 해결 |
| `/refactor-clean` | 리팩토링 |
| `/test-coverage` | 테스트 커버리지 확인 |
| `/update-docs` | 문서 업데이트 |

**개발 플로우**: `/plan` → `/tdd` → `/code-review` → `/update-docs`

### 2.4 Rules (가드레일)

시스템 프롬프트에 **자동 삽입**되는 항상 적용 규칙:
- `security.md` - 보안 규칙
- `coding-style.md` - 코딩 스타일
- `testing.md` - "커버리지 80% 미만 PR 금지"
- `git-workflow.md` - Git 워크플로우
- `performance.md` - 성능 가이드라인

### 2.5 Hooks (자동화 스크립트)

```json
// hooks/hooks.json 예시
{
  "PostToolUse": {
    "matcher": "*.ts",
    "script": "check-console-log.sh"  // console.log 남아있으면 경고
  },
  "SessionEnd": {
    "script": "run-formatter.sh"      // 세션 종료 전 포맷터 실행
  }
}
```

---

## 해커톤 우승 비결: 5가지 차별성

```
┌─────────────────────────────────────────────────────────────┐
│  ① 역할 병렬화                                              │
│     설계·구현·테스트를 서로 다른 에이전트가 동시에 처리       │
├─────────────────────────────────────────────────────────────┤
│  ② 표준화된 개발 루틴                                       │
│     /plan → /tdd → /code-review → /update-docs 자동화       │
├─────────────────────────────────────────────────────────────┤
│  ③ 풀스택 자동화 (MCP)                                      │
│     GitHub 이슈, Supabase 쿼리, Vercel 배포 직접 수행        │
├─────────────────────────────────────────────────────────────┤
│  ④ 훅 기반 안전장치                                         │
│     console.log 제거, 테스트 누락 경고로 품질 유지           │
├─────────────────────────────────────────────────────────────┤
│  ⑤ 전투 검증된 노하우                                       │
│     실무에서 반복 검증한 "다듬어진 프레임워크"               │
└─────────────────────────────────────────────────────────────┘
```

---

## 기술 스택 편향

| 대상 | 최적화 여부 |
|------|-------------|
| **TypeScript 풀스택** | ✅ 첫 번째 타깃 |
| React/Next.js | ✅ frontend-patterns.md |
| Jest/Vitest/Playwright | ✅ e2e, testing 명령 |
| Node/Vite 빌드 | ✅ /build-fix |
| 다른 언어/도메인 | ⚠️ 추가 스킬 작성 필요 |

---

## 기술적 한계와 개선 방향

| 한계 | 설명 | 개선 방향 |
|------|------|-----------|
| **컨텍스트 창 한계** | MCP·규칙·스킬 과다 시 토큰 부족 | 동적 컨텍스트 로딩, 필요 시점별 설정 교체 |
| **모델 비용** | Opus 모델은 느리고 비용 높음 | Sonnet 자동 폴백, 멀티 모델 추상화 |
| **온보딩 난도** | 에이전트·스킬·훅 개념 학습 곡선 높음 | 대화형 설정 마법사, GUI 기반 관리 |
| **도메인 편중** | JS/TS 웹 서비스에 최적화 | 커뮤니티의 다른 도메인 스킬 추가 |
| **실행 안전성** | Bash 권한 에이전트의 잘못된 명령 리스크 | 2단계 승인, 샌드박스, `/dry-run` |
| **성능 최적화** | AI 설명 실행 시 속도 저하 | 응답 캐싱, 에이전트 간 결과 공유 |

---

## 핵심 인사이트

### "AI 운영체제"로서의 관점

> Everything Claude Code는 단순한 설정 파일 모음이 아니라 **AI와 함께 일하는 방식을 설계한 운영체제**에 가깝다.

```
┌─────────────────────────────────────────────────────────────┐
│  역할 단위 프롬프트 + 테스트 중심 규칙 + 훅 기반 자동화     │
│                           +                                 │
│                    MCP 연동                                 │
│                           ↓                                 │
│       "AI를 어떻게 팀원으로 쓸 것인가"에 대한 구체적 답      │
└─────────────────────────────────────────────────────────────┘
```

### 기존 08번 문서와의 관계

| 문서 | 내용 |
|------|------|
| `08-everything-claude-code-complete-guide.md` | 레포지토리 구조 직접 분석 |
| **본 문서 (14번)** | 로보코 컨설턴트의 해설 + 한계 분석 |

---

## 핵심 포인트

1. **도구보다 프로세스**: AI를 팀으로 받아들이려면 프로세스 설계가 먼저
2. **역할 분리의 힘**: 툴 최소화로 에이전트 집중도 향상
3. **표준화된 워크플로우**: 슬래시 명령으로 개발 루틴 버튼화
4. **품질 가드레일**: Rules + Hooks로 실수 조기 차단
5. **실전 검증**: 해커톤 우승 + 10개월 실무 경험

---

## 참고 자료

- [Everything Claude Code Repository (GitHub)](https://github.com/affaan-m/everything-claude-code)
- [Tilnote: Everything Claude Code 정리](https://tilnote.io/en/pages/696db2d265a2e4dd63f35cc7)
- [JP Caparas: The Claude Code setup that won a hackathon](https://jpcaparas.medium.com/the-claude-code-setup-that-won-a-hackathon-a75a161cd41c)
