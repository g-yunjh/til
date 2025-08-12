### 🔹 1. 주식 데이터 불러오기

```python
import FinanceDataReader as fdr

# [패턴] 종목코드, 연도
df = fdr.DataReader('AAPL', '2025')  # 애플
df = fdr.DataReader('TSLA', '2025')  # 테슬라
df = fdr.DataReader('NVDA', '2025')  # 엔비디아
```

---

### 🔹 2. 수익률 계산 (`pct_change`)

```python
# 종가 수익률 계산 (전일 대비 % 변화)
df['pct'] = df['Close'].pct_change() * 100
```

✔️ 주가 변화율은 시험에 무조건 나올 가능성 높음!

---

### 🔹 3. 이동평균선 (rolling)

```python
df['ma5'] = df['Close'].rolling(5).mean()    # 5일 평균
df['ma10'] = df['Close'].rolling(10).mean()  # 10일 평균
```

✔️ `.rolling(숫자).mean()` 형식 기억!

---

### 🔹 4. 시각화 (`matplotlib`, `df.plot`)

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(10, 6))

# 시리즈 단일 그래프
df['Close'].plot(ax=ax)       # 종가
df['ma5'].plot(ax=ax)         # 5일 평균
df['ma10'].plot(ax=ax)        # 10일 평균

ax.set_title('Stock Title')
ax.set_xlabel('date')
ax.set_ylabel('price')
plt.show()
```

또는 아래처럼 `df.plot(y='컬럼이름', ax=ax)` 사용 가능:

```python
df.plot(y='pct', ax=ax)
```

---

### 🔹 5. 시계열 조작 필수 함수 요약

| 함수명                             | 설명              |
| ------------------------------- | --------------- |
| `df['Close'].pct_change()`      | 전일 대비 수익률       |
| `df['Close'].shift(1)`          | 하루 전 종가 (시프트)   |
| `df['Close'].diff()`            | 종가 차이 (당일 - 전일) |
| `df['Close'].rolling(5).mean()` | 이동 평균           |

---

## 🔁 실습 흐름 패턴 익히기

### ✅ 수익률 그래프

```python
df = fdr.DataReader('AAPL', '2025')
df['pct'] = df['Close'].pct_change() * 100

fig, ax = plt.subplots(figsize=(10, 6))
df.plot(y='pct', ax=ax)
ax.set_title('apple stock pct')
ax.set_xlabel('date')
ax.set_ylabel('pct')
plt.show()
```

### ✅ 이동평균선 포함 종가 그래프

```python
df = fdr.DataReader('NVDA', '2025')
df['ma5'] = df['Close'].rolling(5).mean()
df['ma10'] = df['Close'].rolling(10).mean()

fig, ax = plt.subplots(figsize=(10, 6))
df['Close'].plot(ax=ax)
df['ma5'].plot(ax=ax)
df['ma10'].plot(ax=ax)

ax.set_title('nvidia stock price')
ax.set_xlabel('date')
ax.set_ylabel('close')
plt.show()
```

---

### 🔹 1. 주가 데이터 불러오기 (`FinanceDataReader`)

```python
import FinanceDataReader as fdr

df = fdr.DataReader('005930', '2025')  # 삼성전자
df = fdr.DataReader('030200', '2025')  # KT
```

---

### 🔹 2. 캔들차트용 전처리 (문자형 날짜 + 색상 지정)

```python
# 문자열 날짜 (x축 라벨용)
df['str_date'] = df.index.strftime('%m.%d')

# 상승/하락 구분 색상 (몸통 기준)
df['color'] = df['Close'] - df['Open']
df['color'] = df['color'].apply(lambda x: 'r' if x > 0 else 'b')
```

---

### 🔹 3. 이동평균선 만들기 (`rolling.mean()`)

```python
df['MA5'] = df['Close'].rolling(5).mean()
df['MA10'] = df['Close'].rolling(10).mean()
df['MA20'] = df['Close'].rolling(20).mean()
```

---

### 🔹 4. 캔들차트 시각화 (`matplotlib`)

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(nrows=2, ncols=1, figsize=(12, 6), gridspec_kw={'height_ratios': [5, 1]})

# ▶ 캔들 몸통 (시가-종가 기준 막대)
ax[0].bar(df['str_date'], height=df['Close'] - df['Open'], bottom=df['Open'], width=0.8, color=df['color'])

# ▶ 고가~저가 선
ax[0].vlines(df['str_date'], df['Low'], df['High'], colors=df['color'])

# ▶ 이동평균선
ax[0].plot(df['str_date'], df['MA5'], color='red', linewidth=1)
ax[0].plot(df['str_date'], df['MA10'], color='orange', linewidth=1)
ax[0].plot(df['str_date'], df['MA20'], color='green', linewidth=1)

ax[0].set_title('kt stock')
ax[0].set_xticks([])

# ▶ 거래량 그래프
ax[1].bar(df['str_date'], df['Volume'], width=0.8, color=df['color'])
ax[1].set_xticks(df['str_date'][::10])
ax[1].set_xticklabels(df['str_date'][::10], rotation=90)

plt.tight_layout()
plt.show()
```

