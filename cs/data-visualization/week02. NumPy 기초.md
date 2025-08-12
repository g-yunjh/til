## ✅ 배열 선언

- **1차원**
    
    - 넘파이 어레이: `np.array([1, 2, 3])`
        
    - 파이썬 리스트: `[1, 2, 3]`
        
- **2차원 (매트릭스)**
    
    - 넘파이 어레이:  
        `np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12], [13, 14, 15, 16]])`
        
    - 파이썬 리스트:  
        `[[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12], [13, 14, 15, 16]]`
        

---

## ✅ 1차원 인덱싱

- **기본 인덱싱**
    
    - `a1[0], a1[1], a1[2], a1[-2], a1[-1]`  
        → 0번째, 1번째, 2번째, 뒤에서 2번째, 뒤에서 1번째
        
- **슬라이싱**
    
    - `a1[0:2]` → 0~1번째
        
    - `a1[3:]` → 3번째부터 끝까지
        
    - `a1[:-2]` → 처음부터 뒤에서 2번째까지
        
- **리스트 형태 접근 (numpy만 가능)**
    
    - `a1[[1, 2, 4]]` → 1, 2, 4번째 인덱스 원소 반환
        
    - `l1[[1, 2, 4]]` → ❌ TypeError 발생 (리스트는 리스트 인덱싱 불가)
        
- **불리언 인덱싱**
    
    - `a1[[True, False, True, True, False]]`
        
    - `a1[a1 < 3]`
        
    - `l1[a1 < 3]` → ❌ 리스트는 불가능
        

---

## ✅ 2차원 인덱싱

- **리스트 → 넘파이 변환**
    
    - `a2 = np.array(l2)`
        
- **기본 인덱싱**
    
    - `a2[0][1], a2[2][3]`
        
    - `a2[2][-1]`
        
- **넘파이 스타일 인덱싱**
    
    - `a2[0, 1], a2[2, 3], a2[2, -1]`
        
- **슬라이싱**
    
    - `a2[0, 0:3]`
        
    - `a2[0, :]`
        
    - `a2[[0, 2], 2:]`
        
    - `a2[2:, [0, 2]]`
        
    - `a2[0:4:2, :]`
        
    - `a2[0:4:2]`

## ✅ 브로드캐스팅 (Broadcasting) & 벡터화 연산

- **넘파이 배열의 브로드캐스팅**
    
    - `a1 + 1` → 모든 원소에 1 더하기
        
    - `a1 * 2` → 모든 원소에 2 곱하기
        
    - `a1 / 2`, `a1 ** 2` → 나누기, 제곱 연산
        
- **파이썬 리스트와의 차이**
    
    - `l1 + 1` → ❌ 에러
        
    - `[f + 1 for f in l1]` → ✔️ 리스트 컴프리헨션 필요
        
    - `l1 * 2` → 리스트 복제됨 (곱셈 아님)
        

---

## ✅ 배열 생성

- **리스트 기반 생성**
    
    - `np.array([1, 2, 3])`
        
    - `np.array(l1)`
        
    - `np.array(l2)`
        
- **range 기반 생성**
    
    - `np.array(range(5))` → `[0, 1, 2, 3, 4]`
        
    - `np.arange(5)`
        
    - `np.arange(1, 6)` → 1부터 5까지
        
    - `np.arange(1, 6, 2)` → 1, 3, 5
        
- **등간격 배열**
    
    - `np.linspace(0, 10, 5)` → [0. 2.5 5. 7.5 10.]
        
- **0과 1로 채우기**
    
    - `np.ones(10)`, `np.ones([3, 3])`
        
    - `np.zeros(7)`, `np.zeros([7, 7])`
        

---

## ✅ 랜덤 배열 생성

- **Uniform 분포 (0~1)**
    
    - `np.random.random(10)`
        
    - `np.random.random([4, 4])`
        
    - `np.random.rand(10)`
        
    - `np.random.rand(3, 3)`
        
- **정수 랜덤**
    
    - `np.random.randint(1, 10, [4, 4])` → 1~9 사이 정수
        
- **표준 정규분포 (mean=0, std=1)**
    
    - `np.random.randn(10)`
        
    - `np.random.randn(3, 3)`
        

---

## ✅ 배열 변형 (Reshape)

- **기본 reshape**
    
    - `a1 = np.arange(12)`
        
    - `a1.reshape(3, 4)`
        
    - `b2 = a1.reshape(3, 4)`
        
    - `b2.reshape(12)`
        
    - `b2.flatten()`
        
- **-1을 이용한 자동 계산**
    
    - `a1.reshape(3, -1)`
        
    - `a1.reshape(2, -1)`
        
    - `b2.reshape(-1)`
        
