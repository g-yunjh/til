메트릭 데이터의 구조  
	- Prometheus는 **시계열 데이터**로 메트릭을 저장합니다.  
	- 각 메트릭은 이름과 라벨(Label)로 구분됩니다.  
	- 예:  
	```
	node_cpu_seconds_total{mode="idle", instance="node1"}
	```

- PromQL 연산자  
	- **산술 연산자**: `+`, `-`, `*`, `/`  
	- **비교 연산자**: `>`, `<`, `==`, `!=`  
	- **논리 연산자**: `and`, `or`, `unless`

- PromQL 데이터 타입  
	- **Instant Vector**: 특정 시점의 데이터 집합  
	- **Range Vector**: 일정 기간의 데이터 집합  
	- **Scalar**: 단일 숫자 값  
	- **String**: 문자열 데이터

- PromQL 함수  
	- `rate()`: 카운터형 메트릭의 증가율 계산  
	- `sum()`, `avg()`, `max()`, `min()` 등 집계 함수  
	- 예:  
	```promql
	rate(container_cpu_usage_seconds_total[5m])
	```

- 서머리와 히스토그램  
	- **서머리**: 분포형 데이터에 대한 백분위수(예: p95) 계산에 사용  
	- **히스토그램**: 범위별 데이터 수집으로 요청 시간, 트래픽 분포 등을 시각화할 때 사용