---

### 🔹 5. 캔들차트 시각화 (`plotly`)

```python
from plotly.subplots import make_subplots
import plotly.graph_objects as go

fig = make_subplots(rows=2, cols=1, shared_xaxes=True)

# ▶ 캔들스틱 trace
fig.add_trace(go.Candlestick(
    x=df['str_date'], open=df['Open'], high=df['High'],
    low=df['Low'], close=df['Close'],
    increasing_line_color='red', decreasing_line_color='blue',
    name='stock'), row=1, col=1)

# ▶ 이동평균선 trace
fig.add_trace(go.Scatter(x=df['str_date'], y=df['MA5'], name='MA5'), row=1, col=1)
fig.add_trace(go.Scatter(x=df['str_date'], y=df['MA10'], name='MA10'), row=1, col=1)
fig.add_trace(go.Scatter(x=df['str_date'], y=df['MA20'], name='MA20'), row=1, col=1)

# ▶ 거래량 trace
fig.add_trace(go.Bar(x=df['str_date'], y=df['Volume'], name='volume'), row=2, col=1)

# ▶ 기타 설정
fig.update_layout(xaxis_rangeslider_visible=False)
fig.update_layout(title_text='kt stock')
fig.show()
```

---

### 🔹 1. `folium` 설치 및 기본 설정

```bash
pip install folium
```

```python
import folium
import pandas as pd
import json
import requests
```

---

### 🔹 2. 지도 기본 생성 (`folium.Map`)

```python
m = folium.Map(
    location=[36, 128],       # 중심 좌표 (위도, 경도)
    zoom_start=7,             # 확대 정도
    scrollWheelZoom=False     # 스크롤 확대 여부
)
m
```

---

### 🔹 3. 마커 추가 (`folium.Marker`)

```python
folium.Marker(location=[37.5882, 126.9920]).add_to(m)  # 서울 예시
```

---

### 🔹 4. 영역 표시 (폴리곤) - `folium.Polygon`

```python
coords = [[37.58, 126.99], [37.59, 126.99], [37.59, 127.00], [37.58, 127.00]]
folium.Polygon(locations=coords).add_to(m)
```

---

### 🔹 5. GeoJSON 행정구역 경계 데이터 불러오기

```python
# 시도 단위
prov_dict = json.loads(requests.get(
    'https://raw.githubusercontent.com/southkorea/southkorea-maps/master/kostat/2018/json/skorea-provinces-2018-geo.json'
).text)

# 시군구 단위
muni_dict = json.loads(requests.get(
    'https://raw.githubusercontent.com/southkorea/southkorea-maps/master/kostat/2018/json/skorea-municipalities-2018-geo.json'
).text)
```

---

### 🔹 6. Choropleth 기본 형태 (지도 + 통계 데이터)

```python
folium.Choropleth(
    geo_data=prov_dict,              # GeoJSON 형식의 경계선 정보
    data=df,                         # 통계 데이터 (pandas DataFrame)
    key_on='properties.name',        # GeoJSON에서 매핑할 필드
    columns=['행정구역(시군구)별','총인구수 (명)']  # 데이터프레임 기준 (매핑 키, 값)
).add_to(m)
```

---

### 🔹 7. 실습 예제: 시도별 인구 수 시각화

```python
# 1. CSV 불러오기
df = pd.read_csv('https://raw.githubusercontent.com/tysep16/DV25_1/refs/heads/main/2021-11-22-prov.csv')

# 2. 지도 생성
m = folium.Map(location=[36,128], zoom_start=7, scrollWheelZoom=False)

# 3. Choropleth 시각화
folium.Choropleth(
    geo_data=prov_dict,
    key_on='properties.name',
    data=df,
    columns=['행정구역(시군구)별', '총인구수 (명)']
).add_to(m)

# 4. 지도 출력
m
```

