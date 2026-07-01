<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>
<!-- i18n-source: CATALOG.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

# Claude Code 기능 카탈로그

> 모든 Claude Code 기능에 대한 빠른 참조 가이드: 명령어, 에이전트, 스킬, 플러그인, 훅.

**탐색**: [명령어](#슬래시-명령어) | [권한 모드](#권한-모드) | [서브에이전트](#서브에이전트) | [스킬](#스킬) | [플러그인](#플러그인) | [MCP 서버](#mcp-서버) | [훅](#훅) | [메모리 파일](#메모리-파일) | [새로운 기능](#새로운-기능-2026년-5월)

---

## 요약

| 기능 | 내장 | 예제 | 전체 | 참조 |
|------|------|------|------|------|
| **슬래시 명령어** | 60+ | 8 | 68+ | [01-slash-commands/](01-slash-commands/) |
| **서브에이전트** | 6 | 11 | 17 | [04-subagents/](04-subagents/) |
| **스킬** | 9개 번들 | 6 | 15 | [03-skills/](03-skills/) |
| **플러그인** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCP 서버** | 1 | 8 | 9 | [05-mcp/](05-mcp/) |
| **훅** | 29개 이벤트 | 8 | 8 | [06-hooks/](06-hooks/) |
| **메모리** | 7가지 유형 | 3 | 3 | [02-memory/](02-memory/) |
| **합계** | **103** | **47** | **125** | |

---

## 슬래시 명령어

명령어는 사용자가 호출하여 특정 작업을 실행하는 단축키입니다.

### 내장 명령어

| 명령어 | 설명 | 사용 시기 |
|--------|------|---------|
| `/help` | 도움말 정보 표시 | 시작하기, 명령어 배우기 |
| `/btw` | 임시 사이드 질문 — 메인 컨텍스트를 오염시키지 않음 | 빠른 잡다한 질문 |
| `/chrome` | Chrome 통합 설정 | 브라우저 자동화 |
| `/clear` | 대화 기록 지우기 | 새로 시작, 컨텍스트 줄이기 |
| `/diff` | 대화형 diff 뷰어 | 변경 사항 검토 |
| `/config` | 설정 보기/편집 | 동작 사용자 정의 |
| `/status` | 세션 상태 표시 | 현재 상태 확인 |
| `/agents` | 사용 가능한 에이전트 목록 | 위임 옵션 확인 |
| `/skills` | 사용 가능한 스킬 목록 | 자동 호출 기능 확인 |
| `/hooks` | 설정된 훅 목록 | 자동화 디버그 |
| `/insights` | 세션 패턴 분석 | 세션 최적화 |
| `/install-slack-app` | Claude Slack 앱 설치 | Slack 통합 |
| `/keybindings` | 키보드 단축키 사용자 정의 | 키 사용자 정의 |
| `/mcp` | MCP 서버 목록 | 외부 통합 확인 |
| `/memory` | 로드된 메모리 파일 보기 | 컨텍스트 로드 디버그 |
| `/mobile` | 모바일 QR 코드 생성 | 모바일 접근 |
| `/passes` | 사용 패스 보기 | 구독 정보 |
| `/plugin` | 플러그인 관리 | 확장 기능 설치/제거 |
| `/plan` | 계획 모드 진입 | 복잡한 구현 |
| `/proactive` | `/loop`의 별칭 (v2.1.105) | `/loop`와 동일 |
| `/recap` | 세션 복귀 시 세션 요약 표시 | 자리 비움 후 수행된 작업 컨텍스트 확인 |
| `/rewind` | 체크포인트로 되감기 | 변경 취소, 대안 탐색 |
| `/checkpoint` | 체크포인트 관리 | 상태 저장/복원 |
| `/cost` | `/usage`의 비용 탭을 여는 단축키 별칭 (v2.1.118+) | 지출 모니터링 |
| `/context` | 컨텍스트 창 사용량 표시 | 대화 길이 관리 |
| `/export` | 대화 내보내기 | 참조용 저장 |
| `/usage-credits` | 추가 사용 제한 설정 (v2.1.144에서 `/extra-usage`에서 이름 변경; 이전 이름도 별칭으로 작동) | 요율 제한 관리 |
| `/feedback` | 피드백 또는 버그 리포트 제출 | 이슈 보고 |
| `/login` | Anthropic 인증 | 기능 접근 |
| `/logout` | 로그아웃 | 계정 전환 |
| `/sandbox` | 샌드박스 모드 전환 | 안전한 명령어 실행 |
| `/doctor` | 진단 실행 | 문제 해결 |
| `/reload-plugins` | 설치된 플러그인 다시 로드 | 플러그인 관리 |
| `/reload-skills` | 재시작 없이 스킬 디렉토리 다시 스캔 (v2.1.152) | 스킬 관리 |
| `/workflows` | 실행 중 및 완료된 동적 워크플로우 실행 보기 (v2.1.154) | 다중 에이전트 오케스트레이션 |
| `/release-notes` | 릴리스 노트 표시 | 새 기능 확인 |
| `/remote-control` | 원격 제어 활성화 | 원격 접근 |
| `/permissions` | 권한 관리 | 접근 제어 |
| `/session` | 세션 관리 | 다중 세션 워크플로우 |
| `/rename` | 현재 세션 이름 변경 | 세션 정리 |
| `/resume` | 이전 세션 재개 | 작업 계속 |
| `/todo` | 할 일 목록 보기/관리 | 작업 추적 |
| `/tui` | 전체화면 TUI(텍스트 사용자 인터페이스) 모드 전환 | 전체화면/tmux에서 깜빡임 없는 렌더링 |
| `/tasks` | 백그라운드 태스크 보기 | 비동기 작업 모니터링 |
| `/copy` | 마지막 응답을 클립보드에 복사 | 출력 빠르게 공유 |
| `/teleport` | 세션을 다른 머신으로 전송 | 원격으로 작업 계속 |
| `/desktop` | Claude 데스크탑 앱 열기 | 데스크탑 인터페이스로 전환 |
| `/theme` | 색상 테마 변경; v2.1.118에서 `~/.claude/themes/<name>.json`을 통한 커스텀 네임드 테마 추가 (플러그인이 `themes/` 디렉토리 제공 가능) | 외관 사용자 정의 |
| `/usage` | 사용량/비용/통계를 위한 정식 명령어 — `/cost`와 `/stats`를 단일 탭 뷰로 통합 (v2.1.118); v2.1.149부터 비용 보기를 카테고리별(스킬, 서브에이전트, 플러그인, MCP 서버별)로 세분화. **VSCode 확장** (v2.1.174)에서 `/usage` (계정 및 사용량) 대화상자에 속성 분석 추가 — 캐시 미스, 긴 컨텍스트 비용, 서브에이전트, 그리고 스킬/에이전트/플러그인/MCP별 사용량 (24시간 및 7일 기간) | 할당량 및 비용 모니터링 |
| `/focus` | 포커스 뷰 전환 (방해 없는 출력 표시) | 긴 작업 중 시각적 소음 감소 |
| `/fork` | 현재 대화 포크 | 대안 탐색 |
| `/stats` | `/usage`의 통계 탭을 여는 단축키 별칭 (v2.1.118+) | 세션 메트릭 검토 |
| `/statusline` | 상태 줄 설정 | 상태 표시 사용자 정의 |
| `/stickers` | 세션 스티커 보기 | 재미있는 보상 |
| `/fast` | 빠른 출력 모드 전환 | 응답 속도 향상 |
| `/terminal-setup` | 터미널 통합 설정 | 터미널 기능 설정 |
| `/undo` | `/rewind`의 별칭 (v2.1.108) | `/rewind`와 동일 |
| `/upgrade` | 업데이트 확인 | 버전 관리 |
| `/team-onboarding` | 이 프로젝트의 Claude Code 사용에서 팀원 온보딩 가이드 생성 | 새 팀원 온보딩 (v2.1.101) |
| `/ultraplan` | 계획 모드의 Claude Code 웹 세션에 계획 작업 전달 | 대규모 계획 오프로드 (Research Preview, v2.1.91+) |
| `/ultrareview` | 현재 변경 사항에 대해 클라우드 다중 에이전트 코드 리뷰 실행 | 여러 에이전트를 통한 심층 병합 전 리뷰 (v2.1.112) |
| `/less-permission-prompts` | 트랜스크립트를 스캔하고 일반적인 읽기 전용 도구에 대한 우선순위 허용 목록 제안 | 프로젝트에서 반복 권한 프롬프트 감소 (v2.1.112) |

### 커스텀 명령어 (예제)

| 명령어 | 설명 | 사용 시기 | 범위 | 설치 |
|--------|------|---------|------|------|
| `/optimize` | 최적화를 위한 코드 분석 | 성능 개선 | 프로젝트 | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | 풀 리퀘스트 준비 | PR 제출 전 | 프로젝트 | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | API 문서 생성 | API 문서화 | 프로젝트 | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | 컨텍스트와 함께 git 커밋 생성 | 변경 사항 커밋 | 사용자 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | 스테이징, 커밋, 푸시 | 빠른 배포 | 사용자 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | 문서 재구성 | 문서 개선 | 프로젝트 | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | CI/CD 파이프라인 설정 | 새 프로젝트 | 프로젝트 | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | 테스트 커버리지 확장 | 테스트 개선 | 프로젝트 | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **범위**: `사용자` = 개인 워크플로우 (`~/.claude/commands/`), `프로젝트` = 팀 공유 (`.claude/commands/`)

**참조**: [01-slash-commands/](01-slash-commands/) | [공식 문서](https://code.claude.com/docs/en/interactive-mode)

**빠른 설치 (모든 커스텀 명령어)**:
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## 권한 모드

Claude Code는 도구 사용 권한을 제어하는 6가지 권한 모드를 지원합니다.

| 모드 | 설명 | 사용 시기 |
|------|------|---------|
| `default` | 각 도구 호출마다 확인 | 표준 대화형 사용 |
| `acceptEdits` | 파일 편집 자동 승인, 나머지는 확인 | 신뢰할 수 있는 편집 워크플로우 |
| `plan` | 읽기 전용 도구만, 쓰기 없음 | 계획 및 탐색 |
| `auto` | 확인 없이 모든 도구 승인 | 완전 자율 운영 (Research Preview) |
| `bypassPermissions` | 모든 권한 검사 건너뛰기 | CI/CD, 헤드리스 환경 |
| `dontAsk` | 권한이 필요한 도구 건너뛰기 | 비대화형 스크립팅 |

> **참고**: `auto` 모드는 Research Preview 기능입니다 (2026년 3월). `bypassPermissions`는 신뢰할 수 있는 샌드박스 환경에서만 사용하세요.

**참조**: [공식 문서](https://code.claude.com/docs/en/permissions)

---

## 서브에이전트

특정 작업을 위해 격리된 컨텍스트를 갖춘 전문화된 AI 어시스턴트.

> **중첩 생성 (v2.1.172)**: 서브에이전트가 자신의 서브에이전트를 생성할 수 있으며, 최대 5레벨 깊이까지 중첩 가능합니다. 이전 버전에서는 중첩이 허용되지 않았습니다. 특정 서브에이전트가 생성할 수 있는 하위 에이전트를 제한하는 `Agent(agent_type)` 구문은 [04-subagents/README.md](04-subagents/README.md#생성-가능한-서브에이전트-제한)를 참조하세요.

### 내장 서브에이전트

| 에이전트 | 설명 | 도구 | 모델 | 사용 시기 |
|---------|------|------|------|---------|
| **general-purpose** | 다단계 작업, 조사 | 모든 도구 | 모델 상속 | 복잡한 조사, 다중 파일 작업 |
| **Plan** | 구현 계획 | Read, Glob, Grep, Bash | 모델 상속 | 아키텍처 설계, 계획 |
| **Explore** | 코드베이스 탐색 | Read, Glob, Grep | Haiku 4.5 | 빠른 검색, 코드 이해 |
| **Bash** | 명령어 실행 | Bash | 모델 상속 | Git 작업, 터미널 작업 |
| **statusline-setup** | 상태 줄 설정 | Bash, Read, Write | Sonnet 4.6 | 상태 줄 표시 설정 |
| **Claude Code Guide** | 도움말 및 문서 | Read, Glob, Grep | Haiku 4.5 | 도움말 보기, 기능 학습 |

### 서브에이전트 설정 필드

| 필드 | 타입 | 설명 |
|------|------|------|
| `name` | string | 에이전트 식별자 |
| `description` | string | 에이전트가 하는 일 |
| `model` | string | 모델 재정의 (예: `haiku-4.5`) |
| `tools` | array | 허용된 도구 목록 |
| `effort` | string | 추론 노력 수준 (`low`, `medium`, `high`) |
| `initialPrompt` | string | 에이전트 시작 시 주입되는 시스템 프롬프트 |
| `disallowedTools` | array | 이 에이전트에 명시적으로 거부된 도구 |

### 커스텀 서브에이전트 (예제)

| 에이전트 | 설명 | 사용 시기 | 범위 | 설치 |
|---------|------|---------|------|------|
| `code-reviewer` | 종합적인 코드 품질 | 코드 리뷰 세션 | 프로젝트 | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `code-architect` | 기능 아키텍처 설계 | 새 기능 계획 | 프로젝트 | `cp 04-subagents/code-architect.md .claude/agents/` |
| `code-explorer` | 심층 코드베이스 분석 | 기존 기능 이해 | 프로젝트 | `cp 04-subagents/code-explorer.md .claude/agents/` |
| `clean-code-reviewer` | Clean Code 원칙 리뷰 | 유지보수성 리뷰 | 프로젝트 | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | 테스트 전략 및 커버리지 | 테스트 계획 | 프로젝트 | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 기술 문서 | API 문서, 가이드 | 프로젝트 | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | 보안 중심 리뷰 | 보안 감사 | 프로젝트 | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | 전체 기능 구현 | 기능 개발 | 프로젝트 | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 근본 원인 분석 | 버그 조사 | 사용자 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL 쿼리, 데이터 분석 | 데이터 작업 | 사용자 | `cp 04-subagents/data-scientist.md .claude/agents/` |
| `performance-optimizer` | 프로파일링 및 성능 튜닝 | 병목 현상 조사 | 프로젝트 | `cp 04-subagents/performance-optimizer.md .claude/agents/` |

> **범위**: `사용자` = 개인 (`~/.claude/agents/`), `프로젝트` = 팀 공유 (`.claude/agents/`)

**참조**: [04-subagents/](04-subagents/) | [공식 문서](https://code.claude.com/docs/en/sub-agents)

**빠른 설치 (모든 커스텀 에이전트)**:
```bash
cp 04-subagents/*.md .claude/agents/
```

---

## 스킬

지침, 스크립트, 템플릿을 갖춘 자동 호출 기능.

### 예제 스킬

| 스킬 | 설명 | 자동 호출 시기 | 범위 | 설치 |
|------|------|--------------|------|------|
| `code-review-specialist` | 종합적인 코드 리뷰 | "이 코드 검토해줘", "품질 확인" | 프로젝트 | `cp -r 03-skills/code-review-specialist .claude/skills/` |
| `brand-voice` | 브랜드 일관성 검사기 | 마케팅 카피 작성 | 프로젝트 | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 문서 생성기 | "문서 생성해줘", "API 문서화" | 프로젝트 | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 체계적인 코드 리팩토링 (Martin Fowler) | "리팩토링 해줘", "코드 정리" | 사용자 | `cp -r 03-skills/refactor ~/.claude/skills/` |

> **범위**: `사용자` = 개인 (`~/.claude/skills/`), `프로젝트` = 팀 공유 (`.claude/skills/`)

### 스킬 구조

```
~/.claude/skills/skill-name/
├── SKILL.md          # 스킬 정의 및 지침
├── scripts/          # 헬퍼 스크립트
└── templates/        # 출력 템플릿
```

### 스킬 Frontmatter 필드

스킬은 설정을 위해 `SKILL.md`에 YAML frontmatter를 지원합니다:

| 필드 | 타입 | 설명 |
|------|------|------|
| `name` | string | 스킬 표시 이름 |
| `description` | string | 스킬이 하는 일 |
| `autoInvoke` | array | 자동 호출을 위한 트리거 구문 |
| `effort` | string | 추론 노력 수준 (`low`, `medium`, `high`) |
| `shell` | string | 스크립트에 사용할 셸 (`bash`, `zsh`, `sh`) |

**참조**: [03-skills/](03-skills/) | [공식 문서](https://code.claude.com/docs/en/skills)

**빠른 설치 (모든 스킬)**:
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### 번들 스킬

| 스킬 | 설명 | 자동 호출 시기 |
|------|------|--------------|
| `/batch` | 여러 파일에서 프롬프트 실행 | 배치 작업 |
| `/claude-api` | Claude API로 앱 구축 | API 개발 |
| `/debug` | 실패하는 테스트/오류 디버그 | 디버깅 세션 |
| `/fewer-permission-prompts` | 트랜스크립트 스캔 및 우선순위 허용 목록 제안 | 반복 권한 프롬프트 감소 |
| `/loop` | 간격으로 프롬프트 실행 | 반복 작업 |
| `/run` *(v2.1.145+)* | 변경 사항을 확인하기 위해 프로젝트 앱 실행 | 실제 앱에서 변경 확인 |
| `/run-skill-generator` *(v2.1.145+)* | 특정 프로젝트를 `/run`/`/verify`가 처리하는 방법 교육 | `/run`을 위한 첫 프로젝트 설정 |
| `/code-review` | 선택한 노력 수준에서 현재 diff의 정확성 버그 리뷰 (예: `/code-review high`); `--comment`를 전달하여 인라인 PR 코멘트로 결과 게시 | 코드 작성 후, PR 병합 전 |
| `/simplify` *(v2.1.154부터 다시 구분됨)* | 버그를 찾지 않고 수정을 적용하는 정리 전용 리뷰 (재사용/단순화/효율성/수준) | 버그 사냥 없이 코드 정리 |
| `/verify` *(v2.1.145+)* | 앱을 빌드, 실행, 관찰하여 수정 사항 확인 | 종단간 수정 검증 |

---

## 플러그인

명령어, 에이전트, MCP 서버, 훅의 번들 컬렉션.

### 예제 플러그인

| 플러그인 | 설명 | 구성 요소 | 사용 시기 | 범위 | 설치 |
|---------|------|---------|---------|------|------|
| `pr-review` | PR 리뷰 워크플로우 | 3개 명령어, 3개 에이전트, GitHub MCP | 코드 리뷰 | 프로젝트 | `/plugin install pr-review` |
| `devops-automation` | 배포 및 모니터링 | 4개 명령어, 3개 에이전트, K8s MCP | DevOps 작업 | 프로젝트 | `/plugin install devops-automation` |
| `documentation` | 문서 생성 제품군 | 4개 명령어, 3개 에이전트, 템플릿 | 문서화 | 프로젝트 | `/plugin install documentation` |

> **범위**: `프로젝트` = 팀 공유, `사용자` = 개인 워크플로우

### 플러그인 구조

```
.claude-plugin/
├── plugin.json       # 매니페스트 파일
├── commands/         # 슬래시 명령어
├── agents/           # 서브에이전트
├── skills/           # 스킬
├── mcp/              # MCP 설정
├── hooks/            # 훅 스크립트
└── scripts/          # 유틸리티 스크립트
```

**참조**: [07-plugins/](07-plugins/) | [공식 문서](https://code.claude.com/docs/en/plugins)

**플러그인 관리 명령어**:
```bash
/plugin list              # 설치된 플러그인 목록
/plugin install <name>    # 플러그인 설치
/plugin remove <name>     # 플러그인 제거
/plugin update <name>     # 플러그인 업데이트
```

---

## MCP 서버

외부 도구 및 API 접근을 위한 Model Context Protocol 서버.

### 일반적인 MCP 서버

| 서버 | 설명 | 사용 시기 | 범위 | 설치 |
|------|------|---------|------|------|
| **GitHub** | PR 관리, 이슈, 코드 | GitHub 워크플로우 | 프로젝트 | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQL 쿼리, 데이터 접근 | 데이터베이스 작업 | 프로젝트 | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 고급 파일 작업 | 복잡한 파일 작업 | 사용자 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | 팀 커뮤니케이션 | 알림, 업데이트 | 프로젝트 | 설정에서 구성 |
| **Google Docs** | 문서 접근 | 문서 편집, 리뷰 | 프로젝트 | 설정에서 구성 |
| **Asana** | 프로젝트 관리 | 작업 추적 | 프로젝트 | 설정에서 구성 |
| **Stripe** | 결제 데이터 | 재무 분석 | 프로젝트 | 설정에서 구성 |
| **Memory** | 지속적 메모리 | 세션 간 회상 | 사용자 | 설정에서 구성 |
| **Context7** | 라이브러리 문서 | 최신 문서 조회 | 내장 | 내장 |

> **범위**: `프로젝트` = 팀 (`.mcp.json`), `사용자` = 개인 (`~/.claude.json`), `내장` = 사전 설치

### MCP 설정 예제

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**참조**: [05-mcp/](05-mcp/) | [MCP 프로토콜 문서](https://modelcontextprotocol.io)

**빠른 설치 (GitHub MCP)**:
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## 훅

Claude Code 이벤트에 응답하여 셸 명령어를 실행하는 이벤트 기반 자동화.

### 훅 이벤트

| 이벤트 | 설명 | 트리거 시기 | 사용 사례 |
|-------|------|-----------|---------|
| `SessionStart` | 세션 시작/재개 | 세션 초기화 | 설정 작업 |
| `Setup` | 초기 환경 설정 (세션당 한 번) | 첫 세션 부트스트랩 | 도구 프로비저닝, 의존성 설치 |
| `InstructionsLoaded` | 명령어 로드됨 | CLAUDE.md 또는 규칙 파일 로드 | 커스텀 명령어 처리 |
| `UserPromptSubmit` | 프롬프트 처리 전 | 사용자가 메시지 전송 | 입력 검증 |
| `UserPromptExpansion` | 사용자 프롬프트 확장됨 (@멘션, 슬래시 명령어 확인) | 확장 후, 제출 전 | 확장된 프롬프트 변환 또는 검사 |
| `PreToolUse` | 도구 실행 전 | 도구 실행 전 | 검증, 로깅 |
| `PermissionRequest` | 권한 대화상자 표시 | 민감한 작업 전 | 커스텀 승인 흐름 |
| `PermissionDenied` | 사용자가 권한 프롬프트 거부 | 권한 거부 후 | 로깅, 분석, 정책 시행 |
| `PostToolUse` | 도구 성공 후 | 도구 완료 후 | 포맷팅, 알림 |
| `PostToolUseFailure` | 도구 실행 실패 | 도구 오류 후 | 오류 처리, 로깅 |
| `PostToolBatch` | 도구 사용 배치 완료 후 | 도구 배치 종료 | 집계 보고, 배치 검증 |
| `Notification` | 알림 전송 | Claude 알림 전송 | 외부 알림 |
| `SubagentStart` | 서브에이전트 생성됨 | 서브에이전트 작업 시작 | 서브에이전트 컨텍스트 초기화 |
| `SubagentStop` | 서브에이전트 종료 | 서브에이전트 작업 완료 | 작업 연결 |
| `Stop` | Claude 응답 완료 | 응답 완료 | 정리, 보고 |
| `StopFailure` | API 오류로 턴 종료 | API 오류 발생 | 오류 복구, 로깅 |
| `TeammateIdle` | 팀원 에이전트 유휴 상태 | 에이전트 팀 조정 | 작업 분배 |
| `TaskCompleted` | 작업 완료 표시 | 작업 완료 | 사후 작업 처리 |
| `TaskCreated` | TaskCreate를 통해 작업 생성 | 새 작업 생성 | 작업 추적, 로깅 |
| `ConfigChange` | 설정 업데이트됨 | 설정 수정 | 설정 변경 대응 |
| `CwdChanged` | 작업 디렉토리 변경 | 디렉토리 변경됨 | 디렉토리별 설정 |
| `FileChanged` | 감시 중인 파일 변경 | 파일 수정됨 | 파일 모니터링, 재빌드 |
| `PreCompact` | 압축 작업 전 | 컨텍스트 압축 | 상태 보존 |
| `PostCompact` | 압축 완료 후 | 압축 완료 | 압축 후 작업 |
| `WorktreeCreate` | 작업 트리 생성 중 | Git 작업 트리 생성됨 | 작업 트리 환경 설정 |
| `WorktreeRemove` | 작업 트리 제거 중 | Git 작업 트리 제거됨 | 작업 트리 리소스 정리 |
| `Elicitation` | MCP 서버가 입력 요청 | MCP elicitation | 입력 검증 |
| `ElicitationResult` | 사용자가 elicitation에 응답 | 사용자 응답 | 응답 처리 |
| `SessionEnd` | 세션 종료 | 세션 종료 | 정리, 상태 저장 |

### 예제 훅

| 훅 | 설명 | 이벤트 | 범위 | 설치 |
|-----|------|-------|------|------|
| `validate-bash.py` | 명령어 검증 | PreToolUse:Bash | 프로젝트 | `cp 06-hooks/validate-bash.py .claude/hooks/` |
| `security-scan.py` | 보안 스캐닝 | PostToolUse:Write | 프로젝트 | `cp 06-hooks/security-scan.py .claude/hooks/` |
| `format-code.sh` | 자동 포맷팅 | PostToolUse:Write | 사용자 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.py` | 프롬프트 검증 | UserPromptSubmit | 프로젝트 | `cp 06-hooks/validate-prompt.py .claude/hooks/` |
| `context-tracker.py` | 토큰 사용량 추적 | Stop | 사용자 | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `pre-commit.sh` | 커밋 전 검증 | PreToolUse:Bash | 프로젝트 | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | 명령어 로깅 | PostToolUse:Bash | 사용자 | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |
| `dependency-check.sh` | 매니페스트 변경 시 취약점 스캔 | PostToolUse:Write | 프로젝트 | `cp 06-hooks/dependency-check.sh .claude/hooks/` |

> **범위**: `프로젝트` = 팀 (`.claude/settings.json`), `사용자` = 개인 (`~/.claude/settings.json`)

### 훅 설정

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "~/.claude/hooks/validate-bash.py"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "~/.claude/hooks/format-code.sh"
      }
    ]
  }
}
```

**참조**: [06-hooks/](06-hooks/) | [공식 문서](https://code.claude.com/docs/en/hooks)

**빠른 설치 (모든 훅)**:
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## 메모리 파일

세션 간 자동으로 로드되는 지속적 컨텍스트.

### 메모리 유형

| 유형 | 위치 | 범위 | 사용 시기 |
|------|------|------|---------|
| **관리 정책** | 조직 관리 정책 | 조직 | 조직 전체 표준 적용 |
| **프로젝트** | `./CLAUDE.md` | 프로젝트 (팀) | 팀 표준, 프로젝트 컨텍스트 |
| **프로젝트 규칙** | `.claude/rules/` | 프로젝트 (팀) | 모듈형 프로젝트 규칙 |
| **사용자** | `~/.claude/CLAUDE.md` | 사용자 (개인) | 개인 선호도 |
| **사용자 규칙** | `~/.claude/rules/` | 사용자 (개인) | 모듈형 개인 규칙 |
| **로컬** | `./CLAUDE.local.md` | 로컬 (git 제외) | 머신별 로컬 재정의 (git 제외). 지원되는 개발자별 재정의 파일로 https://code.claude.com/docs/en/memory에 문서화됨 |
| **자동 메모리** | 자동 | 세션 | 자동 캡처된 인사이트 및 수정 사항 |

> **범위**: `조직` = 관리자 관리, `프로젝트` = git을 통해 팀 공유, `사용자` = 개인 선호도, `로컬` = 커밋되지 않음, `세션` = 자동 관리

**참조**: [02-memory/](02-memory/) | [공식 문서](https://code.claude.com/docs/en/memory)

**빠른 설치**:
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

---

## 새로운 기능 (2026년 5월)

| 기능 | 설명 | 사용 방법 |
|------|------|---------|
| **/focus** | 방해 없는 출력 표시를 위한 포커스 뷰 전환 (v2.1.110) | `/focus` 실행하여 긴 작업 중 시각적 소음 감소 |
| **/proactive** | `/loop`의 별칭 — 동일한 반복 작업 동작 (v2.1.105) | `/proactive`를 `/loop`와 상호교환적으로 사용 |
| **/recap** | 기존 세션 복귀 시 세션 요약 표시 (v2.1.108) | 자리 비움 후 `/recap` 실행하여 수행된 작업 컨텍스트 확인 |
| **/tui** | 깜빡임 없는 렌더링을 위한 전체화면 TUI(텍스트 사용자 인터페이스) 모드 전환 (v2.1.110) | 전체화면 터미널이나 tmux에서 `/tui` 사용 |
| **/undo** | `/rewind`의 별칭 — 이전 체크포인트로 복귀 (v2.1.108) | `/undo`를 `/rewind`와 상호교환적으로 사용 |
| **Monitor Tool** | 백그라운드 명령어의 stdout 스트림을 감시하고 폴링 대신 이벤트에 반응 (v2.1.98+) | [고급 기능](09-advanced-features/)을 통해 Monitor 도구 사용 |
| **/team-onboarding** | 프로젝트의 Claude Code 설정에서 팀원 온보딩 가이드 자동 생성 (v2.1.101) | 프로젝트에서 `/team-onboarding` 실행 |
| **Ultraplan 자동 생성** | 첫 `/ultraplan` 호출 시 클라우드 환경 자동 생성 — 수동 설정 불필요 (v2.1.101) | `/ultraplan <프롬프트>` 사용 |
| **원격 제어** | API를 통해 Claude Code 세션 원격 제어 | 원격 제어 API를 사용하여 프로그래밍 방식으로 프롬프트 전송 및 응답 수신 |
| **웹 세션** | 브라우저 기반 환경에서 Claude Code 실행 | `claude web` 또는 Anthropic Console을 통해 접근 |
| **데스크탑 앱** | Claude Code용 네이티브 데스크탑 애플리케이션 | `/desktop` 사용 또는 Anthropic 웹사이트에서 다운로드 |
| **에이전트 팀** | 관련 작업에서 여러 에이전트 조정 | 컨텍스트를 공유하고 협업하는 팀원 에이전트 구성 |
| **작업 목록** | 백그라운드 작업 관리 및 모니터링 | `/tasks`를 사용하여 백그라운드 작업 보기 및 관리 |
| **프롬프트 제안** | 컨텍스트 인식 명령어 제안 | 현재 컨텍스트 기반으로 제안이 자동 표시됨 |
| **Git Worktrees** | 병렬 개발을 위한 격리된 git 작업 트리 | 안전한 병렬 브랜치 작업을 위해 작업 트리 명령어 사용 |
| **샌드박싱** | 안전을 위한 격리된 실행 환경 | `/sandbox`로 전환; 제한된 환경에서 명령어 실행 |
| **MCP OAuth** | MCP 서버용 OAuth 인증 | 안전한 접근을 위해 MCP 서버 설정에서 OAuth 자격 증명 구성 |
| **MCP 도구 검색** | 동적으로 MCP 도구 검색 및 발견 | 연결된 서버에서 사용 가능한 MCP 도구를 찾기 위해 도구 검색 사용 |
| **예약된 태스크** | `/loop` 및 cron 도구로 반복 작업 설정 | `/loop 5m /command` 또는 CronCreate 도구 사용 |
| **Chrome 통합** | 헤드리스 Chromium을 사용한 브라우저 자동화 | `--chrome` 플래그 또는 `/chrome` 명령어 사용 |
| **키보드 사용자 정의** | 코드 지원을 포함한 키바인딩 사용자 정의 | `/keybindings` 사용 또는 `~/.claude/keybindings.json` 편집 |
| **오토 모드** | 권한 프롬프트 없이 완전 자율 운영 (Research Preview) | `--mode auto` 또는 `/permissions auto` 사용; 2026년 3월 |
| **채널** | 다중 채널 통신 (Telegram, Slack 등) (Research Preview) | 채널 플러그인 구성; 2026년 3월 |
| **음성 받아쓰기** | 프롬프트를 위한 음성 입력 | 마이크 아이콘 또는 음성 키바인딩 사용 |
| **에이전트 훅 유형** | 셸 명령어 대신 서브에이전트를 생성하는 훅 | 훅 설정에서 `"type": "agent"` 설정 |
| **프롬프트 훅 유형** | 대화에 프롬프트 텍스트를 주입하는 훅 | 훅 설정에서 `"type": "prompt"` 설정 |
| **MCP Elicitation** | MCP 서버가 도구 실행 중 사용자 입력 요청 가능 | `Elicitation` 및 `ElicitationResult` 훅 이벤트를 통해 처리 |
| **플러그인 LSP 지원** | 플러그인을 통한 언어 서버 프로토콜 통합 | 에디터 기능을 위해 `plugin.json`에서 LSP 서버 구성 |
| **관리 드롭인** | 조직 관리 드롭인 설정 (v2.1.83) | 관리 정책을 통해 관리자 구성; 모든 사용자에게 자동 적용 |
| **`claude plugin init`** | `.claude/skills`에 새 플러그인 스캐폴딩; 마켓플레이스 없이도 플러그인이 자동 로드됨 (v2.1.157) | `claude plugin init <name>` 실행 |
| **Bedrock/Vertex/Foundry의 오토 모드** | Opus 4.7/4.8용 타사 제공업체에서 오토 모드 사용 가능 — 옵트인 (v2.1.158) | `CLAUDE_CODE_ENABLE_AUTO_MODE=1` 설정 |

---

## 빠른 참조 매트릭스

### 기능 선택 가이드

| 필요 사항 | 권장 기능 | 이유 |
|----------|-----------|------|
| 빠른 단축키 | 슬래시 명령어 | 수동, 즉시 |
| 지속적 컨텍스트 | 메모리 | 자동 로드 |
| 복잡한 자동화 | 스킬 | 자동 호출 |
| 전문화된 작업 | 서브에이전트 | 격리된 컨텍스트 |
| 외부 데이터 | MCP 서버 | 실시간 접근 |
| 이벤트 자동화 | 훅 | 이벤트 트리거 |
| 완전한 솔루션 | 플러그인 | 올인원 번들 |

### 설치 우선순위

| 우선순위 | 기능 | 명령어 |
|----------|------|--------|
| 1. 필수 | 메모리 | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 일상 사용 | 슬래시 명령어 | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 품질 | 서브에이전트 | `cp 04-subagents/*.md .claude/agents/` |
| 4. 자동화 | 훅 | `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5. 외부 | MCP | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 고급 | 스킬 | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 완전 | 플러그인 | `/plugin install pr-review` |

---

## 전체 원클릭 설치

이 저장소의 모든 예제를 설치합니다:

```bash
# 디렉토리 생성
mkdir -p .claude/{commands,agents,skills} ~/.claude/{hooks,skills}

# 모든 기능 설치
cp 01-slash-commands/*.md .claude/commands/ && \
cp 02-memory/project-CLAUDE.md ./CLAUDE.md && \
cp -r 03-skills/* ~/.claude/skills/ && \
cp 04-subagents/*.md .claude/agents/ && \
cp 06-hooks/*.sh ~/.claude/hooks/ && \
chmod +x ~/.claude/hooks/*.sh
```

---

## 추가 자료

- [공식 Claude Code 문서](https://code.claude.com/docs/en/overview)
- [MCP 프로토콜 명세](https://modelcontextprotocol.io)
- [학습 로드맵](LEARNING-ROADMAP.md)
- [메인 README](README.md)

---

**최종 업데이트**: 2026년 6월 15일
**Claude Code 버전**: 2.1.176
**출처**:
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/changelog#2-1-172
- https://code.claude.com/docs/en/changelog#2-1-174
- https://github.com/anthropics/claude-code/releases/tag/v2.1.145
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
- https://code.claude.com/docs/en/plugins
- https://code.claude.com/docs/en/cli-reference
**호환 모델**: Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
