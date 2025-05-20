# Conventions

- [Git Convention](#git-convention)
  - [Branch Type Description](#branch-type-description)
  - [Commit Message Convention](#commit-message-convention)
  - [Issue Label Setting](#issue-label-setting)
- [Code Style Convention](#code-style-convention)
  - [Black](#black)
  - [pre-commit](#pre-commit)
- [Comment Convention](#comment-convention)
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
        args: ["--target-version", "py313"]
  - repo: https://github.com/pycqa/flake8
    rev: 7.1.1
    hooks:
      - id: flake8
        args: ["--config=.flake8"]
  - repo: https://github.com/pylint-dev/pylint
    rev: v3.3.4
    hooks:
      - id: pylint
        args:
          - "--rcfile=.pylintrc"
```

```shell
pip install pre-commit
# or
uv add pre-commit

pre-commit install
# or
uv run pre-commit install
```

## Comment Convention

- [Todo Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight) Extension 설치
- Commend Palette -> `Preferences: Open User Settings (JSON)` -> 아래 코드 추가
  - TODO: 해야 할 작업 표시 (미구현 기능, 추가 개발 필요 사항)
  - NOTE: 중요한 설명이나 주의사항 기록
  - FIXME: 수정이 필요한 버그나 문제점 표시
  - TEST: 테스트가 필요한 부분이나 테스트 케이스 기록

```json
{
  "todohighlight.include": [
    "**/*.js",
    "**/*.jsx",
    "**/*.ts",
    "**/*.tsx",
    "**/*.html",
    "**/*.php",
    "**/*.css",
    "**/*.scss",
    "**/*.py",
    "*/*"
  ],
  "todohighlight.exclude": [
    "**/node_modules/**",
    "**/bower_components/**",
    "**/dist/**",
    "**/build/**",
    "**/.vscode/**",
    "**/.github/**",
    "**/_output/**",
    "**/*.min.*",
    "**/*.map",
    "**/.next/**"
  ],
  "todohighlight.maxFilesForSearch": 5120,
  "todohighlight.toggleURI": false,
  "todohighlight.isEnable": true,
  "todohighlight.isCaseSensitive": true,
  "todohighlight.defaultStyle": {
    "color": "red",
    "backgroundColor": "#2B2B2B",
    "overviewRulerColor": "#ffab00",
    "cursor": "pointer",
    "border": "1px solid #eee",
    "borderRadius": "2px",
    "isWholeLine": true
  },
  "todohighlight.keywords": [
    // Common
    {
      "text": "TODO:",
      "color": "#DFB6FF",
      "backgroundColor": "#2B2B2B",
      "overviewRulerColor": "#DFB6FF"
    },
    {
      "text": "NOTE:",
      "color": "#98ECAB",
      "backgroundColor": "#2B2B2B",
      "overviewRulerColor": "#98ECAB"
    },
    {
      "text": "FIXME:",
      "color": "#FFB3B3",
      "backgroundColor": "#2B2B2B",
      "overviewRulerColor": "#FFB3B3"
    },
    {
      "text": "TEST:",
      "color": "#C8B9FF",
      "backgroundColor": "#2B2B2B",
      "overviewRulerColor": "#C8B9FF"
    },
    // python linter
    {
      "text": "pylint:",
      "color": "#9B6BAB",
      "backgroundColor": "#2B2B2B",
      "overviewRulerColor": "#9B6BAB"
    },
    {
      "text": "flake8:",
      "color": "#55A465",
      "backgroundColor": "#2B2B2B",
      "overviewRulerColor": "#55A465"
    },
    {
      "text": "noqa:",
      "color": "#407FBF",
      "backgroundColor": "#2B2B2B",
      "overviewRulerColor": "#407FBF"
    }
  ]
}
```

## Cursor Convention

### Code Writing

1. 각 코드 파일의 길이를 500줄 이하로 유지하세요.

> Cursor는 기본적으로 파일의 처음 250줄을 읽고, 필요 시 추가로 250줄을 더 읽습니다. 따라서 파일 길이를 500줄 이하로 유지하면 전체 파일을 읽을 수 있어 코드 이해와 처리가 원활해집니다.

2. 각 코드 파일의 첫 100줄에 해당 파일의 기능과 구현 로직을 명확히 문서화하세요.

> Cursor는 파일 검색 시 최대 100줄의 코드를 읽습니다. 파일의 초반부에 주석을 통해 해당 파일의 목적과 주요 로직을 설명하면, Cursor 에이전트가 파일의 역할을 빠르게 파악하여 적절한 처리를 수행할 수 있습니다.

```tsx
/**
 * @file UserProfile.tsx
 * @description 사용자 프로필 페이지 컴포넌트
 *
 * 이 컴포넌트는 사용자의 프로필 정보를 표시하고 수정하는 기능을 제공합니다.
 *
 * 주요 기능:
 * 1. 사용자 기본 정보 표시 (이름, 이메일, 프로필 이미지)
 * 2. 프로필 정보 수정
 * 3. 프로필 이미지 업로드
 *
 * 구현 로직:
 * - Supabase Auth를 통한 사용자 인증 상태 확인
 * - React Query를 사용한 프로필 데이터 fetching
 * - 이미지 업로드를 위한 Supabase Storage 활용
 * - Form 상태 관리를 위한 React Hook Form 사용
 *
 * @dependencies
 * - @supabase/ssr
 * - @tanstack/react-query
 * - react-hook-form
 * - @heroicons/react
 */

import { useEffect } from "react";
import { useForm } from "react-hook-form";
import { useQuery, useMutation } from "@tanstack/react-query";
import { createClient } from "@/utils/supabase/client";
import { UserCircleIcon } from "@heroicons/react/24/outline";

// ... 컴포넌트 구현 ...
```

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
