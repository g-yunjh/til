### ✅ 1. 시각화란?

- **데이터 시각화 (Data Visualization)**:  
    데이터를 사람이 이해할 수 있는 **시각적 형태로 표현**하는 전략.
    
- 수치 → 도형, 색, 형태 등으로 표현해 **패턴, 이상치, 트렌드**를 직관적으로 파악.
    
- 시각화는 도구가 아닌 **전략**이다.
    

---

### ✅ 2. matplotlib 시작하기

#### 설치 & 임포트

```python
!pip install matplotlib
import matplotlib.pyplot as plt
```

#### 예제용 데이터 생성

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'A': [75, 75, 76, 76, 77, 77, 78, 79, 79, 98],
    'B': [76, 76, 77, 77, 78, 78, 79, 80, 80, 81]
})
```

---

### ✅ 3. 통계로 보기 vs 시각화로 보기

```python
df.describe()
df.mean()
```

- A반 평균이 더 높지만,
    
- **Boxplot**을 보면 B반이 안정적이고 성적이 몰려있다는 걸 시각적으로 확인 가능
    

---

## ✅ 4. 기본 시각화 – Box Plot

```python
plt.boxplot(df['A'])     # A반
plt.boxplot(df['B'])     # B반
plt.boxplot([df['A'], df['B']])  # 두 반 비교
plt.boxplot(df)          # 전체 열에 대해 boxplot
plt.show()
```

---

### ✅ 5. Line Plot (기본 라인 그래프)

```python
plt.plot([1, 4, 9, 16])
plt.title("line plot")
plt.xlabel("x label")
plt.ylabel("y label")
plt.show()
```

#### 옵션 커스터마이징

```python
plt.plot([1, 2, 3, 4], [1, 4, 9, 16],
         color='red', linewidth=2, linestyle='--',
         marker='o', ms=3, mew=10, mfc='blue')
plt.show()
```

#### 축 범위, 라벨, 그리드

```python
plt.xlim(0, 5)
plt.ylim(-5, 20)
plt.grid(True)
```

---

### ✅ 6. 여러 선 그리기

```python
t = np.arange(0, 5, 0.2)
plt.plot(t, t, 'r--', t, t**2, 'bs:', t, t**3, 'g^-')
plt.legend(loc=2)
plt.show()
```

---

### ✅ 7. `figure`와 `subplot`의 구조 이해

#### 기본 구조

- **Figure**: 도화지
    
- **Axes**: 개별 차트
    
- **Axis**: x, y 축
    

---

### ✅ 8. 여러 개의 subplot 만들기

#### 방법 1: `plt.subplot()`

```python
plt.subplot(221); plt.plot(np.random.rand(5)); plt.title("axes 1")
plt.subplot(222); plt.plot(np.random.rand(5)); plt.title("axes 2")
plt.subplot(223); plt.plot(np.random.rand(5)); plt.title("axes 3")
plt.subplot(224); plt.plot(np.random.rand(5)); plt.title("axes 4")
plt.tight_layout(); plt.show()
```

#### 방법 2: `plt.subplots()` (더 추천!)

```python
fig, axs = plt.subplots(2, 2)
axs[0, 0].plot(x, y)
axs[1, 1].plot(x, -y, c='red')
```

> ✅ `subplots()`는 **Figure + Axes 객체를 반환**하므로 더 유연하고 현대적인 방식!

---

### ✅ 9. 두 축 겹치기 – `twinx()`

```python
fig, ax0 = plt.subplots()
ax1 = ax0.twinx()

ax0.plot([10, 5, 2, 9, 7], 'r-', label="y0")
ax1.plot([100, 200, 220, 180, 120], 'g:', label="y1")
```

> ✔️ 한 그래프 안에 **두 개의 y축**을 표현하고 싶을 때 사용

---

## ✅ 10. 막대 그래프 (Bar Chart)

```python
x = range(3)
y = [2, 3, 1]

plt.bar(x, y)
plt.xticks(x, ['a', 'b', 'c'])
plt.xlabel("x axis")
plt.ylabel("y axis")
plt.title("Bar Chart")
plt.show()
```

#### 수평 막대 그래프

```python
plt.barh(y, x, alpha=0.2)
```

---

## ✅ 11. 히스토그램 (Histogram)

```python
x = np.random.randn(1000)
plt.hist(x, bins=10, color='lime', alpha=0.3)
plt.title("histogram")
plt.show()
```

---

## ✅ 12. 산점도 (Scatter Plot)

```python
X = np.random.rand(100)
Y = np.random.rand(100)

plt.scatter(X, Y)
plt.title("scatter plot")
plt.show()
```

#### 버블 차트 (크기 + 색상 정보 포함)

```python
N = 30
x = np.random.rand(N)
y1 = np.random.rand(N)
y2 = np.random.rand(N)
y3 = np.pi * (15 * np.random.rand(N))**2

plt.scatter(x, y1, c=y2, s=y3)
```

---

## ✅ 13. 박스플롯 복습 + 실전 시각화

```python
score = np.round(np.random.rand(100) * 100)
plt.boxplot(score)
plt.title("score boxplot")
plt.show()
```

---

## ✅ 14. 실전 – Titanic 시각화 예제

#### 타이타닉 데이터 불러오기

```python
df = pd.read_csv('https://raw.githubusercontent.com/tysep16/DV25_1/refs/heads/main/titanic.csv')
```

#### Age vs Fare: 산점도

```python
fig, ax = plt.subplots()
ax.scatter(df['Age'], df['Fare'])
ax.set_title("age vs fare")
```

#### 생존자 빈도: 히스토그램

```python
fig, ax = plt.subplots()
ax.hist(df['Survived'])
ax.set_title("Titanic_Survived")
```

#### 30세 이하 생존자 성별 비교: 박스플롯

```python
df1 = df[(df['Age'] <= 30) & (df['Sex'] == 'female') & (df['Survived'] == 1)]['Age']
df2 = df[(df['Age'] <= 30) & (df['Sex'] == 'male') & (df['Survived'] == 1)]['Age']

fig, ax = plt.subplots()
ax.boxplot([df1, df2], tick_labels=['Female', 'Male'])
```
