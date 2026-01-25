# Ralph Claude Code - 자율 AI 개발 루프 구현체

> **URL**: https://github.com/frankbria/ralph-claude-code  
> **유형**: GitHub 레포지토리  
> **Stars**: 4.9k ⭐ | Forks: 324  
> **라이선스**: MIT

---

## 요약

Geoffrey Huntley의 **Ralph 기법**을 Claude Code에 적용한 구현체입니다. AI 코딩 에이전트를 무한 루프로 실행하면서 **지능형 종료 감지**, **속도 제한**, **회로 차단기 패턴** 등을 추가하여 실용적으로 사용할 수 있게 만들었습니다.

## 주요 기능

### 핵심 기능
| 기능 | 설명 |
|------|------|
| **지능형 종료 감지** | 작업 완료를 자동으로 감지하여 루프 종료 |
| **이중 조건 종료 게이트** | 완료 지표 + EXIT_SIGNAL 두 조건 모두 충족 시 종료 |
| **속도 제한** | 시간당 100 API 호출 제한 (설정 가능) |
| **회로 차단기 패턴** | 반복 실패 시 자동 중단 |
| **세션 연속성** | 세션 상태 유지 및 자동 리셋 |
| **tmux 통합** | 실시간 모니터링 대시보드 |

### 모니터링
```bash
# 통합 tmux 모니터링 (권장)
ralph --monitor

# 수동 모니터링
ralph-monitor
```

실시간으로 확인 가능:
- 현재 루프 횟수 및 상태
- API 호출 사용량 vs 제한
- 최근 로그 항목
- 속도 제한 카운트다운

## 설치 방법

### 사전 요구사항
- **Claude Code CLI** 설치
- **tmux** 설치
- **GNU coreutils** (macOS의 경우)

```bash
# tmux 설치
brew install tmux          # macOS
sudo apt-get install tmux  # Ubuntu/Debian

# GNU coreutils (macOS)
brew install coreutils
```

### 설치
```bash
# 클론
git clone https://github.com/frankbria/ralph-claude-code.git
cd ralph-claude-code

# 전역 설치
./install.sh
```

## 사용법

### 기본 사용
```bash
# 새 프로젝트 생성
ralph-setup my-project

# Ralph 시작 (모니터링 포함)
ralph --monitor

# 상태 확인
ralph --status
```

### 주요 옵션
```bash
ralph [OPTIONS]
  -h, --help              도움말 표시
  -c, --calls NUM         시간당 최대 호출 수 (기본: 100)
  -p, --prompt FILE       프롬프트 파일 지정 (기본: PROMPT.md)
  -s, --status            현재 상태 표시 후 종료
  -m, --monitor           tmux 세션과 라이브 모니터 시작
  -v, --verbose           상세 진행 상황 표시
  -t, --timeout MIN       실행 타임아웃 (1-120분, 기본: 15)
  --reset-session         세션 상태 수동 리셋
  --reset-circuit         회로 차단기 리셋
```

### PRD를 프로젝트로 변환
```bash
ralph-import prd.md project-name
```

## 프로젝트 구조

```
ralph-claude-code/
├── .claude/              # Claude 설정
├── docs/                 # 문서
├── examples/             # 예제
├── lib/                  # 라이브러리
├── src/                  # 소스 코드
├── templates/            # 프로젝트 템플릿
├── tests/                # 테스트 (308개, 100% 통과)
├── CLAUDE.md             # Claude 지침
├── CONTRIBUTING.md       # 기여 가이드
└── IMPLEMENTATION_PLAN.md # 구현 계획
```

## 테스트 현황

| 항목 | 수치 |
|------|------|
| 총 테스트 | 308개 |
| 통과율 | **100%** |
| 유닛 테스트 | 164개 |
| 통합 테스트 | 144개 |
| 테스트 파일 | 11개 |

```bash
# 테스트 실행
npm test
bats tests/unit/*.bats
bats tests/integration/*.bats
```

## tmux 컨트롤

| 단축키 | 동작 |
|--------|------|
| `Ctrl+B` → `D` | 세션 분리 (Ralph 계속 실행) |
| `Ctrl+B` → `←/→` | 패널 전환 |
| `tmux list-sessions` | 활성 세션 목록 |
| `tmux attach -t <name>` | 세션 재연결 |

## 문제 해결

| 문제 | 해결책 |
|------|--------|
| **속도 제한** | 자동 대기 및 카운트다운 표시 |
| **5시간 API 제한** | 사용자 액션 프롬프트 (대기 또는 종료) |
| **막힌 루프** | `@fix_plan.md` 확인 |
| **조기 종료** | 종료 임계값 검토 |
| **timeout 명령어 없음 (macOS)** | `brew install coreutils` |

## 개발 로드맵

### 현재: v0.9.9

**완료된 기능:**
- ✅ 핵심 루프 기능 + 지능형 종료 감지
- ✅ 이중 조건 종료 게이트
- ✅ 속도 제한 및 회로 차단기 패턴
- ✅ 의미론적 응답 분석기
- ✅ tmux 통합 및 라이브 모니터링
- ✅ PRD 임포트 기능
- ✅ 세션 수명 관리

### v1.0.0 목표 (~4주)
- 로그 로테이션
- Dry-run 모드
- 설정 파일 지원 (.ralphrc)
- 메트릭 및 분석
- 데스크톱 알림
- Git 백업/롤백 시스템

## 핵심 포인트

- 🔄 Geoffrey Huntley의 Ralph 기법을 **실용적으로 구현**
- 🛡️ 회로 차단기, 속도 제한 등 **안전장치 내장**
- 📊 tmux 기반 **실시간 모니터링**
- 🧪 308개 테스트로 **100% 통과율** 유지
- 🚀 PRD → 프로젝트 자동 변환 지원

## 참고

- [원본 Ralph 기법](https://ghuntley.com/ralph/) - Geoffrey Huntley
- [Claude Code](https://www.anthropic.com/) - Anthropic
