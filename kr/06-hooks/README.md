<!-- i18n-source: 06-hooks/README.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# 훅

훅은 Claude Code 세션 중 특정 이벤트에 응답하여 실행되는 자동화 스크립트입니다. 자동화, 검증, 권한 관리 및 사용자 정의 워크플로우를 가능하게 합니다.

## 개요

훅은 Claude Code에서 특정 이벤트가 발생할 때 자동으로 실행되는 자동화된 작업(셸 명령어, HTTP 웹훅, LLM 프롬프트, MCP 도구 호출 또는 서브에이전트 평가)입니다. JSON 입력을 받고 종료 코드와 JSON 출력을 통해 결과를 전달합니다.

**주요 기능:**
- 이벤트 기반 자동화
- JSON 기반 입출력
- `command`, `http`, `mcp_tool`, `prompt`, `agent` 훅 유형 지원
- 도구별 훅을 위한 패턴 매칭

## 설정

훅은 특정 구조의 설정 파일에서 구성됩니다:

- `~/.claude/settings.json` - 사용자 설정 (모든 프로젝트)
- `.claude/settings.json` - 프로젝트 설정 (공유 가능, 커밋됨)
- `.claude/settings.local.json` - 로컬 프로젝트 설정 (커밋되지 않음)
- 관리 정책 - 조직 전체 설정
- 플러그인 `hooks/hooks.json` - 플러그인 범위 훅
- 스킬/에이전트 frontmatter - 컴포넌트 수명 훅

### 기본 설정 구조

```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolPattern",
        "hooks": [
          {
            "type": "command",
            "command": "your-command-here",
            "timeout": 60
          }
        ]
      }
    ]
  }
}
```

**주요 필드:**

| 필드 | 설명 | 예시 |
|-------|-------------|---------|
| `matcher` | 도구 이름과 일치시키는 패턴 (대소문자 구분) | `"Write"`, `"Edit\|Write"`, `"*"` |
| `hooks` | 훅 정의 배열 | `[{ "type": "command", ... }]` |
| `type` | 훅 유형: `"command"` (bash), `"prompt"` (LLM), `"http"` (웹훅), `"mcp_tool"` (MCP 도구 호출, v2.1.118+), 또는 `"agent"` (서브에이전트) | `"command"` |
| `command` | 실행할 셸 명령어 | `"$CLAUDE_PROJECT_DIR/.claude/hooks/format.sh"` |
| `timeout` | 선택적 시간 초과(초, 기본값 60) | `30` |
| `once` | `true`이면 세션당 한 번만 훅 실행 | `true` |

### 매처 패턴

| 패턴 | 설명 | 예시 |
|---------|-------------|---------|
| 정확한 문자열 | 특정 도구와 일치 | `"Write"` |
| 정규식 패턴 | 여러 도구와 일치 | `"Edit\|Write"` |
| 쉼표로 구분 | 나열된 도구와 일치 (v2.1.191+) | `"Write,Edit"` |
| 와일드카드 | 모든 도구와 일치 | `"*"` 또는 `""` |
| MCP 도구 | 서버 및 도구 패턴 | `"mcp__memory__.*"` |

> **매처는 정확히 일치합니다 (v2.1.195+).** 하이픈이 포함된 식별자(예: 하이픈이 포함된 MCP 도구 이름)가 더 이상 다른 도구와 실수로 하위 문자열 일치하지 않습니다. `"Write,Edit"`와 같은 쉼표로 구분된 매처는 목록의 모든 도구에서 실행됩니다 — 이전 빌드에서는 조용히 실행되지 않았습니다.

**InstructionsLoaded 매처 값:**

| 매처 값 | 설명 |
|---------------|-------------|
| `session_start` | 세션 시작 시 명령 로드됨 |
| `nested_traversal` | 중첩 디렉토리 탐색 중 명령 로드됨 |
| `path_glob_match` | 경로 glob 패턴 매칭을 통해 명령 로드됨 |

### `if` 조건으로 범위 좁히기 (도구 인자 경로)

`matcher` 필드는 **도구 이름**으로 훅을 선택합니다 (`"Write"`, `"Edit|Write"`, `"*"`). 도구의 **인자**로 더 좁게 필터링하려면 — 예를 들어, 편집이 `src/`에 닿을 때만 훅을 실행하거나 시크릿 파일 읽기를 보호하려면 — 개별 훅 핸들러에 `if` 조건을 추가하세요. 이는 도구 이름 매처와는 별개입니다: `matcher`는 *어떤 도구*인지 결정하고, `if`는 *어떤 호출*인지 결정합니다.

