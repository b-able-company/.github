# AI 개발 플로우

## 4단계 규칙

```
ChatGPT → Claude → Codex → Claude Code
헌법제정   법률작성   계획수립   실행
```

### 각자 역할

| 도구 | 만드는 것 | 금지 |
|------|----------|------|
| ChatGPT | `AGENTS.md` (기술헌법) | 모호함 |
| Claude | Spec 문서들 (설계서) | AGENTS 위반 |
| Codex | `Plan.md` (구현계획) | Spec 재해석 |
| Claude Code | 코드 | 독자판단 |

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
3. 구현 순서
4. 파일별 작업 목록 (구체적으로)
5. Edge Case 체크리스트
```

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

### Codex
```
"AGENTS + Spec 검토해서 [기능명]
Plan.md 작성 (Spec 충돌체크 포함)"
```

### Claude Code
```
"Plan.md Phase [N] 구현
Spec 참조 주석 포함"
```

---

## 리뷰 체크 (10초)

```
□ AGENTS 위반?
□ Spec과 다름?
□ Plan 벗어남?
□ Spec 주석 있음?
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
└── plans/
    └── sprint-N.md
```

---

**핵심:** 문서가 코드를 이끈다. AI는 생각하지 않고 따른다.
