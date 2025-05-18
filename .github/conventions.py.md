# Conventions

- [Git Convention](#git-convention)
  - [Branch Type Description](#branch-type-description)
  - [Commit Message Convention](#commit-message-convention)
  - [Issue Label Setting](#issue-label-setting)
- [Code Style Convention](#code-style-convention)
  - [Black](#black)
  - [pre-commit](#pre-commit)
- [Cursor Convention](#cursor-convention)
  - [Code Writing](#code-writing)
  - [File Context](#file-context)
  - [Reference](#reference)

## Git Convention

- 깃 브랜치 전략은 [GitFlow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)를 따르며 이를 기반으로 한 브랜치 네이밍 컨벤션을 사용합니다.
- 브랜치 네이밍 형식: `type/[branch/]description[-#issue]`
  - [] 안의 내용은 선택 사항입니다.
  - `type`: 브랜치 타입
  - `branch`: 분기한 브랜치명 (e.g. `dev`, `main`)
  - `description`: 브랜치 설명
  - `issue`: 관련된 이슈 번호

### Branch Type Description

- **feat** (feature)
  새로운 기능을 추가할 때 사용합니다.
  예: `feat/login-#123`
- **fix** (bug fix)
  버그를 수정할 때 사용합니다.
  예: `fix/button-click-#456`
- **docs** (documentation)
  문서 작업(README, 주석 등)을 할 때 사용합니다.
  예: `docs/api-docs-#789`
- **style** (formatting, missing semi colons, …)
  코드 스타일(포맷 수정, 세미콜론 추가 등)을 수정할 때 사용합니다. 기능 수정은 포함되지 않습니다.
  예: `style/css-format-#101`
- **refactor**
  코드 리팩토링(기능 변경 없이 코드 구조 개선)을 할 때 사용합니다.
  예: `refactor/auth-service-#102`
- **test** (when adding missing tests)
  테스트 코드를 추가하거나 수정할 때 사용합니다.
  예: `test/unit-tests-#103`
- **chore** (maintain)
  프로젝트 유지 보수 작업(빌드 설정, 패키지 업데이트 등)을 할 때 사용합니다.
  예: `chore/dependency-update-#104`

### Commit Message Convention

`git config --local commit.template .github/.gitmessage` 명령어를 통해 커밋 메시지 템플릿을 설정할 수 있습니다.
컨벤션 내용은 [AngularJS Git Commit Message Conventions](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)와 [Conventional Commits](https://www.conventionalcommits.org/ko/v1.0.0/)을 기반으로 작성되어 있으며 `.gitmessage` 파일에 작성되어 있습니다.

### Issue Label Setting

`github-label-sync --access-token <access_token> --labels .github/labels.json <owner>/<repo>`

## Code Style Convention

- [PEP 8](https://peps.python.org/pep-0008/)을 준수하여 코드를 작성합니다.
- [Black](https://black.readthedocs.io/en/latest/the_black_code_style/)을 사용하여 코드 스타일을 관리합니다.
- [Flake8](https://flake8.pycqa.org/en/latest/)과 [Pylint](https://pylint.pycqa.org/en/latest/)를 사용하여 코드 품질을 관리합니다.
  - [.flake8](https://flake8.pycqa.org/en/latest/user/configuration.html)
  - [구글 스타일 가이드 .pylintrc](https://google.github.io/styleguide/pyguide.html)

### Black

`pyproject.toml` 추가

```toml
[tool.black]
line-length = 100
target-version = ['py313']
preview = true
```

### pre-commit

`.pre-commit-config.yaml` 생성

```yaml
repos:
  - repo: https://github.com/psf/black
    rev: 25.1.0
    hooks:
      - id: black
        args: ['--target-version', 'py313']
  - repo: https://github.com/pycqa/flake8
    rev: 7.1.1
    hooks:
      - id: flake8
        args: ['--config=.flake8']
  - repo: https://github.com/pylint-dev/pylint
    rev: v3.3.4
    hooks:
      - id: pylint
        args:
          - '--rcfile=.pylintrc'
```

```shell
pip install pre-commit
# or
uv add pre-commit

pre-commit install
# or
uv run pre-commit install
```

## Cursor Convention

### Code Writing

1. 각 코드 파일의 길이를 500줄 이하로 유지하세요.

> Cursor는 기본적으로 파일의 처음 250줄을 읽고, 필요 시 추가로 250줄을 더 읽습니다. 따라서 파일 길이를 500줄 이하로 유지하면 전체 파일을 읽을 수 있어 코드 이해와 처리가 원활해집니다.

2. 각 코드 파일의 첫 100줄에 해당 파일의 기능과 구현 로직을 명확히 문서화하세요.

> Cursor는 파일 검색 시 최대 100줄의 코드를 읽습니다. 파일의 초반부에 주석을 통해 해당 파일의 목적과 주요 로직을 설명하면, Cursor 에이전트가 파일의 역할을 빠르게 파악하여 적절한 처리를 수행할 수 있습니다.

3. 프로젝트의 상태와 구조를 `README.md`와 같은 전용 파일에 정기적으로 문서화하세요.

> 프로젝트의 전반적인 상태와 구조를 문서화하면 Cursor가 프로젝트를 빠르게 이해하고, 대화 재시작 시 불필요한 컨텍스트를 최소화할 수 있습니다.

### File Context

1. 프로젝트 구조를 이해하고 특정 파일을 대상으로 작업할 때는 Cursor의 `@` 기능을 활용하세요.

> Cursor에서 `@`를 사용하여 특정 파일을 지정하면 해당 파일을 최대한 완전히 읽으려 시도합니다. (최대 2000줄) 이를 통해 필요한 코드 컨텍스트를 확보하여 작업의 정확성을 높일 수 있습니다.

2. `@[파일/폴더]` 태그를 적극적으로 활용하세요.

> Cursor의 `@[파일/폴더]` 태그를 사용하여 특정 파일이나 폴더를 지정하면, 해당 파일들의 전체 내용(최대 2000자)을 언어 모델에 전달할 수 있습니다. 이를 통해 모델이 필요한 컨텍스트를 충분히 확보하여 더 정확한 코드를 생성하거나 수정할 수 있습니다.

3. 새로운 기능을 추가하거나 버그를 수정한 후에는 대화를 재시작하세요.

> 작업 후 대화를 재시작하면 긴 컨텍스트로 인한 혼란을 방지하고, 프로젝트의 최신 상태를 반영한 새로운 컨텍스트로 작업을 이어갈 수 있습니다.

### Reference

- [Understanding Cursor and WindSurf's Code Indexing Logic](https://www.pixelstech.net/article/1734832711-understanding-cursor-and-windsurf-s-code-indexing-logic)
- [How Cursor (AI IDE) Works](https://blog.sshh.io/p/how-cursor-ai-ide-works)