---

### 🔹 8. 실습 예제: 시군구 단위 인구 수 시각화 (과제)

```python
# 1. CSV 불러오기
df = pd.read_csv('https://raw.githubusercontent.com/tysep16/DV25_1/refs/heads/main/2021-11-22-muni.csv')

# 2. 지도 생성
m = folium.Map(location=[36,128], zoom_start=7, scrollWheelZoom=False)

# 3. Choropleth 시각화
folium.Choropleth(
    geo_data=muni_dict,
    key_on='properties.name',
    data=df,
    columns=['행정구역(시군구)별', '총인구수 (명)']
).add_to(m)

# 4. 지도 출력
m
```

---

### 🔹 1. 지도 기반 Choropleth (Plotly 버전)

### 📌 사용 라이브러리

```python
import pandas as pd
import numpy as np
import json
import requests
import plotly.express as px
```

---

### 🔹 2. GeoJSON 데이터 불러오기 (행정구역 경계)

```python
# 시도 단위
prov_dict = json.loads(requests.get(
    'https://raw.githubusercontent.com/southkorea/southkorea-maps/master/kostat/2018/json/skorea-provinces-2018-geo.json'
).text)

# 시군구 단위
muni_dict = json.loads(requests.get(
    'https://raw.githubusercontent.com/southkorea/southkorea-maps/master/kostat/2018/json/skorea-municipalities-2018-geo.json'
).text)
```

---

### 🔹 3. 에너지 데이터 전처리

```python
# 데이터 불러오기
df = pd.read_csv('https://raw.githubusercontent.com/tysep16/DV25_1/refs/heads/main/w13.csv')
df.drop(['Unnamed: 0'], axis=1, inplace=True)

# 문자열 숫자 → 정수 변환
df['에너지사용량(TOE)/전기'] = df['에너지사용량(TOE)/전기'].str.replace(',', '').astype(int)
```

---

### 🔹 4. 한글 시도명 변환 (Lookup 딕셔너리)

```python
# 영문 → 한글 이름 매핑
lookup = {x['properties']['name_eng']: x['properties']['name'] for x in prov_dict['features']}
df = df.assign(시도=[lookup[x] for x in df['시도']])
```

---

### 🔹 5. 시도별 전기 총합 계산

```python
df['시도별전기사용'] = [df[df['시도'] == x]['에너지사용량(TOE)/전기'].sum() for x in df['시도']]
```

---

### 🔹 6. Plotly Choropleth 시각화

```python
px.choropleth_map(
    geojson=prov_dict,
    featureidkey='properties.name',      # GeoJSON 속성
    data_frame=df,
    locations='시도',                    # DataFrame 컬럼
    color='시도별전기사용',               # 시각화할 값
    center={"lat": 36, "lon": 127.5},
    zoom=6,
    height=800,
    width=800
)
```

---

## ✅ 추가 실습: NYC 택시 데이터 분석

---

### 🔹 1. 명소 위치 데이터 → Scatter Map

```python
landmarks = {
    "Name": ["Wall Street", "Times Square", "Central Park"],
    "Latitude": [40.7074, 40.7580, 40.785091],
    "Longitude": [-74.0113, -73.9855, -73.968285]
}
df_landmarks = pd.DataFrame(landmarks)

fig = px.scatter_map(data_frame=df_landmarks, lat='Latitude', lon='Longitude', hover_data='Name',
                     zoom=10, width=750, height=600)
fig.update_traces(marker={'size': 15, 'color': 'red', 'opacity': 0.5})
fig.show()
```

---

### 🔹 2. NYC 택시 데이터 → 거리 & 밀도 분석

```python
# 데이터 불러오기
df = pd.read_csv('https://raw.githubusercontent.com/tysep16/DV25_1/refs/heads/main/nyc.csv')

# 시간 계산
df['duration'] = pd.to_datetime(df['dropoff_datetime']) - pd.to_datetime(df['pickup_datetime'])

# 거리 계산 (유클리디안)
df['dist'] = np.sqrt((df['pickup_latitude'] - df['dropoff_latitude'])**2 +
                     (df['pickup_longitude'] - df['dropoff_longitude'])**2)

# 밀도맵 시각화
fig = px.density_map(
    data_frame=df,
    lat='pickup_latitude',
    lon='pickup_longitude',
    radius=5,
    center={'lat': 40.7322, 'lon': -73.9052},
    z='dist',
    zoom=10,
    width=750,
    height=600
)
fig.show()
```
