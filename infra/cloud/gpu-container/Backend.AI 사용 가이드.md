# Backend.AI 사용 가이드

## 1. 접속 방법
- **웹 기반 접속**
  - 브라우저에서 `https://siriuscluster.skku.edu` 접속
  - 이메일/비밀번호 입력 후 로그인

- **데스크탑 기반 접속**
  - [Backend.AI Desktop 릴리즈 페이지](https://github.com/lablup/backend.ai-webui/releases)에서 OS에 맞는 버전 다운로드 후 설치
  - 실행 후 엔드포인트(`https://siriuscluster.skku.edu`)와 계정 정보 입력

- **차이점**
  - 웹 버전: 기본 기능 제공
  - 데스크탑 버전: SSH / SFTP 지원 (파일 교환, 원격 접속 가능)

---

## 2. UI 주요 기능
- **데이터 & 폴더**
  - 데이터셋/폴더 관리
  - `+ 새폴더` 버튼으로 새 폴더 생성 가능

- **세션(Session)**
  - 연산 세션 생성 및 관리
  - 환경 선택(Python, TensorFlow 등), CPU/GPU/메모리 자원 할당 후 실행

- **Console**
  - 명령어 실행 탭 (코드/스크립트 테스트 가능)

- **App / Utilities**
  - Backend.AI에서 제공하는 다양한 유틸리티 실행

---

## 3. 연산 세션 생성
1. 로그인 후 `세션` 탭 진입
2. 런타임 환경 선택 (예: Python, TensorFlow, PyTorch 등)
3. 자원 설정 (CPU, GPU, 메모리 등)
4. 세션 실행 버튼 클릭 → 몇 초 후 사용 가능
5. 실행된 세션에서:
   - Python 코드 실행
   - Jupyter Notebook 환경 활용
   - 데이터 불러오기 및 분석 수행 가능

---

## 4. SSH / SFTP 활용
- **SSH**
  - 터미널로 원격 서버 접속
  - 서버 내부 디렉토리 접근 및 제어 가능

- **SFTP**
  - FileZilla, WinSCP 등 클라이언트를 이용해 로컬 ↔ 원격 간 파일 업/다운로드
  - 데이터 전처리 파일 업로드, 분석 결과 다운로드에 활용

---

## 5. 핵심 포인트 정리
- 접속: `웹`(간단) vs `데스크탑`(SSH/SFTP 지원)
- UI: 데이터 관리, 세션 관리, 콘솔, 유틸리티
- 세션: 환경 선택 → 자원 할당 → 실행
- 확장: SSH/SFTP를 통한 원격 접속 및 파일 교환 가능
