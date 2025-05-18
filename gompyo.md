# Gompyo ERP 인턴 온보딩 가이드

- [Gompyo ERP 인턴 온보딩 가이드](#gompyo-erp-인턴-온보딩-가이드)
  - [1. 프로젝트 소개 및 목적](#1-프로젝트-소개-및-목적)
  - [2. 기술 스택 및 아키텍처 개요](#2-기술-스택-및-아키텍처-개요)
  - [3. 개발 환경 설정 가이드](#3-개발-환경-설정-가이드)
  - [4. 코드베이스 탐색 가이드](#4-코드베이스-탐색-가이드)
  - [5. 워크플로우 및 협업 프로세스](#5-워크플로우-및-협업-프로세스)
  - [6. 테스트 방법](#6-테스트-방법)
  - [7. 자주 묻는 질문 및 문제 해결 가이드](#7-자주-묻는-질문-및-문제-해결-가이드)
  - [8. 학습 로드맵 및 리소스](#8-학습-로드맵-및-리소스)
  - [Gompyo ERP 상세 코드 설명: 핵심 비즈니스 로직 및 데이터베이스](#gompyo-erp-상세-코드-설명-핵심-비즈니스-로직-및-데이터베이스)
    - [핵심 비즈니스 로직](#핵심-비즈니스-로직)
    - [데이터베이스 (DB)](#데이터베이스-db)

Gompyo ERP 프로젝트에 오신 인턴 여러분을 환영합니다\! 이 문서는 여러분이 프로젝트에 빠르게 적응하고 기여할 수 있도록 돕기 위해 만들어졌습니다.

## 1. 프로젝트 소개 및 목적

**프로젝트 비전 및 목표:**

Gompyo ERP는 효율적인 자원 관리를 목표로 하는 전사적 자원 관리(ERP) 시스템입니다. 이 프로젝트는 복잡한 비즈니스 프로세스를 간소화하고 데이터 기반 의사결정을 지원하여 기업의 생산성 향상에 기여하는 것을 비전으로 합니다.

**주요 기능 및 사용자 가치:**

- **계획 관리:** 계약 정보, 선적 정보, 화물 정보 등을 종합적으로 관리하여 효율적인 계획 수립을 지원합니다.
- **선적 관리:** 선적 현황을 추적하고 관련 정보를 일목요연하게 파악할 수 있도록 돕습니다.
- **수금 및 지출 관리:** 현금 흐름을 체계적으로 관리하고 재정 상태를 명확하게 파악할 수 있도록 지원합니다.
- **상세 정보 조회:** 각 화물 및 계약 건에 대한 상세 정보를 조회하고 관련 문서를 관리할 수 있습니다.
- **정보 관리:** 계좌번호, 사업자번호, 외부 링크 등 필요한 정보를 편리하게 관리합니다.

사용자는 Gompyo ERP를 통해 분산된 정보를 통합적으로 관리하고, 업무 효율성을 높이며, 데이터에 기반한 정확한 의사결정을 내릴 수 있습니다.

## 2. 기술 스택 및 아키텍처 개요

**사용된 프로그래밍 언어, 프레임워크, 라이브러리:**

- **언어:** TypeScript, JavaScript
- **프레임워크/라이브러리:**
  - **Frontend:** Next.js, React, Material-UI (MUI), Tailwind CSS, AG Grid, ApexCharts
  - **Backend/DB:** Node.js (Next.js API routes), Drizzle ORM, better-sqlite3
  - **상태 관리:** Jotai
  - **폼 처리:** React Hook Form, Zod
  - **테스팅:** Jest, Playwright, React Testing Library
  - **코드 스타일:** ESLint, Prettier
  - **아이콘:** Heroicons
- **패키지 매니저:** pnpm

**시스템 구조 다이어그램:**

현재 제공된 파일 내에서 명시적인 시스템 구조 다이어그램은 확인되지 않았습니다. 일반적으로 Next.js 기반 애플리케이션은 클라이언트 사이드 렌더링(CSR)과 서버 사이드 렌더링(SSR) 또는 정적 사이트 생성(SSG)을 혼합하여 사용하며, API 라우트를 통해 백엔드 로직을 처리합니다. 데이터베이스는 SQLite를 사용하며 Drizzle ORM을 통해 접근합니다.

**주요 컴포넌트와 그 기능:**

- **`src/app`**: Next.js의 App Router를 사용하여 라우팅 및 페이지 레이아웃을 정의합니다. 각 페이지(e.g., `dashboard`, `plan`, `shipment`, `cashflow`, `detail`, `info`)는 해당 기능을 담당하는 컨테이너 컴포넌트를 렌더링합니다.
- **`src/containers`**: 각 페이지의 주요 UI 및 로직을 담고 있는 컨테이너 컴포넌트들이 위치합니다. (e.g., `Dashboard`, `Plan`, `Shipment`, `Cashflow`, `DetailView`, `Login`, `Navbar` 등)
- **`src/components`**: 공통적으로 사용되는 UI 컴포넌트들(e.g., `DataGrid`, `DetailForm`, `DynamicForm`, `LinkCard`)이 위치합니다.
- **`src/actions`**: 서버 액션을 정의하여 클라이언트와 서버 간의 데이터 요청 및 변경 작업을 처리합니다. (e.g., `plan.ts`, `shipment.ts`, `cashflow.ts`, `document-actions.ts`)
- **`src/services`**: 데이터베이스 CRUD 및 비즈니스 로직을 처리하는 서비스 레이어입니다. (e.g., `cargo.service.ts`, `contract.service.ts`, `payment.service.ts`)
- **`src/db`**: Drizzle ORM을 사용한 데이터베이스 스키마(`schema.ts`) 및 마이그레이션 파일, 시드 데이터 등이 포함됩니다.
- **`src/states`**: Jotai를 사용한 전역 상태 관리 로직이 위치합니다.

## 3. 개발 환경 설정 가이드

**환경 요구 사항:**

- **Node.js:** v23.10.0
- **pnpm:** v10.7.0

**레포지토리 클론 및 초기 설정 단계:**

1.  GitHub 레포지토리를 로컬 환경으로 클론합니다.

    ```bash
    git clone https://github.com/dev-demo-edu/gompyo
    cd https://github.com/dev-demo-edu/gompyo
    ```

2.  pnpm을 사용하여 의존성을 설치합니다.

    ```bash
    pnpm install
    ```

3.  데이터베이스를 설정합니다. (SQLite 사용)

    ```bash
    pnpm db:push
    ```

    필요에 따라 시드 데이터를 주입할 수 있습니다.

    ```bash
    pnpm db:seed
    ```

**로컬 개발 환경 실행 방법:**

다음 명령어를 사용하여 개발 서버를 실행합니다.

```bash
pnpm run dev
```

기본적으로 `http://localhost:3000` 에서 애플리케이션을 확인할 수 있습니다.

## 4. 코드베이스 탐색 가이드

**폴더 구조 설명:**

```
.
├── .github/             # GitHub 관련 설정 (워크플로우, 이슈/PR 템플릿, 컨벤션 문서 등)
├── __mocks__/           # Jest 테스트를 위한 Mock 파일
├── __tests__/           # Jest 테스트 파일
├── drizzle/             # Drizzle ORM 마이그레이션 및 스키마 스냅샷
├── public/              # 정적 에셋 (이미지 등)
├── src/
│   ├── actions/         # Next.js 서버 액션
│   ├── app/             # Next.js App Router (페이지, 레이아웃)
│   ├── components/      # 재사용 가능한 UI 컴포넌트
│   ├── constants/       # 공통 상수
│   ├── containers/      # 페이지 레벨의 컨테이너 컴포넌트
│   ├── db/              # 데이터베이스 스키마, 시드 데이터, 마이그레이션 관련
│   ├── hooks/           # 커스텀 React Hooks
│   ├── libs/            # 외부 라이브러리 래핑 또는 유틸리티
│   ├── services/        # 비즈니스 로직 및 DB 인터페이스
│   ├── states/          # Jotai를 사용한 전역 상태 관리
│   ├── styles/          # 테마 및 전역 스타일
│   └── types/           # TypeScript 타입 정의
├── ...                  # 기타 설정 파일 (eslint, jest, next, prettier, tailwind, tsconfig 등)
```

**주요 파일 및 모듈 설명:**

- **`src/app/(...)/page.tsx`**: 각 페이지의 진입점입니다.
- **`src/containers/(...)/(...).tsx`**: 각 페이지의 UI 및 로직을 구성하는 주요 컨테이너 컴포넌트입니다.
- **`src/components/data-grid.tsx`, `src/components/filter-grid.tsx`**: AG Grid를 활용한 데이터 그리드 컴포넌트입니다.
- **`src/actions/(...).ts`**: 서버와의 통신 및 데이터 변경 로직을 담당하는 서버 액션 파일들입니다.
- **`src/services/(...).service.ts`**: 각 데이터 모델에 대한 CRUD 및 관련 비즈니스 로직을 포함하는 서비스 파일들입니다.
- **`src/db/schema.ts`**: Drizzle ORM을 사용하여 데이터베이스 테이블 스키마를 정의합니다.
- **`.github/conventions.js.md`**: JavaScript/TypeScript 관련 코드 스타일 및 Git 컨벤션을 정의한 문서입니다.
- **`package.json`**: 프로젝트 의존성 및 스크립트를 관리합니다.

**코드 스타일 및 컨벤션:**

- **Package Manager**: pnpm
- **Code Style**: Prettier와 ESLint를 사용하여 코드 스타일을 일관되게 유지합니다.
  - Prettier 설정은 `.github/conventions.js.md` 문서 내에 명시되어 있습니다. (예: `printWidth: 80`, `singleQuote: false`, `semi: true`)
- **File Naming Convention**: `kebab-case` (예: `date-picker.js`)
- **Function/Variable Naming Convention**: `camelCase`
- **Component Naming Convention**: `PascalCase`
- **Directory Structure**: `src` 디렉토리 내에 기능별로 모듈을 구성하며, `app` 디렉토리 외부에 프로젝트 파일을 저장하는 방식을 따릅니다.
- **CSS**: TailwindCSS v4를 사용하며, `globals.css` 파일만을 사용합니다.
- **UI Components**: ShadCN 컴포넌트 및 Heroicons 아이콘을 사용합니다.
- **Pre-commit Hooks**: Husky와 lint-staged를 사용하여 커밋 전 코드 스타일 검사 및 수정을 자동화합니다. (`.pre-commit-config.yaml` 파일 참조)

## 5. 워크플로우 및 협업 프로세스

**Git 브랜치 전략:**

- GitFlow를 따릅니다.
- **브랜치 네이밍 형식**: `type/[branch/]description[-#issue]`
  - `type`: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore` 등
  - 예시: `feat/login-#123`, `fix/button-click-#456`

**코드 리뷰 프로세스:**

- Pull Request(PR) 생성 시 변경 사항에 대한 설명을 작성합니다. (`.github/PULL_REQUEST_TEMPLATE.md` 참조)
- PR 생성 시 자동으로 리뷰어가 할당될 수 있습니다. (`.github/auto_assign.yml`, `.github/workflows/auto-assign-reviewers.yml` 참조)
- 리뷰어는 코드 변경 사항을 검토하고 피드백을 제공합니다.
- 모든 피드백이 반영되고 테스트가 통과되면 PR이 병합됩니다.

**PR/이슈 관리 방식:**

- **PR 제목 형식**: `[Label]: [Title] [Issue Number]` (예: `feat: 채팅 기능 추가 #33`)
- **PR Checklist**: PR 생성 시 컨벤션 준수, 테스트 추가, 문서 업데이트 여부를 확인합니다.
- **이슈 템플릿**: 기능 추가, 버그 제보, 코드 리팩터링 등 목적에 맞는 이슈 템플릿을 사용합니다. (`.github/ISSUE_TEMPLATE` 폴더 참조)
- **이슈 레이블**: `feature`, `fix`, `documentation`, `style`, `refactor`, `test`, `chore` 등의 레이블을 사용하여 이슈를 분류합니다. (`.github/labels.json` 참조)
- PR 생성 시 자동으로 PR 생성자가 담당자로 할당됩니다. (`.github/workflows/auto-assign-pr-creator.yml` 참조)

## 6. 테스트 방법

**테스트 실행 방법:**

- **단위/통합 테스트:** Jest와 React Testing Library를 사용합니다. (`jest.config.ts`, `__tests__` 폴더 참조)
  - 테스트 실행 명령어: `pnpm test` 또는 `pnpm test:watch`
- **E2E 테스트:** Playwright를 사용합니다. (관련 설정은 `playwright.config.ts` 또는 유사 파일에서 찾을 수 있으나, 현재 제공된 파일 목록에는 명시적으로 보이지 않습니다. `package.json`에 `@playwright/test` 의존성이 존재합니다.)

## 7. 자주 묻는 질문 및 문제 해결 가이드

현재 제공된 파일 내에서 명시적인 FAQ 또는 문제 해결 가이드 문서는 확인되지 않았습니다. 프로젝트 진행 중 발생하는 문제나 궁금한 점은 다음 팀원에게 문의하거나 관련 문서를 참고해 주시기 바랍니다.

- **팀원 정보**: (프로젝트의 실제 팀 구성에 따라 추가 필요)
  - seungwonme
  - mgn1440
- **도움이 될 만한 문서**:
  - `.github/conventions.js.md`
  - `README.md`

## 8. 학습 로드맵 및 리소스

**인턴이 알아야 할 핵심 개념:**

- **웹 기본**: HTML, CSS, JavaScript (ES6+)
- **React**: 컴포넌트, Hook, 상태 관리, JSX
- **Next.js**: App Router, 서버 컴포넌트/클라이언트 컴포넌트, 서버 액션, API 라우트
- **TypeScript**: 기본 타입, 인터페이스, 제네릭
- **Git & GitHub**: 브랜치 전략(GitFlow), PR, 코드 리뷰
- **데이터베이스**: SQLite, Drizzle ORM 기본 사용법
- **UI 라이브러리**: Material-UI, TailwindCSS
- **상태 관리**: Jotai
- **테스팅**: Jest, React Testing Library 기본 사용법

**추천 학습 리소스:**

- **Next.js 공식 문서**: [Next.js Documentation](https://nextjs.org/docs)
- **React 공식 문서**: [React Documentation](https://react.dev/)
- **TypeScript 핸드북**: [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- **Drizzle ORM 문서**: [Drizzle ORM Documentation](https://orm.drizzle.team/docs/overview)
- **Material-UI 문서**: [MUI Documentation](https://mui.com/material-ui/getting-started/)
- **Tailwind CSS 문서**: [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- **AG Grid 문서**: [AG Grid Documentation](https://www.ag-grid.com/react-data-grid/)
- **GitFlow 워크플로우**: [Atlassian Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

이 문서는 시작을 위한 가이드이며, 프로젝트에 참여하면서 실제 코드와 문서를 통해 더 깊이 있는 지식을 습득해 나가시길 바랍니다. 궁금한 점이 있다면 언제든지 팀원들에게 질문해 주세요!

## Gompyo ERP 상세 코드 설명: 핵심 비즈니스 로직 및 데이터베이스

이 문서는 Gompyo ERP 프로젝트의 핵심 비즈니스 로직과 데이터베이스 구조에 대한 심층적인 설명을 제공하여 인턴 여러분의 코드 이해를 돕고자 합니다.

### 핵심 비즈니스 로직

프로젝트의 핵심 비즈니스 로직은 주로 `src/services` 디렉토리 내의 서비스 파일들과 `src/actions` 디렉토리 내의 서버 액션 파일들에 구현되어 있습니다. 이들은 데이터 처리, 계산, 외부 서비스 연동 등 Gompyo ERP의 주요 기능들을 담당합니다.

**1. 서비스 레이어 (`src/services`)**

서비스 레이어는 애플리케이션의 주요 비즈니스 로직을 캡슐화하고 데이터베이스와의 상호작용을 관리합니다. 각 서비스 파일은 특정 도메인 또는 기능에 대한 CRUD(Create, Read, Update, Delete) 작업 및 관련 로직을 담당합니다.

- **`cargo.service.ts`**: 화물(Cargos) 데이터에 대한 CRUD 작업을 제공합니다.
- **`contract.service.ts`**: 계약(Contracts) 정보와 관련된 CRUD 로직을 처리합니다.
- **`payment.service.ts`**: 결제(Payments) 정보 및 T/T, Usance 등 다양한 결제 방식에 따른 상세 정보를 생성하고 관리합니다.
- **`shipment.service.ts`**: 선적(Shipments) 정보의 생성, 조회, 수정, 삭제 로직을 담당하며, 선적 삭제 시 관련된 화물 정보도 함께 처리합니다.
- **`cost.service.ts`** 및 **`cost-detail.service.ts`**: 비용(Costs) 및 세부 비용(CostDetails) 정보를 관리합니다. `CostService`는 `CostDetailService`를 사용하여 비용 상세 정보도 함께 처리합니다.
- **`items.service.ts`** (`item.service.ts`도 유사): 품목(Items) 정보를 관리하며, 품목 정보 변경 시 기존 품목의 참조 여부에 따라 새로운 품목을 생성하거나 기존 품목을 삭제하는 로직을 포함합니다.
- **`importer.service.ts`**: 수입회사(Importers) 정보를 생성, 조회, 수정, 삭제하며, 수입회사 이름에 따른 계산 유형(CalculationType)을 매핑합니다.
- **`user.service.ts`**: 사용자 정보 조회 및 사용자별 화면 설정(컬럼 순서 등)을 관리합니다.
- **`document-service.ts`**: AWS S3를 이용한 문서 업로드, 다운로드 URL 생성, 삭제 및 데이터베이스 내 문서 메타데이터 관리를 담당합니다.
- **`history.service.ts`**: 데이터 변경 이력 및 상태 변경 이력을 기록하고 조회하는 기능을 제공합니다.
- **`account-number.service.ts`** 및 **`business-number-service.ts`**: 계좌번호 및 사업자번호 정보의 CRUD를 담당합니다.
- **`link.service.ts`**: 외부 링크 정보 (제목, URL, 썸네일, 순서)를 관리합니다.

**2. 계산 로직 (`src/services/cargo-calculator.ts`, `src/services/calculation-strategies.ts`)**

Gompyo ERP의 핵심 계산 로직은 `cargo-calculator.ts`와 `calculation-strategies.ts`에 집중되어 있습니다.

- **`cargo-calculator.ts`**: `CargoDetailData`를 입력받아 총 계약가, kg당 원가, 계약자 원가, 이익, 총비용 등 다양한 계산된 값을 포함하는 `CalculatedCargoDetailData`를 반환합니다. `CalculationStrategyFactory`를 통해 수입회사(Importer)의 `calculationType`에 따라 다른 계산 전략을 적용합니다.
- **`calculation-strategies.ts`**: `CalculationStrategy` 인터페이스와 이를 구현하는 다양한 전략 클래스들(`StandardCalculationStrategy`, `DnbCalculationStrategy`, `NamhaeCalculationStrategy`)을 정의합니다. 각 전략은 수입회사 유형별로 공급가, 계약자 이익, 총비용, 마진 등을 다르게 계산합니다.

**3. 서버 액션 (`src/actions`)**

Next.js 서버 액션은 클라이언트(UI)와 서버(서비스 레이어) 간의 통신을 담당합니다. 사용자의 요청에 따라 적절한 서비스를 호출하고 그 결과를 반환하거나 데이터를 변경합니다.

- **`plan.ts`**: 새로운 계획(계약, 선적, 화물, 비용, 결제 정보 등)을 생성하는 `createPlan` 함수와 전체 계획 데이터를 조회하는 `getPlanData` 함수를 제공합니다. `getPlanData`는 `mapAndCalculateCargoDetails`를 사용하여 각 화물에 대한 상세 계산 값을 포함하여 반환합니다.
- **`shipment.ts`**: 선적 관련 데이터를 조회하는 `getShipmentData` 함수를 제공하며, 이 또한 `mapAndCalculateCargoDetails`를 활용합니다.
- **`cashflow.ts`**: 수금 및 지출 내역(`cashflows`)과 회사 정보(`companies`)를 관리하는 액션들을 포함합니다. 잔액 업데이트, 우선순위 변경 등의 로직이 포함되어 있습니다.
- **`detail-view/` 디렉토리**: 상세 페이지와 관련된 액션들이 모여 있습니다.
  - `common.ts`: `getCargoDetail` (특정 화물의 모든 관련 정보 조회), `updateCargoDetail` (상세 정보 업데이트) 등 공통 액션을 제공합니다.
  - `cargo.ts`: 화물의 품목 정보, 포장 정보, 상태 정보 업데이트 로직을 담당합니다. 상태 변경 시 `history.service.ts`를 통해 로그를 기록합니다.
  - `entire.ts`: B/L 번호 변경 시 새로운 선적 정보를 생성하거나 기존 정보를 업데이트하는 로직 등을 포함합니다.
  - `history.ts`: 데이터 변경 이력 및 상태 변경 이력을 추가하고 조회하는 액션을 제공합니다.
- **`document-actions.ts`**: 문서 업로드(S3 presigned URL 사용), 삭제, 조회 등의 액션을 제공합니다.
- **`info/` 디렉토리**: 계좌번호, 사업자번호, 링크 정보 관리 액션들이 위치합니다.

### 데이터베이스 (DB)

Gompyo ERP는 SQLite 데이터베이스를 사용하며, Drizzle ORM을 통해 데이터베이스 스키마를 정의하고 상호작용합니다.

**1. 스키마 정의 (`src/db/schema.ts`)**

`src/db/schema.ts` 파일은 Drizzle ORM을 사용하여 각 테이블의 구조(컬럼, 타입, 제약 조건 등)와 테이블 간의 관계를 정의합니다. 주요 테이블과 그 목적은 다음과 같습니다:

- **`importers`**: 수입회사 정보 (ID, 이름, 계산 유형)
- **`contracts`**: 계약 정보 (ID, 계약 번호, 날짜, 수출자, 수입회사 ID, 인코텀즈 등)
- **`items`**: 품목 정보 (ID, 품목명, 품종, 원산지, HS 코드, 포장 단위)
- **`shipments`**: 선적 정보 (ID, 계약 ID, 도착/출발 예정 시간, 항구, 선사, B/L 번호 등)
- **`cargos`**: 화물 정보 (ID, 품목 ID, 선적 ID, 컨테이너 수, 계약 톤수, 통관/검역/입고일, 진행 상태, 판매가, 마진, 이익 등)
- **`payments`**: 기본 결제 정보 (ID, 결제 만기일, 결제 방식, 계약 ID)
- **`paymentsTt`**: T/T 결제 상세 정보 (선급금/잔금 날짜, 비율, 상대 은행 등)
- **`paymentsUsance`**: Usance 결제 상세 정보 (결제 기간, 계약 환율 등)
- **`costs`**: 기본 비용 정보 (ID, 공급가, 운송비, 인건비, 운송/보관료, 상하차비, Usance 이자, 화물 ID)
- **`costDetails`**: 세부 비용 정보 (ID, 단가, 환율, 관세율, 관세 수수료, 검사료, D/O Charge, 기타 비용, 송금 수수료, 비용 ID)
- **`users`**: 사용자 정보 (ID, 이메일, 비밀번호, 이름, 화면 컬럼 순서 설정)
- **`documents`**: 업로드된 문서 메타데이터 (ID, 문서명, 타입, S3 URL, 업로드 날짜, 관련 ID, 카테고리)
- **`historyLogs`**: 데이터 변경 및 상태 변경 이력 (ID, 대상 타입/ID, 변경 유형, 사용자, 변경 내용, 상태, 생성일)
- **`accountNumbers`**: 계좌번호 정보
- **`businessNumbers`**: 사업자번호 정보
- **`links`**: 외부 링크 정보
- **`cashflows`**: 현금 흐름(수금/지출) 내역
- **`companies`**: 현금 흐름 관리를 위한 회사 정보

또한, `relations` 함수를 사용하여 테이블 간의 관계(예: 계약-결제, 선적-화물, 화물-비용 등)를 정의하고 있습니다.

**2. 데이터베이스 연결 및 설정**

- **`src/db/index.ts`**: `better-sqlite3`를 사용하여 SQLite 데이터베이스에 연결하고, Drizzle ORM 인스턴스를 생성합니다. 데이터베이스 파일 경로는 `drizzle/database.sqlite` 입니다.
- **`drizzle.config.ts`**: Drizzle Kit 설정을 정의하며, 스키마 파일 위치, 출력 디렉토리, 데이터베이스 연결 정보 등을 명시합니다.

**3. 마이그레이션 및 시딩**

- **마이그레이션**: Drizzle Kit을 사용하여 데이터베이스 스키마 변경사항을 관리합니다. 마이그레이션 파일들은 `drizzle/` 디렉토리에 SQL 형태로 저장됩니다. (`0000_...sql`, `0001_...sql` 등)
  - `pnpm db:migrate` 또는 `pnpm drizzle:push` (스키마 직접 푸시) 명령어를 통해 마이그레이션을 실행할 수 있습니다.
- **시딩**: `src/db/runSeed.ts` 파일은 초기 데이터를 데이터베이스에 주입하는 역할을 합니다. `src/db/data/` 디렉토리 내의 시드 데이터 파일들을 참조하여 각 테이블에 데이터를 삽입합니다.
  - `pnpm db:seed` 명령어로 시드 데이터를 주입합니다.

이 설명을 통해 Gompyo ERP의 주요 로직 흐름과 데이터 구조를 이해하는 데 도움이 되기를 바랍니다. 코드를 직접 살펴보면서 각 모듈과 함수가 어떻게 상호작용하는지 파악하면 더욱 깊이 있는 이해가 가능할 것입니다.
