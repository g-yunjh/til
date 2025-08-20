# 파이썬으로 최대 힙 구현하기 (BOJ 11279번 문제)

## 문제 요약

* **연산 규칙**

  * 자연수 x → 배열에 추가 (삽입 연산)
  * 0 → 배열에서 가장 큰 값 출력 후 제거
  * 배열이 비어 있는데 0이 들어오면 `0` 출력

즉, \*\*우선순위 큐(priority queue)\*\*를 최대 힙 형태로 구현하는 문제.

---

## 구현 과정

### 1. 아이디어

* 파이썬 표준 라이브러리 `heapq`는 \*\*최소 힙(min heap)\*\*만 지원.
* 최대 힙을 만들려면 값을 **음수로 변환**해서 저장하면 됨.

  * 예: `heapq.heappush(heap, -x)`
  * 꺼낼 때 다시 `-`를 붙여서 원래 값으로 복구.

### 2. 코드 구현

```python
import sys
import heapq

n = int(sys.stdin.readline())
heap = []

for _ in range(n):
    x = int(sys.stdin.readline())

    if x == 0:
        if heap:
            print(-heapq.heappop(heap))
        else:
            print(0)
    else:
        heapq.heappush(heap, -x)
```

---

## 배운 점

1. **파이썬의 `heapq`는 최소 힙**

   * 최대 힙이 필요한 경우, **부호 반전** 기법을 활용한다.

2. **힙의 시간 복잡도**

   * 삽입(`heappush`) → O(log N)
   * 삭제(`heappop`) → O(log N)
   * 최대/최소 원소 확인 → O(1)

3. **조건 처리**

   * 비어있을 때 pop 연산 요청 시 `0` 출력.
