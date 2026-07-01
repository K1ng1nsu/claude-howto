<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>
<!-- i18n-source: QUICK_REFERENCE.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

# Claude Code 예제 — 빠른 참조 카드

## 🚀 설치 빠른 명령어

### 슬래시 명령어
```bash
# 모두 설치
cp 01-slash-commands/*.md .claude/commands/

# 특정 항목 설치
cp 01-slash-commands/optimize.md .claude/commands/
```

### 메모리
```bash
# 프로젝트 메모리
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 개인 메모리
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

### 스킬
```bash
# 개인 스킬
cp -r 03-skills/code-review-specialist ~/.claude/skills/

# 프로젝트 스킬
cp -r 03-skills/code-review-specialist .claude/skills/
```

### 서브에이전트
```bash
# 모두 설치
cp 04-subagents/*.md .claude/agents/

# 특정 항목 설치
cp 04-subagents/code-reviewer.md .claude/agents/
```

### MCP
```bash
# 자격 증명 설정
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 설정 설치 (프로젝트 범위)
cp 05-mcp/github-mcp.json .mcp.json

# 또는 사용자 범위: ~/.claude.json에 추가
```

### 훅
```bash
# 훅 설치
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 설정에서 구성 (~/.claude/settings.json)
```

### 플러그인
```bash
# 예제에서 설치 (게시된 경우)
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

### 체크포인트
```bash
# 체크포인트는 모든 사용자 프롬프트마다 자동 생성
# 되감기는 Esc를 두 번 누르거나 다음 사용:
/rewind

# 그런 다음 선택: 코드와 대화 복원, 대화 복원,
# 코드 복원, 여기서부터 요약, 또는 취소
```

### 고급 기능
```bash
# 설정에서 구성 (.claude/settings.json)
# 09-advanced-features/config-examples.json 참조

# 계획 모드
/plan Task description

# 권한 모드 (--permission-mode 플래그 사용)
# default        - 위험한 작업에 승인 요청
# acceptEdits    - 파일 편집 자동 승인, 나머지는 요청
# plan           - 읽기 전용 분석, 수정 없음
# dontAsk        - 위험한 작업을 제외한 모든 작업 승인
# auto           - 백그라운드 분류기가 권한을 자동 결정
# bypassPermissions - 모든 작업 승인 (--dangerously-skip-permissions 필요)

# 세션 관리
/resume                # 이전 대화 재개
/rename "name"         # 현재 세션 이름 지정
/fork                  # 현재 세션 포크
claude -c              # 가장 최근 대화 계속
claude -r "session"    # 세션 이름/ID로 재개
```

---

## 📋 기능 치트 시트

| 기능 | 설치 경로 | 사용법 |
|------|----------|-------|
| **슬래시 명령어 (60+)** | `.claude/commands/*.md` | `/command-name` |
| **메모리** | `./CLAUDE.md` | 자동 로드 |
| **스킬** | `.claude/skills/*/SKILL.md` | 자동 호출 |
| **서브에이전트** | `.claude/agents/*.md` | 자동 위임 |
| **MCP** | `.mcp.json` (프로젝트) 또는 `~/.claude.json` (사용자) | `/mcp__server__action` |
| **훅 (29개 이벤트)** | `~/.claude/hooks/*.sh` | 이벤트 트리거 (5가지 유형) |
| **플러그인** | `/plugin install` 통해 | 모든 것 번들 |
| **체크포인트** | 내장 | `Esc+Esc` 또는 `/rewind` |
| **계획 모드** | 내장 | `/plan <작업>` |
| **권한 모드 (6개)** | 내장 | `--allowedTools`, `--permission-mode` |
| **세션** | 내장 | `/session <명령어>` |
| **백그라운드 태스크** | 내장 | 백그라운드 실행 |
| **원격 제어** | 내장 | WebSocket API |
| **웹 세션** | 내장 | `claude web` |
| **Git Worktrees** | 내장 | `/worktree` |
| **자동 메모리** | 내장 | CLAUDE.md에 자동 저장 |
| **작업 목록** | 내장 | `/task list` |
| **번들 스킬 (10개)** | 내장 | `/batch`, `/claude-api`, `/code-review`, `/simplify` *(정리 전용 리뷰; v2.1.154부터 `/code-review`와 다시 구분됨)*, `/debug`, `/fewer-permission-prompts`, `/loop`, `/run` *(v2.1.145+)*, `/run-skill-generator` *(v2.1.145+)*, `/verify` *(v2.1.145+)* |

---

## 🎯 일반적인 사용 사례

### 코드 리뷰
```bash
# 방법 1: 슬래시 명령어
cp 01-slash-commands/optimize.md .claude/commands/
# 사용: /optimize

# 방법 2: 서브에이전트
cp 04-subagents/code-reviewer.md .claude/agents/
# 사용: 자동 위임

# 방법 3: 스킬
cp -r 03-skills/code-review-specialist ~/.claude/skills/
# 사용: 자동 호출

# 방법 4: 플러그인 (최고)
/plugin install pr-review
# 사용: /review-pr
```

### 문서화
```bash
# 슬래시 명령어
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# 서브에이전트
cp 04-subagents/documentation-writer.md .claude/agents/

# 스킬
cp -r 03-skills/doc-generator ~/.claude/skills/

# 플러그인 (완전한 솔루션)
/plugin install documentation
```

### DevOps
```bash
# 완전한 플러그인
/plugin install devops-automation

# 명령어: /deploy, /rollback, /status, /incident
```

### 팀 표준
```bash
# 프로젝트 메모리
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 팀에 맞게 편집
vim CLAUDE.md
```

### 자동화 및 훅
```bash
# 훅 설치 (29개 이벤트, 5가지 유형: command, http, mcp_tool, prompt, agent)
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 예제:
# - 커밋 전 테스트: pre-commit.sh
# - 자동 코드 포맷: format-code.sh
# - 보안 스캐닝: security-scan.sh

# 완전 자율 워크플로우를 위한 오토 모드
claude --enable-auto-mode -p "Refactor and test the auth module"
# 또는 Shift+Tab으로 대화형 모드 전환
```

### 안전한 리팩토링
```bash
# 체크포인트는 각 프롬프트 전에 자동 생성
# 리팩토링 시도
# 성공하면: 계속
# 실패하면: Esc+Esc 또는 /rewind를 사용하여 되돌아가기
```

### 복잡한 구현
```bash
# 계획 모드 사용
/plan Implement user authentication system

# Claude가 상세 계획 수립
# 검토 및 승인
# Claude가 체계적으로 구현
```

### CI/CD 통합
```bash
# 헤드리스 모드에서 실행 (비대화형)
claude -p "Run all tests and generate report"

# CI를 위한 권한 모드
claude -p "Run tests" --permission-mode dontAsk

# 완전 자율 CI 작업을 위한 오토 모드
claude --enable-auto-mode -p "Run tests and fix failures"

# 자동화를 위한 훅
# 09-advanced-features/README.md 참조
```

### 학습 및 실험
```bash
# 안전한 분석을 위한 계획 모드 사용
claude --permission-mode plan

# 안전하게 실험 - 체크포인트가 자동 생성됨
# 되감기가 필요하면: Esc+Esc 또는 /rewind 사용
```

### 에이전트 팀
```bash
# 에이전트 팀 활성화
export CLAUDE_AGENT_TEAMS=1

# 또는 settings.json에서
{ "agentTeams": { "enabled": true } }

# 시작: "팀 접근 방식으로 기능 X를 구현해줘"
```

### 예약된 태스크
```bash
# 5분마다 명령어 실행
/loop 5m /check-status

# 일회성 알림
/loop 30m "배포 상태 확인해줘"
```

---

## 📁 파일 위치 참조

```
Your Project/
├── .claude/
│   ├── commands/              # 슬래시 명령어 위치
│   ├── agents/                # 서브에이전트 위치
│   ├── skills/                # 프로젝트 스킬 위치
│   └── settings.json          # 프로젝트 설정 (훅 등)
├── .mcp.json                  # MCP 설정 (프로젝트 범위)
├── CLAUDE.md                  # 프로젝트 메모리
└── src/
    └── api/
        └── CLAUDE.md          # 디렉토리별 메모리

User Home/
├── .claude/
│   ├── commands/              # 개인 명령어
│   ├── agents/                # 개인 에이전트
│   ├── skills/                # 개인 스킬
│   ├── hooks/                 # 훅 스크립트
│   ├── settings.json          # 사용자 설정
│   ├── managed-settings.d/    # 관리 설정 (엔터프라이즈/조직)
│   └── CLAUDE.md              # 개인 메모리
└── .claude.json               # 개인 MCP 설정 (사용자 범위)
```

---

## 🔍 예제 찾기

### 카테고리별
- **슬래시 명령어**: `01-slash-commands/`
- **메모리**: `02-memory/`
- **스킬**: `03-skills/`
- **서브에이전트**: `04-subagents/`
- **MCP**: `05-mcp/`
- **훅**: `06-hooks/`
- **플러그인**: `07-plugins/`
- **체크포인트**: `08-checkpoints/`
- **고급 기능**: `09-advanced-features/`
- **CLI**: `10-cli/`

### 사용 사례별
- **성능**: `01-slash-commands/optimize.md`
- **보안**: `04-subagents/secure-reviewer.md`
- **테스트**: `04-subagents/test-engineer.md`
- **문서**: `03-skills/doc-generator/`
- **DevOps**: `07-plugins/devops-automation/`

### 복잡도별
- **간단**: 슬래시 명령어
- **중간**: 서브에이전트, 메모리
- **고급**: 스킬, 훅
- **완전**: 플러그인

---

## 🎓 학습 경로

### 1일차
```bash
# 개요 읽기
cat README.md

# 명령어 설치
cp 01-slash-commands/optimize.md .claude/commands/

# 사용해보기
/optimize
```

### 2-3일차
```bash
# 메모리 설정
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
vim CLAUDE.md

# 서브에이전트 설치
cp 04-subagents/code-reviewer.md .claude/agents/
```

### 4-5일차
```bash
# MCP 설정
export GITHUB_TOKEN="your_token"
cp 05-mcp/github-mcp.json .mcp.json

# MCP 명령어 시도
/mcp__github__list_prs
```

### 2주차
```bash
# 스킬 설치
cp -r 03-skills/code-review-specialist ~/.claude/skills/

# 자동 호출 실행
# 그냥 말해보세요: "이 코드에서 문제점을 검토해줘"
```

### 3주차+
```bash
# 완전한 플러그인 설치
/plugin install pr-review

# 번들 기능 사용
/review-pr
/check-security
/check-tests
```

---

## 새로운 기능 (2026년 5월)

| 기능 | 설명 | 사용법 |
|------|------|--------|
| **오토 모드** | 백그라운드 분류기를 사용한 완전 자율 운영 | `--enable-auto-mode` 플래그, `Shift+Tab`으로 모드 전환 |
| **채널** | Discord 및 Telegram 통합 | `--channels` 플래그, Discord/Telegram 봇 |
| **음성 받아쓰기** | Claude에게 명령어와 컨텍스트 말하기 | `/voice` 명령어 |
| **훅 (29개 이벤트)** | 5가지 유형의 확장된 훅 시스템 | command, http, mcp_tool, prompt, agent 훅 유형 |
| **MCP Elicitation** | MCP 서버가 런타임에 사용자 입력 요청 가능 | 서버가 명확화가 필요할 때 자동 프롬프트 |
| **플러그인 LSP** | 플러그인용 언어 서버 프로토콜 지원 | `userConfig`, `${CLAUDE_PLUGIN_DATA}` 변수 |
| **원격 제어** | WebSocket API를 통한 Claude Code 제어 | `claude --remote`로 외부 통합 |
| **웹 세션** | 브라우저 기반 Claude Code 인터페이스 | `claude web`으로 실행 |
| **데스크탑 앱** | 네이티브 데스크탑 애플리케이션 | claude.ai/download에서 다운로드 |
| **작업 목록** | 백그라운드 태스크 관리 | `/task list`, `/task status <id>` |
| **자동 메모리** | 대화에서 자동 메모리 저장 | Claude가 주요 컨텍스트를 CLAUDE.md에 자동 저장 |
| **Git Worktrees** | 병렬 개발을 위한 격리된 워크스페이스 | `/worktree`로 격리된 워크스페이스 생성 |
| **모델 선택** | Sonnet 4.6, Opus 4.8, Haiku 4.5 전환 | `/model` — v2.1.153부터 선택한 모델이 새 세션의 기본값으로 저장됨; `s`를 누르면 세션 전용 |
| **에이전트 팀** | 작업에 여러 에이전트 조정 | `CLAUDE_AGENT_TEAMS=1` 환경 변수로 활성화 |
| **동적 워크플로우** *(v2.1.154)* | 결정론적 다중 에이전트 오케스트레이션 | `/workflows`로 실행 확인; Claude에게 생성 요청 |
| **예약된 태스크** | `/loop`를 사용한 반복 작업 | `/loop 5m /command` 또는 CronCreate 도구 |
| **Chrome 통합** | 브라우저 자동화 | `--chrome` 플래그 또는 `/chrome` 명령어 |
| **키보드 사용자 정의** | 커스텀 키바인딩 | `/keybindings` 명령어 |
| **/usage-credits** | 추가 사용 제한 설정 (v2.1.144에서 `/extra-usage`에서 이름 변경; 이전 이름도 별칭으로 작동) | `/usage-credits` |
| **/run** *(v2.1.145+)* | 변경 사항을 확인하기 위해 프로젝트 앱 실행 | `/run` |
| **/verify** *(v2.1.145+)* | 앱을 빌드, 실행, 관찰하여 수정 사항 확인 | `/verify` |
| **/run-skill-generator** *(v2.1.145+)* | 특정 프로젝트를 `/run`/`/verify`가 처리하는 방법 교육 | `/run-skill-generator` |

