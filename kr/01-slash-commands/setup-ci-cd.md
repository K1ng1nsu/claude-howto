<!-- i18n-source: 01-slash-commands/setup-ci-cd.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

---
name: Setup CI/CD Pipeline
description: 품질 보증을 위한 pre-commit 훅 및 GitHub Actions 구현
tags: ci-cd, devops, automation
---

# CI/CD 파이프라인 설정

프로젝트 유형에 맞게 종합적인 DevOps 품질 게이트를 구현합니다:

1. **프로젝트 분석**: 언어, 프레임워크, 빌드 시스템, 기존 도구 감지
2. **pre-commit 훅 설정** with 언어별 도구:
   - 포맷팅: Prettier/Black/gofmt/rustfmt 등
   - 린팅: ESLint/Ruff/golangci-lint/Clippy 등
   - 보안: Bandit/gosec/cargo-audit/npm audit 등
   - 타입 검사: TypeScript/mypy/flow (해당되는 경우)
   - 테스트: 관련 테스트 스위트 실행
3. **GitHub Actions 워크플로우 생성** (.github/workflows/):
   - 푸시/PR 시 pre-commit 검사 미러링
   - 멀티 버전/플랫폼 매트릭스 (해당되는 경우)
   - 빌드 및 테스트 검증
   - 배포 단계 (필요한 경우)
4. **파이프라인 검증**: 로컬에서 테스트, 테스트 PR 생성, 모든 검사 통과 확인

무료/오픈 소스 도구를 사용합니다. 기존 설정을 존중합니다. 실행 속도를 빠르게 유지합니다.

---
**최종 업데이트**: 2026년 4월 9일
