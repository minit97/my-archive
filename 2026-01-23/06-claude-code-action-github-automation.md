# Claude Code Action - GitHub PR/Issue AI 자동화

> **URL**: https://github.com/anthropics/claude-code-action  
> **유형**: GitHub 레포지토리 (공식 GitHub Action)  
> **작성자**: Anthropic (공식)  
> **라이선스**: MIT  
> **Stars**: 5.3k ⭐ | **Forks**: 1.4k

---

## 요약

Anthropic에서 공식 제공하는 **GitHub Actions용 Claude Code**. PR과 Issue에서 질문 응답, 코드 리뷰, 코드 구현까지 자동으로 수행한다. `@claude` 멘션, Issue 할당, 자동화 트리거에 지능적으로 반응하며, Anthropic API, AWS Bedrock, Google Vertex AI, Microsoft Foundry 등 다양한 인증 방식 지원.

> "Your infrastructure, your runner, Claude's intelligence."

---

## 핵심 기능

```
┌─────────────────────────────────────────────────────────────┐
│  🎯 지능형 모드 감지                                         │
│     워크플로우 컨텍스트에 따라 자동으로 실행 모드 선택         │
├─────────────────────────────────────────────────────────────┤
│  🤖 인터랙티브 코드 어시스턴트                               │
│     코드, 아키텍처, 프로그래밍 질문에 답변                    │
├─────────────────────────────────────────────────────────────┤
│  🔍 코드 리뷰                                                │
│     PR 변경사항 분석 및 개선 제안                            │
├─────────────────────────────────────────────────────────────┤
│  ✨ 코드 구현                                                │
│     간단한 수정, 리팩토링, 새 기능까지 직접 구현              │
├─────────────────────────────────────────────────────────────┤
│  💬 PR/Issue 통합                                            │
│     GitHub 코멘트, PR 리뷰와 원활하게 연동                    │
├─────────────────────────────────────────────────────────────┤
│  📋 진행 상황 추적                                           │
│     체크박스로 실시간 진행 상황 시각화                        │
├─────────────────────────────────────────────────────────────┤
│  📊 구조화된 출력                                            │
│     검증된 JSON 결과 → GitHub Action outputs으로 자동 전환   │
├─────────────────────────────────────────────────────────────┤
│  🏃 자체 인프라 실행                                         │
│     GitHub runner에서 실행, API 호출만 Anthropic             │
└─────────────────────────────────────────────────────────────┘
```

---

## 빠른 시작

### 방법 1: Claude Code CLI 사용 (권장)

```bash
# 터미널에서 Claude Code 열고
claude

# 명령 실행
/install-github-app
```

> **참고**: Repository admin 권한 필요, Anthropic API 직접 사용자만 가능

### 방법 2: 수동 워크플로우 설정

```yaml
# .github/workflows/claude.yml
name: Claude Code Action

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened, assigned]
  pull_request:
    types: [opened, synchronize]

jobs:
  claude:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
```

---

## 사용 예시

### 1️⃣ PR에서 @claude 멘션

```markdown
@claude 이 PR의 성능 이슈를 분석해줘
```

Claude가 자동으로:
- PR 변경사항 분석
- 성능 이슈 식별
- 개선 제안 코멘트

### 2️⃣ Issue 할당

```markdown
# Issue #42: 로그인 버그 수정
assignee: @claude
```

Claude가 자동으로:
- Issue 분석
- 버그 원인 파악
- 수정 PR 생성

### 3️⃣ 자동 코드 리뷰

```yaml
on:
  pull_request:
    types: [opened]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          prompt: "이 PR을 리뷰하고 보안 취약점과 성능 이슈를 확인해줘"
```

---

## 지원하는 클라우드 프로바이더

| 프로바이더 | 인증 방식 |
|-----------|-----------|
| **Anthropic** (직접) | `ANTHROPIC_API_KEY` |
| **AWS Bedrock** | AWS credentials |
| **Google Vertex AI** | GCP credentials |
| **Microsoft Foundry** | Azure credentials |

---

## 솔루션 & 사용 사례

