<!-- i18n-source: .github/TESTING.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->
# 테스트 가이드

이 문서는 Claude How To의 테스트 인프라를 설명합니다.

## 개요

이 프로젝트는 GitHub Actions를 사용하여 모든 푸시와 pull request에 대해 자동으로 테스트를 실행합니다. 테스트 범위:

- **단위 테스트**: pytest를 사용한 Python 테스트
- **코드 품질**: Ruff를 사용한 린팅 및 포맷팅
- **보안**: Bandit을 사용한 취약점 스캔
- **타입 검사**: mypy를 사용한 정적 타입 분석
- **빌드 검증**: EPUB 생성 테스트

## 로컬에서 테스트 실행

### 사전 요구사항

```bash
# uv 설치 (빠른 Python 패키지 관리자)
pip install uv

# 또는 macOS에서 Homebrew 사용
brew install uv
```

### 환경 설정

```bash
# 저장소 클론
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto

# 가상 환경 생성
uv venv

# 활성화
source .venv/bin/activate  # macOS/Linux
# 또는
.venv\Scripts\activate     # Windows

# 개발 의존성 설치
uv pip install -r requirements-dev.txt
```

### 테스트 실행

```bash
# 모든 단위 테스트 실행
pytest scripts/tests/ -v

# 커버리지와 함께 테스트 실행
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 특정 테스트 파일 실행
pytest scripts/tests/test_build_epub.py -v

# 특정 테스트 함수 실행
pytest scripts/tests/test_build_epub.py::test_function_name -v

# 감시 모드로 테스트 실행 (pytest-watch 필요)
ptw scripts/tests/
```

### 린팅 실행

```bash
# 코드 포맷팅 확인
ruff format --check scripts/

# 포맷팅 문제 자동 수정
ruff format scripts/

# 린터 실행
ruff check scripts/

# 린터 문제 자동 수정
ruff check --fix scripts/
```

### 보안 스캔 실행

```bash
# Bandit 보안 스캔 실행
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# JSON 리포트 생성
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/ -f json -o bandit-report.json
```

### 타입 검사 실행

```bash
# mypy로 타입 확인
mypy scripts/ --ignore-missing-imports --no-implicit-optional
```

## GitHub Actions 워크플로우

### 트리거 조건

- **Push** to `main` 또는 `develop` 브랜치 (scripts 변경 시)
- **Pull Request** to `main` (scripts 변경 시)
- 수동 워크플로우 디스패치

### 작업

#### 1. 단위 테스트 (pytest)

- **실행 환경**: Ubuntu latest
- **Python 버전**: 3.10, 3.11, 3.12
- **수행 작업**:
  - `requirements-dev.txt`에서 의존성 설치
  - 커버리지 리포트와 함께 pytest 실행
  - Codecov에 커버리지 업로드
  - 테스트 결과 및 커버리지 HTML 보관

**결과**: 테스트 실패 시 워크플로우 실패 (중요)

#### 2. 코드 품질 (Ruff)

- **실행 환경**: Ubuntu latest
- **Python 버전**: 3.11
- **수행 작업**:
  - `ruff format`으로 코드 포맷팅 확인
  - `ruff check`로 린터 실행
  - 이슈를 보고하지만 워크플로우는 실패하지 않음

**결과**: 비차단 (경고만)

#### 3. 보안 스캔 (Bandit)

- **실행 환경**: Ubuntu latest
- **Python 버전**: 3.11
- **수행 작업**:
  - 보안 취약점 스캔
  - JSON 리포트 생성
  - 아티팩트로 리포트 업로드

**결과**: 비차단 (경고만)

#### 4. 타입 검사 (mypy)

- **실행 환경**: Ubuntu latest
- **Python 버전**: 3.11
- **수행 작업**:
  - 정적 타입 분석 수행
  - 타입 불일치 보고
  - 조기에 버그 발견에 도움

**결과**: 비차단 (경고만)

#### 5. EPUB 빌드

- **실행 환경**: Ubuntu latest
- **의존성**: pytest, lint, security (모두 통과해야 함)
- **수행 작업**:
  - `scripts/build_epub.py`를 사용하여 EPUB 파일 빌드
  - EPUB이 성공적으로 생성되었는지 확인
  - 아티팩트로 EPUB 업로드

**결과**: 빌드 실패 시 워크플로우 실패 (중요)

#### 6. 요약

- **실행 환경**: Ubuntu latest
- **의존성**: 다른 모든 작업
- **수행 작업**:
  - 워크플로우 요약 생성
  - 모든 아티팩트 나열
  - 전체 상태 보고

## 테스트 작성

### 테스트 구조

테스트는 `scripts/tests/`에 `test_*.py` 형식의 이름으로 배치해야 합니다:

