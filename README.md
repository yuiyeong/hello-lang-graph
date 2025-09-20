# hello-lang-graph

LangGraph를 학습하고 실험하기 위한 플레이그라운드입니다.
초기에는 Jupyter Notebook에서 개념과 패턴을 익히고(notebooks), 이후 `src/hello_langgraph` 패키지 아래에서 API 또는 간단한 UI로 발전시키는 것을 목표로 합니다.

## 목표

- LangGraph의 State / Node / Edge /Conditional Edge / Loop 등의 핵심 개념을 실습으로 익히기
- Notebook에서 실험한 그래프를 재사용 가능한 모듈로 구성
- 추후 API(UI) 계층에서 해당 그래프를 호출해 서비스 형태로 제공

## 프로젝트 구조

- notebooks: 실험과 학습용 노트북
- src/hello_langgraph: 패키지 코드(그래프, 상태 정의, 유틸 등)
- pyproject.toml: 의존성 및 빌드 설정
- ruff.toml: 정적 분석/포매팅 설정
- uv.lock: 의존성 잠금 파일

## 기술 스택

- Python 3.12
- 패키지/환경: uv
- LLM/그래프: LangGraph, LangChain
- 실험: JupyterLab, ipykernel
- 품질: ruff, pre-commit
- 설정/비밀값: .env(python-dotenv)

## 빠른 시작

사전 준비

- Python 3.12 이상
- uv 설치: https://github.com/astral-sh/uv

### 의존성 설치

```bash
uv sync --extra dev
```

### JupyterLab 실행

```bash
uv run jupyter lab
```

### Notebook 위치

- notebooks 폴더에서 학습/실험을 진행합니다.
- 처음 시작할 때는 제공된 노트북부터 열어 워크플로우를 경험해 보세요.

## 개발 워크플로우

### 1) 노트북으로 아이디어 검증

- 상태(State) 설계, 노드(Node) 함수, 전이(Edge) 정의, 조건 분기/루프 등을 실험합니다.
- 그래프가 안정화되면 패키지 코드로 옮겨 재사용 가능하게 리팩터링합니다.

### 2) 패키지로 구조화

- src/hello_langgraph 아래에 상태/그래프/유틸을 모듈화합니다.
- 노트북 실험 코드는 최소한의 변경으로 import해서 재사용 가능하도록 구성합니다.

### 3) API 또는 UI 계층 추가(로드맵)

- API: 간단한 엔드포인트에서 그래프 실행 → 입력을 상태로 변환 → 결과 반환
- UI: 폼/채팅 인터페이스로 그래프와 상호작용
- 운영 고려: 입력 검증, 로깅/모니터링, 에러 처리, 타임아웃/취소 관리

### 4) 환경 변수/설정

- python-dotenv를 사용하므로, 루트에 .env 파일을 두고 키/엔드포인트/모델명 등을 관리합니다.
- 예: LLM 모델명, 외부 도구 API 키 등

## 코드 품질

### Ruff 검사/포맷

```bash
uv run ruff check .
uv run ruff format .
```

### pre-commit 훅 설치/실행

```bash
uv run pre-commit install
uv run pre-commit run -a
```

## Git Commit Message

### 1. feat: 새로운 기능 추가
```bash
feat: add user authentication via OAuth
feat: implement dark mode toggle
feat: create shopping cart persistence
feat: enable multi-language support
feat: add PDF export functionality
```

### 2. fix: 버그 수정
```bash
fix: resolve memory leak in image processor
fix: correct timezone calculation error
fix: prevent duplicate form submissions
fix: handle null user session gracefully
fix: restore broken pagination logic
```

### 3. docs: 문서 수정
```bash
docs: update API endpoint examples
docs: add troubleshooting guide
docs: clarify installation prerequisites
docs: document environment variables
docs: translate README to Korean
```

### 4. style: 코드 포맷팅, 세미콜론 누락 등
```bash
style: format code with prettier
style: fix indentation inconsistencies
style: remove trailing whitespaces
style: apply ESLint rules
style: organize imports alphabetically
```

### 5. refactor: 코드 리팩토링
```bash
refactor: extract validation logic to utils
refactor: simplify nested conditionals
refactor: replace loops with array methods
refactor: migrate class to functional component
refactor: consolidate duplicate API calls
```

### 6. test: 테스트 추가/수정
```bash
test: add unit tests for auth service
test: update mocked API responses
test: increase coverage for edge cases
test: fix flaky integration tests
test: add E2E tests for checkout flow
```

### 7. chore: 빌드, 패키지 매니저 등
```bash
chore: update dependencies to latest
chore: configure GitHub Actions workflow
chore: add pre-commit hooks
chore: upgrade Node.js to v20
chore: clean up deprecated packages
```

### 8. perf: 성능 개선
```bash
perf: optimize image loading with lazy load
perf: reduce bundle size by 30%
perf: implement request caching
perf: memoize expensive calculations
perf: add database query indexes
```

### 9. ci: CI 설정 수정
```bash
ci: add automated deployment pipeline
ci: configure test coverage reporting
ci: set up Docker build workflow
ci: add security vulnerability scanning
ci: enable parallel test execution
```

### 10. build: 빌드 시스템/외부 의존성
```bash
build: migrate from Webpack to Vite
build: configure absolute imports
build: add source map generation
build: update TypeScript configuration
build: optimize production build settings
```

### 11. revert: 커밋 되돌리기
```bash
revert: "feat: add payment processing"
revert: accidental deletion of config file
revert: problematic database migration
revert: experimental feature flag
```

### 12. hotfix: 긴급 수정
```bash
hotfix: patch critical security vulnerability
hotfix: restore production database connection
hotfix: fix payment gateway timeout
hotfix: correct pricing calculation error
```

### 🎯 황금률

1. **50자 이내** 제목 작성
2. **현재형 동사** 사용 (add, fix, update)
3. **첫 글자** 소문자 또는 대문자 일관성
4. **불필요한 단어** 제거 (about, the, a)
5. **구체적**이되 **간결**하게
