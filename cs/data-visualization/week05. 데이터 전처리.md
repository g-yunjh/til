### ✅ 1. 결측치 처리 – `fillna()`

#### 🔸 fillna 주요 옵션

```python
df.fillna(value=값, method=‘ffill’ or ‘bfill’, axis=0 or 1, inplace=False)
```

| 옵션        | 의미                                   |     |
| --------- | ------------------------------------ | --- |
| `value`   | 지정 값으로 결측치 채움                        |     |
| `method`  | `ffill`: 앞 값으로 채움, `bfill`: 뒷 값으로 채움 |     |
| `inplace` | 원본 변경 여부                             |     |

---

### ✅ 2. 실습 예제: 결측치 채우기 연습

```python
import numpy as np
import pandas as pd
df = pd.DataFrame(np.round(np.random.rand(4, 4)*100), index=['a', 'b', 'c', 'd'])
df['dvi'] = [pd.NA, 30, 40, 90]
```

#### 일부 결측치 삽입

```python
df.iloc[1:3, 1] = [np.nan, np.nan]
df.loc['d', 'sci'] = pd.NA
```

#### 결측치 확인

```python
df.isnull()
df.isnull().sum(axis=0)
```

#### 결측치 제거

```python
df.dropna(axis=0)  # 행 제거
df.dropna(axis=1)  # 열 제거
```

---

### ✅ 3. 결측치 채우기

#### 전부 0으로 채우기

```python
df.fillna(0)
```

#### 특정 컬럼만 평균으로 채우기

```python
df['sci'].fillna(df['sci'].mean())
```

#### 딕셔너리로 여러 컬럼 동시 처리

```python
df.fillna({'sci': 0, 'dvi': 30})
```

#### 전/후 값으로 채우기

```python
df.ffill()  # 앞으로 채우기
df.bfill()  # 뒤로 채우기
```

---

### ✅ 4. groupby 집계 함수

#### 개념

- 특정 컬럼 기준으로 묶어서 집계 연산 수행 (`sum`, `mean`, `count`, `max`, `min`, ...)
    

---

### ✅ 5. Titanic 데이터로 실습

```python
df = pd.read_csv('https://raw.githubusercontent.com/tysep16/DV25_1/refs/heads/main/titanic.csv')
```

#### 전체 데이터 요약

```python
df.count()
df[['Age', 'Fare']].mean()
```

#### 그룹 기준 집계

```python
df.groupby('Pclass').count()               # 그룹별 전체 개수
df.groupby('Pclass')[['Survived', 'Fare']].mean()  # 그룹별 평균
df.groupby('Pclass')[['Survived', 'Fare']].sum()   # 그룹별 합계
```

#### 다양한 방법 비교

```python
# 방식 1: 직접 필터링
df[df.Pclass == 1]['Survived'].sum()

# 방식 2: query 사용
df.query('Pclass == 1')['Survived'].sum()

# 방식 3: apply + lambda
df.Pclass.apply(lambda x: x if x == 1 else 0).sum()
```

---

### ✅ 6. groupby + agg 다양한 방식

#### 단일 함수

```python
df.groupby('Pclass')[['Survived', 'Fare']].agg('mean')
```

#### 다중 함수 → 멀티 인덱스

```python
df.groupby('Pclass')[['Survived', 'Fare']].agg(['sum', 'mean'])
```

#### 딕셔너리로 함수 지정

```python
df.groupby('Pclass')[['Survived', 'Fare']].agg({'Survived': 'sum', 'Fare': 'mean'})
```

#### 새 컬럼 이름 지정 (추천)

```python
df.groupby('Pclass').agg(Survived_sum=('Survived', 'sum'), Fare_mean=('Fare', 'mean'))
```

---

### ✅ 7. 숫자 컬럼 계산 (브로드캐스팅)

#### 기본 연산

```python
df.Fare + 10         # 각 요소에 10 더하기
df.Fare * 1450       # 원화 환산 (환율 적용)
df.Fare.apply(lambda x: x * 1450)  # 동일 결과
```

> ✔️ 수치 연산은 브로드캐스팅처럼 바로 연산 가능  
> ✔️ apply/lambda는 가독성 떨어질 수 있음

---

### ✅ 8. 문자열 처리 – 이름에서 성 추출하기

#### 실패 예시 (에러 발생)

```python
df.Name.split(',')[0]  # ❌ 불가 (시리즈에는 split 없음)
```

#### 성공 방법

```python
df.Name.str.split(',').str[0]
```

#### 또는 리스트 컴프리헨션

```python
df.assign(Name = [x.split(',')[0] for x in df.Name])
```

---

### ✅ 9. `str` 속성으로 텍스트 다루기

- `.str`을 붙이면 문자열 함수들을 벡터화해서 사용 가능
    

```python
df.Name.str.upper()
df.Name.str.contains('Mrs')
df.Name.str.split(',').str[1]
```

---

### ✅ 10. FIFA2025 실습 예제 – 키(Height) 전처리

#### map + lambda 사용

```python
df.assign(Height = list(map(lambda x: int(x.split('cm')[0]), df.Height)))
```

#### str + apply 사용

```python
df.Height.str.split('cm').str[0].apply(int)
```

---

### ✅ 11. 컬럼 이름 처리 – 공백 제거

```python
df.columns.str.replace(' ', '_')
```

> ✔️ 리스트 컴프리헨션으로도 가능

```python
df.columns = [c.replace(' ', '_') for c in df.columns]
```

---

## ✅ 12. 다양한 파일 불러오기 방법

---

### 🔸 CSV 파일

```python
pd.read_csv('파일경로 또는 URL')
```

---

### 🔸 Excel 파일

```bash
!pip install openpyxl
```

```python
pd.read_excel('파일경로 또는 URL')  # openpyxl 필요
```

---

### 🔸 HTML 테이블 읽기

```bash
!pip install lxml html5lib
```

```python
pd.read_html('https://ko.wikipedia.org/wiki/대한민국의_저출산')  # 반환값은 list
```

```python
tables = pd.read_html('...')
tables[0]  # 첫 번째 테이블 출력
```

