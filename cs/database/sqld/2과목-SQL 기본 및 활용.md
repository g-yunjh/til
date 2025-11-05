# SQL 기본 + 활용 + 관리구문 (SQLD 2과목)

# Part 1: SQL 기본

## 1. 관계형 데이터베이스 개요

### 데이터베이스 관련 용어

#### 데이터베이스 (Database, DB)
- 데이터를 일정한 형태로 저장해 놓은 것
- 예: 엑셀도 하나의 데이터베이스

#### DBMS (Database Management System)
- 데이터 손상 방지 및 복구
- 인증된 사용자만 접근
- 추가 관리 기능 지원

#### RDBMS (Relational DBMS)
- 테이블로 데이터 관리
- 테이블 간 관계를 이용해 데이터 정의
- Oracle, MySQL, PostgreSQL 등

#### 테이블 (Table)
- RDBMS에서 실제 데이터가 저장되는 2차원 배열 형태의 저장소
- 엔터티 → 테이블
- 속성 → 컬럼 (Column)
- 인스턴스 → 튜플/행 (Row/Tuple)

```
직원 테이블 예시
┌──────────┬────────┬──────┬────────┬──────────┐
│  직원ID  │  이름  │ 나이 │  연봉  │  부서ID  │
├──────────┼────────┼──────┼────────┼──────────┤
│  A0001   │ 강태우 │  35  │  5000  │  D001    │
│  A0002   │ 김형준 │  32  │  4800  │  D001    │
│  A0003   │ 송송이 │  27  │  3400  │  D002    │
└──────────┴────────┴──────┴────────┴──────────┘
```

---

### SQL (Structured Query Language)

RDBMS에서 데이터 정의, 조작, 조회, 제어를 위한 언어

#### SQL 분류 ⭐️ 매우 중요!

| 분류 | 영문명 | 명령어 | 용도 |
|------|--------|--------|------|
| **DDL** | Data Definition Language<br>(데이터 정의어) | CREATE, ALTER, DROP,<br>RENAME, TRUNCATE | 테이블 생성/수정/삭제 |
| **DML** | Data Manipulation Language<br>(데이터 조작어) | SELECT, INSERT,<br>UPDATE, DELETE, MERGE | 데이터 조회/입력/수정/삭제 |
| **DCL** | Data Control Language<br>(데이터 제어어) | GRANT, REVOKE | 권한 부여/회수 |
| **TCL** | Transaction Control Language<br>(트랜잭션 제어어) | COMMIT, ROLLBACK,<br>SAVEPOINT | 트랜잭션 제어 |

---

### 집합 연산자와 관계 연산자

#### 일반 집합 연산자

| 연산자 | SQL 구현 | 설명 |
|--------|----------|------|
| UNION | UNION | 합집합 |
| INTERSECTION | INTERSECT | 교집합 |
| DIFFERENCE | MINUS (EXCEPT) | 차집합 |
| PRODUCT | CROSS JOIN | 카티션 곱 |

#### 순수 관계 연산자

| 연산자 | SQL 구현 | 설명 |
|--------|----------|------|
| SELECT | WHERE | 행 선택 |
| PROJECT | SELECT | 열 선택 |
| JOIN | JOIN | 테이블 결합 |
| DIVIDE | (사용 안 함) | - |

---

## 2. SELECT 문

### 기본 구조

```sql
SELECT 컬럼1, 컬럼2, ...
FROM 테이블명
WHERE 조건;
```

**실행 순서:**
1. FROM: 테이블에서 데이터 가져오기
2. WHERE: 조건에 맞는 행만 필터링
3. SELECT: 원하는 컬럼만 출력

---

### 주요 문법

#### (1) * (애스터리스크) - 전체 컬럼 조회

```sql
SELECT * FROM TB_PRD;
-- TB_PRD 테이블의 모든 컬럼 출력
```

#### (2) DISTINCT - 중복 제거

```sql
SELECT DISTINCT PRD_TYPE FROM TB_PRD;
-- PRD_TYPE 컬럼 값 중복 제거하여 출력
```

#### (3) AS (ALIAS) - 별칭 부여

```sql
SELECT CUST_ID AS C_ID
     , BIRTH_DY AS 생년월일
     , MONEY AS PAY_123
FROM TB_CUST;
```

**AS 사용 주의사항:**
- 띄어쓰기 불가
- 문자로 시작해야 함
- 예약어 사용 불가
- 특수문자는 $, _, # 만 가능

**AS 생략 가능:**
```sql
SELECT CUST_ID C_ID, BIRTH_DY 생년월일
FROM TB_CUST;
```

**특수문자/띄어쓰기 포함 시 큰따옴표 사용:**
```sql
SELECT CUST_ID AS "고객 ID", MONEY AS "보유$금액"
FROM TB_CUST;
```

#### (4) 연산자

**사칙연산:**
```sql
SELECT CUST_NAME
     , MONEY
     , MONEY + 100 AS 증가금액
     , MONEY * 1.1 AS "10%인상"
FROM TB_CUST;
```

**연결 연산자 (||):**
```sql
SELECT CUST_NAME || '님' AS 고객명
     , '보유금액: ' || MONEY || '원' AS 금액정보
FROM TB_CUST;
```

---

## 3. 함수

### 함수의 특징

1. 블랙박스처럼 작동 (내부 구조 몰라도 됨)
2. 입력 개수와 출력 개수는 함수마다 다름
3. **입력, 출력, 기능(What)만 알면 사용 가능**

```
입력값 → [함수 기능] → 출력값
  3, 10 → [더하기] → 13
 'AAAAA' → [소문자변환] → 'aaaaa'
```

---

### 문자형 함수

#### UPPER / LOWER

```sql
-- 대문자 변환
SELECT UPPER('abcde123@@') AS UPPER사용
FROM DUAL;
-- 결과: ABCDE123@@

-- 소문자 변환
SELECT LOWER('ABCDE123@@') AS LOWER사용
FROM DUAL;
-- 결과: abcde123@@
```

#### SUBSTR - 문자열 자르기

```sql
-- SUBSTR(문자열, 시작위치, 길이)
SELECT PRD_NAME
     , PRD_DETAIL
     , SUBSTR(PRD_DETAIL, 1, 5) || '...' AS 상품설명생략
FROM TB_PRD
WHERE PRD_TYPE = '가전';
```

#### TRIM - 공백 제거

```sql
-- 양 끝 공백 제거 (중간 공백은 제거 안 됨)
SELECT TRIM('  안녕하세요  ') AS 결과1
     , TRIM('안 녕 하 세 요') AS 결과2
FROM DUAL;
-- 결과1: '안녕하세요'
-- 결과2: '안 녕 하 세 요'
```

#### REPLACE - 문자 치환

```sql
-- REPLACE(문자열, 바뀔값, 바꿀값)
SELECT PRD_DETAIL
     , REPLACE(PRD_DETAIL, ' ', '') AS 공백제거
FROM TB_PRD;
```

---

### 숫자형 함수

#### ROUND - 반올림

```sql
-- ROUND(실수, 소수점자릿수)
SELECT ROUND(1.452, 2) AS 결과1  -- 1.45
     , ROUND(1.452, 1) AS 결과2  -- 1.5
     , ROUND(1.452, 0) AS 결과3  -- 1
FROM DUAL;
```

#### MOD - 나머지

```sql
SELECT MOD(10, 3) AS 나머지  -- 1
FROM DUAL;
```

---

### 날짜형 함수

#### SYSDATE / GETDATE()

```sql
-- Oracle
SELECT SYSDATE FROM DUAL;

-- SQL Server
SELECT GETDATE();
```

---

### 형변환 함수

```
        TO_CHAR()
문자형 ←----------→ 숫자형
   ↑                 ↓
   |   TO_CHAR()     |
   |                 |
   └─────────────────┘
         ↑       ↓
    TO_DATE() TO_NUMBER()
         날짜형
```

#### 형변환 우선순위
**날짜형 > 숫자형 > 문자형**

```sql
-- 자동 형변환 (문자→숫자)
SELECT '100' + 200 FROM DUAL;  -- 300

-- 강제 형변환
SELECT TO_NUMBER('100') + 200 FROM DUAL;  -- 300
SELECT TO_CHAR(100) FROM DUAL;  -- '100'
```

#### 날짜 형변환

```sql
-- 날짜 → 문자
SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD HH24:MI:SS') FROM DUAL;
-- 결과: 2023/01/01 14:30:45

SELECT TO_CHAR(SYSDATE, 'YYYYMMDD') FROM DUAL;
-- 결과: 20230101

-- 문자 → 날짜
SELECT TO_DATE('20230101', 'YYYYMMDD') FROM DUAL;
SELECT TO_DATE('20230101141212', 'YYYYMMDD HH24MISS') FROM DUAL;
```

**날짜 포맷 기호:**
- YYYY: 년 (4자리)
- MM: 월 (2자리)
- DD: 일 (2자리)
- HH24: 시간 (24시간 형식)
- MI: 분
- SS: 초

---

### NULL 함수

#### NULL의 특징

```sql
SELECT 3 + NULL FROM DUAL;      -- NULL
SELECT 3 * NULL FROM DUAL;      -- NULL
SELECT 3 = NULL FROM DUAL;      -- NULL
```

> **중요**: NULL과의 모든 연산 결과는 NULL!

#### NVL - NULL 대체

```sql
-- NVL(data1, data2)
-- data1이 NULL이면 data2 반환, 아니면 data1 반환

SELECT CUST_NAME
     , NVL(MONEY, 0) AS 보유금액
FROM TB_CUST;
```

#### NVL2 - NULL 여부에 따라 다른 값 반환

```sql
-- NVL2(data1, data2, data3)
-- data1이 NULL이 아니면 data2, NULL이면 data3

SELECT CUST_NAME
     , NVL2(MONEY, '보유', '없음') AS 금액여부
FROM TB_CUST;
```

#### COALESCE - 여러 값 중 첫 번째 NOT NULL 값

```sql
-- COALESCE(data1, data2, data3, ...)
-- 처음으로 NULL이 아닌 값 반환

SELECT COALESCE(NULL, NULL, 100, 200) FROM DUAL;  -- 100
```

#### DECODE - 조건 분기

```sql
-- DECODE(data1, data2, data3, data4)
-- data1 = data2이면 data3, 아니면 data4

SELECT CUST_NAME
     , DECODE(MONEY, 0, '없음', '있음') AS 금액여부
FROM TB_CUST;

-- 다중 조건
SELECT CUST_NAME
     , DECODE(부서ID, 'D001', '인사부'
                     , 'D002', '급여부'
                     , 'D003', '개발부'
                     , '기타') AS 부서명
FROM TB_CUST;
```

---

### CASE 문

```sql
CASE WHEN 조건1 THEN 결과1
     WHEN 조건2 THEN 결과2
     ...
     ELSE 기본결과
END
```

**예시: 포인트 등급 부여**

