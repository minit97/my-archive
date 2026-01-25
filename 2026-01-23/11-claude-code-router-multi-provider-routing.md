# Claude Code Router - 멀티 프로바이더 모델 라우팅

> **URL**: https://github.com/musistudio/claude-code-router  
> **유형**: GitHub 레포지토리 (인프라 도구)  
> **작성자**: musistudio  
> **라이선스**: MIT  
> **Stars**: 26.3k ⭐ | **Forks**: 2.1k

---

## 요약

Claude Code를 **코딩 인프라 기반**으로 유지하면서 **다양한 AI 프로바이더**로 요청을 라우팅할 수 있게 해주는 도구. Anthropic의 Claude Code 업데이트를 계속 받으면서도 OpenAI, DeepSeek, Gemini, OpenRouter 등 원하는 모델과 상호작용할 수 있다. 26.3k Stars로 가장 인기 있는 Claude Code 확장 도구.

> "Use Claude Code as the foundation, decide how to interact with the model."

---

## 핵심 기능

```
┌─────────────────────────────────────────────────────────────┐
│  🔄 멀티 프로바이더 라우팅                                   │
│     OpenAI, DeepSeek, Gemini, Azure, Bedrock 등 지원         │
├─────────────────────────────────────────────────────────────┤
│  🎯 시나리오별 모델 선택                                     │
│     default, background, think, longContext, webSearch      │
├─────────────────────────────────────────────────────────────┤
│  💰 비용 최적화                                              │
│     작업별 적절한 모델로 라우팅하여 비용 절감                 │
├─────────────────────────────────────────────────────────────┤
│  🔧 커스텀 라우터                                            │
│     JavaScript로 복잡한 라우팅 로직 구현 가능                │
├─────────────────────────────────────────────────────────────┤
│  📊 Status Line (Beta)                                      │
│     런타임 모니터링 UI                                       │
├─────────────────────────────────────────────────────────────┤
│  🤖 GitHub Actions 통합                                     │
│     CI/CD 파이프라인에서 사용 가능                           │
└─────────────────────────────────────────────────────────────┘
```

---

## 지원 프로바이더

| 프로바이더 | 설명 |
|-----------|------|
| **OpenAI** | GPT-4, GPT-4o 등 |
| **DeepSeek** | DeepSeek-Chat, DeepSeek-Coder |
| **Google Gemini** | Gemini Pro, Flash 등 |
| **OpenRouter** | 100+ 모델 통합 게이트웨이 |
| **Azure OpenAI** | Azure 호스팅 OpenAI |
| **AWS Bedrock** | Claude via AWS |
| **Google Vertex AI** | Claude via GCP |
| **Ollama** | 로컬 모델 |
| **Together AI** | 오픈소스 모델 호스팅 |
| **Mistral** | Mistral 모델 |

---

## 설치 및 시작

### 1단계: 설치

```bash
# npm
npm install -g @musistudio/claude-code-router

# 또는 bunx (권장)
bunx @musistudio/claude-code-router start
```

### 2단계: 설정 파일 생성

```bash
# 설정 디렉토리 생성
mkdir -p ~/.claude-code-router

# config.json 생성
cat > ~/.claude-code-router/config.json << 'EOF'
{
  "log": true,
  "OPENAI_API_KEY": "your-api-key",
  "OPENAI_BASE_URL": "https://api.openai.com/v1",
  "OPENAI_MODEL": "gpt-4o"
}
EOF
```

### 3단계: 라우터 시작

```bash
# 라우터 시작 (기본 포트 3456)
bunx @musistudio/claude-code-router start
```

### 4단계: Claude Code에 연결

```bash
# 환경변수 설정
export ANTHROPIC_BASE_URL=http://localhost:3456

# Claude Code 시작
claude
```

---

## 라우터 설정

### 시나리오별 모델 라우팅

```json
{
  "router": {
    "default": "openai,gpt-4o",
    "background": "openai,gpt-4o-mini",
    "think": "openai,o1-preview",
    "longContext": "google,gemini-1.5-pro",
    "longContextThreshold": 60000,
    "webSearch": "openrouter,perplexity/llama-3.1-sonar-large-128k-online",
    "image": "openai,gpt-4o"
  }
}
```