---

## 팁 & 트릭

### 사용자 정의
- 예제를 있는 그대로 시작
- 필요에 맞게 수정
- 팀과 공유 전에 테스트
- 설정을 버전 관리

### 모범 사례
- 팀 표준에 메모리 사용
- 완전한 워크플로우에 플러그인 사용
- 복잡한 작업에 서브에이전트 사용
- 빠른 작업에 슬래시 명령어 사용

### 문제 해결
```bash
# 파일 위치 확인
ls -la .claude/commands/
ls -la .claude/agents/

# YAML 구문 확인
head -20 .claude/agents/code-reviewer.md

# MCP 연결 테스트
echo $GITHUB_TOKEN
```

---

## 📊 기능 매트릭스

| 필요 사항 | 이 기능 사용 | 예제 |
|----------|-------------|------|
| 빠른 단축키 | 슬래시 명령어 (60+) | `01-slash-commands/optimize.md` |
| 팀 표준 | 메모리 | `02-memory/project-CLAUDE.md` |
| 자동 워크플로우 | 스킬 | `03-skills/code-review-specialist/` |
| 전문화된 작업 | 서브에이전트 | `04-subagents/code-reviewer.md` |
| 외부 데이터 | MCP (+ Elicitation) | `05-mcp/github-mcp.json` |
| 이벤트 자동화 | 훅 (29개 이벤트, 5가지 유형) | `06-hooks/pre-commit.sh` |
| 완전한 솔루션 | 플러그인 (+ LSP 지원) | `07-plugins/pr-review/` |
| 안전한 실험 | 체크포인트 | `08-checkpoints/checkpoint-examples.md` |
| 완전 자율 | 오토 모드 | `--enable-auto-mode` 또는 `Shift+Tab` |
| 채팅 통합 | 채널 | `--channels` (Discord, Telegram) |
| CI/CD 파이프라인 | CLI | `10-cli/README.md` |

