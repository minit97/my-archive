# Eigent - 멀티 에이전트 Workforce 데스크톱 앱

> **경로**: `/study-github/eigent`  
> **유형**: 오픈소스 데스크톱 애플리케이션  
> **라이선스**: Apache 2.0  
> **사이트**: https://www.eigent.ai

---

## 요약

**Eigent**는 **CAMEL-AI** 프레임워크 기반의 오픈소스 멀티 에이전트 데스크톱 앱입니다. 복잡한 워크플로우를 자동화하는 **AI 팀(Workforce)**을 구축, 관리, 배포할 수 있습니다. 병렬 실행, 커스터마이징, 프라이버시 보호가 핵심 특징입니다.

---

## 주요 특징

### ⭐ 핵심 장점

| 특징 | 설명 |
|------|------|
| **100% 오픈소스** | 모든 코드, 커밋, 의사결정 투명 공개 |
| **로컬 배포** | 완전한 데이터 제어, 클라우드 계정 불필요 |
| **MCP 통합** | 외부 도구 연동 (Notion, Slack, Google 등) |
| **Zero Setup** | 기술적 설정 불필요 |
| **멀티 에이전트 협업** | 복잡한 멀티 에이전트 워크플로우 처리 |
| **엔터프라이즈 기능** | SSO / 접근 제어 |
| **커스텀 모델** | vLLM, Ollama, LM Studio 등 지원 |

---

## 핵심 개념

### 1. Workers (작업자)

특정 역할에 맞춤화된 **자율 에이전트**. 팀의 개별 멤버처럼 작동.

```
┌─────────────────────────────────────────────────────────────┐
│  Developer Agent   │  Browser Agent   │  Document Agent    │
│  Browser Agent     │  Multi-Modal Agent                    │
└─────────────────────────────────────────────────────────────┘
```

### 2. Workforce (작업팀)

복잡한 워크플로우를 완료하기 위해 **협업하는 Workers 팀**. AI 프로젝트 팀.

### 3. Workspace

Worker의 프로세스를 실시간으로 볼 수 있는 창 (터미널, 브라우저, 파일 뷰어).

### 4. Tasks & Subtasks

- 사용자가 **Task (미션)** 정의
- Workforce가 **Subtasks (하위 작업)**으로 분해
- 적절한 Workers에게 할당

### 5. MCP (Model Context Protocol)

Workers가 외부 도구를 사용할 수 있게 하는 프로토콜. DB, API, 문서 소스에 연결.

---

## 사전 정의 에이전트

### 🧑‍💻 Developer Agent

코드 작성/실행, 터미널 명령 실행

**장착 도구**:
- HumanToolkit
- TerminalToolkit
- NoteTakingToolkit
- WebDeployToolkit

### 🌐 Browser Agent

웹 검색, 웹페이지 콘텐츠 추출, 브라우저 액션 시뮬레이션

**장착 도구**:
- SearchToolkit
- HybridBrowserToolkit
- HumanToolkit
- NoteTakingToolkit
- TerminalToolkit

### 📄 Document Agent

문서 생성, 수정, 관리 (프레젠테이션 포함)

**장착 도구**:
- FileToolkit, PPTXToolkit
- ExcelToolkit, MarkItDownToolkit
- GoogleDriveMCPToolkit
- HumanToolkit, NoteTakingToolkit
- TerminalToolkit, SearchToolkit

### 🎨 Multi-Modal Agent

이미지, 오디오 등 미디어 콘텐츠 분석 및 생성

**장착 도구**:
- VideoDownloaderToolkit
- AudioAnalysisToolkit
- ImageAnalysisToolkit
- OpenAIImageToolkit
- HumanToolkit, TerminalToolkit
- NoteTakingToolkit, SearchToolkit

---

## 시스템 아키텍처

### 계층적 모듈 설계

```
┌─────────────────────────────────────────────────────────────┐
│                       Workforce                             │
│                    (전체 "팀")                               │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────────┐  ┌──────────────────┐                │
│  │  Coordinator     │  │  Task Planner    │                │
│  │  Agent           │  │  Agent           │                │
│  │  (프로젝트 매니저)  │  │  (전략 리드)      │                │
│  └──────────────────┘  └──────────────────┘                │
├─────────────────────────────────────────────────────────────┤
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐              │
│  │Worker 1│ │Worker 2│ │Worker 3│ │Worker N│              │
│  │(개발자) │ │(브라우저)│ │(문서)   │ │(멀티모달)│              │
│  └────────┘ └────────┘ └────────┘ └────────┘              │
└─────────────────────────────────────────────────────────────┘
```

### 역할 분담

| 역할 | 설명 |
|------|------|
| **Workforce** | 전체 "팀" |
| **Worker Nodes** | 개별 기여자 - 각 노드는 하나 이상의 에이전트 포함 |
| **Coordinator Agent** | "프로젝트 매니저" - 역할/스킬에 따라 작업 라우팅 |
| **Task Planner Agent** | "전략 리드" - 큰 작업을 작은 서브태스크로 분해 |

### 공유 태스크 채널

```
모든 작업 → 채널에 게시 → Worker들이 수락 → 결과 게시 → 다음 단계의 의존성으로 사용
```

### 실패 처리 (내장 견고성)

Worker가 작업 실패 시:

1. **분해 & 재시도**: 더 작은 조각으로 나눠 재할당
2. **에스컬레이션**: 계속 실패하면 해당 문제용 새 Worker 생성
3. **자동 중단**: 3회 이상 실패/분해 시 워크플로우 자동 중단