| 솔루션 | 설명 |
|--------|------|
| 🔍 **자동 PR 코드 리뷰** | 전체 리뷰 자동화 |
| 📂 **경로별 리뷰** | 중요 파일 변경 시 트리거 |
| 👥 **외부 기여자 리뷰** | 신규 기여자 특별 처리 |
| 📝 **커스텀 리뷰 체크리스트** | 팀 표준 강제 적용 |
| 🔄 **예약 유지보수** | 자동 레포지토리 건강 체크 |
| 🏷️ **Issue 분류 & 라벨링** | 자동 카테고리 분류 |
| 📖 **문서 동기화** | 코드 변경 시 문서 업데이트 |
| 🔒 **보안 중심 리뷰** | OWASP 기반 보안 분석 |

---

## 워크플로우 예제

### 자동 PR 리뷰

```yaml
name: Auto PR Review

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
      
    steps:
      - uses: actions/checkout@v4
      
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            이 PR을 철저히 리뷰해줘:
            1. 코드 품질 및 베스트 프랙티스
            2. 잠재적 버그 및 엣지 케이스
            3. 보안 취약점
            4. 성능 고려사항
            5. 테스트 커버리지
```

### Issue 자동 분류

```yaml
name: Issue Triage

on:
  issues:
    types: [opened]

jobs:
  triage:
    runs-on: ubuntu-latest
    permissions:
      issues: write
      
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            이 Issue를 분석하고:
            1. 적절한 라벨 추가 (bug, feature, docs 등)
            2. 우선순위 평가 (P0-P3)
            3. 관련 팀/담당자 추천
```

### 보안 중심 리뷰

```yaml
name: Security Review

on:
  pull_request:
    paths:
      - 'src/auth/**'
      - 'src/api/**'
      - '**/*secret*'

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          prompt: |
            OWASP Top 10 기준으로 보안 리뷰 수행:
            - SQL Injection
            - XSS
            - CSRF
            - 인증/인가 취약점
            - 민감 데이터 노출
```

---

## 레포지토리 구조

```
claude-code-action/
├── .claude/          # Claude 설정
├── .github/          # GitHub 워크플로우
├── base-action/      # 기본 액션 로직
├── docs/             # 문서
│   ├── setup.md
│   ├── usage.md
│   ├── solutions.md
│   ├── cloud-providers.md
│   └── security.md
├── examples/         # 워크플로우 예제
├── src/              # 소스 코드 (TypeScript)
├── test/             # 테스트
├── action.yml        # Action 정의
└── CLAUDE.md         # Claude 컨텍스트
```

---

## 문서 목록

| 문서 | 설명 |
|------|------|
| **Solutions Guide** | 바로 사용 가능한 자동화 패턴 |
| **Migration Guide** | v0.x → v1.0 업그레이드 가이드 |
| **Setup Guide** | 수동 설정, 커스텀 앱, 보안 |
| **Usage Guide** | 기본 사용법, 입력 파라미터 |
| **Custom Automations** | 자동화 워크플로우 예제 |
| **Configuration** | MCP, 권한, 환경변수 |
| **Cloud Providers** | Bedrock, Vertex AI, Foundry |
| **Security** | 접근 제어, 커밋 서명 |

---

## v0.x → v1.0 마이그레이션

```yaml
# v0.x (이전)
- uses: anthropics/claude-code-action@v0
  with:
    mode: "review"
    review_prompt: "..."

# v1.0 (현재)
- uses: anthropics/claude-code-action@v1
  with:
    prompt: "..."
    claude_args: "--model opus"
```

---

## 핵심 포인트

1. **공식 지원**: Anthropic에서 직접 개발/유지보수
2. **지능형 감지**: @claude 멘션, Issue 할당 등 자동 인식
3. **다중 프로바이더**: Anthropic, AWS, GCP, Azure 모두 지원
4. **자체 인프라**: GitHub runner에서 실행, 데이터 외부 유출 없음
5. **풍부한 솔루션**: 리뷰, 분류, 보안, 문서화 등 즉시 사용 가능
6. **5.3k Stars**: 활발한 커뮤니티 및 지속적 업데이트

---

## 참고 자료

- [공식 GitHub 레포지토리](https://github.com/anthropics/claude-code-action)
- [Solutions Guide](https://github.com/anthropics/claude-code-action/blob/main/docs/solutions.md)
- [Migration Guide (v0.x → v1.0)](https://github.com/anthropics/claude-code-action/blob/main/docs/migration.md)
- [Cloud Providers Setup](https://github.com/anthropics/claude-code-action/blob/main/docs/cloud-providers.md)
- [FAQ & Troubleshooting](https://github.com/anthropics/claude-code-action/blob/main/docs/faq.md)
