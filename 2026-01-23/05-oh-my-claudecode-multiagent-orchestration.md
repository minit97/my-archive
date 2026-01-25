# Oh My Claude Code - 멀티에이전트 오케스트레이션 프레임워크

> **URL**: https://github.com/Yeachan-Heo/oh-my-claudecode  
> **유형**: GitHub 레포지토리 (오픈소스 프레임워크)  
> **작성자**: Yeachan Heo (한국인 개발자)  
> **라이선스**: MIT  
> **Stars**: 1.8k ⭐ | **Forks**: 169

---

## 요약

Claude Code CLI를 위한 **멀티에이전트 오케스트레이션 프레임워크**. **28개의 전문 에이전트**와 **31개의 스킬**을 제공하며, 자연어만으로 복잡한 작업을 자동 병렬화한다. "학습 곡선 제로, 최대 파워"를 표방.

> "Zero learning curve. Maximum power."

---

## 핵심 특징

```
┌─────────────────────────────────────────────────────────────┐
│  🤖 28개 전문 에이전트                                       │
│     architect, researcher, designer, scientist 등            │
├─────────────────────────────────────────────────────────────┤
│  🎯 31개 스킬                                                │
│     orchestrate, ultrawork, ralph, planner, research 등      │
├─────────────────────────────────────────────────────────────┤
│  🧠 자동 의도 감지                                           │
│     자연어 입력 → 적절한 에이전트 자동 활성화                 │
├─────────────────────────────────────────────────────────────┤
│  ⚡ 병렬 실행                                                │
│     복잡한 작업을 자동으로 분해 & 병렬 처리                   │
├─────────────────────────────────────────────────────────────┤
│  📊 HUD 상태바                                               │
│     실시간 오케스트레이션 상태 시각화                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 설치 방법

### 1단계: 플러그인 설치

```bash
/plugin install oh-my-claudecode
```

### 2단계: 셋업 실행

```bash
/oh-my-claudecode:omc-setup
```

**끝!** 나머지는 자동.

---

## 자동 동작 원리

| 당신이... | 자동으로... |
|-----------|-------------|
| 복잡한 작업 부여 | 전문 에이전트로 병렬화 |
| "plan this" 말하기 | 플래닝 인터뷰 시작 |
| "don't stop until done" | 완료 검증까지 지속 |
| UI/프론트엔드 작업 | 디자인 감각 활성화 |
| 리서치 필요 | 전문 에이전트에 위임 |
| "build me..." 또는 autopilot | 전체 자율 워크플로우 실행 |

> **명령어 암기 불필요!** 자연어로 의도 감지 후 자동 활성화

---

## 매직 키워드 (선택적 단축어)

자연어로도 충분하지만, 정밀 제어가 필요할 때 사용:

| 키워드 | 효과 |
|--------|------|
| `ralph` | 끝날 때까지 멈추지 않음 (Persistence mode) |
| `ralplan` | 합의까지 반복 플래닝 |
| `ulw` | 최대 병렬 실행 (Ultrawork) |
| `plan` | 플래닝 인터뷰 시작 |
| `autopilot` / `ap` | 완전 자율 실행 |

**조합 예시:**
```
ralph ulw: migrate the database
```

---

## 28개 전문 에이전트

```
┌─────────────────────────────────────────────────────────────┐
│  📐 architect      │  시스템 설계                           │
│  🔬 researcher     │  리서치 & 탐색                         │
│  🎨 designer       │  UI/UX 디자인                          │
│  ✍️  writer        │  문서 작성                             │
│  👁️  vision       │  이미지/비전 분석                      │
│  🎯 critic         │  비평 & 리뷰                           │
│  📊 analyst        │  데이터 분석                           │
│  ⚡ executor       │  실행 & 구현                           │
│  📋 planner        │  계획 수립                             │
│  🧪 qa-tester      │  QA 테스팅                             │
│  🔭 explore        │  탐색 & 발견                           │
│  🧬 scientist      │  과학적 분석 (3-tier)                  │
│  ... 외 16개                                                │
└─────────────────────────────────────────────────────────────┘
```

---

## Scientist 에이전트 (데이터 분석 특화)

### 3-Tier 모델 라우팅

| 에이전트 | 모델 | 용도 |
|----------|------|------|
| `scientist-low` | Haiku | 빠른 데이터 검사, 간단한 통계 |
| `scientist` | Sonnet | 표준 분석, 패턴 감지, 시각화 |
| `scientist-high` | Opus | 복잡한 추론, 가설 검증, ML |

### 핵심 기능

```python
# 변수가 세션 간 유지됨 (Persistent Python REPL)
python_repl(action="execute", researchSessionID="analysis",
            code="import pandas as pd; df = pd.read_csv('data.csv')")

# df가 여전히 존재 - 다시 로드 불필요
python_repl(action="execute", researchSessionID="analysis",
            code="print(df.describe())")