---

## 🔗 빠른 링크

- **메인 가이드**: `README.md`
- **전체 인덱스**: `INDEX.md`
- **원본 가이드**: `claude_concepts_guide.md`

---

## 📞 자주 묻는 질문

**Q: 어떤 것을 사용해야 하나요?**
A: 슬래시 명령어로 시작하고 필요에 따라 기능을 추가하세요.

**Q: 기능을 혼합할 수 있나요?**
A: 네! 함께 작동합니다. 메모리 + 명령어 + MCP = 강력함.

**Q: 팀과 어떻게 공유하나요?**
A: `.claude/` 디렉토리를 git에 커밋하세요.

**Q: 비밀 정보는 어떻게 하나요?**
A: 환경 변수를 사용하고, 절대 하드코딩하지 마세요.

**Q: 예제를 수정할 수 있나요?**
A: 물론입니다! 템플릿이므로 자유롭게 커스터마이징하세요.

---

## ✅ 체크리스트

시작 체크리스트:

- [ ] `README.md` 읽기
- [ ] 슬래시 명령어 1개 설치
- [ ] 명령어 사용해보기
- [ ] 프로젝트 `CLAUDE.md` 생성
- [ ] 서브에이전트 1개 설치
- [ ] MCP 통합 1개 설정
- [ ] 스킬 1개 설치
- [ ] 완전한 플러그인 사용해보기
- [ ] 필요에 맞게 커스터마이징
- [ ] 팀과 공유

---

**빠른 시작**: `cat README.md`

**전체 인덱스**: `cat INDEX.md`

**이 카드**: 빠른 참조를 위해 가까이 두세요!

---
**최종 업데이트**: 2026년 6월 2일
**Claude Code 버전**: 2.1.160
**출처**:
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/commands
- https://github.com/anthropics/claude-code/releases/tag/v2.1.153
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
**호환 모델**: Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
