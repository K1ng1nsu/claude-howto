<!-- i18n-source: claude_concepts_guide.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Claude 개념 완벽 가이드

슬래시 명령어, 서브에이전트, 메모리, MCP 프로토콜, 에이전트 스킬을 표, 다이어그램, 실용적인 예제와 함께 다루는 종합 참조 가이드입니다.

---

## 목차

1. [슬래시 명령어](#슬래시-명령어)
2. [서브에이전트](#서브에이전트)
3. [메모리](#메모리)
4. [MCP 프로토콜](#mcp-프로토콜)
5. [에이전트 스킬](#에이전트-스킬)
6. [플러그인](#claude-code-플러그인)
7. [훅](#훅)
8. [체크포인트 및 되감기](#체크포인트-및-되감기)
9. [고급 기능](#고급-기능)
10. [비교 및 통합](#비교-및-통합)

---

## 슬래시 명령어

### 개요

슬래시 명령어는 사용자가 호출하는 단축키로, Markdown 파일로 저장되어 Claude Code가 실행할 수 있습니다. 팀이 자주 사용하는 프롬프트와 워크플로우를 표준화할 수 있게 해줍니다.

### 아키텍처

```mermaid
graph TD
    A["User Input: /command-name"] -->|Triggers| B["Search .claude/commands/"]
    B -->|Finds| C["command-name.md"]
    C -->|Loads| D["Markdown Content"]
    D -->|Executes| E["Claude Processes Prompt"]
    E -->|Returns| F["Result in Context"]
```

### 파일 구조

```mermaid
graph LR
    A["Project Root"] -->|contains| B[".claude/commands/"]
    B -->|contains| C["optimize.md"]
    B -->|contains| D["test.md"]
    B -->|contains| E["docs/"]
    E -->|contains| F["generate-api-docs.md"]
    E -->|contains| G["generate-readme.md"]
```

### 명령어 구성 표

| 위치 | 범위 | 가용성 | 사용 사례 | Git 추적 |
|----------|-------|--------------|----------|-------------|
| `.claude/commands/` | 프로젝트별 | 팀 구성원 | 팀 워크플로우, 공유 표준 | ✅ 예 |
| `~/.claude/commands/` | 개인 | 개인 사용자 | 프로젝트 간 개인 단축키 | ❌ 아니오 |
| 하위 디렉터리 | 네임스페이스 | 상위 기준 | 카테고리별 구성 | ✅ 예 |

### 기능 및 역량

| 기능 | 예시 | 지원 |
|---------|---------|-----------|
| 셸 스크립트 실행 | `bash scripts/deploy.sh` | ✅ 예 |
| 파일 참조 | `@path/to/file.js` | ✅ 예 |
| Bash 통합 | `$(git log --oneline)` | ✅ 예 |
| 인수 | `/pr --verbose` | ✅ 예 |
| MCP 명령어 | `/mcp__github__list_prs` | ✅ 예 |

### 실용 예제

#### 예제 1: 코드 최적화 명령어

**파일:** `.claude/commands/optimize.md`

```markdown
---
name: 코드 최적화
description: 코드의 성능 문제를 분석하고 최적화를 제안합니다
tags: 성능, 분석
---

# 코드 최적화

제공된 코드를 다음 우선순위로 검토하세요:

1. **성능 병목** - O(n²) 연산, 비효율적인 루프 식별
2. **메모리 누수** - 해제되지 않은 리소스, 순환 참조 찾기
3. **알고리즘 개선** - 더 나은 알고리즘 또는 자료 구조 제안
4. **캐싱 기회** - 반복되는 계산 식별
5. **동시성 문제** - 경쟁 조건 또는 스레딩 문제 찾기

다음 형식으로 응답하세요:
- 문제 심각도 (치명적/높음/중간/낮음)
- 코드 내 위치
- 설명
- 코드 예제가 포함된 권장 수정사항
```

**사용법:**
```bash
# 사용자가 Claude Code에 입력
/optimize

# Claude가 프롬프트를 로드하고 코드 입력을 기다림
```

#### 예제 2: 풀 리퀘스트 도우미 명령어

**파일:** `.claude/commands/pr.md`

```markdown
---
name: 풀 리퀘스트 준비
description: 코드 정리, 변경사항 스테이징, 풀 리퀘스트 준비
tags: git, 워크플로우
---

# 풀 리퀘스트 준비 체크리스트

PR을 생성하기 전에 다음 단계를 실행하세요:

1. 린트 실행: `prettier --write .`
2. 테스트 실행: `npm test`
3. git diff 검토: `git diff HEAD`
4. 변경사항 스테이징: `git add .`
5. 컨벤셔널 커밋에 따라 커밋 메시지 작성:
   - `fix:` 버그 수정
   - `feat:` 새로운 기능
   - `docs:` 문서
   - `refactor:` 코드 구조 개선
   - `test:` 테스트 추가
   - `chore:` 유지보수

6. PR 요약 생성:
   - 변경된 사항
   - 변경 이유
   - 수행된 테스트
   - 잠재적 영향
```

**사용법:**
```bash
/pr

# Claude가 체크리스트를 실행하고 PR을 준비
```

#### 예제 3: 계층형 문서 생성기

**파일:** `.claude/commands/docs/generate-api-docs.md`

```markdown
---
name: API 문서 생성
description: 소스 코드에서 포괄적인 API 문서를 생성합니다
tags: 문서, api
---

# API 문서 생성기

다음 방법으로 API 문서를 생성하세요:

1. `/src/api/`의 모든 파일 스캔
2. 함수 시그니처 및 JSDoc 주석 추출
3. 엔드포인트/모듈별로 구성
4. 예제가 포함된 마크다운 생성
5. 요청/응답 스키마 포함
6. 오류 문서 추가

출력 형식:
- `/docs/api.md`의 마크다운 파일
- 모든 엔드포인트에 curl 예제 포함
- TypeScript 타입 추가
```

### 명령어 생명주기 다이어그램

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant FS as File System
    participant CLI as Shell/Bash

    User->>Claude: Types /optimize
    Claude->>FS: Searches .claude/commands/
    FS-->>Claude: Returns optimize.md
    Claude->>Claude: Loads Markdown content
    Claude->>User: Displays prompt context
    User->>Claude: Provides code to analyze
    Claude->>CLI: (May execute scripts)
    CLI-->>Claude: Results
    Claude->>User: Returns analysis
```

### 모범 사례

| ✅ 권장 | ❌ 권장하지 않음 |
|------|---------|
| 명확하고 행동 지향적인 이름 사용 | 일회성 작업을 위한 명령어 생성 |
| 설명에 트리거 단어 문서화 | 명령어에 복잡한 로직 구축 |
| 명령어를 단일 작업에 집중 | 중복 명령어 생성 |
| 프로젝트 명령어 버전 관리 | 민감한 정보 하드코딩 |
| 하위 디렉터리로 구성 | 긴 명령어 목록 생성 |
| 간단하고 읽기 쉬운 프롬프트 사용 | 축약어나 난해한 표현 사용 |

---

## 서브에이전트

### 개요

서브에이전트는 격리된 컨텍스트 창과 사용자 정의 시스템 프롬프트를 가진 특수 AI 어시스턴트입니다. 관심사의 명확한 분리를 유지하면서 위임된 작업 실행을 가능하게 합니다.

**v2.1.172**부터 서브에이전트는 자신의 서브에이전트를 생성할 수 있으며, **최대 5단계 깊이**까지 중첩이 가능합니다. 따라서 아래 표시된 단일 메인→서브에이전트 계층으로 제한되지 않습니다. 이전 버전에서는 중첩이 허용되지 않았습니다.

### 아키텍처 다이어그램

```mermaid
graph TB
    User["👤 User"]
    Main["🎯 Main Agent<br/>(Coordinator)"]
    Reviewer["🔍 Code Reviewer<br/>Subagent"]
    Tester["✅ Test Engineer<br/>Subagent"]
    Docs["📝 Documentation<br/>Subagent"]

    User -->|asks| Main
    Main -->|delegates| Reviewer
    Main -->|delegates| Tester
    Main -->|delegates| Docs
    Reviewer -->|returns result| Main
    Tester -->|returns result| Main
    Docs -->|returns result| Main
    Main -->|synthesizes| User
```

### 서브에이전트 생명주기

```mermaid
sequenceDiagram
    participant User
    participant MainAgent as Main Agent
    participant CodeReviewer as Code Reviewer<br/>Subagent
    participant Context as Separate<br/>Context Window

    User->>MainAgent: "Build new auth feature"
    MainAgent->>MainAgent: Analyze task
    MainAgent->>CodeReviewer: "Review this code"
    CodeReviewer->>Context: Initialize clean context
    Context->>CodeReviewer: Load reviewer instructions
    CodeReviewer->>CodeReviewer: Perform review
    CodeReviewer-->>MainAgent: Return findings
    MainAgent->>MainAgent: Incorporate results
    MainAgent-->>User: Provide synthesis
```

### 서브에이전트 설정 표

| 설정 | 타입 | 목적 | 예시 |
|---------------|------|---------|---------|
| `name` | String | 에이전트 식별자 | `code-reviewer` |
| `description` | String | 목적 및 트리거 용어 | `종합적인 코드 품질 분석` |
| `tools` | List/String | 허용된 기능 | `read, grep, diff, lint_runner` |
| `system_prompt` | Markdown | 행동 지침 | 사용자 정의 가이드라인 |

### 도구 접근 계층

```mermaid
graph TD
    A["Subagent Configuration"] -->|Option 1| B["Inherit All Tools<br/>from Main Thread"]
    A -->|Option 2| C["Specify Individual Tools"]
    B -->|Includes| B1["File Operations"]
    B -->|Includes| B2["Shell Commands"]
    B -->|Includes| B3["MCP Tools"]
    C -->|Explicit List| C1["read, grep, diff"]
    C -->|Explicit List| C2["Bash(npm:*), Bash(test:*)"]
```

### 실용 예제

#### 예제 1: 완전한 서브에이전트 설정

**파일:** `.claude/agents/code-reviewer.md`

```yaml
---
name: code-reviewer
description: 종합적인 코드 품질 및 유지보수성 분석
tools: read, grep, diff, lint_runner
---

# 코드 리뷰어 에이전트

당신은 다음 분야를 전문으로 하는 코드 리뷰 전문가입니다:
- 성능 최적화
- 보안 취약점
- 코드 유지보수성
- 테스트 커버리지
- 디자인 패턴

## 리뷰 우선순위 (순서대로)

1. **보안 문제** - 인증, 권한 부여, 데이터 노출
2. **성능 문제** - O(n²) 연산, 메모리 누수, 비효율적인 쿼리
3. **코드 품질** - 가독성, 네이밍, 문서화
4. **테스트 커버리지** - 누락된 테스트, 엣지 케이스
5. **디자인 패턴** - SOLID 원칙, 아키텍처

## 리뷰 출력 형식

각 문제에 대해:
- **심각도**: 치명적 / 높음 / 중간 / 낮음
- **카테고리**: 보안 / 성능 / 품질 / 테스트 / 디자인
- **위치**: 파일 경로 및 라인 번호
- **문제 설명**: 무엇이 잘못되었고 왜 그런지
- **제안된 수정**: 코드 예제
- **영향**: 시스템에 미치는 영향

## 리뷰 예시

### 문제: N+1 쿼리 문제
- **심각도**: 높음
- **카테고리**: 성능
- **위치**: src/user-service.ts:45
- **문제**: 루프가 각 반복에서 데이터베이스 쿼리 실행
- **수정**: JOIN 또는 배치 쿼리 사용
```

**파일:** `.claude/agents/test-engineer.md`

```yaml
---
name: test-engineer
description: 테스트 전략, 커버리지 분석, 자동화 테스트
tools: read, write, bash, grep
---

# 테스트 엔지니어 에이전트

당신은 다음 분야의 전문가입니다:
- 포괄적인 테스트 스위트 작성
- 높은 코드 커버리지 보장 (>80%)
- 엣지 케이스 및 오류 시나리오 테스트
- 성능 벤치마킹
- 통합 테스트

## 테스트 전략

1. **단위 테스트** - 개별 함수/메서드
2. **통합 테스트** - 컴포넌트 상호작용
3. **종단간 테스트** - 완전한 워크플로우
4. **엣지 케이스** - 경계 조건
5. **오류 시나리오** - 실패 처리

## 테스트 출력 요구사항

- JavaScript/TypeScript에는 Jest 사용
- 각 테스트에 setup/teardown 포함
- 외부 의존성 모킹
- 테스트 목적 문서화
- 관련 시 성능 단언문 포함

## 커버리지 요구사항

- 최소 80% 코드 커버리지
- 중요 경로는 100%
- 누락된 커버리지 영역 보고
```

**파일:** `.claude/agents/documentation-writer.md`

```yaml
---
name: documentation-writer
description: 기술 문서, API 문서, 사용자 가이드
tools: read, write, grep
---

# 문서 작성 에이전트

당신은 다음을 생성합니다:
- 예제가 포함된 API 문서
- 사용자 가이드 및 튜토리얼
- 아키텍처 문서
- 변경 로그 항목
- 코드 주석 개선

## 문서 표준

1. **명확성** - 간단하고 명확한 언어 사용
2. **예제** - 실용적인 코드 예제 포함
3. **완전성** - 모든 매개변수와 반환값 포함
4. **구조** - 일관된 형식 사용
5. **정확성** - 실제 코드와 비교하여 검증

## 문서 섹션

### API의 경우
- 설명
- 매개변수 (타입 포함)
- 반환값 (타입 포함)
- 예외 (가능한 오류)
- 예제 (curl, JavaScript, Python)
- 관련 엔드포인트

### 기능의 경우
- 개요
- 전제 조건
- 단계별 지침
- 예상 결과
- 문제 해결
- 관련 주제
```

#### 예제 2: 서브에이전트 위임 실제 사례

```markdown
# 시나리오: 결제 기능 구축

## 사용자 요청
"Stripe와 통합되는 안전한 결제 처리 기능 구축"

## 메인 에이전트 흐름

1. **계획 단계**
   - 요구사항 이해
   - 필요한 작업 결정
   - 아키텍처 계획

2. **코드 리뷰어 서브에이전트에 위임**
   - 작업: "보안 측면에서 결제 처리 구현 검토"
   - 컨텍스트: 인증, API 키, 토큰 처리
   - 검토 항목: SQL 인젝션, 키 노출, HTTPS 적용

3. **테스트 엔지니어 서브에이전트에 위임**
   - 작업: "결제 흐름에 대한 포괄적인 테스트 생성"
   - 컨텍스트: 성공 시나리오, 실패, 엣지 케이스
   - 테스트 생성: 유효한 결제, 거절된 카드, 네트워크 실패, 웹훅

4. **문서 작성 서브에이전트에 위임**
   - 작업: "결제 API 엔드포인트 문서화"
   - 컨텍스트: 요청/응답 스키마
   - 결과물: curl 예제, 오류 코드가 포함된 API 문서

5. **종합**
   - 메인 에이전트가 모든 출력 수집
   - 결과 통합
   - 완전한 솔루션을 사용자에게 반환
```

#### 예제 3: 도구 권한 범위 지정

**제한적 설정 - 특정 명령어로 제한**

```yaml
---
name: secure-reviewer
description: 최소 권한으로 보안 중심 코드 리뷰
tools: read, grep
---

# 보안 코드 리뷰어

보안 취약점만 검토합니다.

이 에이전트:
- ✅ 분석할 파일 읽기
- ✅ 패턴 검색
- ❌ 코드를 실행할 수 없음
- ❌ 파일을 수정할 수 없음
- ❌ 테스트를 실행할 수 없음

리뷰어가 실수로 무언가를 망가뜨리지 않도록 보장합니다.
```

**확장 설정 - 구현을 위한 모든 도구**

```yaml
---
name: implementation-agent
description: 기능 개발을 위한 전체 구현 역량
tools: read, write, bash, grep, edit, glob
---

# 구현 에이전트

명세서에서 기능을 구축합니다.

이 에이전트:
- ✅ 명세서 읽기
- ✅ 새 코드 파일 작성
- ✅ 빌드 명령어 실행
- ✅ 코드베이스 검색
- ✅ 기존 파일 편집
- ✅ 패턴과 일치하는 파일 찾기

독립적인 기능 개발을 위한 전체 역량.
```

### 서브에이전트 컨텍스트 관리

```mermaid
graph TB
    A["Main Agent Context<br/>50,000 tokens"]
    B["Subagent 1 Context<br/>20,000 tokens"]
    C["Subagent 2 Context<br/>20,000 tokens"]
    D["Subagent 3 Context<br/>20,000 tokens"]

    A -->|Clean slate| B
    A -->|Clean slate| C
    A -->|Clean slate| D

    B -->|Results only| A
    C -->|Results only| A
    D -->|Results only| A

    style A fill:#e1f5ff
    style B fill:#fff9c4
    style C fill:#fff9c4
    style D fill:#fff9c4
```

### 서브에이전트 사용 시기

| 시나리오 | 서브에이전트 사용 | 이유 |
|----------|--------------|-----|
| 여러 단계가 있는 복잡한 기능 | ✅ 예 | 관심사 분리, 컨텍스트 오염 방지 |
| 빠른 코드 리뷰 | ❌ 아니오 | 불필요한 오버헤드 |
| 병렬 작업 실행 | ✅ 예 | 각 서브에이전트가 자체 컨텍스트 보유 |
| 전문 지식 필요 | ✅ 예 | 사용자 정의 시스템 프롬프트 |
| 장기 실행 분석 | ✅ 예 | 메인 컨텍스트 소진 방지 |
| 단일 작업 | ❌ 아니오 | 불필요하게 지연 추가 |

### 에이전트 팀

에이전트 팀은 관련 작업을 수행하는 여러 에이전트를 조정합니다. 한 번에 하나의 서브에이전트에 위임하는 대신, 에이전트 팀은 메인 에이전트가 협력하고, 중간 결과를 공유하며, 공통 목표를 향해 작업하는 에이전트 그룹을 조율할 수 있게 해줍니다. 이는 프론트엔드 에이전트, 백엔드 에이전트, 테스트 에이전트가 병렬로 작업하는 풀스택 기능 개발과 같은 대규모 작업에 유용합니다.

---

## 메모리

### 개요

메모리는 Claude가 세션과 대화에 걸쳐 컨텍스트를 유지할 수 있게 해줍니다. claude.ai의 자동 합성과 Claude Code의 파일 시스템 기반 CLAUDE.md의 두 가지 형태로 존재합니다.

### 메모리 아키텍처

```mermaid
graph TB
    A["Claude Session"]
    B["User Input"]
    C["Memory System"]
    D["Memory Storage"]

    B -->|User provides info| C
    C -->|Synthesizes every 24h| D
    D -->|Loads automatically| A
    A -->|Uses context| C
```

### Claude Code의 메모리 계층 (7단계)

Claude Code는 7단계에서 메모리를 로드하며, 우선순위가 높은 순서대로 나열됩니다:

```mermaid
graph TD
    A["1. Managed Policy<br/>Enterprise admin policies"] --> B["2. Project Memory<br/>./CLAUDE.md"]
    B --> C["3. Project Rules<br/>.claude/rules/*.md"]
    C --> D["4. User Memory<br/>~/.claude/CLAUDE.md"]
    D --> E["5. User Rules<br/>~/.claude/rules/*.md"]
    E --> F["6. Local Memory<br/>.claude/local/CLAUDE.md"]
    F --> G["7. Auto Memory<br/>Automatically captured preferences"]

    style A fill:#fce4ec,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#e1f5fe,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
    style G fill:#fff3e0,stroke:#333,color:#333
```

### 메모리 위치 표

| 단계 | 위치 | 범위 | 우선순위 | 공유 | 최적 용도 |
|------|----------|-------|----------|--------|----------|
| 1. 관리 정책 | 엔터프라이즈 관리 | 조직 | 가장 높음 | 모든 조직 사용자 | 규정 준수, 보안 정책 |
| 2. 프로젝트 | `./CLAUDE.md` | 프로젝트 | 높음 | 팀 (Git) | 팀 표준, 아키텍처 |
| 3. 프로젝트 규칙 | `.claude/rules/*.md` | 프로젝트 | 높음 | 팀 (Git) | 모듈식 프로젝트 규칙 |
| 4. 사용자 | `~/.claude/CLAUDE.md` | 개인 | 중간 | 개인 | 개인 선호도 |
| 5. 사용자 규칙 | `~/.claude/rules/*.md` | 개인 | 중간 | 개인 | 개인 규칙 모듈 |
| 6. 로컬 | `.claude/local/CLAUDE.md` | 로컬 | 낮음 | 공유되지 않음 | 머신별 설정 |
| 7. 자동 메모리 | 자동 | 세션 | 가장 낮음 | 개인 | 학습된 선호도, 패턴 |

### 자동 메모리

자동 메모리는 세션 중 관찰된 사용자 선호도와 패턴을 자동으로 캡처합니다. Claude는 상호작용에서 학습하고 다음을 기억합니다:

- 코딩 스타일 선호도
- 자주 하는 수정사항
- 프레임워크 및 도구 선택
- 커뮤니케이션 스타일 선호도

자동 메모리는 백그라운드에서 작동하며 수동 설정이 필요하지 않습니다.


### 메모리 업데이트 생명주기

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Editor as File System
    participant Memory as CLAUDE.md

    User->>Claude: "Remember: use async/await"
    Claude->>User: "Which memory file?"
    User->>Claude: "Project memory"
    Claude->>Editor: Open ~/.claude/settings.json
    Claude->>Memory: Write to ./CLAUDE.md
    Memory-->>Claude: File saved
    Claude->>Claude: Load updated memory
    Claude-->>User: "Memory saved!"
```

### 실용 예제

#### 예제 1: 프로젝트 메모리 구조

**파일:** `./CLAUDE.md`

```markdown
# 프로젝트 설정

## 프로젝트 개요
- **이름**: E-Commerce 플랫폼
- **기술 스택**: Node.js, PostgreSQL, React 18, Docker
- **팀 규모**: 5명의 개발자
- **마감일**: 2025년 4분기

## 아키텍처
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## 개발 표준

### 코드 스타일
- 포맷팅에 Prettier 사용
- airbnb 설정으로 ESLint 사용
- 최대 줄 길이: 100자
- 2칸 들여쓰기 사용

### 네이밍 규칙
- **파일**: kebab-case (user-controller.js)
- **클래스**: PascalCase (UserService)
- **함수/변수**: camelCase (getUserById)
- **상수**: UPPER_SNAKE_CASE (API_BASE_URL)
- **데이터베이스 테이블**: snake_case (user_accounts)

### Git 워크플로우
- 브랜치 이름: `feature/description` 또는 `fix/description`
- 커밋 메시지: 컨벤셔널 커밋 따르기
- 병합 전 PR 필수
- 모든 CI/CD 검사 통과 필수
- 최소 1명 승인 필수

### 테스트 요구사항
- 최소 80% 코드 커버리지
- 모든 중요 경로에 테스트 필수
- 단위 테스트에 Jest 사용
- E2E 테스트에 Cypress 사용
- 테스트 파일명: `*.test.ts` 또는 `*.spec.ts`

### API 표준
- RESTful 엔드포인트만 사용
- JSON 요청/응답
- HTTP 상태 코드 올바르게 사용
- API 엔드포인트 버전 관리: `/api/v1/`
- 모든 엔드포인트를 예제와 함께 문서화

### 데이터베이스
- 스키마 변경에 마이그레이션 사용
- 자격 증명을 하드코딩하지 않음
- 커넥션 풀링 사용
- 개발 환경에서 쿼리 로깅 활성화
- 정기 백업 필수

### 배포
- Docker 기반 배포
- Kubernetes 오케스트레이션
- 블루-그린 배포 전략
- 실패 시 자동 롤백
- 배포 전 데이터베이스 마이그레이션 실행

## 공통 명령어

| 명령어 | 목적 |
|---------|---------|
| `npm run dev` | 개발 서버 시작 |
| `npm test` | 테스트 스위트 실행 |
| `npm run lint` | 코드 스타일 검사 |
| `npm run build` | 프로덕션 빌드 |
| `npm run migrate` | 데이터베이스 마이그레이션 실행 |

## 팀 연락처
- 기술 리드: Sarah Chen (@sarah.chen)
- 제품 관리자: Mike Johnson (@mike.j)
- DevOps: Alex Kim (@alex.k)

## 알려진 문제 및 해결 방법
- 피크 시간에 PostgreSQL 커넥션 풀이 20으로 제한됨
- 해결 방법: 쿼리 큐잉 구현
- Safari 14 호환성 문제 (async generators)
- 해결 방법: Babel 트랜스파일러 사용

## 관련 프로젝트
- 분석 대시보드: `/projects/analytics`
- 모바일 앱: `/projects/mobile`
- 관리자 패널: `/projects/admin`
```

#### 예제 2: 디렉터리별 메모리

**파일:** `./src/api/CLAUDE.md`

~~~~markdown
# API 모듈 표준

이 파일은 /src/api/ 내의 모든 항목에 대해 루트 CLAUDE.md를 재정의합니다.

## API별 표준

### 요청 검증
- 스키마 검증에 Zod 사용
- 항상 입력 검증
- 검증 오류 시 400 반환
- 필드 수준 오류 세부 정보 포함

### 인증
- 모든 엔드포인트는 JWT 토큰 필요
- Authorization 헤더에 토큰 포함
- 토큰은 24시간 후 만료
- 리프레시 토큰 메커니즘 구현

### 응답 형식

모든 응답은 다음 구조를 따라야 합니다:

```json
{
  "success": true,
  "data": { /* 실제 데이터 */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```

### 오류 응답:
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "사용자 메시지",
    "details": { /* 필드 오류 */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### 페이지네이션
- 커서 기반 페이지네이션 사용 (오프셋 아님)
- `hasMore` 불리언 포함
- 최대 페이지 크기를 100으로 제한
- 기본 페이지 크기: 20

### 속도 제한
- 인증된 사용자: 시간당 1000개 요청
- 공개 엔드포인트: 시간당 100개 요청
- 초과 시 429 반환
- retry-after 헤더 포함

### 캐싱
- 세션 캐싱에 Redis 사용
- 캐시 기간: 기본 5분
- 쓰기 작업 시 무효화
- 리소스 유형으로 캐시 키 태깅
~~~~

#### 예제 3: 개인 메모리

**파일:** `~/.claude/CLAUDE.md`

~~~~markdown
# 내 개발 선호도

## 자기 소개
- **경력**: 8년 풀스택 개발
- **선호 언어**: TypeScript, Python
- **커뮤니케이션 스타일**: 직설적, 예제 포함
- **학습 스타일**: 코드와 함께 시각적 다이어그램

## 코드 선호도

### 오류 처리
try-catch 블록과 의미 있는 오류 메시지를 통한 명시적 오류 처리를 선호합니다.
일반적인 오류는 피합니다. 디버깅을 위해 항상 오류를 로깅합니다.

### 주석
WHAT이 아닌 WHY에 주석을 사용합니다. 코드는 자체 문서화되어야 합니다.
주석은 비즈니스 로직이나 명확하지 않은 결정을 설명해야 합니다.

### 테스트
TDD(테스트 주도 개발)를 선호합니다.
먼저 테스트를 작성하고, 그다음 구현을 작성합니다.
구현 세부 사항이 아닌 동작에 집중합니다.

### 아키텍처
모듈식의 느슨하게 결합된 디자인을 선호합니다.
테스트 용이성을 위해 의존성 주입을 사용합니다.
관심사 분리 (컨트롤러, 서비스, 리포지토리).

## 디버깅 선호도
- 접두사 `[DEBUG]`와 함께 console.log 사용
- 컨텍스트 포함: 함수 이름, 관련 변수
- 가능한 경우 스택 트레이스 사용
- 로그에 항상 타임스탬프 포함

## 커뮤니케이션
- 복잡한 개념을 다이어그램으로 설명
- 이론을 설명하기 전에 구체적인 예제 제시
- 이전/이후 코드 스니펫 포함
- 마지막에 주요 포인트 요약

## 프로젝트 구성
프로젝트를 다음과 같이 구성합니다:
```
project/
  ├── src/
  │   ├── api/
  │   ├── services/
  │   ├── models/
  │   └── utils/
  ├── tests/
  ├── docs/
  └── docker/
```

## 도구
- **IDE**: vim 키바인딩이 있는 VS Code
- **터미널**: Oh-My-Zsh가 있는 Zsh
- **포맷터**: Prettier (100자 줄 길이)
- **린터**: airbnb 설정의 ESLint
- **테스트 프레임워크**: React Testing Library가 있는 Jest
~~~~

#### 예제 4: 세션 중 메모리 업데이트

**세션 상호작용:**

```markdown
사용자: 모든 새 컴포넌트에 클래스 컴포넌트 대신 React 훅을 사용하는 것을
       선호한다고 기억해줘.

Claude: 메모리에 추가하고 있습니다. 어떤 메모리 파일에 저장할까요?
        1. 프로젝트 메모리 (./CLAUDE.md)
        2. 개인 메모리 (~/.claude/CLAUDE.md)

사용자: 프로젝트 메모리

Claude: ✅ 메모리 저장됨!

./CLAUDE.md에 추가됨:
---

### 컴포넌트 개발
- React Hooks와 함께 함수형 컴포넌트 사용
- 클래스 컴포넌트보다 훅 선호
- 재사용 가능한 로직을 위한 커스텀 훅
- 이벤트 핸들러에 useCallback 사용
- 비용이 많이 드는 계산에 useMemo 사용
```

### Claude 웹/데스크톱의 메모리

#### 메모리 합성 타임라인

```mermaid
graph LR
    A["Day 1: User<br/>Conversations"] -->|24 hours| B["Day 2: Memory<br/>Synthesis"]
    B -->|Automatic| C["Memory Updated<br/>Summarized"]
    C -->|Loaded in| D["Day 2-N:<br/>New Conversations"]
    D -->|Add to| E["Memory"]
    E -->|24 hours later| F["Memory Refreshed"]
```

**메모리 요약 예시:**

```markdown
## Claude의 사용자 메모리

### 전문 배경
- 8년 경력의 시니어 풀스택 개발자
- TypeScript/Node.js 백엔드 및 React 프론트엔드 중심
- 활발한 오픈 소스 기여자
- AI 및 머신러닝에 관심

### 프로젝트 컨텍스트
- 현재 전자상거래 플랫폼 구축 중
- 기술 스택: Node.js, PostgreSQL, React 18, Docker
- 5명의 개발자로 구성된 팀
- CI/CD 및 블루-그린 배포 사용

### 커뮤니케이션 선호도
- 직접적이고 간결한 설명 선호
- 시각적 다이어그램 및 예제 선호
- 코드 스니펫 선호
- 주석에 비즈니스 로직 설명

### 현재 목표
- API 성능 개선
- 테스트 커버리지를 90%로 증가
- 캐싱 전략 구현
- 아키텍처 문서화
```

### 메모리 기능 비교

| 기능 | Claude 웹/데스크톱 | Claude Code (CLAUDE.md) |
|---------|-------------------|------------------------|
| 자동 합성 | ✅ 24시간마다 | ❌ 수동 |
| 프로젝트 간 | ✅ 공유 | ❌ 프로젝트별 |
| 팀 액세스 | ✅ 공유 프로젝트 | ✅ Git 추적 |
| 검색 가능 | ✅ 내장 | ✅ `/memory`를 통해 |
| 편집 가능 | ✅ 채팅 내 | ✅ 직접 파일 편집 |
| 가져오기/내보내기 | ✅ 예 | ✅ 복사/붙여넣기 |
| 지속성 | ✅ 24시간 이상 | ✅ 무기한 |

---

## MCP 프로토콜

### 개요

MCP(Model Context Protocol)는 Claude가 외부 도구, API, 실시간 데이터 소스에 액세스할 수 있는 표준화된 방법입니다. 메모리와 달리 MCP는 변화하는 데이터에 대한 실시간 액세스를 제공합니다.

### MCP 아키텍처

```mermaid
graph TB
    A["Claude"]
    B["MCP Server"]
    C["External Service"]

    A -->|Request: list_issues| B
    B -->|Query| C
    C -->|Data| B
    B -->|Response| A

    A -->|Request: create_issue| B
    B -->|Action| C
    C -->|Result| B
    B -->|Response| A
```

### MCP 생태계

```mermaid
graph TB
    A["Claude"] -->|MCP| B["Filesystem<br/>MCP Server"]
    A -->|MCP| C["GitHub<br/>MCP Server"]
    A -->|MCP| D["Database<br/>MCP Server"]
    A -->|MCP| E["Slack<br/>MCP Server"]
    A -->|MCP| F["Google Docs<br/>MCP Server"]

    B -->|File I/O| G["Local Files"]
    C -->|API| H["GitHub Repos"]
    D -->|Query| I["PostgreSQL/MySQL"]
    E -->|Messages| J["Slack Workspace"]
    F -->|Docs| K["Google Drive"]
```

### MCP 설정 과정

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as Config File
    participant Service as External Service

    User->>Claude: Type /mcp
    Claude->>Claude: List available MCP servers
    Claude->>User: Show options
    User->>Claude: Select GitHub MCP
    Claude->>Config: Update configuration
    Config->>Claude: Activate connection
    Claude->>Service: Test connection
    Service-->>Claude: Authentication successful
    Claude->>User: ✅ MCP connected!
```

### 사용 가능한 MCP 서버 표

| MCP 서버 | 목적 | 일반 도구 | 인증 | 실시간 |
|------------|---------|--------------|------|-----------|
| **Filesystem** | 파일 작업 | read, write, delete | OS 권한 | ✅ 예 |
| **GitHub** | 저장소 관리 | list_prs, create_issue, push | OAuth | ✅ 예 |
| **Slack** | 팀 커뮤니케이션 | send_message, list_channels | 토큰 | ✅ 예 |
| **Database** | SQL 쿼리 | query, insert, update | 자격 증명 | ✅ 예 |
| **Google Docs** | 문서 액세스 | read, write, share | OAuth | ✅ 예 |
| **Asana** | 프로젝트 관리 | create_task, update_status | API 키 | ✅ 예 |
| **Stripe** | 결제 데이터 | list_charges, create_invoice | API 키 | ✅ 예 |
| **Memory** | 영구 메모리 | store, retrieve, delete | 로컬 | ❌ 아니오 |

### 실용 예제

#### 예제 1: GitHub MCP 설정

**파일:** `.mcp.json` (프로젝트 범위) 또는 `~/.claude.json` (사용자 범위)

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**사용 가능한 GitHub MCP 도구:**

~~~~markdown
# GitHub MCP 도구

## 풀 리퀘스트 관리
- `list_prs` - 저장소의 모든 PR 나열
- `get_pr` - PR 상세 정보 (diff 포함) 가져오기
- `create_pr` - 새 PR 생성
- `update_pr` - PR 설명/제목 업데이트
- `merge_pr` - PR을 메인 브랜치에 병합
- `review_pr` - 리뷰 코멘트 추가

요청 예시:
```
/mcp__github__get_pr 456

# 반환:
제목: 다크 모드 지원 추가
작성자: @alice
설명: CSS 변수를 사용하여 다크 테마 구현
상태: OPEN
리뷰어: @bob, @charlie
```

## 이슈 관리
- `list_issues` - 모든 이슈 나열
- `get_issue` - 이슈 상세 정보 가져오기
- `create_issue` - 새 이슈 생성
- `close_issue` - 이슈 닫기
- `add_comment` - 이슈에 코멘트 추가

## 저장소 정보
- `get_repo_info` - 저장소 상세 정보
- `list_files` - 파일 트리 구조
- `get_file_content` - 파일 내용 읽기
- `search_code` - 코드베이스 검색

## 커밋 작업
- `list_commits` - 커밋 기록
- `get_commit` - 특정 커밋 상세 정보
- `create_commit` - 새 커밋 생성
~~~~

#### 예제 2: 데이터베이스 MCP 설정

**설정:**

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    }
  }
}
```

**사용 예시:**

```markdown
사용자: 10개 이상의 주문을 한 모든 사용자 가져오기

Claude: 데이터베이스를 쿼리하여 해당 정보를 찾아보겠습니다.

# MCP 데이터베이스 도구 사용:
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# 결과:
- Alice: 15개 주문
- Bob: 12개 주문
- Charlie: 11개 주문
```

#### 예제 3: 다중 MCP 워크플로우

**시나리오: 일일 보고서 생성**

```markdown
# 여러 MCP를 사용한 일일 보고서 워크플로우

## 설정
1. GitHub MCP - PR 지표 가져오기
2. Database MCP - 판매 데이터 쿼리
3. Slack MCP - 보고서 게시
4. Filesystem MCP - 보고서 저장

## 워크플로우

### 1단계: GitHub 데이터 가져오기
/mcp__github__list_prs completed:true last:7days

출력:
- 총 PR: 42
- 평균 병합 시간: 2.3시간
- 리뷰 처리 시간: 1.1시간

### 2단계: 데이터베이스 쿼리
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

출력:
- 판매: 247
- 수익: $12,450

### 3단계: 보고서 생성
데이터를 HTML 보고서로 결합

### 4단계: 파일 시스템에 저장
report.html을 /reports/에 쓰기

### 5단계: Slack에 게시
요약을 #daily-reports 채널에 전송

최종 출력:
✅ 보고서 생성 및 게시됨
📊 이번 주 47개 PR 병합됨
💰 일일 판매 $12,450
```

#### 예제 4: Filesystem MCP 작업

**설정:**

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    }
  }
}
```

**사용 가능한 작업:**

| 작업 | 명령어 | 목적 |
|-----------|---------|---------|
| 파일 목록 | `ls ~/projects` | 디렉터리 내용 표시 |
| 파일 읽기 | `cat src/main.ts` | 파일 내용 읽기 |
| 파일 쓰기 | `create docs/api.md` | 새 파일 생성 |
| 파일 편집 | `edit src/app.ts` | 파일 수정 |
| 검색 | `grep "async function"` | 파일 내 검색 |
| 삭제 | `rm old-file.js` | 파일 삭제 |

### MCP vs 메모리: 결정 매트릭스

```mermaid
graph TD
    A["Need external data?"]
    A -->|No| B["Use Memory"]
    A -->|Yes| C["Does it change frequently?"]
    C -->|No/Rarely| B
    C -->|Yes/Often| D["Use MCP"]

    B -->|Stores| E["Preferences<br/>Context<br/>History"]
    D -->|Accesses| F["Live APIs<br/>Databases<br/>Services"]

    style B fill:#e1f5ff
    style D fill:#fff9c4
```

### 요청/응답 패턴

```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCP Server
    participant DB as Database

    App->>MCP: Request: "SELECT * FROM users WHERE id=1"
    MCP->>DB: Execute query
    DB-->>MCP: Result set
    MCP-->>App: Return parsed data
    App->>App: Process result
    App->>App: Continue task

    Note over MCP,DB: Real-time access<br/>No caching
```

---


## 에이전트 스킬

### 개요

에이전트 스킬은 지침, 스크립트, 리소스가 포함된 폴더로 패키징된 재사용 가능한 모델 호출 기능입니다. Claude는 관련 스킬을 자동으로 감지하고 사용합니다.

### 스킬 아키텍처

```mermaid
graph TB
    A["Skill Directory"]
    B["SKILL.md"]
    C["YAML Metadata"]
    D["Instructions"]
    E["Scripts"]
    F["Templates"]

    A --> B
    B --> C
    B --> D
    E --> A
    F --> A
```

### 스킬 로딩 과정

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude
    participant System as System
    participant Skill as Skill

    User->>Claude: "Create Excel report"
    Claude->>System: Scan available skills
    System->>System: Load skill metadata
    Claude->>Claude: Match user request to skills
    Claude->>Skill: Load xlsx skill SKILL.md
    Skill-->>Claude: Return instructions + tools
    Claude->>Claude: Execute skill
    Claude->>User: Generate Excel file
```

### 스킬 유형 및 위치 표

| 유형 | 위치 | 범위 | 공유 | 동기화 | 최적 용도 |
|------|----------|-------|--------|------|----------|
| 사전 구축 | 내장 | 전역 | 모든 사용자 | 자동 | 문서 생성 |
| 개인 | `~/.claude/skills/` | 개인 | 아니오 | 수동 | 개인 자동화 |
| 프로젝트 | `.claude/skills/` | 팀 | 예 | Git | 팀 표준 |
| 플러그인 | 플러그인 설치를 통해 | 다양함 | depende | 자동 | 통합 기능 |

### 사전 구축 스킬

```mermaid
graph TB
    A["Pre-built Skills"]
    B["PowerPoint (pptx)"]
    C["Excel (xlsx)"]
    D["Word (docx)"]
    E["PDF"]

    A --> B
    A --> C
    A --> D
    A --> E

    B --> B1["Create presentations"]
    B --> B2["Edit slides"]
    C --> C1["Create spreadsheets"]
    C --> C2["Analyze data"]
    D --> D1["Create documents"]
    D --> D2["Format text"]
    E --> E1["Generate PDFs"]
    E --> E2["Fill forms"]
```

### 번들 스킬

Claude Code에는 이제 기본 제공되는 5개의 번들 스킬이 포함되어 있습니다:

| 스킬 | 명령어 | 목적 |
|-------|---------|---------|
| **코드 리뷰** | `/code-review` | 선택한 노력 수준으로 현재 diff의 정확성 버그 검토 (v2.1.146에서 `/simplify`에서 이름 변경됨) |
| **Batch** | `/batch` | 여러 파일이나 항목에 걸쳐 작업 실행 |
| **Debug** | `/debug` | 근본 원인 분석을 통한 체계적인 문제 디버깅 |
| **Loop** | `/loop` | 타이머로 반복 작업 예약 |
| **Claude API** | `/claude-api` | Anthropic API와 직접 상호작용 |

이러한 번들 스킬은 항상 사용 가능하며 설치나 설정이 필요하지 않습니다.

### 실용 예제

#### 예제 1: 사용자 정의 코드 리뷰 스킬

**디렉터리 구조:**

```
~/.claude/skills/code-review-specialist/
├── SKILL.md
├── templates/
│   ├── review-checklist.md
│   └── finding-template.md
└── scripts/
    ├── analyze-metrics.py
    └── compare-complexity.py
```

**파일:** `~/.claude/skills/code-review-specialist/SKILL.md`

```yaml
---
name: 코드 리뷰 전문가
description: 보안, 성능 및 품질 분석을 포함한 종합적인 코드 리뷰
version: "1.0.0"
tags:
  - code-review
  - quality
  - security
when_to_use: 사용자가 코드 리뷰, 코드 품질 분석, 또는 풀 리퀘스트 평가를 요청할 때
effort: high
shell: bash
---

# 코드 리뷰 스킬

이 스킬은 다음에 중점을 둔 종합적인 코드 리뷰 기능을 제공합니다:

1. **보안 분석**
   - 인증/권한 부여 문제
   - 데이터 노출 위험
   - 인젝션 취약점
   - 암호화 취약점
   - 민감 데이터 로깅

2. **성능 리뷰**
   - 알고리즘 효율성 (Big O 분석)
   - 메모리 최적화
   - 데이터베이스 쿼리 최적화
   - 캐싱 기회
   - 동시성 문제

3. **코드 품질**
   - SOLID 원칙
   - 디자인 패턴
   - 네이밍 규칙
   - 문서화
   - 테스트 커버리지

4. **유지보수성**
   - 코드 가독성
   - 함수 크기 (50줄 미만이어야 함)
   - 순환 복잡도
   - 의존성 관리
   - 타입 안전성

## 리뷰 템플릿

각 리뷰 코드 조각에 대해 다음을 제공하세요:

### 요약
- 전반적인 품질 평가 (1-5)
- 주요 발견 사항 수
- 권장 우선 영역

### 중요 문제 (있는 경우)
- **문제**: 명확한 설명
- **위치**: 파일 및 라인 번호
- **영향**: 중요한 이유
- **심각도**: 치명적/높음/중간
- **수정**: 코드 예제

### 카테고리별 발견 사항

#### 보안 (문제 발견 시)
보안 취약점을 예제와 함께 나열

#### 성능 (문제 발견 시)
복잡도 분석과 함께 성능 문제 나열

#### 품질 (문제 발견 시)
리팩토링 제안과 함께 코드 품질 문제 나열

#### 유지보수성 (문제 발견 시)
개선 사항과 함께 유지보수성 문제 나열
```
## Python 스크립트: analyze-metrics.py

```python
#!/usr/bin/env python3
import re
import sys

def analyze_code_metrics(code):
    """Analyze code for common metrics."""

    # Count functions
    functions = len(re.findall(r'^def\s+\w+', code, re.MULTILINE))

    # Count classes
    classes = len(re.findall(r'^class\s+\w+', code, re.MULTILINE))

    # Average line length
    lines = code.split('\n')
    avg_length = sum(len(l) for l in lines) / len(lines) if lines else 0

    # Estimate complexity
    complexity = len(re.findall(r'(if|elif|else|for|while|and|or)', code))

    return {
        'functions': functions,
        'classes': classes,
        'avg_line_length': avg_length,
        'complexity_score': complexity
    }

if __name__ == '__main__':
    with open(sys.argv[1], 'r') as f:
        code = f.read()
    metrics = analyze_code_metrics(code)
    for key, value in metrics.items():
        print(f"{key}: {value:.2f}")
```

## Python 스크립트: compare-complexity.py

```python
#!/usr/bin/env python3
"""
Compare cyclomatic complexity of code before and after changes.
Helps identify if refactoring actually simplifies code structure.
"""

import re
import sys
from typing import Dict, Tuple

class ComplexityAnalyzer:
    """Analyze code complexity metrics."""

    def __init__(self, code: str):
        self.code = code
        self.lines = code.split('\n')

    def calculate_cyclomatic_complexity(self) -> int:
        """
        Calculate cyclomatic complexity using McCabe's method.
        Count decision points: if, elif, else, for, while, except, and, or
        """
        complexity = 1  # Base complexity

        # Count decision points
        decision_patterns = [
            r'if',
            r'elif',
            r'for',
            r'while',
            r'except',
            r'and(?!$)',
            r'or(?!$)'
        ]

        for pattern in decision_patterns:
            matches = re.findall(pattern, self.code)
            complexity += len(matches)

        return complexity

    def calculate_cognitive_complexity(self) -> int:
        """
        Calculate cognitive complexity - how hard is it to understand?
        Based on nesting depth and control flow.
        """
        cognitive = 0
        nesting_depth = 0

        for line in self.lines:
            # Track nesting depth
            if re.search(r'^\s*(if|for|while|def|class|try)', line):
                nesting_depth += 1
                cognitive += nesting_depth
            elif re.search(r'^\s*(elif|else|except|finally)', line):
                cognitive += nesting_depth

            # Reduce nesting when unindenting
            if line and not line[0].isspace():
                nesting_depth = 0

        return cognitive

    def calculate_maintainability_index(self) -> float:
        """
        Maintainability Index ranges from 0-100.
        > 85: Excellent
        > 65: Good
        > 50: Fair
        < 50: Poor
        """
        lines = len(self.lines)
        cyclomatic = self.calculate_cyclomatic_complexity()
        cognitive = self.calculate_cognitive_complexity()

        # Simplified MI calculation
        mi = 171 - 5.2 * (cyclomatic / lines) - 0.23 * (cognitive) - 16.2 * (lines / 1000)

        return max(0, min(100, mi))

    def get_complexity_report(self) -> Dict:
        """Generate comprehensive complexity report."""
        return {
            'cyclomatic_complexity': self.calculate_cyclomatic_complexity(),
            'cognitive_complexity': self.calculate_cognitive_complexity(),
            'maintainability_index': round(self.calculate_maintainability_index(), 2),
            'lines_of_code': len(self.lines),
            'avg_line_length': round(sum(len(l) for l in self.lines) / len(self.lines), 2) if self.lines else 0
        }


def compare_files(before_file: str, after_file: str) -> None:
    """Compare complexity metrics between two code versions."""

    with open(before_file, 'r') as f:
        before_code = f.read()

    with open(after_file, 'r') as f:
        after_code = f.read()

    before_analyzer = ComplexityAnalyzer(before_code)
    after_analyzer = ComplexityAnalyzer(after_code)

    before_metrics = before_analyzer.get_complexity_report()
    after_metrics = after_analyzer.get_complexity_report()

    print("=" * 60)
    print("CODE COMPLEXITY COMPARISON")
    print("=" * 60)

    print("\nBEFORE:")
    print(f"  Cyclomatic Complexity:    {before_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {before_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {before_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {before_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {before_metrics['avg_line_length']}")

    print("\nAFTER:")
    print(f"  Cyclomatic Complexity:    {after_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {after_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {after_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {after_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {after_metrics['avg_line_length']}")

    print("\nCHANGES:")
    cyclomatic_change = after_metrics['cyclomatic_complexity'] - before_metrics['cyclomatic_complexity']
    cognitive_change = after_metrics['cognitive_complexity'] - before_metrics['cognitive_complexity']
    mi_change = after_metrics['maintainability_index'] - before_metrics['maintainability_index']
    loc_change = after_metrics['lines_of_code'] - before_metrics['lines_of_code']

    print(f"  Cyclomatic Complexity:    {cyclomatic_change:+d}")
    print(f"  Cognitive Complexity:     {cognitive_change:+d}")
    print(f"  Maintainability Index:    {mi_change:+.2f}")
    print(f"  Lines of Code:            {loc_change:+d}")

    print("\nASSESSMENT:")
    if mi_change > 0:
        print("  ✅ Code is MORE maintainable")
    elif mi_change < 0:
        print("  ⚠️  Code is LESS maintainable")
    else:
        print("  ➡️  Maintainability unchanged")

    if cyclomatic_change < 0:
        print("  ✅ Complexity DECREASED")
    elif cyclomatic_change > 0:
        print("  ⚠️  Complexity INCREASED")
    else:
        print("  ➡️  Complexity unchanged")

    print("=" * 60)


if __name__ == '__main__':
    if len(sys.argv) != 3:
        print("Usage: python compare-complexity.py <before_file> <after_file>")
        sys.exit(1)

    compare_files(sys.argv[1], sys.argv[2])
```

## 템플릿: review-checklist.md

```markdown
# 코드 리뷰 체크리스트

## 보안 체크리스트
- [ ] 하드코딩된 자격 증명이나 비밀 정보 없음
- [ ] 모든 사용자 입력에 대한 입력 검증
- [ ] SQL 인젝션 방지 (파라미터화된 쿼리)
- [ ] 상태 변경 작업에 CSRF 보호
- [ ] 적절한 이스케이핑으로 XSS 방지
- [ ] 보호된 엔드포인트에 인증 검사
- [ ] 리소스에 대한 권한 부여 검사
- [ ] 안전한 비밀번호 해싱 (bcrypt, argon2)
- [ ] 로그에 민감 데이터 없음
- [ ] HTTPS 적용

## 성능 체크리스트
- [ ] N+1 쿼리 없음
- [ ] 인덱스 적절히 사용
- [ ] 유익한 곳에 캐싱 구현
- [ ] 메인 스레드에서 차단 작업 없음
- [ ] Async/await 올바르게 사용
- [ ] 대규모 데이터셋 페이지네이션
- [ ] 데이터베이스 연결 풀링
- [ ] 정규 표현식 최적화
- [ ] 불필요한 객체 생성 없음
- [ ] 메모리 누수 방지

## 품질 체크리스트
- [ ] 함수 < 50줄
- [ ] 명확한 변수 네이밍
- [ ] 중복 코드 없음
- [ ] 적절한 오류 처리
- [ ] 주석은 WHAT이 아닌 WHY를 설명
- [ ] 프로덕션에 console.log 없음
- [ ] 타입 검사 (TypeScript/JSDoc)
- [ ] SOLID 원칙 준수
- [ ] 디자인 패턴 올바르게 적용
- [ ] 자체 문서화 코드

## 테스트 체크리스트
- [ ] 단위 테스트 작성됨
- [ ] 엣지 케이스 포함
- [ ] 오류 시나리오 테스트됨
- [ ] 통합 테스트 존재
- [ ] 커버리지 > 80%
- [ ] 불안정한 테스트 없음
- [ ] 외부 의존성 모킹
- [ ] 명확한 테스트 이름
```

## 템플릿: finding-template.md

~~~~markdown
# 코드 리뷰 발견 사항 템플릿

코드 리뷰 중 발견된 각 문제를 문서화할 때 이 템플릿을 사용하세요.

---

## 문제: [제목]

### 심각도
- [ ] 치명적 (배포 차단)
- [ ] 높음 (병합 전 수정 필요)
- [ ] 중간 (곧 수정 필요)
- [ ] 낮음 (있으면 좋음)

### 카테고리
- [ ] 보안
- [ ] 성능
- [ ] 코드 품질
- [ ] 유지보수성
- [ ] 테스트
- [ ] 디자인 패턴
- [ ] 문서

### 위치
**파일:** `src/components/UserCard.tsx`

**라인:** 45-52

**함수/메서드:** `renderUserDetails()`

### 문제 설명

**내용:** 문제가 무엇인지 설명합니다.

**중요한 이유:** 영향과 왜 수정해야 하는지 설명합니다.

**현재 동작:** 문제가 있는 코드나 동작을 보여줍니다.

**예상 동작:** 대신 무엇이 일어나야 하는지 설명합니다.

### 코드 예제

#### 현재 (문제 있음)

```typescript
// N+1 쿼리 문제를 보여줍니다
const users = fetchUsers();
users.forEach(user => {
  const posts = fetchUserPosts(user.id); // 사용자별 쿼리!
  renderUserPosts(posts);
});
```

#### 제안된 수정

```typescript
// JOIN 쿼리로 최적화됨
const usersWithPosts = fetchUsersWithPosts();
usersWithPosts.forEach(({ user, posts }) => {
  renderUserPosts(posts);
});
```

### 영향 분석

| 측면 | 영향 | 심각도 |
|--------|--------|----------|
| 성능 | 20명 사용자에 100개 이상의 쿼리 | 높음 |
| 사용자 경험 | 느린 페이지 로드 | 높음 |
| 확장성 | 규모에서 문제 발생 | 치명적 |
| 유지보수성 | 디버깅 어려움 | 중간 |

### 관련 문제

- `AdminUserList.tsx` 라인 120의 유사한 문제
- 관련 PR: #456
- 관련 이슈: #789

### 추가 리소스

- [N+1 쿼리 문제](https://en.wikipedia.org/wiki/N%2B1_problem)
- [데이터베이스 JOIN 문서](https://docs.example.com/joins)

### 리뷰어 노트

- 이 코드베이스에서 흔한 패턴입니다
- 코드 스타일 가이드에 추가하는 것을 고려하세요
- 헬퍼 함수를 만드는 것이 좋을 수 있습니다

### 작성자 응답 (피드백용)

*코드 작성자가 작성:*

- [ ] 수정이 커밋에 구현됨: `abc123`
- [ ] 수정 상태: 완료 / 진행 중 / 논의 필요
- [ ] 질문이나 우려 사항: (설명)

---

## 발견 사항 통계 (리뷰어용)

여러 발견 사항을 검토할 때 추적:

- **총 발견된 문제:** X
- **치명적:** X
- **높음:** X
- **중간:** X
- **낮음:** X

**권장:** ✅ 승인 / ⚠️ 변경 요청 / 🔄 논의 필요

**전반적 코드 품질:** 1-5점
~~~~

## 사용 예제

~~~~markdown
사용자: 이 React 컴포넌트 코드를 검토해줘

Claude: 코드 리뷰 스킬을 사용하여 종합적인 코드 리뷰를 진행하겠습니다.

[SKILL.md 로드]
[체크리스트에 따라 코드 분석]
[Python 메트릭스 스크립트 실행]
[템플릿 적용]

## 코드 리뷰 결과

### 요약
품질 점수: 3.5/5
주요 발견 사항: 7개 항목
우선 영역: 성능 최적화, 오류 처리

### 중요 문제: 1개

**문제**: N+1 쿼리 문제
**위치**: UserComponent.tsx, 라인 45-52
**영향**: 20명의 사용자 목록에 100개 이상의 데이터베이스 쿼리 발생
**심각도**: 높음
**수정**:
```javascript
// 이전: N+1 쿼리
const users = fetchUsers();
users.forEach(user => fetchUserPosts(user.id)); // 20개 이상의 쿼리

// 이후: JOIN으로 단일 쿼리
const users = fetchUsersWithPosts(); // 1개 쿼리
```

### 성능 발견 사항
- 대규모 목록에 페이지네이션 누락
- 권장: 항목에 React.memo() 사용
- 데이터베이스 쿼리: 인덱스로 최적화 가능

### 품질 발견 사항
- 20번째 라인의 함수가 127줄 (최대: 50)
- 오류 경계 누락
- Props에 TypeScript 타입이 필요
~~~~

#### 예제 2: 브랜드 보이스 스킬

**디렉터리 구조:**

```
.claude/skills/brand-voice/
├── SKILL.md
├── brand-guidelines.md
├── tone-examples.md
└── templates/
    ├── email-template.txt
    ├── social-post-template.txt
    └── blog-post-template.md
```

**파일:** `.claude/skills/brand-voice/SKILL.md`

```yaml
---
name: 브랜드 보이스 일관성
description: 모든 커뮤니케이션이 브랜드 보이스와 톤 가이드라인과 일치하도록 보장
tags:
  - brand
  - writing
  - consistency
when_to_use: 마케팅 카피, 고객 커뮤니케이션, 또는 대외 콘텐츠를 생성할 때
---

# 브랜드 보이스 스킬

## 개요
이 스킬은 모든 커뮤니케이션이 일관된 브랜드 보이스, 톤, 메시징을 유지하도록 보장합니다.

## 브랜드 정체성

### 미션
팀이 AI로 개발 워크플로우를 자동화할 수 있도록 지원

### 가치
- **단순함**: 복잡한 것을 단순하게
- **신뢰성**: 견고한 실행
- **역량 강화**: 인간의 창의성 실현

### 톤 오브 보이스
- **친근하지만 전문적** - 캐주얼하지 않으면서 접근하기 쉬움
- **명확하고 간결** - 전문 용어 피하고 기술 개념을 간단히 설명
- **자신감** - 우리가 하는 일을 잘 알고 있음
- **공감** - 사용자 요구와 문제점 이해

## 작성 가이드라인

### 권장 사항 ✅
- 독자를 지칭할 때 "당신" 사용
- 능동태 사용: "Claude가 보고서를 생성합니다" (수동태: "보고서는 Claude에 의해 생성됩니다")
- 가치 제안으로 시작
- 구체적인 예제 사용
- 문장을 20단어 미만으로 유지
- 명확성을 위해 목록 사용
- 클릭 유도 문구 포함

### 금지 사항 ❌
- 기업 전문 용어 사용 금지
- 가르치려 들거나 지나치게 단순화 금지
- "우리는 믿습니다" 또는 "우리는 생각합니다" 사용 금지
- 강조 외에 모두 대문자 사용 금지
- 긴 텍스트 블록 생성 금지
- 기술 지식 가정 금지


## 어휘

### ✅ 선호 용어
- Claude ("Claude AI"가 아님)
- 코드 생성 ("자동 코딩"이 아님)
- 에이전트 ("봇"이 아님)
- 간소화 ("혁신"이 아님)
- 통합 ("시너지"가 아님)

### ❌ 피해야 할 용어
- "최첨단" (남용됨)
- "게임 체인저" (모호함)
- "레버리지" (기업 용어)
- "활용" ("사용"을 사용하세요)
- "패러다임 전환" (불명확)
```
## 예제

### ✅ 좋은 예제
"Claude가 코드 리뷰 프로세스를 자동화합니다. 각 PR을 수동으로 확인하는 대신, Claude가 보안, 성능, 품질을 검토하여 팀의 시간을 매주 절약해줍니다."

작동 이유: 명확한 가치, 구체적인 이점, 행동 지향적

### ❌ 나쁜 예제
"Claude는 최첨단 AI를 활용하여 포괄적인 소프트웨어 개발 솔루션을 제공합니다."

작동하지 않는 이유: 모호함, 기업 전문 용어, 구체적인 가치 없음

## 템플릿: 이메일

```
제목: [명확하고 혜택 중심의 제목]

안녕하세요 [이름]님,

[시작: 그들에게 어떤 가치가 있는지]

[본문: 작동 방식 / 받을 혜택]

[구체적인 예제 또는 혜택]

[클릭 유도 문구: 명확한 다음 단계]

감사합니다,
[이름]
```

## 템플릿: 소셜 미디어

```
[후크: 첫 줄에서 주목 끌기]
[2-3줄: 가치 또는 흥미로운 사실]
[클릭 유도 문구: 링크, 질문, 또는 참여 유도]
[이모지: 시각적 흥미를 위해 최대 1-2개]
```

## 파일: tone-examples.md
```
신나는 발표:
"코드 리뷰에 주 8시간을 절약하세요. Claude가 PR을 자동으로 검토합니다."

공감적 지원:
"배포가 스트레스가 될 수 있음을 알고 있습니다. Claude가 테스트를 처리하므로 걱정할 필요가 없습니다."

자신감 있는 제품 기능:
"Claude는 단순히 코드를 제안하는 것이 아닙니다. 아키텍처를 이해하고 일관성을 유지합니다."

교육적 블로그 포스트:
"에이전트가 코드 리뷰 워크플로우를 개선하는 방법을 살펴보겠습니다. 배운 내용은..."
```

#### 예제 3: 문서 생성기 스킬

**파일:** `.claude/skills/doc-generator/SKILL.md`

~~~~yaml
---
name: API 문서 생성기
description: 소스 코드에서 포괄적이고 정확한 API 문서 생성
version: "1.0.0"
tags:
  - documentation
  - api
  - automation
when_to_use: API 문서를 생성하거나 업데이트할 때
---

# API 문서 생성기 스킬

## 생성 항목

- OpenAPI/Swagger 명세
- API 엔드포인트 문서
- SDK 사용 예제
- 통합 가이드
- 오류 코드 참조
- 인증 가이드

## 문서 구조

### 각 엔드포인트에 대해

```markdown
## GET /api/v1/users/:id

### 설명
이 엔드포인트가 무엇을 하는지 간단히 설명

### 매개변수

| 이름 | 타입 | 필수 | 설명 |
|------|------|----------|-------------|
| id | string | 예 | 사용자 ID |

### 응답

**200 성공**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-15T10:30:00Z"
}
```

**404 찾을 수 없음**
```json
{
  "error": "USER_NOT_FOUND",
  "message": "사용자가 존재하지 않습니다"
}
```

### 예제

**cURL**
```bash
curl -X GET "https://api.example.com/api/v1/users/usr_123"   -H "Authorization: Bearer YOUR_TOKEN"
```

**JavaScript**
```javascript
const user = await fetch('/api/v1/users/usr_123', {
  headers: { 'Authorization': 'Bearer token' }
}).then(r => r.json());
```

**Python**
```python
response = requests.get(
    'https://api.example.com/api/v1/users/usr_123',
    headers={'Authorization': 'Bearer token'}
)
user = response.json()
```

## Python 스크립트: generate-docs.py

```python
#!/usr/bin/env python3
import ast
import json
from typing import Dict, List

class APIDocExtractor(ast.NodeVisitor):
    """Extract API documentation from Python source code."""

    def __init__(self):
        self.endpoints = []

    def visit_FunctionDef(self, node):
        """Extract function documentation."""
        if node.name.startswith('get_') or node.name.startswith('post_'):
            doc = ast.get_docstring(node)
            endpoint = {
                'name': node.name,
                'docstring': doc,
                'params': [arg.arg for arg in node.args.args],
                'returns': self._extract_return_type(node)
            }
            self.endpoints.append(endpoint)
        self.generic_visit(node)

    def _extract_return_type(self, node):
        """Extract return type from function annotation."""
        if node.returns:
            return ast.unparse(node.returns)
        return "Any"

def generate_markdown_docs(endpoints: List[Dict]) -> str:
    """Generate markdown documentation from endpoints."""
    docs = "# API Documentation\n\n"

    for endpoint in endpoints:
        docs += f"## {endpoint['name']}\n\n"
        docs += f"{endpoint['docstring']}\n\n"
        docs += f"**Parameters**: {', '.join(endpoint['params'])}\n\n"
        docs += f"**Returns**: {endpoint['returns']}\n\n"
        docs += "---\n\n"

    return docs

if __name__ == '__main__':
    import sys
    with open(sys.argv[1], 'r') as f:
        tree = ast.parse(f.read())

    extractor = APIDocExtractor()
    extractor.visit(tree)

    markdown = generate_markdown_docs(extractor.endpoints)
    print(markdown)
~~~~
### 스킬 발견 및 호출

```mermaid
graph TD
    A["User Request"] --> B["Claude Analyzes"]
    B -->|Scans| C["Available Skills"]
    C -->|Metadata check| D["Skill Description Match?"]
    D -->|Yes| E["Load SKILL.md"]
    D -->|No| F["Try next skill"]
    F -->|More skills?| D
    F -->|No more| G["Use general knowledge"]
    E --> H["Extract Instructions"]
    H --> I["Execute Skill"]
    I --> J["Return Results"]
```

### 스킬 vs 다른 기능

```mermaid
graph TB
    A["Extending Claude"]
    B["Slash Commands"]
    C["Subagents"]
    D["Memory"]
    E["MCP"]
    F["Skills"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F

    B -->|User-invoked| G["Quick shortcuts"]
    C -->|Auto-delegated| H["Isolated contexts"]
    D -->|Persistent| I["Cross-session context"]
    E -->|Real-time| J["External data access"]
    F -->|Auto-invoked| K["Autonomous execution"]
```

---

## Claude Code 플러그인

### 개요

Claude Code 플러그인은 단일 명령어로 설치되는 사용자 정의 모음(슬래시 명령어, 서브에이전트, MCP 서버, 훅)입니다. 이는 가장 높은 수준의 확장 메커니즘으로, 여러 기능을 응집력 있고 공유 가능한 패키지로 결합합니다.

### 아키텍처

```mermaid
graph TB
    A["Plugin"]
    B["Slash Commands"]
    C["Subagents"]
    D["MCP Servers"]
    E["Hooks"]
    F["Configuration"]

    A -->|bundles| B
    A -->|bundles| C
    A -->|bundles| D
    A -->|bundles| E
    A -->|bundles| F
```

### 플러그인 로딩 과정

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Plugin as Plugin Marketplace
    participant Install as Installation
    participant SlashCmds as Slash Commands
    participant Subagents
    participant MCPServers as MCP Servers
    participant Hooks
    participant Tools as Configured Tools

    User->>Claude: /plugin install pr-review
    Claude->>Plugin: Download plugin manifest
    Plugin-->>Claude: Return plugin definition
    Claude->>Install: Extract components
    Install->>SlashCmds: Configure
    Install->>Subagents: Configure
    Install->>MCPServers: Configure
    Install->>Hooks: Configure
    SlashCmds-->>Tools: Ready to use
    Subagents-->>Tools: Ready to use
    MCPServers-->>Tools: Ready to use
    Hooks-->>Tools: Ready to use
    Tools-->>Claude: Plugin installed ✅
```

### 플러그인 유형 및 배포

| 유형 | 범위 | 공유 | 권한 | 예제 |
|------|-------|--------|-----------|----------|
| 공식 | 전역 | 모든 사용자 | Anthropic | PR 리뷰, 보안 가이던스 |
| 커뮤니티 | 공개 | 모든 사용자 | 커뮤니티 | DevOps, 데이터 과학 |
| 조직 | 내부 | 팀 구성원 | 회사 | 내부 표준, 도구 |
| 개인 | 개인 | 단일 사용자 | 개발자 | 사용자 정의 워크플로우 |

### 플러그인 정의 구조

```yaml
---
name: plugin-name
version: "1.0.0"
description: "이 플러그인이 하는 일"
author: "이름"
license: MIT

# 플러그인 메타데이터
tags:
  - 카테고리
  - 사용-사례

# 요구사항
requires:
  - claude-code: ">=1.0.0"

# 번들 컴포넌트
components:
  - type: commands
    path: commands/
  - type: agents
    path: agents/
  - type: mcp
    path: mcp/
  - type: hooks
    path: hooks/

# 설정
config:
  auto_load: true
  enabled_by_default: true
---
```

### 플러그인 구조

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── task-1.md
│   ├── task-2.md
│   └── workflows/
├── agents/
│   ├── specialist-1.md
│   ├── specialist-2.md
│   └── configs/
├── skills/
│   ├── skill-1.md
│   └── skill-2.md
├── hooks/
│   └── hooks.json
├── .mcp.json
├── .lsp.json
├── settings.json
├── templates/
│   └── issue-template.md
├── scripts/
│   ├── helper-1.sh
│   └── helper-2.py
├── docs/
│   ├── README.md
│   └── USAGE.md
└── tests/
    └── plugin.test.js
```

### 실용 예제

#### 예제 1: PR 리뷰 플러그인

**파일:** `.claude-plugin/plugin.json`

```json
{
  "name": "pr-review",
  "version": "1.0.0",
  "description": "보안, 테스트 및 문서를 포함한 완전한 PR 리뷰 워크플로우",
  "author": {
    "name": "Anthropic"
  },
  "license": "MIT"
}
```

**파일:** `commands/review-pr.md`

```markdown
---
name: PR 리뷰
description: 보안 및 테스트 검사를 포함한 종합적인 PR 리뷰 시작
---

# PR 리뷰

이 명령어는 다음을 포함한 완전한 풀 리퀘스트 리뷰를 시작합니다:

1. 보안 분석
2. 테스트 커버리지 검증
3. 문서 업데이트
4. 코드 품질 검사
5. 성능 영향 평가
```

**파일:** `agents/security-reviewer.md`

```yaml
---
name: security-reviewer
description: 보안 중심 코드 리뷰
tools: read, grep, diff
---

---

## 비교 및 통합

### 기능 비교 매트릭스

| 기능 | 호출 | 지속성 | 범위 | 사용 사례 |
|---------|-----------|------------|-------|----------|
| **슬래시 명령어** | 수동 (`/cmd`) | 세션 전용 | 단일 명령어 | 빠른 단축키 |
| **서브에이전트** | 자동 위임 | 격리된 컨텍스트 | 전문 작업 | 작업 분배 |
| **메모리** | 자동 로드 | 세션 간 | 사용자/팀 컨텍스트 | 장기 학습 |
| **MCP 프로토콜** | 자동 쿼리 | 실시간 외부 | 실시간 데이터 액세스 | 동적 정보 |
| **스킬** | 자동 호출 | 파일 시스템 기반 | 재사용 가능한 전문성 | 자동화된 워크플로우 |

### 상호작용 타임라인

```mermaid
graph TB
    A["Plugin Marketplace"]
    B["Official<br/>Anthropic"]
    C["Community<br/>Marketplace"]
    D["Enterprise<br/>Registry"]

    A --> B
    A --> C
    A --> D

    B -->|Categories| B1["Development"]
    B -->|Categories| B2["DevOps"]
    B -->|Categories| B3["Documentation"]

    C -->|Search| C1["DevOps Automation"]
    C -->|Search| C2["Mobile Dev"]
    C -->|Search| C3["Data Science"]

    D -->|Internal| D1["Company Standards"]
    D -->|Internal| D2["Legacy Systems"]
    D -->|Internal| D3["Compliance"]
```

### 실용적인 통합 예제: 고객 지원 자동화

#### 아키텍처

```mermaid
graph LR
    A["Discover"] -->|Browse| B["Marketplace"]
    B -->|Select| C["Plugin Page"]
    C -->|View| D["Components"]
    D -->|Install| E["/plugin install"]
    E -->|Extract| F["Configure"]
    F -->|Activate| G["Use"]
    G -->|Check| H["Update"]
    H -->|Available| G
    G -->|Done| I["Disable"]
    I -->|Later| J["Enable"]
    J -->|Back| G
```

#### 요청 흐름

```markdown
## 고객 지원 요청 흐름

### 1. 수신 이메일
"파일을 업로드하려고 하면 500 오류가 발생합니다. 작업 흐름이 차단되었습니다!"

### 2. 메모리 조회
- 지원 표준으로 CLAUDE.md 로드
- 고객 기록 확인: VIP 고객, 이번 달 3번째 사건

### 3. MCP 쿼리
- GitHub MCP: 열린 이슈 나열 (관련 버그 보고서 찾음)
- Database MCP: 시스템 상태 확인 (보고된 중단 없음)
- Slack MCP: 엔지니어링 팀 인지 확인

### 4. 스킬 감지 및 로딩
- 요청이 "기술 지원" 스킬과 일치
- 스킬에서 지원 응답 템플릿 로드

### 5. 서브에이전트 위임
- 기술 지원 서브에이전트로 라우팅
- 컨텍스트 제공: 고객 기록, 오류 세부 정보, 알려진 문제
- 서브에이전트가 read, bash, grep 도구에 대한 전체 액세스 권한 보유

### 6. 서브에이전트 처리
기술 지원 서브에이전트:
- 파일 업로드에서 500 오류 검색
- 커밋 8f4a2c에서 최근 변경 발견
- 해결 방법 문서 생성

### 7. 스킬 실행
응답 생성기 스킬:
- 브랜드 보이스 가이드라인 사용
- 공감을 담은 응답 형식 지정
- 해결 방법 단계 포함
- 관련 문서 링크

### 8. MCP 출력
- #support Slack 채널에 업데이트 게시
- 엔지니어링 팀 태그
- Jira MCP에서 티켓 업데이트

### 9. 응답
고객이 받는 내용:
- 공감적 확인
- 원인 설명
- 즉시 해결 방법
- 영구 수정 일정
- 관련 이슈 링크
```

### 완전한 기능 오케스트레이션

```mermaid
graph TD
    A["Should I create a plugin?"]
    A -->|Need multiple components| B{"Multiple commands<br/>or subagents<br/>or MCPs?"}
    B -->|Yes| C["✅ Create Plugin"]
    B -->|No| D["Use Individual Feature"]
    A -->|Team workflow| E{"Share with<br/>team?"}
    E -->|Yes| C
    E -->|No| F["Keep as Local Setup"]
    A -->|Complex setup| G{"Needs auto<br/>configuration?"}
    G -->|Yes| C
    G -->|No| D
```

### 각 기능 사용 시기

```mermaid
graph LR
    A["Session Start"] -->|Load| B["Memory (CLAUDE.md)"]
    B -->|Discover| C["Available Skills"]
    C -->|Register| D["Slash Commands"]
    D -->|Connect| E["MCP Servers"]
    E -->|Ready| F["User Interaction"]

    F -->|Type /cmd| G["Slash Command"]
    F -->|Request| H["Skill Auto-Invoke"]
    F -->|Query| I["MCP Data"]
    F -->|Complex task| J["Delegate to Subagent"]

    G -->|Uses| B
    H -->|Uses| B
    I -->|Uses| B
    J -->|Uses| B
```

### 선택 결정 트리

```mermaid
graph TB
    User["Customer Email"] -->|Receives| Router["Support Router"]

    Router -->|Analyze| Memory["Memory<br/>Customer history"]
    Router -->|Lookup| MCP1["MCP: Customer DB<br/>Previous tickets"]
    Router -->|Check| MCP2["MCP: Slack<br/>Team status"]

    Router -->|Route Complex| Sub1["Subagent: Tech Support<br/>Context: Technical issues"]
    Router -->|Route Simple| Sub2["Subagent: Billing<br/>Context: Payment issues"]
    Router -->|Route Urgent| Sub3["Subagent: Escalation<br/>Context: Priority handling"]

    Sub1 -->|Format| Skill1["Skill: Response Generator<br/>Brand voice maintained"]
    Sub2 -->|Format| Skill2["Skill: Response Generator"]
    Sub3 -->|Format| Skill3["Skill: Response Generator"]

    Skill1 -->|Generate| Output["Formatted Response"]
    Skill2 -->|Generate| Output
    Skill3 -->|Generate| Output

    Output -->|Post| MCP3["MCP: Slack<br/>Notify team"]
    Output -->|Send| Reply["Customer Reply"]
```

---

## 요약 표

| 측면 | 슬래시 명령어 | 서브에이전트 | 메모리 | MCP | 스킬 | 플러그인 |
|---|---|---|---|---|---|---|
| **설정 난이도** | 쉬움 | 중간 | 쉬움 | 중간 | 중간 | 쉬움 |
| **학습 곡선** | 낮음 | 중간 | 낮음 | 중간 | 중간 | 낮음 |
| **팀 혜택** | 높음 | 높음 | 중간 | 높음 | 높음 | 매우 높음 |
| **자동화 수준** | 낮음 | 높음 | 중간 | 높음 | 높음 | 매우 높음 |
| **컨텍스트 관리** | 단일 세션 | 격리됨 | 영구 | 실시간 | 영구 | 모든 기능 |
| **유지보수 부담** | 낮음 | 중간 | 낮음 | 중간 | 중간 | 낮음 |
| **확장성** | 좋음 | 우수 | 좋음 | 우수 | 우수 | 우수 |
| **공유 용이성** | 보통 | 보통 | 좋음 | 좋음 | 좋음 | 우수 |
| **버전 관리** | 수동 | 수동 | 수동 | 수동 | 수동 | 자동 |
| **설치** | 수동 복사 | 수동 설정 | N/A | 수동 설정 | 수동 복사 | 한 번의 명령어 |

---

## 빠른 시작 가이드

### 1주차: 간단하게 시작
- 일반 작업을 위한 2-3개의 슬래시 명령어 생성
- 설정에서 메모리 활성화
- CLAUDE.md에 팀 표준 문서화

### 2주차: 실시간 액세스 추가
- 1개의 MCP 설정 (GitHub 또는 Database)
- `/mcp`로 설정
- 워크플로우에서 실시간 데이터 쿼리

### 3주차: 작업 분배
- 특정 역할을 위한 첫 번째 서브에이전트 생성
- `/agents` 명령어 사용
- 간단한 작업으로 위임 테스트

### 4주차: 모든 것 자동화
- 반복 자동화를 위한 첫 번째 스킬 생성
- 스킬 마켓플레이스 사용 또는 맞춤 제작
- 전체 워크플로우를 위해 모든 기능 결합

### 지속적
- 매달 메모리 검토 및 업데이트
- 패턴이 나타나면 새 스킬 추가
- MCP 쿼리 최적화
- 서브에이전트 프롬프트 개선

---

**마지막 업데이트**: 2026년 6월 15일
**Claude Code 버전**: 2.1.176
**출처**:
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/model-config
- https://code.claude.com/docs/en/changelog#2-1-172
- https://platform.claude.com/docs/en/about-claude/models/overview
- https://www.anthropic.com/news/claude-opus-4-8
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
**호환 모델**: Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5

---

# 보안 리뷰어

보안 취약점 찾기에 특화:
- 인증/권한 부여 문제
- 데이터 노출
- 인젝션 공격
- 안전한 설정
```

**설치:**

```bash
/plugin install pr-review

# 결과:
# ✅ 3개의 슬래시 명령어 설치됨
# ✅ 3개의 서브에이전트 설정됨
# ✅ 2개의 MCP 서버 연결됨
# ✅ 4개의 훅 등록됨
# ✅ 사용 준비 완료!
```

#### 예제 2: DevOps 플러그인

**컴포넌트:**

```
devops-automation/
├── commands/
│   ├── deploy.md
│   ├── rollback.md
│   ├── status.md
│   └── incident.md
├── agents/
│   ├── deployment-specialist.md
│   ├── incident-commander.md
│   └── alert-analyzer.md
├── mcp/
│   ├── github-config.json
│   ├── kubernetes-config.json
│   └── prometheus-config.json
├── hooks/
│   ├── pre-deploy.js
│   ├── post-deploy.js
│   └── on-error.js
└── scripts/
    ├── deploy.sh
    ├── rollback.sh
    └── health-check.sh
```



#### 예제 3: 문서 플러그인

**번들 컴포넌트:**

```
documentation/
├── commands/
│   ├── generate-api-docs.md
│   ├── generate-readme.md
│   ├── sync-docs.md
│   └── validate-docs.md
├── agents/
│   ├── api-documenter.md
│   ├── code-commentator.md
│   └── example-generator.md
├── mcp/
│   ├── github-docs-config.json
│   └── slack-announce-config.json
└── templates/
    ├── api-endpoint.md
    ├── function-docs.md
    └── adr-template.md
```

### 플러그인 마켓플레이스

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Memory as Memory<br/>CLAUDE.md
    participant MCP as MCP Servers
    participant Skills as Skills
    participant SubAgent as Subagents

    User->>Claude: Request: "Build auth system"
    Claude->>Memory: Load project standards
    Memory-->>Claude: Auth standards, team practices
    Claude->>MCP: Query GitHub for similar implementations
    MCP-->>Claude: Code examples, best practices
    Claude->>Skills: Detect matching Skills
    Skills-->>Claude: Security Review Skill + Testing Skill
    Claude->>SubAgent: Delegate implementation
    SubAgent->>SubAgent: Build feature
    Claude->>Skills: Apply Security Review Skill
    Skills-->>Claude: Security checklist results
    Claude->>SubAgent: Delegate testing
    SubAgent-->>Claude: Test results
    Claude->>User: Complete system delivered
```

### 플러그인 설치 및 생명주기

```mermaid
graph TD
    A["New Task"] --> B{Type of Task?}

    B -->|Repeated workflow| C["Slash Command"]
    B -->|Need real-time data| D["MCP Protocol"]
    B -->|Remember for next time| E["Memory"]
    B -->|Specialized subtask| F["Subagent"]
    B -->|Domain-specific work| G["Skill"]

    C --> C1["✅ Team shortcut"]
    D --> D1["✅ Live API access"]
    E --> E1["✅ Persistent context"]
    F --> F1["✅ Parallel execution"]
    G --> G1["✅ Auto-invoked expertise"]
```

### 플러그인 기능 비교

| 기능 | 슬래시 명령어 | 스킬 | 서브에이전트 | 플러그인 |
|---------|---------------|-------|----------|--------|
| **설치** | 수동 복사 | 수동 복사 | 수동 설정 | 한 번의 명령어 |
| **설정 시간** | 5분 | 10분 | 15분 | 2분 |
| **번들링** | 단일 파일 | 단일 파일 | 단일 파일 | 여러 개 |
| **버전 관리** | 수동 | 수동 | 수동 | 자동 |
| **팀 공유** | 파일 복사 | 파일 복사 | 파일 복사 | 설치 ID |
| **업데이트** | 수동 | 수동 | 수동 | 자동 가능 |
| **종속성** | 없음 | 없음 | 없음 | 포함 가능 |
| **마켓플레이스** | 아니오 | 아니오 | 아니오 | 예 |
| **배포** | 저장소 | 저장소 | 저장소 | 마켓플레이스 |

### 플러그인 사용 사례

| 사용 사례 | 권장 | 이유 |
|----------|-----------------|-----|
| **팀 온보딩** | ✅ 플러그인 사용 | 즉시 설정, 모든 설정 포함 |
| **프레임워크 설정** | ✅ 플러그인 사용 | 프레임워크별 명령어 번들 |
| **엔터프라이즈 표준** | ✅ 플러그인 사용 | 중앙 배포, 버전 관리 |
| **빠른 작업 자동화** | ❌ 명령어 사용 | 과도한 복잡성 |
| **단일 도메인 전문성** | ❌ 스킬 사용 | 너무 무거움, 스킬 사용 |
| **전문 분석** | ❌ 서브에이전트 사용 | 수동 생성 또는 스킬 사용 |
| **실시간 데이터 액세스** | ❌ MCP 사용 | 독립형, 번들 불필요 |

### 플러그인 생성 시기

```mermaid
graph TD
    Start["Need to extend Claude?"]

    Start -->|Quick repeated task| A{"Manual or Auto?"}
    A -->|Manual| B["Slash Command"]
    A -->|Auto| C["Skill"]

    Start -->|Need external data| D{"Real-time?"}
    D -->|Yes| E["MCP Protocol"]
    D -->|No/Cross-session| F["Memory"]

    Start -->|Complex project| G{"Multiple roles?"}
    G -->|Yes| H["Subagents"]
    G -->|No| I["Skills + Memory"]

    Start -->|Long-term context| J["Memory"]
    Start -->|Team workflow| K["Slash Command +<br/>Memory"]
    Start -->|Full automation| L["Skills +<br/>Subagents +<br/>MCP"]
```

### 플러그인 게시

**게시 단계:**

1. 모든 컴포넌트로 플러그인 구조 생성
2. `.claude-plugin/plugin.json` 매니페스트 작성
3. 문서와 함께 `README.md` 생성
4. `/plugin install ./my-plugin`으로 로컬 테스트
5. 플러그인 마켓플레이스에 제출
6. 검토 및 승인
7. 마켓플레이스에 게시
8. 사용자가 한 번의 명령어로 설치 가능

**제출 예시:**

~~~~markdown
# PR 리뷰 플러그인

## 설명
보안, 테스트 및 문서 검사가 포함된 완전한 PR 리뷰 워크플로우.

## 포함 항목
- 다양한 리뷰 유형을 위한 3개의 슬래시 명령어
- 3개의 전문 서브에이전트
- GitHub 및 CodeQL MCP 통합
- 자동화된 보안 검사 훅

## 설치
```bash
/plugin install pr-review
```

## 기능
✅ 보안 분석
✅ 테스트 커버리지 검사
✅ 문서 확인
✅ 코드 품질 평가
✅ 성능 영향 분석

## 사용법
```bash
/review-pr
/check-security
/check-tests
```

## 요구사항
- Claude Code 1.0+
- GitHub 액세스
- CodeQL (선택 사항)
~~~~

### 플러그인 vs 수동 설정

**수동 설정 (2시간 이상):**
- 슬래시 명령어를 하나씩 설치
- 서브에이전트 개별 생성
- MCP를 별도로 설정
- 훅 수동 설정
- 모든 것 문서화
- 팀과 공유 (올바르게 설정되기를 기대)

**플러그인 사용 (2분):**
```bash
/plugin install pr-review
# ✅ 모든 것이 설치되고 설정됨
# ✅ 즉시 사용 가능
# ✅ 팀이 정확히 동일한 설정 복제 가능
```

---

## 훅

### 개요

훅은 Claude Code 이벤트에 응답하여 자동으로 실행되는 이벤트 기반 셸 명령어입니다. 수동 개입 없이 자동화, 검증 및 사용자 정의 워크플로우를 가능하게 합니다.

### 훅 이벤트

Claude Code는 5가지 훅 유형(명령어, http, mcp_tool, 프롬프트, 에이전트)에 걸쳐 **29개의 훅 이벤트**를 지원합니다:

| 훅 이벤트 | 트리거 | 사용 사례 |
|------------|---------|-----------|
| **SessionStart** | 세션 시작/재개/초기화/압축 | 환경 설정, 초기화 |
| **Setup** | 초기 환경 설정 (세션당 한 번) | 도구 프로비저닝, deps 설치 |
| **InstructionsLoaded** | CLAUDE.md 또는 rules 파일 로드됨 | 검증, 변환, 증강 |
| **UserPromptSubmit** | 사용자가 프롬프트 제출 | 입력 검증, 프롬프트 필터링 |
| **UserPromptExpansion** | 사용자 프롬프트 확장 (@-멘션, 슬래시 명령어 해결됨) | 확장된 프롬프트 변환 또는 검사 |
| **PreToolUse** | 도구 실행 전 | 검증, 승인 게이트, 로깅 |
| **PermissionRequest** | 권한 대화상자 표시됨 | 자동 승인/거부 흐름 |
| **PermissionDenied** | 사용자가 권한 프롬프트 거부 | 로깅, 분석, 정책 적용 |
| **PostToolUse** | 도구 성공 후 | 자동 포맷팅, 알림, 정리 |
| **PostToolUseFailure** | 도구 실행 실패 | 오류 처리, 로깅 |
| **PostToolBatch** | 도구 사용 배치 완료 후 | 집계 보고, 배치 검증 |
| **Notification** | 알림 전송됨 | 경고, 외부 통합 |
| **SubagentStart** | 서브에이전트 생성됨 | 컨텍스트 주입, 초기화 |
| **SubagentStop** | 서브에이전트 완료 | 결과 검증, 로깅 |
| **Stop** | Claude 응답 완료 | 요약 생성, 정리 작업 |
| **StopFailure** | API 오류로 턴 종료 | 오류 복구, 로깅 |
| **TeammateIdle** | 에이전트 팀 동료 유휴 상태 | 작업 분배, 조정 |
| **TaskCompleted** | 작업 완료 표시됨 | 작업 후 처리 |
| **TaskCreated** | TaskCreate를 통해 작업 생성됨 | 작업 추적, 로깅 |
| **ConfigChange** | 설정 파일 변경됨 | 검증, 전파 |
| **CwdChanged** | 작업 디렉터리 변경됨 | 디렉터리별 설정 |
| **FileChanged** | 감시 중인 파일 변경됨 | 파일 모니터링, 재빌드 트리거 |
| **PreCompact** | 컨텍스트 압축 전 | 상태 보존 |
| **PostCompact** | 압축 완료 후 | 압축 후 작업 |
| **WorktreeCreate** | 워크트리 생성 중 | 환경 설정, 종속성 설치 |
| **WorktreeRemove** | 워크트리 제거 중 | 정리, 리소스 할당 해제 |
| **Elicitation** | MCP 서버가 사용자 입력 요청 | 입력 검증 |
| **ElicitationResult** | 사용자가 elicitation에 응답 | 응답 처리 |
| **SessionEnd** | 세션 종료 | 정리, 최종 로깅 |

### 일반 훅

훅은 `~/.claude/settings.json` (사용자 수준) 또는 `.claude/settings.json` (프로젝트 수준)에서 설정합니다:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "eslint $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

### 훅 환경 변수

- `$CLAUDE_FILE_PATH` - 편집/쓰기 중인 파일 경로
- `$CLAUDE_TOOL_NAME` - 사용 중인 도구 이름
- `$CLAUDE_SESSION_ID` - 현재 세션 식별자
- `$CLAUDE_PROJECT_DIR` - 프로젝트 디렉터리 경로

### 모범 사례

✅ **권장:**
- 훅을 빠르게 유지 (< 1초)
- 검증 및 자동화에 훅 사용
- 오류를 우아하게 처리
- 절대 경로 사용

❌ **금지:**
- 훅을 대화형으로 만들지 않음
- 장기 실행 작업에 훅 사용하지 않음
- 자격 증명을 하드코딩하지 않음

**참조**: [06-hooks/](06-hooks/) 자세한 예제는 여기를 참조하세요

---

## 체크포인트 및 되감기

### 개요

체크포인트를 사용하면 대화 상태를 저장하고 이전 지점으로 되감을 수 있어 안전한 실험과 여러 접근 방식 탐색이 가능합니다.

### 주요 개념

| 개념 | 설명 |
|---------|-------------|
| **체크포인트** | 메시지, 파일, 컨텍스트를 포함한 대화 상태의 스냅샷 |
| **되감기** | 이전 체크포인트로 돌아가 후속 변경 사항을 폐기 |
| **분기점** | 여러 접근 방식을 탐색하는 체크포인트 |

### 체크포인트 액세스

체크포인트는 모든 사용자 프롬프트와 함께 자동으로 생성됩니다. 되감기를 하려면:

```bash
# Esc를 두 번 눌러 체크포인트 브라우저 열기
Esc + Esc

# 또는 /rewind 명령어 사용
/rewind
```

체크포인트를 선택하면 5가지 옵션 중에서 선택합니다:
1. **코드 및 대화 복원** -- 둘 다 해당 지점으로 되돌리기
2. **대화 복원** -- 메시지 되감기, 현재 코드 유지
3. **코드 복원** -- 파일 되돌리기, 대화 유지
4. **여기서부터 요약** -- 대화를 요약으로 압축
5. **취소** -- 취소

### 사용 사례

| 시나리오 | 워크플로우 |
|----------|----------|
| **접근 방식 탐색** | 저장 → A 시도 → 저장 → 되감기 → B 시도 → 비교 |
| **안전한 리팩토링** | 저장 → 리팩토링 → 테스트 → 실패 시: 되감기 |
| **A/B 테스트** | 저장 → 디자인 A → 저장 → 되감기 → 디자인 B → 비교 |
| **실수 복구** | 문제 발견 → 마지막 정상 상태로 되감기 |

### 설정

```json
{
  "autoCheckpoint": true
}
```

**참조**: [08-checkpoints/](08-checkpoints/) 자세한 예제는 여기를 참조하세요

---

## 고급 기능

### 계획 모드

코딩 전에 상세한 구현 계획을 만듭니다.

**활성화:**
```bash
/plan 사용자 인증 시스템 구현
```

**혜택:**
- 시간 추정이 포함된 명확한 로드맵
- 위험 평가
- 체계적인 작업 분할
- 검토 및 수정 기회

### 확장 사고

복잡한 문제를 위한 심층 추론.

**활성화:**
- 세션 중 `Alt+T`(macOS에서는 `Option+T`)로 전환
- 프로그래밍 방식 제어를 위해 `MAX_THINKING_TOKENS` 환경 변수 설정

```bash
# 환경 변수를 통한 확장 사고 활성화
export MAX_THINKING_TOKENS=50000
claude -p "마이크로서비스와 모놀리스 중 어떤 것을 사용해야 할까요?"
```

**혜택:**
- 트레이드오프에 대한 철저한 분석
- 더 나은 아키텍처 결정
- 엣지 케이스 고려
- 체계적인 평가

### 백그라운드 작업

대화를 차단하지 않고 장기 작업을 실행합니다.

**사용법:**
```bash
사용자: 백그라운드에서 테스트 실행

Claude: bg-1234 작업 시작됨

/task list           # 모든 작업 표시
/task status bg-1234 # 진행 상황 확인
/task show bg-1234   # 출력 보기
/task cancel bg-1234 # 작업 취소
```

### 권한 모드

Claude가 할 수 있는 작업을 제어합니다.

| 모드 | 설명 | 사용 사례 |
|------|-------------|----------|
| **default** | 민감한 작업에 프롬프트가 있는 표준 권한 | 일반 개발 |
| **acceptEdits** | 확인 없이 파일 편집 자동 수락 | 신뢰할 수 있는 편집 워크플로우 |
| **plan** | 분석 및 계획만, 파일 수정 없음 | 코드 리뷰, 아키텍처 계획 |
| **auto** | 안전한 작업 자동 승인, 위험한 작업만 프롬프트 | 안전성과 균형 잡힌 자율성 |
| **dontAsk** | 확인 프롬프트 없이 모든 작업 실행 | 경험자, 자동화 |
| **bypassPermissions** | 전체 무제한 액세스, 안전 검사 없음 | CI/CD 파이프라인, 신뢰할 수 있는 스크립트 |

**사용법:**
```bash
claude --permission-mode plan          # 읽기 전용 분석
claude --permission-mode acceptEdits   # 편집 자동 수락
claude --permission-mode auto          # 안전한 작업 자동 승인
claude --permission-mode dontAsk       # 확인 프롬프트 없음
```

### 헤드리스 모드 (인쇄 모드)

`-p`(인쇄) 플래그를 사용하여 대화형 입력 없이 Claude Code를 실행합니다.

**사용법:**
```bash
# 특정 작업 실행
claude -p "모든 테스트 실행"

# 분석을 위해 입력 파이프
cat error.log | claude -p "이 오류 설명"

# CI/CD 통합 (GitHub Actions)
- name: AI 코드 리뷰
  run: claude -p "PR 변경사항 검토 및 문제 보고"

# 스크립팅을 위한 JSON 출력
claude -p --output-format json "src/의 모든 함수 나열"
```

### 예약된 작업

`/loop` 명령어를 사용하여 반복 일정으로 작업을 실행합니다.

**사용법:**
```bash
/loop every 30m "테스트 실행 및 실패 보고"
/loop every 2h "종속성 업데이트 확인"
/loop every 1d "코드 변경사항 일일 요약 생성"
```

예약된 작업은 백그라운드에서 실행되며 완료 시 결과를 보고합니다. 지속적인 모니터링, 정기적인 검사 및 자동화된 유지보수 워크플로우에 유용합니다.

### Chrome 통합

Claude Code는 웹 자동화 작업을 위해 Chrome 브라우저와 통합할 수 있습니다. 이를 통해 웹 페이지 탐색, 양식 작성, 스크린샷 촬영, 웹사이트에서 데이터 추출 등을 개발 워크플로우 내에서 직접 수행할 수 있습니다.

### 세션 관리

여러 작업 세션을 관리합니다.

**명령어:**
```bash
/resume                # 이전 대화 재개
/rename "기능"      # 현재 세션 이름 지정
/fork                  # 새 세션으로 포크
claude -c              # 가장 최근 대화 계속
claude -r "기능"    # 이름/ID로 세션 재개
```

### 대화형 기능

**키보드 단축키:**
- `Ctrl + R` - 명령어 기록 검색
- `Tab` - 자동 완성
- `↑ / ↓` - 명령어 기록
- `Ctrl + L` - 화면 지우기

**여러 줄 입력:**
```bash
사용자: > 긴 복합 프롬프트
> 여러 줄에 걸친
> \end
```

### 설정

전체 설정 예시:

```json
{
  "planning": {
    "autoEnter": true,
    "requireApproval": true
  },
  "extendedThinking": {
    "enabled": true,
    "showThinkingProcess": true
  },
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5
  },
  "permissions": {
    "mode": "default"
  }
}
```

**참조**: [09-advanced-features/](09-advanced-features/) 포괄적인 가이드는 여기를 참조하세요

---

## 모델 및 추론 노력

Claude Code는 적응형 추론 노력과 함께 다음 모델을 지원합니다:

| 모델 | 컨텍스트 창 | 노력 수준 | 기본 노력 (Claude Code) |
|-------|----------------|---------------|------------------------------|
| Claude Opus 4.8 | 1M 토큰 (네이티브) | `low`, `medium`, `high`, `xhigh`, `max` | `high` (v2.1.154부터) |
| Claude Opus 4.7 (레거시) | 1M 토큰 (네이티브) | `low`, `medium`, `high`, `xhigh`, `max` | `xhigh` (Opus 4.7 출시, 2026-04-16부터) |
| Claude Sonnet 4.6 | 1M 토큰 | `low`, `medium`, `high`, `max` | Pro/Max 가입자 `high` (v2.1.117에서 `medium`에서 상향) |
| Claude Haiku 4.5 | 200K 토큰 | — (노력 지원 없음) | — |

> **참고**: `xhigh`는 Opus 4.8 및 Opus 4.7에서 사용 가능; `max`는 Opus 4.8/4.7/4.6 및 Sonnet 4.6(세션 전용)에서 작동. Haiku 4.5는 노력 수준을 지원하지 않습니다.

> **참고**: v2.1.117에서 Opus 4.7 세션이 `/context`를 네이티브 1M 대신 200K로 계산하던 버그를 수정했습니다. 실제 1M 컨텍스트를 사용하려면 v2.1.117 이상으로 업그레이드하세요. Opus 4.8도 네이티브 1M 토큰 창을 가지고 있습니다.

> **참고**: `/cost`와 `/stats`는 v2.1.118에서 `/usage`로 통합되었습니다. `/usage`는 이제 비용/통계 등을 위한 탭이 있는 표준 명령어입니다. `/cost`와 `/stats`는 해당 탭을 여는 단축 별칭으로 유지됩니다. v2.1.149부터 비용 보기는 카테고리(스킬, 서브에이전트, 플러그인, MCP 서버별 비용)별로 지출을 세분화하여 표시합니다.

## 리소스

- [Claude Code 문서](https://code.claude.com/docs/en/overview)
- [Claude Code 변경 로그](https://code.claude.com/docs/en/changelog)
- [MCP GitHub 서버](https://github.com/modelcontextprotocol/servers)
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

---

**마지막 업데이트**: 2026년 6월 15일
**Claude Code 버전**: 2.1.176
**출처**:
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/model-config
- https://code.claude.com/docs/en/changelog#2-1-172
- https://platform.claude.com/docs/en/about-claude/models/overview
- https://www.anthropic.com/news/claude-opus-4-8
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
**호환 모델**: Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
