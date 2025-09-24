# TIL: AWS - S3, CloudFront, IAM

## Amazon S3 (Simple Storage Service)

* **특징**

  * 객체 스토리지(Object Storage) 서비스
  * 정적 웹 사이트 파일(HTML, CSS, JS)부터 대규모 데이터 저장까지 가능
  * 고가용성, 확장성, 내구성(99.999999999%) 제공

* **주요 용어**

  * **Bucket** : S3의 최상위 컨테이너, 파일을 저장하는 공간
  * **Object** : S3에 저장되는 실제 데이터 단위
  * **Key** : 객체(Object)의 고유 식별자 (경로 같은 역할)
  * **ACL (Access Control List)** : 버킷/객체 접근 권한 제어

* **기능**

  * **버저닝(Versioning)** : 동일한 파일의 여러 버전을 관리
  * **정적 웹 사이트 호스팅** 지원
  * **수명 주기(Lifecycle Policy)** : 오래된 객체를 Glacier 등 저비용 스토리지로 이동

* **요금 체계**

  * 스토리지 사용량(GB/월)
  * 요청 횟수(GET, PUT 등)
  * 데이터 전송량(특히 리전 외부 전송 시 비용 발생)

---

## Amazon CloudFront

* **특징**

  * AWS에서 제공하는 **CDN(Content Delivery Network)** 서비스
  * 전 세계 엣지 로케이션(Edge Location)을 통해 캐싱 → 사용자와 가까운 서버에서 응답
  * 정적 파일 전송 속도를 크게 개선, 대규모 트래픽에도 안정적

* **주요 용어**

  * **Distribution** : CloudFront에서 콘텐츠를 배포하는 단위
  * **Origin** : 원본 서버 (예: S3 버킷, EC2, ALB 등)
  * **TTL (Time to Live)** : 캐시 유지 시간
  * **Invalidation** : 캐시 무효화 요청 (즉시 갱신 필요할 때 사용)

* **요금 체계**

  * 데이터 전송량
  * 요청 횟수
  * Invalidation 요청(무료 한도 초과 시 과금)

---

## AWS IAM (Identity and Access Management)

* **특징**

  * AWS 계정의 **사용자, 그룹, 권한**을 관리하는 서비스
  * 보안 강화 및 최소 권한 원칙(Least Privilege) 적용에 필수

* **주요 용어**

  * **User** : IAM 사용자 (개인 계정 단위)
  * **Group** : 여러 User를 묶어 공통 권한 부여
  * **Role** : 일시적 권한을 부여하는 AWS 자격 (EC2나 Lambda에 많이 사용)
  * **Policy** : JSON 문법 기반 권한 규칙
  * **Permission** : 특정 리소스에 접근할 수 있는 권한

* **Policy 문법 (JSON)**

  * **Statement** : 정책 단위
  * **Effect** : 허용(Allow) / 거부(Deny)
  * **Action** : 수행 가능한 작업(API 호출 등)
  * **Resource** : 적용되는 리소스(ARN 기반)

* **정책 유형**

  * **Identity-based Policy** : User, Group, Role에 부여
  * **Resource-based Policy** : 특정 리소스(S3 버킷 등)에 직접 부여

* **Best Practice**

  * **Root 계정은 사용하지 않고 IAM User를 활용**
  * MFA 활성화
  * 최소 권한 원칙 준수 (필요한 권한만 부여)
