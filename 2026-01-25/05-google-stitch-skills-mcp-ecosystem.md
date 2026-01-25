# Google Stitch Skills & MCP 생태계

> **URL**: https://github.com/google-labs-code/stitch-skills, https://github.com/davideast/stitch-mcp  
> **유형**: GitHub 레포지토리 (AI Agent Skills + MCP Server)  
> **작성자**: Google Labs Code, David East  
> **라이선스**: Apache-2.0  
> **언어**: TypeScript 주요

---

## 요약

Google의 **Stitch 디자인 시스템**을 위한 AI 에이전트 스킬과 MCP 서버 생태계. **Stitch Skills**는 디자인-코드 변환을 위한 Agent Skills 라이브러리이고, **Stitch MCP**는 Google Cloud Stitch API와 연동하는 MCP 서버. 두 프로젝트가 함께 작동하여 디자인 시스템의 자동화된 코드 생성과 검증을 제공.

---

## 1. Stitch Skills (google-labs-code/stitch-skills)

> **Stars**: 684 ⭐ | **Forks**: 49  
> **언어**: TypeScript 78.3%, Shell 21.7%

### 핵심 특징

```
Agent Skills 오픈 스탠다드 준수
       ↓
Antigravity, Gemini CLI, Claude Code, Cursor 호환
       ↓
add-skill CLI로 자동 설치
```

### 제공 스킬 (2개)

| 스킬 | 용도 |
|------|------|
| **design-md** | Stitch 프로젝트 분석 → `DESIGN.md` 생성 (디자인 시스템 문서화) |
| **react-components** | Stitch 화면 → React 컴포넌트 시스템 변환 (검증 + 디자인 토큰 일관성) |

### 설치 방법

```bash
# 사용 가능한 스킬 목록
npx add-skill google-labs-code/stitch-skills --list

# 특정 스킬 설치 (예: React Components)
npx add-skill google-labs-code/stitch-skills --skill react:components --global
```

### 스킬 구조

```
skills/[category]/
├── SKILL.md           ← "Mission Control" (에이전트 지침)
├── scripts/           ← 실행 가능한 검증/네트워킹 스크립트
├── resources/         ← 지식 베이스 (체크리스트, 스타일 가이드)
└── examples/          ← "Gold Standard" 참조 예시
```

---

## 2. Stitch MCP (davideast/stitch-mcp)

> **Stars**: 153 ⭐ | **Forks**: 15  
> **언어**: TypeScript 99.9%

### 핵심 특징

```
Google Cloud Stitch API ↔ MCP 서버 ↔ AI 에이전트
                              ↓
                    디자인 시스템 자동화
```

### 주요 기능

| 기능 | 설명 |
|------|------|
| **API 연동** | Google Cloud Stitch API와 MCP 프로토콜 브리지 |
| **인증 관리** | Google Cloud 인증 자동화 (OAuth + ADC) |
| **프록시 모드** | 토큰 자동 갱신 + 디버그 로깅 |
| **환경 감지** | WSL, SSH, Docker, Cloud Shell 자동 감지 |

### 설치 및 설정

```bash
# 설치
npm install -g @_davideast/stitch-mcp

# 초기 설정 (가이드 체크리스트)
npx @_davideast/stitch-mcp init

# 진단
npx @_davideast/stitch-mcp doctor --verbose
```

### 인증 방식

#### 1. 사용자 인증 (OAuth)
```bash
CLOUDSDK_CONFIG="~/.stitch-mcp/config" gcloud auth login
```

#### 2. 애플리케이션 기본 자격 증명 (ADC)
```bash
gcloud auth application-default login
```

### MCP 설정

#### Direct Connection (HTTP)
```json
{
  "mcpServers": {
    "stitch": {
      "type": "http",
      "url": "https://stitch.googleapis.com/mcp",
      "headers": {
        "Authorization": "Bearer <token>",
        "X-Goog-User-Project": "<project-id>"
      }
    }
  }
}
```

#### Proxy Mode (STDIO) - 권장
```json
{
  "mcpServers": {
    "stitch": {
      "command": "npx",
      "args": ["@_davideast/stitch-mcp", "proxy"],
      "env": {
        "STITCH_PROJECT_ID": "<project-id>"
      }
    }
  }
}
```

---

## 통합 워크플로우