```sql
SELECT CUST_NAME
     , ACT_POINT
     , CASE WHEN ACT_POINT BETWEEN 0 AND 1000 THEN '브론즈'
            WHEN ACT_POINT BETWEEN 1001 AND 10000 THEN '실버'
            WHEN ACT_POINT BETWEEN 10001 AND 100000 THEN '골드'
            ELSE '마스터'
       END AS 등급
FROM TB_CUST;
```

---

## 4. WHERE 절

### WHERE절의 역할

```sql
SELECT * FROM TB_CUST WHERE CUST_ID = 'C0001';
```

**실행 순서:**
1. TB_CUST 테이블 가져오기
2. WHERE 조건에 맞는 행 필터링
3. SELECT 컬럼 출력

---

### 비교 조건

#### 동등 조건 (=)

```sql
-- PRD_TYPE이 '컴퓨터'인 상품
SELECT * FROM TB_PRD 
WHERE PRD_TYPE = '컴퓨터';
```

#### 비동등 조건 (>, <, >=, <=, !=, <>)

```sql
-- 가격이 1,000,000 이상인 상품
SELECT * FROM TB_PRD 
WHERE PRD_AMT >= 1000000;

-- 가격이 500,000이 아닌 상품
SELECT * FROM TB_PRD 
WHERE PRD_AMT != 500000;  -- 또는 <>
```

---

### 논리 조건

#### AND - 모든 조건 만족

```sql
SELECT * FROM TB_CUST
WHERE CUST_ID = 'C0001'
  AND PASSWD = 'pass111';
```

#### OR - 하나 이상 만족

```sql
SELECT * FROM TB_PRD
WHERE PRD_TYPE = '컴퓨터'
   OR PRD_TYPE = '스마트폰';
```

---

### 부정 연산 (NOT)

```sql
-- 가전이 아닌 상품
SELECT * FROM TB_PRD
WHERE PRD_TYPE != '가전';

-- 또는
SELECT * FROM TB_PRD
WHERE NOT PRD_TYPE = '가전';
```

---

### NULL 연산

```sql
-- NULL인 데이터
SELECT * FROM TB_CUST
WHERE MONEY IS NULL;

-- NULL이 아닌 데이터
SELECT * FROM TB_CUST
WHERE MONEY IS NOT NULL;
```

> **주의**: `= NULL` 또는 `!= NULL` 사용 불가!

---

### IN 연산자

```sql
-- 여러 값 중 하나와 일치
SELECT * FROM TB_PRD
WHERE PRD_TYPE IN ('가전', '욕실용품', '스마트폰');

-- 위 쿼리는 아래와 동일
SELECT * FROM TB_PRD
WHERE PRD_TYPE = '가전'
   OR PRD_TYPE = '욕실용품'
   OR PRD_TYPE = '스마트폰';
```

#### NOT IN

```sql
-- 제외할 값들
SELECT * FROM TB_PRD
WHERE PRD_TYPE NOT IN ('가전', '욕실용품');
```

---

### BETWEEN 연산자

```sql
-- 범위 조건 (경계값 포함)
SELECT * FROM TB_PRD
WHERE PRD_AMT BETWEEN 100000 AND 500000;

-- 위 쿼리는 아래와 동일
SELECT * FROM TB_PRD
WHERE PRD_AMT >= 100000
  AND PRD_AMT <= 500000;
```

---

### LIKE 연산자 (패턴 매칭)

#### 와일드카드

- `%`: 0개 이상의 문자
- `_`: 정확히 1개의 문자

```sql
-- '수'로 시작하는 상품
SELECT * FROM TB_PRD
WHERE PRD_NAME LIKE '수%';

-- '용'이 포함되는 상품
SELECT * FROM TB_PRD
WHERE PRD_TYPE LIKE '%용%';

-- '기'로 끝나는 상품
SELECT * FROM TB_PRD
WHERE PRD_NAME LIKE '%기';

-- 3글자이면서 '기'로 끝나는 상품
SELECT * FROM TB_PRD
WHERE PRD_NAME LIKE '__기';
```

---

## 5. GROUP BY, HAVING 절

### 집계함수 (Aggregate Functions)

**특징:**
- 다중행 함수 (여러 행 → 1개 결과)
- NULL 값은 무시

#### COUNT - 행 개수

```sql
-- 전체 행 개수 (NULL 포함)
SELECT COUNT(*) FROM TB_CUST;

-- 특정 컬럼 (NULL 제외)
SELECT COUNT(MONEY) FROM TB_CUST;
```

#### SUM - 합계

```sql
SELECT SUM(MONEY) AS 총금액
FROM TB_CUST;
```

#### AVG - 평균

```sql
SELECT AVG(MONEY) AS 평균금액
FROM TB_CUST;
```

#### MAX / MIN - 최대/최소

```sql
SELECT MAX(MONEY) AS 최대금액
     , MIN(MONEY) AS 최소금액
FROM TB_CUST;
```

---

### GROUP BY

**기능**: 특정 컬럼 기준으로 그룹화하여 집계

```sql
-- 부서별 연봉 합계
SELECT 부서ID
     , SUM(연봉) AS 부서별연봉합
FROM 직원
GROUP BY 부서ID;
```

**실행 결과 예시:**
```
부서ID  부서별연봉합
D001    12000
D002    8500
D003    15000
```

#### GROUP BY 제약사항

**SELECT, HAVING, ORDER BY에 사용 가능한 컬럼:**
1. GROUP BY에 명시된 컬럼
2. 집계함수로 처리된 컬럼

```sql
-- ❌ 오류 (학생이름은 GROUP BY에 없음)
SELECT 소속반, 학생이름, COUNT(*)
FROM 수강생정보
GROUP BY 소속반;

-- ✅ 정상
SELECT 소속반, COUNT(*) AS 학생수
FROM 수강생정보
GROUP BY 소속반;
```

---

### HAVING

**기능**: 집계 결과에 대한 조건 필터링

```sql
-- 평균 성적이 75점 이하인 학생
SELECT 학생ID
     , AVG(성적) AS 평균성적
FROM 성적표
WHERE 과목 != '수학'  -- 먼저 필터링
GROUP BY 학생ID
HAVING AVG(성적) <= 75;  -- 집계 후 필터링
```

#### WHERE vs HAVING

| 구분 | WHERE | HAVING |
|------|-------|--------|
| **실행 시점** | GROUP BY 이전 | GROUP BY 이후 |
| **대상** | 개별 행 | 그룹 |
| **집계함수** | 사용 불가 | 사용 가능 |

**실행 순서:**
```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
```

---

## 6. ORDER BY 절

### 기본 문법

```sql
SELECT 컬럼들
FROM 테이블명
ORDER BY 정렬기준컬럼 [ASC|DESC];
```

- **ASC**: 오름차순 (기본값, 생략 가능)
- **DESC**: 내림차순

---

### 예시

#### 단일 컬럼 정렬

```sql
-- 가격 오름차순
SELECT * FROM TB_PRD
ORDER BY PRD_AMT ASC;

-- 가격 내림차순
SELECT * FROM TB_PRD
ORDER BY PRD_AMT DESC;
```

#### 다중 컬럼 정렬

```sql
-- 상품타입 오름차순, 같은 타입 내에서는 가격 내림차순
SELECT * FROM TB_PRD
ORDER BY PRD_TYPE ASC, PRD_AMT DESC;
```

---

### 다양한 정렬 방식

```sql
-- 1. 컬럼명 사용
ORDER BY PRD_AMT DESC

-- 2. 별칭 사용
SELECT PRD_AMT AS 가격 FROM TB_PRD
ORDER BY 가격 DESC

-- 3. 컬럼 위치 번호 사용
SELECT PRD_TYPE, PRD_AMT FROM TB_PRD
ORDER BY 2 DESC  -- 두 번째 컬럼(PRD_AMT) 기준
```

---

## 7. 조인 (JOIN)

### 조인이란?

**여러 테이블에서 필요한 데이터를 한 번에 가져오는 기술**

```
회원 테이블              회원연락처 테이블
┌──────────┬────────┐  ┌──────────┬──────────┬───────────────┐
│  회원ID  │  이름  │  │  회원ID  │ 구분코드 │    연락처     │
├──────────┼────────┤  ├──────────┼──────────┼───────────────┤
│  A0001   │ 동동일 │  │  A0001   │  휴대폰  │ 010-111-1111  │
│  A0002   │ 동동이 │  │  A0001   │  집전화  │ 062-111-1111  │
│  A0003   │ 동동삼 │  │  A0002   │  집전화  │ 062-222-2222  │
└──────────┴────────┘  └──────────┴──────────┴───────────────┘
```

**질문**: A0001 회원의 이름과 휴대폰 번호는?

---

### 카티션 조인 (Cartesian Join)

**조인 조건 없이 모든 경우의 수를 조합**

```sql
SELECT * FROM 회원, 회원연락처;
```

**결과**: 회원(3행) × 회원연락처(3행) = 9행

---

### 조인 조건 추가

```sql
SELECT *
FROM 회원 A, 회원연락처 B
WHERE A.회원ID = B.회원ID;
```

**결과**: 회원ID가 일치하는 행만 출력 (3행)

---

### 조인 + 일반 조건

```sql
SELECT *
FROM 회원 A, 회원연락처 B
WHERE A.회원ID = B.회원ID      -- 조인 조건
  AND A.회원ID = 'A0001'       -- 일반 조건
  AND B.구분코드 = '휴대폰';   -- 일반 조건
```

---

### FROM 절 별칭 사용

```sql
-- 별칭 사용 (AS 생략)
SELECT A.CUST_ID, A.CUST_NAME, B.BADGE_ID
FROM TB_CUST A, TB_CUST_BADGE B
WHERE A.CUST_ID = B.CUST_ID;
```

**주의**: 별칭 사용 시 이후 원래 테이블명 사용 불가!

```sql
-- ❌ 오류
SELECT TB_CUST.CUST_ID, A.CUST_NAME
FROM TB_CUST A;

-- ✅ 정상
SELECT A.CUST_ID, A.CUST_NAME
FROM TB_CUST A;
```

---

### 조인 조건 최소 개수

**테이블 개수 - 1**

```sql
-- 4개 테이블 조인 → 최소 3개 조인 조건 필요
SELECT *
FROM A, B, C, D
WHERE A.직원ID = B.직원ID    -- 조인 조건 1
  AND B.직원ID = C.직원ID    -- 조인 조건 2
  AND C.직원ID = D.직원ID;   -- 조인 조건 3
```

---

## 8. 표준 조인 (ANSI JOIN)

### INNER JOIN

**두 테이블 간 조건에 일치하는 데이터만 출력**

```
회원          회원연락처
A0001  ───┐  ┌─ A0001
A0002  ───┼──┼─ A0001
A0003      │  ├─ A0002
           └──┤
              └─ A0004
```

#### Oracle 방식

```sql
SELECT *
FROM 회원 A, 회원연락처 B
WHERE A.회원ID = B.회원ID;
```

#### ANSI 방식

```sql
SELECT *
FROM 회원 A
INNER JOIN 회원연락처 B
  ON A.회원ID = B.회원ID;
```

