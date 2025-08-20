# Ch06. 최단 경로 알고리즘

## 1. 개념 요약
- **최단 경로(Shortest Path)**: 그래프에서 두 노드 간 거리가 가장 짧은 경로.
- 유형:
  1. 한 지점 → 다른 한 지점
  2. 한 지점 → 모든 지점
  3. 모든 지점 → 모든 지점

## 2. 주요 알고리즘
### (1) 다익스트라(Dijkstra)
- **특징**: 음의 간선 ❌, 그리디 기반.
- 동작:
  1. 시작 노드 → 모든 노드 거리 테이블 초기화.
  2. 방문하지 않은 노드 중 최단 거리 노드 선택.
  3. 해당 노드를 거쳐 가는 거리 계산 → 기존 값과 비교 후 갱신.
- 시간 복잡도:
  - 단순 구현: O(V²)
  - **우선순위 큐(힙)** 사용: O(E log V)

```python
import heapq

def dijkstra(start):
    q = []
    heapq.heappush(q, (0, start))
    distance[start] = 0
    while q:
        dist, now = heapq.heappop(q)
        if distance[now] < dist:
            continue
        for nxt, cost in graph[now]:
            new_cost = dist + cost
            if new_cost < distance[nxt]:
                distance[nxt] = new_cost
                heapq.heappush(q, (new_cost, nxt))
```

### (2) 우선순위 큐 (Priority Queue)
- **정의**: 우선순위 높은 데이터가 먼저 나오는 자료구조.
- 구현: 파이썬 `heapq` → 최소 힙.
- 삽입/삭제: O(log N)

### (3) 플로이드-워셜(Floyd-Warshall)
- **모든 노드 → 모든 노드** 최단 거리.
- 다이나믹 프로그래밍 기반.
- 점화식:  
  `D[a][b] = min(D[a][b], D[a][k] + D[k][b])`
- 시간 복잡도: O(N³)

```python
n = int(input())
m = int(input())
INF = int(1e9)
graph = [[INF]*(n+1) for _ in range(n+1)]

for a in range(1, n+1):
    graph[a][a] = 0

for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a][b] = c

for k in range(1, n+1):
    for a in range(1, n+1):
        for b in range(1, n+1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])
```

## 3. 대표 예시
### (1) 전보
- 한 도시에서 출발 → 도달 가능한 도시 개수 & 최단 시간 계산.
- N, M 크기 큼 → 우선순위 큐 사용 다익스트라 효율적.

```python
n, m, start = map(int, input().split())
graph = [[] for _ in range(n+1)]
INF = int(1e9)
distance = [INF]*(n+1)

for _ in range(m):
    x, y, z = map(int, input().split())
    graph[x].append((y, z))

dijkstra(start)

count = 0
max_dist = 0
for d in distance:
    if d != INF and d != 0:
        count += 1
        max_dist = max(max_dist, d)

print(count, max_dist)
```

### (2) 미래 도시
- 1번 회사 → K번 회사 → X번 회사 최소 시간.
- N 작음 → 플로이드-워셜로 모든 최단 거리 계산 후  
  `distance[1][K] + distance[K][X]`

```python
n, m = map(int, input().split())
INF = int(1e9)
graph = [[INF]*(n+1) for _ in range(n+1)]
for a in range(1, n+1):
    graph[a][a] = 0

for _ in range(m):
    a, b = map(int, input().split())
    graph[a][b] = 1
    graph[b][a] = 1

x, k = map(int, input().split())

for c in range(1, n+1):
    for a in range(1, n+1):
        for b in range(1, n+1):
            graph[a][b] = min(graph[a][b], graph[a][c] + graph[c][b])

distance = graph[1][k] + graph[k][x]
print(distance if distance < INF else -1)
```

## 4. 함정 & 주의
- **다익스트라**: 음의 간선 존재 시 사용 불가.
- **플로이드-워셜**: N³ → N이 크면 시간 초과.
- 거리 초기화 시 **INF 설정** 주의.
- 인접 리스트/인접 행렬 선택 시 **메모리 & 시간 복잡도** 고려.
