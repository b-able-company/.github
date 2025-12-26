# AI 기반 개발 플로우

## 5단계 규칙 (TDD 방식)

```
ChatGPT → Claude → Codex → Test Spec → Claude Code
헌법제정   법률작성   계획수립   테스트먼저    구현
```

### 각자 역할

| 도구 | 만드는 것 | 금지 |
|------|----------|------|
| ChatGPT | `AGENTS.md` (기술헌법) | 모호함 |
| Claude | Spec 문서들 (설계서) | AGENTS 위반 |
| Codex | `Plan.md` (구현계획) | Spec 재해석 |
| **Codex** | **Test Code (먼저)** | **테스트 생략** |
| Claude Code | 구현 코드 (테스트 통과용) | 독자판단 |

---

## AGENTS.md 필수 항목

```markdown
1. 기술스택 (버전까지)
2. API 규칙 (URL, 응답포맷)
3. DB 원칙 (PK타입, UTC, soft delete)
4. 인증방식 (JWT/Session)
5. 핵심 비즈니스 룰
```

---

## Spec 문서 5종

1. **ia.md** - 화면/플로우
2. **db-schema.md** - DB 설계
3. **api-spec.md** - API 계약
4. **func-spec.md** - 기능 요구사항
5. **permissions.md** - 권한 규칙

---

## Plan.md 필수 구조

```markdown
1. Spec 충돌 체크
2. 구현 범위 (P0만)
3. **테스트 케이스 작성 (먼저)**
4. 구현 순서 (테스트 통과 기준)
5. 파일별 작업 목록
6. Edge Case 체크리스트
```

**TDD 원칙:** 테스트 먼저 → 구현은 테스트 통과용

---

## 절대 규칙

### 우선순위
```
AGENTS.md > Spec > Plan > Code

충돌 시 → 항상 위쪽이 맞다
```

### 리버트 기준
- ✅ 인증/API포맷/핵심모델이 AGENTS와 다를 때
- ❌ 함수 로직이 복잡할 때 (리팩토링)

### 코드 작성 시
```python
# 파일 상단 필수
# Spec ref: db-schema.md#User
# Plan ref: Plan.md#Phase1
```

---

## 프롬프트 템플릿

### ChatGPT
```
"Django+DRF, JWT, PostgreSQL 기반 [프로젝트명]
AGENTS.md 형식으로 기술헌법 작성"
```

### Claude
```
"AGENTS.md 기반으로 [기능명]의
ia/db/api/func/permissions 명세 작성"
```

### Codex (Plan)
```
"AGENTS + Spec 검토해서 [기능명]
Plan.md 작성 (Spec 충돌체크 포함)"
```

### Codex (Test 먼저)
```
"Plan.md 기반으로 [기능명] 테스트 코드 작성
모든 Edge Case 포함, pytest/jest 사용"
```

### Claude Code (MCP 연결)
```
"테스트 통과하는 [기능명] 구현
Spec 참조 주석 포함
MCP로 DB 스키마 / API 로그 확인하며 작성"
```

---

## 리뷰 체크 (2단계)

### 1단계: AI 자동 리뷰 (먼저)
```
Claude에게 프롬프트:
"이 코드를 AGENTS.md 기준으로 리뷰해줘
- AGENTS 위반?
- Spec과 다름?
- Plan 벗어남?
- 테스트 통과?"
```

### 2단계: 사람 리뷰 (10초)
```
□ AI 리뷰 통과?
□ Spec 주석 있음?
□ 비즈니스 로직 맞음?
```

---

## 긴급상황

**버그:** Spec 위반? → 코드 수정 / Spec 틀림? → 둘 다 수정  
**엉뚱한 구현:** 기초오류? → 리버트 / 디테일? → 수정

---

## 폴더 구조

```
docs/
├── AGENTS.md
├── specs/
│   ├── ia.md
│   ├── db-schema.md
│   ├── api-spec.md
│   ├── func-spec.md
│   └── permissions.md
├── plans/
│   └── sprint-N.md
└── tests/              # 테스트 먼저
    └── test-spec.md

.mcp/                   # MCP 설정
└── config.json
```

---

**핵심:** 
1. 문서가 코드를 이끈다
2. **테스트가 구현을 이끈다 (TDD)**
3. AI는 생각하지 않고 따른다
4. **MCP로 실시간 인프라 연결**

**3가지 비약적 향상 포인트:**
- 테스트 먼저 짜면 → 정확도 ↑↑
- AI 자동 리뷰 먼저 하면 → 속도 ↑↑  
- MCP로 DB/로그 연결하면 → 긴급대응 ↑↑
