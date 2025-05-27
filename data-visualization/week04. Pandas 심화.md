### ✅ 1. 열 이름 변경 (rename, set_axis)

#### 🔸 예시 데이터 생성

```python
import numpy as np
import pandas as pd

df = pd.DataFrame(np.round(np.random.rand(4*4).reshape(4, 4)*100))
```

#### 🔸 방법 1: 직접 할당

```python
df.columns = ['math', 'eng', 'sci', 'kor']
```

- ✔️ 간단함
    
- ❌ 원본 df가 수정됨
    

#### 🔸 방법 2: `set_axis()` 메서드

```python
df.set_axis(['art', 'eng', 'sci', 'com'], axis=1)
```

- ✔️ 원본 변경 없이 사용 가능
    
- axis=1 → 열
    

#### 🔸 방법 3: `rename()` 메서드

```python
df.rename({'kor': 'com'}, axis=1)
```

- ✔️ 원하는 열만 변경 가능
    
- ✔️ 원본 유지됨
    

---

### ✅ 2. 행 이름(index) 변경

#### 🔸 방법 1: 직접 할당

```python
df.index = ['aa', 'bb', 'cc', 'dd']
```

- ❌ 원본 수정됨
    

#### 🔸 방법 2: `set_axis()`

```python
df.set_axis(['a', 'b', 'c', 'f'], axis=0)
```

- ✔️ 원본 유지
    

#### 🔸 방법 3: `rename()`

```python
df.rename({'dd': 'ff'}, axis=0)
```

- ✔️ 원하는 행만 변경
    

#### 🔸 인덱스를 특정 컬럼으로 지정

```python
df.index = df['math']
```

- ❌ 원본 수정됨
    

```python
df.set_axis(df['eng'], axis=0)  # 원본 유지
```

---

### ✅ 3. 인덱스 이름 설정

```python
df.index.name = '학생이름'
```

---

### ✅ 4. 인덱스 초기화 (reset_index)

```python
df.reset_index(drop=True, inplace=True)
```

- drop=True → 인덱스를 열로 추가하지 않음
    
- inplace=True → 원본 df 변경
    

---

### ✅ 5. 결측치 다루기 (NA, nan)

#### 🔸 결측치 넣기

```python
df['dvi'] = [pd.NA, np.nan, 40, 90]
```

#### 🔸 결측치 확인

```python
df.isna()
df.dvi.isnull()
```

#### 🔸 결측치 개수

```python
df.dvi.isna().sum()
```

#### 🔸 평균 계산

```python
df.dvi.mean()  # NaN은 자동 제외됨
```

---

### ✅ 6. 데이터 구조/타입 확인

```python
df.info()       # 구조
df.dtypes       # 전체 열 타입
df['kor'].dtype # 특정 시리즈의 dtype
```

---

### ✅ 7. 열/행 제거 (`drop()`)

```python
df.drop(['math'], axis=1)  # 열 삭제
df.drop([0, 2], axis=0)    # 행 삭제
```

> ✅ 바로 출력하면 원본은 유지됨  
> ❌ 할당 안 하면 df 자체는 안 바뀜

---

### ✅ 8. 결측치 제거 (`dropna()`)

```python
df.dropna(axis=1)  # 결측치 있는 열 제거
df.dropna(axis=0)  # 결측치 있는 행 제거
```

---

### ✅ 9. 정렬 (`sort_values()`)

```python
df.sort_values(by='eng')                  # 오름차순
df.sort_values(by='math', ascending=False) # 내림차순
df.sort_values(by='eng').reset_index(drop=True)
```

---
### ✅ 10. 실전 CSV 불러오기 & 구조 보기 (FIFA25 예제)

#### 🔸 CSV 불러오기

```python
df = pd.read_csv('https://raw.githubusercontent.com/tysep16/DV25_1/refs/heads/main/fifa25.csv')
```

#### 🔸 데이터 구조 확인

