# TIL: Terraform

## IaC (Infrastructure as Code)

* **정의**: 인프라 구성을 코드로 관리하는 방식
* **장점**

  * 버전 관리(Git 등) 가능
  * 인프라 환경을 일관되게 재현 가능
  * 자동화/반복 작업 최적화
  * 협업 및 리뷰 프로세스 적용 용이

---

## Terraform 개요

* **HashiCorp**에서 만든 오픈소스 IaC 도구
* AWS, GCP, Azure 등 **멀티 클라우드 지원**
* 선언적 방식(Desired State 기반)으로 인프라를 정의

---

## Terraform 주요 용어

* **Provider**

  * Terraform과 클라우드 서비스(API) 간 연결 역할
  * 예: `aws`, `google`, `azurerm`

* **Resource**

  * 실제로 생성/관리되는 인프라 객체
  * 예: EC2 인스턴스, S3 버킷

* **State**

  * Terraform이 관리하는 인프라의 현재 상태를 기록하는 파일 (`terraform.tfstate`)
  * 선언된 코드와 실제 인프라 상태를 동기화

---

## Terraform 주요 명령어

* `terraform init` : 프로젝트 초기화 (Provider 플러그인 설치)
* `terraform plan` : 실행 계획 미리보기 (무엇이 생성/변경/삭제되는지)
* `terraform apply` : 실제 인프라 반영
* `terraform destroy` : 생성한 인프라 삭제

---

## HCL (HashiCorp Configuration Language)

* **특징**

  * JSON과 유사한 구조적 문법
  * 선언적 방식으로 리소스를 정의

* **구조**

  * **Block** : 가장 기본 단위 (예: `resource`, `provider`)
  * **Argument** : 속성 값 정의 (key = value 형태)
  * **Expression** : 동적 값 할당 가능 (변수, 함수 등 사용)

* **주요 함수**

  * `for_each` : 반복 리소스 생성
  * `lookup` : 맵(Map)에서 키로 값 조회
  * `file` : 파일 내용 읽기
  * `templatefile` : 템플릿 파일을 불러와 변수 적용

---

## 핵심 정리

* Terraform = **IaC 구현 도구**
* Provider/Resource/State 세 가지 개념을 확실히 이해해야 함
* 기본 명령어(`init`, `plan`, `apply`, `destroy`) 숙지 필수
* HCL 문법은 간결하고 직관적 → 실제 인프라 코드를 선언적으로 표현 가능