---

### OUTER JOIN

**한쪽 테이블 기준으로 조건 불일치 데이터도 출력**

```
회원          회원연락처
A0001  ───┐  ┌─ A0001
A0002  ───┼──┼─ A0001
A0003  ◄──┘  ├─ A0002  (기준: 회원)
              └─ A0004
```

#### Oracle 방식 ((+) 기호)

```sql
-- 회원 테이블 기준
SELECT *
FROM 회원 A, 회원연락처 B
WHERE A.회원ID = B.회원ID(+);
```

**(+) 기호 규칙:**
- (+)가 붙은 **반대편** 테이블이 기준
- Oracle 전용 문법

#### ANSI 방식

```sql
-- LEFT OUTER JOIN
SELECT *
FROM 회원 A
LEFT OUTER JOIN 회원연락처 B
  ON A.회원ID = B.회원ID;

-- RIGHT OUTER JOIN  
SELECT *
FROM 회원연락처 A
RIGHT OUTER JOIN 회원 B
  ON A.회원ID = B.회원ID;

-- FULL OUTER JOIN
SELECT *
FROM 회원 A
FULL OUTER JOIN 회원연락처 B
  ON A.회원ID = B.회원ID;
```

---

### CROSS JOIN

**모든 조합 (카티션 곱)**

```sql
-- ANSI 방식
SELECT *
FROM 회원 A
CROSS JOIN 회원연락처 B;

-- Oracle 방식
SELECT *
FROM 회원 A, 회원연락처 B;
```

---

### 동등 조인 vs 비동등 조인

#### 동등 조인
**조인 조건이 모두 "="**

```sql
SELECT *
FROM A, B
WHERE A.ID = B.ID;
```

#### 비동등 조인
**조인 조건에 =이 아닌 연산자 포함**

```sql
SELECT *
FROM 급여등급 A, 직원 B
WHERE B.연봉 BETWEEN A.최소연봉 AND A.최대연봉;
```

---

### Oracle vs ANSI 비교

| 구분 | Oracle 방식 | ANSI 방식 |
|------|------------|-----------|
| **조인 조건** | WHERE 절 | ON 절 |
| **가독성** | FROM 깔끔, WHERE 복잡 | FROM 복잡, WHERE 깔끔 |
| **OUTER JOIN** | (+) 기호 | LEFT/RIGHT/FULL 명시 |
| **호환성** | Oracle 전용 | 모든 DBMS |

**권장**: ANSI 방식 (표준, 모든 DBMS 지원)

---

# Part 2: SQL 활용

## 1. 서브쿼리 (Sub Query)

### 서브쿼리란?

**쿼리 내부에 다른 쿼리를 삽입하여 다양한 데이터 출력**

```sql
SELECT 컬럼
FROM 테이블
WHERE 컬럼 = (SELECT ...)  -- 서브쿼리
```

### 서브쿼리 종류

| 위치 | 명칭 | 특징 |
|------|------|------|
| SELECT | 스칼라 서브쿼리 | 1행 1열 반환 |
| FROM | 인라인 뷰 | 가상 테이블 생성 |
| WHERE | 중첩 서브쿼리 | 조건 필터링 |

---

## 스칼라 서브쿼리 (Scalar Subquery)

**SELECT 절에서 사용, 1행 1열만 반환**

```sql
SELECT A.직원ID
     , A.이름
     , A.연봉
     , (SELECT B.부서명 
        FROM 부서 B 
        WHERE B.부서ID = A.부서ID) AS 부서명
FROM 직원 A;
```

### 실행 원리

1. 메인쿼리에서 각 행을 순회
2. 각 행마다 서브쿼리 실행
3. 서브쿼리 결과를 해당 행에 추가

**주의사항:**
- 반드시 1행 1열만 반환
- 결과가 없으면 NULL 반환
- 성능: 행 개수만큼 서브쿼리 실행

### 스칼라 서브쿼리 vs OUTER JOIN

```sql
-- 스칼라 서브쿼리
SELECT A.직원ID
     , (SELECT B.부서명 FROM 부서 B WHERE B.부서ID = A.부서ID)
FROM 직원 A;

-- OUTER JOIN (동일 결과)
SELECT A.직원ID, B.부서명
FROM 직원 A
LEFT OUTER JOIN 부서 B
  ON A.부서ID = B.부서ID;
```

---

## 인라인 뷰 (Inline View)

**FROM 절에서 사용, 가상 테이블 생성**

```sql
SELECT A.직원ID, A.이름, B.평균연봉
FROM 직원 A
   , (SELECT 부서ID, AVG(연봉) AS 평균연봉
      FROM 직원
      GROUP BY 부서ID) B
WHERE A.부서ID = B.부서ID;
```

**특징:**
- 독립적으로 실행 가능한 쿼리
- 복잡한 쿼리를 단계별로 분리
- TOP-N 쿼리에 활용

---

## 중첩 서브쿼리 (Nested Subquery)

**WHERE 절에서 사용**

### 비상관 서브쿼리

**메인쿼리 컬럼 미사용 → 서브쿼리 먼저 실행**

```sql
-- 평균 연봉 이상인 직원
SELECT 직원ID, 이름, 연봉
FROM 직원
WHERE 연봉 >= (SELECT AVG(연봉) FROM 직원);
```

**실행 순서:**
1. 서브쿼리 실행 → 평균연봉 계산
2. 메인쿼리 실행 → 조건 비교

---

### 상관 서브쿼리

**메인쿼리 컬럼 사용 → 메인쿼리 각 행마다 서브쿼리 실행**

```sql
-- 부서별 최저연봉 직원
SELECT A.직원ID, A.이름, A.연봉, A.부서ID
FROM 직원 A
WHERE A.연봉 = (SELECT MIN(연봉) 
                FROM 직원 B 
                WHERE B.부서ID = A.부서ID);
```

**실행 원리:**
1. 메인쿼리 첫 행 조회
2. 해당 행의 부서ID로 서브쿼리 실행
3. 결과 비교 후 조건 만족 시 출력
4. 모든 행에 대해 반복

---

### 단일행 vs 다중행 연산자

#### 단일행 연산자 (1개 이하 결과)

```sql
=, !=, >, <, >=, <=
```

```sql
-- ✅ 정상 (서브쿼리 결과 1개)
SELECT * FROM 직원
WHERE 연봉 = (SELECT MIN(연봉) FROM 직원);

-- ❌ 오류 (서브쿼리 결과 여러 개)
SELECT * FROM 직원
WHERE 연봉 = (SELECT MIN(연봉) FROM 직원 GROUP BY 부서ID);
```

#### 다중행 연산자 (여러 개 결과)

```sql
IN, ANY, ALL, EXISTS, NOT EXISTS
```

---

### IN 연산자

**여러 값 중 일치하는 모든 값 출력**

```sql
-- 부서별 최저연봉과 일치하는 직원
SELECT * FROM 직원
WHERE 연봉 IN (SELECT MIN(연봉) 
               FROM 직원 
               GROUP BY 부서ID);
```

#### 다중 컬럼 서브쿼리 (Oracle 전용)

```sql
SELECT * FROM 직원
WHERE (부서ID, 연봉) IN (SELECT 부서ID, MIN(연봉) 
                         FROM 직원 
                         GROUP BY 부서ID);
```

---

### ANY 연산자

**하나라도 일치하면 출력**

```sql
-- 부서별 최저연봉 중 하나보다 큰 연봉
SELECT * FROM 직원
WHERE 연봉 > ANY (SELECT MIN(연봉) 
                  FROM 직원 
                  GROUP BY 부서ID);
```

---

### ALL 연산자

**모두 만족해야 출력**

```sql
-- 모든 부서의 최저연봉보다 큰 연봉
SELECT * FROM 직원
WHERE 연봉 > ALL (SELECT MIN(연봉) 
                  FROM 직원 
                  GROUP BY 부서ID);
```

---

### EXISTS 연산자

**조건 만족하는 행이 존재하면 TRUE**

```sql
-- 연락처가 있는 직원
SELECT A.직원ID, A.이름
FROM 직원 A
WHERE EXISTS (SELECT 1 
              FROM 직원연락처 B 
              WHERE B.직원ID = A.직원ID);
```

**특징:**
- 첫 번째 일치 행 발견 시 즉시 TRUE 반환
- 성능 우수 (불필요한 스캔 생략)
- SELECT 절 내용 무의미 (1, 'X' 등 아무거나)

---

### NOT EXISTS 연산자

**조건 만족하는 행이 없으면 TRUE**

```sql
-- 연락처가 없는 직원
SELECT A.직원ID, A.이름
FROM 직원 A
WHERE NOT EXISTS (SELECT 1 
                  FROM 직원연락처 B 
                  WHERE B.직원ID = A.직원ID);
```

---

## 2. 집합연산자

### 집합연산자란?

**여러 SQL 결과를 결합**

```
테이블A: {1,2,3,4,5}    테이블B: {4,5,6,7,8}

UNION         → {1,2,3,4,5,6,7,8}  (중복제거)
UNION ALL     → {1,2,3,4,5,4,5,6,7,8}  (중복포함)
INTERSECT     → {4,5}  (교집합)
MINUS/EXCEPT  → {1,2,3}  (차집합)
```

---

### UNION - 합집합 (중복 제거)

```sql
SELECT COL1, COL2 FROM 테이블A
UNION
SELECT COL1, COL2 FROM 테이블B;
```

**특징:**
- 중복 제거
- 자동 정렬
- 컬럼 개수, 데이터타입 일치 필요

---

### UNION ALL - 합집합 (중복 포함)

```sql
SELECT COL1, COL2 FROM 테이블A
UNION ALL
SELECT COL1, COL2 FROM 테이블B;
```

**특징:**
- 중복 포함
- 정렬 안 함
- UNION보다 빠름

---

### INTERSECT - 교집합

```sql
SELECT COL1, COL2 FROM 테이블A
INTERSECT
SELECT COL1, COL2 FROM 테이블B;
```

---

### MINUS (EXCEPT) - 차집합

```sql
-- Oracle
SELECT COL1, COL2 FROM 테이블A
MINUS
SELECT COL1, COL2 FROM 테이블B;

-- SQL Server
SELECT COL1, COL2 FROM 테이블A
EXCEPT
SELECT COL1, COL2 FROM 테이블B;
```

---

### 집합연산자 주의사항

#### 1. 컬럼 개수 일치

```sql
-- ❌ 오류
SELECT COL1 FROM 테이블A
UNION
SELECT COL1, COL2 FROM 테이블B;
```

#### 2. 데이터타입 일치

```sql
-- ❌ 오류
SELECT 숫자컬럼 FROM 테이블A
UNION
SELECT 문자컬럼 FROM 테이블B;
```

#### 3. 컬럼명은 첫 번째 쿼리 기준

```sql
SELECT COL1 AS A, COL2 AS B FROM 테이블A
UNION
SELECT COL3, COL4 FROM 테이블B;
-- 결과 컬럼명: A, B
```