```python
df.info()
df.columns
df.dtypes
```

#### 🔸 컬럼명에 공백이 있다면?

공백을 `_`로 바꾸기

- 방법 1: 컴프리헨션
    

```python
df.columns = [col.replace(' ', '_') for col in df.columns]
```

- 방법 2: `set_axis()`
    

```python
df.set_axis([col.replace(' ', '_') for col in df.columns], axis=1)
```

- 방법 3: `rename()` + dict comprehension
    

```python
df.rename({col: col.replace(' ', '_') for col in df.columns}, axis=1)
```

---

### ✅ 11. 결측치 50% 이상인 열 제거

#### 🔸 조건에 해당하는 열 보기

```python
df.loc[:, df.isna().sum(axis=0) / len(df) > 0.5]
```

#### 🔸 해당 열 제거

```python
df.drop(df.columns[df.isna().sum(axis=0)/len(df) > 0.5], axis=1)
```

---

### ✅ 12. 데이터 조건 추출 – `query()`

```python
df.query('math > 50 and eng > 50')
df.query('math > eng > com')
df.query('(math + eng)/2 > 50 and kor == "pass"')
df.query("index < 2 and (math + eng)/2 > 50 and kor == 'pass'")
df.query("math > math.mean()")
```

> ✅ 문자열 형태로 SQL처럼 쓸 수 있음  
> ✅ `.query()`는 코드 깔끔하게 쓸 때 유용함

---

### ✅ 13. 컬럼 계산 – `[]`, `assign()`, `eval()`

#### 🔸 방법 1: `[]` 이용 (가장 기본)

```python
df['total'] = df.math*0.3 + df.eng*0.2 + df.sci*0.1 + df.com*0.4
```

- ❌ 원본 변경됨
    

#### 🔸 방법 2: `assign()` 이용

```python
df.assign(total = df.math*0.3 + df.eng*0.2 + df.sci*0.1 + df.com*0.4)
```

- ✔️ 원본 유지
    
- ✔️ 결과만 출력됨
    

#### 🔸 방법 3: `eval()` 이용

```python
df.eval('total = math*0.3 + eng*0.2 + sci*0.1 + com*0.4')
```

- ✔️ 문자열로 계산식 표현
    
- ✔️ assign처럼 사용
    

---

### ✅ 14. Lambda 함수 & 고차함수

---

#### 🔸 lambda 기초

```python
f = lambda x: x + 1
f(2)  # 출력: 3

f = lambda x, y: x + y
f(1, 2)  # 출력: 3

f = lambda x: 1 if x == 'pass' else 0
f('pass'), f('fail')  # 출력: (1, 0)
```

---

### ✅ 15. `map()` 함수 사용

```python
list(map(lambda x: x - 10, df.com))  # 10점 빼기
[a - 10 for a in df.com]             # 동일 결과 (컴프리헨션)

# FIFA 예제 – 키(cm) 정수로 변환
df['Height'] = [int(h.split('cm')[0]) for h in df.Height]
```

---

### ✅ 16. `apply()` 함수 사용

```python
df.Height.apply(lambda x: int(x.split('cm')[0]))
df.Height.apply(lambda x: x.split('cm')[0]).apply(int)  # chaining
```

---

### ✅ 17. 속도 비교 (람다 vs 컴프리헨션 vs apply)

```python
import time

df = pd.DataFrame(np.random.randn(10_000_000, 2), columns=['a', 'b'])

# map + lambda
t1 = time.time()
df.assign(b = list(map(lambda x: x**2, df.a)))
print(time.time() - t1)

# 컴프리헨션
t1 = time.time()
df.assign(b = [x**2 for x in df.a])
print(time.time() - t1)

# apply
df.a.iloc[:100].apply(lambda x: x**2)
pd.Series([a**2 for a in df.a.iloc[:100]])
```

> 💡 속도: 컴프리헨션 > map > apply (보통은)