# SKILL.md: AI 기반 운영

## Overview
개발 플로우(AGENTS.md, Spec)를 기반으로 코드 리뷰, 문서화, 장애 대응을 자동화. Delegate-Review-Own 패턴 적용.

## When to use
- PR 생성 시 AI 자동 리뷰 필요
- 릴리스 시 문서 자동 생성 필요
- 장애 발생 시 로그 분석 및 원인 추적
- 주간 시스템 헬스체크

## Prerequisites
```
✅ AGENTS.md 존재
✅ Spec 문서 5종 작성됨
✅ 코드에 "Spec ref: ..." 주석 포함
```

**중요:** 개발 플로우 없이 단독 사용 시 효과 50% ↓

---

## How

### 1. AI 코드 리뷰
```yaml
# .github/workflows/ai-review.yml
on: [pull_request]
jobs:
  review:
    steps:
      - run: codex review --against docs/AGENTS.md --check-specs
```

**Engineer 10초 체크:**
```
□ AI 지적 P0 이슈 해결?
□ 비즈니스 로직 정확?
□ 아키텍처 일관성?
```

---

### 2. 자동 문서화
```bash
# 릴리스 노트 생성
$ codex generate-release-notes --from v1.2.0 --to v1.3.0

# AGENTS.md에 규칙 추가
## Documentation Standards
- 릴리스 태그: CHANGELOG.md 자동 생성
- API 변경: api-spec.md 자동 갱신
- Mermaid 다이어그램 자동 생성
```

**Delegate-Review-Own:**
- 함수 docstring → AI 100%, 검토 불필요
- API 문서 → AI 초안, Engineer 검증
- 법적 문서 → AI 사용 금지

---

### 3. 장애 대응
```markdown
Claude에게:
"엔드포인트 /api/payments 에러율 급증 (최근 2시간)
1. 에러 로그 분석 (MCP: logs)
2. 최근 배포 확인 (MCP: git)
3. 의심 커밋 추출
4. Spec 참조해 원인 파악
5. 핫픽스 제안"
```

**MCP 설정:**
```json
// .mcp/config.json
{
  "servers": [
    {"name": "logs", "type": "datadog"},
    {"name": "git", "type": "github"}
  ]
}
```

---

## Examples

**Example 1: AI 리뷰 결과**
```
✅ AGENTS.md 규칙 준수
✅ Spec 참조: func-spec.md#auth
⚠️ 복잡도 12 (목표: 10)

→ Engineer: 복잡도 개선 요청
```

**Example 2: 장애 분석**
```
Alert: /api/payments 에러율 18%

AI 분석:
- 에러: Connection timeout
- 의심 커밋: "timeout 30s → 5s"
- 원인: payments 테이블 인덱스 없음

해결: timeout 롤백 (즉시)
```

---

## Common Pitfalls
```
❌ AI 리뷰를 무조건 신뢰
   → P0는 Engineer 재확인 필수

❌ 문서 자동화를 모든 파일에 적용
   → 법적/보안 문서는 수동 작성

❌ AI 장애 분석을 100% 신뢰
   → 힌트로만 사용, Engineer 판단 우선

❌ False Positive 방치
   → "Acknowledged" 코멘트로 기록
```

---

## Measuring Success
| 지표 | 목표 | 도구 |
|------|------|------|
| MTTR | 30분 → 10분 | 장애 티켓 분석 |
| AI 리뷰 정확도 | 70% 유용 | PR 👍 비율 |
| 문서 최신성 | 깨진 링크 0 | 주간 검증 |

---

## Quick Start
```
Day 1: AI 리뷰 설정 (1시간)
Day 2: 릴리스 노트 자동화 (30분)
Day 3: MCP 연결 + 장애 대응 테스트 (1시간)
```