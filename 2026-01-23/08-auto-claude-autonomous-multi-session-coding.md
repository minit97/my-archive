# Auto-Claude - 자율 멀티세션 AI 코딩 데스크톱 앱

> **URL**: https://github.com/AndyMik90/Auto-Claude  
> **유형**: GitHub 레포지토리 (데스크톱 애플리케이션)  
> **작성자**: AndyMik90  
> **라이선스**: AGPL-3.0  
> **Stars**: 9.9k ⭐ | **Forks**: 1.5k | **버전**: v2.7.5

---

## 요약

**자율 멀티세션 AI 코딩** 데스크톱 애플리케이션. 최대 12개의 에이전트 터미널을 병렬로 실행하며, Git Worktree 기반 격리된 작업 공간에서 계획, 구현, 검증까지 자동으로 수행한다. Windows, macOS, Linux 네이티브 앱 제공.

> "Describe your goal; agents handle planning, implementation, and validation."

---

## 핵심 기능

```
┌─────────────────────────────────────────────────────────────┐
│  🤖 Autonomous Tasks                                        │
│     목표 설명 → 에이전트가 계획, 구현, 검증 자동 수행         │
├─────────────────────────────────────────────────────────────┤
│  ⚡ Parallel Execution                                      │
│     최대 12개 에이전트 터미널 동시 실행                       │
├─────────────────────────────────────────────────────────────┤
│  🔒 Isolated Workspaces                                     │
│     Git Worktree 기반 격리 - 메인 브랜치 안전                 │
├─────────────────────────────────────────────────────────────┤
│  ✅ Self-Validating QA                                      │
│     내장 QA 루프가 리뷰 전 이슈 자동 검출                     │
├─────────────────────────────────────────────────────────────┤
│  🔀 AI-Powered Merge                                        │
│     메인 브랜치 통합 시 충돌 자동 해결                        │
├─────────────────────────────────────────────────────────────┤
│  🧠 Memory Layer                                            │
│     세션 간 인사이트 유지 → 더 스마트한 빌드                  │
├─────────────────────────────────────────────────────────────┤
│  🔗 GitHub/GitLab/Linear 연동                               │
│     이슈 가져오기, AI 조사, MR 생성                          │
├─────────────────────────────────────────────────────────────┤
│  🖥️ Cross-Platform                                          │
│     Windows, macOS (Intel/Apple Silicon), Linux             │
└─────────────────────────────────────────────────────────────┘
```

---

## 다운로드

### Stable Release (v2.7.5)

| 플랫폼 | 다운로드 |
|--------|----------|
| **Windows** | `Auto-Claude-2.7.5-win32-x64.exe` |
| **macOS (Apple Silicon)** | `Auto-Claude-2.7.5-darwin-arm64.dmg` |
| **macOS (Intel)** | `Auto-Claude-2.7.5-darwin-x64.dmg` |
| **Linux (AppImage)** | `Auto-Claude-2.7.5-linux-x86_64.AppImage` |
| **Linux (Debian)** | `Auto-Claude-2.7.5-linux-amd64.deb` |
| **Linux (Flatpak)** | `Auto-Claude-2.7.5-linux-x86_64.flatpak` |

---

## 요구사항

- **Claude Pro/Max 구독** 필수
- **Claude Code CLI**: `npm install -g @anthropic-ai/claude-code`
- **Git 레포지토리**: 프로젝트가 git repo로 초기화되어 있어야 함

---

## 빠른 시작

```
1. 앱 다운로드 및 설치
       ↓
2. 프로젝트 열기 (Git 레포지토리 폴더 선택)
       ↓
3. Claude 연결 (OAuth 설정 가이드 제공)
       ↓
4. 태스크 생성 (빌드할 내용 설명)
       ↓
5. 작업 관찰 (에이전트가 자율적으로 계획-코딩-검증)
```

---

## 인터페이스

### 1️⃣ Kanban Board

```
┌──────────────┬──────────────┬──────────────┬──────────────┐
│   Backlog    │  In Progress │   Review     │    Done      │
├──────────────┼──────────────┼──────────────┼──────────────┤
│  Task 1      │  Task 3      │  Task 5      │  Task 7      │
│  Task 2      │  Task 4      │  Task 6      │  Task 8      │
│              │   🤖 Agent   │              │              │
└──────────────┴──────────────┴──────────────┴──────────────┘
```

- 계획 → 완료까지 시각적 태스크 관리
- 실시간 에이전트 진행 상황 모니터링

### 2️⃣ Agent Terminals

```
┌─────────────────────────────────────────────────────────────┐
│  Terminal 1: Backend API    │  Terminal 2: Frontend UI     │
│  🤖 Implementing auth...    │  🤖 Building components...   │
├─────────────────────────────┼─────────────────────────────  │
│  Terminal 3: Database       │  Terminal 4: Tests           │
│  🤖 Setting up schema...    │  🤖 Writing unit tests...    │
└─────────────────────────────┴─────────────────────────────  ┘
```

- 원클릭 태스크 컨텍스트 주입
- 병렬 작업을 위한 다중 에이전트 스폰

### 3️⃣ Roadmap

- AI 지원 기능 계획
- 경쟁사 분석
- 타겟 오디언스 정의