```python
# scripts/tests/test_example.py
import pytest
from scripts.example_module import some_function

def test_basic_functionality():
    """some_function이 올바르게 작동하는지 테스트합니다."""
    result = some_function("input")
    assert result == "expected_output"

def test_error_handling():
    """some_function이 오류를 적절히 처리하는지 테스트합니다."""
    with pytest.raises(ValueError):
        some_function("invalid_input")

@pytest.mark.asyncio
async def test_async_function():
    """비동기 함수를 테스트합니다."""
    result = await async_function()
    assert result is not None
```

### 테스트 모범 사례

- **설명적인 이름 사용**: `test_function_returns_correct_value()`
- **테스트당 하나의 assertion** (가능한 경우): 실패 디버깅이 더 쉬움
- **재사용 가능한 설정에 fixture 사용**: `scripts/tests/conftest.py` 참조
- **외부 서비스 모킹**: `unittest.mock` 또는 `pytest-mock` 사용
- **엣지 케이스 테스트**: 빈 입력, None 값, 오류
- **테스트 속도 유지**: sleep() 및 외부 I/O 피하기
- **pytest 마커 사용**: 느린 테스트에 `@pytest.mark.slow`

### Fixture

공통 fixture는 `scripts/tests/conftest.py`에 정의되어 있습니다:

```python
# 테스트에서 fixture 사용
def test_something(tmp_path):
    """tmp_path fixture는 임시 디렉토리를 제공합니다."""
    test_file = tmp_path / "test.txt"
    test_file.write_text("content")
    assert test_file.read_text() == "content"
```

## 커버리지 리포트

### 로컬 커버리지

```bash
# 커버리지 리포트 생성
pytest scripts/tests/ --cov=scripts --cov-report=html

# 브라우저에서 커버리지 리포트 열기
open htmlcov/index.html
```

### 커버리지 목표

- **최소 커버리지**: 80%
- **브랜치 커버리지**: 활성화
- **중점 영역**: 핵심 기능 및 오류 경로

## Pre-commit 훅

이 프로젝트는 pre-commit 훅을 사용하여 커밋 전에 자동으로 검사를 실행합니다:

```bash
# pre-commit 훅 설치
pre-commit install

# 훅 수동 실행
pre-commit run --all-files

# 커밋에 대해 훅 건너뛰기 (권장하지 않음)
git commit --no-verify
```

`.pre-commit-config.yaml`에 구성된 훅:
- Ruff formatter
- Ruff linter
- Bandit security scanner
- YAML 유효성 검사
- 파일 크기 검사
- 병합 충돌 감지

## 문제 해결

### 로컬에서는 통과하지만 CI에서 실패

일반적인 원인:
1. **Python 버전 차이**: CI는 3.10, 3.11, 3.12 사용
2. **의존성 누락**: `requirements-dev.txt` 업데이트
3. **플랫폼 차이**: 경로 구분자, 환경 변수
4. **불안정한 테스트**: 타이밍이나 순서에 의존하는 테스트

해결:
```bash
# 동일한 Python 버전으로 테스트
uv python install 3.10 3.11 3.12

# 깨끗한 환경으로 테스트
rm -rf .venv
uv venv
uv pip install -r requirements-dev.txt
pytest scripts/tests/
```

### Bandit 오탐지

일부 보안 경고는 오탐지일 수 있습니다. `pyproject.toml`에서 설정:

```toml
[tool.bandit]
exclude_dirs = ["scripts/tests"]
skips = ["B101"]  # assert_used 경고 건너뛰기
```

### 타입 검사가 너무 엄격함

특정 파일에 대해 타입 검사 완화:

```python
# 파일 상단에 추가
# type: ignore

# 또는 특정 라인에 대해
some_dynamic_code()  # type: ignore
```

## 지속적 통합 모범 사례

1. **테스트 속도 유지**: 각 테스트는 1초 이내에 완료되어야 함
2. **외부 API 테스트 금지**: 외부 서비스 모킹
3. **격리된 테스트**: 각 테스트는 독립적이어야 함
4. **명확한 assertion**: `assert x` 대신 `assert x == 5`
5. **비동기 테스트 처리**: `@pytest.mark.asyncio` 사용
6. **리포트 생성**: 커버리지, 보안, 타입 검사

## 리소스

- [pytest Documentation](https://docs.pytest.org/)
- [Ruff Documentation](https://docs.astral.sh/ruff/)
- [Bandit Documentation](https://bandit.readthedocs.io/)
- [mypy Documentation](https://mypy.readthedocs.io/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

## 테스트 기여

PR 제출 시:

1. **새 기능에 대한 테스트 작성**
2. **로컬에서 테스트 실행**: `pytest scripts/tests/ -v`
3. **커버리지 확인**: `pytest scripts/tests/ --cov=scripts`
4. **린팅 실행**: `ruff check scripts/`
5. **보안 스캔**: `bandit -r scripts/ --exclude scripts/tests/`
6. **테스트 변경 시 문서 업데이트**

모든 PR에는 테스트가 필요합니다! 🧪

---

테스트에 대한 질문이나 이슈가 있으면 GitHub 이슈 또는 토론을 열어 주세요.
