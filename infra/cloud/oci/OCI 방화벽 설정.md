### 🔥 방화벽 설정 관리

- **방화벽 해제 (포트 개방)**  
  - 특정 포트(예: 8000번)를 외부에서 접근할 수 있도록 허용할 수 있음.  
  - 아래 명령어로 8000번 포트를 열어 웹 서비스나 API가 외부에서 접근할 수 있도록 할 수 있음.
  ```bash
  sudo ufw allow 8000/tcp
  ```
  - 🔑 **Tip:** `tcp` 대신 `udp`를 열어야 하는 경우에는 `8000/udp`로 지정할 수 있음.

- **방화벽 상태 확인**  
  - 방화벽이 현재 어떤 규칙으로 설정돼 있는지 확인할 수 있음.  
  - 아래 명령어로 허용된 포트 목록과 기본 정책을 확인할 수 있음.
  ```bash
  sudo ufw status
  ```
  - 출력 결과 예시:  
    ```
    Status: active
    To                         Action      From
    --                         ------      ----
    8000/tcp                   ALLOW       Anywhere
    22/tcp                     ALLOW       Anywhere
    ```

---

💡 **추가 Tip**  
✅ `sudo ufw enable` : UFW 방화벽을 활성화할 수 있음.  
✅ `sudo ufw disable` : UFW 방화벽을 비활성화할 수 있음.  
✅ `sudo ufw delete allow 8000/tcp` : 허용된 8000 포트 규칙을 삭제할 수 있음.  
✅ `sudo ufw allow from <IP주소>/24` : 특정 IP 범위만 허용할 수 있음.