```

- **구조화된 마커**: `[FINDING]`, `[STAT:*]`, `[DATA]`, `[LIMITATION]`
- **품질 게이트**: 모든 발견에 통계적 증거 필요 (CI, 효과 크기, p-value)
- **자동 시각화**: `.omc/scientist/figures/`에 차트 저장

---

## /research 명령어 (v3.3.8 신규)

```bash
/oh-my-claudecode:research <goal>          # 체크포인트 포함 표준 리서치
/oh-my-claudecode:research AUTO: <goal>    # 완료까지 완전 자율
/oh-my-claudecode:research status          # 현재 세션 확인
/oh-my-claudecode:research resume          # 중단된 세션 재개
/oh-my-claudecode:research report <id>     # 리포트 생성
```

### 리서치 프로토콜

```
┌─────────────────────────────────────────────────────────────┐
│  1. Decomposition                                           │
│     목표를 3-7개 독립 단계로 분해                            │
├─────────────────────────────────────────────────────────────┤
│  2. Parallel Execution                                      │
│     최대 5개 scientist 에이전트 동시 실행                    │
├─────────────────────────────────────────────────────────────┤
│  3. Cross-Validation                                        │
│     발견 간 일관성 검증                                      │
├─────────────────────────────────────────────────────────────┤
│  4. Synthesis                                               │
│     종합 마크다운 리포트 생성                                │
└─────────────────────────────────────────────────────────────┘
```

---

## HUD 상태바

실시간 오케스트레이션 상태 표시:

```
[OMC] | 5h:0% wk:100%(1d6h) | ctx:45% | agents:Ae
todos:3/5 (working: Implementing feature)
```

| 요소 | 의미 |
|------|------|
| `5h:0%` | 5시간 Rate limit 0% |
| `wk:100%(1d6h)` | 주간 한도 100%, 1일 6시간 후 리셋 |
| `ctx:45%` | 컨텍스트 45% 사용 |
| `agents:Ae` | 활성 에이전트 (코드화) |
| `todos:3/5` | 5개 중 3개 완료 |

---

## MCP 서버 지원

```bash
/oh-my-claudecode:mcp-setup
```

| 서버 | 설명 | API 키 |
|------|------|--------|
| **Context7** | 라이브러리 문서/컨텍스트 | 불필요 |
| **Exa** | 강화된 웹 검색 | 필요 |
| **Filesystem** | 확장 파일 시스템 | 불필요 |
| **GitHub** | GitHub API 연동 | 필요 (PAT) |

---

## 레포지토리 구조

```
oh-my-claudecode/
├── agents/           # 28개 전문 에이전트 정의
├── skills/           # 31개 스킬 정의
├── commands/         # 슬래시 명령어
├── hooks/            # 이벤트 훅
├── bridge/           # 브릿지 모듈
├── templates/        # 템플릿
├── docs/             # 문서
├── examples/         # 예제
└── src/              # 소스 코드 (TypeScript)
```

---

## 2.x에서 마이그레이션

**기존 명령어 계속 작동!**

```bash
/oh-my-claudecode:ralph "task"      # 여전히 작동 (또는 "ralph: task")
/oh-my-claudecode:ultrawork "task"  # 여전히 작동 (또는 "ulw" 키워드)
/oh-my-claudecode:planner "task"    # 여전히 작동 (또는 "plan this")
```

차이점: 이제 명령어 **없이도** 자동 활성화.

---

## 영감을 준 프로젝트

- [oh-my-opencode](https://github.com/travisennis/oh-my-opencode)
- [claude-hud](https://github.com/example/claude-hud)
- [Superpowers](https://github.com/example/superpowers)
- [everything-claude-code](https://github.com/affaan-m/everything-claude-code)

---

## 요구사항

- Claude Code CLI
- 다음 중 하나:
  - **Claude Max/Pro 구독** (개인 추천)
  - **Anthropic API 키** (API 기반 사용)

---

## 핵심 포인트

1. **학습 곡선 제로**: 자연어만으로 모든 기능 사용
2. **28개 전문 에이전트**: 역할별 최적화된 AI 팀
3. **자동 의도 감지**: 명령어 암기 불필요
4. **병렬 실행**: 복잡한 작업 자동 분해 & 동시 처리
5. **Scientist 3-tier**: 데이터 분석 작업 모델별 라우팅
6. **HUD 상태바**: 실시간 오케스트레이션 모니터링

---

## 참고 자료

- [공식 웹사이트](https://yeachan-heo.github.io/oh-my-claudecode-website)
- [Full Reference (800+ lines)](https://github.com/Yeachan-Heo/oh-my-claudecode/tree/main/docs)
- [Migration Guide](https://github.com/Yeachan-Heo/oh-my-claudecode/blob/main/docs/migration.md)
- [Architecture](https://github.com/Yeachan-Heo/oh-my-claudecode/blob/main/docs/architecture.md)
