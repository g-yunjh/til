## 1. 정렬 기본

**[2750 - 수 정렬하기](https://www.acmicpc.net/problem/2750)**
```python
n = int(input())
nums = [int(input()) for _ in range(n)]
for x in sorted(nums):
    print(x)
```

---

## 2. 스택 기초

**[9012 - 괄호](https://www.acmicpc.net/problem/9012)**
```python
t = int(input())
for _ in range(t):
    s = input()
    st = []
    valid = True
    for ch in s:
        if ch == '(':
            st.append(ch)
        else:
            if st: st.pop()
            else:
                valid = False
                break
    print("YES" if valid and not st else "NO")
```

---

## 3. 큐 기본

**[2164 - 카드2](https://www.acmicpc.net/problem/2164)**
```python
from collections import deque
n = int(input())
q = deque(range(1, n+1))
while len(q) > 1:
    q.popleft()
    q.append(q.popleft())
print(q[0])
```

---

## 4. DFS 기초

**[2667 - 단지번호붙이기](https://www.acmicpc.net/problem/2667)**
```python
n = int(input())
grid = [list(map(int, input().strip())) for _ in range(n)]
visited = [[0]*n for _ in range(n)]
dx, dy = [1,-1,0,0], [0,0,1,-1]
res = []

def dfs(x, y):
    visited[x][y] = 1
    cnt = 1
    for i in range(4):
        nx, ny = x+dx[i], y+dy[i]
        if 0<=nx<n and 0<=ny<n and grid[nx][ny]==1 and not visited[nx][ny]:
            cnt += dfs(nx, ny)
    return cnt

for i in range(n):
    for j in range(n):
        if grid[i][j]==1 and not visited[i][j]:
            res.append(dfs(i,j))

print(len(res))
for x in sorted(res):
    print(x)
```

---

## 5. BFS 기초

**[2178 - 미로 탐색](https://www.acmicpc.net/problem/2178)**
```python
from collections import deque
n, m = map(int, input().split())
maze = [list(map(int, input().strip())) for _ in range(n)]
visited = [[0]*m for _ in range(n)]
dx, dy = [1,-1,0,0], [0,0,1,-1]

q = deque([(0,0)])
visited[0][0] = 1

while q:
    x, y = q.popleft()
    for i in range(4):
        nx, ny = x+dx[i], y+dy[i]
        if 0<=nx<n and 0<=ny<m and maze[nx][ny]==1 and not visited[nx][ny]:
            visited[nx][ny] = visited[x][y] + 1
            q.append((nx, ny))

print(visited[n-1][m-1])
```

---

## 6. 이분 탐색

**[1920 - 수 찾기](https://www.acmicpc.net/problem/1920)**
```python
n = int(input())
arr = sorted(map(int, input().split()))
m = int(input())
targets = list(map(int, input().split()))

def binary(a, x):
    l, r = 0, len(a)-1
    while l <= r:
        mid = (l+r)//2
        if a[mid]==x:
            return True
        elif a[mid]<x:
            l = mid+1
        else:
            r = mid-1
    return False

for t in targets:
    print(1 if binary(arr, t) else 0)
```

---

## 7. DP 기초

**[1463 - 1로 만들기](https://www.acmicpc.net/problem/1463)**
```python
n = int(input())
dp = [0]*(n+1)
for i in range(2, n+1):
    dp[i] = dp[i-1]+1
    if i%2==0: dp[i] = min(dp[i], dp[i//2]+1)
    if i%3==0: dp[i] = min(dp[i], dp[i//3]+1)
print(dp[n])
```

---

## 8. 누적합

**[11659 - 구간 합 구하기 4](https://www.acmicpc.net/problem/11659)**
```python
import sys
input = sys.stdin.readline
n, m = map(int, input().split())
arr = [0]+list(map(int, input().split()))
prefix = [0]*(n+1)
for i in range(1, n+1):
    prefix[i] = prefix[i-1]+arr[i]
for _ in range(m):
    a, b = map(int, input().split())
    print(prefix[b]-prefix[a-1])
```

---

## 9. 그리디 기본

**[11047 - 동전 0](https://www.acmicpc.net/problem/11047)**
```python
n, k = map(int, input().split())
coins = [int(input()) for _ in range(n)][::-1]
cnt = 0
for c in coins:
    if k==0: break
    cnt += k//c
    k %= c
print(cnt)
```

---

## 10. 투 포인터

**[2003 - 수들의 합 2](https://www.acmicpc.net/problem/2003)**
```python
n, m = map(int, input().split())
a = list(map(int, input().split()))
l = r = s = cnt = 0
while True:
    if s >= m:
        if s == m: cnt += 1
        s -= a[l]; l += 1
    else:
        if r == n: break
        s += a[r]; r += 1
print(cnt)
```

---

## 11. 힙 기본

**[11279 - 최대 힙](https://www.acmicpc.net/problem/11279)**
```python
import heapq, sys
input = sys.stdin.readline
h = []
for _ in range(int(input())):
    x = int(input())
    if x: heapq.heappush(h, -x)
    else: print(-heapq.heappop(h) if h else 0)
```

---

## 12. 문자열

**[1157 - 단어 공부](https://www.acmicpc.net/problem/1157)**
```python
s = input().upper()
cnt = [0]*26
for ch in s:
    cnt[ord(ch)-65]+=1
m = max(cnt)
if cnt.count(m)>1:
    print("?")
else:
    print(chr(cnt.index(m)+65))
```

---

## 13. 수학

**[2609 - 최대공약수와 최소공배수](https://www.acmicpc.net/problem/2609)**
```python
a, b = map(int, input().split())
def gcd(x, y):
    while y:
        x, y = y, x%y
    return x
g = gcd(a,b)
print(g)
print(a*b//g)
```

---

## 14. DP 패턴 2

**[2579 - 계단 오르기](https://www.acmicpc.net/problem/2579)**
```python
n = int(input())
s = [0]+[int(input()) for _ in range(n)]
dp = [0]*(n+1)
if n>=1: dp[1]=s[1]
if n>=2: dp[2]=s[1]+s[2]
for i in range(3, n+1):
    dp[i]=max(dp[i-2], dp[i-3]+s[i-1])+s[i]
print(dp[n])
```