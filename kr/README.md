<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>
<!-- i18n-source: README.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

<p align="center">
  <a href="https://github.com/trending">
    <img src="https://img.shields.io/badge/GitHub-🔥%20%231%20Trending-purple?style=for-the-badge&logo=github"/>
  </a>
</p>

[![GitHub Stars](https://img.shields.io/github/stars/luongnv89/claude-howto?style=flat&color=gold)](https://github.com/luongnv89/claude-howto/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/luongnv89/claude-howto?style=flat)](https://github.com/luongnv89/claude-howto/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-2.1.160-brightgreen)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude_Code-2.1+-purple)](https://code.claude.com)

🌐 **Language / Ngôn ngữ / 语言 / Мова:** [English](../README.md) | [Tiếng Việt](../vi/README.md) | [中文](../zh/README.md) | [Українська](../uk/README.md) | [日本語](../ja/README.md) | [한국어](README.md)

# 주말에 Claude Code 마스터하기

`claude` 입력부터 에이전트, 훅, 스킬, MCP 서버 조율까지 — 시각적 튜토리얼, 복사-붙여넣기 템플릿, 안내된 학습 경로와 함께 제공합니다.

**[15분 안에 시작하기](#15분-안에-시작하기)** | **[내 수준 찾기](#어디부터-시작해야-할지-모르시겠나요)** | **[기능 카탈로그 살펴보기](CATALOG.md)**

---

## 목차

- [문제점](#문제점)
- [Claude How To의 해결 방법](#claude-how-to의-해결-방법)
- [작동 방식](#작동-방식)
- [어디부터 시작해야 할지 모르시겠나요?](#어디부터-시작해야-할지-모르시겠나요)
- [15분 안에 시작하기](#15분-안에-시작하기)
- [이걸로 무엇을 만들 수 있나요?](#이걸로-무엇을-만들-수-있나요)
- [FAQ](#faq)
- [기여하기](#기여하기)
- [라이선스](#라이선스)

---

## 문제점

Claude Code를 설치했습니다. 몇 가지 프롬프트를 실행해봤습니다. 이제 뭘 해야 할까요?

- **공식 문서는 기능을 설명하지만, 기능을 결합하는 방법은 보여주지 않습니다.** 슬래시 명령어가 있다는 건 알지만, 이를 훅, 메모리, 서브에이전트와 연결하여 실제로 시간을 절약하는 워크플로우로 만드는 방법을 모릅니다.
- **명확한 학습 경로가 없습니다.** MCP를 훅보다 먼저 배워야 할까요? 스킬을 서브에이전트보다 먼저 배워야 할까요? 결국 모든 것을 대충 훑어보고 아무것도 제대로 익히지 못하게 됩니다.
- **예제가 너무 기초적입니다.** "hello world" 슬래시 명령어로는 메모리를 사용하고, 전문화된 에이전트에 위임하며, 보안 검사를 자동으로 실행하는 프로덕션 코드 리뷰 파이프라인을 구축하는 데 도움이 되지 않습니다.

여러분은 Claude Code 성능의 90%를 사용하지 않고 있으며 — 그 사실조차 모르고 있습니다.

---

## Claude How To의 해결 방법

이것은 또 다른 기능 레퍼런스가 아닙니다. **구조화된, 시각적인, 예제 중심의 가이드**로, 오늘 바로 프로젝트에 복사하여 사용할 수 있는 실제 템플릿과 함께 모든 Claude Code 기능을 가르쳐줍니다.

|  | 공식 문서 | 이 가이드 |
|--|----------|-----------|
| **형식** | 레퍼런스 문서 | Mermaid 다이어그램을 포함한 시각적 튜토리얼 |
| **깊이** | 기능 설명 | 내부 동작 방식 |
| **예제** | 기본 스니펫 | 바로 사용 가능한 프로덕션 템플릿 |
| **구조** | 기능별 구성 | 점진적 학습 경로 (초급부터 고급까지) |
| **온보딩** | 자기 주도 | 시간 예상치가 포함된 안내 로드맵 |
| **자기 평가** | 없음 | 빈틈을 찾고 맞춤 경로를 구성하는 대화형 퀴즈 |

### 제공 사항:

- **10개 튜토리얼 모듈** — 슬래시 명령어부터 커스텀 에이전트 팀까지 모든 Claude Code 기능을 포괄
- **복사-붙여넣기 설정** — 슬래시 명령어, CLAUDE.md 템플릿, 훅 스크립트, MCP 설정, 서브에이전트 정의, 전체 플러그인 번들
- **Mermaid 다이어그램** — 각 기능의 내부 동작 방식을 보여주어 *어떻게*뿐만 아니라 *왜* 작동하는지 이해
- **초급자에서 고급 사용자로 안내하는 학습 경로** (11-13시간 소요)
- **내장 자기 평가** — Claude Code에서 `/self-assessment` 또는 `/lesson-quiz hooks`를 실행하여 빈틈을 파악

**[학습 경로 시작하기 ->](LEARNING-ROADMAP.md)**

---

## 작동 방식

### 1. 자신의 수준을 찾으세요

[자기 평가 퀴즈](LEARNING-ROADMAP.md#내-수준-찾기)를 보거나 Claude Code에서 `/self-assessment`를 실행하세요. 이미 알고 있는 내용을 기반으로 맞춤 로드맵을 제공합니다.

### 2. 안내된 경로를 따라가세요

10개 모듈을 순서대로 학습하세요 — 각 모듈은 이전 모듈을 기반으로 합니다. 학습하면서 템플릿을 프로젝트에 바로 복사하세요.

### 3. 기능을 워크플로우로 결합하세요

진정한 힘은 기능을 결합하는 데 있습니다. 슬래시 명령어 + 메모리 + 서브에이전트 + 훅을 연결하여 코드 리뷰, 배포, 문서 생성을 처리하는 자동화 파이프라인을 만드는 방법을 배웁니다.

### 4. 이해도를 테스트하세요

각 모듈 후에 `/lesson-quiz [주제]`를 실행하세요. 퀴즈는 놓친 부분을 정확히 짚어주므로 빠르게 보완할 수 있습니다.

**[15분 안에 시작하기](#15분-안에-시작하기)**

---

## 개발자들에게 신뢰받는 이유

- **GitHub 별** — Claude Code를 매일 사용하는 개발자들로부터
- **포크** — 이 가이드를 자체 워크플로우에 맞게 적용하는 팀들로부터
- **적극적으로 유지 관리** — 모든 Claude Code 릴리스와 동기화 (최신: v2.1.160, 2026년 6월)
- **커뮤니티 주도** — 실제 설정을 공유하는 개발자들의 기여

[![Star History Chart](https://api.star-history.com/svg?repos=luongnv89/claude-howto&type=Date)](https://star-history.com/#luongnv89/claude-howto&Date)

---

## 어디부터 시작해야 할지 모르시겠나요?

자기 평가를 하거나 자신의 수준을 선택하세요:

| 수준 | 할 수 있는 일 | 여기서 시작 | 시간 |
|------|-------------|-----------|------|
| **초급** | Claude Code를 시작하고 채팅하기 | [슬래시 명령어](01-slash-commands/) | ~2.5시간 |
| **중급** | CLAUDE.md와 커스텀 명령어 사용하기 | [스킬](03-skills/) | ~3.5시간 |
| **고급** | MCP 서버와 훅 설정하기 | [고급 기능](09-advanced-features/) | ~5시간 |

**10개 모듈 전체 학습 경로:**

| 순서 | 모듈 | 수준 | 시간 |
|------|------|------|------|
| 1 | [슬래시 명령어](01-slash-commands/) | 초급 | 30분 |
| 2 | [메모리](02-memory/) | 초급+ | 45분 |
| 3 | [체크포인트](08-checkpoints/) | 중급 | 45분 |
| 4 | [CLI 기초](10-cli/) | 초급+ | 30분 |
| 5 | [스킬](03-skills/) | 중급 | 1시간 |
| 6 | [훅](06-hooks/) | 중급 | 1시간 |
| 7 | [MCP](05-mcp/) | 중급+ | 1시간 |
| 8 | [서브에이전트](04-subagents/) | 중급+ | 1.5시간 |
| 9 | [고급 기능](09-advanced-features/) | 고급 | 2-3시간 |
| 10 | [플러그인](07-plugins/) | 고급 | 2시간 |

**[전체 학습 로드맵 ->](LEARNING-ROADMAP.md)**

---

## 15분 안에 시작하기

> **설치 참고사항**: v2.1.113부터 Claude Code는 네이티브 플랫폼별 바이너리(macOS/Linux/Windows)로 제공됩니다. `npm install -g @anthropic-ai/claude-code`도 여전히 작동하며, 첫 사용 시 네이티브 바이너리가 선택적 종속성으로 다운로드됩니다. v2.1.116부터 다운로드는 `https://downloads.claude.ai/claude-code-releases`에서 제공되며, 회사 프록시는 이 호스트를 허용 목록에 추가해야 합니다.

```bash
# 1. 가이드 클론
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto

# 2. 첫 번째 슬래시 명령어 복사
mkdir -p /path/to/your-project/.claude/commands
cp 01-slash-commands/optimize.md /path/to/your-project/.claude/commands/

# 3. 사용해보기 — Claude Code에서 다음 입력:
# /optimize

# 4. 더 많은 준비가 되었다면? 프로젝트 메모리 설정:
cp 02-memory/project-CLAUDE.md /path/to/your-project/CLAUDE.md

# 5. 스킬 설치:
cp -r 03-skills/code-review-specialist ~/.claude/skills/
```

전체 설정을 원하시나요? **1시간 필수 설정**입니다:

```bash
# 슬래시 명령어 (15분)
cp 01-slash-commands/*.md .claude/commands/

# 프로젝트 메모리 (15분)
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 스킬 설치 (15분)
cp -r 03-skills/code-review-specialist ~/.claude/skills/

# 주말 목표: 훅, 서브에이전트, MCP, 플러그인 추가
# 학습 경로에 따라 안내된 설정 진행
```

**[전체 설치 레퍼런스 보기](#15분-안에-시작하기)**

---

## 이걸로 무엇을 만들 수 있나요?

| 사용 사례 | 결합할 기능 |
|-----------|-----------|
| **자동화된 코드 리뷰** | 슬래시 명령어 + 서브에이전트 + 메모리 + MCP |
| **팀 온보딩** | 메모리 + 슬래시 명령어 + 플러그인 |
| **CI/CD 자동화** | CLI 레퍼런스 + 훅 + 백그라운드 태스크 |
| **문서 생성** | 스킬 + 서브에이전트 + 플러그인 |
| **보안 감사** | 서브에이전트 + 스킬 + 훅 (읽기 전용 모드) |
| **DevOps 파이프라인** | 플러그인 + MCP + 훅 + 백그라운드 태스크 |
| **복잡한 리팩토링** | 체크포인트 + 계획 모드 + 훅 |

---

## FAQ

**무료인가요?**
네. MIT 라이선스로 영구 무료입니다. 개인 프로젝트, 직장, 팀에서 자유롭게 사용 가능하며, 라이선스 표시 외에는 제한이 없습니다.

**유지 관리되나요?**
적극적으로 유지 관리됩니다. 모든 Claude Code 릴리스와 동기화됩니다. 현재 버전: v2.1.160 (2026년 6월), Claude Code 2.1+와 호환됩니다.

**공식 문서와 어떻게 다른가요?**
공식 문서는 기능 레퍼런스입니다. 이 가이드는 다이어그램, 프로덕션 준비 템플릿, 점진적 학습 경로를 갖춘 튜토리얼입니다. 서로 보완적입니다 — 여기서 배우고, 공식 문서에서 세부 사항을 확인하세요.

**모든 내용을 익히는 데 얼마나 걸리나요?**
전체 경로 기준 11-13시간입니다. 하지만 15분 안에 즉각적인 가치를 얻을 수 있습니다 — 슬래시 명령어 템플릿을 복사하여 바로 사용해보세요.

**Claude Sonnet / Haiku / Opus와 함께 사용할 수 있나요?**
네. 모든 템플릿은 Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5와 호환됩니다.

**기여할 수 있나요?**
물론입니다. [CONTRIBUTING.md](CONTRIBUTING.md)에서 가이드라인을 확인하세요. 새로운 예제, 버그 수정, 문서 개선, 커뮤니티 템플릿을 환영합니다.

**오프라인에서도 읽을 수 있나요?**
네. `uv run scripts/build_epub.py`를 실행하여 모든 콘텐츠와 렌더링된 다이어그램이 포함된 EPUB 전자책을 생성하세요.

---

## 지금 Claude Code 마스터링 시작하기

Claude Code는 이미 설치되어 있습니다. 10배 생산성 사이에 있는 유일한 장애물은 사용법을 아는 것입니다. 이 가이드는 구조화된 경로, 시각적 설명, 그리고 목표에 도달하기 위한 복사-붙여넣기 템플릿을 제공합니다.

MIT 라이선스. 영구 무료. 클론하고, 포크하고, 여러분의 것으로 만드세요.

**[학습 경로 시작하기 ->](LEARNING-ROADMAP.md)** | **[기능 카탈로그 살펴보기](CATALOG.md)** | **[15분 안에 시작하기](#15분-안에-시작하기)**

---

<details>
<summary>빠른 탐색 — 모든 기능</summary>

| 기능 | 설명 | 폴더 |
|------|------|------|
| **기능 카탈로그** | 설치 명령어가 포함된 전체 레퍼런스 | [CATALOG.md](CATALOG.md) |
| **슬래시 명령어** | 사용자가 호출하는 단축키 | [01-slash-commands/](01-slash-commands/) |
| **메모리** | 지속적인 컨텍스트 | [02-memory/](02-memory/) |
| **스킬** | 재사용 가능한 기능 | [03-skills/](03-skills/) |
| **서브에이전트** | 전문화된 AI 어시스턴트 | [04-subagents/](04-subagents/) |
| **MCP 프로토콜** | 외부 도구 접근 | [05-mcp/](05-mcp/) |
| **훅** | 이벤트 기반 자동화 | [06-hooks/](06-hooks/) |
| **플러그인** | 번들 기능 | [07-plugins/](07-plugins/) |
| **체크포인트** | 세션 스냅샷 및 되감기 | [08-checkpoints/](08-checkpoints/) |
| **고급 기능** | 계획, 사고, 백그라운드 태스크 | [09-advanced-features/](09-advanced-features/) |
| **CLI 레퍼런스** | 명령어, 플래그, 옵션 | [10-cli/](10-cli/) |
| **블로그 포스트** | 실제 사용 예제 | [블로그 포스트](https://medium.com/@luongnv89) |

</details>

<details>
<summary>기능 비교</summary>

| 기능 | 호출 방식 | 지속성 | 최적 용도 |
|------|----------|--------|---------|
| **슬래시 명령어** | 수동 (`/cmd`) | 세션만 | 빠른 단축키 |
| **메모리** | 자동 로드 | 세션 간 | 장기 학습 |
| **스킬** | 자동 호출 | 파일시스템 | 자동화된 워크플로우 |
| **서브에이전트** | 자동 위임 | 격리된 컨텍스트 | 작업 분배 |
| **MCP 프로토콜** | 자동 쿼리 | 실시간 | 실시간 데이터 접근 |
| **훅** | 이벤트 트리거 | 설정 기반 | 자동화 및 검증 |
| **플러그인** | 한 번의 명령어 | 모든 기능 | 완전한 솔루션 |
| **체크포인트** | 수동/자동 | 세션 기반 | 안전한 실험 |
| **계획 모드** | 수동/자동 | 계획 단계 | 복잡한 구현 |
| **백그라운드 태스크** | 수동 | 작업 기간 | 장기 실행 작업 |
| **CLI 레퍼런스** | 터미널 명령어 | 세션/스크립트 | 자동화 및 스크립팅 |

</details>

<details>
<summary>설치 빠른 참조</summary>

```bash
# 슬래시 명령어
cp 01-slash-commands/*.md .claude/commands/

# 메모리
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 스킬
cp -r 03-skills/code-review-specialist ~/.claude/skills/

# 서브에이전트
cp 04-subagents/*.md .claude/agents/

# MCP
export GITHUB_TOKEN="token"
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 훅
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 플러그인
/plugin install pr-review

# 체크포인트 (자동 활성화, 설정에서 구성)
# 08-checkpoints/README.md 참조

# 고급 기능 (설정에서 구성)
# 09-advanced-features/config-examples.json 참조

# CLI 레퍼런스 (설치 불필요)
# 10-cli/README.md에서 사용 예제 확인
```

</details>

<details>
<summary>01. 슬래시 명령어</summary>

**위치**: [01-slash-commands/](01-slash-commands/)

**정의**: Markdown 파일로 저장된 사용자 호출 단축키

**예제**:
- `optimize.md` — 코드 최적화 분석
- `pr.md` — 풀 리퀘스트 준비
- `generate-api-docs.md` — API 문서 생성기

**설치**:
```bash
cp 01-slash-commands/*.md /path/to/project/.claude/commands/
```

**사용법**:
```
/optimize
/pr
/generate-api-docs
```

**더 알아보기**: [Claude Code 슬래시 명령어 알아보기](https://medium.com/@luongnv89/discovering-claude-code-slash-commands-cdc17f0dfb29)

</details>

<details>
<summary>02. 메모리</summary>

**위치**: [02-memory/](02-memory/)

**정의**: 세션 간 지속되는 컨텍스트

**예제**:
- `project-CLAUDE.md` — 팀 전체 프로젝트 표준
- `directory-api-CLAUDE.md` — 디렉토리별 규칙
- `personal-CLAUDE.md` — 개인 선호도

**설치**:
```bash
# 프로젝트 메모리
cp 02-memory/project-CLAUDE.md /path/to/project/CLAUDE.md

# 디렉토리 메모리
cp 02-memory/directory-api-CLAUDE.md /path/to/project/src/api/CLAUDE.md

# 개인 메모리
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

**사용법**: Claude가 자동으로 로드

</details>

<details>
<summary>03. 스킬</summary>

**위치**: [03-skills/](03-skills/)

**정의**: 지침과 스크립트를 갖춘 재사용 가능한 자동 호출 기능

**예제**:
- `code-review-specialist/` — 스크립트를 포함한 종합 코드 리뷰
- `brand-voice/` — 브랜드 음성 일관성 검사기
- `doc-generator/` — API 문서 생성기

**설치**:
```bash
# 개인 스킬
cp -r 03-skills/code-review-specialist ~/.claude/skills/

# 프로젝트 스킬
cp -r 03-skills/code-review-specialist /path/to/project/.claude/skills/
```

**사용법**: 관련 상황에서 자동 호출

</details>

<details>
<summary>04. 서브에이전트</summary>

**위치**: [04-subagents/](04-subagents/)

**정의**: 격리된 컨텍스트와 커스텀 프롬프트를 갖춘 전문화된 AI 어시스턴트

**예제**:
- `code-reviewer.md` — 종합적인 코드 품질 분석
- `test-engineer.md` — 테스트 전략 및 커버리지
- `documentation-writer.md` — 기술 문서
- `secure-reviewer.md` — 보안 중심 리뷰 (읽기 전용)
- `implementation-agent.md` — 전체 기능 구현

**설치**:
```bash
cp 04-subagents/*.md /path/to/project/.claude/agents/
```

**사용법**: 메인 에이전트가 자동으로 위임

</details>

<details>
<summary>05. MCP 프로토콜</summary>

**위치**: [05-mcp/](05-mcp/)

**정의**: 외부 도구 및 API 접근을 위한 Model Context Protocol

**예제**:
- `github-mcp.json` — GitHub 통합
- `database-mcp.json` — 데이터베이스 쿼리
- `filesystem-mcp.json` — 파일 작업
- `multi-mcp.json` — 여러 MCP 서버

**설치**:
```bash
# 환경 변수 설정
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# CLI를 통해 MCP 서버 추가
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 또는 프로젝트 .mcp.json에 수동 추가 (예제는 05-mcp/ 참조)
```

**사용법**: 설정 완료 후 MCP 도구를 Claude가 자동으로 사용 가능

</details>

<details>
<summary>06. 훅</summary>

**위치**: [06-hooks/](06-hooks/)

**정의**: Claude Code 이벤트에 응답하여 자동으로 실행되는 이벤트 기반 셸 명령어

**예제**:
- `format-code.sh` — 쓰기 전 코드 자동 포맷
- `pre-commit.sh` — 커밋 전 테스트 실행
- `security-scan.sh` — 보안 이슈 스캔
- `log-bash.sh` — 모든 bash 명령어 로깅
- `validate-prompt.sh` — 사용자 프롬프트 검증
- `notify-team.sh` — 이벤트 발생 시 팀 알림

**설치**:
```bash
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

`~/.claude/settings.json`에서 훅 구성:
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/format-code.sh"]
    }],
    "PostToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/security-scan.sh"]
    }]
  }
}
```

**사용법**: 훅이 이벤트 발생 시 자동 실행

**훅 유형** (5가지 유형, 29개 이벤트):
- **도구 훅**: `PreToolUse`, `PostToolUse`, `PostToolUseFailure`, `PermissionRequest`
- **세션 훅**: `SessionStart`, `SessionEnd`, `Stop`, `StopFailure`, `SubagentStart`, `SubagentStop`
- **태스크 훅**: `UserPromptSubmit`, `TaskCompleted`, `TaskCreated`, `TeammateIdle`
- **생명주기 훅**: `ConfigChange`, `CwdChanged`, `FileChanged`, `PreCompact`, `PostCompact`, `WorktreeCreate`, `WorktreeRemove`, `Notification`, `InstructionsLoaded`, `Elicitation`, `ElicitationResult`

</details>

<details>
<summary>07. 플러그인</summary>

**위치**: [07-plugins/](07-plugins/)

**정의**: 명령어, 에이전트, MCP, 훅의 번들 컬렉션

**예제**:
- `pr-review/` — 완전한 PR 리뷰 워크플로우
- `devops-automation/` — 배포 및 모니터링
- `documentation/` — 문서 생성

**설치**:
```bash
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

**사용법**: 번들된 슬래시 명령어 및 기능 사용

</details>

<details>
<summary>08. 체크포인트 및 되감기</summary>

**위치**: [08-checkpoints/](08-checkpoints/)

**정의**: 대화 상태를 저장하고 이전 지점으로 되감아 다양한 접근 방식을 탐색

**핵심 개념**:
- **체크포인트**: 대화 상태의 스냅샷
- **되감기**: 이전 체크포인트로 복귀
- **분기점**: 동일 체크포인트에서 여러 접근 방식 탐색

**사용법**:
```
# 체크포인트는 모든 사용자 프롬프트마다 자동 생성
# 되감기는 Esc를 두 번 누르거나 다음 사용:
/rewind

# 다섯 가지 옵션 중 선택:
# 1. 코드와 대화 복원
# 2. 대화 복원
# 3. 코드 복원
# 4. 여기서부터 요약
# 5. 취소
```

**사용 사례**:
- 다양한 구현 접근 방식 시도
- 실수로부터 복구
- 안전한 실험
- 대체 솔루션 비교
- 다양한 디자인 A/B 테스트

</details>

<details>
<summary>09. 고급 기능</summary>

**위치**: [09-advanced-features/](09-advanced-features/)

**정의**: 복잡한 워크플로우와 자동화를 위한 고급 기능

**포함 사항**:
- **계획 모드** — 코딩 전 상세한 구현 계획 수립
- **확장 사고** — 복잡한 문제에 대한 심층 추론 (`Alt+T` / `Option+T`로 전환)
- **백그라운드 태스크** - 차단 없이 장기 작업 실행
- **권한 모드** — `default`, `acceptEdits`, `plan`, `dontAsk`, `bypassPermissions`
- **헤드리스 모드** — CI/CD에서 Claude Code 실행: `claude -p "Run tests and generate report"`
- **세션 관리** — `/resume`, `/rename`, `/fork`, `claude -c`, `claude -r`
- **설정** — `~/.claude/settings.json`에서 동작 사용자 정의

전체 설정은 [config-examples.json](09-advanced-features/config-examples.json)을 참조하세요.

</details>

<details>
<summary>10. CLI 레퍼런스</summary>

**위치**: [10-cli/](10-cli/)

**정의**: Claude Code의 완전한 명령줄 인터페이스 레퍼런스

**빠른 예제**:
```bash
# 대화형 모드
claude "explain this project"

# 출력 모드 (비대화형)
claude -p "review this code"

# 파일 내용 처리
cat error.log | claude -p "explain this error"

# 스크립트용 JSON 출력
claude -p --output-format json "list functions"

# 세션 재개
claude -r "feature-auth" "continue implementation"
```

**사용 사례**: CI/CD 파이프라인 통합, 스크립트 자동화, 배치 처리, 다중 세션 워크플로우, 커스텀 에이전트 설정

</details>

<details>
<summary>예제 워크플로우</summary>

### 완전한 코드 리뷰 워크플로우

```markdown
# 사용: 슬래시 명령어 + 서브에이전트 + 메모리 + MCP

사용자: /review-pr

Claude:
1. 프로젝트 메모리 로드 (코딩 표준)
2. GitHub MCP를 통해 PR 가져오기
3. code-reviewer 서브에이전트에 위임
4. test-engineer 서브에이전트에 위임
5. 결과 종합
6. 종합 리뷰 제공
```

### 자동화된 문서화

```markdown
# 사용: 스킬 + 서브에이전트 + 메모리

사용자: "auth 모듈의 API 문서를 생성해줘"

Claude:
1. 프로젝트 메모리 로드 (문서 표준)
2. 문서 생성 요청 감지
3. doc-generator 스킬 자동 호출
4. api-documenter 서브에이전트에 위임
5. 예제가 포함된 종합 문서 생성
```

### DevOps 배포

```markdown
# 사용: 플러그인 + MCP + 훅

사용자: /deploy production

Claude:
1. 사전 배포 훅 실행 (환경 검증)
2. deployment-specialist 서브에이전트에 위임
3. Kubernetes MCP를 통해 배포 실행
4. 진행 상황 모니터링
5. 사후 배포 훅 실행 (헬스 체크)
6. 상태 보고
```

</details>

<details>
<summary>디렉토리 구조</summary>

```
├── 01-slash-commands/
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   └── README.md
├── 02-memory/
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   └── README.md
├── 03-skills/
│   ├── code-review-specialist/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   └── templates/
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   └── templates/
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   └── README.md
├── 04-subagents/
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   └── README.md
├── 05-mcp/
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
├── 06-hooks/
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   └── README.md
├── 07-plugins/
│   ├── pr-review/
│   ├── devops-automation/
│   ├── documentation/
│   └── README.md
├── 08-checkpoints/
│   ├── checkpoint-examples.md
│   └── README.md
├── 09-advanced-features/
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
├── 10-cli/
│   └── README.md
└── README.md (이 파일)
```

</details>

<details>
<summary>모범 사례</summary>

### Do's
- 슬래시 명령어로 간단하게 시작
- 기능을 점진적으로 추가
- 팀 표준에 메모리 사용
- 설정을 먼저 로컬에서 테스트
- 커스텀 구현 문서화
- 프로젝트 설정을 버전 관리
- 플러그인을 팀과 공유

### Don'ts
- 중복 기능을 만들지 말 것
- 자격 증명을 하드코딩하지 말 것
- 문서화를 건너뛰지 말 것
- 간단한 작업을 과도하게 복잡하게 만들지 말 것
- 보안 모범 사례를 무시하지 말 것
- 민감한 데이터를 커밋하지 말 것

</details>

<details>
<summary>문제 해결</summary>

### 기능이 로드되지 않는 경우
1. 파일 위치와 이름 확인
2. YAML frontmatter 구문 확인
3. 파일 권한 확인
4. Claude Code 버전 호환성 확인

### MCP 연결 실패
1. 환경 변수 확인
2. MCP 서버 설치 확인
3. 자격 증명 테스트
4. 네트워크 연결 확인

### 서브에이전트가 위임되지 않는 경우
1. 도구 권한 확인
2. 에이전트 설명의 명확성 확인
3. 작업 복잡도 검토
4. 에이전트를 독립적으로 테스트

</details>

<details>
<summary>테스트</summary>

이 프로젝트는 포괄적인 자동화 테스트를 포함합니다:

- **단위 테스트**: pytest를 사용한 Python 테스트 (Python 3.10, 3.11, 3.12)
- **코드 품질**: Ruff를 사용한 린팅 및 포맷팅
- **보안**: Bandit을 사용한 취약점 스캔
- **타입 검사**: mypy를 사용한 정적 타입 분석
- **빌드 검증**: EPUB 생성 테스트
- **커버리지 추적**: Codecov 통합

```bash
# 개발 종속성 설치
uv pip install -r requirements-dev.txt

# 모든 단위 테스트 실행
pytest scripts/tests/ -v

# 커버리지 리포트와 함께 테스트 실행
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 코드 품질 검사 실행
ruff check scripts/
ruff format --check scripts/

# 보안 스캔 실행
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# 타입 검사 실행
mypy scripts/ --ignore-missing-imports
```

테스트는 `main`/`develop` 브랜치에 푸시할 때마다 그리고 `main`에 대한 모든 PR에서 자동으로 실행됩니다. 자세한 내용은 [TESTING.md](.github/TESTING.md)를 참조하세요.

</details>

<details>
<summary>EPUB 생성</summary>

이 가이드를 오프라인에서 읽고 싶으신가요? EPUB 전자책을 생성하세요:

```bash
uv run scripts/build_epub.py
```

이 명령어는 렌더링된 Mermaid 다이어그램을 포함한 모든 콘텐츠가 담긴 `claude-howto-guide.epub`을 생성합니다.

더 많은 옵션은 [scripts/README.md](scripts/README.md)를 참조하세요.

</details>

<details>
<summary>기여하기</summary>

문제를 발견하셨거나 예제를 기여하고 싶으신가요? 여러분의 도움을 환영합니다!

**자세한 가이드라인은 [CONTRIBUTING.md](CONTRIBUTING.md)를 참조하세요:**
- 기여 유형 (예제, 문서, 기능, 버그, 피드백)
- 개발 환경 설정 방법
- 디렉토리 구조 및 콘텐츠 추가 방법
- 작성 가이드라인 및 모범 사례
- 커밋 및 PR 프로세스

**커뮤니티 표준:**
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) — 서로를 대하는 방법
- [SECURITY.md](SECURITY.md) — 보안 정책 및 취약점 신고

### 보안 이슈 신고

보안 취약점을 발견한 경우 책임감 있게 신고해 주세요:

1. **GitHub 비공개 취약점 신고 사용**: https://github.com/luongnv89/claude-howto/security/advisories
2. **또는** [.github/SECURITY_REPORTING.md](.github/SECURITY_REPORTING.md)에서 자세한 지침 확인
3. 보안 취약점에 대해 **공개 이슈를 열지 마세요**

빠른 시작:
1. 저장소를 포크하고 클론
2. 설명적인 브랜치 생성 (`add/feature-name`, `fix/bug`, `docs/improvement`)
3. 가이드라인에 따라 변경
4. 명확한 설명과 함께 풀 리퀘스트 제출

**도움이 필요하신가요?** 이슈나 토론을 열면 프로세스를 안내해 드립니다.

</details>

<details>
<summary>추가 자료</summary>

- [Claude Code 문서](https://code.claude.com/docs/en/overview)
- [MCP 프로토콜 명세](https://modelcontextprotocol.io)
- [스킬 저장소](https://github.com/luongnv89/skills) — 바로 사용 가능한 스킬 모음
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [Boris Cherny의 Claude Code 워크플로우](https://x.com/bcherny/status/2007179832300581177) — Claude Code의 창시자가 체계화된 워크플로우 공유: 병렬 에이전트, 공유 CLAUDE.md, Plan 모드, 슬래시 명령어, 서브에이전트, 자율 장기 실행 세션을 위한 검증 훅

</details>

---

## 기여하기

기여를 환영합니다! 시작하는 방법에 대한 자세한 내용은 [기여 가이드](CONTRIBUTING.md)를 참조하세요.

---

## 라이선스

MIT 라이선스 — [LICENSE](../LICENSE) 참조. 자유롭게 사용, 수정, 배포 가능합니다. 유일한 요구사항은 라이선스 표시를 포함하는 것입니다.

---

**최종 업데이트**: 2026년 6월 2일
**Claude Code 버전**: 2.1.160
**출처**:
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/changelog
- https://platform.claude.com/docs/en/about-claude/models/overview
- https://github.com/anthropics/claude-code/releases
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
**호환 모델**: Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
