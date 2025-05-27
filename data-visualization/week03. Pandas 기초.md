### ✅ 1. 판다스(Pandas) 소개

#### 🔸 개념 설명

- 파이썬에서 R처럼 **데이터프레임(DataFrame)** 다룰 수 있는 패키지
    
- 넘파이 기반이지만 리스트가 아닌 **딕셔너리 기반** 설계
    
- 넘파이보다 느리지만, 구조적으로 데이터 분석에 훨씬 유리함
    
- **패널 데이터(PANel DAta)** 처리에 유용 (가로+세로 의미가 있는 데이터)
    

#### 🔸 설치 및 임포트

```python
!pip install pandas
import numpy as np
import pandas as pd
```

---

### ✅ 2. 시리즈(Series)

#### 🔸 개념 설명

- 넘파이의 1차원 배열과 유사하지만, **인덱스 라벨**이 있음
    
- `Series = 인덱스(index) + 값(value)`
    
- 벡터와 유사함
    

#### 🔸 시리즈 생성

```python
ps = pd.Series([90, 95, 80, 87, 85])
```

#### 🔸 출력 예시

```
0    90
1    95
2    80
3    87
4    85
dtype: int64
```

#### 🔸 다양한 시리즈 생성 예시

```python
# 인덱스 설정
ps = pd.Series([90, 95, 80, 87, 85], index=['a', 'b', 'c', 'd', 'e'])

# 딕셔너리 → 시리즈
pd.Series({'a': 90, 'b': 95, 'c': 80, 'd': 87, 'e': 85}, name='math')
```

#### 🔸 속성 확인

```python
ps.index         # Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
ps.values        # array([...])
ps.name = 'math' # 시리즈에 이름 지정
```

---

### ✅ 3. 시리즈 인덱싱/슬라이싱

#### 🔸 위치 기반 인덱싱 (주의 필요)

```python
ps[0], ps[1], ps[-1]
```

⚠️ **경고**: 앞으로는 위치는 `iloc`을 써야 안전!

#### 🔸 라벨 기반 인덱싱

```python
ps['a'], ps['e']
ps[['a', 'c', 'e']]
```

#### 🔸 슬라이싱

```python
ps['a':'c']     # a~c까지 (c 포함)
ps[:'d']        # 처음부터 d까지
```

#### 🔸 불리언 인덱싱

```python
ps[ps > 85]     # 조건 기반 필터링
ps[(ps > 85) & (ps < 95)]
```

#### 🔸 속성처럼 접근 (비추)

```python
ps.a, ps.d      # 가능은 하나 헷갈림 많음
ps.math         # ❌ 에러 발생 -> math라는 이름의 값이 실제로 존재하지 않음
```

---

### ✅ 4. 데이터프레임 생성

#### 🔸 기본 생성 (딕셔너리)

```python
dc = {
  'math': [65, 95, 65, 55, 80],
  'eng':  [55,100, 90, 80, 30],
  'sci':  [50, 50, 60, 75, 30],
  'com':  [40, 80, 30, 80,100]
}
df = pd.DataFrame(dc)
```

#### 🔸 인덱스 & 컬럼 라벨 설정

```python
df.index = ['a', 'b', 'c', 'd', 'e']
df.columns = ['math', 'eng', 'sci', 'kor']
df.index.name = 'students'
df.columns.name = 'mid_term'
```

#### 🔸 값과 구조 확인

```python
df.index, df.columns, df.values
df.T  # transpose
```

---
## ✅ 5. 데이터프레임 인덱싱 (feat. 열방향)

#### 🔸 열 선택

```python
df['math']        # Series 반환 (1차원)
df[['math']]       # DataFrame 반환 (2차원)
df[['math', 'eng']] # 복수 열 선택
```

> ✅ `df['math']`과 `df.math` 모두 가능하지만, 후자는 에러 가능성이 있어서 비추천  
> ✅ `df[['math']]` → 슬라이싱이나 열 방향 결합할 때 유용

#### 🔸 불리언 인덱싱

```python
df['math'] > 70
df[df['math'] > 70]           # 조건 만족하는 행 반환
df[df['math'] > 70]['math']   # 특정 열만 다시 추출
```

---

## ✅ 6. 데이터프레임 인덱싱 (feat. 행방향)

#### 🔸 직접 행 선택은 ❌

```python
df['a']   # ❌ 에러: 열로 인식함
```

#### 🔸 행 슬라이싱은 가능

