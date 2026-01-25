# Plugins for Claude Natives - 파워 유저를 위한 Claude Code 플러그인 모음

> **URL**: https://github.com/team-attention/plugins-for-claude-natives  
> **유형**: GitHub Repository  
> **Stars**: 425 ⭐ | **Forks**: 46

---

## 요약

Claude Code의 기본 기능을 확장하고 싶은 파워 유저를 위한 플러그인 컬렉션입니다. 멀티 AI 모델 합의, 요구사항 명확화, 유튜브 요약, 카카오톡 연동 등 다양한 생산성 플러그인을 제공합니다.

---

## 빠른 시작

```bash
# 마켓플레이스 추가
/plugin marketplace add team-attention/plugins-for-claude-natives

# 플러그인 설치
/plugin install <plugin-name>
```

---

## 플러그인 목록

| 플러그인 | 설명 |
|---------|------|
| **agent-council** | 여러 AI 모델(Gemini, GPT, Codex)의 의견을 수집하여 합의 도출 |
| **clarify** | 모호한 요구사항을 반복 질문을 통해 정확한 스펙으로 변환 |
| **dev** | 개발자 커뮤니티 스캔 + 기술 의사결정 분석 |
| **interactive-review** | 웹 UI를 통한 계획/문서 시각적 리뷰 |
| **say-summary** | macOS TTS로 Claude 응답 음성 출력 (한/영) |
| **youtube-digest** | 유튜브 영상 요약, 한글 번역, 퀴즈 생성 |
| **google-calendar** | 멀티 계정 Google Calendar 통합 |
| **kakaotalk** | macOS에서 카카오톡 메시지 송수신 |
| **session-wrap** | 세션 종료 요약 + 히스토리 분석 툴킷 |

---

## 주요 플러그인 상세

### 1. agent-council (AI 협의회)

여러 AI 모델에게 동시에 질문하여 다양한 관점의 합의된 답변을 얻습니다.

**트리거 문구:**
- "summon the council"
- "ask other AIs"
- "what do other models think?"

**작동 방식:**
1. 질문을 여러 AI 에이전트에 동시 전송
2. 각 에이전트가 관점 제시
3. Claude가 응답을 합의 의견으로 통합

```
예시: "summon the council - TypeScript vs JavaScript 어떤 걸 써야 할까?"
```

---

### 2. clarify (요구사항 명확화)

모호한 지시사항을 코드 작성 전에 명확한 스펙으로 변환합니다.

**트리거 문구:**
- "/clarify"
- "clarify requirements"

**변환 예시:**

| Before | After |
|--------|-------|
| "로그인 기능 추가해줘" | 목표: 사용자명/비밀번호 로그인 + 자가등록<br>범위: 로그인, 로그아웃, 회원가입, 비밀번호 재설정<br>제약: 24시간 세션, bcrypt, 5회 시도 제한 |

---

### 3. dev (개발자 워크플로우)

#### `/dev-scan` - 커뮤니티 스캔
- Reddit, Hacker News, Dev.to, Lobsters 병렬 검색
- 합의, 논쟁점, 주목할 의견 종합

#### `/tech-decision` - 기술 의사결정 분석
4개 에이전트가 병렬로 분석:

```
Phase 1: 정보 수집 (병렬)
┌─────────────┬─────────────┬─────────────┬─────────────┐
│ codebase-   │ docs-       │ dev-scan    │ agent-      │
│ explorer    │ researcher  │ (커뮤니티)   │ council     │
└─────────────┴─────────────┴─────────────┴─────────────┘
                           ↓
Phase 2: 분석 & 종합
┌─────────────────────────────────────────────────────────┐
│              tradeoff-analyzer                          │
│              decision-synthesizer                       │
└─────────────────────────────────────────────────────────┘
```

**트리거:** "A vs B", "어떤 라이브러리를 써야 할까", "기술 의사결정"

---

### 4. youtube-digest (유튜브 요약)

유튜브 URL을 입력하면 완전한 분석 제공:

1. **요약** - 3-5문장 개요
2. **인사이트** - 실행 가능한 핵심 포인트
3. **전체 자막** - 한글 번역 + 타임스탬프
4. **3단계 퀴즈** - 기초/중급/고급 (총 9문제)
5. **심층 리서치** (선택) - 웹 검색으로 주제 확장

**출력 위치:** `research/readings/youtube/YYYY-MM-DD-title.md`

---

### 5. kakaotalk (카카오톡 연동)

macOS에서 카카오톡 메시지 송수신 (Accessibility API 활용)

**트리거 문구:**
- "카톡 보내줘", "카카오톡 메시지"
- "~에게 메시지 보내줘"
- "채팅 읽어줘"

```bash
# 예시
"구봉한테 밥 먹었어? 보내줘"
"구봉이랑 대화 내역 보여줘"
```

**요구사항:**
- macOS 전용
- KakaoTalk 앱 실행 중
- Accessibility 권한 필요

---

### 6. session-wrap (세션 래핑)

#### `/wrap` - 세션 종료 워크플로우
2단계 멀티 에이전트 파이프라인:

```
Phase 1: 분석 (병렬)
┌────────────┬────────────┬────────────┬────────────┐
│doc-updater │automation- │learning-   │followup-   │
│            │scout       │extractor   │suggester   │
└────────────┴────────────┴────────────┴────────────┘
                          ↓
Phase 2: 검증
┌─────────────────────────────────────────────────────┐
│                 duplicate-checker                   │
└─────────────────────────────────────────────────────┘
```

#### `/history-insight` - 히스토리 분석
세션 히스토리에서 패턴, 결정, 반복 주제 추출

#### `/session-analyzer` - 세션 검증
SKILL.md 스펙 대비 에이전트/훅/도구 실행 검증

---

## 기타 플러그인

### interactive-review
웹 브라우저에서 Claude의 계획을 시각적으로 리뷰하고 체크박스로 승인/거부

### say-summary (macOS)
Claude 응답을 3-10단어로 요약 후 TTS로 음성 출력
- 한글: Yuna 음성
- 영어: Samantha 음성

### google-calendar
멀티 Google 계정 캘린더 통합
- 병렬 쿼리, 충돌 감지, CRUD 지원

```bash
# 계정별 초기 설정
uv run python scripts/setup_auth.py --account work
uv run python scripts/setup_auth.py --account personal
```

---

## 핵심 포인트

- `/plugin marketplace add` 명령으로 간편 설치
- 멀티 AI 모델 합의를 통한 균형 잡힌 의사결정 지원
- 요구사항 모호함을 사전에 제거하는 clarify 워크플로우
- 개발자 커뮤니티 의견 + AI 분석을 결합한 기술 의사결정
- macOS 특화 기능: TTS, 카카오톡 연동
- 세션 종료 시 문서화/자동화 기회 놓치지 않는 wrap 기능

---

## 참고 사항

- 라이선스: MIT
- 언어 구성: Python 56.6%, JavaScript 34.7%, Shell 8.7%
- macOS 전용 기능 다수 (say-summary, kakaotalk)
- 일부 플러그인은 외부 서비스 연동 필요 (Google Calendar, Gemini CLI 등)
