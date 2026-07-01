<!-- i18n-source: 01-slash-commands/pr.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

---
description: 코드 정리, 변경사항 스테이징, 풀 리퀘스트 준비
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(npm test:*), Bash(npm run lint:*)
---

# 풀 리퀘스트 준비 체크리스트

PR을 생성하기 전에 다음 단계를 실행합니다:

1. 린팅 실행: `prettier --write .`
2. 테스트 실행: `npm test`
3. git diff 검토: `git diff HEAD`
4. 변경사항 스테이징: `git add .`
5. conventional commits 형식에 따라 커밋 메시지 생성:
   - `fix:` 버그 수정
   - `feat:` 새 기능
   - `docs:` 문서
   - `refactor:` 코드 재구성
   - `test:` 테스트 추가
   - `chore:` 유지보수

6. PR 요약 생성 포함:
   - 변경된 내용
   - 변경 이유
   - 수행된 테스트
   - 잠재적 영향

---
**최종 업데이트**: 2026년 4월 9일
