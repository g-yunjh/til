# TIL: GitHub Actions

## CI/CD 개요

* **CI (Continuous Integration)**

  * 여러 개발자가 작업한 코드를 **지속적으로 통합**
  * merge 시 자동으로 빌드·테스트 실행 → 코드 품질 보장

* **CD (Continuous Delivery / Deployment)**

  * 테스트 통과 후, 자동으로 배포 환경까지 반영
  * 오류를 조기에 발견하고 배포 속도를 높여줌

---

## GitHub Actions 개요

* GitHub에서 제공하는 **CI/CD 도구**
* **YAML 파일**로 워크플로우를 정의하고, 특정 이벤트 발생 시 자동 실행
* GitHub 저장소와 긴밀히 통합 → 별도 외부 서비스 없이 간단하게 설정 가능

---

## 주요 구성 요소

* **Workflow**

  * YAML 파일 단위의 자동화 프로세스
  * `.github/workflows/` 경로에 정의

* **Event**

  * 워크플로우 실행을 트리거하는 이벤트
  * 예: `push`, `pull_request`, `schedule`, `workflow_dispatch`

* **Job**

  * 하나의 워크플로우 안에서 실행되는 작업 단위
  * 병렬/순차 실행 가능, 각 job은 자체 가상 환경에서 실행됨

* **Action**

  * Job 안에서 실행되는 실제 단일 작업
  * GitHub Marketplace에서 제공되는 액션 재사용 가능 (예: checkout, setup-node 등)

---

## GitHub Actions 문법

* **기본 구조**

```yaml
name: CI
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run tests
        run: npm test
```

* **핵심 키워드**

  * `name`: 워크플로우 이름
  * `on`: 트리거 이벤트
  * `jobs`: 실행할 작업들
  * `runs-on`: 실행 환경 (예: `ubuntu-latest`, `windows-latest`)
  * `steps`: 각 작업 단계 (명령 실행 or 액션 호출)

---

## Secrets 관리

* 민감한 정보(API 키, 토큰, 비밀번호 등)는 **GitHub Secrets**에 저장
* 워크플로우 안에서 환경 변수처럼 사용 가능

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

---

## 보안 & 확장

* **CodeQL**: GitHub Actions에서 제공하는 정적 분석 도구 → 보안 취약점 탐지
* **Marketplace**: 오픈소스/공식 액션을 가져다 써서 빠르게 CI/CD 파이프라인 구성 가능

---

## 핵심 정리

* CI/CD = 자동화된 빌드, 테스트, 배포
* GitHub Actions = GitHub에 최적화된 CI/CD 도구 (YAML 기반)
* 구성 요소: Workflow → Event → Job → Action
* 실무에서는 Secret 관리와 보안 점검(CodeQL 등) 활용이 중요