---

## Human-in-the-Loop

작업이 막히거나 불확실할 때 **자동으로 사람 입력 요청**.

```
Agent 작업 중 → 불확실 상황 → HumanToolkit 호출 → 사용자 응답 대기 → 계속 진행
```

---

## 기술 스택

### Backend

| 기술 | 용도 |
|------|------|
| **FastAPI** | 웹 프레임워크 |
| **uv** | 패키지 매니저 |
| **Uvicorn** | 비동기 서버 |
| **OAuth 2.0, Passlib** | 인증 |
| **CAMEL** | 멀티 에이전트 프레임워크 |

### Frontend

| 기술 | 용도 |
|------|------|
| **React** | UI 프레임워크 |
| **Electron** | 데스크톱 앱 프레임워크 |
| **TypeScript** | 언어 |
| **Tailwind CSS** | 스타일링 |
| **Radix UI** | UI 컴포넌트 |
| **Zustand** | 상태 관리 |
| **React Flow** | 플로우 에디터 |
| **Framer Motion** | 애니메이션 |

---

## 시작하기

### 빠른 시작 (Cloud-Connected)

```bash
git clone https://github.com/eigent-ai/eigent.git
cd eigent
npm install
npm run dev
```

### 로컬 배포 (권장)

```bash
# 프론트엔드
git clone https://github.com/eigent-ai/eigent.git
cd eigent
npm install
npm run dev

# 별도 터미널에서 백엔드 서버
cd eigent/server
docker compose up
```

### 환경 설정

`.env.development` 파일:
```
VITE_USE_LOCAL_PROXY=true
VITE_PROXY_URL=http://localhost:3001
TRACEROOT_ENABLE_SPAN_CLOUD_EXPORT=false
TRACEROOT_ENABLE_LOG_CLOUD_EXPORT=false
TRACEROOT_ENABLE_LOG_CONSOLE_EXPORT=false
```

---

## 사용 사례

| # | 사례 | 설명 |
|---|------|------|
| 1 | **여행 일정 + Slack** | 테니스 대회 여행 일정 생성 → HTML 리포트 → Slack 전송 |
| 2 | **재무 보고서** | CSV 은행 데이터 → Q2 재무제표 → 차트 포함 HTML |
| 3 | **시장 조사** | UK 헬스케어 산업 분석 → HTML 리포트 → Slack 알림 |
| 4 | **시장 진출 분석** | 독일 전동 스케이트보드 시장 → 진입 가능성 보고서 |
| 5 | **SEO 감사** | 웹사이트 SEO 분석 → 최적화 보고서 |
| 6 | **중복 파일 찾기** | 폴더 스캔 → 중복 파일 식별 |
| 7 | **PDF 서명** | PDF 서명 영역에 이미지 서명 추가 |

---

## 커스텀 Worker 생성

### 1. MCP 서버 설정

1. Settings → MCP and Tools 탭
2. + Add MCP Server 클릭
3. JSON 설정 붙여넣기 + 자격 증명 추가

### 2. Worker 생성

1. Canvas에서 + Add Worker 클릭
2. Worker 이름, 설명 입력
3. Agent Tool 드롭다운에서 MCP 서버 선택
4. Save

---

## 로드맵

| 주제 | 이슈 |
|------|------|
| **Context Engineering** | 프롬프트 캐싱, 시스템 프롬프트 최적화, 컨텍스트 압축 |
| **Multi-modal 강화** | 더 정확한 이미지 이해, 고급 비디오 생성 |
| **Multi-agent 시스템** | 고정 워크플로우 지원, 멀티 라운드 대화 |
| **Browser Toolkit** | BrowseComp 통합, 벤치마크 개선 |
| **Environment & RL** | 환경 설계, 데이터 생성, RL 프레임워크 통합 |

---

## 프로젝트 구조

```
eigent/
├── backend/              # Python 백엔드 (FastAPI)
│   ├── app/              # 앱 코드 (68 .py 파일)
│   ├── main.py           # 진입점
│   └── pyproject.toml    # Python 의존성
├── server/               # 서버 (Docker)
│   ├── app/              # 서버 앱 코드
│   ├── docker-compose.yml
│   └── alembic/          # DB 마이그레이션
├── electron/             # Electron 메인/프리로드
├── src/                  # React 프론트엔드 (291 파일)
├── docs/                 # Mintlify 문서
├── test/                 # 테스트
├── package.json          # npm 의존성
└── README.md
```

---

## 핵심 포인트

1. **CAMEL-AI 기반**: 검증된 오픈소스 멀티 에이전트 프레임워크 활용
2. **병렬 실행**: 여러 Worker가 동시에 작업 수행
3. **Human-in-the-Loop**: 불확실할 때 자동으로 사람 입력 요청
4. **MCP 통합**: Notion, Slack, Google, GitHub 등 외부 도구 연동
5. **로컬 우선**: 완전한 데이터 제어, 프라이버시 보호
6. **실패 복구**: 자동 분해, 재시도, 에스컬레이션

---

## 참고 사항

- **GitHub**: https://github.com/eigent-ai/eigent
- **공식 사이트**: https://www.eigent.ai
- **문서**: https://docs.eigent.ai
- **Discord**: https://discord.com/invite/CNcNpquyDc
- **CAMEL-AI**: https://www.camel-ai.org

---

## 관련 프로젝트

- **CAMEL-AI**: Eigent의 기반 멀티 에이전트 프레임워크
- **OpenSkills**: AI 에이전트용 스킬 로더 (09번 문서 참조)
