---

## 💻 서버 접속 및 Docker 컨테이너 관리

### 🌐 1. 서버 접속
- **SSH 접속**  
  - 아래 명령어로 SSH 키를 사용해 서버에 접속할 수 있음.
  ```bash
  ssh -i ~/keys/ssh-key-2025-03-09.key ubuntu@158.179.162.221
  ```
  - 🔑 **Tip:** SSH 키 파일의 퍼미션(권한)을 `chmod 400`으로 제한해 접속 오류를 방지할 수 있음.

---

### 🐳 2. Docker 서비스 관리
- **Docker 서비스 시작**
  ```bash
  sudo systemctl start docker
  ```
  - Docker 데몬을 수동으로 시작할 수 있음.

- **부팅 시 자동 시작 설정**
  ```bash
  sudo systemctl enable docker
  ```
  - 서버를 재부팅해도 Docker가 자동으로 시작되도록 설정할 수 있음.

- **Docker 상태 확인**
  ```bash
  sudo systemctl status docker
  ```
  - Docker 서비스가 실행 중인지 확인하고, 서비스 상태나 로그도 볼 수 있음.

---

### 📦 3. Docker 컨테이너 관리

- **컨테이너를 백그라운드에서 실행**  
  - 예: `videoapi-app`이라는 이미지를 기반으로, 컨테이너 이름을 `videoapi`로 지정해 8000번 포트를 호스트와 연결할 수 있음.
  ```bash
  sudo docker run -d -p 8000:8000 --name videoapi videoapi-app
  ```
  - 옵션 설명:
    - `-d` : 백그라운드 모드로 실행할 수 있음  
    - `-p 8000:8000` : 호스트 포트 8000을 컨테이너의 8000 포트와 연결할 수 있음  
    - `--name videoapi` : 컨테이너 이름을 지정할 수 있음

- **실행 중인 컨테이너 목록 확인**
  ```bash
  sudo docker ps
  ```
  - 현재 실행 중인 컨테이너를 확인할 수 있음.

- **컨테이너 로그 확인 (실시간)**
  ```bash
  sudo docker logs -f videoapi
  ```
  - `-f` 옵션으로 로그를 실시간으로 스트리밍해서 확인할 수 있음.

- **컨테이너 중지**
  ```bash
  sudo docker stop videoapi
  ```
  - `videoapi` 컨테이너를 중지할 수 있음.

---

### 🛑 4. Docker 서비스 전체 중단
- **Docker 자체를 끄는 명령어**
  ```bash
  sudo systemctl stop docker
  ```
  - Docker 서비스를 완전히 중단시켜 컨테이너도 같이 멈출 수 있음.

---

### 💡 추가 정보보
✅ `sudo systemctl restart docker` : Docker 서비스를 재시작할 수 있음.  
✅ `sudo docker rm <컨테이너 이름>` : 중지된 컨테이너를 삭제할 수 있음.  
✅ `sudo docker images` : 다운로드된 이미지 목록을 확인할 수 있음.

---

📌 **정리**  
1️⃣ 서버 접속 후 `sudo systemctl start docker`로 Docker 서비스를 시작할 수 있음.  
2️⃣ `sudo docker run -d ...`로 컨테이너를 실행할 수 있음.  
3️⃣ `sudo docker logs -f ...`로 상태를 모니터링할 수 있음.  
4️⃣ 필요 시 `sudo docker stop ...`으로 컨테이너를 중지할 수 있음.  
5️⃣ `sudo systemctl stop docker`로 Docker 전체 서비스를 종료할 수 있음.
