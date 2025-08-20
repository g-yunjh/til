# 파이썬으로 스택 구현하기 (BOJ 10828번 문제)

## 문제 요약

* **명령어 종류 (5가지)**

  * `push X`: 스택에 정수 X 삽입
  * `pop`: 가장 위의 정수 출력 후 제거 (없으면 -1)
  * `size`: 스택 크기 출력
  * `empty`: 비어있으면 1, 아니면 0
  * `top`: 가장 위의 정수 출력 (없으면 -1)

즉, 기본적인 **Stack 자료구조의 동작**을 구현하는 문제.

---

## 구현 과정

### 1. 기본 아이디어

* 파이썬에서는 `list`가 동적 배열이므로, `append`와 `pop()`을 사용하면 스택처럼 동작함.
* 다만, 문제에서 요구하는 출력 조건에 맞게 직접 구현해야 함.

### 2. 코드 구현

```python
import sys

n = int(sys.stdin.readline())
stack = []

for _ in range(n):
    cmd = sys.stdin.readline().strip().split()

    if cmd[0] == "push":
        stack.append(int(cmd[1]))
    elif cmd[0] == "pop":
        if stack:
            print(stack.pop())
        else:
            print(-1)
    elif cmd[0] == "size":
        print(len(stack))
    elif cmd[0] == "empty":
        print(1 if not stack else 0)
    elif cmd[0] == "top":
        if stack:
            print(stack[-1])
        else:
            print(-1)
```

---

## 배운 점

1. **입출력 최적화 필요성**

   * 백준에서 `input()` 대신 `sys.stdin.readline()`을 쓰는 이유:
     반복문에서 많은 입력을 처리할 때 속도 차이가 크다.

2. **리스트를 스택처럼 활용하기**

   * `append()` → `push`
   * `pop()` → `pop` (마지막 원소 제거)
   * 인덱싱 `[-1]` → `top`

3. **문제 조건 처리**

   * 비어있을 때 예외 처리를 반드시 해줘야 한다. (`pop`, `top`에서 -1 출력)
