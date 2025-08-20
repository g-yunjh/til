# 파이썬으로 큐 구현하기 (BOJ 10845번 문제)

## 문제 요약

* **명령어 종류 (6가지)**

  * `push X`: 큐에 정수 X 삽입
  * `pop`: 큐에서 가장 앞에 있는 정수 출력 후 제거 (없으면 -1)
  * `size`: 큐 크기 출력
  * `empty`: 비어있으면 1, 아니면 0
  * `front`: 큐의 가장 앞 정수 출력 (없으면 -1)
  * `back`: 큐의 가장 뒤 정수 출력 (없으면 -1)

즉, **Queue 자료구조의 기본 동작**을 구현하는 문제.

---

## 구현 과정

### 1. 기본 아이디어

* 큐는 FIFO 구조 → **앞에서 꺼내고 뒤에서 넣기**
* 파이썬 `list`를 그대로 쓰면 `pop(0)`에서 **O(n)** 시간 복잡도가 걸려서 비효율적.
* 따라서 **`collections.deque`** 를 활용하면 앞뒤에서 O(1)에 삽입/삭제가 가능하다.

### 2. 코드 구현

```python
import sys
from collections import deque

n = int(sys.stdin.readline())
queue = deque()

for _ in range(n):
    cmd = sys.stdin.readline().strip().split()

    if cmd[0] == "push":
        queue.append(int(cmd[1]))
    elif cmd[0] == "pop":
        if queue:
            print(queue.popleft())
        else:
            print(-1)
    elif cmd[0] == "size":
        print(len(queue))
    elif cmd[0] == "empty":
        print(1 if not queue else 0)
    elif cmd[0] == "front":
        if queue:
            print(queue[0])
        else:
            print(-1)
    elif cmd[0] == "back":
        if queue:
            print(queue[-1])
        else:
            print(-1)
```

---

## 배운 점

1. **큐 구현 시 `list` vs `deque` 차이**

   * `list.pop(0)` → O(n) (앞 원소 제거 시 모든 요소를 한 칸씩 당겨야 함)
   * `deque.popleft()` → O(1) (양쪽 끝에서 삽입/삭제 최적화)

2. **front / back 접근**

   * `queue[0]` → 맨 앞 원소
   * `queue[-1]` → 맨 뒤 원소

3. **문제 조건 처리**

   * 비어있을 때 `pop`, `front`, `back` 호출 시 -1 출력