#### 4. ORDER BY는 마지막에만

```sql
SELECT COL1 FROM 테이블A
UNION
SELECT COL1 FROM 테이블B
ORDER BY COL1;  -- 여기만 가능!
```

---

## 3. 그룹함수

### 그룹함수 종류

```
그룹함수
├─ ROLLUP      (계층적 소계)
├─ CUBE        (모든 조합)
└─ GROUPING SETS  (지정된 그룹)
```

---

### ROLLUP - 계층적 소계

**앞에서부터 계층적으로 그룹화**

```sql
SELECT 부서ID, 직급, SUM(연봉)
FROM 직원
GROUP BY ROLLUP(부서ID, 직급);

-- 아래와 동일
SELECT 부서ID, 직급, SUM(연봉) FROM 직원 GROUP BY 부서ID, 직급
UNION ALL
SELECT 부서ID, NULL, SUM(연봉) FROM 직원 GROUP BY 부서ID
UNION ALL
SELECT NULL, NULL, SUM(연봉) FROM 직원;
```

**결과:**
```
부서ID  직급    연봉합
D001    사원    5000
D001    대리    7000
D001    NULL    12000  ← 부서별 소계
D002    사원    6000
D002    NULL    6000   ← 부서별 소계
NULL    NULL    18000  ← 전체 합계
```

---

### CUBE - 모든 조합

**입력된 컬럼의 2^N개 조합**

```sql
SELECT 고객ID, 월, SUM(사용량)
FROM 가스사용량
GROUP BY CUBE(고객ID, 월);

-- 아래와 동일 (2^2 = 4개)
SELECT 고객ID, 월, SUM(사용량) FROM 가스사용량 GROUP BY 고객ID, 월
UNION ALL
SELECT 고객ID, NULL, SUM(사용량) FROM 가스사용량 GROUP BY 고객ID
UNION ALL
SELECT NULL, 월, SUM(사용량) FROM 가스사용량 GROUP BY 월
UNION ALL
SELECT NULL, NULL, SUM(사용량) FROM 가스사용량;
```

---

### GROUPING SETS - 지정된 그룹

**명시한 그룹만 집계**

```sql
SELECT 고객ID, 월, SUM(사용량)
FROM 가스사용량
GROUP BY GROUPING SETS(고객ID, 월);

-- 아래와 동일
SELECT 고객ID, NULL AS 월, SUM(사용량) FROM 가스사용량 GROUP BY 고객ID
UNION ALL
SELECT NULL AS 고객ID, 월, SUM(사용량) FROM 가스사용량 GROUP BY 월;
```

---

### GROUPING() 함수

**NULL이 실제 NULL인지 집계용 NULL인지 구분**

```sql
SELECT CASE WHEN GROUPING(부서ID) = 1 THEN '전체'
            ELSE 부서ID 
       END AS 부서
     , CASE WHEN GROUPING(직급) = 1 THEN '소계'
            ELSE 직급 
       END AS 직급
     , SUM(연봉)
FROM 직원
GROUP BY ROLLUP(부서ID, 직급);
```

**GROUPING() 반환값:**
- 0: 해당 컬럼으로 그룹화 중
- 1: 해당 컬럼으로 그룹화 안 함 (집계용 NULL)

---

## 4. 윈도우 함수

### 윈도우 함수란?

**행과 행 간의 관계를 정의하고 비교, 연산하는 함수**

#### 윈도우 함수 종류

| 분류 | 함수 |
|------|------|
| **순위 함수** | RANK, DENSE_RANK, ROW_NUMBER |
| **집계 함수** | SUM, MAX, MIN, AVG, COUNT |
| **행 순서 함수** | FIRST_VALUE, LAST_VALUE, LAG, LEAD |
| **비율 함수** | CUME_DIST, PERCENT_RANK |

---

### 윈도우 함수 문법

```sql
함수명(매개변수) OVER (
    [PARTITION BY 컬럼]     -- 그룹화
    [ORDER BY 컬럼]         -- 정렬
    [WINDOWING 절]          -- 범위 지정
)
```

---

### 순위 함수

#### RANK - 동순위 건너뛰기

```sql
SELECT 지점코드
     , 매출액
     , RANK() OVER (ORDER BY 매출액 DESC) AS 순위
FROM 월별매출;
```

**결과:**
```
지점    매출액   순위
A지점   1000    1
B지점   800     2
C지점   800     2
D지점   600     4  ← 3을 건너뜀
```

#### DENSE_RANK - 동순위 안 건너뛰기

```sql
SELECT 지점코드
     , 매출액
     , DENSE_RANK() OVER (ORDER BY 매출액 DESC) AS 순위
FROM 월별매출;
```

**결과:**
```
지점    매출액   순위
A지점   1000    1
B지점   800     2
C지점   800     2
D지점   600     3  ← 건너뛰지 않음
```

#### ROW_NUMBER - 고유 번호

```sql
SELECT 지점코드
     , 매출액
     , ROW_NUMBER() OVER (ORDER BY 매출액 DESC) AS 순위
FROM 월별매출;
```

**결과:**
```
지점    매출액   순위
A지점   1000    1
B지점   800     2
C지점   800     3  ← 동순위 없음
D지점   600     4
```

---

### PARTITION BY - 그룹별 순위

```sql
SELECT 팀명
     , 선수명
     , 포지션
     , 키
     , RANK() OVER (PARTITION BY 팀명 ORDER BY 키 DESC) AS 팀내순위
FROM 선수;
```

**결과:**
```
팀명    선수명   키    팀내순위
삼성    홍길동   190   1
삼성    김철수   185   2
LG      이영희   188   1
LG      박민수   180   2
```

---

### WINDOWING 절 - 범위 지정

```sql
SELECT 상품ID
     , 가격
     , SUM(가격) OVER (ORDER BY 가격 
                      ROWS BETWEEN UNBOUNDED PRECEDING 
                                AND CURRENT ROW) AS 누적합
FROM 상품;
```

**WINDOWING 옵션:**
- `ROWS BETWEEN ... AND ...`: 행 범위
- `UNBOUNDED PRECEDING`: 첫 행
- `CURRENT ROW`: 현재 행
- `UNBOUNDED FOLLOWING`: 마지막 행
- `N PRECEDING`: 이전 N행
- `N FOLLOWING`: 이후 N행

---

### LAG / LEAD - 이전/이후 행 값

#### LAG - 이전 행

```sql
SELECT 년월
     , 매출액
     , LAG(매출액, 1, 0) OVER (ORDER BY 년월) AS 전월매출
FROM 월별매출;
```

#### LEAD - 이후 행

```sql
SELECT 년월
     , 매출액
     , LEAD(매출액, 1, 0) OVER (ORDER BY 년월) AS 다음월매출
FROM 월별매출;
```

**문법:**
- `LAG(컬럼, N, DEFAULT)`: N행 이전 값, 없으면 DEFAULT
- `LEAD(컬럼, N, DEFAULT)`: N행 이후 값, 없으면 DEFAULT

---

## 5. TOP-N 쿼리

### ROWNUM (Oracle)

**각 행에 부여되는 임시 일련번호**

```sql
-- 상위 5개 행
SELECT * FROM 직원
WHERE ROWNUM <= 5;
```

---

### ROWNUM 특징

**1부터 시작해야 사용 가능**

```sql
-- ✅ 정상
WHERE ROWNUM = 1
WHERE ROWNUM <= 5
WHERE ROWNUM BETWEEN 1 AND 10

-- ❌ 오류
WHERE ROWNUM = 2   -- 1을 먼저 사용 안 함
WHERE ROWNUM > 5   -- 1을 먼저 사용 안 함
```

---

### ROWNUM으로 특정 행 가져오기

```sql
-- ROWNUM = 2인 행 가져오기
SELECT *
FROM (SELECT ROWNUM AS RN, A.*
      FROM 직원 A)
WHERE RN = 2;
```

---

### TOP-N 쿼리 패턴

**연봉 상위 5명 조회**

```sql
-- ❌ 잘못된 방법 (정렬 전 5개 선택)
SELECT * FROM 직원
WHERE ROWNUM <= 5
ORDER BY 연봉 DESC;

-- ✅ 올바른 방법 (정렬 후 5개 선택)
SELECT *
FROM (SELECT * FROM 직원 ORDER BY 연봉 DESC)
WHERE ROWNUM <= 5;
```

**실행 순서:**
1. 인라인 뷰: 연봉 내림차순 정렬
2. ROWNUM: 상위 5개 선택

---

### TOP (SQL Server)

```sql
-- 상위 5개
SELECT TOP 5 * FROM 직원
ORDER BY 연봉 DESC;

-- 상위 10%
SELECT TOP 10 PERCENT * FROM 직원
ORDER BY 연봉 DESC;
```

---

## 6. 계층형 질의와 셀프 조인

### 계층형 질의란?

**동일 테이블 내 계층 관계를 표현하는 SQL**

```
메뉴 구조 예시:
홈
├─ 훈련연계과정
├─ 수강생관리
├─ 회사소개
│  ├─ 인사말
│  └─ 오시는 길
└─ QNA
```

---

### 계층형 질의 문법 (Oracle)

```sql
SELECT LEVEL, 메뉴ID, 메뉴명, 상위메뉴ID
FROM 메뉴
START WITH 상위메뉴ID IS NULL  -- 루트 노드
CONNECT BY PRIOR 메뉴ID = 상위메뉴ID  -- 계층 연결
ORDER SIBLINGS BY 메뉴ID;  -- 형제 노드 정렬
```

**주요 키워드:**
- `START WITH`: 시작점 (루트)
- `CONNECT BY PRIOR`: 계층 연결 조건
- `LEVEL`: 계층 레벨 (루트=1)
- `ORDER SIBLINGS BY`: 형제 노드 정렬

---

### PRIOR 위치에 따른 방향

```sql
-- 순방향 (부모→자식)
CONNECT BY PRIOR 자식 = 부모

-- 역방향 (자식→부모)
CONNECT BY PRIOR 부모 = 자식
```

---

### 계층형 질의 함수

```sql
-- 계층 레벨
SELECT LEVEL, 메뉴명
FROM 메뉴
START WITH 상위메뉴ID IS NULL
CONNECT BY PRIOR 메뉴ID = 상위메뉴ID;

-- 루트 노드 값
SELECT SYS_CONNECT_BY_PATH(메뉴명, '/') AS 경로
FROM 메뉴
START WITH 상위메뉴ID IS NULL
CONNECT BY PRIOR 메뉴ID = 상위메뉴ID;
-- 결과: /홈/회사소개/인사말

-- 단말 노드 여부
SELECT 메뉴명
     , CONNECT_BY_ISLEAF AS 단말여부
FROM 메뉴
START WITH 상위메뉴ID IS NULL
CONNECT BY PRIOR 메뉴ID = 상위메뉴ID;
```

---

### 셀프 조인 (Self Join)

**동일 테이블을 조인**

