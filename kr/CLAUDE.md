<!-- i18n-source: CLAUDE.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

# CLAUDE.md

튜토리얼 저장소입니다. 출력물은 앱이 아닌 `01-`부터 `10-`까지 번호가 매겨진 모듈의 마크다운입니다. `scripts/`의 스크립트는 문서 검증과 EPUB 빌드만을 위해 존재합니다.

스택/명령어에 대해서는 `.claude/CLAUDE.md`를, 레슨 구조에 대해서는 `STYLE_GUIDE.md`를 참고하세요.

## 주요 명령어

```bash
# 품질 게이트 (pre-commit 훅을 통해 커밋 시에도 실행됨)
pre-commit run --all-files

# 테스트
pytest scripts/tests/ -v

# EPUB 빌드 (Mermaid 렌더링을 위해 Kroki.io API 호출 — 네트워크 필요)
uv run scripts/build_epub.py

# Python 도구
ruff check scripts/ && ruff format scripts/
mypy scripts/ --ignore-missing-imports
bandit -c scripts/pyproject.toml -r scripts/ --exclude scripts/tests/
```

Pre-commit은 5가지 검사를 실행합니다: markdown-lint, cross-references, mermaid-syntax, link-check, build-epub (`.md` 변경 시). 모두 통과해야 합니다.

## 아키텍처 맵

- `01-` … `10-` — 튜토리얼 모듈. **번호 접두사 = 학습 순서**이며, 알파벳 순서가 아닙니다. 순서를 변경하지 마세요.
- 각 모듈: `README.md` + 복사-붙여넣기 템플릿 (`.md`, `.json`, `.sh`).
- `scripts/` — 유틸리티 (EPUB 빌더, 링크/Mermaid/교차 참조 검증기). 제품이 아닙니다.
- `02-memory/*.md` — 사용자가 자신의 프로젝트에 복사하는 CLAUDE.md 템플릿입니다. 이 파일과 혼동하지 마세요.
- `openspec/` — 스펙 기반 변경 제안.

## 엄격한 규칙

- **명시적인 사용자 요청 없이 커밋하거나 푸시해서는 안 됩니다.**
- **커밋 메시지에 `Co-Authored-By: Claude`를 추가해서는 안 됩니다.**
- Python 스크립트 실행 전에는 항상 `.venv`를 활성화하세요 (`venv/`, `.venv/`, `env/` 확인).
- 내부 링크는 **상대 경로**를 사용하며 (예: `01-slash-commands/README.md`), 앵커는 `#heading-name`을 사용합니다.
- 코드 펜스는 **반드시** 언어를 명시해야 합니다 (`bash`, `python`, `json`, …). 그렇지 않으면 교차 참조 검사가 실패합니다.
- 외부 URL은 접근 가능하고 안정적이어야 합니다. 임시 링크는 사용할 수 없습니다.
- Mermaid 다이어그램은 파싱되어야 합니다 (pre-commit에서 검증됨). EPUB 빌드 실패는 보통 잘못된 Mermaid 또는 Kroki 네트워크 문제입니다.
- 커밋 형식: `type(scope): subject`에서 `scope`는 모듈 폴더와 일치해야 합니다 (예: `feat(slash-commands):`, `docs(memory):`, `fix(README):`).
- `01-`–`10-` 번호 순서를 재구성하지 마세요. 순서는 커리큘럼입니다.

## 워크플로우 선호 사항

- 레슨 편집 시 `STYLE_GUIDE.md`의 구조/명명/다이어그램 규칙을 따르세요.
- 작은 수정은 최소 diff로 처리하세요. 오타 하나 고치려고 섹션 전체를 다시 작성하지 마세요.
- 모듈 페이지를 추가할 때: 먼저 README + 템플릿을 작성한 후, 순서/일정이 변경되면 루트 `README.md` 인덱스와 `LEARNING-ROADMAP.md`를 업데이트하세요.
- 튜토리얼 > 라이브러리: 재사용 가능한 추상화보다 명확한 설명과 복사-붙여넣기 예제를 우선시하세요.
- 품질 검사가 실패하면 근본 원인을 해결하세요. `--no-verify`로 우회하지 마세요.

## 토큰 효율

- 방금 작성하거나 편집한 파일을 다시 읽지 마세요. 내용을 알고 있습니다.
- 결과가 불확실하지 않은 한 명령어를 다시 실행하여 "확인"하지 마세요.
- 요청하지 않은 이상 큰 코드 블록이나 파일 내용을 다시 출력하지 마세요.
- 관련 편집을 단일 작업으로 묶으세요. 1번으로 처리할 수 있는데 5번의 편집을 하지 마세요.
- "계속하겠습니다..." 같은 확인 메시지를 생략하세요. 그냥 실행하세요.
- 작업에 1개의 도구 호출이 필요하면 3개를 사용하지 마세요. 실행 전에 계획하세요.
- 결과가 모호하거나 추가 입력이 필요하지 않은 이상 방금 한 일을 요약하지 마세요.
