# 파이썬으로 최소 힙 구현하기 (BOJ 1927번)

## 문제 요약

* 연산 규칙

  * 자연수 `x` 입력 → 힙에 `x` 삽입
  * `0` 입력 → 힙에서 **가장 작은 값**을 출력하고 제거
  * 힙이 비어 있는데 `0`이 들어오면 `0` 출력
* 처음 힙은 빈 상태

---

## 구현 아이디어

* 파이썬의 `heapq`는 **기본이 최소 힙**이므로 그대로 사용하면 된다.
* 입출력이 많으므로 `sys.stdin.readline()` 사용.
* `0`이 들어왔을 때 비어 있으면 `0`, 아니면 `heapq.heappop` 결과 출력.

---

## 코드

```python
import sys
import heapq

input = sys.stdin.readline

n = int(input())
heap = []

for _ in range(n):
    x = int(input())
    if x == 0:
        if heap:
            print(heapq.heappop(heap))
        else:
            print(0)
    else:
        heapq.heappush(heap, x)
```

---

## 배운 점 / 체크포인트

1. **시간 복잡도**

   * 삽입(`heappush`) O(log N), 삭제(`heappop`) O(log N), 최솟값 조회 O(1)
2. **빈 힙 처리**

   * `0` 명령에서 빈 힙이면 `0` 출력 (예외처리 필수)
3. **I/O 최적화**

   * 반복 입력이 커서 `input()` 대신 `sys.stdin.readline()`이 유리
