# Ch07. 기타 그래프 이론

## 1. 서로소 집합 (Disjoint Sets, Union-Find)
- **정의**: 공통 원소가 없는 집합들의 집합.
- **주요 연산**:
  1. **Find(x)**: 원소 x가 속한 집합의 루트 노드 찾기.
  2. **Union(a, b)**: a와 b가 속한 집합 합치기.
- **경로 압축(Path Compression)**: Find 연산 시 루트를 바로 부모로 연결 → 성능 향상.

```python
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b
```

### 사이클 판별
- 무방향 그래프에서 사이클 여부 판단:
  - 두 노드가 같은 루트 → 사이클 발생.

```python
cycle = False
for a, b in edges:
    if find_parent(parent, a) == find_parent(parent, b):
        cycle = True
        break
    else:
        union_parent(parent, a, b)
```

## 2. 최소 신장 트리 (MST)
- **정의**: 모든 노드를 포함, 사이클 없이 연결하는 간선 집합 중 최소 비용.
- 대표 알고리즘: **크루스컬(Kruskal)** (그리디 기반).
  1. 간선 비용 오름차순 정렬.
  2. 작은 간선부터 확인 → 사이클 없으면 포함.
- 시간 복잡도: O(E log E)

```python
edges.sort(key=lambda x: x[2])  # 비용 기준 정렬
result = 0
for a, b, cost in edges:
    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost
```

## 3. 위상 정렬 (Topological Sort)
- **정의**: 방향 그래프(DAG)의 모든 노드를 방향성에 어긋나지 않게 순서대로 나열.
- **조건**: 사이클이 없어야 함.
- **동작 과정**:
  1. 진입차수 0인 노드 모두 큐에 삽입.
  2. 큐에서 노드 꺼내 결과에 추가.
  3. 해당 노드에서 나가는 간선 제거 → 새롭게 진입차수 0인 노드 큐 삽입.
- 시간 복잡도: O(V+E)

```python
from collections import deque

def topology_sort():
    result = []
    q = deque()
    for i in range(1, v+1):
        if indegree[i] == 0:
            q.append(i)
    while q:
        now = q.popleft()
        result.append(now)
        for nxt in graph[now]:
            indegree[nxt] -= 1
            if indegree[nxt] == 0:
                q.append(nxt)
    return result
```

## 4. 대표 예시
### (1) 사이클 판별
- Union-Find로 구현.
- 같은 루트를 가지는 간선 발견 시 사이클 존재.

### (2) 크루스컬 MST
- 간선 비용 최소화.
- 네트워크 연결 비용 문제 등에 활용.

### (3) 위상 정렬
- 작업 순서, 선수 과목 문제 등.

## 5. 함정 & 주의
- **Union-Find**: 경로 압축 + 랭크(Union by rank) 최적화로 시간 단축.
- **MST**: 간선 정렬 시 O(E log E), E 많으면 메모리 고려.
- **위상 정렬**: 사이클 존재 시 모든 노드 방문 불가 → 큐가 빔.
