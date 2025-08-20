# Ch05. 다이나믹 프로그래밍

## 1. 개념 요약
- **다이나믹 프로그래밍(DP)**: 큰 문제를 작은 문제로 쪼개고, **이미 계산한 결과를 저장**하여 중복 계산을 피하는 알고리즘.
- 시간 효율성을 비약적으로 향상.
- 대표 특징:
  1. **최적 부분 구조 (Optimal Substructure)**: 큰 문제의 해가 작은 문제 해의 조합으로 표현 가능.
  2. **중복되는 부분 문제 (Overlapping Subproblems)**: 동일한 하위 문제가 반복적으로 등장.

## 2. 구현 방식
### (1) Top-down (Memoization)
- 큰 문제 → 작은 문제로 **재귀 호출**.
- 계산한 결과를 **메모리(배열, 딕셔너리)**에 저장.
- 재귀 깊이 제한 고려 필요.

### (2) Bottom-up (Tabulation)
- 작은 문제부터 차례대로 해결 → **반복문** 사용.
- 결과 저장용 배열을 **DP 테이블**이라 부름.
- 재귀 호출 오버헤드가 없음.

## 3. DP vs 분할 정복
- 둘 다 최적 부분 구조 필요.
- 차이점: DP는 **부분 문제가 중복**됨, 분할 정복은 중복되지 않음 (예: 퀵 정렬).

## 4. 대표 예시

### (1) 피보나치 수열
```python
# Top-down
memo = [0] * 100
def fibo(x):
    if x == 1 or x == 2:
        return 1
    if memo[x] != 0:
        return memo[x]
    memo[x] = fibo(x-1) + fibo(x-2)
    return memo[x]

# Bottom-up
dp = [0] * 100
dp[1], dp[2] = 1, 1
n = 99
for i in range(3, n+1):
    dp[i] = dp[i-1] + dp[i-2]
```
- 단순 재귀: O(2^N) → DP: O(N)

### (2) 개미 전사
- 인접한 식량창고 선택 불가 → 최대 식량량 구하기.
- 점화식:  
  `dp[i] = max(dp[i-1], dp[i-2] + k_i)`

```python
n = int(input())
arr = list(map(int, input().split()))
dp = [0] * n
dp[0] = arr[0]
dp[1] = max(arr[0], arr[1])
for i in range(2, n):
    dp[i] = max(dp[i-1], dp[i-2] + arr[i])
print(dp[n-1])
```

### (3) 1로 만들기
- 연산: -1, /2, /3, /5 가능.
- 점화식:  
  `dp[i] = min(dp[i-1], dp[i/2], dp[i/3], dp[i/5]) + 1`

```python
x = int(input())
dp = [0] * (x+1)
for i in range(2, x+1):
    dp[i] = dp[i-1] + 1
    if i % 2 == 0:
        dp[i] = min(dp[i], dp[i//2] + 1)
    if i % 3 == 0:
        dp[i] = min(dp[i], dp[i//3] + 1)
    if i % 5 == 0:
        dp[i] = min(dp[i], dp[i//5] + 1)
print(dp[x])
```

### (4) 효율적인 화폐 구성
- N종류 화폐로 M원을 만드는 최소 화폐 개수.
- 점화식:  
  `dp[i] = min(dp[i], dp[i-k] + 1)`

```python
n, m = map(int, input().split())
coins = [int(input()) for _ in range(n)]
dp = [10001] * (m+1)
dp[0] = 0
for coin in coins:
    for i in range(coin, m+1):
        if dp[i-coin] != 10001:
            dp[i] = min(dp[i], dp[i-coin] + 1)
print(-1 if dp[m] == 10001 else dp[m])
```

## 5. 문제 접근 팁
1. **그리디 / 완전 탐색**으로 풀 수 있는지 먼저 검토.
2. 두 조건(**최적 부분 구조**, **중복 부분 문제**)를 만족하면 DP 고려.
3. **점화식**부터 세우기.
4. 배열 인덱스, 초기값 설정에 주의.

## 6. 함정 & 주의
- Top-down: 재귀 깊이 초과 가능 → `sys.setrecursionlimit()` 필요.
- Bottom-up: 초기값 설정 누락 시 오답.
- 점화식 오류가 가장 빈번한 실수.
- 메모리 제한 고려: DP 테이블 크기 최소화.
