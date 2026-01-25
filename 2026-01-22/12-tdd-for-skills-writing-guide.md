# TDD로 Skill 작성하기 - 문서도 테스트하는 시대

> **URL**: https://github.com/obra/superpowers/blob/main/skills/writing-skills/SKILL.md  
> **유형**: 스킬 작성 가이드  
> **출처**: obra/superpowers

---

## 요약

**Skill 작성 = TDD(Test-Driven Development)를 문서에 적용하는 것.**

테스트 케이스(압박 시나리오)를 만들고, 실패를 확인하고(baseline), 스킬 문서를 작성하고, 테스트 통과를 확인하고, 리팩토링하는 과정을 거칩니다.

**핵심 원칙**: Skill 없이 에이전트가 실패하는 것을 먼저 보지 않으면, 그 Skill이 올바른 것을 가르치는지 알 수 없다.

---

## 왜 Skill을 테스트해야 하나?

```
"방법을 알려줬으니 당연히 더 잘하겠지" → ❌ 착각

실제로 테스트해보면:
- 스킬의 절반은 없어도 되는 내용
- 누락된 설명이 여러 개 발견됨
```

---

## TDD ↔ Skill 작성 매핑

| TDD 개념 | Skill 작성 |
|----------|-----------|
| **테스트 케이스** | 압박 시나리오 + subagent |
| **프로덕션 코드** | Skill 문서 (SKILL.md) |
| **테스트 실패 (RED)** | Skill 없이 에이전트가 규칙 위반 (baseline) |
| **테스트 통과 (GREEN)** | Skill 있으면 에이전트가 규칙 준수 |
| **리팩토링** | 허점 발견 → 수정 → 재검증 |
| **테스트 먼저 작성** | Skill 작성 전 baseline 시나리오 실행 |
| **실패 관찰** | 에이전트가 사용하는 정확한 변명 기록 |
| **최소한의 코드** | 그 특정 위반만 해결하는 Skill 작성 |
| **통과 관찰** | 에이전트가 이제 준수하는지 확인 |
| **리팩토링 사이클** | 새 변명 발견 → 막기 → 재검증 |

---

## RED-GREEN-REFACTOR 사이클

### 🔴 RED: 실패하는 테스트 작성 (Baseline)

**Skill 없이** subagent에게 압박 시나리오 실행. 정확한 행동 기록:

- 어떤 선택을 했는가?
- 어떤 변명(rationalization)을 사용했는가? (원문 그대로)
- 어떤 압박이 위반을 유발했는가?

```
이것이 "테스트 실패 관찰" 단계
→ Skill 작성 전에 에이전트가 자연스럽게 무엇을 하는지 봐야 함
```

### 🟢 GREEN: 최소한의 Skill 작성

**그 특정 변명들만** 해결하는 Skill 작성. 가상의 케이스를 위한 추가 내용 X.

같은 시나리오를 **Skill과 함께** 실행. 에이전트가 이제 준수해야 함.

### 🔵 REFACTOR: 허점 막기

에이전트가 새 변명을 찾았다면? 명시적 대응 추가. 완벽해질 때까지 재테스트.

---

## Skill이란?

**Skill = 검증된 기법, 패턴, 도구에 대한 참조 가이드**

| Skill인 것 | Skill 아닌 것 |
|------------|--------------|
| 재사용 가능한 기법 | 한 번 문제 해결한 이야기 |
| 패턴 | 프로젝트별 규칙 (→ CLAUDE.md에) |
| 도구 참조 | 다른 곳에 잘 문서화된 표준 관행 |
| 참조 가이드 | 자동화 가능한 기계적 제약 |

### 언제 Skill을 만들어야 하나?

**만들어야 할 때:**
- 기법이 직관적이지 않았을 때
- 프로젝트에 걸쳐 다시 참조할 것 같을 때
- 패턴이 광범위하게 적용될 때
- 다른 사람도 도움 받을 때

**만들지 말아야 할 때:**
- 일회성 솔루션
- regex/validation으로 강제 가능한 것 (자동화해라)

---

## Skill 유형별 테스트 방법

### 1. 규율 Skill (discipline)

**예시**: TDD, verification-before-completion

**테스트 방법**:
- 학문적 질문: 규칙을 이해하는가?
- 압박 시나리오: 스트레스 하에서도 준수하는가?
- **복합 압박**: 시간 + 매몰비용 + 피로
- 변명 식별 → 명시적 대응 추가

### 2. 기법 Skill (technique)