```sql
-- 직원과 상사 정보
SELECT A.직원명 AS 직원
     , B.직원명 AS 상사
FROM 직원 A
LEFT OUTER JOIN 직원 B
  ON A.상사ID = B.직원ID;
```

---

## 7. PIVOT & UNPIVOT

### PIVOT - 행을 열로

**행 데이터를 열로 전환**

```sql
SELECT *
FROM (SELECT 지점코드, 년월, 매출액
      FROM 월별매출)
PIVOT (SUM(매출액)              -- 집계함수
       FOR 년월 IN ('202201' AS "1월",
                    '202202' AS "2월",
                    '202203' AS "3월"));
```

**변환 전:**
```
지점코드  년월     매출액
A지점    202201   1000
A지점    202202   1200
B지점    202201   800
```

**변환 후:**
```
지점코드  1월    2월    3월
A지점    1000  1200   ...
B지점     800   ...   ...
```

---

### UNPIVOT - 열을 행으로

**열 데이터를 행으로 전환**

```sql
SELECT *
FROM 월별매출_피벗
UNPIVOT (매출액                 -- 값 컬럼
         FOR 년월 IN ("1월", "2월", "3월"));
```

**변환 전:**
```
지점코드  1월    2월    3월
A지점    1000  1200  1300
```

**변환 후:**
```
지점코드  년월   매출액
A지점    1월    1000
A지점    2월    1200
A지점    3월    1300
```

---

## 8. 정규 표현식

### 정규 표현식 함수

| 함수 | 설명 |
|------|------|
| REGEXP_LIKE | 패턴 매칭 (WHERE 조건) |
| REGEXP_REPLACE | 패턴 치환 |
| REGEXP_SUBSTR | 패턴 추출 |
| REGEXP_INSTR | 패턴 위치 |

### 기본 패턴

| 패턴 | 의미 |
|------|------|
| `.` | 임의의 한 문자 |
| `^` | 문자열 시작 |
| `# SQL 기본 + 활용 + 관리구문 (SQLD 2과목)

## 📚 목차

### Part 1: SQL 기본
1. 관계형 데이터베이스 개요
2. SELECT 문
3. 함수
4. WHERE 절
5. GROUP BY, HAVING 절
6. ORDER BY 절
7. 조인 (JOIN)
8. 표준 조인

### Part 2: SQL 활용
1. 서브쿼리
2. 집합연산자
3. 그룹함수
4. 윈도우함수
5. TOP-N 쿼리
6. 계층형 질의와 셀프 조인
7. PIVOT & UNPIVOT
8. 정규 표현식

### Part 3: 관리구문
1. DML (INSERT, UPDATE, DELETE, MERGE)
2. TCL (COMMIT, ROLLBACK, SAVEPOINT)
3. DDL (CREATE, ALTER, DROP, RENAME, TRUNCATE)
4. DCL (GRANT, REVOKE, ROLE)

---

# Part 1: SQL 기본

## 1. 관계형 데이터베이스 개요

### 데이터베이스 관련 용어

#### 데이터베이스 (Database, DB)
- 데이터를 일정한 형태로 저장해 놓은 것
- 예: 엑셀도 하나의 데이터베이스

#### DBMS (Database Management System)
- 데이터 손상 방지 및 복구
- 인증된 사용자만 접근
- 추가 관리 기능 지원

#### RDBMS (Relational DBMS)
- 테이블로 데이터 관리
- 테이블 간 관계를 이용해 데이터 정의
- Oracle, MySQL, PostgreSQL 등

#### 테이블 (Table)
- RDBMS에서 실제 데이터가 저장되는 2차원 배열 형태의 저장소
- 엔터티 → 테이블
- 속성 → 컬럼 (Column)
- 인스턴스 → 튜플/행 (Row/Tuple)

```
직원 테이블 예시
┌──────────┬────────┬──────┬────────┬──────────┐
│  직원ID  │  이름  │ 나이 │  연봉  │  부서ID  │
├──────────┼────────┼──────┼────────┼──────────┤
│  A0001   │ 강태우 │  35  │  5000  │  D001    │
│  A0002   │ 김형준 │  32  │  4800  │  D001    │
│  A0003   │ 송송이 │  27  │  3400  │  D002    │
└──────────┴────────┴──────┴────────┴──────────┘
```

---

### SQL (Structured Query Language)

RDBMS에서 데이터 정의, 조작, 조회, 제어를 위한 언어

#### SQL 분류 ⭐️ 매우 중요!

| 분류 | 영문명 | 명령어 | 용도 |
|------|--------|--------|------|
| **DDL** | Data Definition Language<br>(데이터 정의어) | CREATE, ALTER, DROP,<br>RENAME, TRUNCATE | 테이블 생성/수정/삭제 |
| **DML** | Data Manipulation Language<br>(데이터 조작어) | SELECT, INSERT,<br>UPDATE, DELETE, MERGE | 데이터 조회/입력/수정/삭제 |
| **DCL** | Data Control Language<br>(데이터 제어어) | GRANT, REVOKE | 권한 부여/회수 |
| **TCL** | Transaction Control Language<br>(트랜잭션 제어어) | COMMIT, ROLLBACK,<br>SAVEPOINT | 트랜잭션 제어 |

---

### 집합 연산자와 관계 연산자

#### 일반 집합 연산자

| 연산자 | SQL 구현 | 설명 |
|--------|----------|------|
| UNION | UNION | 합집합 |
| INTERSECTION | INTERSECT | 교집합 |
| DIFFERENCE | MINUS (EXCEPT) | 차집합 |
| PRODUCT | CROSS JOIN | 카티션 곱 |

#### 순수 관계 연산자

| 연산자 | SQL 구현 | 설명 |
|--------|----------|------|
| SELECT | WHERE | 행 선택 |
| PROJECT | SELECT | 열 선택 |
| JOIN | JOIN | 테이블 결합 |
| DIVIDE | (사용 안 함) | - |

---

## 2. SELECT 문

### 기본 구조

```sql
SELECT 컬럼1, 컬럼2, ...
FROM 테이블명
WHERE 조건;
```

**실행 순서:**
1. FROM: 테이블에서 데이터 가져오기
2. WHERE: 조건에 맞는 행만 필터링
3. SELECT: 원하는 컬럼만 출력

---

### 주요 문법

#### (1) * (애스터리스크) - 전체 컬럼 조회

```sql
SELECT * FROM TB_PRD;
-- TB_PRD 테이블의 모든 컬럼 출력
```

#### (2) DISTINCT - 중복 제거

```sql
SELECT DISTINCT PRD_TYPE FROM TB_PRD;
-- PRD_TYPE 컬럼 값 중복 제거하여 출력
```

#### (3) AS (ALIAS) - 별칭 부여

```sql
SELECT CUST_ID AS C_ID
     , BIRTH_DY AS 생년월일
     , MONEY AS PAY_123
FROM TB_CUST;
```

**AS 사용 주의사항:**
- 띄어쓰기 불가
- 문자로 시작해야 함
- 예약어 사용 불가
- 특수문자는 $, _, # 만 가능

**AS 생략 가능:**
```sql
SELECT CUST_ID C_ID, BIRTH_DY 생년월일
FROM TB_CUST;
```

**특수문자/띄어쓰기 포함 시 큰따옴표 사용:**
```sql
SELECT CUST_ID AS "고객 ID", MONEY AS "보유$금액"
FROM TB_CUST;
```

#### (4) 연산자

**사칙연산:**
```sql
SELECT CUST_NAME
     , MONEY
     , MONEY + 100 AS 증가금액
     , MONEY * 1.1 AS "10%인상"
FROM TB_CUST;
```

**연결 연산자 (||):**
```sql
SELECT CUST_NAME || '님' AS 고객명
     , '보유금액: ' || MONEY || '원' AS 금액정보
FROM TB_CUST;
```

---

## 3. 함수

### 함수의 특징

1. 블랙박스처럼 작동 (내부 구조 몰라도 됨)
2. 입력 개수와 출력 개수는 함수마다 다름
3. **입력, 출력, 기능(What)만 알면 사용 가능**

```
입력값 → [함수 기능] → 출력값
  3, 10 → [더하기] → 13
 'AAAAA' → [소문자변환] → 'aaaaa'
```

---

### 문자형 함수

#### UPPER / LOWER

```sql
-- 대문자 변환
SELECT UPPER('abcde123@@') AS UPPER사용
FROM DUAL;
-- 결과: ABCDE123@@

-- 소문자 변환
SELECT LOWER('ABCDE123@@') AS LOWER사용
FROM DUAL;
-- 결과: abcde123@@
```

#### SUBSTR - 문자열 자르기

```sql
-- SUBSTR(문자열, 시작위치, 길이)
SELECT PRD_NAME
     , PRD_DETAIL
     , SUBSTR(PRD_DETAIL, 1, 5) || '...' AS 상품설명생략
FROM TB_PRD
WHERE PRD_TYPE = '가전';
```

#### TRIM - 공백 제거

```sql
-- 양 끝 공백 제거 (중간 공백은 제거 안 됨)
SELECT TRIM('  안녕하세요  ') AS 결과1
     , TRIM('안 녕 하 세 요') AS 결과2
FROM DUAL;
-- 결과1: '안녕하세요'
-- 결과2: '안 녕 하 세 요'
```

#### REPLACE - 문자 치환

```sql
-- REPLACE(문자열, 바뀔값, 바꿀값)
SELECT PRD_DETAIL
     , REPLACE(PRD_DETAIL, ' ', '') AS 공백제거
FROM TB_PRD;
```

---

### 숫자형 함수

#### ROUND - 반올림

```sql
-- ROUND(실수, 소수점자릿수)
SELECT ROUND(1.452, 2) AS 결과1  -- 1.45
     , ROUND(1.452, 1) AS 결과2  -- 1.5
     , ROUND(1.452, 0) AS 결과3  -- 1
FROM DUAL;
```

#### MOD - 나머지

```sql
SELECT MOD(10, 3) AS 나머지  -- 1
FROM DUAL;
```

---

### 날짜형 함수

#### SYSDATE / GETDATE()

```sql
-- Oracle
SELECT SYSDATE FROM DUAL;

-- SQL Server
SELECT GETDATE();
```

---

### 형변환 함수

```
        TO_CHAR()
문자형 ←----------→ 숫자형
   ↑                 ↓
   |   TO_CHAR()     |
   |                 |
   └─────────────────┘
         ↑       ↓
    TO_DATE() TO_NUMBER()
         날짜형
```

#### 형변환 우선순위
**날짜형 > 숫자형 > 문자형**

```sql
-- 자동 형변환 (문자→숫자)
SELECT '100' + 200 FROM DUAL;  -- 300

-- 강제 형변환
SELECT TO_NUMBER('100') + 200 FROM DUAL;  -- 300
SELECT TO_CHAR(100) FROM DUAL;  -- '100'
```

#### 날짜 형변환

```sql
-- 날짜 → 문자
SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD HH24:MI:SS') FROM DUAL;
-- 결과: 2023/01/01 14:30:45