```python
df['a':'c']   # 행 슬라이싱 가능 (인덱스 라벨 기준)
df[:2]        # 정수 슬라이싱도 가능 (a~b)
df[::2]       # 스트라이딩도 가능
```

#### 🔸 불리언 인덱싱으로 행 추출

```python
df[df.math < 70]
df[[True, False, True, False, True]]
```

---

## ✅ 7. iloc (정수 기반 인덱싱) – 넘파이처럼!

#### 🔸 열방향

```python
df.iloc[:, 0]        # 첫 번째 열
df.iloc[:, -1]       # 마지막 열
df.iloc[:, [0, 2]]   # 특정 열들
df.iloc[:, 0:3:2]    # 슬라이싱 + 스트라이딩
```

#### 🔸 행방향

```python
df.iloc[0]           # 첫 번째 행
df.iloc[[1, 3]]      # 2행, 4행
df.iloc[:3]          # 앞에서 3개 행
df.iloc[[f < 70 for f in df['kor']]]  # 조건문 적용
```

---

## ✅ 8. loc (라벨 기반 인덱싱)

#### 🔸 열방향

```python
df.loc[:, 'math']             # 단일 열
df.loc[:, 'math':'sci']       # 열 슬라이싱
df.loc[:, ['eng', 'sci']]     # 복수 선택
df.loc[:, [True, False, True, False]]  # 불리언
```

#### 🔸 행방향

```python
df.loc['a']                   # 라벨이 'a'인 행
df.loc[['a', 'c', 'f']]       # 여러 행 선택
df.loc['a':'c']               # 슬라이싱
df.loc['a'::2]                # 스트라이딩
df.loc[df.math < 70]          # 조건 적용
```

---

## ✅ 9. 정리 요약표

|인덱싱 유형|`.iloc[]` (정수)|`.loc[]` (라벨)|`[]` 직접 접근|
|---|---|---|---|
|단일 열 선택|✔️|✔️|✔️ (`df['col']`)|
|열 슬라이싱|✔️|✔️|❌|
|열 리스트 선택|✔️|✔️|✔️|
|행 단일 선택|✔️|✔️|❌|
|행 슬라이싱|✔️|✔️|✔️ (`df[:]`)|
|행 리스트 선택|✔️|✔️|❌|
|불리언 인덱싱|✔️|✔️|✔️|

> 💡 **추천:**
> 
> - 숫자 기반이면 `iloc[]`
>     
> - 라벨 기반이면 `loc[]`
>     
> - 그 외는 될 수도 있고 안 될 수도 있어서 혼란 방지 차원에서 피하는 게 좋아!
>     

---
## ✅ 10. 실전: CSV 파일 불러오기 & 탐색

---

### 🔸 a. 파일 불러오기

#### 📌 인터넷에서 직접 가져오기

```python
df = pd.read_csv('https://raw.githubusercontent.com/tysep16/DV25_1/main/data/movie.csv')
```

#### 📌 로컬 파일에서 불러오기

```python
df = pd.read_csv('./data/movie.csv')
```

---

### 🔸 b. 기본 탐색

#### 📌 구조 확인

```python
df.shape          # (4916, 28)
df.columns        # 컬럼명 확인
df.index          # 인덱스 확인
df.head()         # 앞 5개
df.tail()         # 뒤 5개
```

#### 📌 더 많이 보고 싶을 때

```python
pd.options.display.min_rows = 30
df  # 전체에서 앞/뒤 + 중간 생략 없이 일부 보여줌
```

---

### 🔸 c. 데이터프레임 구조 분석

```python
df.columns
```

출력:

```text
Index(['color', 'director_name', 'num_critic_for_reviews', 'duration',
       'director_facebook_likes', ..., 'movie_facebook_likes'],
      dtype='object')
```

---

### 🔸 d. 컬럼/행 선택

#### 🎬 감독 이름만 출력

```python
df['director_name']
df.director_name  # 가능하지만 비추천
```

#### 🎞 원하는 컬럼만 추출

```python
df[['movie_title', 'genres', 'imdb_score', 'movie_facebook_likes']]
```

> ✅ 원하는 순서로 컬럼 출력 가능!

```python
df[['imdb_score', 'movie_title', 'genres']]
df.loc[:, ['movie_title', 'genres', 'imdb_score']]
```

---

### 🔸 e. 전치(transpose)

```python
df.T
```

> ✅ 컬럼이 행이 되고, 행이 컬럼이 됨

---

### 🔸 f. 조건 필터링

```python
df[df['imdb_score'] > 8.0]
df[df['genres'].str.contains('Action')]
```
