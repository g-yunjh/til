# Ch04. 이진 탐색 알고리즘

## 1. 개념 요약
- **이진 탐색(Binary Search)**: **정렬된** 배열/구간에서 탐색 범위를 절반씩 줄여 목표 값을 찾는 알고리즘.
- 시간 복잡도: **O(log N)**, 공간 복잡도: 재귀 사용 시 O(log N) (호출 스택), 반복문은 O(1).

## 2. 전제 조건
- 입력 데이터가 **오름차순 정렬**되어 있어야 함.
- 비교 연산으로 **단조성(모노토닉)** 이 보장되는 결정 문제에 적합.

## 3. 구현 패턴
### (1) 반복문 템플릿
```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid
        if arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1  # not found
```

### (2) 재귀 템플릿
```python
def bs_rec(arr, target, lo, hi):
    if lo > hi:
        return -1
    mid = (lo + hi) // 2
    if arr[mid] == target:
        return mid
    if arr[mid] < target:
        return bs_rec(arr, target, mid + 1, hi)
    return bs_rec(arr, target, lo, mid - 1)
```

## 4. 파이썬 이진 탐색 라이브러리 (`bisect`)
- `bisect_left(a, x)`: x를 **삽입할 가장 왼쪽 인덱스**.
- `bisect_right(a, x)`: x를 **삽입할 가장 오른쪽 인덱스**(오른쪽 경계).
- 응용: 특정 값 또는 구간에 속하는 원소 **개수 세기**.

```python
from bisect import bisect_left, bisect_right

def count_by_value(a, x):
    # a는 정렬되어 있어야 함
    return bisect_right(a, x) - bisect_left(a, x)

def count_in_range(a, left, right):
    return bisect_right(a, right) - bisect_left(a, left)
```

## 5. Parametric Search (매개변수 탐색)
- **최적화 문제 → 결정 문제(Yes/No)** 로 바꿔 **가능 여부**를 이진 탐색으로 판단하며 최적 값을 찾는 기법.
- 핵심: 
  1) **정답의 범위**를 수치 구간으로 잡기  
  2) 임의의 값 `mid`에 대해 **조건을 만족하는지(결정 함수)** 판정  
  3) 판정 결과에 따라 탐색 구간 이동

### (1) 템플릿
```python
def is_ok(x) -> bool:
    # x가 주어졌을 때 조건을 만족하는지 판단 (단조성 필요)
    ...

def parametric_search(lo, hi):
    ans = lo - 1  # 최대화 문제: 조건을 만족하는 최댓값
    while lo <= hi:
        mid = (lo + hi) // 2
        if is_ok(mid):
            ans = mid
            lo = mid + 1   # 더 크게 시도
        else:
            hi = mid - 1   # 줄여서 시도
    return ans
```

## 6. 대표 예시
### (1) 떡볶이 떡 만들기 (절단기 높이 H의 최댓값)
- 문제: 절단기 높이 `H`를 정했을 때, 잘려 나온 떡의 총 길이 ≥ `M`이 되도록 하는 **가장 큰 H** 찾기.
- 결정 함수: `H`가 주어지면 합계 길이 계산 후 **M 이상인지** 확인.

```python
def max_cutter_height(heights, M):
    lo, hi = 0, max(heights)
    ans = 0
    while lo <= hi:
        mid = (lo + hi) // 2
        total = 0
        for h in heights:
            if h > mid:
                total += (h - mid)
        if total >= M:       # 충분히 잘렸음 → 더 높여도 됨
            ans = mid
            lo = mid + 1
        else:                # 부족 → 낮춰야 함
            hi = mid - 1
    return ans
```

### (2) 정렬된 배열에서 특정 수의 개수 구하기
- 아이디어: `bisect_right(a, x) - bisect_left(a, x)` 로 O(log N) 두 번에 해결.

```python
from bisect import bisect_left, bisect_right

def count_x(a, x):
    return bisect_right(a, x) - bisect_left(a, x)
```

## 7. 실전 팁
- 경계 조건 주의: `while lo <= hi`, 중간값 계산, 인덱스 범위 확인.
- **정렬 필수**: 입력이 정렬되지 않았다면 먼저 정렬 (O(N log N)).
- **오버플로**: 파이썬은 안전하지만, 일반론적으로 `mid = lo + (hi - lo)//2` 습관화.
- **단조성 체크**: `is_ok(mid)` 가 `mid`에 대해 단조(True/False가 한 번만 바뀜)해야 이진 탐색이 가능.
- **중복 처리**: 첫/마지막 위치는 `bisect_left/right` 또는 직접 경계 탐색 루틴을 사용.

## 8. 추가 유틸 (첫/마지막 위치 찾기)
```python
def lower_bound(a, x):
    lo, hi = 0, len(a)
    while lo < hi:
        mid = (lo + hi) // 2
        if a[mid] < x:
            lo = mid + 1
        else:
            hi = mid
    return lo  # x 이상 첫 위치

def upper_bound(a, x):
    lo, hi = 0, len(a)
    while lo < hi:
        mid = (lo + hi) // 2
        if a[mid] <= x:
            lo = mid + 1
        else:
            hi = mid
    return lo  # x 초과 첫 위치
```

## 9. 함정 & 주의
- **정렬 전제 누락**: 정렬되지 않은 데이터에 이진 탐색 적용 → 오답.
- **무한 루프**: 경계 갱신 실수(`lo = mid`/`hi = mid`로 고정) 주의.
- **조건식 방향 오류**: `is_ok(mid)`의 의미(최소화/최대화)를 헷갈리면 탐색 방향 반대.
- **카운팅 문제**: 값이 존재하지 않는 경우 개수 0 반환을 명확히 처리.