SELECT TO_CHAR(SYSDATE, 'YYYYMMDD') FROM DUAL;
-- 결과: 20230101

-- 문자 → 날짜
SELECT TO_DATE('20230101', 'YYYYMMDD') FROM DUAL;
SELECT TO_DATE('20230101141212', 'YYYYMMDD HH24MISS') FROM DUAL;
```

**날짜 포맷 기호:**
- YYYY: 년 (4자리)
- MM: 월 (2자리)
- DD: 일 (2자리)
- HH24: 시간 (24시간 형식)
- MI: 분
- SS: 초

---

### NULL 함수

#### NULL의 특징

```sql
SELECT 3 + NULL FROM DUAL;      -- NULL
SELECT 3 * NULL FROM DUAL;      -- NULL
SELECT 3 = NULL FROM DUAL;      -- NULL
```

> **중요**: NULL과의 모든 연산 결과는 NULL!

#### NVL - NULL 대체

```sql
-- NVL(data1, data2)
-- data1이 NULL이면 data2 반환, 아니면 data1 반환

SELECT CUST_NAME
     , NVL(MONEY, 0) AS 보유금액
FROM TB_CUST;
```

#### NVL2 - NULL 여부에 따라 다른 값 반환

```sql
-- NVL2(data1, data2, data3)
-- data1이 NULL이 아니면 data2, NULL이면 data3

SELECT CUST_NAME
     , NVL2(MONEY, '보유', '없음') AS 금액여부
FROM TB_CUST;
```

#### COALESCE - 여러 값 중 첫 번째 NOT NULL 값

```sql
-- COALESCE(data1, data2, data3, ...)
-- 처음으로 NULL이 아닌 값 반환

SELECT COALESCE(NULL, NULL, 100, 200) FROM DUAL;  -- 100
```

#### DECODE - 조건 분기

```sql
-- DECODE(data1, data2, data3, data4)
-- data1 = data2이면 data3, 아니면 data4

SELECT CUST_NAME
     , DECODE(MONEY, 0, '없음', '있음') AS 금액여부
FROM TB_CUST;

-- 다중 조건
SELECT CUST_NAME
     , DECODE(부서ID, 'D001', '인사부'
                     , 'D002', '급여부'
                     , 'D003', '개발부'
                     , '기타') AS 부서명
FROM TB_CUST;
```

---

### CASE 문

```sql
CASE WHEN 조건1 THEN 결과1
     WHEN 조건2 THEN 결과2
     ...
     ELSE 기본결과
END
```

**예시: 포인트 등급 부여**

```sql
SELECT CUST_NAME
     , ACT_POINT
     , CASE WHEN ACT_POINT BETWEEN 0 AND 1000 THEN '브론즈'
            WHEN ACT_POINT BETWEEN 1001 AND 10000 THEN '실버'
            WHEN ACT_POINT BETWEEN 10001 AND 100000 THEN '골드'
            ELSE '마스터'
       END AS 등급
FROM TB_CUST;
```

---

## 4. WHERE 절

### WHERE절의 역할

```sql
SELECT * FROM TB_CUST WHERE CUST_ID = 'C0001';
```

**실행 순서:**
1. TB_CUST 테이블 가져오기
2. WHERE 조건에 맞는 행 필터링
3. SELECT 컬럼 출력

---

### 비교 조건

#### 동등 조건 (=)

```sql
-- PRD_TYPE이 '컴퓨터'인 상품
SELECT * FROM TB_PRD 
WHERE PRD_TYPE = '컴퓨터';
```

#### 비동등 조건 (>, <, >=, <=, !=, <>)

```sql
-- 가격이 1,000,000 이상인 상품
SELECT * FROM TB_PRD 
WHERE PRD_AMT >= 1000000;

-- 가격이 500,000이 아닌 상품
SELECT * FROM TB_PRD 
WHERE PRD_AMT != 500000;  -- 또는 <>
```

---

### 논리 조건

#### AND - 모든 조건 만족

```sql
SELECT * FROM TB_CUST
WHERE CUST_ID = 'C0001'
  AND PASSWD = 'pass111';
```

#### OR - 하나 이상 만족

```sql
SELECT * FROM TB_PRD
WHERE PRD_TYPE = '컴퓨터'
   OR PRD_TYPE = '스마트폰';
```

---

### 부정 연산 (NOT)

```sql
-- 가전이 아닌 상품
SELECT * FROM TB_PRD
WHERE PRD_TYPE != '가전';

-- 또는
SELECT * FROM TB_PRD
WHERE NOT PRD_TYPE = '가전';
```

---

### NULL 연산

```sql
-- NULL인 데이터
SELECT * FROM TB_CUST
WHERE MONEY IS NULL;

-- NULL이 아닌 데이터
SELECT * FROM TB_CUST
WHERE MONEY IS NOT NULL;
```

> **주의**: `= NULL` 또는 `!= NULL` 사용 불가!

---

### IN 연산자

```sql
-- 여러 값 중 하나와 일치
SELECT * FROM TB_PRD
WHERE PRD_TYPE IN ('가전', '욕실용품', '스마트폰');

-- 위 쿼리는 아래와 동일
SELECT * FROM TB_PRD
WHERE PRD_TYPE = '가전'
   OR PRD_TYPE = '욕실용품'
   OR PRD_TYPE = '스마트폰';
```

#### NOT IN

```sql
-- 제외할 값들
SELECT * FROM TB_PRD
WHERE PRD_TYPE NOT IN ('가전', '욕실용품');
```

---

### BETWEEN 연산자

```sql
-- 범위 조건 (경계값 포함)
SELECT * FROM TB_PRD
WHERE PRD_AMT BETWEEN 100000 AND 500000;

-- 위 쿼리는 아래와 동일
SELECT * FROM TB_PRD
WHERE PRD_AMT >= 100000
  AND PRD_AMT <= 500000;
```

---

### LIKE 연산자 (패턴 매칭)

#### 와일드카드

- `%`: 0개 이상의 문자
- `_`: 정확히 1개의 문자

```sql
-- '수'로 시작하는 상품
SELECT * FROM TB_PRD
WHERE PRD_NAME LIKE '수%';

-- '용'이 포함되는 상품
SELECT * FROM TB_PRD
WHERE PRD_TYPE LIKE '%용%';

-- '기'로 끝나는 상품
SELECT * FROM TB_PRD
WHERE PRD_NAME LIKE '%기';

-- 3글자이면서 '기'로 끝나는 상품
SELECT * FROM TB_PRD
WHERE PRD_NAME LIKE '__기';
```

---

## 5. GROUP BY, HAVING 절

### 집계함수 (Aggregate Functions)

**특징:**
- 다중행 함수 (여러 행 → 1개 결과)
- NULL 값은 무시

#### COUNT - 행 개수

```sql
-- 전체 행 개수 (NULL 포함)
SELECT COUNT(*) FROM TB_CUST;

-- 특정 컬럼 (NULL 제외)
SELECT COUNT(MONEY) FROM TB_CUST;
```

#### SUM - 합계

```sql
SELECT SUM(MONEY) AS 총금액
FROM TB_CUST;
```

#### AVG - 평균

```sql
SELECT AVG(MONEY) AS 평균금액
FROM TB_CUST;
```

#### MAX / MIN - 최대/최소

```sql
SELECT MAX(MONEY) AS 최대금액
     , MIN(MONEY) AS 최소금액
FROM TB_CUST;
```

---

### GROUP BY

**기능**: 특정 컬럼 기준으로 그룹화하여 집계

```sql
-- 부서별 연봉 합계
SELECT 부서ID
     , SUM(연봉) AS 부서별연봉합
FROM 직원
GROUP BY 부서ID;
```

**실행 결과 예시:**
```
부서ID  부서별연봉합
D001    12000
D002    8500
D003    15000
```

#### GROUP BY 제약사항

**SELECT, HAVING, ORDER BY에 사용 가능한 컬럼:**
1. GROUP BY에 명시된 컬럼
2. 집계함수로 처리된 컬럼

```sql
-- ❌ 오류 (학생이름은 GROUP BY에 없음)
SELECT 소속반, 학생이름, COUNT(*)
FROM 수강생정보
GROUP BY 소속반;

-- ✅ 정상
SELECT 소속반, COUNT(*) AS 학생수
FROM 수강생정보
GROUP BY 소속반;
```

---

### HAVING

**기능**: 집계 결과에 대한 조건 필터링

```sql
-- 평균 성적이 75점 이하인 학생
SELECT 학생ID
     , AVG(성적) AS 평균성적
FROM 성적표
WHERE 과목 != '수학'  -- 먼저 필터링
GROUP BY 학생ID
HAVING AVG(성적) <= 75;  -- 집계 후 필터링
```

#### WHERE vs HAVING

| 구분 | WHERE | HAVING |
|------|-------|--------|
| **실행 시점** | GROUP BY 이전 | GROUP BY 이후 |
| **대상** | 개별 행 | 그룹 |
| **집계함수** | 사용 불가 | 사용 가능 |

**실행 순서:**
```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
```

---

## 6. ORDER BY 절

### 기본 문법

```sql
SELECT 컬럼들
FROM 테이블명
ORDER BY 정렬기준컬럼 [ASC|DESC];
```

- **ASC**: 오름차순 (기본값, 생략 가능)
- **DESC**: 내림차순

---

### 예시

#### 단일 컬럼 정렬

```sql
-- 가격 오름차순
SELECT * FROM TB_PRD
ORDER BY PRD_AMT ASC;

-- 가격 내림차순
SELECT * FROM TB_PRD
ORDER BY PRD_AMT DESC;
```

#### 다중 컬럼 정렬

```sql
-- 상품타입 오름차순, 같은 타입 내에서는 가격 내림차순
SELECT * FROM TB_PRD
ORDER BY PRD_TYPE ASC, PRD_AMT DESC;
```

---

### 다양한 정렬 방식

```sql
-- 1. 컬럼명 사용
ORDER BY PRD_AMT DESC

-- 2. 별칭 사용
SELECT PRD_AMT AS 가격 FROM TB_PRD
ORDER BY 가격 DESC

