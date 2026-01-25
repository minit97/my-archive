# Awesome Claude Skills (travisvn) - Claude Code 중심 스킬 가이드

> **URL**: https://github.com/travisvn/awesome-claude-skills  
> **유형**: GitHub 레포지토리 (Awesome List)  
> **작성자**: travisvn  
> **Stars**: 5.7k ⭐ | **Forks**: 364

---

## 요약

Claude AI 워크플로우 커스터마이징을 위한 스킬, 리소스, 도구의 큐레이션 목록. **Claude Code에 특화**되어 있으며, Skills의 작동 원리, 보안 가이드라인, MCP/Subagents와의 비교 등 **실용적인 가이드**를 상세히 제공한다.

> "Claude Skills teach Claude how to perform tasks in a repeatable way"

---

## Skills 작동 원리

### Progressive Disclosure Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  1단계: Metadata Loading (~100 토큰)                        │
│     Claude가 사용 가능한 Skills 스캔 → 관련성 매칭           │
├─────────────────────────────────────────────────────────────┤
│  2단계: Full Instructions (<5k 토큰)                        │
│     해당 Skill 적용 시에만 전체 지침 로드                    │
├─────────────────────────────────────────────────────────────┤
│  3단계: Bundled Resources                                   │
│     파일, 실행 코드는 필요할 때만 로드                       │
└─────────────────────────────────────────────────────────────┘
```

> **핵심**: 여러 Skills가 컨텍스트 윈도우를 압도하지 않으면서 사용 가능

---

## 설치 방법

### Claude.ai 웹 인터페이스

1. Settings > Capabilities 이동
2. Skills 토글 활성화
3. 사용 가능한 스킬 탐색 또는 커스텀 스킬 업로드
4. **Team/Enterprise**: 관리자가 먼저 조직 전체 활성화 필요

### Claude Code CLI

```bash
# 마켓플레이스에서 설치
/plugin marketplace add anthropics/skills

# 로컬 디렉토리에서 설치
/plugin add /path/to/skill-directory
```

### Claude API

```python
import anthropic