- **reshape + 시드 고정**
    
    - `np.random.seed(7)`
        
    - `np.random.randn(4).reshape(2, 2)`
        

---
## ✅ 행렬 계산 (Matrix Calculation)

- **행렬 곱**
    
    - `a = np.array([[1, 2], [3, 4]])`
        
    - `b = np.array([[5], [6]])`
        
    - `a @ b` → 행렬곱 (dot product)
        
- **역행렬 계산**
    
    - `np.linalg.inv(a)`
        
    - `a @ np.linalg.inv(a)` → 단위행렬 확인
        
- **연립방정식 예제**
    
    ```python
    a = np.array([[0, 1, 1, 1],
                  [1, 0, 1, 1],
                  [1, 1, 0, 1],
                  [1, 1, 1, 0]])
    b = np.array([3, 3, 3, 3]).reshape(4, 1)
    np.linalg.inv(a) @ b
    ```
    

---

## ✅ 배열 차원과 축

- **차원 확인**
    
    - `a.shape`, `a.ndim`
        
- **차원 예시**
    
    - 스칼라(0D): `np.array(3)`
        
    - 벡터(1D): `np.array([1,2,3])`
        
    - 매트릭스(2D): `np.array([[1,2],[3,4]])`
        
    - 텐서(3D): `np.array(range(24)).reshape(2, 3, 4)`
        
- **축 (axis)**
    
    - axis=0 → 행 방향
        
    - axis=1 → 열 방향
        

---

## ✅ 배열 결합

- **concatenate**
    
    ```python
    np.concatenate([a, b], axis=0)  # 수직 결합
    np.concatenate([a, b], axis=1)  # 수평 결합
    ```
    
- **다양한 축 결합 예시**
    
    ```python
    a = np.array([1,2,3,4]).reshape(2,2)
    b = np.array([5,6]).reshape(2,1)
    c = np.array([10,11]).reshape(1,2)
    
    np.concatenate([a, c], axis=0)
    np.concatenate([a, b], axis=1)
    np.concatenate([a, b.T, c], axis=0)
    np.concatenate([a, b, c.T], axis=1)
    ```
    
- **hstack & vstack**
    
    ```python
    np.vstack([a, b]) # 수직 결합
    np.hstack([a, b]) # 수평 결합
    ```
    

---

## ✅ 수학 함수

- **기초 수학 함수**
    
    ```python
    np.sqrt(a1)     # 제곱근
    np.exp(a1)      # 지수
    np.log(a1)      # 로그
    np.sin(a1)      # 사인
    ```
    
- **축방향 계산**
    
    ```python
    a.sum(), a.sum(axis=0), a.sum(axis=1)
    a.mean(), a.mean(axis=0), a.mean(axis=1)
    a.std(), a.max(), a.min(), a.prod()
    ```
    

---
## ✅ 넘파이의 빠른 연산 — 시간 비교

- **리스트 반복 연산 (느림)**
    
    ```python
    import time
    import numpy as np
    
    n = 5000
    x = np.random.rand(n, n)
    y = np.random.rand(n, n)
    z = np.zeros((n, n))
    
    t1 = time.time()
    for i in range(n):
        for j in range(n):
            z[i, j] = (x[i, j] - y[i, j])**2
    t2 = time.time()
    print(t2 - t1)
    ```
    
- **넘파이 벡터화 연산 (빠름)**
    
    ```python
    t1 = time.time()
    z = (x - y) ** 2
    t2 = time.time()
    print(t2 - t1)
    ```
    
- **다른 방법들**
    
    ```python
    ((x.reshape(-1) - y.reshape(-1))**2).reshape(n, n)
    ```
    

---

## ✅ 실습 과제 예제 정리

- **벡터 생성**
    
    ```python
    a0 = np.array([1, 2, 3, 4])
    ```
    
- **arange로 매트릭스 생성**
    
    ```python
    a1 = np.arange(16).reshape(4, 4)
    ```
    
- **표준정규분포 매트릭스 생성**
    
    ```python
    a2 = np.random.randn(16).reshape(4, 4)
    ```
    
- **a0를 a1에 열 방향으로 추가**
    
    ```python
    np.concatenate([a1, a0.reshape(4, 1)], axis=1)
    ```
    
- **a2를 a1에 행 방향으로 추가**
    
    ```python
    np.vstack([a1, a2])
    ```
    
- **a2에 평균 5, 분산 1 적용**
    
    ```python
    a2 + 5  # 브로드캐스팅
    ```
    
- **a1의 행 평균과 열 합**
    
    ```python
    np.mean(a1, axis=0), np.sum(a1, axis=1)
    ```
    
- **연립방정식 해 구하기**
    
    ```python
    np.linalg.inv(np.array([[1, 1], [1, 0]])) @ np.array([1, 2]).reshape(2, 1)
    ```
    