**예시**: condition-based-waiting, root-cause-tracing

**테스트 방법**:
- 적용 시나리오: 기법을 올바르게 적용할 수 있는가?
- 변형 시나리오: 엣지 케이스를 처리하는가?
- 누락 정보 테스트: 지침에 빈틈이 있는가?

### 3. 패턴 Skill (pattern)

**예시**: reducing-complexity, information-hiding

**테스트 방법**:
- 인식 시나리오: 패턴이 적용되어야 할 때를 인식하는가?
- 적용 시나리오: 멘탈 모델을 사용할 수 있는가?
- 반례: 적용하지 말아야 할 때를 아는가?

### 4. 참조 Skill (reference)

**예시**: API 문서, 명령어 참조

**테스트 방법**:
- 검색 시나리오: 올바른 정보를 찾을 수 있는가?
- 적용 시나리오: 찾은 것을 올바르게 사용하는가?
- 빈틈 테스트: 일반적인 사용 사례가 다 커버되는가?

---

## 변명(Rationalization) 방어하기

### 모든 허점을 명시적으로 막기

```markdown
# ❌ 나쁜 예
테스트 전에 코드를 작성했다면? 삭제해라.

# ✅ 좋은 예
테스트 전에 코드를 작성했다면? 삭제해라. 처음부터 다시 시작.

**예외 없음:**
- "참조용"으로 보관하지 마라
- 테스트 작성하면서 "적응"시키지 마라
- 보지도 마라
- 삭제는 삭제다
```

### 변명 테이블 구축

baseline 테스트에서 나온 변명들을 모두 기록:

| 변명 | 현실 |
|------|------|
| "너무 단순해서 테스트 불필요" | 단순한 코드도 깨진다. 테스트는 30초. |
| "나중에 테스트할게" | 즉시 통과하는 테스트는 아무것도 증명 안 함. |
| "테스트-후도 같은 목적" | 테스트-후 = "이게 뭐하는 거지?" / 테스트-먼저 = "이게 뭘 해야 하지?" |

### Red Flags 목록 만들기

```markdown
## Red Flags - 멈추고 처음부터 다시

- 테스트 전에 코드 작성
- "이미 수동으로 테스트했어"
- "테스트-후도 같은 목적 달성해"
- "정신을 따르는 거지 형식이 아니야"
- "이건 다르니까..."

**이 모든 것 = 코드 삭제. TDD로 다시 시작.**
```

### "정신 vs 형식" 논쟁 차단

초반에 기본 원칙 추가:

```markdown
**규칙의 형식을 위반하는 것은 규칙의 정신을 위반하는 것이다.**
```

---

## SKILL.md 구조

### YAML Frontmatter

```yaml
---
name: Skill-Name-With-Hyphens
description: Use when [구체적인 트리거 조건과 증상]
---
```

**규칙**:
- `name`: 문자, 숫자, 하이픈만 (괄호, 특수문자 X)
- `description`: 3인칭, **언제 사용하는지만** (무엇을 하는지 X)
- 최대 1024자

### Description 작성의 함정

**치명적 실수**: description에 워크플로우를 요약하면 Claude가 전체 Skill을 읽지 않고 description만 따름

```yaml
# ❌ 나쁜 예: 워크플로우 요약 - Claude가 이것만 따름
description: Use when executing plans - dispatches subagent per task with code review between tasks

# ✅ 좋은 예: 트리거 조건만
description: Use when executing implementation plans with independent tasks in the current session
```

### 본문 구조

```markdown
# Skill Name

## Overview
이게 뭔가? 핵심 원칙 1-2문장.

## When to Use
- 증상과 사용 사례 (불릿)
- 사용하지 말아야 할 때

## Core Pattern
Before/after 코드 비교

## Quick Reference
스캔용 테이블 또는 불릿

## Implementation
간단한 패턴은 인라인 코드
무거운 참조는 파일 링크

## Common Mistakes
잘못되는 것 + 수정 방법
```

---

## Claude Search Optimization (CSO)

### 검색 최적화 체크리스트

1. **풍부한 Description 필드**
   - "Use when..."으로 시작
   - 구체적 트리거, 증상, 상황 포함
   - **워크플로우 요약 절대 금지**

2. **키워드 커버리지**
   - 에러 메시지: "Hook timed out", "race condition"
   - 증상: "flaky", "hanging", "zombie"
   - 동의어: "timeout/hang/freeze"
   - 도구명: 실제 명령어, 라이브러리명