client = anthropic.Client(api_key="your-api-key")
# /v1/skills 엔드포인트로 Skills 접근
```

---

## 공식 스킬

### 📄 Document Skills

| 스킬 | 설명 |
|------|------|
| **docx** | Word 문서 생성/편집/분석, 변경 추적, 코멘트 |
| **pdf** | PDF 텍스트/테이블 추출, 생성, 병합/분할, 양식 |
| **pptx** | PowerPoint 생성/편집, 레이아웃, 차트, 자동 슬라이드 |
| **xlsx** | Excel 생성/편집, 수식, 포맷팅, 데이터 분석, 시각화 |

### 🎨 Design & Creative

| 스킬 | 설명 |
|------|------|
| **algorithmic-art** | p5.js로 제너러티브 아트, 플로우 필드, 파티클 |
| **canvas-design** | .png/.pdf 형식 비주얼 아트 디자인 |
| **slack-gif-creator** | Slack 사이즈 최적화 애니메이션 GIF |

### 💻 Development

| 스킬 | 설명 |
|------|------|
| **frontend-design** | "AI slop" 회피, 대담한 디자인 결정 (React/Tailwind) |
| **artifacts-builder** | React, Tailwind, shadcn/ui로 HTML 아티팩트 빌드 |
| **mcp-builder** | 고품질 MCP 서버 생성 가이드 |
| **webapp-testing** | Playwright로 로컬 웹앱 테스트 |

### 📝 Communication

| 스킬 | 설명 |
|------|------|
| **brand-guidelines** | Anthropic 공식 브랜드 색상/타이포그래피 적용 |
| **internal-comms** | 상태 보고서, 뉴스레터, FAQ 작성 |

### 🛠️ Skill Creation

| 스킬 | 설명 |
|------|------|
| **skill-creator** | Q&A로 새 스킬 생성 가이드 |

---

## 커뮤니티 스킬 (주요)

### 컬렉션 & 라이브러리

| 스킬 | Stars | 설명 |
|------|-------|------|
| **obra/superpowers** | - | 20+ 검증된 스킬 (TDD, 디버깅, 협업 패턴) |
| **todo-skill** | - | TodoWrite 도구 사용 가이드 |
| **CLAUDE.md Collection** | - | 다양한 프로젝트용 CLAUDE.md 템플릿 |
| **anthropic-skills** | - | Anthropic 공식 스킬 레포지토리 |
| **oh-my-claudecode** | - | 제로 러닝커브 설정 시스템 |
| **everything-claude-code** | - | 해커톤 우승 설정 컬렉션 |

### 개발 스킬

| 스킬 | 설명 |
|------|------|
| **claude-skill-tree** | 프로젝트 구조 및 레이아웃 매핑 |
| **claude-test-runner** | Pytest 통합 테스트 러너 |
| **anthropic-mcp-skill** | MCP 개발 공식 가이드 |
| **git-semver** | 시맨틱 버저닝 자동화 |
| **git-commit-skill** | Git 커밋 워크플로우 |
| **ship-learn-next** | 피드백 루프 기반 다음 빌드 결정 |

### 비즈니스 & 생산성

| 스킬 | 설명 |
|------|------|
| **lead-research-assistant** | 리드 리서치 및 분석 |
| **tapestry** | 문서 연결 및 지식 네트워크 |
| **sop-creator** | 표준 운영 절차 문서 생성 |
| **competitive-ads-extractor** | 경쟁사 광고 전략 분석 |

---

## Skills vs 다른 도구 비교

### Skills vs MCP vs Subagents vs Projects

| 기능 | Skills | Prompts | Projects | Subagents | MCP |
|------|--------|---------|----------|-----------|-----|
| **용도** | 대화 간 재사용 가능한 절차적 지식 | 일회성 지시 | 워크스페이스 내 배경 지식 | 특정 권한의 독립 작업 | 외부 데이터 연결 |

**사용 시점**:
- **Skills**: 어떤 Claude 인스턴스에서든 접근 가능해야 할 때
- **Subagents**: 특정 목적의 독립 에이전트 필요 시
- **조합**: Subagents가 Skills를 활용 가능

> **핵심 인사이트**: *여러 대화에서 같은 프롬프트를 반복 입력한다면, Skill을 만들 때*

### Skills vs MCP 상세 비교

| 특성 | Skills | MCP |
|------|--------|-----|
| **목적** | 작업별 전문성 및 워크플로우 | 외부 데이터/API 통합 |
| **이식성** | 모든 곳 동일 포맷 | 서버 설정 필요 |
| **코드 실행** | 실행 스크립트 포함 가능 | 도구/리소스 제공 |
| **토큰 효율** | 로드 전 30-50 토큰 | 구현에 따라 다름 |
| **최적 용도** | 반복 작업, 문서 워크플로우 | DB 접근, API 통합 |

> **함께 사용**: Skills가 MCP 서버를 생성할 수 있음! `mcp-builder` 스킬 활용

### Skills vs System Prompts

| 특성 | Skills | System Prompts |
|------|--------|----------------|
| **구조** | YAML 프론트매터 + 지침 + 스크립트 폴더 | 플레인 텍스트 |
| **재사용성** | 버전 관리, 공유, 조합 가능 | 복사-붙여넣기, 대화별 |
| **로딩** | 온디맨드 (관련 시에만) | 항상 컨텍스트에 |
| **유지보수** | 중앙 집중 업데이트 | 대화별 수동 업데이트 |
| **조합성** | 여러 스킬 자동 스택 | 수동 조합 |

---

## 🔒 보안 & 베스트 프랙티스

### ⚠️ 경고

> **Skills는 Claude 환경에서 임의 코드를 실행할 수 있습니다.**  
> **신뢰할 수 있는 소스의 스킬만 설치하세요.**

### 보안 가이드라인

```
┌─────────────────────────────────────────────────────────────┐
│  스킬 검증 체크리스트                                        │
├─────────────────────────────────────────────────────────────┤
│  ✅ 신뢰할 수 있는 소스에서만 설치                           │
│  ✅ SKILL.md 및 모든 스크립트 활성화 전 검토                 │
│  ✅ 민감 데이터 접근 요청 스킬 주의                          │
│  ✅ 프로덕션/엔터프라이즈 배포 전 철저 감사                  │
└─────────────────────────────────────────────────────────────┘
```

### 보안 우려사항

- **악의적 스킬**: 취약점 또는 데이터 유출 가능
- **프롬프트 인젝션**: 손상된 스킬로 증폭 가능
- **샌드박싱 제한**: 엔터프라이즈 배포 전 보안 모델 이해 필요

### 베스트 프랙티스

| 프랙티스 | 설명 |
|----------|------|
| **버전 관리** | Git에서 적절한 버전 태그로 추적 |
| **코드 리뷰** | 팀 배포 전 피어 리뷰 |
| **최소 권한** | 필요한 권한/접근만 부여 |
| **정기 감사** | 설치된 스킬 주기적 검토 |
| **문서화** | 커스텀 스킬 명확한 문서 유지 |
| **테스팅** | 비프로덕션 환경에서 먼저 테스트 |

---

## FAQ

### 토큰 사용량

> **Q: Skills가 토큰 사용량에 얼마나 영향?**
> 
> A: Progressive disclosure 덕분에 매우 효율적.
> - 메타데이터 스캔: ~100 토큰
> - 활성화 시 전체 로드: <5k 토큰
> - 번들 리소스: 필요 시에만

### 접근 가능 사용자

> **Q: 누가 Skills를 사용할 수 있나요?**
> 
> A: **Pro, Max, Team, Enterprise** 사용자.  
> Free tier는 접근 불가.

### 스킬 선택 방식

> **Q: Claude가 어떤 스킬을 사용할지 어떻게 결정?**
> 
> A: Claude가:
> 1. 모든 스킬의 프론트매터(name, description) 스캔
> 2. 현재 작업과의 관련성 평가
> 3. 관련 스킬의 전체 내용 로드
> 4. 여러 스킬 자동 조합 가능

### Skills + MCP 함께 사용

> **Q: Skills와 MCP를 함께 사용할 수 있나요?**
> 
> A: **물론!** 상호 보완적.
> - Skills: 작업별 워크플로우
> - MCP: 외부 데이터/API 통합
> - `mcp-builder` 스킬로 MCP 서버 생성 가능

---

## 알려진 이슈

### Linux 경로 버그 (Oct 18, 2025)

Agent SDK가 환경 홈 디렉토리 대신 하드코딩된 macOS 경로 사용

**해결**: 스킬 경로 수동 지정

### 엔터프라이즈 배포

Claude.ai에서 커스텀 스킬의 중앙 집중 관리자 관리 아직 미지원

**해결**: Git 레포지토리로 팀 배포

---

## ComposioHQ vs travisvn 비교

| 레포지토리 | Stars | 특징 |
|-----------|-------|------|
| **ComposioHQ/awesome-claude-skills** | 24.4k | 스킬 목록 중심, 많은 카테고리 |
| **travisvn/awesome-claude-skills** | 5.7k | 상세 가이드, 보안, MCP 비교 |

---

## 핵심 포인트

1. **Progressive Disclosure**: 100 토큰 → 5k 토큰 → 필요 시 리소스
2. **Claude Code 특화**: CLI 사용법 상세 설명
3. **MCP/Subagents 비교**: 언제 무엇을 사용할지 명확
4. **보안 가이드**: 스킬 검증 체크리스트 제공
5. **실용적 FAQ**: 토큰, 접근권한, 선택 방식 등

---

## 참고 자료

- [GitHub 레포지토리](https://github.com/travisvn/awesome-claude-skills)
- [How to Create Your First Claude Skill](https://docs.anthropic.com/skills/first-skill)
- [Skills Explained - Anthropic Blog](https://www.anthropic.com/news/skills)
- [Simon Willison: Claude Skills Analysis](https://simonwillison.net/2025/Oct/10/claude-skills/)
- [Security Research: Weaponizing Claude Code Skills](https://security.anthropic.com/skills)
