# Hugging Face Skills - AI/ML 태스크를 위한 에이전트 스킬

> **URL**: https://github.com/huggingface/skills  
> **유형**: GitHub 레포지토리 (AI Agent Skills)  
> **작성자**: Hugging Face  
> **라이선스**: Apache-2.0  
> **언어**: Python 91.6%, Shell 5.8%, HTML 2.2%  
> **Stars**: 1k ⭐ | **Forks**: 97

---

## 요약

Hugging Face에서 제공하는 **AI/ML 태스크를 위한 에이전트 스킬 정의**. 데이터셋 생성, 모델 훈련, 평가 등의 작업을 위한 스킬을 제공하며, **모든 주요 코딩 에이전트 도구와 호환**된다 (OpenAI Codex, Claude Code, Gemini CLI, Cursor).

> "Skills are definitions for AI/ML tasks like dataset creation, model training, and evaluation."

---

## 핵심 특징

### 🔄 크로스 플랫폼 호환성

| 플랫폼 | 지원 방식 |
|--------|-----------|
| **Claude Code** | `/plugin` 명령으로 설치 |
| **OpenAI Codex** | `AGENTS.md` 파일 인식 |
| **Gemini CLI** | `gemini-extension.json` |
| **Cursor** | 통합 예정 |
| **Windsurf** | 통합 예정 |
| **Continue** | 통합 예정 |

> **참고**: 'Skills'는 Anthropic 용어지만, 이 레포는 모든 에이전트 도구와 호환됨

---

## 스킬 구조

```
skills/
├── my-skill/
│   ├── SKILL.md          ← YAML frontmatter + 지침
│   ├── scripts/          ← 헬퍼 스크립트
│   └── templates/        ← 템플릿 파일
```

### SKILL.md 구조

```yaml
---
name: my-skill-name
description: 스킬 설명 및 사용 시점
---

# Skill Title

Guidance + examples + guardrails
```

---

## 설치 방법

### Claude Code

```bash
# 1. 마켓플레이스 등록
/plugin marketplace add huggingface/skills

# 2. 스킬 설치
/plugin install <skill-name>@huggingface/skills

# 예시
/plugin install hugging-face-cli@huggingface/skills
```

### OpenAI Codex

```bash
# AGENTS.md 파일 자동 인식
codex --ask-for-approval never "Summarize the current instructions."
```

### Gemini CLI

```bash
# 로컬 설치
gemini extensions install . --consent

# GitHub URL로 설치
gemini extensions install https://github.com/huggingface/skills.git --consent
```

---

## 제공되는 스킬 (8개)

| 스킬 | 설명 |
|------|------|
| **hugging-face-cli** | HF Hub 작업 (모델/데이터셋 다운로드, 파일 업로드, 레포 관리, 클라우드 컴퓨팅 작업) |
| **hugging-face-datasets** | 데이터셋 생성/관리 (레포 초기화, 설정/시스템 프롬프트, 스트리밍 업데이트, SQL 쿼리/변환) |
| **hugging-face-evaluation** | 평가 결과 관리 (README에서 eval 테이블 추출, Artificial Analysis API 점수 가져오기, vLLM/lighteval 커스텀 평가) |
| **hugging-face-jobs** | HF 인프라에서 컴퓨팅 작업 실행 (Python 스크립트 실행, 스케줄링, 상태 모니터링) |
| **hugging-face-model-trainer** | TRL로 모델 훈련/파인튜닝 (SFT, DPO, GRPO, Reward Modeling, GGUF 변환, 비용 추정) |
| **hugging-face-paper-publisher** | 연구 논문 게시/관리 (논문 페이지 생성, 모델/데이터셋 연결, 저자 인증) |
| **hugging-face-tool-builder** | HF API 작업용 재사용 스크립트 빌드 (API 호출 체이닝, 반복 작업 자동화) |
| **hugging-face-trackio** | ML 훈련 실험 추적/시각화 (Python API 메트릭 로깅, CLI 조회, HF Spaces 대시보드) |

---

## 스킬 상세

### 1. hugging-face-cli

```
HF Hub 작업 CLI
├── 모델/데이터셋 다운로드
├── 파일 업로드
├── 레포 관리
└── 클라우드 컴퓨팅 작업 실행
```

### 2. hugging-face-datasets

```
데이터셋 생성/관리
├── 레포 초기화
├── 설정/시스템 프롬프트 정의
├── 스트리밍 row 업데이트
└── SQL 기반 쿼리/변환
```

### 3. hugging-face-model-trainer 🔥