-- 3. 컬럼 위치 번호 사용
SELECT PRD_TYPE, PRD_AMT FROM TB_PRD
ORDER BY 2 DESC  -- 두 번째 컬럼(PRD_AMT) 기준
```

---

## 7. 조인 (JOIN)

### 조인이란?

**여러 테이블에서 필요한 데이터를 한 번에 가져오는 기술**

```
회원 테이블              회원연락처 테이블
┌──────────┬────────┐  ┌──────────┬──────────┬───────────────┐
│  회원ID  │  이름  │  │  회원ID  │ 구분코드 │    연락처     │
├──────────┼────────┤  ├──────────┼──────────┼───────────────┤
│  A0001   │ 동동일 │  │  A0001   │  휴대폰  │ 010-111-1111  │
│  A0002   │ 동동이 │  │  A0001   │  집전화  │ 062-111-1111  │
│  A0003   │ 동동삼 │  │  A0002   │  집전화  │ 062-222-2222  │
└──────────┴────────┘  └──────────┴──────────┴───────────────┘
```

**질문**: A0001 회원의 이름과 휴대폰 번호는?

---

### 카티션 조인 (Cartesian Join)

**조인 조건 없이 모든 경우의 수를 조합**

```sql
SELECT * FROM 회원, 회원연락처;
```

**결과**: 회원(3행) × 회원연락처(3행) = 9행

---

### 조인 조건 추가

```sql
SELECT *
FROM 회원 A, 회원연락처 B
WHERE A.회원ID = B.회원ID;
```

**결과**: 회원ID가 일치하는 행만 출력 (3행)

---

### 조인 + 일반 조건

```sql
SELECT *
FROM 회원 A, 회원연락처 B
WHERE A.회원ID = B.회원ID      -- 조인 조건
  AND A.회원ID = 'A0001'       -- 일반 조건
  AND B.구분코드 = '휴대폰';   -- 일반 조건
```

---

### FROM 절 별칭 사용

```sql
-- 별칭 사용 (AS 생략)
SELECT A.CUST_ID, A.CUST_NAME, B.BADGE_ID
FROM TB_CUST A, TB_CUST_BADGE B
WHERE A.CUST_ID = B.CUST_ID;
```

**주의**: 별칭 사용 시 이후 원래 테이블명 사용 불가!

```sql
-- ❌ 오류
SELECT TB_CUST.CUST_ID, A.CUST_NAME
FROM TB_CUST A;

-- ✅ 정상
SELECT A.CUST_ID, A.CUST_NAME
FROM TB_CUST A;
```

---

### 조인 조건 최소 개수

**테이블 개수 - 1**

```sql
-- 4개 테이블 조인 → 최소 3개 조인 조건 필요
SELECT *
FROM A, B, C, D
WHERE A.직원ID = B.직원ID    -- 조인 조건 1
  AND B.직원ID = C.직원ID    -- 조인 조건 2
  AND C.직원ID = D.직원ID;   -- 조인 조건 3
```

---

## 8. 표준 조인 (ANSI JOIN)

### INNER JOIN

**두 테이블 간 조건에 일치하는 데이터만 출력**

```
회원          회원연락처
A0001  ───┐  ┌─ A0001
A0002  ───┼──┼─ A0001
A0003      │  ├─ A0002
           └──┤
              └─ A0004
```

#### Oracle 방식

```sql
SELECT *
FROM 회원 A, 회원연락처 B
WHERE A.회원ID = B.회원ID;
```

#### ANSI 방식

```sql
SELECT *
FROM 회원 A
INNER JOIN 회원연락처 B
  ON A.회원ID = B.회원ID;
```

---

### OUTER JOIN

**한쪽 테이블 기준으로 조건 불일치 데이터도 출력**

```
회원          회원연락처
A0001  ───┐  ┌─ A0001
A0002  ───┼──┼─ A0001
A0003  ◄──┘  ├─ A0002  (기준: 회원)
              └─ A0004
```

#### Oracle 방식 ((+) 기호)

```sql
-- 회원 테이블 기준
SELECT *
FROM 회원 A, 회원연락처 B
WHERE A.회원ID = B.회원ID(+);
```

**(+) 기호 규칙:**
- (+)가 붙은 **반대편** 테이블이 기준
- Oracle 전용 문법

#### ANSI 방식

```sql
-- LEFT OUTER JOIN
SELECT *
FROM 회원 A
LEFT OUTER JOIN 회원연락처 B
  ON A.회원ID = B.회원ID;

-- RIGHT OUTER JOIN  
SELECT *
FROM 회원연락처 A
RIGHT OUTER JOIN 회원 B
  ON A.회원ID = B.회원ID;

-- FULL OUTER JOIN
SELECT *
FROM 회원 A
FULL OUTER JOIN 회원연락처 B
  ON A.회원ID = B.회원ID;
```

---

### CROSS JOIN

**모든 조합 (카티션 곱)**

```sql
-- ANSI 방식
SELECT *
FROM 회원 A
CROSS JOIN 회원연락처 B;

-- Oracle 방식
SELECT *
FROM 회원 A, 회원연락처 B;
```

---

### 동등 조인 vs 비동등 조인

#### 동등 조인
**조인 조건이 모두 "="**

```sql
SELECT *
FROM A, B
WHERE A.ID = B.ID;
```

#### 비동등 조인
**조인 조건에 =이 아닌 연산자 포함**

```sql
SELECT *
FROM 급여등급 A, 직원 B
WHERE B.연봉 BETWEEN A.최소연봉 AND A.최대연봉;
```

---

 | 문자열 끝 |
| `*` | 0회 이상 반복 |
| `+` | 1회 이상 반복 |
| `?` | 0회 또는 1회 |
| `[abc]` | a, b, c 중 하나 |
| `[^abc]` | a, b, c 제외 |
| `[0-9]` | 숫자 |
| `\d` | 숫자 (digit) |
| `\w` | 영문자/숫자/_ |

---

# Part 3: 관리구문

## 1. DML (Data Manipulation Language)

### INSERT - 데이터 입력

#### 기본 문법

```sql
INSERT INTO 테이블명 (컬럼1, 컬럼2, ...)
VALUES (값1, 값2, ...);
```

#### 예시

```sql
INSERT INTO TB_CUST (CUST_ID, CUST_NAME, MONEY)
VALUES ('C0001', '홍길동', 10000);
```

---

### INSERT 에러 케이스

#### 1. PK에 NULL 입력

```sql
-- ❌ 오류
INSERT INTO TB_CUST (CUST_ID, CUST_NAME)
VALUES (NULL, '홍길동');
```

#### 2. NOT NULL 컬럼에 NULL

```sql
-- ❌ 오류 (CUST_NAME은 NOT NULL)
INSERT INTO TB_CUST (CUST_ID, CUST_NAME)
VALUES ('C0001', NULL);
```

#### 3. 자료형 불일치

```sql
-- ❌ 오류 (MONEY는 숫자형)
INSERT INTO TB_CUST (CUST_ID, CUST_NAME, MONEY)
VALUES ('C0001', '홍길동', '13점');
```

#### 4. PK 중복

```sql
-- 첫 실행: ✅
INSERT INTO TB_CUST (CUST_ID, CUST_NAME)
VALUES ('C0001', '홍길동');

-- 재실행: ❌ 오류 (PK 중복)
```

#### 5. 컬럼 개수 불일치

```sql
-- ❌ 오류
INSERT INTO TB_CUST (CUST_ID, CUST_NAME)
VALUES ('C0001', '홍길동', 10000);  -- 값 3개
```

---

### INSERT 고급 기법

#### 컬럼 리스트 생략

```sql
-- 모든 컬럼을 순서대로 입력
INSERT INTO TB_CUST
VALUES ('C0001', '홍길동', ... , 10000);
```

**주의**: 실무에서는 권장하지 않음 (유지보수 어려움)

#### DEFAULT 값 활용

```sql
-- 생략한 컬럼은 DEFAULT 값 입력
INSERT INTO TB_CUST (CUST_ID, CUST_NAME)
VALUES ('C0001', '홍길동');
-- MONEY 컬럼은 DEFAULT (NULL 또는 설정값)
```

---

### UPDATE - 데이터 수정

#### 기본 문법

```sql
UPDATE 테이블명
SET 컬럼1 = 값1, 컬럼2 = 값2, ...
WHERE 조건;
```

#### 예시

```sql
-- 특정 고객의 금액 변경
UPDATE TB_CUST
SET MONEY = 20000
WHERE CUST_ID = 'C0001';

-- 여러 컬럼 동시 수정
UPDATE TB_CUST
SET MONEY = 20000
  , CUST_NAME = '김철수'
WHERE CUST_ID = 'C0001';
```

#### ⚠️ WHERE 절 필수!

```sql
-- ❌ 위험! 모든 행이 변경됨
UPDATE TB_CUST
SET MONEY = 0;

-- ✅ 안전
UPDATE TB_CUST
SET MONEY = 0
WHERE CUST_ID = 'C0001';
```

---

### DELETE - 데이터 삭제

#### 기본 문법

```sql
DELETE FROM 테이블명
WHERE 조건;
```

#### 예시

```sql
-- 특정 고객 삭제
DELETE FROM TB_CUST
WHERE CUST_ID = 'C0001';

-- 조건에 맞는 여러 행 삭제
DELETE FROM TB_CUST
WHERE MONEY = 0;
```

#### ⚠️ WHERE 절 필수!

```sql
-- ❌ 위험! 모든 행이 삭제됨
DELETE FROM TB_CUST;

-- ✅ 안전
DELETE FROM TB_CUST
WHERE CUST_ID = 'C0001';
```

---

### MERGE - 병합 (INSERT + UPDATE)

**존재하면 UPDATE, 없으면 INSERT**

```sql
MERGE INTO 직원 A
USING 직원_신입 B
  ON (A.직원ID = B.직원ID)
WHEN MATCHED THEN
  UPDATE SET A.이름 = B.이름
           , A.연봉 = B.연봉
WHEN NOT MATCHED THEN
  INSERT (직원ID, 이름, 연봉)
  VALUES (B.직원ID, B.이름, B.연봉);
```

**동작:**
1. 직원ID가 일치하면 → UPDATE
2. 일치하지 않으면 → INSERT

---

## 2. TCL (Transaction Control Language)

### 트랜잭션이란?

**업무를 수행하기 위한 일련의 단계**

#### 트랜잭션 특징 (ACID)

| 특징 | 영문 | 설명 |
|------|------|------|
| **원자성** | Atomicity | 전부 성공 or 전부 실패 (All or Nothing) |
| **일관성** | Consistency | 트랜잭션 전후 데이터 무결성 유지 |
| **격리성** | Isolation | 다른 트랜잭션과 독립적 실행 |
| **지속성** | Durability | 완료된 트랜잭션은 영구 반영 |

---

### 트랜잭션 예시: 계좌이체

```sql
-- 1. A계좌 잔액 확인
SELECT 잔액 FROM 계좌정보 
WHERE 계좌번호 = 'A계좌' AND 잔액 >= 1000000;

-- 2. A계좌에서 차감
UPDATE 계좌정보 
SET 잔액 = 잔액 - 1000000 
WHERE 계좌번호 = 'A계좌';

-- 3. B계좌에 추가
UPDATE 계좌정보 
SET 잔액 = 잔액 + 1000000 
WHERE 계좌번호 = 'B계좌';

-- 4. 모두 성공하면 영구 반영
COMMIT;
```

**만약 중간에 오류 발생?**
→ ROLLBACK으로 모든 작업 취소!

---

### COMMIT - 영구 반영

```sql
INSERT INTO TAB1 VALUES ('A', 'A');
INSERT INTO TAB1 VALUES ('B', 'B');
COMMIT;  -- 여기까지 영구 저장
```

---

### ROLLBACK - 취소

```sql
INSERT INTO TAB1 VALUES ('A', 'A');
INSERT INTO TAB1 VALUES ('B', 'B');
COMMIT;

