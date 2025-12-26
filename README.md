# AI 기반 개발 플로우

## 핵심 원칙

```
1. 문서가 코드를 이끈다 (Spec → Code)
2. 테스트가 구현을 이끈다 (Test → Implementation)
3. AI는 제안, Engineer는 결정 (Delegate-Review-Own)
```

---

## 6단계 워크플로우

```
ChatGPT → Claude → Claude → Codex → Codex → Claude Code
프로젝트규약  설계문서   UI프로토타입  구현계획   테스트작성   코드구현
```

---

## 역할과 책임 (Delegate-Review-Own)

| 단계 | AI가 하는 일 (Delegate) | Engineer 검증 (Review) | Engineer 책임 (Own) |
|------|------------------------|----------------------|-------------------|
| **1. 프로젝트 규약** | AGENTS.md 초안 생성 | 문법/일관성 검증 | 기술스택 최종 결정 |
| **2. 설계** | Spec 5종 작성 | 논리 정합성 확인 | 비즈니스 룰 승인 |
| **3. UI 디자인** | 디자인→코드 변환 | 접근성/UX 검증 | 사용자 경험 결정 |
| **4. 계획** | Plan.md + 충돌 체크 | Edge Case 보강 | 구현 범위 결정 |
| **5. 테스트** | 테스트 코드 생성 | 커버리지 확인 | 테스트 전략 수립 |
| **6. 구현** | 기능 코드 작성 | 아키텍처 검증 | 프로덕션 배포 승인 |

---

## AGENTS.md 체크리스트

```markdown
# AGENTS.md (프로젝트 규약)

## 필수 섹션
□ 기술스택 (언어, 프레임워크, DB, 버전)
□ 인증/보안 (JWT 만료, CORS, Rate Limit)
□ API 규칙 (URL 패턴, 에러 형식, 페이지네이션)
□ DB 원칙 (PK 타입, 타임스탬프, Soft Delete)
□ 테스트 기준 (커버리지 목표, 테스트 피라미드)
□ 코드 품질 (린터, 복잡도 제한, 타입 체크)
□ 핵심 비즈니스 룰 (도메인별 정책)
□ 금지 패턴 (안티패턴 목록)
```

---

## Spec 문서 5종

| 문서 | 내용 | 핵심 질문 |
|------|------|---------|
| **ia.md** | 화면 구조, 사용자 플로우 | "사용자가 어떻게 이동하나?" |
| **db-schema.md** | 테이블, 관계, 인덱스 | "데이터를 어떻게 저장하나?" |
| **api-spec.md** | 엔드포인트, 요청/응답 | "외부와 어떻게 통신하나?" |
| **func-spec.md** | 기능 요구사항, 엣지 케이스 | "무엇을 구현하나?" |
| **permissions.md** | 역할, 권한 매트릭스 | "누가 무엇을 할 수 있나?" |

**템플릿:** 각 Spec 파일 헤더에 "Spec ref: AGENTS.md#[section]" 필수

---

## Plan.md 구조

```markdown
# Implementation Plan: [Feature Name]

## 1. Spec 검증
- [ ] AGENTS.md 충돌?
- [ ] Spec 5종 일치?
- [ ] DB 마이그레이션 필요?

## 2. 테스트 케이스 (먼저 작성)
- Unit: [목록]
- Integration: [목록]
- E2E: [목록]

## 3. 구현 순서
Phase 1: DB → Phase 2: API → Phase 3: UI

## 4. Edge Cases
[Case] | [Handling] | [Test]

## 5. 파일 체크리스트
- [ ] /src/models/[name].py (Spec ref: db-schema.md#[table])
- [ ] /tests/test_[name].py
```

---

## 프롬프트 템플릿

> **[ ]는 당신의 정보로 교체** | 예: [Django] → "Next.js 14"

### 1. ChatGPT: AGENTS.md 생성
```
"[Django+PostgreSQL] 기반 [B2B SaaS] 프로젝트의 AGENTS.md 작성.

필수 포함:
- JWT 인증 규칙
- REST API 표준
- DB PK는 UUID
- 테스트 커버리지 80%
- 금지: n+1 쿼리, 하드코딩

출력: 위 체크리스트 형식"
```
**예:** "FastAPI+MongoDB 기반 E-commerce 프로젝트의 AGENTS.md 작성..."

