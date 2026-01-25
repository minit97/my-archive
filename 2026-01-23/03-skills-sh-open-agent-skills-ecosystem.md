# Skills.sh - 오픈 에이전트 스킬 에코시스템

> **URL**: https://skills.sh/  
> **유형**: 스킬 레지스트리/마켓플레이스  
> **설치 명령**: `npx skills add <owner/repo>`

---

## 요약

**Skills.sh**는 AI 코딩 에이전트를 위한 **오픈 스킬 에코시스템**이다. 단일 명령어로 재사용 가능한 스킬을 설치하여 에이전트의 절차적 지식(procedural knowledge)을 확장할 수 있다. 현재 **200개 이상의 공개 스킬**과 **15개 이상의 지원 에이전트**를 보유하고 있다.

---

## 지원 에이전트

```
┌─────────────────────────────────────────────────────────────┐
│  AMP · Antigravity · Claude Code · ClawdBot · Codex         │
│  Cursor · Droid · Gemini · GitHub Copilot · Goose           │
│  Kilo · Kiro CLI · OpenCode · Roo · Trae · Windsurf         │
└─────────────────────────────────────────────────────────────┘
```

> 이전 정리한 **add-skill** (13번 문서)보다 더 많은 에이전트 지원!

---

## 설치 방법

```bash
# 기본 설치
npx skills add <owner/repo>

# 예시: Vercel의 React 베스트 프랙티스 스킬
npx skills add vercel-labs/agent-skills
```

---

## 🏆 Skills Leaderboard (TOP 50)

### 🥇 프론트엔드/디자인 스킬

| 순위 | 스킬명 | 출처 | 설치수 |
|------|--------|------|--------|
| 1 | **vercel-react-best-practices** | vercel-labs/agent-skills | 31.6K |
| 2 | **web-design-guidelines** | vercel-labs/agent-skills | 23.9K |
| 3 | **remotion-best-practices** | remotion-dev/skills | 11.5K |
| 4 | **frontend-design** | anthropics/skills | 3.3K |
| 18 | **ui-ux-pro-max** | nextlevelbuilder | 466 |

### 🛠️ 개발자 도구 스킬

| 순위 | 스킬명 | 출처 | 설치수 |
|------|--------|------|--------|
| 5 | **skill-creator** | anthropics/skills | 2.2K |
| 10 | **agent-browser** | vercel-labs/agent-browser | 1.6K |
| 57 | **mcp-builder** | anthropics/skills | 576 |
| 55 | **webapp-testing** | anthropics/skills | 616 |

### 📱 Expo/React Native 스킬

| 순위 | 스킬명 | 출처 | 설치수 |
|------|--------|------|--------|
| 6 | **building-native-ui** | expo/skills | 2.1K |
| 7 | **upgrading-expo** | expo/skills | 1.8K |
| 8 | **native-data-fetching** | expo/skills | 1.7K |
| 11 | **expo-dev-client** | expo/skills | 1.6K |
| 12 | **expo-deployment** | expo/skills | 1.5K |

### 📣 마케팅 스킬 (coreyhaines31)

| 순위 | 스킬명 | 설치수 |
|------|--------|--------|
| 15 | **seo-audit** | 1.4K |
| 18 | **copywriting** | 1.4K |
| 20 | **marketing-psychology** | 1.1K |
| 21 | **programmatic-seo** | 1.0K |
| 22 | **marketing-ideas** | 962 |

### 🔧 Superpowers 시리즈 (obra)

| 순위 | 스킬명 | 설치수 |
|------|--------|--------|
| 49 | **brainstorming** | 683 |
| 59 | **test-driven-development** | 532 |
| 60 | **systematic-debugging** | 515 |
| 65 | **writing-plans** | 475 |
| 71 | **verification-before-completion** | 439 |
| 75 | **writing-skills** | 416 |

### 📄 문서 처리 스킬 (Anthropic)

| 순위 | 스킬명 | 설치수 |
|------|--------|--------|
| 28 | **pdf** | 849 |
| 47 | **xlsx** | 710 |
| 48 | **pptx** | 703 |
| 52 | **docx** | 669 |

---

## 주요 스킬 제공자

| 제공자 | 스킬 카테고리 | 대표 스킬 |
|--------|---------------|-----------|
| **vercel-labs** | 프론트엔드, 브라우저 | react-best-practices, web-design |
| **anthropics** | 개발도구, 문서처리 | skill-creator, mcp-builder, pdf |
| **expo** | React Native/모바일 | building-native-ui, deployment |
| **obra (superpowers)** | 개발 방법론 | TDD, systematic-debugging |
| **coreyhaines31** | 마케팅 | SEO, copywriting, pricing |
| **better-auth** | 인증 | auth-best-practices |
| **jimliu (baoyu)** | 콘텐츠 생성 | slide-deck, comic, cover-image |
| **hyf0** | Vue.js | vue-best-practices, pinia |
| **onmax** | Nuxt.js | nuxt, nuxt-ui, motion |
| **trailofbits** | 보안 | semgrep, codeql, variant-analysis |

---

## 카테고리별 인기 스킬

### 프레임워크별

```
React/Next.js   → vercel-labs/agent-skills (31.6K)
Vue.js          → hyf0/vue-skills (646)
Nuxt.js         → onmax/nuxt-skills (346)
React Native    → expo/skills (2.1K)
NestJS          → kadajett/agent-nestjs-skills (132)
```

### 용도별

```
디자인/UI       → web-design-guidelines (23.9K)
테스팅          → test-driven-development (532)
문서화          → pdf, xlsx, pptx, docx
마케팅          → seo-audit (1.4K)
보안            → semgrep, codeql (108+)
```

---

## OpenSkills / add-skill과의 비교

| 항목 | Skills.sh | add-skill | OpenSkills |
|------|-----------|-----------|------------|
| **설치 명령** | `npx skills add` | `npx add-skill` | `npx openskills` |
| **지원 에이전트** | 15+ | 4 | Claude Code + Universal |
| **스킬 개수** | 200+ (공개 리더보드) | 저장소 기반 | 저장소 기반 |
| **리더보드** | ✅ | ❌ | ❌ |
| **관리** | 커뮤니티 | Vercel Labs | 커뮤니티 |

---

## 핵심 포인트

1. **가장 큰 에코시스템**: 200+ 스킬, 15+ 에이전트 지원
2. **리더보드 기반**: 인기 스킬을 한눈에 확인 가능
3. **다양한 카테고리**: 프론트엔드, 마케팅, 보안, 문서처리 등
4. **단일 명령 설치**: `npx skills add <owner/repo>`
5. **Anthropic 공식 스킬 포함**: skill-creator, mcp-builder, pdf 등

---

## 추천 스킬 (입문자용)

```bash
# 1. 프론트엔드 개발자
npx skills add vercel-labs/agent-skills

# 2. TDD/품질 중심 개발
npx skills add obra/superpowers

# 3. React Native/모바일
npx skills add expo/skills

# 4. Vue.js 개발자
npx skills add hyf0/vue-skills

# 5. 마케팅/콘텐츠
npx skills add coreyhaines31/marketingskills
```

---

## 참고 사항

- **Superpowers 스킬** (`obra/superpowers`)은 이전에 정리한 `12-tdd-for-skills-writing-guide.md`의 출처
- **Anthropic 공식 스킬**이 레지스트리에 등록되어 있어 신뢰도 높음
- 설치 수 기준으로 **Vercel의 React 베스트 프랙티스**가 압도적 1위 (31.6K)
