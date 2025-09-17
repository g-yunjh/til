## AWS 개요

* **클라우드 컴퓨팅 플랫폼**

  * 세계 3대 클라우드 서비스: **AWS, Microsoft Azure, Google Cloud Platform**
  * AWS는 가장 오래되고 사용자가 많음 → 타 클라우드 서비스 학습에도 도움이 됨
* **특징**

  * 다양한 서비스 제공 (컴퓨팅, 스토리지, 네트워크, 보안, AI 등)
  * 필요에 따라 확장/축소 가능 (온디맨드, 스케일링)
  * 사용량 기반 과금 (Pay-as-you-go)
* **회원가입 시 주의사항**

  * **MFA(Multi-Factor Authentication)** 설정 필수 (보안 위협 방지)
  * 계정 해킹 시 과금 피해 위험 → secret, token 노출 주의 (GitHub 업로드 금지)
* **Free Tier**

  * 신규 계정은 12개월 동안 제한된 리소스를 무료 사용 가능
  * 예: `t2.micro` 인스턴스 750시간/월

---

## Amazon EC2 (Elastic Compute Cloud)

* **개념**

  * AWS에서 제공하는 **가상 머신 서비스**
  * 빠르게 인스턴스를 생성/삭제/관리 가능
* **주요 개념**

  * **Instance Type** : CPU, 메모리, 네트워크 성능에 따라 다양한 사양 제공
  * **구매 옵션** : 온디맨드, 예약 인스턴스, 스팟 인스턴스 등
  * **Life Cycle** : pending → running → stopped/terminated
* **EC2 인스턴스 생성**

  * Free Tier에서는 `t2.micro` 권장
  * Security Group 설정 → 포트 제어 (예: SSH 22번 포트 열기)
* **접속 방법**

  * **Instance Connect** : AWS 콘솔에서 브라우저 기반 SSH 접속
  * **SSH** : 로컬 터미널에서 key pair를 이용해 접속
* **Elastic IP (EIP)**

  * 고정 IP 주소를 인스턴스에 할당 가능
  * 인스턴스를 껐다 켜도 IP 유지
  * 단, 과금 정책 존재 (사용하지 않고 할당만 하면 비용 발생)

---

## AWS Network

* **Region & Availability Zone (AZ)**

  * **Region** : 물리적으로 분리된 AWS 데이터센터 위치 (예: 서울, 도쿄, 버지니아)
  * **Availability Zone** : Region 내에서 독립적인 데이터센터 단위 → 장애 대비
  * 다중 AZ 배포를 통해 가용성과 내결함성 확보
* **VPC (Virtual Private Cloud)**

  * 사용자가 직접 구성 가능한 가상 네트워크
  * 기본 개념:

    * **Subnet** : VPC 내부 네트워크 범위 (Public / Private)
    * **Route Table** : 트래픽 라우팅 규칙 정의
    * **Internet Gateway** : VPC와 인터넷을 연결
  * VPC를 통해 보안적으로 격리된 네트워크 환경 구성 가능

---

## 참고 개념

* VPC는 AWS 네트워크 구조의 핵심 → 익숙해지면 로드밸런서, NAT Gateway, Peering, VPN 등으로 확장 가능
* 인스턴스 타입 비교 사이트: [instances.vantage.sh](https://instances.vantage.sh)
