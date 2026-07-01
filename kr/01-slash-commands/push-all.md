<!-- i18n-source: 01-slash-commands/push-all.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

---
description: 모든 변경사항을 스테이징하고, 커밋을 생성하며, 원격으로 푸시 (주의해서 사용)
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git push:*), Bash(git diff:*), Bash(git log:*), Bash(git pull:*)
---

# 모든 변경사항 커밋 및 푸시

⚠️ **주의**: 모든 변경사항을 스테이징하고, 커밋하며, 원격으로 푸시합니다. 모든 변경사항이 함께 속해 있다고 확신할 때만 사용하세요.

## 워크플로우

### 1. 변경사항 분석
병렬 실행:
- `git status` - 수정/추가/삭제/추적되지 않은 파일 표시
- `git diff --stat` - 변경 통계 표시
- `git log -1 --oneline` - 메시지 스타일을 위한 최근 커밋 표시

### 2. 안전 검사

**❌ 감지 시 중지 및 경고:**
- 시크릿: `.env*`, `*.key`, `*.pem`, `credentials.json`, `secrets.yaml`, `id_rsa`, `*.p12`, `*.pfx`, `*.cer`
- API 키: 실제 값이 있는 `*_API_KEY`, `*_SECRET`, `*_TOKEN` 변수 (플레이스홀더 `your-api-key`, `xxx`, `placeholder` 아님)
- 대용량 파일: Git LFS 없이 `>10MB`
- 빌드 아티팩트: `node_modules/`, `dist/`, `build/`, `__pycache__/`, `*.pyc`, `.venv/`
- 임시 파일: `.DS_Store`, `thumbs.db`, `*.swp`, `*.tmp`

**API 키 검증:**
수정된 파일에서 다음과 같은 패턴 확인:
```bash
OPENAI_API_KEY=sk-proj-xxxxx  # ❌ 실제 키 감지!
AWS_SECRET_KEY=AKIA...         # ❌ 실제 키 감지!
STRIPE_API_KEY=sk_live_...    # ❌ 실제 키 감지!

# ✅ 허용 가능한 플레이스홀더:
API_KEY=your-api-key-here
SECRET_KEY=placeholder
TOKEN=xxx
API_KEY=<your-key>
SECRET=${YOUR_SECRET}
```

**✅ 확인:**
- `.gitignore`가 적절히 설정됨
- 병합 충돌 없음
- 올바른 브랜치 (main/master인 경우 경고)
- API 키가 플레이스홀더인지 확인

### 3. 확인 요청

요약 제시:
```
📊 변경사항 요약:
- X개 파일 수정, Y개 추가, Z개 삭제
- 총계: +AAA 삽입, -BBB 삭제

🔒 안전: ✅ 시크릿 없음 | ✅ 대용량 파일 없음 | ⚠️ [경고]
🌿 브랜치: [name] → origin/[name]

수행할 작업: git add . → commit → push

진행하려면 'yes', 취소하려면 'no'를 입력하세요.
```

**진행하기 전에 명시적인 "yes"를 기다립니다.**

### 4. 실행 (확인 후)

순차적으로 실행:
```bash
git add .
git status  # 스테이징 확인
```

### 5. 커밋 메시지 생성

변경사항을 분석하고 conventional commit 생성:

**형식:**
```
[type]: 간단한 요약 (최대 72자)

- 주요 변경사항 1
- 주요 변경사항 2
- 주요 변경사항 3
```

**타입:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `build`, `ci`

**예제:**
```
docs: 종합 문서와 함께 개념 README 파일 업데이트

- 아키텍처 다이어그램 및 테이블 추가
- 실제 예제 포함
- 모범 사례 섹션 확장
```

### 6. 커밋 및 푸시

```bash
git commit -m "$(cat <<'EOF'
[생성된 커밋 메시지]
EOF
)"
git push  # 실패 시: git pull --rebase && git push
git log -1 --oneline --decorate  # 확인
```

### 7. 성공 확인

```
✅ 원격 푸시 성공!

커밋: [hash] [message]
브랜치: [branch] → origin/[branch]
변경된 파일: X (+삽입, -삭제)
```

## 오류 처리

- **git add 실패**: 권한 확인, 잠긴 파일 확인, 저장소 초기화 확인
- **git commit 실패**: pre-commit 훅 수정, git config 확인 (user.name/email)
- **git push 실패**:
  - Non-fast-forward: `git pull --rebase && git push`
  - 원격 브랜치 없음: `git push -u origin [branch]`
  - 보호된 브랜치: 대신 PR 워크플로우 사용

## 사용 시기

✅ **적합:**
- 여러 파일의 문서 업데이트
- 테스트 및 문서가 포함된 기능
- 여러 파일에 걸친 버그 수정
- 프로젝트 전체 포맷팅/리팩토링
- 설정 변경

❌ **피해야 할 경우:**
- 무엇이 커밋되는지 확실하지 않을 때
- 시크릿/민감 데이터 포함
- 검토 없이 보호된 브랜치
- 병합 충돌이 있는 경우
- 세분화된 커밋 기록을 원할 때
- pre-commit 훅이 실패할 때

## 대안

사용자가 제어를 원하면 다음을 제안:
1. **선택적 스테이징**: 특정 파일 검토/스테이징
2. **인터랙티브 스테이징**: 패치 선택을 위한 `git add -p`
3. **PR 워크플로우**: 브랜치 생성 → 푸시 → PR (`/pr` 커맨드 사용)

**⚠️ 기억하세요**: 푸시 전에 항상 변경사항을 검토하세요. 확실하지 않으면 개별 git 커맨드를 사용하여 더 많은 제어권을 확보하세요.

---
**최종 업데이트**: 2026년 4월 9일