```
┌─────────────────────────────────────────────────────────────┐
│  Google Stitch 생태계                                       │
├─────────────────────────────────────────────────────────────┤
│  1. 디자인 시스템 (Stitch)                                  │
│       ↓                                                     │
│  2. MCP 서버 (stitch-mcp)                                   │
│       ↓                                                     │
│  3. AI 에이전트 (Claude Code, Cursor, Gemini CLI)           │
│       ↓                                                     │
│  4. Agent Skills (stitch-skills)                            │
│       ↓                                                     │
│  5. 자동화된 코드 생성                                       │
│       ├── DESIGN.md 문서화                                  │
│       └── React 컴포넌트 변환                               │
└─────────────────────────────────────────────────────────────┘
```

### 사용 시나리오

1. **디자인 문서화**
   ```
   Stitch 프로젝트 → design-md 스킬 → DESIGN.md 자동 생성
   ```

2. **코드 변환**
   ```
   Stitch 화면 → react-components 스킬 → React 컴포넌트 + 검증
   ```

3. **API 연동**
   ```
   AI 에이전트 → Stitch MCP → Google Cloud Stitch API
   ```

---

## 환경별 지원

### 개발 환경

| 환경 | 지원 | 특이사항 |
|------|------|----------|
| **로컬** | ✅ | 기본 브라우저 인증 |
| **WSL** | ✅ | URL 수동 복사 필요 |
| **SSH** | ✅ | 터미널 URL 출력 |
| **Docker** | ✅ | 컨테이너 내 인증 |
| **Cloud Shell** | ✅ | 자동 감지 |

### 호환 에이전트

| 에이전트 | 지원 |
|----------|------|
| **Claude Code** | ✅ |
| **Cursor** | ✅ |
| **Gemini CLI** | ✅ |
| **Antigravity** | ✅ |

---

## 새로운 스킬 추가 가이드

### 추천 스킬 유형

| 유형 | 설명 |
|------|------|
| **Validation** | Stitch HTML → 다른 UI 프레임워크 변환 + 문법 검증 |
| **Decoupling Data** | 정적 디자인 콘텐츠 → 외부 목 데이터 파일 분리 |
| **Generate Designs** | 주어진 데이터셋 → 새로운 Stitch 디자인 화면 생성 |

### 스킬 구조 요구사항

```
skills/new-skill/
├── SKILL.md           ← 에이전트 미션 컨트롤
├── scripts/           ← 검증 스크립트
├── resources/         ← 체크리스트, 가이드
└── examples/          ← 참조 예시
```

---

## 트러블슈팅

### 일반적인 문제

| 문제 | 해결 |
|------|------|
| **Permission Denied** | GCP 프로젝트 Owner/Editor 역할 확인 |
| **인증 URL 미표시** | 터미널 출력 확인 (5초 타임아웃) |
| **API 연결 실패** | `doctor --verbose` 실행 |
| **이미 인증됨 오류** | `logout --force --clear-config` |

### 진단 명령어

```bash
# 종합 진단
npx @_davideast/stitch-mcp doctor --verbose

# 인증 초기화
npx @_davideast/stitch-mcp logout --force
npx @_davideast/stitch-mcp init

# 디버그 로그 확인
tail -f /tmp/stitch-proxy-debug.log
```

---

## 핵심 포인트

1. **Google 공식 아님**: 실험적 독립 프로젝트 (David East 개발)
2. **Agent Skills 표준**: 오픈 스탠다드 준수로 다중 에이전트 호환
3. **MCP 프로토콜**: Google Cloud API와 AI 에이전트 간 브리지
4. **자동화 워크플로우**: 디자인 → 문서화 → 코드 변환 → 검증
5. **환경 적응성**: WSL, SSH, Docker 등 다양한 환경 지원
6. **토큰 관리**: 프록시 모드로 자동 토큰 갱신

---

## 주의사항

⚠️ **실험적 프로젝트**
- Google LLC와 **무관한** 독립 도구
- **보증 없음** (AS-IS 제공)
- **자체 위험 부담** 사용

---

## 참고 자료

- [Stitch Skills GitHub](https://github.com/google-labs-code/stitch-skills)
- [Stitch MCP GitHub](https://github.com/davideast/stitch-mcp)
- [Agent Skills 오픈 스탠다드](https://github.com/ComposioHQ/awesome-claude-skills)
- [MCP 프로토콜](https://github.com/modelcontextprotocol/servers)