| 시나리오 | 용도 |
|----------|------|
| `default` | 일반 작업용 기본 모델 |
| `background` | 백그라운드 작업 (비용 절감용 작은 모델) |
| `think` | 추론 집약 작업 (Plan Mode 등) |
| `longContext` | 긴 컨텍스트 처리 (>60K 토큰) |
| `longContextThreshold` | longContext 트리거 토큰 수 (기본 60000) |
| `webSearch` | 웹 검색 작업 (모델 자체 지원 필요) |
| `image` | 이미지 관련 작업 |

---

## 커스텀 라우터

### 설정

```json
{
  "CUSTOM_ROUTER_PATH": "/User/xxx/.claude-code-router/custom-router.js"
}
```

### 커스텀 라우터 예시

```javascript
// custom-router.js
module.exports = async function router(req, config) {
  const userMessage = req.body.messages.find((m) => m.role === "user")?.content;

  // 코드 설명 요청 → 강력한 모델 사용
  if (userMessage && userMessage.includes("explain this code")) {
    return "openrouter,anthropic/claude-3.5-sonnet";
  }

  // 기본 라우터로 폴백
  return null;
};
```

---

## 프로바이더별 설정 예시

### OpenAI

```json
{
  "OPENAI_API_KEY": "sk-xxx",
  "OPENAI_BASE_URL": "https://api.openai.com/v1",
  "OPENAI_MODEL": "gpt-4o"
}
```

### DeepSeek

```json
{
  "OPENAI_API_KEY": "your-deepseek-key",
  "OPENAI_BASE_URL": "https://api.deepseek.com",
  "OPENAI_MODEL": "deepseek-chat"
}
```

### Google Gemini

```json
{
  "GOOGLE_API_KEY": "your-google-key",
  "GOOGLE_MODEL": "gemini-1.5-pro"
}
```

### OpenRouter (100+ 모델)

```json
{
  "OPENROUTER_API_KEY": "your-openrouter-key",
  "OPENROUTER_MODEL": "anthropic/claude-3.5-sonnet"
}
```

### Ollama (로컬)

```json
{
  "OLLAMA_BASE_URL": "http://localhost:11434",
  "OLLAMA_MODEL": "llama3"
}
```

---

## 동적 모델 전환

Claude Code 내에서 `/model` 명령으로 실시간 모델 전환:

```
/model openrouter,anthropic/claude-3.5-sonnet
/model openai,gpt-4o
/model google,gemini-1.5-pro
```

---

## 서브에이전트 라우팅

서브에이전트 프롬프트 시작 부분에 모델 지정:

```markdown
<CCR-SUBAGENT-MODEL>openrouter,anthropic/claude-3.5-sonnet</CCR-SUBAGENT-MODEL>
이 코드 스니펫을 최적화 가능성 관점에서 분석해주세요...
```

---

## Status Line (Beta)

런타임 모니터링 UI:

```
┌─────────────────────────────────────────────────────────────┐
│  [CCR] Provider: openai | Model: gpt-4o | Tokens: 12.5k    │
│  Router: default | Latency: 1.2s | Cost: $0.03             │
└─────────────────────────────────────────────────────────────┘
```

설정에서 활성화:
```json
{
  "UI": {
    "statusline": true
  }
}
```

---

## GitHub Actions 통합

```yaml
name: Claude Code

on:
  issue_comment:
    types: [created]

jobs:
  claude:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Prepare Environment
        run: |
          curl -fsSL https://bun.sh/install | bash
          mkdir -p $HOME/.claude-code-router
          cat << 'EOF' > $HOME/.claude-code-router/config.json
          {
            "log": true,
            "NON_INTERACTIVE_MODE": true,
            "OPENAI_API_KEY": "${{ secrets.OPENAI_API_KEY }}",
            "OPENAI_BASE_URL": "https://api.deepseek.com",
            "OPENAI_MODEL": "deepseek-chat"
          }
          EOF

      - name: Start Claude Code Router
        run: |
          nohup ~/.bun/bin/bunx @musistudio/claude-code-router@1.0.8 start &

      - name: Run Claude Code
        uses: anthropics/claude-code-action@beta
        env:
          ANTHROPIC_BASE_URL: http://localhost:3456
        with:
          anthropic_api_key: "any-string-is-ok"
```

