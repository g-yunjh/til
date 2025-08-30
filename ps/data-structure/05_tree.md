## 1) BOJ 1068 — 트리

### 문제 핵심

* 입력: 정점 수 `N`, 각 노드의 부모를 나타내는 배열 `parent[]` (`-1`은 루트), 그리고 삭제할 노드 `K`.
* 작업: 노드 `K`와 그 **서브트리 전체**를 지운 뒤, 남은 트리에서 **리프 노드 개수**를 출력해야 한다.

### 풀이 아이디어

1. `parent` 배열을 기반으로 **자식 리스트**를 만든다.
2. `K`가 루트라면 트리가 전부 사라지므로 답은 `0`.
3. 루트에서 DFS를 하되, 삭제된 노드 `K`는 아예 탐색에서 제외한다.
4. 탐색한 노드가 더 이상 유효한 자식이 없으면 리프라고 본다.

### 코드

```python
import sys
sys.setrecursionlimit(1_000_000)
input = sys.stdin.readline

N = int(input())
parent = list(map(int, input().split()))
K = int(input())

children = [[] for _ in range(N)]
root = -1

for i, p in enumerate(parent):
    if p == -1:
        root = i
    else:
        children[p].append(i)

if K == root:
    print(0)
    sys.exit(0)

def dfs(u):
    valid_children = [v for v in children[u] if v != K]
    if not valid_children:
        return 1
    cnt = 0
    for v in valid_children:
        cnt += dfs(v)
    return cnt

print(dfs(root))
```

### 정리

* 시간 복잡도: O(N) (각 노드를 최대 한 번 방문)
* 공간 복잡도: O(N) (자식 리스트 + 재귀 스택)
* 자식이 모두 삭제된 경우 부모가 새 리프가 되는 점이 핵심 포인트

---

## 2) BOJ 1991 — 트리 순회

### 문제 핵심

* 입력: 노드 개수 `N`, 그리고 `N`개의 줄 (`부모 왼쪽 오른쪽`).
  자식이 없으면 `.`으로 주어진다.
* 출력: **전위**, **중위**, **후위** 순회 결과를 각각 한 줄씩.

### 풀이 아이디어

* 문자 노드(`A`부터 시작)가 주어지므로 딕셔너리로 트리를 저장한다:
  `tree[node] = (left, right)`
* 순회 규칙:

  * 전위: 루트 → 왼쪽 → 오른쪽
  * 중위: 왼쪽 → 루트 → 오른쪽
  * 후위: 왼쪽 → 오른쪽 → 루트

### 코드

```python
import sys
input = sys.stdin.readline

N = int(input())
tree = {}

for _ in range(N):
    p, l, r = input().split()
    tree[p] = (l, r)

def preorder(u, out):
    if u == '.':
        return
    l, r = tree[u]
    out.append(u)
    preorder(l, out)
    preorder(r, out)

def inorder(u, out):
    if u == '.':
        return
    l, r = tree[u]
    inorder(l, out)
    out.append(u)
    inorder(r, out)

def postorder(u, out):
    if u == '.':
        return
    l, r = tree[u]
    postorder(l, out)
    postorder(r, out)
    out.append(u)

root = 'A'  # 문제 조건에서 루트는 A
pre, ino, post = [], [], []
preorder(root, pre)
inorder(root, ino)
postorder(root, post)

print(''.join(pre))
print(''.join(ino))
print(''.join(post))
```

### 정리

* 시간 복잡도: O(N)
* 공간 복잡도: O(N)
* `.` 처리를 빼먹지 않는 것이 중요
* 출력은 전위 / 중위 / 후위 순서로 정확히 세 줄