### 2. Claude: Spec 작성
```
"AGENTS.md 참고해 [사용자 인증] 기능의 Spec 5종 작성.
- ia.md: 로그인 플로우
- db-schema.md: users 테이블
- api-spec.md: POST /auth/login
- func-spec.md: 비밀번호 규칙, 재시도 제한
- permissions.md: 인증 전후 권한

AGENTS.md 위반 시 경고"
```
**예:** "AGENTS.md 참고해 상품 검색 기능의 Spec 5종 작성..."

### 3. Claude: 디자인→코드
```
"[첨부: login.png]를 React 컴포넌트로 변환.
- Tailwind CSS 사용
- 접근성: ARIA labels
- 반응형: mobile-first
- TypeScript props 정의"
```
**예:** "[첨부: dashboard.fig]를 Vue 컴포넌트로 변환..."

### 4. Codex: Plan 작성
```
"AGENTS.md + Spec 검토 후 [사용자 인증] Plan.md 작성.
- Spec 충돌 체크
- Edge Cases: 중복 이메일, 만료 토큰, Rate limit
- 테스트 우선 순서
- 파일 목록 (Spec ref 포함)"
```
**예:** "AGENTS.md + Spec 검토 후 결제 시스템 Plan.md 작성..."

### 5. Codex: 테스트 작성
```
"Plan.md 기반 테스트 코드 작성 (pytest).
- 모든 Edge Case 커버
- Arrange-Act-Assert 패턴
- 테스트 실패 먼저 확인 (Red)
- 커버리지: 함수 90%, 브랜치 80%"
```
**예:** "Plan.md 기반 테스트 코드 작성 (jest)..."

### 6. Claude Code: 구현
```
"테스트 통과하는 [사용자 인증] 구현.
- Spec ref 주석 필수
- AGENTS.md 규칙 준수
- 린터 + 테스트 통과 후 완료"
```
**예:** "테스트 통과하는 장바구니 기능 구현..."

---

## 즉시 시작 (Day 1)

1. ChatGPT로 AGENTS.md 작성 (1시간)
2. 첫 기능 선택 (간단한 것)
3. 프롬프트 템플릿으로 구현 시작

**끝. 나머지는 필요할 때 추가.**

---

## 리뷰 프로세스 (2단계)

### 1단계: AI 자동 리뷰
```bash
$ codex review --against AGENTS.md --check-specs

출력 예시:
⚠️ AGENTS.md 위반: JWT 만료 규칙
✅ Spec 참조 유효
⚠️ 복잡도 12 (제한: 10)
```

### 2단계: Engineer 10초 체크
```
□ AI 리뷰 통과?
□ 비즈니스 로직 정확?
□ 아키텍처 일관성?
```

---

## 긴급상황 대응

| 상황 | 진단 | 해결 |
|------|------|------|
| **Spec 위반** | Spec vs Code 대조 | Spec 맞으면 Code 수정, Spec 틀리면 둘 다 수정 |
| **테스트 실패** | Red-Green 확인 | AI에게 수정 요청 → Engineer 검증 |
| **AGENTS 충돌** | 비즈니스 요구사항 확인 | AGENTS 수정 → Spec 업데이트 → Code 재생성 |

---

## 프로젝트 구조

```
project-root/
├── docs/
│   ├── AGENTS.md                 # 프로젝트 규약
│   └── specs/
│       ├── ia.md
│       ├── db-schema.md
│       ├── api-spec.md
│       ├── func-spec.md
│       └── permissions.md
├── plans/
│   └── [feature]-plan.md
├── .mcp/
│   └── config.json              # MCP 서버 설정
└── tests/
    └── ...
```

---

## Engineer의 새로운 역할

### ❌ 이제 하지 않는 일
- 라인바이라인 코딩
- 보일러플레이트 작성
- 디자인→HTML 수작업 변환

### ✅ 집중할 일
- 시스템 아키텍처 설계
- 비즈니스 로직 엣지 케이스 발굴
- AI 출력물 품질 검증
- AGENTS.md/Spec 유지보수

---

## 다음 단계: 운영 자동화

개발 플로우 적용 1주 후:
- AI 코드 리뷰 자동화
- 문서 자동 생성
- 장애 대응 AI 지원

→ **AI 기반 운영 스킬** 참조

---

## 측정 지표 (선택 참고)

> 팀 리드/관리자용. 신입 개발자는 건너뛰어도 됩니다.

| 지표 | 목표 | 도구 |
|------|------|------|
| Spec-Code 일치율 | 95% | AI 검증 스크립트 |
| 테스트 커버리지 | 80% | pytest-cov, jest --coverage |
| AI 생성 코드 수정률 | 20% 이하 | Git diff 분석 |
| 개발 속도 | 2배 | 기능당 소요시간 측정 |
```