> **참고**: 자동화 환경에서는 `"NON_INTERACTIVE_MODE": true` 설정 필수

---

## 커스텀 트랜스포머

요청/응답 변환을 위한 플러그인:

```json
{
  "transformers": [
    {
      "path": "/User/xxx/.claude-code-router/plugins/gemini-cli.js",
      "options": {
        "project": "xxx"
      }
    }
  ]
}
```

---

## 비용 최적화 전략

```
┌─────────────────────────────────────────────────────────────┐
│  작업 유형            →  권장 모델             →  비용       │
├─────────────────────────────────────────────────────────────┤
│  간단한 수정/포맷팅    →  gpt-4o-mini / DeepSeek →  $       │
│  일반 코딩            →  gpt-4o / Claude Sonnet →  $$      │
│  복잡한 추론          →  o1-preview / Opus     →  $$$     │
│  긴 컨텍스트          →  Gemini 1.5 Pro        →  $$      │
│  백그라운드 작업       →  로컬 Ollama           →  무료     │
└─────────────────────────────────────────────────────────────┘
```

### 예시 설정 (비용 최적화)

```json
{
  "router": {
    "default": "openai,gpt-4o",
    "background": "ollama,llama3",
    "think": "openai,o1-preview",
    "longContext": "google,gemini-1.5-pro"
  }
}
```

---

## 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│                      Claude Code CLI                        │
│                           │                                 │
│                           ▼                                 │
│  ┌─────────────────────────────────────────────────────┐   │
│  │           Claude Code Router (localhost:3456)        │   │
│  │                                                      │   │
│  │   ┌──────────────────────────────────────────────┐  │   │
│  │   │             라우터 로직                       │  │   │
│  │   │  - 시나리오 감지 (default/think/long...)     │  │   │
│  │   │  - 커스텀 라우터 실행                        │  │   │
│  │   │  - 모델 선택                                 │  │   │
│  │   └──────────────────────────────────────────────┘  │   │
│  │                         │                            │   │
│  │          ┌──────────────┼──────────────┐            │   │
│  │          ▼              ▼              ▼            │   │
│  │   ┌──────────┐   ┌──────────┐   ┌──────────┐       │   │
│  │   │  OpenAI  │   │ DeepSeek │   │  Gemini  │       │   │
│  │   └──────────┘   └──────────┘   └──────────┘       │   │
│  │          ▼              ▼              ▼            │   │
│  │   ┌──────────┐   ┌──────────┐   ┌──────────┐       │   │
│  │   │  Ollama  │   │ OpenRouter│   │  Azure   │       │   │
│  │   └──────────┘   └──────────┘   └──────────┘       │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## 유사 도구 비교

| 도구 | Stars | 특징 |
|------|-------|------|
| **Claude Code Router** | 26.3k | 멀티 프로바이더 라우팅, 비용 최적화 |
| **Awesome Claude Skills** | 24.4k | 스킬 큐레이션 |
| **Auto-Claude** | 9.9k | 데스크톱 앱, 멀티세션 |
| **Oh My Claude Code** | 1.8k | 28 에이전트, 오케스트레이션 |

---

## 핵심 포인트

1. **26.3k Stars**: 가장 인기 있는 Claude Code 확장
2. **멀티 프로바이더**: OpenAI, DeepSeek, Gemini, Ollama 등 10+ 프로바이더
3. **시나리오별 라우팅**: 작업 유형에 따라 최적 모델 자동 선택
4. **비용 최적화**: 간단한 작업은 저렴한 모델로, 복잡한 작업은 강력한 모델로
5. **커스텀 라우터**: JavaScript로 복잡한 라우팅 로직 구현
6. **GitHub Actions**: CI/CD 통합 지원
7. **동적 전환**: `/model` 명령으로 실시간 모델 변경

---

## 참고 자료

- [GitHub 레포지토리](https://github.com/musistudio/claude-code-router)
- [공식 문서](https://musistudio.github.io/claude-code-router/)
- [Project Motivation](https://github.com/musistudio/claude-code-router/blob/main/blog/motivation.md)
- [Maybe We Can Do More with the Router](https://github.com/musistudio/claude-code-router/blob/main/blog/more.md)
