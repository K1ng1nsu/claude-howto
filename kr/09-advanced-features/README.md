<!-- i18n-source: 09-advanced-features/README.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# 고급 기능

Claude Code의 고급 기능에 대한 포괄적인 가이드로, planning mode, extended thinking, auto mode, background tasks, permission modes, print mode (non-interactive), session management, interactive features, channels, voice dictation, remote control, web sessions, desktop app, task list, prompt suggestions, git worktrees, sandboxing, managed settings, 설정을 다룹니다.

## 목차

1. [개요](#개요)
2. [Planning Mode](#planning-mode)
3. [Ultraplan (클라우드 계획 초안 작성)](#ultraplan-클라우드-계획-초안-작성)
4. [Extended Thinking](#extended-thinking)
5. [Auto Mode](#auto-mode)
6. [Background Tasks](#background-tasks)
7. [Monitor Tool (이벤트 기반 스트림)](#monitor-tool-이벤트-기반-스트림)
8. [동적 워크플로우](#동적-워크플로우)
9. [예약 작업](#예약-작업)
10. [Permission Modes](#permission-modes)
11. [Headless Mode](#headless-mode)
12. [세션 관리](#세션-관리)
13. [대화형 기능](#대화형-기능)
14. [TUI 모드 (전체 화면)](#tui-모드-전체-화면)
15. [음성 받아쓰기](#음성-받아쓰기)
16. [Channels](#channels)
17. [Chrome 통합](#chrome-통합)
18. [원격 제어](#원격-제어)
19. [웹 세션](#웹-세션)
20. [데스크탑 앱](#데스크탑-앱)
21. [작업 목록](#작업-목록)
22. [프롬프트 제안](#프롬프트-제안)
23. [Git Worktrees](#git-worktrees)
24. [샌드박싱](#샌드박싱)
25. [관리형 설정 (엔터프라이즈)](#관리형-설정-엔터프라이즈)
26. [설정](#설정)
27. [에이전트 팀](#에이전트-팀)
28. [모범 사례](#모범-사례)
29. [추가 자료](#추가-자료)

---

## 개요

Claude Code의 고급 기능은 계획, 추론, 자동화 및 제어 메커니즘으로 핵심 기능을 확장합니다. 이러한 기능은 복잡한 개발 작업, 코드 리뷰, 자동화 및 다중 세션 관리를 위한 정교한 워크플로우를 가능하게 합니다.

**주요 고급 기능:**
- **Planning Mode**: 코딩 전에 상세한 구현 계획 생성
- **Extended Thinking**: 복잡한 문제에 대한 심층 추론
- **Auto Mode**: 백그라운드 안전 분류기가 각 작업을 실행 전에 검토 (Research Preview)
- **Background Tasks**: 대화를 차단하지 않고 장기 작업 실행
- **Permission Modes**: Claude가 할 수 있는 작업 제어 (`default`, `acceptEdits`, `plan`, `auto`, `dontAsk`, `bypassPermissions`)
- **Print Mode**: 자동화 및 CI/CD를 위해 Claude Code를 비대화형으로 실행 (`claude -p`)
- **세션 관리**: 여러 작업 세션 관리
- **대화형 기능**: 키보드 단축키, 여러 줄 입력, 명령어 기록
- **음성 받아쓰기**: 20개 언어 STT 지원으로 푸시투톡 음성 입력
- **Channels**: MCP 서버가 실행 중인 세션으로 메시지 전송 (Research Preview)
- **원격 제어**: Claude.ai 또는 Claude 앱에서 Claude Code 제어
- **웹 세션**: claude.ai/code에서 브라우저로 Claude Code 실행
- **데스크탑 앱**: 시각적 diff 리뷰 및 여러 세션을 위한 독립 실행형 앱
- **작업 목록**: 컨텍스트 압축 전반에 걸친 지속적 작업 추적
- **프롬프트 제안**: 컨텍스트 기반 스마트 명령어 제안
- **Git Worktrees**: 병렬 작업을 위한 격리된 worktree 브랜치
- **샌드박싱**: OS 수준의 파일 시스템 및 네트워크 격리
- **관리형 설정**: plist, Registry 또는 관리 파일을 통한 엔터프라이즈 배포
- **설정**: JSON 설정 파일로 동작 사용자 정의

---

## Planning Mode

Planning mode를 사용하면 Claude가 구현 전에 복잡한 작업을 생각하고, 검토 및 승인할 수 있는 상세한 계획을 만들 수 있습니다.

### Planning Mode란?

Planning mode는 두 단계 접근 방식입니다:
1. **계획 단계**: Claude가 작업을 분석하고 상세한 구현 계획 생성
2. **구현 단계**: 승인 후 Claude가 계획 실행

### Planning Mode 사용 시기

✅ Planning mode 사용:
- 복잡한 다중 파일 리팩토링
- 새 기능 구현
- 아키텍처 변경
- 데이터베이스 마이그레이션
- 주요 API 재설계

❌ Planning mode를 사용하지 말아야 하는 경우:
- 간단한 버그 수정
- 포맷팅 변경
- 단일 파일 편집
- 빠른 질문

### Planning Mode 활성화

**Slash command**:
```bash
/plan 사용자 인증 시스템 구현
```

**CLI 플래그**:
```bash
claude --permission-mode plan
```

**기본값으로 설정**:
```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

**키보드 단축키**:
- `Shift + Tab` - Permission modes 전환 (plan 포함)
- `Alt + M` - Permission modes 전환 (Windows/Linux)

### Planning Mode의 이점

- **구조화된 구현 접근 방식**: 구현 단계의 상세한 분석
- **검토 및 승인**: 실행 전 계획 승인 또는 조정
- **위험 식별**: 구현 전 잠재적 문제 식별
- **명확한 단계**: 체계적인 구현 단계 및 마일스톤

### 예제: 기능 구현

```
User: /plan 실시간 알림 시스템 추가

Claude: 실시간 알림 구현 계획을 세우겠습니다.

## 구현 계획

### 1단계: 인프라 설정
1. WebSocket 라이브러리 설치 (socket.io)
2. WebSocket 서버 설정
3. 연결 관리 구성

### 2단계: 백엔드 구현
4. 알림 서비스 생성
5. 알림 큐 구현
6. 알림용 데이터베이스 스키마 추가
7. API 엔드포인트 생성

### 3단계: 프론트엔드 통합
8. WebSocket 클라이언트 설정
9. 알림 UI 컴포넌트 생성
10. 알림 상태 관리 구현
11. 알림 권한 추가

### 4단계: 테스트 및 배포
12. 단위 테스트 작성
13. 통합 테스트 작성
14. 문서 업데이트
15. 스테이징에 배포

예상 시간: 3-4시간
수정할 파일: 12개
생성할 새 파일: 8개

계속 진행하시겠습니까? (yes/no/modify)

User: yes

Claude: [계획에 따라 구현 시작]
```

### 계획 수정

```
User: 계획 수정 - 큐는 지금 건너뛰고 나중에 추가합시다

Claude: 업데이트된 계획:
[큐가 제거된 수정된 계획 표시]

User: 좋아요, 진행해 주세요

Claude: [수정된 계획 구현]
```

### Planning Mode 설정

Planning mode는 CLI 플래그 또는 slash command로 활성화됩니다:

```bash
# CLI를 통해 plan 모드 활성화
claude --permission-mode plan

# 또는 REPL 내에서 /plan slash command 사용
/plan 사용자 인증 시스템 구현
```

**계획용 모델 별칭**: `opusplan`을 모델 별칭으로 사용하여 계획에는 Opus를, 실행에는 Sonnet을 사용:

```bash
claude --model opusplan "새 API 설계 및 구현"
```

**외부에서 계획 편집**: `Ctrl+G`를 누르면 현재 계획이 외부 편집기에서 열려 상세 수정이 가능합니다.

> **v2.1.112 업데이트**: 계획 파일이 이제 (무작위 단어 대신) 해당 계획을 생성한 프롬프트의 이름을 따서 명명되어, 탐색 및 재사용이 더 쉬워졌습니다.

> **v2.1.136 업데이트 — plan-mode 쓰기 차단은 무조건적**: Plan 모드는 이제 모든 파일 쓰기를 차단합니다. 여기에는 `permissions.allow`에 일치하는 `Edit(...)` 규칙이 있는 경우도 포함됩니다. 이전에는 허용적인 `Edit(...)` 규칙이 plan 모드에서 쓰기를 통과시킬 수 있었습니다. 해당 우회는 차단되었습니다. 이전 동작에 의존하는 워크플로우가 있다면 편집 전에 plan 모드를 종료하세요(`Shift+Tab`).

---

## Ultraplan (클라우드 계획 초안 작성)

> **v2.1.101 신규**: Ultraplan이 처음 호출 시 웹 클라우드 환경에 Claude Code를 자동 생성합니다 — 수동 설정이나 초안 시작 전 컨테이너 준비 대기 시간이 없습니다.

> **참고**: Ultraplan은 research preview이며 Claude Code v2.1.91 이상이 필요합니다.

`/ultraplan`은 로컬 CLI의 계획 작업을 plan 모드로 실행되는 웹 세션의 Claude Code에 전달합니다. Claude가 클라우드에서 계획을 작성하는 동안 터미널은 다른 작업에 자유롭게 사용할 수 있으며, 그런 다음 브라우저에서 초안을 검토하고 실행 위치를 선택합니다 — 동일한 클라우드 세션 또는 터미널로 다시 가져옵니다.

### Ultraplan 사용 시기

- 터미널보다 더 풍부한 검토 인터페이스를 원할 때: 인라인 댓글, 이모지 반응, 개요 사이드바, 지속적 기록
- 로컬에서 코딩을 계속하는 동안 계획 작성을 맡기고 싶을 때 — 클라우드 세션이 저장소를 조사하고 CLI를 차단하지 않고 계획을 작성
- 실행 전에 이해관계자 검토가 필요한 계획 — 공유 가능한 웹 URL이 터미널 스크롤백을 붙여넣는 것보다 나음

### 요구사항

- Claude Code on the web 계정
- GitHub 저장소 (클라우드 세션이 실제 코드에 대해 계획을 작성하기 위해 저장소를 클론)
- **Amazon Bedrock, Google Cloud Vertex AI, Microsoft Foundry에서는 사용 불가**

### 세 가지 실행 방법

- **명령어**: `/ultraplan <prompt>` — 명시적 호출
- **키워드**: 일반 프롬프트에 `ultraplan` 단어를 포함하면 Claude가 요청을 클라우드로 라우팅
- **로컬 계획에서**: Claude가 로컬에서 계획을 완료한 후 승인 대화상자에서 "아니요, Claude Code on the web에서 Ultraplan으로 개선"을 선택하여 초안 작성을 맡김

### 사용 예시

```bash
/ultraplan 인증 서비스를 세션에서 JWT로 마이그레이션
```

Claude가 확인하고, 클라우드 환경을 시작한 후(v2.1.101+에서 첫 실행 시 자동 생성) 브라우저에서 열 수 있는 세션 링크를 반환합니다.

### 상태 표시기

| 상태 | 의미 |
|------|------|
| `◇ ultraplan` | Claude가 코드베이스를 조사하고 계획을 작성 중 |
| `◇ ultraplan needs your input` | Claude에게 명확화 질문이 있음; 세션 링크를 열어 응답 |
| `◆ ultraplan ready` | 브라우저에서 검토할 계획 준비 완료 |

### 실행 옵션

계획이 준비되면 두 가지 실행 경로가 있습니다. 브라우저에서 계획을 승인하면 동일한 클라우드 세션에서 실행됩니다 — Claude가 원격으로 변경 사항을 구현하고 웹 UI에서 pull request를 엽니다. 또는 "계획 승인하고 터미널로 다시 가져오기"를 선택하여 로컬에서 구현합니다. 터미널 텔레포트 대화상자에서 세 가지 선택이 가능합니다:

- **여기서 구현** — 현재 터미널 세션에서 승인된 계획 실행
- **새 세션 시작** — 동일한 작업 디렉토리에서 새 세션을 열어 구현
- **취소** — 계획을 파일에 저장하여 나중에 다시 사용

> **경고**: ultraplan이 시작되면 Remote Control이 연결 해제됩니다. 두 기능 모두 claude.ai/code 인터페이스를 공유하므로 한 번에 하나만 활성화될 수 있습니다.

---

## Extended Thinking

Extended thinking을 사용하면 Claude가 솔루션을 제공하기 전에 복잡한 문제에 대해 더 많은 시간을 추론할 수 있습니다.

### Extended Thinking이란?

Extended thinking은 의도적이고 단계별 추론 프로세스로, Claude가:
- 복잡한 문제 분석
- 여러 접근 방식 고려
- 장단점 평가
- 엣지 케이스 추론

### Extended Thinking 활성화

**키보드 단축키**:
- `Option + T` (macOS) / `Alt + T` (Windows/Linux) - Extended thinking 전환

**자동 활성화**:
- 모든 모델에 기본 활성화됨 (Opus 4.8, Opus 4.7, Sonnet 4.6, Haiku 4.5)
- Opus 4.8: 노력 수준이 있는 적응형 추론: `low` (○), `medium` (◐), `high` (●), `xhigh`, `max`. Opus 4.8(v2.1.154), Opus 4.6, Sonnet 4.6의 기본값은 `high`, Opus 4.7의 기본값은 `xhigh`. `xhigh`는 Opus 4.8 및 Opus 4.7에서 사용 가능(Opus 4.6/Sonnet 4.6에서는 `high`로 대체). `max`는 Opus 4.8/4.7/4.6 및 Sonnet 4.6에서 작동(세션 전용). Haiku 4.5에는 노력 수준이 없음. Opus 4.8과 Opus 4.7은 1M 토큰 네이티브 컨텍스트 창 보유(v2.1.117에서 1M 컨텍스트 수정 — 그 이전에는 `/context`가 Opus 4.7을 200K 창에 대해 잘못 계산하여 조기 자동 압축 유발). v2.1.129부터 `/context`는 UI 내에서만 시각화를 표시하며, ASCII 시각화가 더 이상 대화 컨텍스트에 누출되지 않음(호출당 ~1.6k 토큰 절약), 따라서 `/context`를 자유롭게 호출해도 안전
- Pro/Max 구독자가 Opus 4.6/Sonnet 4.6 사용 시: v2.1.117에서 기본 노력 수준이 `medium`에서 `high`로 상향
- 기타 모델: 최대 31,999 토큰의 고정 예산

**설정 방법**:
- 전환: `Alt+T` / `Option+T`, 또는 `/config`
- 추론 보기: `Ctrl+O` (상세 모드)
- 노력 설정: `/effort` 명령어 또는 `--effort` 플래그

**사용자 지정 예산**:
```bash
export MAX_THINKING_TOKENS=1024
```

**노력 수준** (Opus 4.8, Opus 4.7, Opus 4.6, Sonnet 4.6 지원 — Haiku 4.5는 미지원):
```bash
export CLAUDE_CODE_EFFORT_LEVEL=high   # low (○), medium (◐), high (●), xhigh (Opus 4.8/4.7), 또는 max — Opus 4.8 기본값은 high
```

**CLI 플래그**:
```bash
claude --effort high "복잡한 아키텍처 검토"
```

**Slash command**:
```
/effort high
```

> **참고**: 프롬프트의 "ultrathink" 키워드는 심층 추론 모드를 활성화합니다. 노력 수준 `low`, `medium`, `high`, `max`는 Opus 4.8, Opus 4.7, Opus 4.6, Sonnet 4.6에서 지원됨(Haiku 4.5는 없음). `xhigh`는 Opus 4.8 및 Opus 4.7에서 사용 가능. 기본 노력 수준은 Opus 4.8(및 Opus 4.6/Sonnet 4.6)에서 `high`, Opus 4.7에서 `xhigh`. `/effort` 메뉴는 `ultracode`도 제공하는데, 이는 **모델 노력 수준이 아님** — `xhigh`를 보내고 Claude가 동적 워크플로우를 오케스트레이션하도록 함(세션 전용).

### Extended Thinking의 이점

- **심층 추론**: 복잡한 문제에 대해 더 많은 시간을 생각
- **더 나은 아키텍처 결정**: 포괄적인 평가로 정보에 기반한 선택
- **엣지 케이스 고려**: 잠재적 문제 및 시나리오 검토
- **철저한 분석**: 복잡한 문제 해결을 위한 구조화된 접근 방식

### 예제: 아키텍처 결정

```
User: ultrathink 전자상거래 플랫폼에 마이크로서비스와 모놀리스 중 어떤 것을 사용해야 할까요?

Claude: 이 아키텍처 결정을 신중히 생각해 보겠습니다...

<extended_thinking>
## 맥락 분석
- 팀 규모: 5명의 개발자
- 예상 트래픽: 초기 일 10k 사용자
- 100k+ 사용자로 확장 필요
- 제한된 DevOps 리소스
- MVP까지 6개월 일정

## 접근 방식 1: 마이크로서비스
장점:
+ 독립적 확장
+ 기술 유연성
+ 팀 독립성
+ 장애 격리

단점:
- 운영 복잡성
- 네트워크 오버헤드
- 분산 추적 필요
- DevOps 전문성 필요
- 느린 초기 개발

## 접근 방식 2: 모놀리스
장점:
+ 간단한 배포
+ 빠른 초기 개발
+ 쉬운 디버깅
+ 낮은 운영 오버헤드
+ 단일 코드베이스

단점:
- 나중에 확장 문제
- 올오어나씽 배포
- 기술 종속
- 강한 결합 가능성

## 결정 요소
1. 팀 규모 (5명) → 모놀리스에 유리
2. DevOps 리소스 (제한적) → 모놀리스에 유리
3. 일정 (6개월) → 모놀리스에 유리
4. 미래 규모 (100k 사용자) → 모놀리스 + 캐싱으로 달성 가능
5. 복잡성 예산 → 제한적

## 권장
**모듈식 모놀리스**로 시작:
- 단일 배포 단위
- 명확한 모듈 경계
- 모듈 간 깔끔한 인터페이스
- 필요시 향후 마이크로서비스 추출 준비

이를 통해 얻는 것:
✓ 빠른 초기 개발
✓ 낮은 운영 복잡성
✓ 나중에 서비스 추출 옵션
✓ 명확한 아키텍처

마이그레이션 경로: 50k+ 사용자에 도달하거나 특정 모듈에 독립적 확장이 필요하면 해당 모듈을 마이크로서비스로 추출하세요.
</extended_thinking>

**권장**: 깔끔한 모듈 경계로 모듈식 모놀리스를 구현하세요.

이 접근 방식은 현재 제약 조건(팀 규모, 일정, DevOps 리소스)과 미래 확장성 요구사항의 균형을 맞춥니다. 명확한 비즈니스 필요가 생기면 나중에 특정 모듈을 마이크로서비스로 마이그레이션할 수 있습니다.
```

### Extended Thinking 설정

Extended thinking은 환경 변수, 키보드 단축키 및 CLI 플래그로 제어됩니다:

```bash
# 생각 토큰 예산 설정
export MAX_THINKING_TOKENS=16000

# 노력 수준 설정 (Opus 4.8, Opus 4.7, Opus 4.6, Sonnet 4.6): low (○), medium (◐), high (●), xhigh (Opus 4.8/4.7), 또는 max — Opus 4.8 기본값은 high
export CLAUDE_CODE_EFFORT_LEVEL=high
```

세션 중 `Alt+T` / `Option+T`로 전환, `/effort`로 노력 설정, 또는 `/config`로 설정합니다.

> **간결한 시스템 프롬프트 (v2.1.154):** 간결한 시스템 프롬프트가 이제 Haiku, Sonnet, Opus 4.7-이하를 제외한 모든 모델의 **기본값**이 되어, Opus 4.8의 기본 토큰 오버헤드를 줄입니다.

---

## Auto Mode

Auto Mode는 Research Preview 권한 모드(2026년 3월)로, 백그라운드 안전 분류기를 사용하여 각 작업을 실행 전에 검토합니다. Claude가 자율적으로 작업하면서 위험한 작업을 차단할 수 있게 합니다.

### 요구사항

- **요금제**: Team, Enterprise, 또는 API (Pro 또는 Max 요금제에서는 사용 불가)
- **모델**: Claude Sonnet 4.6 또는 Opus 4.8
- **제공자**: Anthropic API 전용 (Bedrock, Vertex, Foundry에서는 지원되지 않음)
- **분류기**: Claude Sonnet 4.6에서 실행 (추가 토큰 비용 발생)

### Auto Mode 활성화

```bash
# CLI 플래그로 auto mode 잠금 해제 (Opus 4.7의 Max 구독자에게는 더 이상 필요하지 않음 — 직접 접근 가능)
claude --enable-auto-mode

# 그런 다음 REPL에서 Shift+Tab으로 전환
```

> **v2.1.112 업데이트**: Auto mode에 더 이상 `--enable-auto-mode` 플래그가 필요하지 않습니다. Max 구독자는 Opus 4.7에서 직접 접근합니다.

> **v2.1.158 업데이트**: Auto mode가 이제 Opus 4.7/4.8에 대해 Bedrock, Vertex, Foundry에서 사용 가능 — `CLAUDE_CODE_ENABLE_AUTO_MODE=1`을 설정하여 **옵트인**하세요.

또는 기본 권한 모드로 설정:

```bash
claude --permission-mode auto
```

설정을 통한 설정:
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 분류기 작동 방식

백그라운드 분류기는 다음 결정 순서로 각 작업을 평가합니다:

1. **허용/거부 규칙** -- 명시적 권한 규칙이 먼저 확인됨
2. **읽기 전용/편집 자동 승인** -- 파일 읽기 및 편집이 자동으로 통과
3. **분류기** -- 백그라운드 분류기가 작업 검토
4. **폴백** -- 3회 연속 또는 총 20회 차단 후 프롬프트로 폴백

### 기본 차단 작업

Auto mode는 다음을 기본적으로 차단합니다:

| 차단된 작업 | 예시 |
|------------|------|
| 파이프투쉘 설치 | `curl \| bash` |
| 외부로 민감한 데이터 전송 | 네트워크를 통한 API 키, 자격증명 |
| 프로덕션 배포 | 프로덕션을 대상으로 하는 배포 명령어 |
| 대량 삭제 | 대규모 디렉토리에 `rm -rf` |
| IAM 변경 | 권한 및 역할 수정 |
| 메인 브랜치 강제 푸시 | `git push --force origin main` |

### 기본 허용 작업

| 허용된 작업 | 예시 |
|------------|------|
| 로컬 파일 작업 | 프로젝트 파일 읽기, 쓰기, 편집 |
| 선언된 의존성 설치 | manifest의 `npm install`, `pip install` |
| 읽기 전용 HTTP | 문서 가져오기 위한 `curl` |
| 현재 브랜치 푸시 | `git push origin feature-branch` |

### Auto Mode 설정

**기본 규칙을 JSON으로 출력**:
```bash
claude auto-mode defaults
```

**신뢰할 수 있는 인프라 설정**은 엔터프라이즈 배포를 위한 `autoMode.environment` 관리 설정을 통해 구성합니다. 이를 통해 관리자가 신뢰할 수 있는 CI/CD 환경, 배포 대상 및 인프라 패턴을 정의할 수 있습니다.

#### `"$defaults"`로 기본값 확장 (v2.1.118)

v2.1.118부터 `autoMode.allow`, `autoMode.soft_deny`, `autoMode.environment`는 `"$defaults"` 토큰을 허용하여 사용자 규칙을 기본 제공 목록을 **대체하지 않고 추가**합니다. v2.1.118 이전에는 사용자 정의 배열이 조용히 기본 제공 규칙을 덮어썼습니다.

#### `autoMode.hard_deny`로 무조건적 차단 (v2.1.136)

`autoMode.hard_deny`(v2.1.136+)는 **추론된 사용자 의도와 관계없이** 작업 클래스를 차단하는 분류기 규칙의 배열입니다. auto mode에서 절대 실행되어서는 안 되는 작업(예: 루트 경로의 `rm -rf` 또는 보호된 브랜치의 `git push --force`)에 사용하세요. `soft_deny`와 달리 hard-deny 규칙은 분류기가 협상할 수 없습니다.

```json
{
  "autoMode": {
    "hard_deny": ["Bash(rm -rf /:*)", "Bash(git push --force*)"]
  }
}
```

**이전** (기본 제공 규칙 대체 — v2.1.118 이전 동작):

```json
{
  "autoMode": {
    "allow": ["Bash(gh pr list:*)"]
  }
}
```

**이후** (기본 제공 규칙 확장 — v2.1.118+):

```json
{
  "autoMode": {
    "allow": ["$defaults", "Bash(gh pr list:*)"],
    "soft_deny": ["$defaults", "Bash(kubectl delete:*)"],
    "environment": ["$defaults", "trusted-ci.internal"]
  }
}
```

`"$defaults"`를 사용하여 제공된 기본 규칙을 유지하면서 조직 또는 프로젝트별 추가 사항을 계층화하세요.

#### 모든 셸 명령어 분류 (`autoMode.classifyAllShell`, v2.1.193)

`autoMode.classifyAllShell` (boolean, v2.1.193+)은 **모든** Bash/PowerShell 명령어를 auto-mode 분류기를 통해 라우팅합니다. 세션의 모든 셸 명령어를 분류기가 검사하도록 하려면 활성화하세요.

```json
{
  "autoMode": {
    "classifyAllShell": true
  }
}
```

동일한 릴리스에서는 auto mode가 작업을 차단할 때 **거부 이유**를 표시합니다 — 트랜스크립트, 거부 토스트, `/permissions` 아래의 최근 거부 목록에서 확인할 수 있습니다(v2.1.193+).

#### 내장 의도 기반 보호 (v2.1.183)

사용자가 설정한 `hard_deny`와 별도로, auto mode는 이 세션에서 명시적으로 요청하지 않은 경우 다음 파괴적 명령어를 기본적으로 차단합니다:

- `git reset --hard`, `git checkout -- .`, `git clean -fd`, `git stash drop`
- `git commit --amend` (커밋이 이번 세션에서 에이전트에 의해 생성되지 않은 경우)
- `terraform destroy`, `pulumi destroy`, `cdk destroy` (특정 스택을 요청하지 않은 경우)

이는 추론된 의도에 의해 구동되는 내장 기본 보호입니다 — 이를 `hard_deny`에 직접 추가할 필요가 없습니다.

### 폴백 동작

분류기가 확실하지 않은 경우, auto mode는 사용자에게 프롬프트로 폴백합니다:
- **3회 연속** 분류기 차단 후
- 세션에서 **총 20회** 분류기 차단 후

이는 분류기가 작업을 확신 있게 승인할 수 없을 때 사용자가 항상 제어권을 유지하도록 보장합니다.

### Auto-Mode-Equivalent 권한 시딩 (Team 요금제 불필요)

Team 요금제가 없거나 백그라운드 분류기 없이 더 간단한 접근 방식을 원하는 경우, `~/.claude/settings.json`에 안전한 권한 규칙의 보수적 기준선을 시딩할 수 있습니다. 스크립트는 읽기 전용 및 로컬 검사 규칙으로 시작한 후, 원할 때만 편집, 테스트, 로컬 git 쓰기, 패키지 설치, GitHub 쓰기 작업을 옵트인할 수 있게 합니다.

**파일:** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 추가될 내용 미리보기 (변경 사항 작성 안 함)
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 보수적 기준선 적용
python3 09-advanced-features/setup-auto-mode-permissions.py

# 필요할 때만 더 많은 기능 추가
python3 09-advanced-features/setup-auto-mode-permissions.py --include-edits --include-tests
python3 09-advanced-features/setup-auto-mode-permissions.py --include-git-write --include-packages
```

스크립트는 다음 카테고리에 규칙을 추가합니다:

| 카테고리 | 예시 |
|----------|------|
| 핵심 읽기 전용 도구 | `Read(*)`, `Glob(*)`, `Grep(*)`, `Agent(*)`, `WebSearch(*)`, `WebFetch(*)` |
| 로컬 검사 | `Bash(git status:*)`, `Bash(git log:*)`, `Bash(git diff:*)`, `Bash(cat:*)` |
| 선택적 편집 | `Edit(*)`, `Write(*)`, `NotebookEdit(*)` |
| 선택적 테스트/빌드 | `Bash(pytest:*)`, `Bash(python3 -m pytest:*)`, `Bash(cargo test:*)` |
| 선택적 git 쓰기 | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git stash:*)` |
| Git (로컬 쓰기) | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git checkout:*)` |
| 패키지 관리자 | `Bash(npm install:*)`, `Bash(pip install:*)`, `Bash(cargo build:*)` |
| 빌드 및 테스트 | `Bash(make:*)`, `Bash(pytest:*)`, `Bash(go test:*)` |
| 일반 셸 | `Bash(ls:*)`, `Bash(cat:*)`, `Bash(find:*)`, `Bash(cp:*)`, `Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`, `Bash(gh pr create:*)`, `Bash(gh issue list:*)` |

위험한 작업(`rm -rf`, `sudo`, 강제 푸시, `DROP TABLE`, `terraform destroy` 등)은 의도적으로 제외되었습니다. 스크립트는 멱등적입니다 — 두 번 실행해도 규칙이 중복되지 않습니다.

---

## Background Tasks

Background tasks는 대화를 차단하지 않고 장기 실행 작업을 수행할 수 있게 합니다.

### Background Tasks란?

Background tasks는 작업을 계속하는 동안 비동기적으로 실행됩니다:
- 긴 테스트 스위트
- 빌드 프로세스
- 데이터베이스 마이그레이션
- 배포 스크립트
- 분석 도구

**기본 사용법:**
```bash
User: 백그라운드에서 테스트 실행해 주세요

Claude: 태스크 bg-1234 시작됨

/task list           # 모든 작업 표시
/task status bg-1234 # 진행 상황 확인
/task show bg-1234   # 출력 보기
/task cancel bg-1234 # 작업 취소
```

### Background Tasks 시작

```
User: 전체 테스트 스위트를 백그라운드에서 실행해 주세요

Claude: 백그라운드에서 테스트 시작 (task-id: bg-1234)
테스트가 실행되는 동안 계속 작업할 수 있습니다.

[대화를 계속할 수 있음]

User: 그동안 인증 모듈을 리팩토링합시다

Claude: [테스트가 백그라운드에서 실행되는 동안 인증 모듈 작업]

[나중에, 테스트 완료 시]

Claude: 📢 백그라운드 태스크 bg-1234 완료:
✅ 245개 테스트 통과
❌ 3개 테스트 실패
결과 보기: /task show bg-1234
```

### Background Tasks 관리

**활성 작업 목록**:
```
User: /task list

활성 백그라운드 태스크:
1. [bg-1234] 테스트 실행 (50% 완료, 2분 남음)
2. [bg-1235] Docker 이미지 빌드 (25% 완료, 8분 남음)
3. [bg-1236] 스테이징에 배포 (90% 완료, 30초 남음)
```

**작업 상태 확인**:
```
User: /task status bg-1234

태스크 bg-1234: 테스트 실행
상태: 진행 중
진행률: 120/245 테스트 (49%)
시작: 2025-11-08 10:30:15
예상 완료: 2025-11-08 10:34:22
```

**작업 출력 보기**:
```
User: /task show bg-1234

[테스트 실행의 실시간 출력 표시]
```

**작업 취소**:
```
User: /task cancel bg-1234

백그라운드 태스크 bg-1234 취소됨
```

### 예제: 병렬 개발

```
User: 백그라운드에서 빌드 실행해 주세요

Claude: 빌드 시작... (task-id: bg-5001)

User: 린터도 백그라운드에서 실행해 주세요

Claude: 린터 시작... (task-id: bg-5002)

User: 그동안 새 API 엔드포인트를 구현합시다

Claude: [빌드와 린터가 실행되는 동안 API 엔드포인트 구현]

[10분 후]

Claude: 📢 빌드 성공적으로 완료 (bg-5001)
📢 린터에서 12개 이슈 발견 (bg-5002)

User: 린터 이슈를 보여주세요

Claude: [bg-5002의 린터 출력 표시]
```

### 설정

```json
{
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5,
    "notifyOnCompletion": true,
    "autoCleanup": true,
    "logOutput": true
  }
}
```

---

## Monitor Tool (이벤트 기반 스트림)

> **v2.1.98 신규**: Monitor 도구를 사용하면 Claude가 백그라운드 명령어의 stdout을 감시하고 일치하는 이벤트가 나타나는 순간 반응할 수 있습니다 — 장기 실행 프로세스를 기다리기 위한 폴링 루프와 `sleep`을 대체합니다.

Monitor는 stdout에 쓰는 모든 셸 명령어에 연결됩니다. 명령어의 각 stdout 라인이 세션을 깨우는 알림이 됩니다. Claude가 명령어를 지정하고, 하네스가 출력을 스트리밍하고 이벤트가 발생할 때 전달합니다. 기본 프로세스 시작에 대한 관련 [Background Tasks](#background-tasks) 섹션을 참조하세요.

### 중요한 이유

`/loop` 또는 `sleep`으로 폴링하면 변경 사항 여부와 관계없이 매 사이클마다 전체 API 라운드트립이 소모됩니다. Monitor는 이벤트가 발생할 때까지 조용히 대기하여 명령어가 조용한 동안 **제로 토큰**을 소비합니다. 이벤트가 발생하면 Claude가 즉시 반응합니다 — 다음 폴링 틱을 기다려 지연 발견할 필요가 없습니다. 몇 분 이상 실행되는 작업의 경우 폴링 루프보다 저렴하고 빠릅니다.

### 두 가지 일반적인 패턴

**스트림 필터**는 장기 실행 소스의 연속 출력을 감시합니다. 명령어가 영원히 실행되고, 일치하는 모든 라인이 이벤트입니다.

```bash
tail -f /var/log/app.log | grep --line-buffered "ERROR"
```

**폴링 및 방출 필터**는 소스를 주기적으로 확인하고 변경 시에만 방출합니다. API, 데이터베이스 또는 네이티브 스트림이 없는 항목에 사용하세요.

```bash
last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
while true; do
  gh api "repos/owner/repo/issues/123/comments?since=$last" || true
  last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
  sleep 30
done
```

### 구체적인 예시

"개발 서버를 시작하고 오류를 모니터링해 주세요." Claude가 서버를 백그라운드 작업으로 시작하고, Monitor 필터(`tail -F server.log | grep --line-buffered -E "ERROR|FATAL"`)를 연결하면 세션이 조용해집니다. 로그에 오류 라인이 나타나는 순간 Claude가 깨어나 오류를 읽고 반응할 수 있습니다 — 서버 재시작, 버그 수정, 또는 사용자에게 표시 — 사용자가 확인할 필요 없이.

> **경고**: `grep`으로 파이핑할 때는 **항상** `grep --line-buffered`를 사용하세요. 이것이 없으면 grep이 stdout을 4KB 청크로 버퍼링하여 트래픽이 적은 스트림에서 이벤트가 몇 분 지연될 수 있습니다. 실제로 Monitor가 작동하지 않는 가장 흔한 원인입니다 — 필터가 조용해야 할 때 소리 없으면 `--line-buffered` 플래그를 먼저 확인하세요.

---

## 동적 워크플로우

> **v2.1.154 신규**

동적 워크플로우를 사용하면 Claude가 수십에서 수백 개의 백그라운드 [subagent](../04-subagents/README.md)를 **결정론적으로** 오케스트레이션할 수 있습니다 — 팬아웃, 파이프라인, 병렬 단계가 모델의 즉흥성에 맡겨지는 대신 스크립트로 인코딩됩니다. 단일 에이전트가 하나의 컨텍스트 창을 보유하는 반면, 워크플로우는 많은 에이전트에 작업을 분해하고 결과를 재결합합니다.

### 사용 시기

- **포괄적 커버리지** — 많은 파일/차원에 걸쳐 병렬로 감사 또는 검토
- **신뢰도** — 독립적 관점을 생성한 후 커밋 전에 적대적으로 결과 검증
- **단일 컨텍스트를 넘는 규모** — 단일 컨텍스트가 담을 수 없는 대규모 마이그레이션, 광범위한 정리 또는 연구

이미 이해하고 있는 일회성 작업에는 단일 에이전트(또는 직접 편집)가 여전히 올바른 도구입니다 — 워크플로우는 작업이 확장될 때 효과적입니다.

### 시작 및 보기

- **시작**: Claude에게 작업에 대한 워크플로우 생성을 요청하세요(예: "`src/`의 모든 파일을 검토하는 워크플로우 실행"). Claude가 오케스트레이션 스크립트를 작성하고 백그라운드에서 실행합니다.
- **보기**: `/workflows` 명령어는 실행 중 및 완료된 워크플로우 실행을 실시간 진행률과 함께 표시합니다.
- **`ultracode`**: `/effort` 메뉴에서 `ultracode`를 선택하면 세션에 대해 이 기능이 켜집니다 — 모델에 `xhigh`를 보내고 Claude가 기본적으로 동적 워크플로우를 오케스트레이션하도록 합니다. 세션 전용이며 설정 파일에서 허용되지 않습니다. (v2.1.160 기준 트리거 키워드는 `ultracode`이며, "workflow"라는 단어만으로는 더 이상 실행이 트리거되지 않습니다.)

워크플로우는 subagent 모델을 기반으로 구축됩니다 — 개별 에이전트가 어떻게 정의되고 범위가 지정되는지는 [Subagents](../04-subagents/README.md)를 참조하세요.

---

## 예약 작업

예약 작업을 사용하면 정기적인 일정이나 일회성 알림으로 프롬프트를 자동으로 실행할 수 있습니다. 작업은 세션 범위로 지정되며, Claude Code가 활성화된 동안 실행되고 세션이 종료되면 지워집니다. v2.1.72+부터 사용 가능.

> **claude.com에서 "Routines"로 마케팅 (2026-05-14)**: Anthropic의 제품 블로그에서 이 기능을 **Routines**로 소개합니다. CLI 명령어는 `/schedule`로 유지됩니다; 이 가이드는 연속성을 위해 원래 이름인 "예약 작업"을 사용합니다. claude.com 문서나 데스크탑 앱에서 "Routines"를 보면 동일한 기능을 가리킵니다.

### `/loop` 명령어

```bash
# 명시적 간격
/loop 5m 배포가 완료되었는지 확인

# 자연어
/loop 30분마다 빌드 상태 확인
```

정확한 스케줄링을 위해 표준 5필드 cron 표현식도 지원됩니다.

### 일회성 알림

특정 시간에 한 번 실행되는 알림 설정:

```
오후 3시에 릴리스 브랜치를 푸시하라고 알려줘
45분 후에 통합 테스트를 실행해 줘
```

### 예약 작업 관리

| 도구 | 설명 |
|------|------|
| `CronCreate` | 새 예약 작업 생성 |
| `CronList` | 모든 활성 예약 작업 목록. v2.1.136부터 출력에 한정자와 예약된 프롬프트 본문도 포함되어, 각 cron이 무엇을 실행할지 열지 않고도 감사할 수 있습니다. |
| `CronDelete` | 예약 작업 제거 |

**제한 및 동작**:
- 세션당 최대 **50개 예약 작업**
- 세션 범위 — 세션이 종료되면 지워짐
- 반복 작업은 **3일 후** 자동 만료
- 작업은 Claude Code가 실행 중인 동안에만 실행됨 — 놓친 실행에 대한 캐치업 없음

### 동작 세부사항

| 항목 | 세부사항 |
|------|---------|
| **반복 지터** | 간격의 최대 10% (최대 15분) |
| **일회성 지터** | :00/:30 경계에서 최대 90초 |
| **놓친 실행** | 캐치업 없음 — Claude Code가 실행 중이 아니면 건너뜀 |
| **지속성** | 재시작 간 유지되지 않음 |

### 클라우드 예약 작업

`/schedule`을 사용하여 Anthropic 인프라에서 실행되는 클라우드 예약 작업 생성:

```
/schedule 매일 오전 9시에 테스트 스위트를 실행하고 실패를 보고해 줘
```

클라우드 예약 작업은 재시작 간에도 유지되며 Claude Code가 로컬에서 실행 중일 필요가 없습니다.

### 예약 작업 비활성화

```bash
export CLAUDE_CODE_DISABLE_CRON=1
```

> **API 키 계층에 의한 `/schedule` 자동 비활성화 (v2.1.139)**: `ANTHROPIC_API_KEY`, `ANTHROPIC_AUTH_TOKEN`, 또는 `apiKeyHelper` 중 하나라도 설정되면 클라우드 `/schedule`이 조용히 사용 불가능해집니다 — claude.ai에 로그인되어 있어도 마찬가지입니다. 동일한 조건이 [Remote Control](#원격-제어-비활성화-disableremotecontrol-v21128), claude.ai MCP 커넥터, 알림 기본 설정도 비활성화합니다. `/schedule`을 사용하려면 API 키를 설정 해제하거나(또는 Pro/Max OAuth 계층에서 실행). 로컬 `CronCreate`는 영향을 받지 않습니다.

### 예제: 배포 모니터링

```
/loop 5m 스테이징 환경의 배포 상태를 확인해 줘.
        배포가 성공하면 알려주고 루프를 중단해 줘.
        실패하면 오류 로그를 보여줘.
```

> **팁**: 예약 작업은 세션 범위입니다. 재시작 후에도 지속되는 자동화에는 CI/CD 파이프라인, GitHub Actions 또는 데스크탑 앱 예약 작업을 대신 사용하세요.

---

## Permission Modes

Permission modes는 Claude가 명시적 승인 없이 할 수 있는 작업을 제어합니다.

### 사용 가능한 Permission Modes

| 모드 | 동작 |
|------|------|
| `default` | 파일만 읽기; 다른 모든 작업은 프롬프트 |
| `acceptEdits` | 파일 읽기 및 편집; 명령어는 프롬프트 |
| `plan` | 파일만 읽기 (연구 모드, 편집 불가) |
| `auto` | 백그라운드 안전 분류기 검사로 모든 작업 (Research Preview) |
| `bypassPermissions` | 모든 작업, 권한 검사 없음 (위험) |
| `dontAsk` | 사전 승인된 도구만 실행; 나머지는 모두 거부 |

CLI에서 `Shift+Tab`으로 모드를 순환합니다. `--permission-mode` 플래그 또는 `permissions.defaultMode` 설정으로 기본값을 설정합니다.

v2.1.160부터 `acceptEdits`도 셸 시작 파일(`.zshenv`, `.zlogin`, `.bash_login`, `~/.config/git/`) 및 코드 실행 빌드 설정(`.npmrc`, `.yarnrc*`, `bunfig.toml`, `.bazelrc`, `.pre-commit-config.yaml`, `.devcontainer/`, ...)을 쓰기 전에 프롬프트하며, 이는 의도하지 않은 명령어 실행으로 이어질 수 있습니다.

> **`--dangerously-skip-permissions` 확장 경로 커버리지 (v2.1.121, v2.1.126)**: `--dangerously-skip-permissions` CLI 플래그(및 동등한 `bypassPermissions` 모드)가 이제 훨씬 더 넓은 허용 목록에 대한 쓰기 프롬프트를 우회합니다 — `.claude/skills/`, `.claude/agents/`, `.claude/commands/`, `.claude/`, `.git/`, `.vscode/`, 셸 설정 파일. 치명적 제거 명령어(`rm -rf /` 등)는 여전히 모드와 관계없이 프롬프트합니다. 플래그를 이전보다 더 예리한 도구로 취급하고, 일회용 샌드박스에서만 사용하세요.

> **Windows 셸 감지 (v2.1.120, v2.1.126)**: Git for Windows / Git Bash가 더 이상 필요하지 않습니다. Git Bash가 없으면 Claude Code가 PowerShell을 셸 도구로 사용합니다. v2.1.126부터 PowerShell 도구가 활성화되면 PowerShell이 *기본* 셸이며, Microsoft Store, PATH가 없는 MSI, 또는 `.NET global tool`로 설치된 PowerShell 7도 감지합니다.

> **Windows에서 PowerShell 도구 기본 활성화 (Bedrock/Vertex/Foundry, v2.1.143)**: v2.1.143부터 Windows에서 PowerShell 도구가 Bedrock, Vertex, Foundry 사용자에 대해 **기본 활성화**됩니다. Claude Code는 `-ExecutionPolicy Bypass`로 PowerShell을 호출하여 시스템 정책이 `Restricted`이더라도 스크립트가 실행됩니다. Claude Code가 시스템 실행 정책을 따르게 하려면 `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1`을 설정하세요. PowerShell 도구를 완전히 비활성화하려면 `CLAUDE_CODE_USE_POWERSHELL_TOOL=0`을 설정하세요.

### 활성화 방법

**키보드 단축키**:
```bash
Shift + Tab  # 6개 모드 순환
```

**Slash command**:
```bash
/plan                  # Plan 모드 진입
```

**CLI 플래그**:
```bash
claude --permission-mode plan
claude --permission-mode auto
```

**설정**:
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### Permission Mode 예제

#### Default Mode
Claude가 중요한 작업에 대해 확인을 요청:

```
User: auth.ts의 버그를 수정해 주세요

Claude: 버그를 수정하기 위해 src/auth.ts를 수정해야 합니다.
변경 사항은 비밀번호 검증 로직을 업데이트합니다.

이 변경을 승인하시겠습니까? (yes/no/show)
```

#### Plan Mode
실행 전 구현 계획 검토:

```
User: /plan 사용자 인증 시스템 구현

Claude: 인증 구현 계획을 세우겠습니다.

## 구현 계획
[단계와 단계가 포함된 상세 계획]

계속 진행하시겠습니까? (yes/no/modify)
```

#### Accept Edits Mode
파일 수정 자동 승인:

```
User: acceptEdits
User: auth.ts의 버그를 수정해 주세요

Claude: [묻지 않고 변경 수행]
```

### 사용 사례

**코드 리뷰**:
```
User: claude --permission-mode plan
User: 이 PR을 검토하고 개선점을 제안해 주세요

Claude: [코드를 읽고 피드백 제공, 수정은 불가]
```

**페어 프로그래밍**:
```
User: claude --permission-mode default
User: 함께 기능을 구현해 봅시다

Claude: [각 변경 전에 승인 요청]
```

**자동화된 작업**:
```
User: claude --permission-mode acceptEdits
User: 코드베이스의 모든 린팅 문제를 수정해 주세요

Claude: [묻지 않고 파일 편집 자동 승인]
```

---

## Headless Mode

Print mode (`claude -p`)를 사용하면 대화형 입력 없이 Claude Code를 실행할 수 있어 자동화 및 CI/CD에 적합합니다. 이는 구형 `--headless` 플래그를 대체하는 비대화형 모드입니다.

### Print Mode란?

Print mode를 사용하면:
- 자동화된 스크립트 실행
- CI/CD 통합
- 배치 처리
- 예약 작업

### Print Mode로 실행 (비대화형)

```bash
# 특정 작업 실행
claude -p "모든 테스트 실행"

# 파이프된 콘텐츠 처리
cat error.log | claude -p "이 오류들을 분석해 주세요"

# CI/CD 통합 (GitHub Actions)
- name: AI 코드 리뷰
  run: claude -p "PR 리뷰"
```

### 추가 Print Mode 사용 예시

```bash
# 출력 캡처로 특정 작업 실행
claude -p "모든 테스트 실행 및 커버리지 리포트 생성"

# 구조화된 출력으로
claude -p --output-format json "코드 품질 분석"

# stdin에서 입력 받기
echo "코드 품질 분석" | claude -p "설명해 주세요"
```

### 예제: CI/CD 통합

**GitHub Actions**:
```yaml
# .github/workflows/code-review.yml
name: AI 코드 리뷰

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Claude Code 설치
        run: npm install -g @anthropic-ai/claude-code

      - name: Claude Code 리뷰 실행
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p --output-format json \
            --max-turns 3 \
            "이 PR을 다음에 대해 리뷰:
            - 코드 품질 문제
            - 보안 취약점
            - 성능 우려사항
            - 테스트 커버리지
            결과를 JSON으로 출력" > review.json

      - name: 리뷰 댓글 게시
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: JSON.stringify(review, null, 2)
            });
```

### Print Mode 설정

Print mode (`claude -p`)는 자동화를 위한 여러 플래그를 지원:

```bash
# 자율 턴 수 제한
claude -p --max-turns 5 "이 모듈 리팩토링"

# 구조화된 JSON 출력
claude -p --output-format json "이 코드베이스 분석"

# 스키마 검증 포함
claude -p --json-schema '{"type":"object","properties":{"issues":{"type":"array"}}}' \
  "이 코드의 버그를 찾아 주세요"

# 세션 지속성 비활성화
claude -p --no-session-persistence "일회성 분석"
```

### Safe Mode (문제 해결)

`--safe-mode`(및 `CLAUDE_CODE_SAFE_MODE` 환경 변수, 예: `CLAUDE_CODE_SAFE_MODE=1`)는 **모든 사용자 정의를 비활성화**하고 Claude Code를 시작합니다 — CLAUDE.md, plugins, skills, hooks, MCP 서버가 모두 꺼집니다.

```bash
# 모든 사용자 정의를 비활성화하고 실행
claude --safe-mode

# 환경 변수를 통한 동등한 방법
CLAUDE_CODE_SAFE_MODE=1 claude
```

문제 해결 도구입니다: 사용자 정의 설정이 문제를 일으키는 경우, safe mode로 실행하여 문제가 설정에 있는지 Claude Code 자체에 있는지 격리하세요.

---

## 세션 관리

여러 Claude Code 세션을 효과적으로 관리합니다.

### 세션 관리 명령어

| 명령어 | 설명 |
|--------|------|
| `/resume` | ID 또는 이름으로 대화 재개 |
| `/rename` | 현재 세션 이름 지정 |
| `/fork` | 현재 세션을 새 브랜치로 포크 |
| `claude -c` | 가장 최근 대화 계속 |
| `claude -r "session"` | 이름 또는 ID로 세션 재개 |

### 세션 재개

**마지막 대화 계속**:
```bash
claude -c
```

**이름이 있는 세션 재개**:
```bash
claude -r "auth-refactor" "이 PR 마무리"
```

**현재 세션 이름 변경** (REPL 내부):
```
/rename auth-refactor
```

### 세션 포크

원본을 잃지 않고 대체 접근 방식을 시도하기 위해 세션 포크:

```
/fork
```

또는 CLI에서:
```bash
claude --resume auth-refactor --fork-session "대신 OAuth 시도"
```

### 세션 지속성

세션이 자동으로 저장되고 재개 가능:

```bash
# 마지막 대화 계속
claude -c

# 이름 또는 ID로 특정 세션 재개
claude -r "auth-refactor"

# 실험을 위해 재개 및 포크
claude --resume auth-refactor --fork-session "대체 접근 방식"
```

### 세션 요약 (v2.1.108)

자리를 비웠다가 세션으로 돌아오면 Claude가 수행된 작업에 대한 간단한 요약을 보여줄 수 있습니다. 이는 원격 측정이 비활성화된 사용자(Bedrock, Vertex, Foundry 사용자)에게 기본 활성화됩니다.

> **OTEL 텔레메트리 — 피드백 설문조사 재활성화 (v2.1.136+)**: OpenTelemetry 데이터를 캡처하는 조직은 `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL=1`을 설정하여 Anthropic의 세션 품질 설문조사를 재활성화할 수 있습니다. OTEL 배포에서는 설문조사가 기본적으로 꺼져 있는데, 이전에 텔레메트리 파이프라인에서 리디렉션되었기 때문입니다.

> **OTEL 텔레메트리 — `assistant_response` 로그 이벤트 (v2.1.193+)**: Claude Code가 `claude_code.assistant_response` OpenTelemetry 로그 이벤트를 내보내 모델의 응답 텍스트를 전달하여, OTEL 파이프라인이 기존 도구/이벤트 텔레메트리와 함께 Claude가 말한 내용을 캡처할 수 있습니다.

**요약 동작 제어:**

```bash
/recap                                 # 수동으로 요약 트리거
/config                                # 자동 요약 켜기/끄기
```

또는 환경 변수:
```bash
CLAUDE_CODE_ENABLE_AWAY_SUMMARY=0 claude   # 요약 비활성화
CLAUDE_CODE_ENABLE_AWAY_SUMMARY=1 claude   # 요약 강제 활성화
```

---

## 대화형 기능

### 키보드 단축키

Claude Code는 효율성을 위한 키보드 단축키를 지원합니다. 공식 문서에서 가져온 완전한 참조입니다:

| 단축키 | 설명 |
|--------|------|
| `Ctrl+C` | 현재 입력/생성 취소 |
| `Ctrl+D` | Claude Code 종료 |
| `Ctrl+G` | 외부 편집기에서 계획 편집 |
| `Ctrl+L` | 터미널 화면 지우기 |
| `Ctrl+O` | 상세 출력 전환 (추론 보기) |
| `Ctrl+R` | 기록 역검색. 기본값은 **모든 프로젝트의 모든 프롬프트**(v2.1.129+); 선택기 내에서 `Ctrl+S`를 누르면 현재 프로젝트로 좁힐 수 있습니다. 이전 버전은 프로젝트 전용이 기본이었습니다. |
| `Ctrl+T` | 작업 목록 보기 전환 |
| `Ctrl+B` | 백그라운드 실행 작업 |
| `Esc+Esc` | 코드/대화 되감기 |
| `Shift+Tab` / `Alt+M` | Permission modes 전환 |
| `Option+P` / `Alt+P` | 모델 전환 |
| `Option+T` / `Alt+T` | Extended thinking 전환 |

**라인 편집 (표준 readline 단축키):**

| 단축키 | 동작 |
|--------|------|
| `Ctrl + A` | 줄 시작으로 이동 |
| `Ctrl + E` | 줄 끝으로 이동 |
| `Ctrl + K` | 줄 끝까지 잘라내기 |
| `Ctrl + U` | 줄 시작까지 잘라내기 |
| `Ctrl + W` | 단어 뒤로 삭제 |
| `Ctrl + Y` | 붙여넣기 (yank) |
| `Tab` | 자동 완성 |
| `↑ / ↓` | 명령어 기록 |

### 키바인딩 사용자 정의

`/keybindings`를 실행하여 사용자 정의 키보드 단축키를 생성하면, 편집을 위해 `~/.claude/keybindings.json`이 열립니다(v2.1.18+).

**설정 형식**:

```json
{
  "$schema": "https://www.schemastore.org/claude-code-keybindings.json",
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+e": "chat:externalEditor",
        "ctrl+u": null,
        "ctrl+k ctrl+s": "chat:stash"
      }
    },
    {
      "context": "Confirmation",
      "bindings": {
        "ctrl+a": "confirmation:yes"
      }
    }
  ]
}
```

바인딩을 `null`로 설정하면 기본 단축키를 해제합니다.

### 사용 가능한 컨텍스트

키바인딩은 특정 UI 컨텍스트로 범위가 지정됩니다:

| 컨텍스트 | 주요 동작 |
|---------|-----------|
| **Chat** | `submit`, `cancel`, `cycleMode`, `modelPicker`, `thinkingToggle`, `undo`, `externalEditor`, `stash`, `imagePaste` |
| **Confirmation** | `yes`, `no`, `previous`, `next`, `nextField`, `cycleMode`, `toggleExplanation` |
| **Global** | `interrupt`, `exit`, `toggleTodos`, `toggleTranscript` |
| **Autocomplete** | `accept`, `dismiss`, `next`, `previous` |
| **HistorySearch** | `search`, `previous`, `next` |
| **Settings** | 컨텍스트별 설정 탐색 |
| **Tabs** | 탭 전환 및 관리 |
| **Help** | 도움말 패널 탐색 |

`Transcript`, `Task`, `ThemePicker`, `Attachments`, `Footer`, `MessageSelector`, `DiffDialog`, `ModelPicker`, `Select`를 포함하여 총 18개의 컨텍스트가 있습니다.

### 코드 지원

키바인딩은 코드 시퀀스(다중 키 조합)를 지원합니다:

```
"ctrl+k ctrl+s"   → 두 키 시퀀스: ctrl+k를 누른 다음 ctrl+s
"ctrl+shift+p"    → 동시 수정자 키
```

**키 입력 구문**:
- **수정자**: `ctrl`, `alt` (또는 `opt`), `shift`, `meta` (또는 `cmd`)
- **대문자는 Shift 의미**: `K`는 `shift+k`와 동일
- **특수 키**: `escape`, `enter`, `return`, `tab`, `space`, `backspace`, `delete`, 화살표 키

### 예약 및 충돌 키

| 키 | 상태 | 참고 |
|-----|------|------|
| `Ctrl+C` | 예약 | 리바인드 불가 (인터럽트) |
| `Ctrl+D` | 예약 | 리바인드 불가 (종료) |
| `Ctrl+B` | 터미널 충돌 | tmux 접두사 키 |
| `Ctrl+A` | 터미널 충돌 | GNU Screen 접두사 키 |
| `Ctrl+Z` | 터미널 충돌 | 프로세스 일시 중단 |

> **팁**: 단축키가 작동하지 않으면 터미널 에뮬레이터 또는 멀티플렉서와의 충돌을 확인하세요.

### 탭 완성

Claude Code는 지능적인 탭 완성을 제공:

```
User: /rew<TAB>
→ /rewind

User: /plu<TAB>
→ /plugin

User: /plugin <TAB>
→ /plugin install
→ /plugin enable
→ /plugin disable
```

### 명령어 기록

이전 명령어 접근:

```
User: <↑>  # 이전 명령어
User: <↓>  # 다음 명령어
User: Ctrl+R  # 기록 검색

(역방향-i-검색)`test': 모든 테스트 실행
```

### 여러 줄 입력

복잡한 쿼리를 위해 여러 줄 모드 사용:

```bash
User: \
> 긴 복잡한 프롬프트
> 여러 줄에 걸쳐
> \end
```

**예시:**

```
User: \
> 사용자 인증 시스템 구현
> 다음 요구사항 포함:
> - JWT 토큰
> - 이메일 검증
> - 비밀번호 재설정
> - 2FA 지원
> \end

Claude: [여러 줄 요청 처리]
```

### 인라인 편집

보내기 전에 명령어 편집:

```
User: prodcution에 배포<Backspace><Backspace>uction

[보내기 전에 제자리에서 편집]
```

### Vim 모드

텍스트 편집을 위한 Vi/Vim 키바인딩 활성화:

**활성화**:
- `/config`("Editor / Vim mode" 전환)를 통해 또는 `~/.claude/settings.json`에서 `editorMode: "vim"`으로 활성화. 독립형 `/vim` slash command는 제거됨([이슈 #43370](https://github.com/anthropics/claude-code/issues/43370) 참조); vim 모드는 이제 설정 기반입니다.
- NORMAL의 `Esc`, INSERT의 `i/a/o`, VISUAL의 `v`, VISUAL-LINE의 `V`로 모드 전환(v2.1.118+)

**탐색 키**:
- `h` / `l` - 왼쪽/오른쪽으로 이동
- `j` / `k` - 아래/위로 이동
- `w` / `b` / `e` - 단어별 이동
- `0` / `$` - 줄 시작/끝으로 이동
- `gg` / `G` - 텍스트 시작/끝으로 점프

**텍스트 객체**:
- `iw` / `aw` - 단어 내부/주변
- `i"` / `a"` - 따옴표 문자열 내부/주변
- `i(` / `a(` - 괄호 내부/주변

**Visual 모드 (v2.1.118+)**:

| 키 | 모드 | 동작 |
|-----|------|------|
| `v` | Visual | 시각적 피드백이 있는 문자 단위 선택; 모션 키로 확장 |
| `V` | Visual-line | 줄 단위 선택; 항상 전체 줄 선택 |
| `y` | Yank | 현재 시각적 선택 복사 |
| `d` / `x` | Delete | 현재 시각적 선택 삭제 |
| `c` | Change | 선택 삭제 후 INSERT 모드 진입 |
| `Esc` | 종료 | NORMAL 모드로 돌아가기 |

Visual 선택은 입력 필드에서 강조 표시되므로 연산자를 커밋하기 전에 정확히 무엇이 yank, 삭제 또는 변경될지 볼 수 있습니다.

### Bash 모드

`!` 접두사로 셸 명령어 직접 실행:

```bash
! npm test
! git status
! cat src/index.js
```

컨텍스트 전환 없이 빠른 명령어 실행에 사용하세요.

**v2.1.193부터:** bash 모드(`!`)에 실시간 파일 경로 자동 완성이 있어 프롬프트를 떠나지 않고 셸 명령어를 입력할 때 경로가 완성됩니다.

**v2.1.186부터:** `!` 명령어의 출력이 이제 자동으로 Claude에 전송되어 응답합니다. 출력이 응답 없이 컨텍스트에만 추가되는 이전 동작을 유지하려면 `settings.json`에서 `"respondToBashCommands": false`를 설정하세요.

---

## TUI 모드 (전체 화면)

> **v2.1.110 신규**

TUI(Text User Interface) 모드는 Claude Code를 전체 화면으로 깜빡임 없는 출력으로 렌더링합니다 — tmux 또는 iTerm2 분할 창과 같은 터미널 멀티플렉서에 이상적입니다.

### TUI 모드 활성화

`/tui` 명령어로 TUI 모드를 전환하거나 `--tui` 플래그로 실행:

```bash
/tui          # 세션 내에서 전환
claude --tui  # TUI 모드로 직접 시작
```

### 설정

| 설정 | 설명 | 기본값 |
|------|------|--------|
| `autoScrollEnabled` | 최신 메시지로 자동 스크롤 | `true` |

`/config` 또는 `settings.json`을 통해 자동 스크롤 비활성화:

```json
{
  "autoScrollEnabled": false
}
```

### 집중 보기

`/focus` 명령어는 집중 보기를 전환합니다 — 가장 관련성 높은 출력만 표시하는 방해 요소 없는 디스플레이. `Ctrl+O`는 이제 일반 및 상세 트랜스크립트 전환만 수행합니다(집중 보기는 `/focus`).

---

## 음성 받아쓰기

음성 받아쓰기는 Claude Code에 푸시투톡 음성 입력을 제공하여, 입력 대신 말로 프롬프트를 보낼 수 있습니다.

### 음성 받아쓰기 활성화

```
/voice
```

### 기능

| 기능 | 설명 |
|------|------|
| **푸시투톡** | 키를 눌러 녹음, 놓아서 전송 |
| **20개 언어** | 음성-텍스트가 20개 언어 지원 |
| **사용자 정의 키바인딩** | `/keybindings`를 통해 푸시투톡 키 설정 |
| **계정 필요** | STT 처리를 위한 Claude.ai 계정 필요 |

### 설정

키바인딩 파일(`/keybindings`)에서 푸시투톡 키바인딩을 사용자 정의하세요. 음성 받아쓰기는 Claude.ai 계정을 사용하여 음성-텍스트 처리를 수행합니다.

---

## Channels

Channels은 Research Preview 기능으로, MCP 서버를 통해 외부 서비스의 이벤트를 실행 중인 Claude Code 세션으로 푸시합니다. Telegram, Discord, iMessage 및 임의의 webhook을 소스로 포함하여, Claude가 폴링 없이 실시간 알림에 반응할 수 있습니다.

> **인증 (v2.1.128+)**: `--channels`는 이제 Pro/Max OAuth **및** API-key(콘솔) 인증 모두에서 작동합니다. 이전 릴리스에서는 OAuth가 필요했습니다.

### Channels 구독

```bash
# 시작 시 채널 플러그인 구독
claude --channels discord,telegram

# 여러 소스 구독
claude --channels discord,telegram,imessage,webhooks
```

### 지원되는 통합

| 통합 | 설명 |
|------|------|
| **Discord** | 세션에서 Discord 메시지 수신 및 응답 |
| **Telegram** | 세션에서 Telegram 메시지 수신 및 응답 |
| **iMessage** | 세션에서 iMessage 알림 수신 |
| **Webhooks** | 임의의 webhook 소스에서 이벤트 수신 |

### 설정

시작 시 `--channels` 플래그로 채널을 설정하세요. 엔터프라이즈 배포의 경우 관리 설정을 사용하여 허용된 채널 플러그인을 제어:

```json
{
  "allowedChannelPlugins": ["discord", "telegram"]
}
```

`allowedChannelPlugins` 관리 설정은 조직 전체에서 어떤 채널 플러그인이 허용되는지 제어합니다.

### 작동 방식

1. MCP 서버가 외부 서비스에 연결되는 채널 플러그인 역할
2. 수신 메시지 및 이벤트가 활성 Claude Code 세션으로 푸시
3. Claude가 세션 컨텍스트 내에서 메시지를 읽고 응답 가능
4. 채널 플러그인은 `allowedChannelPlugins` 관리 설정을 통해 승인되어야 함
5. 폴링 불필요 — 이벤트가 실시간으로 푸시됨

---

## Chrome 통합

Chrome 통합은 Claude Code를 Chrome 또는 Microsoft Edge 브라우저에 연결하여 실시간 웹 자동화 및 디버깅을 가능하게 합니다. v2.0.73+부터 사용 가능한 베타 기능입니다(Edge 지원은 v1.0.36+에 추가).

### Chrome 통합 활성화

**시작 시**:

```bash
claude --chrome      # Chrome 연결 활성화
claude --no-chrome   # Chrome 연결 비활성화
```

**세션 내에서**:

```
/chrome
```

"기본 활성화"를 선택하면 모든 향후 세션에 대해 Chrome 통합이 활성화됩니다. Claude Code는 브라우저의 로그인 상태를 공유하므로 인증된 웹 앱과 상호작용할 수 있습니다.

### 기능

| 기능 | 설명 |
|------|------|
| **실시간 디버깅** | 콘솔 로그 읽기, DOM 요소 검사, JavaScript 실시간 디버그 |
| **디자인 검증** | 렌더링된 페이지를 디자인 목업과 비교 |
| **양식 검증** | 양식 제출, 입력 검증, 오류 처리 테스트 |
| **웹 앱 테스트** | 인증된 앱과 상호작용 (Gmail, Google Docs, Notion 등) |
| **데이터 추출** | 웹 페이지에서 콘텐츠 스크래핑 및 처리 |
| **세션 녹화** | 브라우저 상호작용을 GIF 파일로 녹화 |

### 사이트별 권한

Chrome 확장 프로그램이 사이트별 접근을 관리합니다. 확장 프로그램 팝업을 통해 특정 사이트에 대한 접근 권한을 언제든지 부여하거나 취소할 수 있습니다. Claude Code는 명시적으로 허용한 사이트와만 상호작용합니다.

### 작동 방식

Claude Code가 표시 가능한 창에서 브라우저를 제어합니다 — 실시간으로 작업이 진행되는 것을 볼 수 있습니다. 브라우저가 로그인 페이지나 CAPTCHA를 만나면 Claude가 일시 중지하고 수동으로 처리할 때까지 기다립니다.

### 알려진 제한 사항

- **브라우저 지원**: Chrome 및 Edge만 — Brave, Arc 및 기타 Chromium 브라우저는 지원되지 않음
- **WSL**: Linux용 Windows 하위 시스템에서 사용 불가
- **타사 제공자**: Bedrock, Vertex, Foundry API 제공자에서 지원되지 않음
- **서비스 워커 유휴**: Chrome 확장 프로그램 서비스 워커가 장기 세션 중 유휴 상태가 될 수 있음

> **팁**: Chrome 통합은 베타 기능입니다. 브라우저 지원은 향후 릴리스에서 확장될 수 있습니다.

---

## 원격 제어

원격 제어를 사용하면 휴대폰, 태블릿 또는 모든 브라우저에서 로컬에서 실행 중인 Claude Code 세션을 계속할 수 있습니다. 로컬 세션은 머신에서 계속 실행됩니다 — 클라우드로 이동하는 것은 없습니다. Pro, Max, Team, Enterprise 요금제에서 사용 가능(v2.1.51+).

### 원격 제어 시작

**CLI에서**:

```bash
# 기본 세션 이름으로 시작
claude remote-control

# 사용자 지정 이름으로 시작
claude remote-control --name "인증 리팩토링"
```

**세션 내에서**:

```
/remote-control
/remote-control "인증 리팩토링"
```

**사용 가능한 플래그**:

| 플래그 | 설명 |
|------|------|
| `--name "title"` | 쉽게 식별할 수 있도록 사용자 지정 세션 제목 |
| `--verbose` | 상세 연결 로그 표시 |
| `--sandbox` | 파일 시스템 및 네트워크 격리 활성화 |
| `--no-sandbox` | 샌드박싱 비활성화 (기본값) |

### 세션에 연결

다른 기기에서 연결하는 세 가지 방법:

1. **세션 URL** — 세션이 시작될 때 터미널에 출력됨; 모든 브라우저에서 열기
2. **QR 코드** — 시작 후 `spacebar`를 눌러 스캔 가능한 QR 코드 표시
3. **이름으로 찾기** — claude.ai/code 또는 Claude 모바일 앱(iOS/Android)에서 세션 탐색

### 보안

- **머신에서 인바운드 포트가 열리지 않음**
- **TLS를 통한 아웃바운드 HTTPS만**
- **범위가 지정된 자격증명** — 여러 개의 단기적이고 좁은 범위의 토큰
- **세션 격리** — 각 원격 세션은 독립적

### 원격 제어 vs Claude Code on the Web

| 항목 | 원격 제어 | Claude Code on Web |
|------|----------|-------------------|
| **실행** | 사용자 머신에서 실행 | Anthropic 클라우드에서 실행 |
| **로컬 도구** | 로컬 MCP 서버, 파일, CLI에 완전 접근 | 로컬 의존성 없음 |
| **사용 사례** | 다른 기기에서 로컬 작업 계속 | 모든 브라우저에서 새로 시작 |

### 제한 사항

- Claude Code 인스턴스당 하나의 원격 세션
- 호스트 머신에서 터미널이 계속 열려 있어야 함
- 네트워크에 연결할 수 없으면 ~10분 후 세션 시간 초과

### 사용 사례

- 책상에서 떨어져 있는 동안 모바일 기기나 태블릿에서 Claude Code 제어
- 로컬 도구 실행을 유지하면서 더 풍부한 claude.ai UI 사용
- 전체 로컬 개발 환경으로 이동 중 빠른 코드 리뷰

### 푸시 알림 (v2.1.110)

원격 제어가 활성화되고 `/config`에서 "Push when Claude decides"가 활성화되면, Claude가 휴대폰으로 모바일 푸시 알림을 보낼 수 있습니다 — 예를 들어, 긴 작업이 완료되거나 사용자 입력이 필요할 때.

활성화 방법:
1. 원격 제어 활성화: `/remote-control` 또는 `claude --rc`
2. `/config` 열고 **Push when Claude decides** 활성화

푸시 알림에는 Claude 구독과 Claude 모바일 앱이 필요합니다.

### 원격 제어 비활성화 (`disableRemoteControl`, v2.1.128+)

Team 또는 Enterprise 요금제의 관리자는 `disableRemoteControl` 설정으로 원격 제어를 완전히 차단할 수 있습니다. `true`이면 `claude remote-control`과 `/remote-control` 모두 실행을 거부합니다.

```json
{
  "disableRemoteControl": true
}
```

설정은 **관리/정책** 범위(예: macOS에서 `/Library/Application Support/ClaudeCode/managed-settings.json`)에서 적용되므로 개별 사용자가 재정의할 수 없습니다. 로컬 전용 실행을 조직 전체에 적용해야 할 때 유용합니다.

> **API 키 계층에 의한 원격 제어 자동 비활성화 (v2.1.139)**: 다음 중 하나라도 설정되면 원격 제어가 **조용히 비활성화**됩니다(claude.ai에 동시에 로그인되어 있어도):
>
> - `ANTHROPIC_API_KEY`
> - `ANTHROPIC_AUTH_TOKEN`
> - `apiKeyHelper` (settings.json)
>
> 동일한 조건이 [`/schedule`](#예약-작업), claude.ai MCP 커넥터, 알림 기본 설정을 비활성화합니다 — 네 가지 claude.ai-브리지 표면 모두 OAuth 로그인이 활성 자격증명이어야 게이트가 열립니다. 이러한 기능을 사용하려면 API 키를 설정 해제하거나(또는 Pro/Max OAuth 계층에서 실행).

---

## 웹 세션

웹 세션을 사용하면 claude.ai/code에서 브라우저로 직접 Claude Code를 실행하거나 CLI에서 웹 세션을 만들 수 있습니다.

### 웹 세션 생성

```bash
# CLI에서 새 웹 세션 생성
claude --remote "새 API 엔드포인트 구현"
```

이렇게 하면 모든 브라우저에서 접근할 수 있는 claude.ai의 Claude Code 세션이 시작됩니다.

### 웹 세션 로컬에서 재개

웹에서 세션을 시작하고 로컬에서 계속하려면:

```bash
# 로컬 터미널에서 웹 세션 재개
claude --teleport
```

또는 대화형 REPL 내에서:
```
/teleport
```

### 사용 사례

- 한 머신에서 작업을 시작하고 다른 머신에서 계속
- 팀원과 세션 URL 공유
- 시각적 diff 리뷰에 웹 UI를 사용한 후 실행을 위해 터미널로 전환

---

## 데스크탑 앱

Claude Code 데스크탑 앱은 시각적 diff 리뷰, 병렬 세션 및 통합 커넥터가 있는 독립 실행형 애플리케이션을 제공합니다. macOS 및 Windows에서 사용 가능(Pro, Max, Team, Enterprise 요금제).

### 설치

[claude.ai](https://claude.ai)에서 플랫폼에 맞게 다운로드:
- **macOS**: 유니버설 빌드 (Apple Silicon 및 Intel)
- **Windows**: x64 및 ARM64 설치 프로그램 제공

설정 지침은 [데스크탑 빠른 시작](https://code.claude.com/docs/en/desktop-quickstart)을 참조하세요.

### CLI에서 전환

현재 CLI 세션을 데스크탑 앱으로 전송:

```
/desktop
```

### 핵심 기능

| 기능 | 설명 |
|------|------|
| **Diff 보기** | 인라인 댓글이 있는 파일별 시각적 리뷰; Claude가 댓글을 읽고 수정 |
| **앱 미리보기** | 라이브 검증을 위한 내장 브라우저로 개발 서버 자동 시작 |
| **PR 모니터링** | GitHub CLI 통합으로 CI 실패 자동 수정 및 검사 통과 시 자동 병합 |
| **병렬 세션** | 자동 Git worktree 격리가 있는 사이드바의 여러 세션 |
| **예약 작업** | 앱이 열려 있는 동안 실행되는 반복 작업(매시간, 매일, 평일, 매주) |
| **리치 렌더링** | 구문 강조가 있는 코드, 마크다운 및 다이어그램 렌더링; GitHub-Flavored-Markdown 작업 목록 체크박스(`- [ ]` / `- [x]`)가 체크박스로 렌더링(v2.1.149+) |

### 앱 미리보기 설정

`.claude/launch.json`에서 개발 서버 동작 설정:

```json
{
  "command": "npm run dev",
  "port": 3000,
  "readyPattern": "ready on",
  "persistCookies": true
}
```

### 커넥터

더 풍부한 컨텍스트를 위해 외부 서비스 연결:

| 커넥터 | 기능 |
|--------|------|
| **GitHub** | PR 모니터링, 이슈 추적, 코드 리뷰 |
| **Slack** | 알림, 채널 컨텍스트 |
| **Linear** | 이슈 추적, 스프린트 관리 |
| **Notion** | 문서, 지식 베이스 접근 |
| **Asana** | 작업 관리, 프로젝트 추적 |
| **Calendar** | 일정 인식, 회의 컨텍스트 |

> **참고**: 커넥터는 원격(클라우드) 세션에서 사용할 수 없습니다.

### 원격 및 SSH 세션

- **원격 세션**: Anthropic 클라우드 인프라에서 실행; 앱이 닫혀도 계속 실행. claude.ai/code 또는 Claude 모바일 앱에서 접근 가능
- **SSH 세션**: SSH를 통해 원격 머신에 연결하여 원격 파일 시스템 및 도구에 완전히 접근. 원격 머신에 Claude Code가 설치되어 있어야 함

### 데스크탑의 Permission Modes

데스크탑 앱은 CLI와 동일한 4가지 permission modes를 지원:

| 모드 | 동작 |
|------|------|
| **권한 요청** (기본값) | 모든 편집과 명령어 검토 및 승인 |
| **편집 자동 승인** | 파일 편집이 자동 승인; 명령어는 수동 승인 필요 |
| **Plan 모드** | 변경 전 접근 방식 검토 |
| **권한 우회** | 자동 실행 (샌드박스 전용, 관리자 제어) |

### 엔터프라이즈 기능

- **관리 콘솔**: 조직의 Code 탭 접근 및 권한 설정 제어
- **MDM 배포**: macOS에서 MDM을 통해 또는 Windows에서 MSIX를 통해 배포
- **SSO 통합**: 조직 구성원에게 Single Sign-On 요구
- **관리형 설정**: 팀 설정 및 모델 가용성을 중앙에서 관리

---

## 작업 목록

작업 목록 기능은 컨텍스트 압축(대화 기록이 컨텍스트 창에 맞게 정리될 때)에도 유지되는 지속적 작업 추적을 제공합니다.

### 작업 목록 전환

`Ctrl+T`를 눌러 세션 중 작업 목록 보기를 켜거나 끕니다.

### 지속적 작업

작업은 컨텍스트 압축 전반에 걸쳐 유지되어, 대화 컨텍스트가 정리될 때 장기 실행 작업 항목이 손실되지 않도록 합니다. 이는 복잡한 다단계 구현에 특히 유용합니다.

### 이름이 있는 작업 디렉토리

`CLAUDE_CODE_TASK_LIST_ID` 환경 변수를 사용하여 세션 간에 공유되는 이름이 있는 작업 디렉토리 생성:

```bash
export CLAUDE_CODE_TASK_LIST_ID=my-project-sprint-3
```

이를 통해 여러 세션이 동일한 작업 목록을 공유할 수 있어 팀 워크플로우 또는 다중 세션 프로젝트에 유용합니다.

---

## 프롬프트 제안

프롬프트 제안은 git 기록 및 현재 대화 컨텍스트를 기반으로 회색으로 표시된 예제 명령어를 표시합니다.

### 작동 방식

- 입력 프롬프트 아래에 회색 텍스트로 제안이 표시됨
- `Tab`을 눌러 제안 수락
- `Enter`를 눌러 수락하고 즉시 제출
- 제안은 컨텍스트를 인식하여 git 기록 및 대화 상태에서 가져옴

### 프롬프트 제안 비활성화

```bash
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
```

---

## Git Worktrees

Git Worktrees를 사용하면 격리된 worktree에서 Claude Code를 시작하여, 스태싱이나 전환 없이 다른 브랜치에서 병렬 작업이 가능합니다.

### Worktree에서 시작

```bash
# 격리된 worktree에서 Claude Code 시작
claude --worktree
# 또는
claude -w
```

### Worktree 위치

Worktree는 다음 위치에 생성됩니다:
```
<repo>/.claude/worktrees/<name>
```

### 모노레포를 위한 Sparse Checkout

`worktree.sparsePaths` 설정을 사용하여 모노레포에서 sparse-checkout을 수행하면 디스크 사용량과 클론 시간이 줄어듭니다:

```json
{
  "worktree": {
    "sparsePaths": ["packages/my-package", "shared/"]
  }
}
```

### 기본 브랜치 참조 (`worktree.baseRef`)

**`worktree.baseRef`** (v2.1.133 추가) — `claude --worktree`가 `origin/<default>` 또는 로컬 `HEAD`에서 브랜치할지 제어합니다.

- `"fresh"` (기본값) — `origin/<default-branch>`에서 브랜치, 로컬 푸시되지 않은 커밋 무시. **v2.1.128에서 도입된 동작을 되돌리므로** v2.1.128 이후 로컬-HEAD 브랜칭에 의존한 사용자는 다시 옵트인해야 합니다.
- `"head"` — 로컬 `HEAD`에서 브랜치, 푸시되지 않은 커밋 유지.

`~/.claude/settings.json`에서 설정:

```json
{ "worktree": { "baseRef": "head" } }
```

### 백그라운드 세션 격리 (`worktree.bgIsolation`)

**`worktree.bgIsolation`** (v2.1.143 추가) — 백그라운드 세션(예: `/bg`, `claude --bg`, 또는 Agent View)이 자체 worktree를 가질지 아니면 포그라운드 작업 복사본을 직접 편집할지 제어합니다.

- *(기본값)* — 백그라운드 세션이 `<repo>/.claude/worktrees/` 아래에 격리된 worktree를 생성합니다. `--worktree`와 동일한 방식입니다.
- `"none"` — 백그라운드 세션이 현재 작업 복사본을 직접 편집합니다. worktree가 비실용적인 경우(예: 무거운 네이티브 빌드 아티팩트) 또는 백그라운드 에이전트가 포그라운드 세션과 편집을 조정해야 하는 경우 사용하세요.

```json
{ "worktree": { "bgIsolation": "none" } }
```

절충: `"none"`은 worktree 격리의 안전망을 제거합니다 — 백그라운드와 포그라운드 세션의 동시 편집이 라이브 작업 복사본에서 병합 충돌을 일으킬 수 있습니다.

### Worktree 도구 및 훅

| 항목 | 설명 |
|------|------|
| `EnterWorktree` | worktree에 진입하는 도구; v2.1.157부터 세션 중 Claude 관리 worktree 간 전환 가능 |
| `ExitWorktree` | 현재 worktree를 종료하고 정리하는 도구 |
| `WorktreeCreate` | worktree가 생성될 때 발생하는 Hook 이벤트 |
| `WorktreeRemove` | worktree가 제거될 때 발생하는 Hook 이벤트 |

v2.1.157부터 Claude가 관리하는 worktree는 에이전트가 완료될 때 잠금 해제 상태로 두어 `git worktree remove`/`prune`이 정리할 수 있습니다.

### 자동 정리

Worktree에서 변경 사항이 없으면 세션이 종료될 때 자동으로 정리됩니다.

### 사용 사례

- 기능 브랜치에서 작업하면서 main 브랜치를 건드리지 않음
- 작업 디렉토리에 영향을 주지 않고 격리된 상태에서 테스트 실행
- 폐기 가능한 환경에서 실험적 변경 시도
- 빠른 시작을 위해 모노레포에서 특정 패키지 Sparse-checkout

---

## 샌드박싱

샌드박싱은 Claude Code가 실행하는 Bash 명령어에 대해 OS 수준의 파일 시스템 및 네트워크 격리를 제공합니다. 이는 권한 규칙을 보완하며 추가 보안 레이어를 제공합니다.

### 샌드박싱 활성화

**Slash command**:
```
/sandbox
```

**CLI 플래그**:
```bash
claude --sandbox       # 샌드박싱 활성화
claude --no-sandbox    # 샌드박싱 비활성화
```

### 설정

| 설정 | 설명 |
|------|------|
| `sandbox.enabled` | 샌드박싱 활성화 또는 비활성화 |
| `sandbox.failIfUnavailable` | 샌드박싱을 활성화할 수 없으면 실패 |
| `sandbox.filesystem.allowWrite` | 쓰기 접근이 허용된 경로 |
| `sandbox.filesystem.allowRead` | 읽기 접근이 허용된 경로 |
| `sandbox.filesystem.denyRead` | 읽기 접근이 거부된 경로 |
| `sandbox.network.allowedDomains` | Bash 시작 프로세스가 접근할 수 있는 도메인 (`*.` 와일드카드 지원) |
| `sandbox.network.deniedDomains` | `allowedDomains` 와일드카드가 허용하더라도 차단할 도메인 (v2.1.113+) |
| `sandbox.enableWeakerNetworkIsolation` | macOS에서 약한 네트워크 격리 활성화 |
| `sandbox.bwrapPath` | (v2.1.133+, Linux/WSL) `bubblewrap` 바이너리 경로. 기본값: `$PATH` 조회. |
| `sandbox.socatPath` | (v2.1.133+, Linux/WSL) `socat` 바이너리 경로. 기본값: `$PATH` 조회. |
| `sandbox.credentials` | (v2.1.187+) 샌드박스 명령어가 자격증명 파일 및 비밀 환경 변수를 읽지 못하도록 차단. |
| `sandbox.allowAppleEvents` | (v2.1.181+, macOS) 샌드박스 명령어가 Apple Events를 보낼 수 있도록 옵트인. |

**Linux/WSL 바이너리 경로** (v2.1.133+) — Claude Code가 비표준 설치 위치를 가리키도록 설정:

```json
{
  "sandbox": {
    "bwrapPath": "/opt/bubblewrap/bin/bwrap",
    "socatPath": "/opt/socat/bin/socat"
  }
}
```

광범위한 와일드카드를 재정의하는 `deniedDomains` 예시 (v2.1.113+):

```json
{
  "sandbox": {
    "network": {
      "allowedDomains": ["*.example.com"],
      "deniedDomains": ["evil.example.com"]
    }
  }
}
```

와일드카드는 `example.com`의 모든 것을 허용하지만, `deniedDomains`는 특별히 지정된 호스트를 차단합니다.

### 설정 예시

```json
{
  "sandbox": {
    "enabled": true,
    "failIfUnavailable": true,
    "filesystem": {
      "allowWrite": ["/Users/me/project"],
      "allowRead": ["/Users/me/project", "/usr/local/lib"],
      "denyRead": ["/Users/me/.ssh", "/Users/me/.aws"]
    },
    "enableWeakerNetworkIsolation": true
  }
}
```

### 작동 방식

- Bash 명령어가 제한된 파일 시스템 접근으로 샌드박스 환경에서 실행됨
- 네트워크 접근을 격리하여 의도하지 않은 외부 연결 방지 가능
- 심층 방어를 위해 권한 규칙과 함께 작동
- macOS에서는 네트워크 제한에 `sandbox.enableWeakerNetworkIsolation` 사용 (macOS에서는 전체 네트워크 격리 불가)

### 사용 사례

- 신뢰할 수 없거나 생성된 코드를 안전하게 실행
- 프로젝트 외부 파일의 우발적 수정 방지
- 자동화된 작업 중 네트워크 접근 제한

---

## 관리형 설정 (엔터프라이즈)

관리형 설정을 통해 엔터프라이즈 관리자는 플랫폼 네이티브 관리 도구를 사용하여 조직 전체에 Claude Code 설정을 배포할 수 있습니다.

### 배포 방법

| 플랫폼 | 방법 | 시작 |
|--------|------|------|
| macOS | 관리 plist 파일 (MDM) | v2.1.51+ |
| Windows | Windows 레지스트리 | v2.1.51+ |
| 크로스 플랫폼 | 관리 설정 파일 | v2.1.51+ |
| 크로스 플랫폼 | 관리 드롭인 (`managed-settings.d/` 디렉토리) | v2.1.83+ |

### 관리 드롭인

v2.1.83부터 관리자는 여러 관리 설정 파일을 `managed-settings.d/` 디렉토리에 배포할 수 있습니다. 파일은 알파벳 순서로 병합되어 팀 간 모듈식 설정을 가능하게 합니다:

```
~/.claude/managed-settings.d/
  00-org-defaults.json
  10-team-policies.json
  20-project-overrides.json
```

### 사용 가능한 관리 설정

| 설정 | 설명 |
|------|------|
| `disableBypassPermissionsMode` | 사용자가 우회 권한을 활성화하지 못하도록 방지 |
| `availableModels` | 사용자가 선택할 수 있는 모델 제한 |
| `enforceAvailableModels` | (v2.1.175) `true`이면 `availableModels` 허용 목록이 **기본** 모델도 제한 — 설정된 기본값이 목록에 없으면 Claude Code가 첫 번째 허용된 모델로 대체. 사용자 및 프로젝트 설정이 더 이상 관리 `availableModels` 목록을 확장할 수 없음. |
| `allowedChannelPlugins` | 허용되는 채널 플러그인 제어 |
| `autoMode.environment` | auto mode를 위한 신뢰할 수 있는 인프라 설정 |
| `wslInheritsWindowsSettings` | Windows/WSL만 (v2.1.118+): `true`이면 WSL 내에서 실행되는 Claude Code가 Windows 호스트의 관리 설정을 상속하여 레지스트리/MDM을 통해 배포된 엔터프라이즈 정책이 Windows 및 WSL 셸에서 균일하게 적용됨 |
| `parentSettingsBehavior` | (v2.1.133+, admin-tier) SDK의 `managedSettings`가 부모 프로세스 설정과 병합되는 방식을 제어. `"first-wins"`는 기존 우선순위 유지(충돌 시 먼저 설정된 값이 우선); `"merge"`는 값을 깊게 병합. |
| 사용자 정의 정책 | 조직별 권한 및 도구 정책 |

### 예시: macOS Plist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>disableBypassPermissionsMode</key>
  <true/>
  <key>availableModels</key>
  <array>
    <string>claude-sonnet-4-6</string>
    <string>claude-haiku-4-5</string>
  </array>
</dict>
</plist>
```

---

## 설정

### 설정 파일 위치

1. **전역 설정**: `~/.claude/config.json`
2. **프로젝트 설정**: `./.claude/config.json`
3. **사용자 설정**: `~/.config/claude-code/settings.json`

### 전체 설정 예시

**핵심 고급 기능 설정:**

```json
{
  "permissions": {
    "mode": "default"
  },
  "hooks": {
    "PreToolUse:Edit": "eslint --fix ${file_path}",
    "PostToolUse:Write": "~/.claude/hooks/security-scan.sh"
  },
  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"]
      }
    }
  }
}
```

**확장 설정 예시:**

```json
{
  "permissions": {
    "mode": "default",
    "allowedTools": ["Bash(git log:*)", "Read"],
    "disallowedTools": ["Bash(rm -rf:*)"]
  },

  "hooks": {
    "PreToolUse": [{ "matcher": "Edit", "hooks": ["eslint --fix ${file_path}"] }],
    "PostToolUse": [{ "matcher": "Write", "hooks": ["~/.claude/hooks/security-scan.sh"] }],
    "Stop": [{ "hooks": ["~/.claude/hooks/notify.sh"] }]
  },

  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_TOKEN": "${GITHUB_TOKEN}"
        }
      }
    }
  }
}
```

### 폴백 모델 (`fallbackModel`)

`fallbackModel` 설정을 사용하면 기본 모델이 과부하되거나 사용 불가능할 때 **최대 세 개**의 폴백 모델을 순서대로 시도하도록 설정할 수 있습니다.

```json
{
  "fallbackModel": ["claude-opus-4-8", "claude-sonnet-4-6", "claude-haiku-4-5"]
}
```

**v2.1.166**부터 `--fallback-model` 플래그가 대화형 세션에도 적용됩니다(헤드리스만이 아님). 폴백 시 Claude Code는 예상치 못한 재시도 불가능 오류를 한 번 재시도합니다; 인증, 속도 제한, 요청 크기, 전송 오류는 여전히 즉시 실패합니다.

### 환경 변수

설정을 환경 변수로 재정의:

```bash
# 모델 선택
export ANTHROPIC_MODEL=claude-opus-4-8
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-8
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-6
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-haiku-4-5

# API 설정
export ANTHROPIC_API_KEY=sk-ant-...

# 생각 설정
export MAX_THINKING_TOKENS=16000
export CLAUDE_CODE_EFFORT_LEVEL=high   # low, medium, high, xhigh (Opus 4.8/4.7), 또는 max — Opus 4.8 기본값은 high (Opus 4.8, Opus 4.7, Opus 4.6, Sonnet 4.6 지원)

# 기능 토글
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=true
export CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true
export CLAUDE_CODE_DISABLE_CRON=1
export CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS=true
export CLAUDE_CODE_DISABLE_TERMINAL_TITLE=true
export CLAUDE_CODE_DISABLE_1M_CONTEXT=true
export CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK=true
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
export CLAUDE_CODE_ENABLE_TASKS=true
export CLAUDE_CODE_SIMPLE=true              # --bare 플래그로 설정

# MCP 설정
export MAX_MCP_OUTPUT_TOKENS=50000
export ENABLE_TOOL_SEARCH=true

# 프롬프트 캐싱
export ENABLE_PROMPT_CACHING_1H=1      # 1시간 프롬프트 캐시 TTL 사용 (기본값은 5분)

# 작업 관리
export CLAUDE_CODE_TASK_LIST_ID=my-project-tasks

# 에이전트 팀 (실험적)
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1

# Subagent 및 플러그인 설정
export CLAUDE_CODE_SUBAGENT_MODEL=sonnet
export CLAUDE_CODE_PLUGIN_SEED_DIR=./my-plugins
export CLAUDE_CODE_NEW_INIT=1

# 서브프로세스 및 스트리밍
export CLAUDE_CODE_SUBPROCESS_ENV_SCRUB="SECRET_KEY,DB_PASSWORD"
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=80
export CLAUDE_STREAM_IDLE_TIMEOUT_MS=30000
export ANTHROPIC_CUSTOM_MODEL_OPTION=my-custom-model
export SLASH_COMMAND_TOOL_CHAR_BUDGET=50000

# 출력 및 패키지 관리자 (v2.1.129+)
export CLAUDE_CODE_FORCE_SYNC_OUTPUT=1                      # 자동 감지가 실패하는 터미널(Emacs eat 등)에 대해 동기 출력 강제
export CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE=1            # Homebrew/WinGet 설치에 대한 백그라운드 업그레이드 활성화
export CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1         # ANTHROPIC_BASE_URL이 설정된 경우 /v1/models 게이트웨이 검색 옵트인

# Windows PowerShell 도구 (v2.1.143+) — Windows의 Bedrock/Vertex/Foundry에서 기본 켜짐
export CLAUDE_CODE_USE_POWERSHELL_TOOL=0                    # PowerShell 도구 완전 비활성화
export CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1    # `-ExecutionPolicy Bypass` 대신 시스템 ExecutionPolicy 준수

# 워크로드 아이덴티티 페더레이션 (v2.1.141+)
export ANTHROPIC_WORKSPACE_ID=ws_abc123                     # 규칙이 여러 개를 커버할 때 페더레이션 토큰을 특정 워크스페이스로 범위 지정

# 정지 훅 안전 캡 (v2.1.143+)
export CLAUDE_CODE_STOP_HOOK_BLOCK_CAP=8                    # 세션이 경고와 함께 종료되기 전 최대 연속 정지-훅 블록 수. 캡을 비활성화하려면 0 설정.
```

> **v2.1.108**: `ENABLE_PROMPT_CACHING_1H=1` — 기본 5분 TTL 대신 1시간 프롬프트 캐시 TTL 사용. 길고 안정적인 세션에서 캐시 미스를 줄입니다. (v2.1.129는 1시간 TTL이 조용히 5분으로 다운그레이드되는 회귀를 수정합니다.)

> **v2.1.129**: `CLAUDE_CODE_FORCE_SYNC_OUTPUT=1`은 기능 자동 감지가 실패하는 터미널(예: Emacs `eat`)에 대해 동기 출력을 강제합니다. `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE=1`은 그렇지 않으면 절대 자동 업데이트되지 않는 Homebrew/WinGet 설치에서 백그라운드 업그레이드를 활성화합니다.

### 설정 관리 명령어

```
User: /config
[대화형 설정 메뉴 열림]
```

`/config` 명령어는 다음과 같은 설정을 전환하는 대화형 메뉴를 제공합니다:
- Extended thinking 켜기/끄기
- 상세 출력
- Permission mode
- 모델 선택

대화형 메뉴에서 Enter 또는 Space를 눌러 선택한 설정을 변경하고, Esc를 눌러 저장하고 닫습니다(v2.1.183+).

메뉴를 열지 않고 프롬프트에서 직접 설정할 수도 있습니다:

```bash
/config thinking=false      # 인라인으로 단일 설정 (v2.1.181+)
/config --help              # 사용 가능한 단축키 목록 (v2.1.183+)
```

`key=value` 단축키는 대화형 세션, `-p`, Remote Control에서 작동합니다.

### 프로젝트별 설정

프로젝트에 `.claude/config.json` 생성:

```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Bash", "hooks": ["npm test && npm run lint"] }]
  },
  "permissions": {
    "mode": "default"
  },
  "mcp": {
    "servers": {
      "project-db": {
        "command": "mcp-postgres",
        "env": {
          "DATABASE_URL": "${PROJECT_DB_URL}"
        }
      }
    }
  }
}
```

---

## 에이전트 팀

에이전트 팀은 여러 Claude Code 인스턴스가 작업에 협업할 수 있는 실험적 기능입니다. 기본적으로 비활성화되어 있습니다.

### 에이전트 팀 활성화

환경 변수 또는 설정을 통해 활성화:

```bash
# 환경 변수
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

또는 설정 JSON에 추가:

```json
{
  "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
}
```

### 에이전트 팀 작동 방식

- **팀 리더**가 전체 작업을 조정하고 팀원에게 하위 작업 위임
- **팀원**은 각자 고유한 컨텍스트 창으로 독립적으로 작업
- **공유 작업 목록**이 팀원 간의 자체 조정 가능
- Subagent 정의(`.claude/agents/` 또는 `--agents` 플래그)를 사용하여 팀원 역할 및 전문화 정의

### 표시 모드

에이전트 팀은 `--teammate-mode` 플래그로 설정되는 두 가지 표시 모드를 지원:

| 모드 | 설명 |
|------|------|
| `in-process` (기본값) | 팀원이 동일한 터미널 프로세스 내에서 실행 |
| `tmux` | 각 팀원이 전용 분할 창을 가짐 (tmux 또는 iTerm2 필요) |
| `auto` | 최적의 표시 모드 자동 선택 |

```bash
# 팀원 표시에 tmux 분할 창 사용
claude --teammate-mode tmux

# 명시적으로 in-process 모드 사용
claude --teammate-mode in-process
```

### 사용 사례

- 다른 팀원이 다른 모듈을 처리하는 대규모 리팩토링 작업
- 병렬 코드 리뷰 및 구현
- 코드베이스 전반에 걸친 조정된 다중 파일 변경

> **참고**: 에이전트 팀은 실험적이며 향후 릴리스에서 변경될 수 있습니다. 전체 참조는 [code.claude.com/docs/en/agent-teams](https://code.claude.com/docs/en/agent-teams)를 참조하세요.

---

## 모범 사례

### Planning Mode
- ✅ 복잡한 다단계 작업에 사용
- ✅ 승인 전에 계획 검토
- ✅ 필요시 계획 수정
- ❌ 간단한 작업에는 사용하지 않음

### Extended Thinking
- ✅ 아키텍처 결정에 사용
- ✅ 복잡한 문제 해결에 사용
- ✅ 추론 과정 검토
- ❌ 간단한 질문에는 사용하지 않음

### Background Tasks
- ✅ 장기 실행 작업에 사용
- ✅ 작업 진행 상황 모니터링
- ✅ 작업 실패를 우아하게 처리
- ❌ 너무 많은 동시 작업 시작 금지

### Permissions
- ✅ 코드 리뷰에는 `plan` 사용 (읽기 전용)
- ✅ 대화형 개발에는 `default` 사용
- ✅ 자동화 워크플로우에는 `acceptEdits` 사용
- ✅ 안전 가드레일이 있는 자율 작업에는 `auto` 사용
- ❌ 절대적으로 필요하지 않으면 `bypassPermissions` 사용 금지

### Sessions
- ✅ 다른 작업에 별도 세션 사용
- ✅ 중요한 세션 상태 저장
- ✅ 오래된 세션 정리
- ✅ 한 세션에 관련 없는 작업 혼합 금지

---

## 추가 자료

Claude Code 및 관련 기능에 대한 자세한 정보:

- [공식 Interactive Mode 문서](https://code.claude.com/docs/en/interactive-mode)
- [공식 Headless Mode 문서](https://code.claude.com/docs/en/headless)
- [CLI 참조](https://code.claude.com/docs/en/cli-reference)
- [체크포인트 가이드](../08-checkpoints/) - 세션 관리 및 되감기
- [Slash Commands](../01-slash-commands/) - 명령어 참조
- [메모리 가이드](../02-memory/) - 영구적 컨텍스트
- [스킬 가이드](../03-skills/) - 자율 기능
- [Subagents 가이드](../04-subagents/) - 위임된 작업 실행
- [MCP 가이드](../05-mcp/) - 외부 데이터 접근
- [Hooks 가이드](../06-hooks/) - 이벤트 기반 자동화
- [플러그인 가이드](../07-plugins/) - 번들 확장
- [공식 예약 작업 문서](https://code.claude.com/docs/en/scheduled-tasks)
- [공식 Chrome 통합 문서](https://code.claude.com/docs/en/chrome)
- [공식 원격 제어 문서](https://code.claude.com/docs/en/remote-control)
- [공식 키바인딩 문서](https://code.claude.com/docs/en/keybindings)
- [공식 데스크탑 앱 문서](https://code.claude.com/docs/en/desktop)
- [공식 에이전트 팀 문서](https://code.claude.com/docs/en/agent-teams)

---

**마지막 업데이트**: 2026년 6월 28일
**Claude Code 버전**: 2.1.195
**출처**:
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://docs.anthropic.com/en/docs/claude-code/settings
- https://code.claude.com/docs/en/troubleshooting
- https://code.claude.com/docs/en/changelog#2-1-175
- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/interactive-mode
- https://code.claude.com/docs/en/settings
- https://code.claude.com/docs/en/cli-reference
- https://code.claude.com/docs/en/model-config
- https://www.anthropic.com/news/claude-opus-4-8
- https://claude.com/blog/introducing-routines-in-claude-code
- https://github.com/anthropics/claude-code/releases/tag/v2.1.117
- https://github.com/anthropics/claude-code/releases/tag/v2.1.139
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/sub-agents
**호환 모델**: Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