### 4️⃣ 추가 기능

| 기능 | 설명 |
|------|------|
| **Insights** | 코드베이스 탐색 채팅 인터페이스 |
| **Ideation** | 개선점, 성능 이슈, 취약점 발견 |
| **Changelog** | 완료된 태스크에서 릴리스 노트 자동 생성 |

---

## 프로젝트 구조

```
Auto-Claude/
├── apps/
│   ├── backend/     # Python 에이전트, 스펙, QA 파이프라인
│   └── frontend/    # Electron 데스크톱 애플리케이션
├── guides/          # 추가 문서
├── tests/           # 테스트 스위트
└── scripts/         # 빌드 유틸리티
```

---

## CLI 사용법

헤드리스 운영, CI/CD 통합, 터미널 전용 워크플로우:

```bash
cd apps/backend

# 대화형으로 스펙 생성
python spec_runner.py --interactive

# 자율 빌드 실행
python run.py --spec 001

# 리뷰 및 머지
python run.py --spec 001 --review
python run.py --spec 001 --merge
```

---

## 워크플로우

```
┌─────────────────────────────────────────────────────────────┐
│  1. Task 생성                                               │
│     "사용자 인증 시스템 구현해줘"                             │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  2. Spec 생성 (AI)                                          │
│     요구사항 분석 → specs/001-auth-system.md                 │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  3. Parallel Build (Git Worktree)                           │
│     ┌─────────────┐ ┌─────────────┐ ┌─────────────┐        │
│     │ Agent 1     │ │ Agent 2     │ │ Agent 3     │        │
│     │ Backend API │ │ DB Schema   │ │ Tests       │        │
│     └─────────────┘ └─────────────┘ └─────────────┘        │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  4. Self-Validating QA                                      │
│     테스트 실행 → 이슈 검출 → 자동 수정                       │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  5. AI-Powered Merge                                        │
│     충돌 자동 해결 → 메인 브랜치 통합                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 보안 모델 (3계층)

```
┌─────────────────────────────────────────────────────────────┐
│  Layer 1: OS Sandbox                                        │
│     Bash 명령이 격리된 환경에서 실행                          │
├─────────────────────────────────────────────────────────────┤
│  Layer 2: Filesystem Restrictions                           │
│     작업이 프로젝트 디렉토리로 제한                           │
├─────────────────────────────────────────────────────────────┤
│  Layer 3: Dynamic Command Allowlist                         │
│     감지된 프로젝트 스택 기반 승인된 명령만 실행              │
└─────────────────────────────────────────────────────────────┘
```

**추가 보안:**
- VirusTotal 스캔 후 릴리스
- SHA256 체크섬 제공
- macOS 코드 서명

---

## npm 스크립트

| 명령 | 설명 |
|------|------|
| `npm run install:all` | 백엔드 & 프론트엔드 의존성 설치 |
| `npm start` | 데스크톱 앱 빌드 & 실행 |
| `npm run dev` | 개발 모드 (핫 리로드) |
| `npm run package` | 현재 플랫폼용 패키징 |
| `npm run package:mac` | macOS용 패키징 |
| `npm run package:win` | Windows용 패키징 |
| `npm run package:linux` | Linux용 패키징 |
| `npm run package:flatpak` | Flatpak 패키징 |
| `npm test` | 프론트엔드 테스트 |
| `npm run test:backend` | 백엔드 테스트 |

---

## 유사 도구 비교

| 도구 | 특징 | 차이점 |
|------|------|--------|
| **Auto-Claude** | 데스크톱 앱, 멀티세션, Kanban | GUI 중심, 시각적 관리 |
| **Oh My Claude Code** | CLI 플러그인, 28 에이전트 | CLI 중심, 명령어 기반 |
| **Everything Claude Code** | 설정 모음집 | 프레임워크, 직접 설정 |
| **Claude Code Action** | GitHub Action | CI/CD 통합 전용 |

---

## 기술 스택

```
Frontend: Electron + TypeScript (57.1%)
Backend:  Python (41.3%)
```

---

## 핵심 포인트

1. **데스크톱 네이티브**: Windows/macOS/Linux GUI 앱
2. **최대 12개 병렬 에이전트**: 대규모 병렬 코딩
3. **Git Worktree 격리**: 메인 브랜치 안전 보장
4. **Self-Validating QA**: 내장 품질 검증 루프
5. **AI-Powered Merge**: 충돌 자동 해결
6. **Memory Layer**: 세션 간 컨텍스트 유지
7. **9.9k Stars**: 활발한 커뮤니티

---

## 커뮤니티

- **Discord**: 커뮤니티 참여
- **Issues**: 버그 리포트/기능 요청
- **Discussions**: Q&A

---

## 참고 자료

- [GitHub 레포지토리](https://github.com/AndyMik90/Auto-Claude)
- [CLI 사용 가이드](https://github.com/AndyMik90/Auto-Claude/blob/develop/guides/CLI-USAGE.md)
- [Linux 빌드 가이드](https://github.com/AndyMik90/Auto-Claude/blob/develop/guides/linux.md)
- [CONTRIBUTING.md](https://github.com/AndyMik90/Auto-Claude/blob/develop/CONTRIBUTING.md)