`if`는 [권한 규칙 문법](https://code.claude.com/docs/en/permissions) (`ToolName(pattern)`)을 사용하며, 도구 이름 **과** 그 인자를 함께 평가합니다. `Read`/`Edit`/`Write`의 경우 경로 패턴은 gitignore 의미를 따르며, 권한 규칙과 동일한 앵커를 사용합니다: `.env`와 같은 단순 이름은 모든 깊이에서 일치, `src/**`는 현재 디렉토리 기준, `/src/**`는 프로젝트 루트 기준, `~/...`는 홈 디렉토리, `//...`는 절대 파일 시스템 경로입니다.

`if` 필드는 **훅 핸들러 수준**에 위치합니다 — `type` 및 `command`의 형제로, `hooks` 배열 내부에 있으며 `matcher`에 있지 않습니다:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "if": "Edit(src/**)",
            "command": "./hooks/lint-src.sh"
          }
        ]
      },
      {
        "matcher": "Read",
        "hooks": [
          {
            "type": "command",
            "if": "Read(.env)",
            "command": "./hooks/block-secret-read.sh"
          }
        ]
      }
    ]
  }
}
```

유효한 `if` 패턴 예시: `Edit(src/**)` (`src/` 아래의 편집), `Read(~/.ssh/**)` (모든 SSH 키 읽기), `Read(.env)` (현재 디렉토리 또는 아래의 모든 `.env`), `Bash(git push *)` (`git push` 하위 명령어만).

## 훅 유형

Claude Code는 다섯 가지 훅 유형을 지원합니다:

### 명령어 훅

기본 훅 유형입니다. 셸 명령어를 실행하고 JSON stdin/stdout 및 종료 코드를 통해 통신합니다.

```json
{
  "type": "command",
  "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py\"",
  "timeout": 60
}
```

#### Exec 형식 (`args`)

> v2.1.139에서 추가됨.

셸 형식의 `"command": "..."` 대신, 명령어 훅은 `args` 배열로 `execve()`를 통해 직접 바이너리를 실행할 수 있습니다. 셸 구문 분석이 없으므로 경로 플레이스홀더에 인용이 필요 없으며 설정이 셸 인젝션 버그에 면역됩니다.

```json
{
  "type": "command",
  "args": ["python3", "$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py", "--strict"],
  "timeout": 60
}
```

두 형식은 **상호 배타적**입니다 — `command`와 `args`가 모두 설정된 훅은 설정 로드 시 거부됩니다. 파이프, 리디렉션, `&&` 체이닝 또는 셸 확장이 필요하면 `command`를 사용하고, 인자와 함께 하나의 바이너리를 호출할 때는 `args`를 사용하세요.

### HTTP 훅

> v2.1.63에서 추가됨.

명령어 훅과 동일한 JSON 입력을 수신하는 원격 웹훅 엔드포인트입니다. HTTP 훅은 URL에 JSON을 POST하고 JSON 응답을 받습니다. HTTP 훅은 샌드박싱이 활성화된 경우 샌드박스를 통해 라우팅됩니다. URL의 환경 변수 보간에는 보안을 위해 명시적인 `allowedEnvVars` 목록이 필요합니다.

```json
{
  "hooks": {
    "PostToolUse": [{
      "type": "http",
      "url": "https://my-webhook.example.com/hook",
      "matcher": "Write"
    }]
  }
}
```

**주요 속성:**
- `"type": "http"` -- HTTP 훅으로 식별
- `"url"` -- 웹훅 엔드포인트 URL
- 샌드박스가 활성화된 경우 샌드박스를 통해 라우팅됨
- URL의 환경 변수 보간을 위해 명시적인 `allowedEnvVars` 목록 필요

### 프롬프트 훅

훅 콘텐츠가 Claude가 평가하는 프롬프트인 LLM 평가 프롬프트입니다. 주로 `Stop` 및 `SubagentStop` 이벤트와 함께 지능적인 작업 완료 확인에 사용됩니다.

```json
{
  "type": "prompt",
  "prompt": "Evaluate if Claude completed all requested tasks.",
  "timeout": 30
}
```

LLM이 프롬프트를 평가하고 구조화된 결정을 반환합니다 (자세한 내용은 [프롬프트 기반 훅](#프롬프트-기반-훅) 참조).

### MCP 도구 훅

> v2.1.118에서 추가됨.

`mcp_tool` 유형은 설정된 MCP 도구를 직접 호출합니다; 설정은 셸 명령어나 URL 대신 MCP 서버와 도구 이름을 참조합니다. 이는 검증 또는 반응 로직이 이미 설정한 MCP 서버에 있을 때 유용합니다.

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "mcp_tool",
    "server": "my-mcp-server",
    "tool": "validate_edit"
  }]
}
```

**주요 속성:**
- `"type": "mcp_tool"` -- MCP 도구 훅으로 식별
- `"server"` -- 설정된 MCP 서버의 이름
- `"tool"` -- 호출할 서버의 도구 이름

훅 입력(도구 이름, 도구 입력, 세션 컨텍스트)이 MCP 도구의 인자로 전달됩니다. MCP 서버 설정은 [MCP 서버 설정](../05-mcp/README.md)을 참조하세요.

### 에이전트 훅

조건을 평가하거나 복잡한 검사를 수행하기 위해 전용 에이전트를 생성하는 서브에이전트 기반 검증 훅입니다. 프롬프트 훅(단일 턴 LLM 평가)과 달리, 에이전트 훅은 도구를 사용하고 다단계 추론을 수행할 수 있습니다.

```json
{
  "type": "agent",
  "prompt": "Verify the code changes follow our architecture guidelines. Check the relevant design docs and compare.",
  "timeout": 120
}
```

**주요 속성:**
- `"type": "agent"` -- 에이전트 훅으로 식별
- `"prompt"` -- 서브에이전트의 작업 설명
- 에이전트는 도구(Read, Grep, Bash 등)를 사용하여 평가 수행 가능
- 프롬프트 훅과 유사한 구조화된 결정 반환

## 훅 이벤트

Claude Code는 **30개의 훅 이벤트**를 지원합니다:

| 이벤트 | 트리거 시점 | 매처 입력 | 차단 가능 | 일반적인 사용 |
|-------|---------------|---------------|-----------|------------|
| **SessionStart** | 세션 시작/재개/클리어/압축 | startup/resume/clear/compact | 아니요 | 환경 설정 |
| **Setup** | 초기 환경 설정 (세션당 한 번) | (없음) | 아니요 | 도구 프로비저닝, 의존성 설치 |
| **InstructionsLoaded** | CLAUDE.md 또는 규칙 파일 로드 후 | (없음) | 아니요 | 명령 수정/필터링 |
| **UserPromptSubmit** | 사용자 프롬프트 제출 | (없음) | 예 | 프롬프트 검증 |
| **UserPromptExpansion** | 사용자 프롬프트 확장 (예: `@` 멘션, 슬래시 명령어 해결) | (없음) | 예 | 확장된 프롬프트 변환 또는 검사 |
| **PreToolUse** | 도구 실행 전 | 도구 이름 | 예 (허용/거부/질문) | 입력 검증, 수정 |
| **PermissionRequest** | 권한 대화상자 표시 | 도구 이름 | 예 | 자동 승인/거부 |
| **PermissionDenied** | 사용자가 권한 프롬프트 거부 | 도구 이름 | 아니요 | 로깅, 분석, 정책 적용 |
| **PostToolUse** | 도구 성공 후 | 도구 이름 | 아니요 | 컨텍스트 추가, 피드백 |
| **PostToolUseFailure** | 도구 실행 실패 | 도구 이름 | 아니요 | 오류 처리, 로깅 |
| **PostToolBatch** | 도구 사용 배치 완료 후 | (없음) | 아니요 | 집계 보고, 배치 검증 |
| **Notification** | 알림 전송 | 알림 유형 | 아니요 | 사용자 정의 알림 |
| **MessageDisplay** | 어시스턴트 메시지 텍스트 표시 중 | (없음) | 아니요 | 표시된 메시지 텍스트 변환 또는 숨김 (v2.1.152) |
| **SubagentStart** | 서브에이전트 생성됨 | 에이전트 유형 이름 | 아니요 | 서브에이전트 설정 |
| **SubagentStop** | 서브에이전트 완료 | 에이전트 유형 이름 | 예 | 서브에이전트 검증 |
| **Stop** | Claude 응답 완료 | (없음) | 예 | 작업 완료 확인 |
| **StopFailure** | API 오류로 턴 종료 | (없음) | 아니요 | 오류 복구, 로깅 |
| **TeammateIdle** | 에이전트 팀 팀원 유휴 상태 | (없음) | 예 | 팀원 조정 |
| **TaskCompleted** | 작업 완료 표시됨 | (없음) | 예 | 작업 후 조치 |
| **TaskCreated** | TaskCreate로 작업 생성됨 | (없음) | 아니요 | 작업 추적, 로깅 |
| **ConfigChange** | 설정 파일 변경 | (없음) | 예 (정책 제외) | 설정 업데이트에 반응 |
| **CwdChanged** | 작업 디렉토리 변경 | (없음) | 아니요 | 디렉토리별 설정 |
| **FileChanged** | 감시 중인 파일 변경 | (없음) | 아니요 | 파일 모니터링, 재빌드 |
| **PreCompact** | 컨텍스트 압축 전 | manual/auto | 아니요 | 압축 전 조치 |
| **PostCompact** | 압축 완료 후 | (없음) | 아니요 | 압축 후 조치 |
| **WorktreeCreate** | 워크트리 생성 중 | (없음) | 예 (경로 반환) | 워크트리 초기화 |
| **WorktreeRemove** | 워크트리 제거 중 | (없음) | 아니요 | 워크트리 정리 |
| **Elicitation** | MCP 서버가 사용자 입력 요청 | (없음) | 예 | 입력 검증 |
| **ElicitationResult** | 사용자가 요청에 응답 | (없음) | 예 | 응답 처리 |
| **SessionEnd** | 세션 종료 | (없음) | 아니요 | 정리, 최종 로깅 |

> **PostToolUse 기간 (v2.1.119):** `PostToolUse` 및 `PostToolUseFailure` 훅 입력에 이제 `duration_ms`가 포함됩니다 — 자세한 내용은 [PostToolUse](#posttooluse) 섹션을 참조하세요.

### PreToolUse

Claude가 도구 파라미터를 생성한 후 처리 전에 실행됩니다. 도구 입력을 검증하거나 수정하는 데 사용하세요.

**설정:**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py"
          }
        ]
      }
    ]
  }
}
```

**일반적인 매처:** `Task`, `Bash`, `Glob`, `Grep`, `Read`, `Edit`, `Write`, `WebFetch`, `WebSearch`

**출력 제어:**
- `permissionDecision`: `"allow"`, `"deny"`, 또는 `"ask"`
- `permissionDecisionReason`: 결정에 대한 설명
- `updatedInput`: 수정된 도구 입력 파라미터

### PostToolUse

도구 완료 직후 실행됩니다. 검증, 로깅 또는 Claude에 컨텍스트 제공에 사용합니다.

**설정:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py"
          }
        ]
      }
    ]
  }
}
```

**출력 제어:**
- `"block"` 결정은 피드백과 함께 Claude에 프롬프트
- `additionalContext`: Claude를 위해 추가된 컨텍스트

**추가 입력 필드 (v2.1.119):**

| 필드 | 유형 | 설명 |
|-------|------|-------------|
| `duration_ms` | number | 도구 실행 시간(밀리초). 권한 프롬프트 및 PreToolUse 훅 실행 시간은 제외. `PostToolUse` 및 `PostToolUseFailure` 훅 모두에서 사용 가능. |

#### 복구 가능한 블록 (`continueOnBlock`, v2.1.139)

기본적으로 `"decision": "block"`을 반환하는 `PostToolUse` 훅은 현재 턴을 중단합니다. 훅에 `"continueOnBlock": true`를 설정하면 거부를 `tool_result`로 Claude에 다시 전달하여 모델이 피드백을 읽고 재시도하거나 조정할 수 있습니다.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/policy-check.py",
            "continueOnBlock": true
          }
        ]
      }
    ]
  }
}
```

훅의 `reason`이 Claude가 조치할 수 있는 내용일 때 사용하세요(예: "이 파일은 읽기 전용입니다; 다른 곳에 쓰세요"); 블록이 턴을 완전히 중단해야 할 때는 빼두세요.

### UserPromptSubmit

사용자가 프롬프트를 제출할 때, Claude가 처리하기 전에 실행됩니다.

**설정:**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py"
          }
        ]
      }
    ]
  }
}
```

**출력 제어:**
- `decision`: 처리를 방지하려면 `"block"`
- `reason`: 차단된 경우 설명
- `additionalContext`: 프롬프트에 추가된 컨텍스트

### Stop 및 SubagentStop

Claude가 응답을 완료할 때(Stop) 또는 서브에이전트가 완료될 때(SubagentStop) 실행됩니다. 지능적인 작업 완료 확인을 위한 프롬프트 기반 평가를 지원합니다.

**추가 입력 필드:** `Stop` 및 `SubagentStop` 훅 모두 JSON 입력에 `last_assistant_message` 필드를 수신하며, 중지 전 Claude 또는 서브에이전트의 최종 메시지가 포함됩니다. 이는 작업 완료 평가에 유용합니다.

**설정:**
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Evaluate if Claude completed all requested tasks.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

> **연속 블록 안전 장치 (v2.1.143)**: `Stop` 훅이 동일한 턴에 대해 **8번 연속**으로 `"decision": "block"`(또는 `continue: false`)을 반환하면 Claude Code가 루프를 단축하고 경고와 함께 세션을 종료합니다. `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP=<integer>` 환경 변수로 임계값을 재정의하세요 (`0`으로 설정하면 캡이 완전히 비활성화됨). 이는 잘못된 Stop 훅이 세션을 영원히 루프시키는 것을 방지합니다.

**반환 필드 (v2.1.163):** `Stop` 또는 `SubagentStop` 훅은 `hookSpecificOutput.additionalContext`를 반환하여 Claude에게 피드백을 제공하고 **오류 레이블을 표시하지 않고 턴을 계속**할 수 있습니다. 이전에는 Stop 훅에서 모델에 영향을 주는 것이 불편했습니다. 이제 훅이 컨텍스트를 깔끔하게 주입하여 이전 피드백 경로(예: `"decision": "block"`)의 오류 레이블 동작을 피할 수 있습니다.

```json
{
  "hookSpecificOutput": {
    "hookEventName": "Stop",
    "additionalContext": "Reminder: run the test suite before declaring done."
  }
}
```

### SubagentStart

서브에이전트가 실행을 시작할 때 실행됩니다. 매처 입력은 에이전트 유형 이름으로, 훅이 특정 서브에이전트 유형을 대상으로 할 수 있습니다.

**설정:**
```json
{
  "hooks": {
    "SubagentStart": [
      {
        "matcher": "code-review",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/subagent-init.sh"
          }
        ]
      }
    ]
  }
}
```

### SessionStart

세션이 시작되거나 재개될 때 실행됩니다. 환경 변수를 유지할 수 있습니다.

**매처:** `startup`, `resume`, `clear`, `compact`

**특수 기능:** `CLAUDE_ENV_FILE`을 사용하여 환경 변수 유지 (`CwdChanged` 및 `FileChanged` 훅에서도 사용 가능):

```bash
#!/bin/bash
if [ -n "$CLAUDE_ENV_FILE" ]; then
  echo 'export NODE_ENV=development' >> "$CLAUDE_ENV_FILE"
fi
exit 0
```

**세션 범위 출력 (v2.1.152):** `SessionStart` 훅은 JSON을 반환하여 스킬을 재스캔하고 세션 제목을 설정할 수 있습니다:

```json
{
  "reloadSkills": true,
  "hookSpecificOutput": {
    "sessionTitle": "Payments migration"
  }
}
```

최상위 `reloadSkills: true`는 동일한 세션에서 스킬 재스캔을 트리거합니다(`/reload-skills` 명령어와 동일한 동작). 훅이 방금 설치한 스킬을 즉시 사용할 수 있게 만듭니다. `hookSpecificOutput.sessionTitle`은 시작 및 재개 시 세션의 표시 제목을 설정합니다.

### SessionEnd

세션이 종료될 때 정리를 수행하거나 최종 로깅을 위해 실행됩니다. 종료를 차단할 수 없습니다.

**이유 필드 값:**
- `clear` - 사용자가 세션을 지움
- `logout` - 사용자가 로그아웃함
- `prompt_input_exit` - 사용자가 프롬프트 입력을 통해 종료함
- `other` - 기타 이유

**설정:**
```json
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-cleanup.sh\""
          }
        ]
      }
    ]
  }
}
```

### 알림 이벤트

알림 이벤트에 대한 업데이트된 매처:
- `permission_prompt` - 권한 요청 알림
- `idle_prompt` - 유휴 상태 알림
- `auth_success` - 인증 성공
- `elicitation_dialog` - 사용자에게 표시된 대화상자

## 컴포넌트 범위 훅

훅을 특정 컴포넌트(스킬, 에이전트, 명령어)의 frontmatter에 연결할 수 있습니다:

**SKILL.md, agent.md 또는 command.md에서:**

```yaml
---
name: secure-operations
description: Perform operations with security checks
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/check.sh"
          once: true  # Only run once per session
---
```

**컴포넌트 훅에 지원되는 이벤트:** `PreToolUse`, `PostToolUse`, `Stop`

이를 통해 컴포넌트를 사용하는 곳에 직접 훅을 정의하여 관련 코드를 함께 유지할 수 있습니다.

### 서브에이전트 Frontmatter의 훅

서브에이전트의 frontmatter에 `Stop` 훅이 정의되면 해당 서브에이전트로 범위가 지정된 `SubagentStop` 훅으로 자동 변환됩니다. 이렇게 하면 중지 훅이 메인 세션이 중지될 때가 아니라 해당 특정 서브에이전트가 완료될 때만 실행됩니다.

```yaml
---
name: code-review-agent
description: Automated code review subagent
hooks:
  Stop:
    - hooks:
        - type: prompt
          prompt: "Verify the code review is thorough and complete."
  # The above Stop hook auto-converts to SubagentStop for this subagent
---
```

## PermissionRequest 이벤트

사용자 정의 출력 형식으로 권한 요청을 처리합니다:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow|deny",
      "updatedInput": {},
      "message": "Custom message",
      "interrupt": false
    }
  }
}
```

## 훅 입력 및 출력

### JSON 입력 (stdin을 통해)

모든 훅은 stdin을 통해 JSON 입력을 수신합니다:

```json
{
  "session_id": "abc123",
  "transcript_path": "/path/to/transcript.jsonl",
  "cwd": "/current/working/directory",
  "permission_mode": "default",
  "hook_event_name": "PreToolUse",
  "tool_name": "Write",
  "tool_input": {
    "file_path": "/path/to/file.js",
    "content": "..."
  },
  "tool_use_id": "toolu_01ABC123...",
  "agent_id": "agent-abc123",
  "agent_type": "main",
  "worktree": "/path/to/worktree",
  "effort": { "level": "medium" }
}
```

**공통 필드:**

| 필드 | 설명 |
|-------|-------------|
| `session_id` | 고유 세션 식별자 |
| `transcript_path` | 대화 기록 파일 경로 |
| `cwd` | 현재 작업 디렉토리 |
| `hook_event_name` | 훅을 트리거한 이벤트 이름 |
| `agent_id` | 이 훅을 실행하는 에이전트의 식별자 |
| `agent_type` | 에이전트 유형 (`"main"`, 서브에이전트 유형 이름 등) |
| `worktree` | 에이전트가 실행 중인 git 워크트리 경로 |
| `effort.level` | (v2.1.133+) 활성 노력 수준: `low`, `medium`, `high`, `xhigh`, 또는 `max` |

### 종료 코드

| 종료 코드 | 의미 | 동작 |
|-----------|---------|----------|
| **0** | 성공 | 계속, JSON stdout 파싱 |
| **2** | 차단 오류 | 작업 차단, stderr가 오류로 표시됨 |
| **기타** | 비차단 오류 | 계속, stderr가 상세 모드에서 표시됨 |

### JSON 출력 (stdout, 종료 코드 0)

```json
{
  "continue": true,
  "stopReason": "Optional message if stopping",
  "suppressOutput": false,
  "systemMessage": "Optional warning message",
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "File is in allowed directory",
    "updatedInput": {
      "file_path": "/modified/path.js"
    }
  }
}
```

> **범위 (v2.1.121+):** `hookSpecificOutput.updatedToolOutput`이 이제 **모든** 도구에서 적용됩니다(MCP 도구뿐만 아니라). `Bash`, `Edit`, `Read` 등의 `PostToolUse` 훅은 Claude가 보기 전에 도구의 출력을 다시 쓸 수 있습니다 — 시크릿 삭제, diff 정규화, 시끄러운 명령어 출력 필터링에 유용합니다. 예시 (`Bash` 출력에서 ANSI 색상 코드 제거):
>
> ```json
> {
>   "hookSpecificOutput": {
>     "hookEventName": "PostToolUse",
>     "updatedToolOutput": "<plain-text output with ANSI escapes removed>"
>   }
> }
> ```

#### `terminalSequence` (v2.1.141)

훅은 JSON 출력에 `terminalSequence`를 설정하여 원시 OSC(운영 체제 명령) 이스케이프 시퀀스를 내보낼 수 있습니다. 훅이 반환될 때 호스트가 시퀀스를 제어 터미널에 기록합니다 — 자체 TTY 없이 데스크탑 알림, 창 제목 업데이트 및 터미널 벨에 유용합니다.

| 필드 | 유형 | 설명 |
|-------|------|-------------|
| `terminalSequence` | string | 원시 이스케이프 시퀀스 (일반적으로 OSC 9 / OSC 0 / OSC 777). 호스트 터미널에 그대로 기록됨. |

예시 — 긴 작업이 완료될 때 OSC 9 데스크탑 알림 실행:

```json
{
  "terminalSequence": "]9;Task complete"
}
```

Claude가 턴을 완료할 때 알림이 실행되도록 `Stop` 훅에 설정하세요. 시퀀스 지원은 터미널에 따라 다릅니다; Kitty/iTerm2/Windows 터미널이 OSC 9을 지원합니다.

## 환경 변수

| 변수 | 사용 가능 | 설명 |
|----------|-------------|-------------|
| `CLAUDE_PROJECT_DIR` | 모든 훅 | 프로젝트 루트의 절대 경로 |
| `CLAUDE_ENV_FILE` | SessionStart, CwdChanged, FileChanged | 환경 변수 유지를 위한 파일 경로 |
| `CLAUDE_CODE_REMOTE` | 모든 훅 | 원격 환경에서 실행 중이면 `"true"` |
| `${CLAUDE_PLUGIN_ROOT}` | 플러그인 훅 | 플러그인 디렉토리 경로 |
| `${CLAUDE_PLUGIN_DATA}` | 플러그인 훅 | 플러그인 데이터 디렉토리 경로 |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd 훅 | SessionEnd 훅의 설정 가능한 시간 초과(밀리초, 기본값 재정의) |
| `CLAUDE_CODE_SESSION_ID` | Bash 도구 하위 프로세스 (v2.1.132+) | 세션 UUID; 훅 입력 JSON의 `session_id` 필드와 일치. Bash 로그를 훅 텔레메트리와 연관 짓는 데 사용 |
| `CLAUDE_EFFORT` | Bash 도구 하위 프로세스 (v2.1.133+) | 활성 노력 수준 (`low`/`medium`/`high`/`xhigh`/`max`); 훅 입력 JSON의 `effort.level`과 일치 |
| `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP` | 프로세스 전체 (v2.1.143+) | 세션이 경고와 함께 종료되기 전 최대 연속 Stop 훅 블록 수 (기본값 `8`). `0`으로 설정하면 캡 비활성화 |

## 프롬프트 기반 훅

`Stop` 및 `SubagentStop` 이벤트의 경우 LLM 기반 평가를 사용할 수 있습니다:

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review if all tasks are complete. Return your decision.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

**LLM 응답 스키마:**
```json
{
  "decision": "approve",
  "reason": "All tasks completed successfully",
  "continue": false,
  "stopReason": "Task complete"
}
```

## 예제

### 예제 1: Bash 명령어 검증기 (PreToolUse)

**파일:** `.claude/hooks/validate-bash.py`

```python
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"\brm\s+-rf\s+/", "Blocking dangerous rm -rf / command"),
    (r"\bsudo\s+rm", "Blocking sudo rm command"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name != "Bash":
        sys.exit(0)

    command = input_data.get("tool_input", {}).get("command", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, command):
            print(message, file=sys.stderr)
            sys.exit(2)  # Exit 2 = blocking error

    sys.exit(0)

if __name__ == "__main__":
    main()
```

**설정:**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\""
          }
        ]
      }
    ]
  }
}
```

### 예제 2: 보안 스캐너 (PostToolUse)

**파일:** `.claude/hooks/security-scan.py`

```python
#!/usr/bin/env python3
import json
import sys
import re

SECRET_PATTERNS = [
    (r"password\s*=\s*['\"][^'\"]+['\"]", "Potential hardcoded password"),
    (r"api[_-]?key\s*=\s*['\"][^'\"]+['\"]", "Potential hardcoded API key"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name not in ["Write", "Edit"]:
        sys.exit(0)

    tool_input = input_data.get("tool_input", {})
    content = tool_input.get("content", "") or tool_input.get("new_string", "")
    file_path = tool_input.get("file_path", "")

    warnings = []
    for pattern, message in SECRET_PATTERNS:
        if re.search(pattern, content, re.IGNORECASE):
            warnings.append(message)

    if warnings:
        output = {
            "hookSpecificOutput": {
                "hookEventName": "PostToolUse",
                "additionalContext": f"Security warnings for {file_path}: " + "; ".join(warnings)
            }
        }
        print(json.dumps(output))

    sys.exit(0)

if __name__ == "__main__":
    main()
```

### 예제 3: 자동 코드 포맷 (PostToolUse)

**파일:** `.claude/hooks/format-code.sh`

```bash
#!/bin/bash

# Read JSON from stdin
INPUT=$(cat)
TOOL_NAME=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_name', ''))")
FILE_PATH=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_input', {}).get('file_path', ''))")

if [ "$TOOL_NAME" != "Write" ] && [ "$TOOL_NAME" != "Edit" ]; then
    exit 0
fi

# Format based on file extension
case "$FILE_PATH" in
    *.js|*.jsx|*.ts|*.tsx|*.json)
        command -v prettier &>/dev/null && prettier --write "$FILE_PATH" 2>/dev/null
        ;;
    *.py)
        command -v black &>/dev/null && black "$FILE_PATH" 2>/dev/null
        ;;
    *.go)
        command -v gofmt &>/dev/null && gofmt -w "$FILE_PATH" 2>/dev/null
        ;;
esac

exit 0
```

### 예제 4: 프롬프트 검증기 (UserPromptSubmit)

**파일:** `.claude/hooks/validate-prompt.py`

```python
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"delete\s+(all\s+)?database", "Dangerous: database deletion"),
    (r"rm\s+-rf\s+/", "Dangerous: root deletion"),
]

def main():
    input_data = json.load(sys.stdin)
    prompt = input_data.get("user_prompt", "") or input_data.get("prompt", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, prompt, re.IGNORECASE):
            output = {
                "decision": "block",
                "reason": f"Blocked: {message}"
            }
            print(json.dumps(output))
            sys.exit(0)

    sys.exit(0)

if __name__ == "__main__":
    main()
```

### 예제 5: 지능형 Stop 훅 (프롬프트 기반)

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review if Claude completed all requested tasks. Check: 1) Were all files created/modified? 2) Were there unresolved errors? If incomplete, explain what's missing.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### 예제 6: 컨텍스트 사용량 트래커 (훅 쌍)

`UserPromptSubmit`(메시지 전) 및 `Stop`(응답 후) 훅을 함께 사용하여 요청당 토큰 소비를 추적합니다.

**파일:** `.claude/hooks/context-tracker.py`

```python
#!/usr/bin/env python3
"""
Context Usage Tracker - Tracks token consumption per request.

Uses UserPromptSubmit as "pre-message" hook and Stop as "post-response" hook
to calculate the delta in token usage for each request.

Token Counting Methods:
1. Character estimation (default): ~4 chars per token, no dependencies
2. tiktoken (optional): More accurate (~90-95%), requires: pip install tiktoken
"""
import json
import os
import sys
import tempfile

# Configuration
CONTEXT_LIMIT = 128000  # Claude's context window (adjust for your model)
USE_TIKTOKEN = False    # Set True if tiktoken is installed for better accuracy


def get_state_file(session_id: str) -> str:
    """Get temp file path for storing pre-message token count, isolated by session."""
    return os.path.join(tempfile.gettempdir(), f"claude-context-{session_id}.json")


def count_tokens(text: str) -> int:
    """
    Count tokens in text.

    Uses tiktoken with p50k_base encoding if available (~90-95% accuracy),
    otherwise falls back to character estimation (~80-90% accuracy).
    """
    if USE_TIKTOKEN:
        try:
            import tiktoken
            enc = tiktoken.get_encoding("p50k_base")
            return len(enc.encode(text))
        except ImportError:
            pass  # Fall back to estimation

    # Character-based estimation: ~4 characters per token for English
    return len(text) // 4


def read_transcript(transcript_path: str) -> str:
    """Read and concatenate all content from transcript file."""
    if not transcript_path or not os.path.exists(transcript_path):
        return ""

    content = []
    with open(transcript_path, "r") as f:
        for line in f:
            try:
                entry = json.loads(line.strip())
                # Extract text content from various message formats
                if "message" in entry:
                    msg = entry["message"]
                    if isinstance(msg.get("content"), str):
                        content.append(msg["content"])
                    elif isinstance(msg.get("content"), list):
                        for block in msg["content"]:
                            if isinstance(block, dict) and block.get("type") == "text":
                                content.append(block.get("text", ""))
            except json.JSONDecodeError:
                continue

    return "\n".join(content)


def handle_user_prompt_submit(data: dict) -> None:
    """Pre-message hook: Save current token count before request."""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # Save to temp file for later comparison
    state_file = get_state_file(session_id)
    with open(state_file, "w") as f:
        json.dump({"pre_tokens": current_tokens}, f)


def handle_stop(data: dict) -> None:
    """Post-response hook: Calculate and report token delta."""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # Load pre-message count
    state_file = get_state_file(session_id)
    pre_tokens = 0
    if os.path.exists(state_file):
        try:
            with open(state_file, "r") as f:
                state = json.load(f)
                pre_tokens = state.get("pre_tokens", 0)
        except (json.JSONDecodeError, IOError):
            pass

    # Calculate delta
    delta_tokens = current_tokens - pre_tokens
    remaining = CONTEXT_LIMIT - current_tokens
    percentage = (current_tokens / CONTEXT_LIMIT) * 100

    # Report usage
    method = "tiktoken" if USE_TIKTOKEN else "estimated"
    print(f"Context ({method}): ~{current_tokens:,} tokens ({percentage:.1f}% used, ~{remaining:,} remaining)", file=sys.stderr)
    if delta_tokens > 0:
        print(f"This request: ~{delta_tokens:,} tokens", file=sys.stderr)


def main():
    data = json.load(sys.stdin)
    event = data.get("hook_event_name", "")

    if event == "UserPromptSubmit":
        handle_user_prompt_submit(data)
    elif event == "Stop":
        handle_stop(data)

    sys.exit(0)


if __name__ == "__main__":
    main()
```

**설정:**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ]
  }
}
```

**작동 방식:**
1. `UserPromptSubmit`이 프롬프트가 처리되기 전에 실행 — 현재 토큰 수 저장
2. `Stop`이 Claude가 응답한 후 실행 — 델타 계산 및 사용량 보고
3. 각 세션은 임시 파일 이름의 `session_id`를 통해 격리됨

**토큰 계산 방법:**

| 방법 | 정확도 | 의존성 | 속도 |
|--------|----------|--------------|-------|
| 문자 추정 | ~80-90% | 없음 | <1ms |
| tiktoken (p50k_base) | ~90-95% | `pip install tiktoken` | <10ms |

> **참고:** Anthropic은 공식 오프라인 토크나이저를 출시하지 않았습니다. 두 방법 모두 근사치입니다. 기록에는 사용자 프롬프트, Claude의 응답 및 도구 출력이 포함되지만 시스템 프롬프트나 내부 컨텍스트는 포함되지 않습니다.

### 예제 7: 시드 자동 모드 권한 (일회성 설정 스크립트)

Claude Code의 자동 모드 기준과 동일한 약 67개의 안전한 권한 규칙을 `~/.claude/settings.json`에 시드하는 일회성 설정 스크립트 — 훅 없이, 미래 선택을 기억하지 않습니다. 한 번 실행하세요; 다시 실행해도 안전합니다(이미 있는 규칙은 건너뜁니다).

**파일:** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 추가될 내용 미리보기
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 적용
python3 09-advanced-features/setup-auto-mode-permissions.py
```

**추가되는 내용:**

| 카테고리 | 예시 |
|----------|---------|
| 내장 도구 | `Read(*)`, `Edit(*)`, `Write(*)`, `Glob(*)`, `Grep(*)`, `Agent(*)`, `WebSearch(*)` |
| Git 읽기 | `Bash(git status:*)`, `Bash(git log:*)`, `Bash(git diff:*)` |
| Git 쓰기 (로컬) | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git checkout:*)` |
| 패키지 관리자 | `Bash(npm install:*)`, `Bash(pip install:*)`, `Bash(cargo build:*)` |
| 빌드 및 테스트 | `Bash(make:*)`, `Bash(pytest:*)`, `Bash(go test:*)` |
| 일반 셸 | `Bash(ls:*)`, `Bash(cat:*)`, `Bash(find:*)`, `Bash(cp:*)`, `Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`, `Bash(gh pr create:*)`, `Bash(gh issue list:*)` |

**의도적으로 제외된 것** (이 스크립트로 절대 추가되지 않음):
- `rm -rf`, `sudo`, 강제 푸시, `git reset --hard`
- `DROP TABLE`, `kubectl delete`, `terraform destroy`
- `npm publish`, `curl | bash`, 프로덕션 배포

### 예제 8: 학습 진행 로거 (SessionEnd)

각 Claude Code 세션 종료 시 학습한 모듈을 기록합니다. 진행 상황은 `~/.claude-howto-progress.json`에 저장됩니다 — 저장소 외부에 있으므로 `git pull` 후에도 덮어쓰이지 않습니다.

**왜 `Stop`이 아니라 `SessionEnd`인가?**
`Stop`은 *모든* Claude 응답 후에 실행됩니다. `SessionEnd`는 세션이 종료될 때 한 번 실행됩니다 — 세션 종료 일기 항목에 정확히 필요한 것입니다.

**왜 입력에 `/dev/tty`를 사용하는가?**
훅 스크립트는 `stdin`을 통해 JSON 훅 페이로드를 수신하므로, 대화형 `read`는 터미널에 도달하기 위해 `/dev/tty`를 직접 사용해야 합니다.

**파일:** `06-hooks/session-end.sh`

```bash
#!/usr/bin/env bash
# SessionEnd hook: prompts for modules worked on, then appends a session record
# to ~/.claude-howto-progress.json for persistent learning progress tracking.

PROGRESS_FILE="$HOME/.claude-howto-progress.json"

# Guard: only run inside this repo
if [[ "$CLAUDE_PROJECT_DIR" != *"claude-howto"* ]] && [[ "$PWD" != *"claude-howto"* ]]; then
  exit 0
fi

if [ ! -f "$PROGRESS_FILE" ]; then
  echo '{"sessions":[]}' > "$PROGRESS_FILE"
fi

DATE=$(date +"%Y-%m-%d")
TIME=$(date +"%H:%M")

echo ""
echo " Which modules did you work on? (e.g. 06,07 or press Enter to skip)"
echo " 01=Slash  02=Memory  03=Skills  04=Subagents  05=MCP"
echo " 06=Hooks  07=Plugins 08=Checkpoints 09=Advanced 10=CLI"
printf " > "
read -r INPUT </dev/tty

if [ -z "$INPUT" ] || [ "$INPUT" = "skip" ]; then
  exit 0
fi

MODULES_JSON=$(echo "$INPUT" | tr ',' '\n' | tr -d ' ' | while read -r m; do
  case "$m" in
    01) echo '"01-slash-commands"' ;;
    02) echo '"02-memory"' ;;
    03) echo '"03-skills"' ;;
    04) echo '"04-subagents"' ;;
    05) echo '"05-mcp"' ;;
    06) echo '"06-hooks"' ;;
    07) echo '"07-plugins"' ;;
    08) echo '"08-checkpoints"' ;;
    09) echo '"09-advanced-features"' ;;
    10) echo '"10-cli"' ;;
    *)  echo "\"$m\"" ;;
  esac
done | paste -sd ',' -)

printf " Notes? (optional, press Enter to skip): "
read -r NOTES </dev/tty

# Pass NOTES as a separate argument so Python handles JSON escaping —
# avoids broken JSON when notes contain quotes or backslashes.
python3 - "$PROGRESS_FILE" "$DATE" "$TIME" "$MODULES_JSON" "$NOTES" <<'PYEOF'
import sys, json

path, date, time_str, modules_raw, notes = sys.argv[1], sys.argv[2], sys.argv[3], sys.argv[4], sys.argv[5]

new_session = {
    "date": date,
    "time": time_str,
    "modules": json.loads(f"[{modules_raw}]") if modules_raw else [],
    "notes": notes,
}

with open(path, 'r') as f:
    data = json.load(f)

data.setdefault('sessions', []).append(new_session)

with open(path, 'w') as f:
    json.dump(data, f, indent=2)
PYEOF

echo " Saved to $PROGRESS_FILE"
```

**설치** — 스크립트를 프로젝트의 훅 디렉토리에 복사하여 `settings.json`의 경로가 확인되도록:

```bash
mkdir -p .claude/hooks
cp 06-hooks/session-end.sh .claude/hooks/
chmod +x .claude/hooks/session-end.sh
```

**설정** (`.claude/settings.json`에서):

```json
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-end.sh\""
          }
        ]
      }
    ]
  }
}
```

**출력 — `~/.claude-howto-progress.json`:**

```json
{
  "sessions": [
    {
      "date": "2026-04-18",
      "time": "14:32",
      "modules": ["06-hooks", "07-plugins"],
      "notes": "Installed first hook, tried pre-commit example"
    }
  ]
}
```

**주요 패턴:**

| 패턴 | 중요한 이유 |
|---------|----------------|
| `SessionEnd` 이벤트 | 종료 시 한 번 실행 — `Stop`처럼 모든 응답 후가 아님 |
| `read -r INPUT </dev/tty` | 훅이 `stdin`(JSON 페이로드)을 소유; 사용자 입력에는 `/dev/tty` 사용 |
| `$CLAUDE_PROJECT_DIR` | 이식 가능한 경로 — 절대 경로 하드코딩 금지 |
| 상단의 가드 절 | 전역 설치 시 관련 없는 프로젝트에서 훅 실행 방지 |
| 저장소 외부에 저장 | `~/` 경로가 `git pull` 후에도 데이터를 보존 |

**함께 사용: 시각적 진행 트래커**

전체 10개 모듈을 다루는 체크박스 기반 UI를 위해 포함된 트래커를 브라우저에서 여세요:

```bash
open local-progress/index.html
```

진행 상황은 브라우저 `localStorage`에 저장됩니다(저장소 내 디스크에 기록되지 않음). **내보내기** 버튼을 사용하여 JSON 스냅샷을 저장하고, **가져오기**로 복원하세요.

## 플러그인 훅

플러그인은 `hooks/hooks.json` 파일에 훅을 포함할 수 있습니다:

**파일:** `plugins/hooks/hooks.json`

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/validate.sh"
          }
        ]
      }
    ]
  }
}
```

**플러그인 훅의 환경 변수:**
- `${CLAUDE_PLUGIN_ROOT}` - 플러그인 디렉토리 경로
- `${CLAUDE_PLUGIN_DATA}` - 플러그인 데이터 디렉토리 경로

이를 통해 플러그인이 사용자 정의 검증 및 자동화 훅을 포함할 수 있습니다.

## MCP 도구 훅

MCP 도구는 `mcp__<server>__<tool>` 패턴을 따릅니다:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "mcp__memory__.*",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"systemMessage\": \"Memory operation logged\"}'"
          }
        ]
      }
    ]
  }
}
```

## 보안 고려 사항

### 면책 조항

**사용에 따른 책임**: 훅은 임의의 셸 명령어를 실행합니다. 다음은 전적으로 귀하의 책임입니다:
- 구성하는 명령어
- 파일 접근/수정 권한
- 잠재적 데이터 손실 또는 시스템 손상
- 프로덕션 사용 전 안전한 환경에서 훅 테스트

### 보안 참고 사항

- **작업 공간 신뢰 필요:** `statusLine` 및 `fileSuggestion` 훅 출력 명령어는 이제 작업 공간 신뢰 수락 후에만 적용됩니다.
- **상태 줄 터미널 크기 (v2.1.153):** 상태 줄 명령어 스크립트는 이제 `COLUMNS` 및 `LINES` 환경 변수를 수신하므로 스크립트가 터미널 너비/높이에 맞게 출력을 조정할 수 있습니다 (예: `[ "$COLUMNS" -lt 80 ] && short_output`).
- **HTTP 훅 및 환경 변수:** HTTP 훅은 URL의 환경 변수 보간을 사용하려면 명시적인 `allowedEnvVars` 목록이 필요합니다. 이는 민감한 환경 변수가 원격 엔드포인트로 우발적으로 유출되는 것을 방지합니다.
- **관리형 설정 계층:** `disableAllHooks` 설정은 이제 관리형 설정 계층을 존중하므로, 조직 수준 설정이 개별 사용자가 재정의할 수 없는 훅 비활성화를 적용할 수 있습니다.
- **PowerShell 자동 승인 (v2.1.119):** PowerShell 도구 명령어는 Bash와 일치하게 권한 모드에서 자동 승인될 수 있습니다. 이는 Windows 사용자가 PowerShell 기반 셸 도구로 Claude Code를 실행할 때 동등성을 제공합니다.
- **Bash 단순 env-var 자동 승인 종료 (v2.1.145):** v2.1.145 이전에는 `FOO=bar somecommand` 형식의 Bash 명령어(허용 목록에 없는 명령어와 함께 인라인 변수 할당)가 `FOO=bar` 단독으로 허용 목록에 있을 때 자동 승인될 수 있었습니다. v2.1.145에서 이를 종료했습니다 — 이러한 명령어는 이제 권한 프롬프트를 표시합니다. 암시적 허용에 의존하던 스크립트는 프롬프트를 표시하기 시작합니다; 변수 할당뿐만 아니라 전체 명령어를 포함하는 `Bash(...)` 권한 규칙을 통해 명시적으로 다시 허용하세요.

### 모범 사례

| Do | Don't |
|-----|-------|
| 모든 입력 검증 및 정화 | 입력 데이터를 맹목적으로 신뢰 |
| 셸 변수 인용: `"$VAR"` | 인용 없이 사용: `$VAR` |
| 경로 순회 차단 (`..`) | 임의 경로 허용 |
| `$CLAUDE_PROJECT_DIR`로 절대 경로 사용 | 경로 하드코딩 |
| 민감한 파일 건너뛰기 (`.env`, `.git/`, 키) | 모든 파일 처리 |
| 먼저 격리된 환경에서 훅 테스트 | 테스트되지 않은 훅 배포 |
| HTTP 훅에 명시적 `allowedEnvVars` 사용 | 모든 env var를 웹훅에 노출 |

## 디버깅

### 디버그 모드 활성화

디버그 플래그로 Claude를 실행하여 상세한 훅 로그 확인:

```bash
claude --debug
```

### 상세 모드

Claude Code에서 `Ctrl+O`를 사용하여 상세 모드를 활성화하고 훅 실행 진행 상황을 확인하세요.

### 훅 독립적으로 테스트

```bash
# 샘플 JSON 입력으로 테스트
echo '{"tool_name": "Bash", "tool_input": {"command": "ls -la"}}' | python3 .claude/hooks/validate-bash.py

# 종료 코드 확인
echo $?
```

## 완전한 설정 예시

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/format-code.sh\"",
            "timeout": 30
          },
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py\""
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "matcher": "startup",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-init.sh\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Verify all tasks are complete before stopping.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

## 훅 실행 세부 사항

| 측면 | 동작 |
|--------|----------|
| **시간 초과** | 기본 60초, 명령어별 설정 가능 |
| **병렬화** | 일치하는 모든 훅이 병렬로 실행 |
| **중복 제거** | 동일한 훅 명령어는 중복 제거됨 |
| **환경** | Claude Code의 환경으로 현재 디렉토리에서 실행 |

## 문제 해결

### 훅이 실행되지 않음
- JSON 설정 구문이 올바른지 확인
- 매처 패턴이 도구 이름과 일치하는지 확인
- 스크립트가 존재하고 실행 가능한지 확인: `chmod +x script.sh`
- `claude --debug` 실행하여 훅 실행 로그 확인
- 훅이 stdin에서 JSON을 읽는지 확인(명령어 인자가 아님)

### 훅이 예기치 않게 차단됨
- 샘플 JSON으로 훅 테스트: `echo '{"tool_name": "Write", ...}' | ./hook.py`
- 종료 코드 확인: 0은 허용, 2는 차단
- stderr 출력 확인 (종료 코드 2에서 표시됨)

### JSON 파싱 오류
- 항상 stdin에서 읽기, 명령어 인자에서 읽지 않음
- 적절한 JSON 파싱 사용(문자열 조작 아님)
- 누락된 필드를 우아하게 처리

## 설치

### 1단계: 훅 디렉토리 생성
```bash
mkdir -p ~/.claude/hooks
```

### 2단계: 예제 훅 복사
```bash
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

### 3단계: 설정에서 구성
`~/.claude/settings.json` 또는 `.claude/settings.json`을 위의 훅 설정으로 편집하세요.

## 관련 개념

- **[체크포인트 및 되감기](../08-checkpoints/)** - 대화 상태 저장 및 복원
- **[슬래시 명령어](../01-slash-commands/)** - 사용자 정의 슬래시 명령어 생성
- **[스킬](../03-skills/)** - 재사용 가능한 자율 기능
- **[서브에이전트](../04-subagents/)** - 위임된 작업 실행
- **[플러그인](../07-plugins/)** - 번들 확장 패키지
- **[고급 기능](../09-advanced-features/)** - 고급 Claude Code 기능 탐색

## 추가 자료

- **[공식 훅 문서](https://code.claude.com/docs/en/hooks)** - 완전한 훅 참조
- **[CLI 참조](https://code.claude.com/docs/en/cli-reference)** - 명령줄 인터페이스 문서
- **[메모리 가이드](../02-memory/)** - 영구 컨텍스트 설정

---

**마지막 업데이트**: 2026년 6월 28일
**Claude Code 버전**: 2.1.195
**출처**:
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/permissions
- https://code.claude.com/docs/en/changelog
- https://code.claude.com/docs/en/changelog#2-1-176
- https://github.com/anthropics/claude-code/releases/tag/v2.1.139
- https://github.com/anthropics/claude-code/releases/tag/v2.1.145
- https://github.com/anthropics/claude-code/releases/tag/v2.1.152
- https://github.com/anthropics/claude-code/releases/tag/v2.1.153
**호환 모델**: Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