3. **설명적 이름**
   - ✅ `creating-skills` (동사-명사)
   - ❌ `skill-creation` (명사-명사)

4. **토큰 효율성**
   - getting-started: <150 단어
   - 자주 로드되는 Skill: <200 단어
   - 기타 Skill: <500 단어

---

## 테스트 스킵 변명들

| 변명 | 현실 |
|------|------|
| "Skill이 명확해" | 너한테 명확 ≠ 다른 에이전트한테 명확. 테스트해라. |
| "그냥 참조 문서야" | 참조도 빈틈이 있다. 검색 테스트해라. |
| "테스트는 과해" | 테스트 안 한 Skill은 항상 문제 있다. 15분 테스트가 몇 시간 절약. |
| "문제 생기면 테스트할게" | 문제 = 에이전트가 Skill 못 쓴다. 배포 전 테스트해라. |
| "자신 있어" | 과신 = 문제 보장. 어쨌든 테스트해라. |
| "학문적 검토면 충분해" | 읽기 ≠ 사용하기. 적용 시나리오 테스트해라. |
| "시간 없어" | 테스트 안 한 Skill 배포하면 나중에 더 많은 시간 낭비. |

**모든 변명의 결론: 배포 전 테스트. 예외 없음.**

---

## Skill 작성 체크리스트

### 🔴 RED Phase - 실패하는 테스트 작성

- [ ] 압박 시나리오 생성 (규율 Skill은 3개 이상 복합 압박)
- [ ] Skill **없이** 시나리오 실행 - baseline 행동 원문 기록
- [ ] 변명/실패 패턴 식별

### 🟢 GREEN Phase - 최소한의 Skill 작성

- [ ] 이름: 문자, 숫자, 하이픈만
- [ ] YAML frontmatter: name, description만 (최대 1024자)
- [ ] Description: "Use when..."으로 시작, 트리거/증상 포함
- [ ] Description: 3인칭, **워크플로우 요약 금지**
- [ ] 검색 키워드 전체에 배치
- [ ] 핵심 원칙과 함께 명확한 Overview
- [ ] RED에서 식별한 구체적 baseline 실패 해결
- [ ] Skill **과 함께** 시나리오 실행 - 에이전트가 이제 준수 확인

### 🔵 REFACTOR Phase - 허점 막기

- [ ] 테스트에서 새 변명 식별
- [ ] 명시적 대응 추가 (규율 Skill인 경우)
- [ ] 모든 테스트 반복에서 변명 테이블 구축
- [ ] Red flags 목록 생성
- [ ] 완벽해질 때까지 재테스트

### ✅ Quality Checks

- [ ] 결정이 자명하지 않을 때만 작은 플로우차트
- [ ] Quick reference 테이블
- [ ] Common mistakes 섹션
- [ ] 서사적 스토리텔링 없음
- [ ] 지원 파일은 도구나 무거운 참조만

### 🚀 Deployment

- [ ] Skill을 git에 커밋하고 fork에 push
- [ ] 광범위하게 유용하면 PR 기여 고려

---

## Anti-Patterns

| ❌ 나쁜 예 | 이유 |
|----------|------|
| "2025-10-03 세션에서 빈 projectDir가..." | 너무 구체적, 재사용 불가 |
| example-js.js, example-py.py, example-go.go | 품질 저하, 유지보수 부담 |
| 플로우차트에 코드 | 복사-붙여넣기 불가, 읽기 어려움 |
| helper1, helper2, step3 | 의미 없는 레이블 |

---

## 핵심 포인트

1. **Skill 작성 = 문서에 TDD 적용**: 같은 Iron Law, 같은 사이클, 같은 이점
2. **실패 먼저 관찰**: Skill 없이 에이전트가 실패하는 것을 먼저 봐야 함
3. **변명 방어**: 에이전트는 똑똑해서 압박 받으면 허점을 찾음 → 명시적으로 막기
4. **Description ≠ 워크플로우**: 트리거 조건만, 프로세스 요약 금지
5. **테스트 스킵 금지**: 모든 변명은 "어쨌든 테스트해라"로 귀결

---

## 참고 사항

- **원문**: [obra/superpowers - writing-skills](https://github.com/obra/superpowers/blob/main/skills/writing-skills/SKILL.md)
- **관련 Skill**: superpowers:test-driven-development, testing-skills-with-subagents.md
- **참고**: persuasion-principles.md (설득 기법 연구)

---

> **"코드에 TDD를 따른다면, Skill에도 TDD를 따르라. 같은 규율을 문서에 적용하는 것이다."**
