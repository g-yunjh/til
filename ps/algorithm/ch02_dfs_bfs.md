# Ch02. 그래프 탐색 알고리즘 : DFS & BFS

## 1. 개념 요약
- **탐색(Search)**: 많은 데이터 중 원하는 데이터를 찾는 과정.
- 코딩 테스트 단골: **DFS**(깊이 우선 탐색) & **BFS**(너비 우선 탐색).

## 2. 기초 자료구조
### (1) 스택(Stack)
- **LIFO** (Last-In, First-Out, 선입후출).
- 입구·출구 동일.
- 파이썬: `append()` / `pop()`.

### (2) 큐(Queue)
- **FIFO** (First-In, First-Out, 선입선출).
- 파이썬: `collections.deque`  
  `append()` / `popleft()` 사용.

## 3. 재귀 함수(Recursive Function)
- 자기 자신을 호출하는 함수.
- **종료 조건** 필수 (무한 호출 방지).
- 예시: 팩토리얼, 유클리드 호제법.
- 장점: 복잡한 알고리즘 간결 구현.
- 단점: 이해 어려울 수 있음, 반복문으로 대체 가능.

## 4. DFS (Depth-First Search)
- **깊은 부분 우선** 탐색.
- 구현:
  - **재귀** 방식 (방문 처리 → 인접 노드 재귀 호출).
  - **스택** 사용.
- 특징: 경로 탐색, 연결 요소 찾기에 유용.

## 5. BFS (Breadth-First Search)
- **가까운 노드부터** 탐색.
- 구현:
  - **큐** 사용.
- 특징: 각 간선 비용이 동일할 때 **최단 거리** 탐색에 유리.

## 6. 대표 예시
### (1) 음료수 얼려 먹기
- 연결 요소(Connected Component) 개수 세기.
- DFS/BFS로 인접한 1(또는 0) 영역 전체 방문 처리.

### (2) 미로 탈출
- 시작점 → 목표점 최단 거리.
- BFS 사용 (간선 비용 동일 조건).

## 7. 파이썬 코드
```python
# DFS 예제
def dfs(x, y):
    if x < 0 or x >= n or y < 0 or y >= m:
        return False
    if graph[x][y] == 0:
        graph[x][y] = 1
        dfs(x-1, y)
        dfs(x+1, y)
        dfs(x, y-1)
        dfs(x, y+1)
        return True
    return False

n, m = map(int, input().split())
graph = [list(map(int, input())) for _ in range(n)]

result = 0
for i in range(n):
    for j in range(m):
        if dfs(i, j):
            result += 1
print(result)

# BFS 예제 (미로 탈출)
from collections import deque
n, m = map(int, input().split())
graph = [list(map(int, input())) for _ in range(n)]

dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

def bfs(x, y):
    queue = deque()
    queue.append((x, y))
    while queue:
        x, y = queue.popleft()
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            if nx < 0 or nx >= n or ny < 0 or ny >= m:
                continue
            if graph[nx][ny] == 0:
                continue
            if graph[nx][ny] == 1:
                graph[nx][ny] = graph[x][y] + 1
                queue.append((nx, ny))
    return graph[n-1][m-1]

print(bfs(0, 0))
```

## 8. 함정 & 주의
- DFS: 재귀 깊이 제한 → `sys.setrecursionlimit()` 고려.
- BFS: 큐 사용 시 `pop(0)` → O(N) 비효율, **deque** 사용.
- 방문 처리 누락 시 **무한 루프** 발생 가능.
