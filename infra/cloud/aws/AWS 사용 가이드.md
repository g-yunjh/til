# AWS 사용 시 필수 체크리스트

아래 내용은 AWS 사용 시 **보안**, **비용 관리**, **계정 관리**, **접근 제어** 등에서 반드시 알아야 할 핵심 사항과 **실무 절차**, **예시 명령어**, **실제 사고 사례**를 포함한 정리본입니다.

---

## 1. 계정 및 IAM 관리

### 1.1 Root 계정 사용 최소화

* Root 계정은 가입 시 생성되는 **마스터 권한** 계정이며, 모든 리소스에 대한 무제한 접근 권한을 가짐.
* 운영 및 개발 시에는 **IAM 사용자**를 생성하여 사용.
* Root 계정은 다음 경우에만 사용:

  * 계정 설정 변경 (예: 결제 정보 수정)
  * 서비스 제한 해제 요청
* **Root 계정 비밀번호는 오프라인 보관** + 절대 공유 금지.

### 1.2 MFA(다중 인증) 설정

* MFA 설정 경로:

  1. AWS 콘솔 로그인 → `IAM` → `사용자` 선택
  2. `보안 자격 증명` 탭 → `MFA 디바이스 할당`
  3. 가상 MFA 앱(예: Google Authenticator, Authy) 연결
* MFA 설정 시 **QR 코드 스캔** 또는 **키 수동 입력**.
* MFA 복구 코드는 별도 보관(휴대폰 분실 대비).

### 1.3 IAM 사용자 및 권한 관리

* 각 작업자별 개별 IAM 사용자 계정 생성 (공유 계정 금지).
* **정책(Policy)** 최소 권한 원칙 적용:

  * 예: EC2 인스턴스 관리만 필요한 사용자는 `AmazonEC2FullAccess` 대신 **커스텀 정책**으로 범위 제한.
* IAM 그룹 활용:

  * 개발자 그룹, 운영자 그룹, 관리자 그룹 등 권한을 묶어서 관리.

---

## 2. 보안 그룹 및 네트워크 설정

### 2.1 SSH/RDP 접근 제한

* **절대 금지**: 인바운드 규칙에서 `0.0.0.0/0`로 22(SSH), 3389(RDP) 허용.
* 고정 IP 환경:

  ```plaintext
  예) SSH 허용 IP → 203.0.113.25/32
  ```
* 유동 IP 환경:

  * VPN 또는 Bastion Host(점프 서버) 사용.
  * 접속 전 방화벽 규칙을 동적으로 업데이트.

### 2.2 Bastion Host 운영

* Bastion Host는 관리용 인스턴스로만 사용하며, 외부에서 직접 서비스 서버로 접근 불가.
* Bastion Host 접근도 MFA 적용 가능.
* SSH 설정 예시 (`~/.ssh/config`):

  ```plaintext
  Host bastion
    HostName bastion.example.com
    User ec2-user
    IdentityFile ~/.ssh/bastion.pem

  Host app-server
    HostName 10.0.1.10
    User ec2-user
    IdentityFile ~/.ssh/app.pem
    ProxyJump bastion
  ```

---

## 3. 키 페어 및 접속 관리

### 3.1 키 페어 다운로드 및 보관

* 인스턴스 생성 시 `.pem` 키 파일은 **발급 시 1회만 다운로드 가능**.
* 백업:

  * 로컬 저장소(암호화 폴더)
  * 비상 복구용 외장 드라이브
* 권한 설정(리눅스):

  ```bash
  chmod 400 my-key.pem
  ```

### 3.2 Windows → Linux 접속

* PuTTY 사용 시 `.pem` → `.ppk` 변환 필요:

  * PuTTYgen → `Load` → All Files → `.pem` 선택 → `Save private key`.
* 접속 예시:

  ```plaintext
  Host: ec2-1-2-3-4.compute.amazonaws.com
  User: ubuntu
  Auth: my-key.ppk
  ```

### 3.3 계정 비밀번호 초기 설정

* 루트 계정 활성화:

  ```bash
  sudo passwd root
  ```
* 루트 계정 로그인:

  ```bash
  su
  ```

---

## 4. 인스턴스 및 리소스 관리

### 4.1 OS 선택 및 비용

* **Ubuntu** 권장 (윈도우는 라이선스 비용 포함으로 2배 이상 비쌈).
* 예시:

  * `c5.xlarge` (4 vCPU, 8GB RAM)

    * Ubuntu: \$140.55/월
    * Windows: \$275.24/월

### 4.2 스토리지 설정

* 운영 안정성을 위해 최소 50GB EBS 할당.
* EBS 타입: gp3 권장 (IOPS/Throughput 조정 가능).

### 4.3 GPU/Baremetal 사용 제한

* Quota(서비스 한도)에서 GPU 및 Baremetal 인스턴스 차단:

  * AWS Support Center → Service Quotas → EC2 → GPU 인스턴스 요청 0으로 설정.

---

## 5. 비용 및 모니터링

### 5.1 Budget 알림 설정

* 경로: `AWS Budgets` → `Create budget` → 예산 설정 → 알림 조건 지정.
* SMS 알림:

  * AWS SNS에서 주제(Topic) 생성 후, Budget과 연동.

### 5.2 비용 이상 감지

* AWS Cost Anomaly Detection 활성화:

  * 서비스별, 리전별 비용 급증 감지.
* 예: GPU 인스턴스가 주말에 생성되는 경우 즉시 알림.

### 5.3 리소스 점검 루틴

* 매주:

  * 모든 리전 인스턴스 상태 확인.
  * 미사용 Elastic IP, EBS, 스냅샷 제거.
* 매월:

  * 비용 보고서 검토.

---

## 6. 사고 대응 절차 (실제 사례 기반)

1. **액세스 키 유출**

   * 알림 메일 수신 → 키 즉시 비활성화/삭제.
   * Root 계정 비밀번호 변경.
   * 관련 로그 확인(CloudTrail).
2. **비용 이상 감지**

   * 원인: 모든 리전에 무단 인스턴스 생성.
   * 조치: 모든 리전 인스턴스 삭제, 권한 회수.
3. **GPU 서버로 인한 비용 폭증**

   * 연휴 주말에 생성 → 비용 초과.
   * 사전 차단: Quota로 GPU 요청 불가 설정.
4. **EC2 Abuse 신고**

   * AWS로부터 공격 경고 메일 수신.
   * 해당 인스턴스 격리 및 보안 그룹 차단.
   * 조치 후 AWS에 회신.

---

## 7. 추가 보안 팁

* CloudTrail 활성화 → 모든 API 호출 로그 저장.
* S3 버킷 공개 여부 수시 점검 (Block Public Access).
* IAM 정책 최소 권한 원칙(Least Privilege).
* 환경별 VPC/Subnet 분리로 보안 경계 강화.