```
모델 훈련/파인튜닝 (TRL 기반)
├── 지원 훈련 방법
│   ├── SFT (Supervised Fine-Tuning)
│   ├── DPO (Direct Preference Optimization)
│   ├── GRPO (Group Relative Policy Optimization)
│   └── Reward Modeling
├── GGUF 변환 (로컬 배포용)
├── 하드웨어 선택
├── 비용 추정
├── Trackio 모니터링
└── Hub 저장
```

### 4. hugging-face-evaluation

```
평가 결과 관리
├── README에서 eval 테이블 추출
├── Artificial Analysis API 점수 가져오기
└── vLLM/lighteval 커스텀 평가
```

### 5. hugging-face-trackio

```
ML 실험 추적/시각화
├── Python API 메트릭 로깅
├── CLI 조회
└── HF Spaces 실시간 대시보드 동기화
```

---

## 사용 예시

코딩 에이전트에 스킬 설치 후, 직접 언급하여 사용:

```
"Use the HF LLM trainer skill to estimate the GPU memory needed for a 70B model run."
```

```
"Use the HF model evaluation skill to launch `run_eval_job.py` on the latest checkpoint."
```

```
"Use the HF dataset creator skill to draft new few-shot classification templates."
```

```
"Use the HF paper publisher skill to index my arXiv paper and link it to my model."
```

→ 에이전트가 해당 `SKILL.md` 지침과 헬퍼 스크립트를 자동 로드하여 태스크 완료

---

## 커스텀 스킬 기여 방법

### 1. 폴더 생성

```bash
cp -r skills/hf-datasets skills/my-skill
```

### 2. SKILL.md 수정

```yaml
---
name: my-skill-name
description: 스킬 설명 및 사용 시점
---

# Skill Title

Guidance + examples + guardrails
```

### 3. 지원 파일 추가

스크립트, 템플릿, 문서 등

### 4. marketplace.json 등록

`.claude-plugin/marketplace.json`에 엔트리 추가

### 5. 검증

```bash
python scripts/generate_agents.py
```

### 6. 리로드

코딩 에이전트에서 스킬 번들 재설치/리로드

---

## 마켓플레이스

`.claude-plugin/marketplace.json` 파일이 스킬 목록 관리:

| 용도 | 설명 |
|------|------|
| **SKILL.md description** | Claude가 스킬 활성화 시점 판단 |
| **marketplace description** | 사람이 스킬 브라우징용 |

CI가 `SKILL.md`와 `marketplace.json` 간 스킬 이름/경로 일치 검증

---

## 레포지토리 구조

```
huggingface/skills/
├── .claude-plugin/        ← Claude Code 플러그인 설정
├── .github/workflows/     ← CI/CD
├── agents/                ← AGENTS.md (Codex용)
├── apps/                  ← 앱
├── assets/                ← 에셋
├── scripts/               ← 유틸리티 스크립트
├── skills/                ← 스킬 폴더들
├── gemini-extension.json  ← Gemini CLI 확장
└── README.md
```

---

## 다른 스킬 컬렉션과의 비교

| 레포지토리 | 초점 | Stars |
|------------|------|-------|
| **huggingface/skills** | **AI/ML 태스크 특화** (HF 생태계) | 1k |
| ComposioHQ/awesome-claude-skills | 범용 Claude 스킬 큐레이션 | 24.4k |
| travisvn/awesome-claude-skills | Claude Code 특화 가이드 | 5.7k |
| muratcankoylan/Agent-Skills-for-Context-Engineering | 컨텍스트 엔지니어링 | 7.7k |

---

## 핵심 포인트

1. **공식 HF 스킬**: Hugging Face 공식 AI/ML 태스크 스킬 컬렉션
2. **크로스 플랫폼**: Claude Code, Codex, Gemini CLI 모두 호환
3. **실용적 스킬 8개**: 데이터셋, 모델 훈련, 평가, 논문 게시 등
4. **표준 포맷**: `SKILL.md` (YAML frontmatter + 마크다운 지침)
5. **기여 가능**: 폴더 복사 → 수정 → marketplace.json 등록
6. **Fallback**: 에이전트가 스킬 미지원 시 `agents/AGENTS.md` 직접 사용

---

## 참고 자료

- [GitHub 레포지토리](https://github.com/huggingface/skills)
- [Hugging Face 문서](https://huggingface.co/docs)
- [Codex AGENTS 가이드](https://github.com/openai/codex/blob/main/docs/AGENTS.md)
- [Gemini CLI Extensions 문서](https://ai.google.dev/gemini-api/docs/cli/extensions)