INSERT INTO TAB1 VALUES ('C', 'C');
ROLLBACK;  -- 마지막 COMMIT 이후 작업 취소
-- A, B는 남고 C만 취소됨
```

---

### COMMIT / ROLLBACK 예제

```sql
INSERT INTO TAB3 (COL1, COL2) VALUES ('A', 'A');
INSERT INTO TAB3 (COL1, COL2) VALUES ('B', 'B');
COMMIT;  -- A, B 영구 저장

INSERT INTO TAB3 (COL1, COL2) VALUES ('C', 'C');
ROLLBACK;  -- C 취소

INSERT INTO TAB3 (COL1, COL2) VALUES ('D', 'D');
INSERT INTO TAB3 (COL1, COL2) VALUES ('E', 'E');
COMMIT;  -- D, E 영구 저장

SELECT COUNT(*) FROM TAB3;
-- 결과: 4개 (A, B, D, E)
```

---

### SAVEPOINT - 저장점

**특정 지점까지만 ROLLBACK**

```sql
INSERT INTO TAB1 VALUES ('A');
SAVEPOINT SV1;

INSERT INTO TAB1 VALUES ('B');
SAVEPOINT SV2;

INSERT INTO TAB1 VALUES ('C');
SAVEPOINT SV3;

ROLLBACK TO SV2;  -- SV2 이후만 취소
-- 결과: A, B만 남음 (C 취소)

COMMIT;
```

---

### AUTO COMMIT 주의사항

| 구분 | Oracle | SQL Server |
|------|--------|------------|
| **DML** | 수동 COMMIT | 자동 COMMIT |
| **DDL** | 자동 COMMIT | 자동 COMMIT |

```sql
-- Oracle
INSERT INTO TAB1 VALUES ('A');
-- COMMIT 필요!

CREATE TABLE TAB2 (...);
-- 자동 COMMIT됨!
```

---

## 3. DDL (Data Definition Language)

### 자료형

#### VARCHAR2(n) - 가변 문자열

```sql
-- 최대 n바이트까지 저장
CUST_NAME VARCHAR2(100)

-- 'abc' 입력 시 3바이트만 사용
```

#### CHAR(n) - 고정 문자열

```sql
-- 항상 n바이트 사용
CODE CHAR(10)

-- 'abc' 입력 시 10바이트 사용 (나머지 공백)
```

#### NUMBER(n, m) - 숫자

```sql
-- n: 전체 자릿수, m: 소수점 자릿수
NUMBER(5, 2)  -- 999.99
NUMBER(10)    -- 정수 10자리
NUMBER        -- 제한 없음 (실무 권장)
```

#### DATE - 날짜

```sql
-- 날짜 및 시간 저장
REG_DATE DATE

-- TIMESTAMP: 더 정밀한 시간 (밀리초)
```

---

### CREATE TABLE - 테이블 생성

```sql
CREATE TABLE 테이블명 (
    컬럼명1 자료형 [DEFAULT 기본값] [NULL | NOT NULL],
    컬럼명2 자료형 [DEFAULT 기본값] [NULL | NOT NULL],
    ...
);
```

#### 예시

```sql
CREATE TABLE TB_MEMBER (
    MEMBER_ID VARCHAR2(20) NOT NULL,
    MEMBER_NAME VARCHAR2(100) NOT NULL,
    EMAIL VARCHAR2(200),
    JOIN_DATE DATE DEFAULT SYSDATE NOT NULL,
    STATUS VARCHAR2(1) DEFAULT 'Y'
);
```

---

### 제약조건 (Constraints)

#### PRIMARY KEY (PK)

**식별자: NOT NULL + UNIQUE**

```sql
-- 방법 1: 테이블 생성 시
CREATE TABLE TB_MEMBER (
    MEMBER_ID VARCHAR2(20) PRIMARY KEY,
    ...
);

-- 방법 2: ALTER TABLE
ALTER TABLE TB_MEMBER 
ADD CONSTRAINT PK_TB_MEMBER 
PRIMARY KEY (MEMBER_ID);
```

#### UNIQUE KEY (UK)

**중복 불가, NULL 허용**

```sql
ALTER TABLE TB_MEMBER 
ADD CONSTRAINT UK_TB_MEMBER_EMAIL 
UNIQUE (EMAIL);
```

#### NOT NULL

**NULL 입력 불가**

```sql
CREATE TABLE TB_MEMBER (
    MEMBER_NAME VARCHAR2(100) NOT NULL,
    ...
);
```

#### CHECK

**특정 값만 입력 가능**

```sql
ALTER TABLE TB_MEMBER 
ADD CONSTRAINT CK_TB_MEMBER_STATUS 
CHECK (STATUS IN ('Y', 'N'));
```

#### FOREIGN KEY (FK)

**다른 테이블 참조**

```sql
ALTER TABLE TB_ORDER 
ADD CONSTRAINT FK_TB_ORDER_MEMBER 
FOREIGN KEY (MEMBER_ID) 
REFERENCES TB_MEMBER(MEMBER_ID);
```

---

### ALTER TABLE - 테이블 수정

#### 컬럼 추가 (ADD)

```sql
ALTER TABLE TB_CUST 
ADD BIRTH_YEAR VARCHAR2(8);
```

#### 컬럼 삭제 (DROP COLUMN)

```sql
ALTER TABLE TB_CUST 
DROP COLUMN BIRTH_YEAR;
```

#### 컬럼 수정 (MODIFY)

```sql
-- 자료형 변경
ALTER TABLE TB_CUST 
MODIFY (CUST_NAME VARCHAR2(200));

-- NOT NULL 추가
ALTER TABLE TB_CUST 
MODIFY (CUST_NAME VARCHAR2(100) NOT NULL);
```

#### 컬럼 이름 변경 (RENAME COLUMN)

```sql
ALTER TABLE STUDENT 
RENAME COLUMN 학생ID TO STUDENT_ID;
```

---

### DROP TABLE - 테이블 삭제

```sql
-- 테이블만 삭제 (FK 있으면 오류)
DROP TABLE 테이블명;

-- 테이블 + 관련 FK 모두 삭제
DROP TABLE 테이블명 CASCADE CONSTRAINTS;
```

---

### TRUNCATE vs DELETE vs DROP

| 구분 | TRUNCATE | DELETE | DROP |
|------|----------|--------|------|
| **분류** | DDL | DML | DDL |
| **삭제 대상** | 데이터만 | 데이터만 | 테이블 + 데이터 |
| **롤백** | 불가 | 가능 | 불가 |
| **속도** | 빠름 | 느림 | 빠름 |
| **WHERE 조건** | 불가 | 가능 | - |
| **AUTO COMMIT** | 즉시 | 수동 | 즉시 |

```sql
-- DELETE: 조건부 삭제 가능, 롤백 가능
DELETE FROM TB_CUST WHERE CUST_ID = 'C0001';
ROLLBACK;  -- 복구 가능

-- TRUNCATE: 모든 데이터 삭제, 롤백 불가
TRUNCATE TABLE TB_CUST;
-- 복구 불가!

-- DROP: 테이블 자체 삭제
DROP TABLE TB_CUST;
-- 테이블 사라짐
```

---

### RENAME - 테이블명 변경

```sql
RENAME 기존테이블명 TO 새테이블명;

-- 예시
RENAME TB_CUST TO TB_CUSTOMER;
```

---

## 4. DCL (Data Control Language)

### 권한 관리의 필요성

```
USER_A ──→ 조회 요청 ──→ USER_B의 TAB1
         ←── 권한 없음 ←──
```

---

### GRANT - 권한 부여

```sql
GRANT 권한 [ON 객체] TO 사용자 [WITH GRANT OPTION];
```

#### 예시

```sql
-- SELECT 권한 부여
GRANT SELECT ON TAB1 TO USER_A;

-- SELECT, INSERT 권한 부여
GRANT SELECT, INSERT ON TAB1 TO USER_A;

-- 모든 권한 부여
GRANT ALL ON TAB1 TO USER_A;

-- 재부여 권한 포함
GRANT SELECT ON TAB1 TO USER_A WITH GRANT OPTION;
```

---

### WITH GRANT OPTION

**권한 받은 사용자가 다른 사용자에게 재부여 가능**

```sql
-- 관리자 → USER_A (재부여 권한 포함)
GRANT SELECT ON TAB1 TO USER_A WITH GRANT OPTION;

-- USER_A → USER_B (재부여)
GRANT SELECT ON USER_A.TAB1 TO USER_B;
```

---

### REVOKE - 권한 회수

```sql
REVOKE 권한 [ON 객체] FROM 사용자 [CASCADE];
```

#### 예시

```sql
-- SELECT 권한 회수
REVOKE SELECT ON TAB1 FROM USER_A;

-- UPDATE 권한 회수
REVOKE UPDATE ON TAB1 FROM USER_A;

-- 연쇄 회수 (CASCADE)
REVOKE SELECT ON TAB1 FROM USER_A CASCADE;
-- USER_A가 부여한 권한도 모두 회수
```

---

### 권한 부여/회수 예제

```sql
-- 1단계: USER_A → USER_B (재부여 권한 포함)
USER_A: GRANT SELECT, UPDATE ON TAB1 TO USER_B 
        WITH GRANT OPTION;

-- 2단계: USER_B → USER_C (재부여)
USER_B: GRANT SELECT, UPDATE ON USER_A.TAB1 TO USER_C;

-- 현재 상태:
-- USER_B: SELECT, UPDATE 권한
-- USER_C: SELECT, UPDATE 권한

-- 3단계: UPDATE 권한만 회수
USER_A: REVOKE UPDATE ON TAB1 FROM USER_B;

-- 결과:
-- USER_B: SELECT 권한만 남음
-- USER_C: SELECT, UPDATE 권한 (영향 없음)

-- 4단계: SELECT 권한 CASCADE 회수
USER_A: REVOKE SELECT ON TAB1 FROM USER_B CASCADE;

-- 결과:
-- USER_B: 권한 없음
-- USER_C: UPDATE만 남음 (SELECT는 회수됨)
```

---

### ROLE - 권한 묶음

**여러 권한을 하나로 묶어 관리**

```sql
-- ROLE 생성
CREATE ROLE 역할명;

-- ROLE에 권한 부여
GRANT SELECT, INSERT, UPDATE ON TB_MEMBER TO 역할명;
GRANT SELECT ON TB_ORDER TO 역할명;

-- 사용자에게 ROLE 부여
GRANT 역할명 TO USER_A;
```

#### 사전 정의 ROLE (Oracle)

**CONNECT**
- CREATE SESSION
- CREATE TABLE
- CREATE VIEW
- CREATE SYNONYM

**RESOURCE**
- CREATE TABLE
- CREATE PROCEDURE
- CREATE TRIGGER
- CREATE SEQUENCE
- ...

```sql
-- 사용 예시
GRANT CONNECT, RESOURCE TO USER_A;
```
