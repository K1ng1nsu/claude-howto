<!-- i18n-source: CHANGELOG.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->
# 변경 로그

## [v2.1.160] — 2026-06-02

### Claude Code v2.1.160에 동기화됨

튜토리얼 범위를 Claude Code v2.1.160 릴리스로 업데이트합니다. 이전 v2.1.156 동기화(Claude Opus 4.8, #129)는 문서에 적용되었지만 별도로 변경 로그에 기록되지 않았습니다. 이 항목은 그 이후를 다루며 v2.1.157–v2.1.160 범위의 변경사항을 포함합니다. 이 범위에서 호환성을 깨는 변경사항은 없었습니다. 추가된 내용은 몇 가지 새로운 CLI/기능 표면과 일상적인 푸터 업데이트입니다. 타사 제공업체의 자동 모드는 **옵트인**이며, 새로운 기본값이 아닙니다.

### 추가됨

- **`claude plugin init <name>` (v2.1.157)** — 플러그인을 `.claude/skills`에 직접 스캐폴딩합니다. 해당 위치에 배치된 플러그인은 마켓플레이스 없이 자동 로드됩니다. `10-cli/README.md`, `07-plugins/README.md`, `CATALOG.md`에 문서화됨.
- **Bedrock / Vertex / Foundry 자동 모드 (v2.1.158)** — 이제 Opus 4.7/4.8용 세 타사 제공업체에서 자동 모드를 사용할 수 있으며, `CLAUDE_CODE_ENABLE_AUTO_MODE=1` 환경 변수를 통해 **옵트인** 방식으로 제공됩니다. `09-advanced-features/README.md`, `10-cli/README.md`, `CATALOG.md`에 문서화됨.
- **`EnterWorktree` 세션 내 전환 (v2.1.157)** — `EnterWorktree` 도구가 이제 세션 내에서 Claude가 관리하는 워크트리 간 전환을 지원하며, 완료된 워크트리는 잠금 해제 상태로 남아 `git worktree remove`/`prune`이 정리할 수 있습니다. `09-advanced-features/README.md`에 문서화됨.

### 동작 변경사항

- **`acceptEdits` 쓰기-안전 프롬프트 (v2.1.160)** — `acceptEdits` 모드에서도 Claude Code는 이제 셸 시작 파일(`.zshenv`, `.zlogin`, `.bash_login`, `~/.config/git/`) 및 코드 실행 빌드 설정(`.npmrc`, `.yarnrc*`, `bunfig.toml`, `.bazelrc`, `.pre-commit-config.yaml`, `.devcontainer/`)에 쓰기 전에 프롬프트를 표시합니다. 이는 의도하지 않은 명령 실행을 방지하기 위함입니다. `09-advanced-features/README.md`에 문서화됨.
- **동적 워크플로우 트리거 키워드 `workflow` → `ultracode` (v2.1.160)** — "workflow" 단일 단어는 더 이상 동적 워크플로우 실행을 트리거하지 않습니다. 트리거 키워드는 이제 `ultracode`입니다. `09-advanced-features/README.md`에 명시됨.

### 제거됨

- **`CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE`는 이제 무효 (v2.1.160)** — 환경 변수가 제거되어 더 이상 효과가 없습니다. `10-cli/README.md`의 환경 변수 테이블 문구가 "removed 2026-06-01"에서 "제거됨 (v2.1.160부터 무효)"로 업데이트되었습니다.

### 문서화

- `README.md`의 내부적으로 일관성 없는 세 개의 버전 문자열 수정 (배지 및 FAQ 문구가 `2.1.145` / `v2.1.150`에 고정되어 있음), 오래된 Sources 링크 정규화.
- 모든 영어 문서의 메타데이터 푸터를 일관된 동기화를 위해 **v2.1.160 / 2026년 6월 2일**로 업데이트.

## [v2.1.150] — 2026-05-25

### Claude Code v2.1.150에 동기화됨

튜토리얼 범위를 Claude Code v2.1.145 → v2.1.150로 업데이트 (2026년 5월 23일 릴리스). Anthropic은 마지막 동기화 이후 5개의 패치(v2.1.146 ~ v2.1.150)를 출시했습니다. 주요 변경사항은 **번들 `/simplify` 스킬의 `/code-review`로의 이름 변경** (v2.1.146)입니다. 순수한 이름 변경이며 **별칭이 없어** 이전 이름은 더 이상 작동하지 않습니다. 이 저장소도 자체 로컬 코드 리뷰 스킬을 제공하므로, 디렉터리 이름이 `code-review-specialist`로 변경되어 새로운 내장 스킬과 충돌을 피했습니다. 기타 주요 사항: `/usage`가 이제 카테고리별 비용을 세분화, 백그라운드 세션을 `Ctrl+T`로 고정 가능, 마크다운 렌더러가 GFM 작업 목록 체크박스 지원, 새로운 `allowAllClaudeAiMcps` 관리 설정 추가. 이 동기화는 v2.1.143에 고정되어 있던 4개의 모듈 README(`04-subagents`, `05-mcp`, `07-plugins`, `09-advanced-features`)도 업데이트합니다.

### 동작 변경사항

- **`/simplify` → `/code-review`로 이름 변경 (v2.1.146)**: 번들 리뷰 스킬이 이제 `/code-review`로 호출되며 선택적 노력 수준(예: `/code-review high`)을 받습니다. `--comment`를 전달하면 결과를 인라인 GitHub PR 코멘트로 게시합니다 (v2.1.147). 이전 `/simplify` 이름은 더 이상 작동하지 않습니다. 별칭이 없습니다. `01-slash-commands/README.md`, `03-skills/README.md`, `CATALOG.md`, `QUICK_REFERENCE.md`, `claude_concepts_guide.md`에서 업데이트됨.

### 변경됨

- **저장소의 로컬 `code-review` 스킬을 `code-review-specialist`로 이름 변경** — 새로운 내장 `/code-review`와의 충돌을 방지하기 위함. 디렉터리 `03-skills/code-review/` → `03-skills/code-review-specialist/`, 모든 설치 명령어, 디렉터리 트리, 상호 참조가 `README.md`, `QUICK_REFERENCE.md`, `INDEX.md`, `CATALOG.md`, `LEARNING-ROADMAP.md`, `claude_concepts_guide.md`, `03-skills/README.md`에서 업데이트됨. 충돌 설명과 내장 스킬 가리기를 피하는 방법에 대한 노트 추가.

### 추가됨

- **`/usage` 카테고리별 비용 세분화 (v2.1.149)** — 비용 보기가 이제 카테고리(스킬, 서브에이전트, 플러그인, MCP 서버별 비용)별로 지출을 세분화하여 표시합니다. `CATALOG.md` 및 `claude_concepts_guide.md`에 문서화됨.
- **고정된 백그라운드 세션 — `Ctrl+T` (v2.1.147)** — `claude agents`에서 세션을 고정하면 유휴 상태에서도 활성 상태를 유지하고, Claude Code 업데이트를 적용하기 위해 제자리에서 재시작하며, 메모리 압력 시 고정되지 않은 세션 이후에만 해제됩니다. `10-cli/README.md`에 문서화됨.
- **GFM 작업 목록 체크박스 렌더링 (v2.1.149)** — 마크다운 렌더러가 이제 `- [ ]` / `- [x]`를 체크박스로 렌더링합니다. `09-advanced-features/README.md`에 문서화됨.
- **`allowAllClaudeAiMcps` 관리 설정 (v2.1.149)** — 조직 전체에서 claude.ai 클라우드 MCP 커넥터 로드를 허용합니다. `05-mcp/README.md`에 문서화됨.

### 제거됨

- **Stop/SubagentStop 훅 입력 필드 `background_tasks` 및 `session_crons`** — `06-hooks/README.md` 및 `resources.md`에서 제거됨. v2.1.145 릴리스 노트에서 추가되었으나 공식 훅 참조 페이지에 열거되지 않았음; 게시된 참조와 문서를 일치시키기 위해 제거.

### 문서화

- 4개 모듈 README를 v2.1.143에서 v2.1.150으로 업데이트: `04-subagents/README.md`, `05-mcp/README.md`, `07-plugins/README.md`, `09-advanced-features/README.md`.
- 모든 영어 문서의 메타데이터 푸터를 일관된 동기화를 위해 **v2.1.150 / 2026년 5월 25일**로 업데이트.

## [v2.1.145] — 2026-05-20

### Claude Code v2.1.145에 동기화됨

튜토리얼 범위를 Claude Code v2.1.143 → v2.1.145로 업데이트 (2026년 5월 19일 릴리스). Anthropic은 마지막 동기화 이후 2개의 패치(v2.1.144 및 v2.1.145)를 출시했습니다. 주요 사항: `/extra-usage`의 `/usage-credits`로의 이름 변경, `/model`이 기본적으로 세션 전용으로 변경, 3개의 새로운 번들 스킬(`/run`, `/verify`, `/run-skill-generator`), Stop/SubagentStop 훅 입력 필드 `background_tasks` 및 `session_crons`, 스크립팅용 `claude agents --json`, 순수 환경 변수 Bash 자동 승인 허점을 막는 보안 수정. 이 동기화는 v2.1.143 동기화에서 누락되어 v2.1.138에 고정되어 있던 6개의 루트 수준 참조 문서(`LEARNING-ROADMAP.md`, `QUICK_REFERENCE.md`, `INDEX.md`, `resources.md`, `claude_concepts_guide.md`, `STYLE_GUIDE.md`)도 업데이트합니다.

### 추가됨

- `/usage-credits` 슬래시 명령어 (v2.1.144) — `/extra-usage`를 대체하는 기본 이름; `/extra-usage`는 별칭으로 계속 작동함. `01-slash-commands/README.md` 및 `CATALOG.md`에 문서화됨.
- 3개의 새로운 번들 스킬 (v2.1.145) — `/run` (변경사항을 확인하기 위해 프로젝트 앱 실행), `/verify` (앱을 빌드, 실행, 관찰하여 수정이 작동하는지 확인), `/run-skill-generator` (프로젝트별 스킬을 생성하여 `/run`/`/verify`에 특정 프로젝트 처리 방법을 가르침). `03-skills/README.md`, `CATALOG.md`, `QUICK_REFERENCE.md`에 문서화됨. 표준 번들 스킬 수를 **9**개로 증가.
- Stop/SubagentStop 훅 입력 필드 `background_tasks` 및 `session_crons` (v2.1.145) — 훅 작성자는 이를 읽어 백그라운드 작업이나 예약된 작업이 보류 중인 동안 중지를 차단할지 결정할 수 있음. `06-hooks/README.md`에 문서화됨.
- `claude agents --json` (v2.1.145) — 에이전트 목록을 머신이 읽을 수 있는 JSON으로 출력 (상태 표시줄, 세션 선택기, tmux-resurrect 등). `10-cli/README.md`에 문서화됨.
- 요약 테이블에서 누락된 5개의 훅 이벤트 행 — `Setup`, `UserPromptExpansion`, `PermissionDenied`, `PostToolBatch` (설명에는 이미 "29개 이벤트"라고 되어 있었으나, `CATALOG.md`, `claude_concepts_guide.md`, `INDEX.md`의 요약 테이블에는 25개만 나열되어 있었음).

### 동작 변경사항

- **`/model`이 기본적으로 세션 전용 (v2.1.144)**: 모델 선택이 이제 현재 세션에만 적용됩니다. 선택 후 `d`를 누르면 해당 선택을 향후 세션의 새 기본값으로 설정할 수 있습니다. `01-slash-commands/README.md`에 문서화됨.
- **Bash 순수 환경 변수 자동 승인 차단 (v2.1.145 보안 수정)**: `FOO=bar somecommand` 형식의 명령어는 허용 목록에 `FOO=bar`만 있을 경우 더 이상 자동 승인되지 않습니다. 전체 명령어를 포함하는 `Bash(...)` 권한 규칙을 통해 명시적으로 다시 허용해야 합니다. `06-hooks/README.md`에 문서화됨.
- **`context: fork` 무한 루프 수정 (v2.1.145)**: `context: fork`를 사용하는 스킬이 드물게 무한 재호출 루프를 트리거할 수 있었습니다. `03-skills/README.md`에 노트로 문서화됨.

### 문서화

- 6개 루트 수준 참조 문서를 v2.1.138에서 v2.1.145로 업데이트: `LEARNING-ROADMAP.md`, `QUICK_REFERENCE.md`, `INDEX.md`, `resources.md`, `claude_concepts_guide.md`, `STYLE_GUIDE.md`.
- 번들 스킬 불일치 수정 — `CATALOG.md`, `QUICK_REFERENCE.md`, `03-skills/README.md`에 이전에 각각 다른 3개의 5개 항목 목록이 있었음; 표준 9개로 통일 (`/batch`, `/claude-api`, `/debug`, `/fewer-permission-prompts`, `/loop`, `/run`, `/run-skill-generator`, `/simplify`, `/verify`). `QUICK_REFERENCE.md` 셀에도 `/voice`와 `/browse`가 번들 스킬로 잘못 나열되어 있었음 — 둘 다 번들 스킬이 아님.
- "새로운 기능 (2026년 3월)" → "새로운 기능 (2026년 5월)"로 `QUICK_REFERENCE.md` 및 `resources.md`에서 이름 변경 (저장소의 나머지 부분과 일치).
- `README.md`의 버전 배지를 `2.1.138`에서 `2.1.145`로 업데이트하고 본문의 두 "최신: v2.1.138" 문구 업데이트.
- STYLE_GUIDE의 샘플 메타데이터 푸터를 `2.1.97`에서 `2.1.145`로 업데이트하여 기여자가 현재 버전을 복사하도록 함.

## [v2.1.143] — 2026-05-19

### Claude Code v2.1.143에 동기화됨

튜토리얼 범위를 Claude Code v2.1.138 → v2.1.143로 업데이트 (2026년 5월 15일 릴리스). Anthropic은 마지막 동기화 이후 5개의 패치(v2.1.139–v2.1.143)를 출시했습니다. 주요 사항: `/goal` 및 `/scroll-speed` 슬래시 명령어, 전체 디스패치 플래그 세트를 갖춘 `claude agents` 에이전트 뷰 (연구 프리뷰), Stop 훅 안전 캡, 훅 실행 형식(`args`), PostToolUse용 `continueOnBlock`, 훅 `terminalSequence` 출력, Fast Mode 기본값 Opus 4.7, Windows에서 Bedrock/Vertex/Foundry용 PowerShell 기본 설정, `worktree.bgIsolation` 설정.

### 추가됨

- `/goal <statement>` 슬래시 명령어 (v2.1.139) — 경과 시간, 턴 수, 토큰 사용량을 표시하는 라이브 오버레이 패널과 함께 세션 수준 완료 조건을 등록합니다. `01-slash-commands/README.md`에 문서화되고 `10-cli/README.md`에서 교차 링크됨.
- `/scroll-speed <±N>` 슬래시 명령어 (v2.1.139) — TUI 라이브 프리뷰 스크롤 속도를 조정합니다. 머신별로 지속됩니다. `01-slash-commands/README.md`에 문서화됨.
- `claude agents` 에이전트 뷰 (연구 프리뷰, v2.1.139) — 디스패치 플래그 `--cwd` (v2.1.141), `--add-dir`, `--settings`, `--mcp-config`, `--plugin-dir`, `--permission-mode`, `--model`, `--effort`, `--dangerously-skip-permissions` (v2.1.142) 포함. `10-cli/README.md`에 문서화됨.
- `claude plugin details <name>` (v2.1.139) — 전체 플러그인 인벤토리와 예상 턴당/호출당 토큰 비용 추정. v2.1.142에서 LSP 서버가 세부 정보 창에 추가됨. `07-plugins/README.md`에 문서화됨.
- `/plugin` 브라우즈 창의 마켓플레이스 컨텍스트 비용 예측 (v2.1.143). `07-plugins/README.md`에 문서화됨.
- 훅 **실행 형식** (`args: string[]`, v2.1.139) — 셸 구문 분석 없이 직접 `execve()` 실행; 셸 형식 `command` 필드와 상호 배타적. `06-hooks/README.md`에 문서화됨.
- PostToolUse의 훅 `continueOnBlock: true` 필드 (v2.1.139) — 차단된 도구 결과를 턴을 중단하는 대신 `tool_result`로 Claude에 다시 전달합니다. `06-hooks/README.md`에 문서화됨.
- 훅 `terminalSequence` JSON 출력 필드 (v2.1.141) — 데스크톱 알림, 창 제목, 벨을 위한 원시 OSC 이스케이프 시퀀스를 출력합니다. `06-hooks/README.md`에 문서화됨.
- `worktree.bgIsolation: "none"` 설정 (v2.1.143) — 백그라운드 세션이 격리된 워크트리 대신 현재 작업 복사본을 직접 편집합니다. `09-advanced-features/README.md`에 문서화됨.
- `CLAUDE_PROJECT_DIR`이 이제 모든 MCP stdio 서버의 환경에 전달되며 (v2.1.139), `${CLAUDE_PROJECT_DIR}` 치환은 플러그인 및 프로젝트 `.mcp.json`의 `command`/`args`/`env` 필드에서 지원됩니다. `05-mcp/README.md`에 문서화됨.
- 서브에이전트 OTEL 헤더 `x-claude-code-agent-id` 및 `x-claude-code-parent-agent-id` (v2.1.139), `claude_code.llm_request` OTEL 스팬의 `agent_id` / `parent_agent_id` 속성으로 노출됨. `04-subagents/README.md`에 문서화됨.
- `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1` (v2.1.142) — v2.1.142 기본값이 Opus 4.7로 변경된 후 Fast Mode를 Opus 4.6으로 고정합니다. `10-cli/README.md`에 문서화됨.
- `CLAUDE_CODE_USE_POWERSHELL_TOOL=0` 및 `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1` (v2.1.143) — 기본으로 활성화된 PowerShell 도구를 선택 해제하거나, `-ExecutionPolicy Bypass` 대신 시스템 실행 정책을 따르도록 설정. `09-advanced-features/README.md`에 문서화됨.
- `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP` (v2.1.143) — Stop 훅의 8연속 차단 안전 캡을 재정의합니다 (`0` 설정으로 비활성화). `06-hooks/README.md` 및 `09-advanced-features/README.md`에 문서화됨.
- `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` (v2.1.141) — SSH 키가 없는 CI 실행기를 위해 GitHub 플러그인 소스를 HTTPS를 통해 클론하도록 강제합니다. `07-plugins/README.md`에 문서화됨.
- `ANTHROPIC_WORKSPACE_ID` (v2.1.141) — 페더레이션 워크로드 ID 토큰을 특정 워크스페이스로 범위를 제한합니다. `09-advanced-features/README.md`에 문서화됨.
- 루트 수준 `SKILL.md` 플러그인 패턴 (v2.1.142) — 최상위 `SKILL.md`만 있는 플러그인(`skills/` 하위 디렉터리 없음)은 단일 스킬로 표시됩니다. `07-plugins/README.md`에 문서화됨.
- `/schedule`용 플러그인 마케팅 이름 **루틴** (Anthropic 블로그, 2026-05-14) — `09-advanced-features/README.md`에 한 줄 노트로 표시됨; CLI 표면은 `/schedule`로 유지.

### 동작 변경사항

- **Fast Mode 기본값 Opus 4.7로 변경 (v2.1.142)**: `/fast`가 이제 기본적으로 Opus 4.7에서 실행됩니다 (이전은 Opus 4.6). 다시 옵트인하려면 `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1`을 설정하세요.
- **Windows에서 Bedrock/Vertex/Foundry용 PowerShell 도구 기본 활성화 (v2.1.143)**: Claude Code가 `-ExecutionPolicy Bypass`로 PowerShell을 호출합니다. `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1` (시스템 정책 준수) 또는 `CLAUDE_CODE_USE_POWERSHELL_TOOL=0` (도구 비활성화)으로 선택 해제 가능.
- **API 키 인증 설정 시 원격 제어, `/schedule`, claude.ai MCP 커넥터, 알림 환경설정 자동 비활성화 (v2.1.139)**: `ANTHROPIC_API_KEY`, `ANTHROPIC_AUTH_TOKEN` 또는 `apiKeyHelper`를 설정하면 claude.ai 로그인이 활성화되어 있어도 4가지 claude.ai 브리지 표면이 모두 비활성화됩니다.
- **Stop 훅 블록 루프를 8연속 블록으로 제한 (v2.1.143)**: 8회 연속 후 세션이 경고와 함께 종료되어, 버그가 있는 Stop 훅이 세션을 영원히 루프시키는 것을 방지합니다. `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP`으로 재정의 가능.
- **`subagent_type` 일치가 이제 대소문자 및 구분자 구분 없음 (v2.1.140)**: `code-reviewer`, `Code Reviewer`, `code_reviewer` 모두 동일한 에이전트로 확인됩니다. `04-subagents/README.md`에 문서화됨.

### 변경됨

- 루트 참조 문서(`README.md`, `CATALOG.md`)가 `28개 훅 이벤트`에서 `29개 훅 이벤트`로 업데이트됨 — v2.1.138에서 `Setup` 훅이 추가된 후 `06-hooks/README.md` 및 `LEARNING-ROADMAP.md`와 일치.

### 번역 유지 관리자 참고사항

- 튜토리얼 번역(`vi/`, `ja/`, `uk/`, `zh/`)은 영어를 따릅니다. 이번 라운드의 변경사항을 모듈 README 및 위 CHANGELOG에 동기화하세요. 푸터는 `마지막 업데이트: 2026년 5월 19일` 및 `Claude Code 버전: 2.1.143`을 반영해야 합니다.

## [v2.1.138] — 2026-05-09

### Claude Code v2.1.138에 동기화됨

튜토리얼 범위를 Claude Code v2.1.131 → v2.1.138로 업데이트 (2026년 5월 9일 릴리스). Anthropic은 마지막 동기화 이후 v2.1.132에서 v2.1.138 사이에 7개의 패치를 출시했습니다.

### 추가됨 (영문 문서)

- `worktree.baseRef` 설정 (v2.1.133) — `claude --worktree`가 `origin/<default>`(`"fresh"`, 기본값) 또는 로컬 `HEAD`(`"head"`)에서 브랜치를 생성할지 제어합니다. **동작 변경**: `"fresh"` 기본값은 v2.1.128 동작을 되돌리므로, v2.1.128 이후 로컬 `HEAD` 브랜칭에 의존하던 사용자는 다시 옵트인해야 합니다. `09-advanced-features/README.md`에 문서화됨.
- `autoMode.hard_deny` 관리 키 (v2.1.136) — 추론된 사용자 의도와 관계없이 특정 작업 클래스를 차단하는 분류기 규칙 배열입니다. 자동 모드에서 절대 실행되어서는 안 되는 작업(예: `rm -rf /`, 보호된 브랜치에 강제 푸시)에 사용합니다. `soft_deny`와 달리 hard-deny 규칙은 분류기가 협상할 수 없습니다. `09-advanced-features/README.md`에 문서화됨.
- `parentSettingsBehavior` 관리 키 (v2.1.133+, 관리자 계층) — SDK의 `managedSettings`가 상위 프로세스 설정과 병합되는 방식을 제어합니다. `"first-wins"`는 기존 우선순위를 유지하고, `"merge"`는 값을 깊게 병합합니다. `09-advanced-features/README.md`에 문서화됨.
- `Setup` 훅 이벤트 — 초기 환경 설정 (세션당 한 번); 도구 프로비저닝 또는 종속성 설치에 사용. 문서화된 훅 이벤트 수를 28개에서 29개로 증가. `06-hooks/README.md`에 문서화됨.
- 훅 입력 JSON의 `effort.level` 필드 (v2.1.133) — 활성 노력 수준(`low`/`medium`/`high`/`xhigh`/`max`)을 훅에 노출. `06-hooks/README.md`에 문서화됨.
- Bash 하위 프로세스의 `CLAUDE_CODE_SESSION_ID` 환경 변수 (v2.1.132) — 훅 입력 JSON의 `session_id` 필드와 일치하는 세션 UUID로, bash 로그를 훅 텔레메트리와 연결하는 데 사용. `06-hooks/README.md`에 문서화됨.
- Bash 하위 프로세스의 `CLAUDE_EFFORT` 환경 변수 (v2.1.133) — 훅 입력 JSON의 `effort.level`과 일치하는 활성 노력 수준. `06-hooks/README.md`에 문서화됨.
- `sandbox.bwrapPath` 및 `sandbox.socatPath` 설정 (v2.1.133+, Linux/WSL) — `bubblewrap` 및 `socat`의 비표준 설치 위치를 Claude Code에 지정합니다. 기본값은 `$PATH` 조회. `09-advanced-features/README.md`에 문서화됨.
- `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` 환경 변수 (v2.1.132). `09-advanced-features/README.md`에 문서화됨.
- `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` 환경 변수 (v2.1.136) — OpenTelemetry 데이터를 캡처하는 조직을 위해 세션 품질 설문을 다시 활성화합니다. OTEL 배포에서는 기본적으로 꺼져 있음. `09-advanced-features/README.md`에 문서화됨.

### 변경됨

- **동작 변경**: 계획 모드는 이제 조건 없이 모든 파일 쓰기를 차단합니다 (v2.1.136). 여기에는 `permissions.allow`에 일치하는 `Edit(...)` 규칙이 있는 경우도 포함됩니다. 이전에는 허용적인 `Edit(...)` 규칙이 계획 모드에서 쓰기를 통과시킬 수 있었지만, 그 우회로가 차단되었습니다. 이전 동작에 의존하던 워크플로우는 편집 전에 계획 모드를 종료(`Shift+Tab`)해야 합니다. `09-advanced-features/README.md`에 문서화됨.
- 플러그인 공백 포함 슬래시 명령어 (예: `/myplugin review`)가 이제 `/myplugin:review`로 확인됩니다. 플러그인 `skills` 설정 항목이 더 이상 기본 `skills/` 디렉터리를 숨기지 않습니다 — 둘 다 병합됩니다. `07-plugins/README.md`에 문서화됨.
- MCP 서버가 이제 `/clear` 이후에도 유지됩니다 (v2.1.132+). `05-mcp/README.md`에 문서화됨.
- 서브에이전트가 Skill 도구를 통해 프로젝트, 사용자 및 플러그인 스킬을 발견합니다 (v2.1.133). `04-subagents/README.md`에 문서화됨.
- `--permission-mode`가 이제 계획 모드 세션을 재개할 때 적용됩니다 (v2.1.132). `09-advanced-features/README.md`에 문서화됨.
- `CronList` 출력에 이제 한정자와 예약된 프롬프트 본문이 포함됩니다 (v2.1.136). 각 크론이 무엇을 실행할지 열지 않고도 감사할 수 있습니다. `09-advanced-features/README.md`에 문서화됨.

### 수정됨

- OAuth 리프레시 토큰 동시 리프레시 경쟁 조건.
- INDEX.md 개수 불일치: 스킬 28 → 16, 플러그인 40 → 27, 훅 스크립트 8 → 9 (마크다운 콘텐츠 트리에서 다시 계산). 새 총계는 빌드 아티팩트 및 설정이 아닌 튜토리얼 콘텐츠로 범위를 제한하는 `.md` 전용 방법론을 반영합니다.
- `CATALOG.md`(v2.1.118 → v2.1.138) 및 `claude_concepts_guide.md`(v2.1.117 → v2.1.138)의 오래된 소스 URL. 개념 가이드에서 중복된 레거시 푸터 제거.

### 번역 유지 관리자 참고사항

`vi/`, `zh/`, `uk/`, `ja/` 지역화 트리는 커뮤니티에서 유지 관리되며 영어 소스보다 뒤쳐질 수 있습니다. 번역을 동기화하는 기여자는 이 릴리스에서 업데이트된 영어 파일을 비교(diff)해야 합니다.

## [v2.1.131] — 2026-05-06

### Claude Code v2.1.131에 동기화됨

튜토리얼 범위를 Claude Code v2.1.126 → v2.1.131로 업데이트 (2026년 5월 6일 릴리스). Anthropic은 마지막 동기화 이후 v2.1.128, v2.1.129, v2.1.131을 출시했습니다. v2.1.127과 v2.1.130은 건너뛰어 공개 출시되지 않았습니다.

### 추가됨 (영문 문서)

- `--plugin-url <url>` 플래그 (v2.1.129) — URL에서 플러그인 `.zip` 아카이브를 가져와 현재 세션에 적용합니다. 반복 가능. `07-plugins/README.md`에 문서화됨.
- `CLAUDE_CODE_FORCE_SYNC_OUTPUT` 환경 변수 (v2.1.129) — 자동 감지가 실패하는 터미널(예: Emacs `eat`)에서 동기 출력을 강제합니다. `10-cli/README.md` 및 `09-advanced-features/README.md`에 문서화됨.
- `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` 환경 변수 (v2.1.129) — Homebrew/WinGet 설치(일반적으로 자동 업데이트되지 않음)의 백그라운드 업그레이드를 활성화합니다. `10-cli/README.md` 및 `09-advanced-features/README.md`에 문서화됨.
- `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` 환경 변수 (v2.1.129) — `/v1/models` 게이트웨이 디스커버리에 옵트인하는 데 필요 (변경사항 참조). `10-cli/README.md`에 문서화됨.
- `disableRemoteControl` 설정 (v2.1.128) — 관리자가 관리/정책 범위를 통해 `claude remote-control` 및 `/remote-control`을 차단할 수 있음. `09-advanced-features/README.md`에 문서화됨.
- `--plugin-dir`이 `.zip` 아카이브를 허용 (v2.1.128) — 디렉터리 입력과 함께. `07-plugins/README.md`에 문서화됨.
- `skillOverrides`가 `"name-only"` 및 `"user-invocable-only"`를 허용 (v2.1.129) — 이전의 `"on"`/`"off"`에 추가. `03-skills/README.md`에 문서화됨.

### 변경됨

- **동작 변경**: 게이트웨이 `/v1/models` 디스커버리가 이제 **옵트인** (v2.1.129). 이전(v2.1.126)에는 `ANTHROPIC_BASE_URL`을 설정하면 게이트웨이의 `/v1/models` 엔드포인트에서 `/model`이 자동으로 채워졌습니다. v2.1.129부터 사용자는 추가로 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`을 설정해야 합니다. 환경 변수가 없으면 `/model`은 내장 정적 목록으로 대체됩니다. `10-cli/README.md`에 문서화됨.
- `/mcp`가 서버별 도구 수를 표시하고 0개 도구를 보고하는 서버를 시각적으로 표시 (v2.1.128). `05-mcp/README.md`에 문서화됨.
- 단독 `/color`(인수 없음)는 무작위 세션 색상을 선택 (v2.1.128). 명시적 `/color <name|hex>`는 계속 특정 색상을 설정합니다. `01-slash-commands/README.md`에 문서화됨.
- `--channels` 플래그가 이제 API 키(콘솔) 인증과 작동 (v2.1.128). 이전 릴리스에서는 Pro/Max OAuth가 필요했습니다. `09-advanced-features/README.md`에 문서화됨.
- Ctrl+R 기록 선택기가 기본적으로 **모든 프로젝트의 모든 프롬프트**를 표시 (v2.1.129). 선택기 내에서 Ctrl+S를 누르면 현재 프로젝트로 범위를 좁힐 수 있습니다. `09-advanced-features/README.md`에 문서화됨.
- `/context`가 더 이상 대화에 ASCII 시각화를 덤프하지 않음 (v2.1.129). 시각화는 UI 내에서만 표시됩니다. 호출당 ~1.6k 토큰 비용이 더 이상 발생하지 않음. `09-advanced-features/README.md`에 문서화됨.
- 드래그 앤 드롭의 과도하게 큰 이미지가 자동으로 다운스케일됨 (v2.1.128) — 이전 버전은 이미지를 완전히 거부했습니다.

### 수정됨

- Windows에서 VS Code 확장 활성화 (v2.1.131).
- Mantle 엔드포인트 인증 (v2.1.131).
- 1시간 프롬프트 캐시 TTL이 더 이상 5분으로 잘리지 않음 (v2.1.129).
- 10MB를 초과하는 stdin 페이로드에서 충돌 (v2.1.128).

### 번역 유지 관리자 참고사항

`vi/`, `zh/`, `uk/`, `ja/` 지역화 트리는 커뮤니티에서 유지 관리되며 영어 소스보다 뒤쳐질 수 있습니다. 번역을 동기화하는 기여자는 이 릴리스에서 업데이트된 영어 파일을 비교(diff)해야 합니다.

## [v2.1.126] — 2026-05-02

### Claude Code v2.1.126에 동기화됨

튜토리얼 범위를 Claude Code v2.1.119 → v2.1.126로 업데이트 (2026년 5월 1일 릴리스). v2.1.120은 첫 릴리스 당일(2026-04-24)에 롤백되었으나, 2026-04-28에 원래 보고된 회귀 문제가 수정되어 재출시되었습니다. v2.1.124와 v2.1.125는 Anthropic에 의해 건너뛰어 출시되지 않았습니다.

### 추가됨 (영문 문서)

- `claude project purge [path]` 하위 명령어 (v2.1.126) — 프로젝트의 모든 Claude Code 상태(트랜스크립트, 태스크, 디버그 로그, 파일 편집 기록, 프롬프트 기록, `~/.claude.json` 항목)를 삭제합니다. `--dry-run`, `-y/--yes`, `-i/--interactive`, `--all`을 지원합니다. `10-cli/README.md`에 문서화됨.
- `claude plugin prune` 하위 명령어 (v2.1.121) — 고아가 된 자동 설치 플러그인 종속성을 제거합니다. `plugin uninstall --prune`이 연쇄적으로 적용됩니다. `07-plugins/README.md`에 문서화됨.
- `claude ultrareview [target]` 하위 명령어 (v2.1.120) — CI/스크립트에서 비대화식으로 `/ultrareview`를 실행하고, 결과를 stdout에 출력하며, 성공/실패 시 0/1로 종료합니다. `--json` 및 `--timeout <minutes>`을 지원합니다. `10-cli/README.md`에 문서화됨.
- 스킬 콘텐츠 내에서 사용 가능한 `${CLAUDE_EFFORT}` 플레이스홀더 (v2.1.120) — 현재 노력 수준으로 확인됩니다. `03-skills/README.md`에 문서화됨.
- `alwaysLoad` MCP 서버 설정 옵션 (v2.1.121) — `true`로 설정하면 해당 서버의 모든 도구가 도구 검색 지연을 건너뜁니다. `05-mcp/README.md`에 문서화됨.
- `PostToolUse.hookSpecificOutput.updatedToolOutput`이 이제 모든 도구에서 작동 (v2.1.121), 이전에는 MCP 전용. `06-hooks/README.md`에 문서화됨.
- `ANTHROPIC_BEDROCK_SERVICE_TIER` 환경 변수 (v2.1.122) — Bedrock 서비스 계층 선택 (`default`, `flex`, `priority`). `10-cli/README.md` 환경 변수 테이블에 문서화됨.
- `--dangerously-skip-permissions` 확장 경로 범위 (v2.1.121, v2.1.126) — 이제 `.claude/skills/`, `.claude/agents/`, `.claude/commands/`, `.claude/`, `.git/`, `.vscode/`, 셸 설정 파일에 대한 쓰기 프롬프트를 우회합니다. 재앙적 제거 명령어(`rm -rf /` 등)는 여전히 프롬프트를 표시합니다. `09-advanced-features/README.md` 권한 모드 섹션에 문서화됨.
- OAuth 코드 붙여넣기 대체 (v2.1.126) — `claude auth login`이 브라우저 콜백이 localhost(WSL2, SSH, 컨테이너)에 도달할 수 없을 때 터미널에 붙여넣은 OAuth 코드를 수락합니다. `10-cli/README.md`에 문서화됨.
- 타입-투-필터 `/skills` 메뉴 (v2.1.121). `03-skills/README.md`에 문서화됨.
- `AI_AGENT` 환경 변수 (v2.1.120) — 하위 프로세스에 설정되어 `gh`가 트래픽을 Claude Code에 귀속시킬 수 있음. `10-cli/README.md` 환경 변수 테이블에 문서화됨.

### 변경됨

- `--from-pr` (v2.1.119) 및 `/resume` PR-URL 검색 (v2.1.122)이 이제 모두 GitHub, GitHub Enterprise, GitLab, Bitbucket URL을 지원합니다.
- Windows: Git for Windows / Git Bash가 더 이상 필요하지 않음 (v2.1.120) — Claude Code는 Git Bash가 없을 때 PowerShell을 셸 도구로 사용합니다. v2.1.126부터 PowerShell 도구가 활성화되면 PowerShell이 기본 셸이 됩니다. 감지가 Microsoft Store, PATH 없는 MSI, `.NET global tool`을 통해 설치된 PowerShell 7로 확장되었습니다. `09-advanced-features/README.md` 플랫폼 노트에 문서화됨.
- `/model` 선택기가 이제 `ANTHROPIC_BASE_URL`이 Anthropic 호환 게이트웨이를 가리킬 때 게이트웨이의 `/v1/models` 엔드포인트에서 모델을 나열 (v2.1.126). `10-cli/README.md`에 문서화됨.
- `--dangerously-skip-permissions`가 더욱 확장된 허용 목록(추가됨 참조)에 대한 쓰기 프롬프트를 표시하지 않음. 재앙적 제거는 여전히 프롬프트를 표시합니다.
- 이미지 붙여넣기 자동 다운스케일 (v2.1.126) — 2000px를 초과하는 이미지는 붙여넣기 시 다운스케일됩니다. 기록의 과도하게 큰 이미지는 자동으로 제거되고 요청이 재시도됩니다. (안전성/UX 노트로만 튜토리얼과 관련됨.)

### 보안

- 더 높은 우선순위의 관리 설정 소스에 `sandbox` 블록이 없을 때 `allowManagedDomainsOnly` / `allowManagedReadPathsOnly`가 무시되던 문제 수정 (v2.1.126).

### 번역 유지 관리자 참고사항

`vi/`, `zh/`, `uk/`, `ja/` 지역화 트리는 커뮤니티에서 유지 관리되며 영어 소스보다 뒤쳐질 수 있습니다. 번역을 동기화하는 기여자는 이 릴리스에서 업데이트된 영어 파일을 비교(diff)해야 합니다.

## [v2.4.0] — 2026-04-27

### Claude Code v2.1.119에 동기화됨

튜토리얼 범위를 Claude Code v2.1.112 → v2.1.119로 업데이트 (2026년 4월 23일 릴리스). v2.1.120은 4월 24일에 출시되었으나 같은 날 회귀 문제로 잠시 롤백되었고, 4월 28일에 수정되어 재출시되었습니다. 이제 정규 릴리스 라인의 일부입니다. 이후 v2.1.126(2026년 5월 1일)이 다음 안정적인 대상이며 위의 v2.1.126 항목에서 다룹니다.

### 추가됨 (영문 문서)

- 네이티브 바이너리 패키징 노트 (v2.1.113) — CLI가 이제 플랫폼별 네이티브 바이너리로 제공됨
- 네이티브 macOS/Linux 빌드의 `bfs`/`ugrep` Glob/Grep 대체 각주 (v2.1.117)
- 예제가 포함된 `mcp_tool` 훅 유형 (v2.1.118)
- PostToolUse / PostToolUseFailure 입력의 `duration_ms` 필드 (v2.1.119)
- `prUrlTemplate` 설정 (v2.1.119) 및 확장된 `--from-pr` 제공업체 목록 (GitLab, Bitbucket)
- `cleanupPeriodDays` 확장 범위 (체크포인트 + 태스크 + 셸 스냅샷 + 백업, v2.1.117)
- 모든 라이프사이클 이벤트에서 플러그인 마켓플레이스 강제 적용 (v2.1.117) 및 `hostPattern`/`pathPattern` 정규식 (v2.1.119)
- 새 환경 변수: `DISABLE_UPDATES`, `CLAUDE_CODE_HIDE_CWD`, `CLAUDE_CODE_FORK_SUBAGENT`, `OTEL_LOG_TOOL_DETAILS`, `ENABLE_TOOL_SEARCH` Vertex 옵트인
- 새 슬래시 명령어: `/btw`, `/theme` (사용자 정의 테마 포함)
- `/usage` 표준 명령어 (`/cost` + `/stats` 통합, v2.1.118)
- 포크된 서브에이전트 (`CLAUDE_CODE_FORK_SUBAGENT=1`, v2.1.117)
- 자동 모드 `"$defaults"` 토큰 (v2.1.118)
- `wslInheritsWindowsSettings` 관리 정책 (v2.1.118)
- Vim 시각적 / 시각적-라인 모드 (v2.1.118)
- `claude install [version]` 및 `claude plugin tag` 하위 명령어

### 변경됨

- 문서 호스트 변경: `docs.anthropic.com/en/docs/claude-code/*` → `code.claude.com/docs/en/*`
- Opus 4.7 노력 수준: `xhigh`가 2026-04-16 출시 이후 Claude Code 기본값이 됨; Opus 4.7 네이티브 컨텍스트 창이 1M로 확인됨 (v2.1.117에서 `/context`가 200K로 잘못 계산하던 문제 수정)
- Pro/Max 가입자의 Opus 4.6 / Sonnet 4.6 기본 노력이 `medium`에서 `high`로 상향 (v2.1.117)
- `STYLE_GUIDE.md` 소스 URL이 Claude Apps 문서에서 `code.claude.com/docs/en/changelog`로 업데이트됨

### 사용 중단됨 (추적됨, 제거되지 않음)

- `includeCoAuthoredBy` 설정 → `attribution.commit` / `attribution.pr` 사용
- `voiceEnabled` 설정 → `voice.enabled` 사용

### 번역 유지 관리자 참고사항

`vi/`, `zh/`, `uk/` 지역화 트리는 커뮤니티에서 유지 관리되며 영어 소스보다 뒤쳐질 수 있습니다. 번역을 동기화하는 기여자는 이 릴리스에서 업데이트된 영어 파일을 비교(diff)해야 합니다.

## v2.1.112 — 2026-04-16

### 주요 사항

- 모든 영어 튜토리얼을 Claude Code v2.1.112 및 새로운 Opus 4.7 모델(`claude-opus-4-7`)에 동기화. 여기에는 새로운 `xhigh` 노력 수준(Opus 4.7 기본값, `high`와 `max` 사이), 두 개의 새로운 내장 슬래시 명령어(`/ultrareview`, `/less-permission-prompts`), Opus 4.7 Max 가입자에게 더 이상 `--enable-auto-mode`가 필요하지 않은 자동 모드, Windows용 PowerShell 도구, "자동 (터미널 일치)" 테마, 프롬프트 이름을 딴 계획 파일이 포함됩니다. 18개 EN 문서 푸터가 Claude Code v2.1.112로 업데이트됨. @Luong NGUYEN

### 기능

- 모든 모듈, 루트 문서, 예제 및 참조에 대한 완전한 우크라이나어(uk) 지역화 추가 (039dde2) @Evgenij I

### 버그 수정

- pre-tool-check.sh 훅 프로토콜 버그 수정 (bce7cf8) @yarlinghe
- 잘못된 mermaid 예제를 텍스트 블록으로 변경하여 CI 통과 (b8a7b1f) @Evgenij I
- 우크라이나어 claude_concepts_guide.md 목차의 CP1251 인코딩 수정 (d970cc6) @Evgenij I
- 스텁 우크라이나어 README를 전체 번역으로 대체, 깨진 앵커 수정 (f6d73e2) @Evgenij I
- 모든 푸터의 Claude Code 버전을 2.1.97로 수정 (63a1416) @Luong NGUYEN
- 2026-04-09 문서 정확성 업데이트 적용 (e015f39) @Luong NGUYEN

### 문서화

- Claude Code v2.1.112에 동기화 (Opus 4.7, `xhigh` 노력, `/ultrareview`, `/less-permission-prompts`, PowerShell 도구, 자동-매치-터미널 테마) @Luong NGUYEN
- Claude Code v2.1.110에 동기화 (TUI, 푸시 알림, 세션 요약) (15f0085) @Luong NGUYEN
- Claude Code v2.1.101에 동기화 (`/team-onboarding`, `/ultraplan`, Monitor 도구) (2deba3a) @Luong NGUYEN
- 베트남어 문서를 영어 소스와 동기화 (561c6cb) @Thiên Toán
- 모든 파일의 마지막 업데이트 날짜 및 Claude Code 버전 업데이트 (7f2e773) @Luong NGUYEN
- 언어 전환기에 우크라이나어 언어 링크 추가 (9c224ff) @Luong NGUYEN
- 기여자 섹션 제거 (f07313d) @Luong NGUYEN
- GitHub 지표를 21,800+ 스타, 2,585+ 포크로 업데이트 (4f55374) @Luong NGUYEN

**전체 변경 로그**: https://github.com/luongnv89/claude-howto/compare/v2.3.0...v2.1.112

---

## v2.3.0 — 2026-04-07

### 기능

- 언어별 EPUB 아티팩트 빌드 및 게시 (90e9c30) @Thiên Toán
- 06-hooks에 누락된 pre-tool-check.sh 훅 추가 (b511ed1) @JiayuWang
- zh/ 디렉터리에 중국어 번역 추가 (89e89d4) @Luong NGUYEN
- 성능 최적화 서브에이전트 및 종속성 검사 훅 추가 (f53d080) @qk

### 버그 수정

- Windows Git Bash 호환성 + stdin JSON 프로토콜 (2cbb10c) @Luong NGUYEN
- 08-checkpoints의 autoCheckpoint 설정 문서 수정 (749c79f) @JiayuWang
- SVG 이미지를 플레이스홀더로 대체하지 않고 포함 (1b16709) @Thiên Toán
- memory README의 중첩 코드 펜스 렌더링 (ce24423) @Zhaoshan Duan
- 스쿼시 병합으로 누락된 리뷰 수정사항 적용 (34259ca) @Luong NGUYEN
- 훅 스크립트를 Windows Git Bash와 호환되게 만들고 stdin JSON 프로토콜 사용 (107153d) @binyu li

### 문서화

- 모든 튜토리얼을 최신 Claude Code 문서(2026년 4월)와 동기화 (72d3b01) @Luong NGUYEN
- 언어 전환기에 중국어 링크 추가 (6cbaa4d) @Luong NGUYEN
- 영어와 베트남어 간 언어 전환기 추가 (100c45e) @Luong NGUYEN
- GitHub #1 트렌딩 배지 추가 (0ca8c37) @Luong NGUYEN
- 컨텍스트 영역 모니터링을 위한 cc-context-stats 소개 (d41b335) @Luong NGUYEN
- luongnv89/skills 컬렉션 및 luongnv89/asm 스킬 관리자 소개 (7e3c0b6) @Luong NGUYEN
- 현재 GitHub 지표(5,900+ 스타, 690+ 포크)를 반영하도록 README 통계 업데이트 (5001525) @Luong NGUYEN
- 현재 GitHub 지표(3,900+ 스타, 460+ 포크)를 반영하도록 README 통계 업데이트 (9cb92d6) @Luong NGUYEN

### 리팩토링

- Kroki HTTP 종속성을 로컬 mmdc 렌더링으로 대체 (e76bbe4) @Luong NGUYEN
- 품질 검사를 pre-commit으로 이동, CI는 2차 검증 (6d1e0ae) @Luong NGUYEN
- 자동 모드 권한 기준선 축소 (2790fb2) @Luong NGUYEN
- 자동 적응 훅을 일회성 권한 설정 스크립트로 대체 (995a5d6) @Luong NGUYEN

### 기타

- 품질 게이트 왼쪽 이동 — pre-commit에 mypy 추가, CI 실패 수정 (699fb39) @Luong NGUYEN
- 베트남어(Tiếng Việt) 지역화 추가 (a70777e) @Thiên Toán

**전체 변경 로그**: https://github.com/luongnv89/claude-howto/compare/v2.2.0...v2.3.0

---

## v2.2.0 — 2026-03-26

### 문서화

- 모든 튜토리얼 및 참조를 Claude Code v2.1.84에 동기화 (f78c094) @luongnv89
  - 슬래시 명령어를 55개 이상의 내장 + 5개 번들 스킬로 업데이트, 3개 사용 중단 표시
  - 훅 이벤트를 18개에서 25개로 확장, `agent` 훅 유형 추가 (현재 4개 유형)
  - 고급 기능에 자동 모드, 채널, 음성 받아쓰기 추가
  - 스킬 프런트매터에 `effort`, `shell` 필드 추가; 에이전트 필드에 `initialPrompt`, `disallowedTools` 추가
  - WebSocket MCP 전송, elicitation, 2KB 도구 제한 추가
  - 플러그인 LSP 지원, `userConfig`, `${CLAUDE_PLUGIN_DATA}` 추가
  - 모든 참조 문서 업데이트 (CATALOG, QUICK_REFERENCE, LEARNING-ROADMAP, INDEX)
- README를 랜딩 페이지 구조의 가이드로 재작성 (32a0776) @luongnv89

### 버그 수정

- CI 준수를 위해 누락된 cSpell 단어 및 README 섹션 추가 (93f9d51) @luongnv89
- cSpell 사전에 `Sandboxing` 추가 (b80ce6f) @luongnv89

**전체 변경 로그**: https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### 버그 수정

- CI 링크 검사를 실패시키는 죽은 마켓플레이스 링크 제거 (3fdf0d6) @luongnv89
- cSpell 사전에 `sandboxed` 및 `pycache` 추가 (dc64618) @luongnv89

**전체 변경 로그**: https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### 기능

- 적응형 학습 경로를 위한 자기 평가 및 수업 퀴즈 스킬 추가 (1ef46cd) @luongnv89
  - `/self-assessment` — 10개 기능 영역에 걸친 대화형 숙련도 퀴즈와 개인화된 학습 경로 제공
  - `/lesson-quiz [lesson]` — 수업별 지식 확인 (8-10개의 대상 질문)

### 버그 수정

- 깨진 URL, 사용 중단 사항, 오래된 참조 업데이트 (8fe4520) @luongnv89
- 리소스 및 자기 평가 스킬의 깨진 링크 수정 (7a05863) @luongnv89
- 개념 가이드의 중첩 코드 블록에 물결표 펜스 사용 (5f82719) @VikalpP
- cSpell 사전에 누락된 단어 추가 (8df7572) @luongnv89

### 문서화

- 5단계 QA — 문서 전반의 일관성, URL 및 용어 수정 (00bbe4c) @luongnv89
- 3-4단계 완료 — 새로운 기능 범위 및 참조 문서 업데이트 (132de29) @luongnv89
- MCP 컨텍스트 bloating 섹션에 MCPorter 런타임 추가 (ef52705) @luongnv89
- 6개 가이드에 걸쳐 누락된 명령어, 기능 및 설정 추가 (4bc8f15) @luongnv89
- 기존 저장소 규칙을 기반으로 스타일 가이드 추가 (84141d0) @luongnv89
- 가이드 비교 테이블에 자기 평가 행 추가 (8fe0c96) @luongnv89
- PR #7에 대한 기여자 목록에 VikalpP 추가 (d5b4350) @luongnv89
- README 및 로드맵에 자기 평가 및 수업 퀴즈 스킬 참조 추가 (d5a6106) @luongnv89

### 새로운 기여자

- @VikalpP가 #7에서 첫 번째 기여를 했습니다

**전체 변경 로그**: https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### 기능

- 모든 문서를 Claude Code 2026년 2월 기능과 동기화 (487c96d)
  - 10개 튜토리얼 디렉터리 및 7개 참조 문서의 26개 파일 업데이트
  - **자동 메모리** 문서 추가 — 프로젝트별 지속적 학습
  - **원격 제어**, **웹 세션**, **데스크톱 앱** 문서 추가
  - **에이전트 팀** 문서 추가 (실험적 다중 에이전트 협업)
  - **MCP OAuth 2.0**, **도구 검색**, **Claude.ai 커넥터** 문서 추가
  - **영구 메모리** 및 서브에이전트용 **워크트리 격리** 문서 추가
  - **백그라운드 서브에이전트**, **작업 목록**, **프롬프트 제안** 문서 추가
  - **샌드박싱** 및 **관리 설정** 문서 추가 (엔터프라이즈)
  - **HTTP 훅** 및 7개의 새로운 훅 이벤트 문서 추가
  - **플러그인 설정**, **LSP 서버** 및 마켓플레이스 업데이트 문서 추가
  - **체크포인트에서 요약** 되감기 옵션 문서 추가
  - 17개의 새로운 슬래시 명령어 문서화 (`/fork`, `/desktop`, `/teleport`, `/tasks`, `/fast` 등)
  - 새로운 CLI 플래그 문서화 (`--worktree`, `--from-pr`, `--remote`, `--teleport`, `--teammate-mode` 등)
  - 자동 메모리, 노력 수준, 에이전트 팀 등을 위한 새로운 환경 변수 문서화

### 디자인

- 로고를 컴퍼스-브래킷 마크로 최소 팔레트로 재디자인 (20779db)

### 버그 수정 / 정정

- 모델 이름 업데이트: Sonnet 4.5 → **Sonnet 4.6**, Opus 4.5 → **Opus 4.6**
- 권한 모드 이름 수정: 가상의 "Unrestricted/Confirm/Read-only"를 실제 `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions`로 대체
- 훅 이벤트 수정: 가상의 `PreCommit`/`PostCommit`/`PrePush` 제거, 실제 이벤트 추가 (`SubagentStart`, `WorktreeCreate`, `ConfigChange` 등)
- CLI 구문 수정: `claude-code --headless`를 `claude -p` (인쇄 모드)로 대체
- 체크포인트 명령어 수정: 가상의 `/checkpoint save/list/rewind/diff`를 실제 `Esc+Esc` / `/rewind` 인터페이스로 대체
- 세션 관리 수정: 가상의 `/session list/new/switch/save`를 실제 `/resume`/`/rename`/`/fork`로 대체
- 플러그인 매니페스트 형식 수정: `plugin.yaml` → `.claude-plugin/plugin.json`으로 마이그레이션
- MCP 설정 경로 수정: `~/.claude/mcp.json` → `.mcp.json` (프로젝트) / `~/.claude.json` (사용자)
- 문서 URL 수정: `docs.claude.com` → `docs.anthropic.com`; 가상의 `plugins.claude.com` 제거
- 여러 파일에서 가상의 설정 필드 제거
- 모든 "마지막 업데이트" 날짜를 2026년 2월로 업데이트

**전체 변경 로그**: https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
