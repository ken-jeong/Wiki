---
aliases: []
type: Lecture
tags:
draft: false
date: 2025-07-04
---
* [github-actions](https://jyheo.github.io/github-lecture/github-actions.html)

## 1. GitHub Actions 기본 개념
- **GitHub Actions란?**
	- GitHub에서 제공하는 CI/CD 플랫폼
	- 빌드, 테스트, 배포를 자동화하는 workflow 생성
	- 다양한 운영체제 지원 (Linux, Windows, macOS)

### 1.1. 핵심 구성 요소
**1) Workflows**
- YAML 형식으로 작성
- `.github/workflows` 디렉터리에 저장
- 이벤트 발생 시 자동 실행 또는 스케줄/수동 실행
- 여러 workflow 동시 관리 가능

**2) Events**
- Workflow를 시작하는 트리거
- 예: push, pull request, issue 생성
- 스케줄, REST API, 수동 트리거도 가능

**3) Jobs**
- 동일한 runner에서 실행되는 step들의 집합
- 순차 또는 병렬 실행 가능
- 데이터 공유 가능

**4) Actions**
- 복잡하지만 자주 사용되는 작업을 수행하는 커스텀 응용
- GitHub Marketplace에서 검색 및 사용 가능
- 직접 제작도 가능

**5) Runners**
- Workflow를 실행하는 서버
- GitHub 제공 가상 머신 또는 자체 호스팅 시스템 사용
- 각 runner는 한 번에 하나의 job 실행

## 2. 간단한 Workflow 예제
- **learn-github-actions.yml**
```yaml
name: learn-github-actions
run-name: ${{ github.actor }} is learning GitHub Actions
on: [push]
jobs:
  check-bats-version:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install -g bats
      - run: bats -v
```

- **주요 키워드**
	- **name**: workflow 이름
	- **run-name**: 실행 중인 workflow 이름
	- **on**: 트리거 이벤트
	- **jobs**: job 정의
	- **runs-on**: runner 지정
	- **steps**: 작업 단계
	- **uses**: 사전 정의된 액션 사용
	- **run**: 명령어 수행

## 3. Starter Workflow
- **C/C++ with Make 템플릿 예제**
```yml
name: C/C++ CI
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: make
      run: make
    - name: make check
      run: make check
```

## 4. 고급 기능
### 4.1. Uses Actions
- **GitHub Marketplace**: `{owner}/{repo}@{ref}` 형식
- **로컬 action**: `./.github/actions/hello-action`
- **Docker Hub**: 공개 도커 이미지

### 4.2. Artifacts (파일 공유)
```yml
jobs:
  upload-job:
    steps:
      - run: expr 1 + 1 > output.log
      - uses: actions/upload-artifact@v4
        with:
          name: output-log-file
          path: output.log
  download-job:
    needs: upload-job
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: output-log-file
      - run: cat output.log
```

### 4.3. 환경 변수
```yml
env:
  DAY_OF_WEEK: Monday    # workflow 수준
jobs:
  example-job:
    env:
      Greeting: Hello    # job 수준
    steps:
      - run: echo "$Greeting"
        env:
          First_Name: Mona  # step 수준
```

### 4.4. Expression
- 문법: `${{ expression }}`
- 조건부 실행, 변수 지정에 사용

```yml
- run: node client.js
  if: ${{ success() }}
  env:
    MY_ENV_VAR: ${{ 1+1 }}
```

### 4.5. Context
- **github**: workflow 실행 정보
    - `github.actor`: 실행자
    - `github.ref`: 브랜치/태그
- **env**: 환경 변수
- **job**: 현재 job 정보
- **steps**: step 정보
- **runner**: runner 정보 (OS 등)

## 5. Workflow 트리거 상세
- **기본 트리거**
	- `on: push`
	- `on: pull_request`
	- `on: workflow_dispatch` (수동)
	- `on: workflow_call` (다른 workflow에서 호출)
	- `on: [push, pull_request]` (복수)

- **조건부 트리거**
```yml
on:
  issues:
    types:
      - opened
      - labeled
  push:
    branches:
      - main
      - 'releases/**'
      - '!releases/**-alpha'
```

## 6. Reusable Workflow
- **reusable-workflow.yml** (재사용 가능한 workflow)
```yml
on:
  workflow_call:
    inputs:
      input1:
        required: true
        type: string
    outputs:
      output1:
        value: ${{ jobs.example_job.outputs.output1 }}
jobs:
  example_job:
    outputs:
      output1: ${{ steps.step1.outputs.result }}
    steps:
    - id: step1
      run: echo "result=`expr ${{ inputs.input1 }} + 10`" >> $GITHUB_OUTPUT
```

- **호출 방법**
```yml
jobs:
  job1:
    uses: jyheo/test3/.github/workflows/reusable-workflow.yml@main
    with:
      input1: 10
  job2:
    needs: job1
    steps:
      - run: echo ${{ needs.job1.outputs.output1 }}
```

## 7. Cache Dependency
- **기본 캐싱**
```yml
- uses: actions/cache@v3
  with:
    path: |
      ~/.gradle/caches
      ~/.gradle/wrapper
    key: ${{ runner.os }}-build-${{ hashFiles('a-file') }}
```

- **setup 액션 활용**
	- **Python**: `setup-python` with `cache: 'pip'`
	- **Node**: `setup-node` with `cache: 'npm'`
	- **Java**: `setup-java` with `cache: 'gradle'`

## 8. Jobs 활용
- **순차 실행**
```yml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
```

- **조건부 실행**
```yml
jobs:
  production-deploy:
    if: github.repository == 'octo-org/octo-repo-prod'
```

- **Matrix 전략**
```yml
jobs:
  example_matrix:
    strategy:
      matrix:
        os: [ubuntu-22.04, ubuntu-20.04]
        version: [10, 12, 14]
    runs-on: ${{ matrix.os }}
```

## 9. 실습 예제
- **Python 프로젝트**
	- `str_util.py`, `test_str_util.py` 작성
	- `requirements.txt`에 numpy 추가
	- Python Application starter workflow 사용
	- pip 캐싱으로 실행 시간 단축
