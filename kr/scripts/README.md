<!-- i18n-source: scripts/README.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# 빌드 스크립트

이 디렉토리에는 튜토리얼 마크다운 파일을 배포 가능한 형식으로 변환하는 두 개의 생성기가 포함되어 있습니다:

- [**EPUB 빌더**](#epub-빌더-스크립트) — `build_epub.py`
- [**정적 웹사이트 빌더**](#정적-웹사이트-빌더) — `build_website.py`

둘 다 `.md` 파일을 단일 진실 공급원으로 취급합니다 — 마크다운 편집 후 관련 스크립트를 다시 실행하여 출력을 재생성하세요.

---

# EPUB 빌더 스크립트

Claude How-To 마크다운 파일로 EPUB 전자책을 빌드합니다.

## 기능

- 폴더 구조별로 챕터 구성 (01-slash-commands, 02-memory 등)
- Kroki.io API를 통해 Mermaid 다이어그램을 PNG 이미지로 렌더링
- 비동기 동시 가져오기 — 모든 다이어그램을 병렬로 렌더링
- 프로젝트 로고에서 커버 이미지 생성
- 내부 마크다운 링크를 EPUB 챕터 참조로 변환
- 엄격한 오류 모드 — 다이어그램을 렌더링할 수 없으면 실패

## 요구사항

- Python 3.10+
- [uv](https://github.com/astral-sh/uv)
- Mermaid 다이어그램 렌더링을 위한 인터넷 연결

## 빠른 시작

```bash
# 가장 간단한 방법 - uv가 모든 것을 처리
uv run scripts/build_epub.py
```

## 개발 설정

```bash
# 가상 환경 생성
uv venv

# 활성화 및 의존성 설치
source .venv/bin/activate
uv pip install -r requirements-dev.txt

# 테스트 실행
pytest scripts/tests/ -v

# 스크립트 실행
python scripts/build_epub.py
```

## 명령줄 옵션

```
usage: build_epub.py [-h] [--root ROOT] [--output OUTPUT] [--verbose]
                     [--timeout TIMEOUT] [--max-concurrent MAX_CONCURRENT]

options:
  -h, --help            도움말 메시지 표시 및 종료
  --root, -r ROOT       루트 디렉토리 (기본값: 저장소 루트)
  --output, -o OUTPUT   출력 경로 (기본값: claude-howto-guide.epub)
  --verbose, -v         상세 로깅 활성화
  --timeout TIMEOUT     API 타임아웃 (초) (기본값: 30)
  --max-concurrent N    최대 동시 요청 수 (기본값: 10)
```

## 예시

```bash
# 상세 출력으로 빌드
uv run scripts/build_epub.py --verbose

# 사용자 지정 출력 위치
uv run scripts/build_epub.py --output ~/Desktop/claude-guide.epub

# 동시 요청 제한 (속도 제한이 있는 경우)
uv run scripts/build_epub.py --max-concurrent 5
```

## 출력

저장소 루트 디렉토리에 `claude-howto-guide.epub`를 생성합니다.

EPUB에는 다음이 포함됩니다:
- 프로젝트 로고가 있는 커버 이미지
- 중첩된 섹션이 있는 목차
- EPUB 호환 HTML로 변환된 모든 마크다운 콘텐츠
- PNG 이미지로 렌더링된 Mermaid 다이어그램

## 테스트 실행

```bash
# 가상 환경 사용
source .venv/bin/activate
pytest scripts/tests/ -v

# 또는 uv로 직접 실행
uv run --with pytest --with pytest-asyncio \
    --with ebooklib --with markdown --with beautifulsoup4 \
    --with httpx --with pillow --with tenacity \
    pytest scripts/tests/ -v
```

## 의존성

PEP 723 인라인 스크립트 메타데이터로 관리:

| 패키지 | 용도 |
|---------|------|
| `ebooklib` | EPUB 생성 |
| `markdown` | Markdown을 HTML로 변환 |
| `beautifulsoup4` | HTML 파싱 |
| `httpx` | 비동기 HTTP 클라이언트 |
| `pillow` | 커버 이미지 생성 |
| `tenacity` | 재시도 로직 |

## 문제 해결

**네트워크 오류로 빌드 실패**: 인터넷 연결 및 Kroki.io 상태 확인. `--timeout 60` 시도.

**속도 제한**: `--max-concurrent 3`으로 동시 요청 감소.

**로고 없음**: `claude-howto-logo.png`가 없으면 스크립트가 텍스트 전용 커버를 생성합니다.

---

# 정적 웹사이트 빌더

EPUB 빌드에서 사용하는 것과 동일한 마크다운 파일로 모바일 친화적인 우아한 정적 웹사이트를 생성합니다. 웹사이트는 렌더링된 뷰이며, `.md` 파일은 단일 진실 공급원으로 유지됩니다.

## 기능

- 마크다운 소스당 하나의 HTML 페이지 — 내부 `.md` 링크는 사이트의 해당 페이지로 다시 작성됨
- 마크다운이 아닌 저장소 파일(템플릿, 스크립트, JSON)에 대한 참조는 github.com에서 소스를 여는 GitHub blob URL이 됨
- Mermaid 다이어그램은 빌드된 사이트에서 제공되는 `mermaid.min.js`를 통해 클라이언트 측에서 렌더링 (런타임에 CDN 없음)
- 독립 실행형 CLI(Go 바이너리, Node.js 불필요)로 컴파일된 Tailwind CSS를 빌드된 사이트에서 제공 — 사이드바 탐색, 페이지 내 TOC, 다크 모드 토글, 이전/다음 페이지 탐색이 포함된 반응형 레이아웃
- Inter + JetBrains Mono 폰트는 CSS와 함께 자체 호스팅 — 페이지 로드 시 타사 요청 없음
- EPUB 커리큘럼 순서 반영 (`01-` … `10-` plus 최상위 문서)
- 일반 정적 파일로 호스팅 가능 — GitHub Pages에 배포하도록 설계

## 빠른 시작

```bash
# 영어 웹사이트를 ./site/에 빌드
uv run scripts/build_website.py

# 로컬에서 미리보기
python -m http.server --directory site 8080
# 그런 다음 http://localhost:8080 열기
```

## 명령줄 옵션

```
usage: build_website.py [-h] [--root ROOT] [--output OUTPUT]
                        [--lang {en,vi,zh,ja,uk}] [--repo-url REPO_URL]
                        [--branch BRANCH] [--verbose]

options:
  --root, -r ROOT       소스 루트 (기본값: 저장소 루트)
  --output, -o OUTPUT   출력 디렉토리 (기본값: <repo>/site)
  --lang LANG           빌드할 언어: en | vi | zh | ja | uk
  --repo-url URL        blob 링크용 GitHub 저장소 (기본값: luongnv89/claude-howto)
  --branch BRANCH       blob 링크용 브랜치 (기본값: main)
  --verbose, -v         상세 로깅 활성화
```

## GitHub Pages 배포

저장소는 `main`에 푸시될 때마다 (`.md` 또는 생성기 파일이 변경될 때) 사이트를 빌드하고 `actions/deploy-pages`를 통해 게시하는 `.github/workflows/pages.yml` 워크플로우를 제공합니다. 활성화하려면 저장소 설정에서 **Source: GitHub Actions**로 GitHub Pages를 활성화하세요.

## 아키텍처

`build_website.py`는 `build_epub.py`의 챕터 순서 로직을 재사용하고 `scripts/website_templates/` 아래에 HTML 템플릿을 제공합니다:

- `page.html.j2` — 사이드바 탐색, TOC, 이전/다음이 있는 페이지당 Jinja2 템플릿
- `tailwind.config.js`, `tailwind.input.css` — Tailwind 독립 실행형 CLI용 설정 + 진입 CSS; CLI는 빌드된 HTML을 스캔하여 실제로 사용된 유틸리티만 포함하는 `site/assets/tailwind.css`를 생성
- `site.css` — 사이트별 스타일 + Pygments 테마의 작은 레이어

Tailwind CLI 바이너리, Mermaid 번들 및 폰트 파일은 첫 번째 빌드 시 `scripts/.vendor-cache/`(gitignored)에 다운로드됩니다 — `scripts/vendor_assets.py` 참조.

제목 앵커는 `check_cross_references.heading_to_anchor`의 정확한 알고리즘을 사용하여 생성되므로 pre-commit 훅에서 검증된 `#anchor` 링크가 렌더링된 사이트에서 올바르게 확인됩니다.
