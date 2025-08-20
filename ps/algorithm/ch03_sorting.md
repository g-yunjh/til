# Ch03. 정렬 알고리즘

## 1. 개념 요약
- **정렬(Sorting)**: 데이터를 특정 기준에 따라 순서대로 나열.
- 다양한 알고리즘 존재 → 상황에 맞춰 선택.

## 2. 주요 정렬 알고리즘
### (1) 선택 정렬 (Selection Sort)
- 처리되지 않은 데이터 중 **가장 작은 값** 선택 → 맨 앞 데이터와 교환.
- 시간 복잡도: **O(N²)**
- 장점: 구현 쉬움.
- 단점: 대규모 데이터 비효율.

### (2) 삽입 정렬 (Insertion Sort)
- 처리되지 않은 데이터를 하나씩 꺼내 적절한 위치에 삽입.
- 시간 복잡도: **O(N²)**  
  단, **거의 정렬된 상태**라면 O(N) 수준으로 빠름.

### (3) 퀵 정렬 (Quick Sort)
- **기준(Pivot)** 설정 후, 피벗보다 작은 값·큰 값 분리.
- 평균: **O(N log N)** / 최악(이미 정렬): **O(N²)**.
- 대부분 언어의 표준 정렬 라이브러리의 기반.

### (4) 계수 정렬 (Counting Sort)
- 데이터의 **최댓값 K**가 작고, 정수로 표현 가능할 때 사용.
- 시간 복잡도: **O(N+K)** / 공간 복잡도 동일.
- 동일 값이 여러 개 있는 경우 효율적.

## 3. 알고리즘 비교
| 알고리즘   | 시간 복잡도(평균) | 공간 복잡도 | 특징 |
|------------|-------------------|-------------|------|
| 선택 정렬  | O(N²)             | O(1)        | 구현 간단, 느림 |
| 삽입 정렬  | O(N²)             | O(1)        | 거의 정렬된 경우 빠름 |
| 퀵 정렬    | O(N log N)        | O(log N)    | 평균적 성능 우수 |
| 계수 정렬  | O(N+K)            | O(N+K)      | K가 작을 때 매우 빠름 |

## 4. 대표 예시
### 두 배열의 원소 교체
- 배열 A 합 최대화 →  
  A: 오름차순, B: 내림차순 정렬 후 앞에서부터 K번 원소 교체.

## 5. 파이썬 코드
```python
# 선택 정렬
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]
for i in range(len(array)):
    min_index = i
    for j in range(i+1, len(array)):
        if array[j] < array[min_index]:
            min_index = j
    array[i], array[min_index] = array[min_index], array[i]
print(array)

# 삽입 정렬
for i in range(1, len(array)):
    for j in range(i, 0, -1):
        if array[j] < array[j-1]:
            array[j], array[j-1] = array[j-1], array[j]
        else:
            break

# 퀵 정렬 (재귀)
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[0]
    tail = arr[1:]
    left_side = [x for x in tail if x <= pivot]
    right_side = [x for x in tail if x > pivot]
    return quick_sort(left_side) + [pivot] + quick_sort(right_side)

print(quick_sort(array))

# 계수 정렬
count = [0] * (max(array) + 1)
for i in array:
    count[i] += 1
for i in range(len(count)):
    for _ in range(count[i]):
        print(i, end=' ')

# 두 배열의 원소 교체
n, k = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
a.sort()
b.sort(reverse=True)

for i in range(k):
    if a[i] < b[i]:
        a[i], b[i] = b[i], a[i]
    else:
        break
print(sum(a))
```

## 6. 함정 & 주의
- 퀵 정렬 최악 O(N²) → **피벗 선택 전략** 중요.
- 계수 정렬: K가 클 경우 메모리 사용량 폭증.
- 내장 정렬(`sort()`, `sorted()`): Timsort (병합+삽입) 기반, 안정 정렬.
