### 🧮 NumPy 관련 메서드

- `np.array()`
    
- `np.arange()`
    
- `np.linspace()`
    
- `np.ones()`, `np.zeros()`
    
- `np.random.random()`, `np.random.rand()`, `np.random.randint()`, `np.random.randn()`
    
- `np.reshape()`, `.flatten()`
    
- `np.linalg.inv()` (역행렬)
    
- `np.mean()`, `np.std()`, `np.sum()`, `np.max()`, `np.min()`, `np.prod()`
    
- `np.concatenate()`, `np.vstack()`, `np.hstack()`
    

---

### 🐼 Pandas 관련 메서드

- `pd.Series()`, `pd.DataFrame()`
    
- `df.index`, `df.columns`, `df.values`
    
- `df.T` (transpose)
    
- `df.loc[]`, `df.iloc[]`
    
- `df.mean()`, `df.sum()`, `df.max()`, `df.min()`, `df.std()`, `df.prod()`
    
- `df['col'] > 값` → 불리언 인덱싱
    
- `df[['col1', 'col2']]` → 열 선택
    
- `df[df['col'] > 값]` → 행 필터링
    

---

### 📈 시각화 (Matplotlib)

- `plt.plot()`
    
- `plt.scatter()`
    
- `plt.bar()`
    
- `plt.hist()`
    
- `plt.boxplot()`
    
- `plt.subplot()` / `plt.subplots()`
    
- `plt.title()`, `plt.xlabel()`, `plt.ylabel()`


## 🎯 **시험 대비 필수 코드 리스트 (Iris CSV 기반)**

### 📌 1. CSV 파일 불러오기

```python
import pandas as pd

df = pd.read_csv("iris.csv")  # 혹은 시험 환경에 맞게 경로 조정
```

---

### 📌 2. 기본 탐색

```python
df.head()          # 처음 5개 행 보기
df.info()          # 데이터 타입, 결측치 확인
df.describe()      # 수치 요약 통계
df.columns         # 칼럼명 확인
```

---

### 📌 3. 칼럼 이름 처리 (예: " (cm)" 제거)

```python
df.columns = [col.replace(" (cm)", "") for col in df.columns]
```

---

### 📌 4. 고유값 & 그룹 확인

```python
df['Species'].unique()        # ['setosa', 'versicolor', 'virginica']
df['Species'].value_counts()  # 각 종의 개수
```

---

### 📌 5. 조건 필터링

```python
df[df['SepalLength'] > 6]             # 조건 필터링
df[df['Species'] == 'setosa']         # setosa만 보기
df[(df['SepalWidth'] > 3) & (df['PetalLength'] < 2)]
```

---

### 📌 6. 그룹별 통계

```python
df.groupby('Species').mean()          # 각 품종별 평균
df.groupby('Species').agg(['mean', 'std'])  # 평균 + 표준편차
```

---

### 📌 7. 새로운 열 만들기

```python
df['SepalArea'] = df['SepalLength'] * df['SepalWidth']
```

---

### 📌 8. 정렬

```python
df.sort_values(by='PetalLength', ascending=False)
```

---

### 📌 9. 시각화 (선택 가능)

```python
import matplotlib.pyplot as plt

# 산점도
plt.scatter(df['SepalLength'], df['SepalWidth'])
plt.title('Sepal Length vs Width')
plt.xlabel('SepalLength')
plt.ylabel('SepalWidth')
plt.show()

# 품종별 평균 SepalLength bar plot
df.groupby('Species')['SepalLength'].mean().plot(kind='bar')
plt.title('Average Sepal Length per Species')
plt.show()
```

---

### 📌 10. iloc / loc 연습

```python
df.iloc[0]           # 첫 번째 행
df.iloc[:, 0:2]      # 첫 2개 열
df.loc[0:4, 'Species']  # 첫 5개 행의 Species 컬럼
```
