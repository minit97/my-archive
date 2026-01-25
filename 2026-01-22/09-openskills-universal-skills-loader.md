# OpenSkills - AI 코딩 에이전트를 위한 유니버설 스킬 로더

> **경로**: `/study-github/openskills`  
> **유형**: CLI 도구 (npm 패키지)  
> **라이선스**: Apache 2.0  
> **요구사항**: Node.js 20.6+, Git
> **원본**: [openskills](https://github.com/numman-ali/openskills)

---

## 요약

**OpenSkills**는 Anthropic의 스킬 시스템을 **모든 AI 코딩 에이전트**에서 사용할 수 있게 해주는 유니버설 로더입니다. Claude Code, Cursor, Windsurf, Aider, Codex 등 `AGENTS.md`를 읽을 수 있는 모든 에이전트에서 동일한 형식의 스킬을 사용할 수 있습니다.

---

## 핵심 개념

### "SKILL.md의 유니버설 인스톨러"

```
┌─────────────────────────────────────────────────────────────┐
│  하나의 CLI. 모든 에이전트. Claude Code와 동일한 포맷.       │
└─────────────────────────────────────────────────────────────┘
```

### Claude Code vs OpenSkills 비교

| 측면 | Claude Code | OpenSkills |
|------|-------------|------------|
| **프롬프트 포맷** | `<available_skills>` XML | 동일한 XML |
| **스킬 저장소** | `.claude/skills/` | `.claude/skills/` (기본) |
| **호출 방법** | `Skill("name")` 도구 | `npx openskills read <name>` |
| **마켓플레이스** | Anthropic 마켓플레이스 | GitHub (anthropics/skills) |
| **점진적 공개** | ✅ | ✅ |

---

## 빠른 시작

```bash
# 스킬 설치 (Anthropic 마켓플레이스에서)
npx openskills install anthropics/skills

# AGENTS.md에 동기화
npx openskills sync
```

기본 설치는 **프로젝트 로컬** (`./.claude/skills`). `--global`로 `~/.claude/skills`에 전역 설치.

---

## 주요 명령어

| 명령어 | 설명 |
|--------|------|
| `npx openskills install <source>` | GitHub, 로컬 경로, 프라이빗 레포에서 설치 |
| `npx openskills sync` | AGENTS.md 업데이트 |
| `npx openskills list` | 설치된 스킬 목록 |
| `npx openskills read <name>` | 스킬 로드 (에이전트용) |
| `npx openskills update [name...]` | 설치된 스킬 업데이트 |
| `npx openskills manage` | 스킬 관리 (인터랙티브) |
| `npx openskills remove <name>` | 특정 스킬 제거 |

### 플래그

| 플래그 | 설명 |
|--------|------|
| `--global` | `~/.claude/skills`에 전역 설치 |
| `--universal` | `.agent/skills/`에 설치 (멀티 에이전트용) |
| `-y, --yes` | 프롬프트 스킵 (CI용) |
| `-o, --output <path>` | sync 출력 파일 (기본: AGENTS.md) |

---

## 설치 소스

### Anthropic 마켓플레이스

```bash
npx openskills install anthropics/skills
```

### GitHub 레포

```bash
npx openskills install your-org/your-skills
```

### 로컬 경로

```bash
npx openskills install ./local-skills/my-skill
```

### 프라이빗 Git 레포

```bash
npx openskills install git@github.com:your-org/private-skills.git
```

---

## SKILL.md 포맷

### 기본 구조

```markdown
---
name: pdf
description: PDF 조작 툴킷. 텍스트/테이블 추출, 새 PDF 생성, 문서 병합/분할...
---

# PDF Skill Instructions

PDF 작업 요청 시 다음 단계를 따르세요:
1. 의존성 설치: `pip install pypdf2`
2. scripts/extract_text.py로 텍스트 추출
3. 상세 내용은 references/api-docs.md 참조
```

### YAML 프론트매터 (필수)

```yaml
---
name: skill-name           # 필수: 하이픈 케이스 식별자
description: 사용 시점     # 필수: 1-2문장, 3인칭
---
```

### 마크다운 본문 규칙

**좋은 예:**
- "X를 수행하려면 Y를 실행"
- "Z일 때 이 스킬 로드"
- "상세 내용은 references/guide.md 참조"

**피해야 할 예:**
- "You should do X"
- "If you need Y"
- "When you want Z"

---

## 스킬 구조

### 최소 구조

```
my-skill/
└── SKILL.md
```

### 리소스 포함

```
my-skill/
├── SKILL.md              # (~2,000 단어)
├── references/           # 필요 시 컨텍스트에 로드
│   └── api-docs.md
├── scripts/              # 실행 가능, 컨텍스트에 카운트 안 됨
│   └── helper.py
└── assets/               # 출력 파일, 컨텍스트에 로드 안 됨
    └── template.pdf
```

### 점진적 공개 (Progressive Disclosure)

스킬은 3단계로 로드:

```
1단계: 메타데이터 (항상 컨텍스트에)
       → name + description

2단계: SKILL.md (관련 시 로드)
       → 핵심 지침

3단계: 리소스 (필요 시 로드)
       → 상세 문서
```

### 파일 크기 가이드라인

| 항목 | 제한 |
|------|------|
| `SKILL.md` | 5,000 단어 미만 |
| `references/` | 무제한 (선택적 로드) |
| `scripts/` | 실행 가능, 카운트 안 됨 |
| `assets/` | 컨텍스트에 로드 안 됨 |

---

## AGENTS.md 포맷

OpenSkills가 생성하는 정확한 XML 포맷:

```xml
<skills_system priority="1">

## Available Skills

<!-- SKILLS_TABLE_START -->
<usage>
작업 요청 시 아래 스킬이 도움될 수 있는지 확인하세요.

스킬 사용법:
- 호출: `npx openskills read <skill-name>` (쉘에서 실행)
- 스킬 콘텐츠가 상세 지침과 함께 로드됨
- 번들된 리소스 해석을 위한 베이스 디렉토리 제공

사용 노트:
- 아래 <available_skills>에 있는 스킬만 사용
- 이미 컨텍스트에 로드된 스킬은 다시 호출하지 않음
</usage>

<available_skills>

<skill>
<name>pdf</name>
<description>PDF 조작 툴킷...</description>
<location>project</location>
</skill>

</available_skills>
<!-- SKILLS_TABLE_END -->

</skills_system>
```

---

## 유니버설 모드 (멀티 에이전트)

Claude Code와 다른 에이전트를 함께 사용할 때, `.agent/skills/`에 설치하여 충돌 방지:

```bash
npx openskills install anthropics/skills --universal
```

### 우선순위 (높은 것이 우선)

1. `./.agent/skills/`
2. `~/.agent/skills/`
3. `./.claude/skills/`
4. `~/.claude/skills/`

---

## 스킬 직접 만들기

### 1. 디렉토리 생성

```bash
mkdir my-skill/
```

### 2. SKILL.md 작성

```markdown
---
name: my-skill
description: 스킬 사용 시점 설명
---

# My Skill

지침 내용...
```

### 3. 설치

```bash
npx openskills install ./my-skill
```

### 4. 로컬 개발 (심볼릭 링크)

```bash
git clone git@github.com:your-org/my-skills.git ~/dev/my-skills
mkdir -p .claude/skills
ln -s ~/dev/my-skills/my-skill .claude/skills/my-skill
```

### 5. 스킬 작성 가이드 로드

```bash
npx openskills install anthropics/skills
npx openskills read skill-creator
```

---

## 프로젝트 구조

```
openskills/
├── src/
│   ├── cli.ts              # 메인 진입점
│   ├── commands/           # 명령어 구현
│   │   ├── install.ts      # 설치 로직
│   │   ├── sync.ts         # AGENTS.md 동기화
│   │   ├── read.ts         # 스킬 읽기
│   │   ├── list.ts         # 목록 표시
│   │   ├── update.ts       # 업데이트
│   │   ├── manage.ts       # 인터랙티브 관리
│   │   └── remove.ts       # 제거
│   ├── utils/              # 유틸리티
│   │   ├── agents-md.ts    # AGENTS.md 파싱/생성
│   │   ├── skills.ts       # 스킬 검색
│   │   ├── yaml.ts         # YAML 프론트매터 파싱
│   │   └── ...
│   └── types.ts            # TypeScript 인터페이스
├── tests/                  # 테스트
├── examples/               # 예시 스킬
└── package.json
```

---

## FAQ

### 왜 MCP 대신 CLI인가?

**MCP는 동적 도구용**. 스킬은 정적 지침 + 리소스.

- 스킬은 파일일 뿐 → 서버 불필요
- 모든 에이전트에서 작동 → MCP 지원 불필요
- Anthropic 설계와 일치 → SKILL.md가 스펙

MCP와 스킬은 다른 문제를 해결. OpenSkills는 스킬을 경량화하고 유니버설하게 유지.

---

## 핵심 포인트

1. **유니버설 로더**: Claude Code, Cursor, Windsurf, Aider 등 모두 지원
2. **정확히 동일한 포맷**: Anthropic의 `<available_skills>` XML 그대로
3. **점진적 공개**: 필요할 때만 스킬 로드 (컨텍스트 깨끗하게 유지)
4. **프라이빗 친화적**: 로컬 경로, 프라이빗 Git 레포 지원
5. **레포 친화적**: 스킬을 프로젝트에 포함하고 버전 관리 가능

---

## 관련 링크

- [Anthropic Agent Skills 스펙](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- [npm 패키지](https://www.npmjs.com/package/openskills)

---

## 참고

**Anthropic과 제휴 아님.** Claude, Claude Code, Agent Skills는 Anthropic, PBC의 상표입니다.
