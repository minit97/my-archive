# add-skill - AI 코딩 에이전트용 스킬 설치 CLI 도구

> **URL**: https://add-skill.org/  
> **유형**: 오픈소스 CLI 도구  
> **관리**: Vercel Labs  
> **버전**: 1.0.5 (2026년 1월 15일 기준)

---

## 요약

**add-skill**은 Git 저장소에서 **Agent Skills**를 설치할 수 있는 CLI 도구다. OpenCode, Claude Code, Codex, Cursor 등 주요 AI 코딩 에이전트를 지원하며, `npx add-skill`로 바로 사용 가능하다. 이전에 정리한 **OpenSkills**와 유사하지만, Vercel Labs에서 관리하는 별도의 도구이다.

---

## 핵심 개념

### Agent Skills란?

```
┌─────────────────────────────────────────────────────────────┐
│  Agent Skills = SKILL.md 파일에 정의된 재사용 가능한        │
│                 지시문(instruction set)                     │
├─────────────────────────────────────────────────────────────┤
│  • YAML frontmatter: name, description                      │
│  • Markdown body: 상세 지시 사항                            │
│  • 복잡한 워크플로우 자동화                                  │
│  • 외부 도구 통합                                           │
│  • 팀 컨벤션 표준화                                         │
└─────────────────────────────────────────────────────────────┘
```

### 활용 예시

| 스킬 | 설명 |
|------|------|
| 📝 **Release Notes** | Git 히스토리에서 자동으로 릴리즈 노트 생성 |
| 🔀 **Create PR** | 팀 PR 템플릿/컨벤션에 맞춰 자동 생성 |
| 🔗 **Tool Integration** | Linear, Notion, Jira 등 외부 도구 연동 |

---

## 주요 기능

### 1️⃣ 멀티 소스 지원

```bash
# GitHub shorthand
npx add-skill vercel-labs/agent-skills

# Full URL
npx add-skill https://github.com/user/repo

# GitLab도 지원
npx add-skill https://gitlab.com/user/repo
```

### 2️⃣ 에이전트 자동 감지

- 설치된 코딩 에이전트를 **자동으로 감지**
- 각 에이전트의 설정 디렉토리를 확인하여 올바른 위치에 설치
- 수동 설정 불필요

### 3️⃣ 프로젝트 & 글로벌 스코프

```bash
# 프로젝트 레벨 (팀과 공유)
npx add-skill vercel-labs/agent-skills

# 글로벌 레벨 (모든 프로젝트에서 사용)
npx add-skill vercel-labs/agent-skills -g
```

### 4️⃣ 선택적 스킬 설치

```bash
# 사용 가능한 스킬 목록 확인
npx add-skill vercel-labs/agent-skills --list

# 특정 스킬만 설치
npx add-skill vercel-labs/agent-skills --skill frontend-design
```

### 5️⃣ CI/CD 친화적

```bash
# Non-interactive 모드 (-y 플래그)
npx add-skill vercel-labs/agent-skills --skill frontend-design -g -y
```

- **Zero dependencies**: 의존성 없음
- **Instant installation**: npx로 즉시 실행

---

## 지원 에이전트 및 설치 경로

| 에이전트 | 프로젝트 경로 | 글로벌 경로 |
|----------|---------------|-------------|
| **OpenCode** | `.opencode/skill/<name>/` | `~/.config/opencode/skill/` |
| **Claude Code** | `.claude/skills/<name>/` | `~/.claude/skills/` |
| **Codex** | `.codex/skills/<name>/` | `~/.codex/skills/` |
| **Cursor** | `.cursor/skills/<name>/` | `~/.cursor/skills/` |

---

## 사용법 3단계

```
┌─────────────────────────────────────────────────────────────┐
│  Step 1: 스킬 목록 확인                                      │
│  $ npx add-skill vercel-labs/agent-skills --list            │
├─────────────────────────────────────────────────────────────┤
│  Step 2: 스킬 설치                                          │
│  $ npx add-skill vercel-labs/agent-skills --skill <name>    │
├─────────────────────────────────────────────────────────────┤
│  Step 3: 에이전트에서 사용                                   │
│  /<skill-name> 명령어로 즉시 사용                           │
└─────────────────────────────────────────────────────────────┘
```

---

## CLI 명령어 정리

### 기본 명령어

```bash
# 저장소의 모든 스킬 설치
npx add-skill vercel-labs/agent-skills

# 특정 스킬만 설치
npx add-skill vercel-labs/agent-skills --skill frontend-design --skill skill-creator

# 글로벌 + 특정 에이전트 지정
npx add-skill vercel-labs/agent-skills -g -a claude-code -a opencode

# CI/CD용 non-interactive
npx add-skill vercel-labs/agent-skills --skill frontend-design -g -y
```

### 옵션 플래그

| 플래그 | 설명 |
|--------|------|
| `--list` | 저장소의 사용 가능한 스킬 목록 표시 |
| `--skill <name>` | 특정 스킬만 설치 (복수 사용 가능) |
| `-g` | 글로벌 설치 |
| `-a <agent>` | 특정 에이전트에만 설치 |
| `-y` | Non-interactive 모드 (자동 승인) |

---

## OpenSkills와의 비교

| 항목 | add-skill | OpenSkills |
|------|-----------|------------|
| **관리 주체** | Vercel Labs | 커뮤니티 |
| **설치 방식** | `npx add-skill` | `npx openskills` |
| **AGENTS.md 연동** | 미확인 | ✅ XML 블록 자동 생성 |
| **지원 에이전트** | OpenCode, Claude Code, Codex, Cursor | Claude Code + Universal Mode |
| **스킬 포맷** | SKILL.md (YAML frontmatter) | 동일 |
| **소스** | Git 저장소 | Git 저장소, npm, 로컬 |

---

## 핵심 포인트

1. **Zero-config**: 에이전트 자동 감지로 설정 불필요
2. **Cross-agent**: 여러 AI 코딩 에이전트에서 동일한 스킬 사용 가능
3. **Git 기반**: 팀/조직의 스킬을 Git 저장소로 공유 및 버전 관리
4. **선택적 설치**: 필요한 스킬만 골라서 설치
5. **CI/CD 친화적**: non-interactive 모드로 자동화 파이프라인에 통합

---

## 참고 링크

- **GitHub**: [add-skill Repository](https://github.com/vercel-labs/add-skill)
- **Agent Skills 저장소**: [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)
- **라이선스**: MIT
