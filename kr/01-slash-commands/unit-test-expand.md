<!-- i18n-source: 01-slash-commands/unit-test-expand.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

---
name: Expand Unit Tests
description: 테스트되지 않은 브랜치와 엣지 케이스를 대상으로 테스트 커버리지 증가
tags: testing, coverage, unit-tests
---

# 단위 테스트 확장

프로젝트의 테스트 프레임워크에 맞게 기존 단위 테스트를 확장합니다:

1. **커버리지 분석**: 커버리지 리포트를 실행하여 테스트되지 않은 브랜치, 엣지 케이스, 낮은 커버리지 영역 식별
2. **갭 식별**: 논리적 브랜치, 오류 경로, 경계 조건, null/빈 입력을 위해 코드 검토
3. **테스트 작성** using 프로젝트의 프레임워크:
   - Jest/Vitest/Mocha (JavaScript/TypeScript)
   - pytest/unittest (Python)
   - Go testing/testify (Go)
   - Rust test framework (Rust)
4. **특정 시나리오 대상**:
   - 오류 처리 및 예외
   - 경계 값 (최소/최대, 빈 값, null)
   - 엣지 케이스 및 코너 케이스
   - 상태 전환 및 부작용
5. **개선 확인**: 커버리지를 다시 실행, 측정 가능한 증가 확인

새 테스트 코드 블록만 제시합니다. 기존 테스트 패턴 및 명명 규칙을 따릅니다.

---
**최종 업데이트**: 2026년 4월 9일
