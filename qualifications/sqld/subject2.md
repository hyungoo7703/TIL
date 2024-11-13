# SQL[개발자] 과목2

<hr>

## SQL 기본

## SQL 활용

### 1. 서브쿼리
[정의] <br>
서브쿼리는 하나의 SQL 문 안에 포함되어 있는 또 다른 SQL 문을 의미

#### 서브쿼리 사용 가능 위치
+ SELECT 절
+ FROM 절
+ WHERE 절
+ HAVING 절
+ ORDER BY 절
+ INSERT 문의 VALUES 절
+ UPDATE 문의 SET 절

#### 서브쿼리의 주요 특징
+ 괄호로 감싸서 사용
+ 단일 행 또는 복수 행 비교 연산자와 함께 사용 가능
+ 서브쿼리 내에서는 ORDER BY 사용 불가

> #### 단일 행 또는 복수 행 비교 연산자와 함께 사용 가능하다?
☞ 서브쿼리의 결과가 반환하는 행의 개수에 따라 사용할 수 있는 비교 연산자가 다르다는 의미이다.

##### 단일행 비교 연산자(서브쿼리가 하나의 행만 반환할 때 사용)
[예시 코드]
```SQL
-- 평균 급여보다 많이 받는 직원 조회
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

##### 다중행 비교 연산자(서브쿼리가 여러 행을 반환할 때 사용)
[예시 코드]
```SQL
-- IT 부서 직원들의 급여와 같은 급여를 받는 직원 조회
SELECT name, salary
FROM employees
WHERE salary IN (SELECT salary FROM employees WHERE dept = 'IT');
```

#### 서브쿼리 분류
![image](https://github.com/user-attachments/assets/20ec17a0-1859-4fa9-9b88-9c56742e92c5)

다중행 다중열 서브쿼리도 존재한다. (여러 행, 여러 컬럼 반환)

### 2. 집합 연산자
여러 SELECT 쿼리를 결합할 때 유용하게 사용하는 연산자.

+ **UNION**: 두 개의 SELECT 쿼리 결과를 결합하여 중복을 제거한 결과를 반환
+ **UNION ALL**: 두 개의 SELECT 쿼리 결과를 결합하여 중복을 포함한 모든 결과를 반환
+ **INTERSECT**: 두 개의 SELECT 쿼리 결과에서 공통된 결과만 반환
+ **MINUS, EXCEPT**: 첫 번째 SELECT 쿼리 결과에서 두 번째 SELECT 쿼리 결과를 뺀 결과를 반환
  + **MINUS** - Oracle
  + **EXCEPT** - SQL Server

### 3. 그룹 함수

#### ROLLUP
[기본 개념]
+ 상위 레벨에서 하위 레벨로 순차적으로 데이터를 집계하는 계층적 구조로 데이터를 집계
+ 왼쪽에서 오른쪽으로 계층 구조를 따라 집계

[예시 코드]
```SQL
SELECT department, region, SUM(sales) as total_sales
FROM sales
GROUP BY ROLLUP(department, region);
```
이 쿼리는 다음과 같은 결과를 생성: <br>
department와 region별 합계, department별 합계, 전체 총계

#### CUBE
[기본 개념]
+ 모든 가능한 조합에 대한 소계를 생성
+ N개 컬럼에 대해 2^N개의 그룹화 조합 생성

[예시 코드]
```SQL
SELECT department, region, SUM(sales) as total_sales
FROM sales
GROUP BY CUBE(department, region);
```
이 쿼리는 다음 조합을 생성: <br>
(department, region) <br>
(department) <br>
(region) <br>
() → 전체 총계

#### GROUPING SETS
[기본 개념]
+ 원하는 그룹화 조합만 선택적으로 지정 가능
+ UNION ALL을 사용한 결과와 동일한 효과
+ 컬럼 순서가 결과에 영향을 미치지 않음

[예시 코드]
```SQL
SELECT department, region, SUM(sales) as total_sales
FROM sales
GROUP BY GROUPING SETS(
    (department, region),
    (department),
    ()
);
```

### 4. 윈도우 함수
윈도의 함수에 대한 설명은 TIL에 정리한 [윈도우 함수](https://github.com/hyungoo7703/TIL/blob/main/sql/%EC%9C%88%EB%8F%84%EC%9A%B0-%ED%95%A8%EC%88%98.md) 참조.

### 5. Top N 쿼리
Top N 쿼리에 대한 설명은 TIL에 정리한 [Top N 쿼리](https://github.com/hyungoo7703/TIL/blob/main/sql/TOP-N-%EC%BF%BC%EB%A6%AC.md) 참조.

### 6. 계층형 질의와 셀프 조인

#### 계층형 질의
테이블의 계층적 데이터(주로 부모-자식 관계를 가진 데이터)를 처리할 때 사용한다.

> #### Oracle 계층형 질의
[기본 문법]
```SQL
SELECT [LEVEL], column, ...
FROM table
START WITH condition
CONNECT BY [NOCYCLE] PRIOR condition
[ORDER SIBLINGS BY column];
```
+ **LEVE**L: 계층의 깊이 (루트 = 1)
+ **START WITH 절**: 계층 구조의 루트(row)를 지정
+ **CONNECT BY 절**: 계층 구조를 정의하는 데 사용, 부모-자식 관계를 지정

> ##### PRIOR(Oracle의 계층형 질의에서 사용되는 특별한 연산자)
+ 계층형 구조에서 현재 행과 부모/자식 행을 연결할 때 사용
+ CONNECT BY 절에서 사용되어 계층 구조의 방향을 결정

[사용 방식] <br>
PRIOR의 위치는 "이미 처리된" 데이터 <br>
1. 부모에서 자식 방향 (Top-down):
```SQL
CONNECT BY PRIOR 자식컬럼 = 부모컬럼
```
2. 자식에서 부모 방향 (Bottom-up):
```SQL
CONNECT BY 자식컬럼 = PRIOR 부모컬럼
```

[예시]
```SQL
-- 조직도를 위에서 아래로 전개
SELECT employee_id, manager_id
FROM employees
START WITH manager_id IS NULL
CONNECT BY PRIOR employee_id = manager_id;

-- 조직도를 아래에서 위로 전개
SELECT employee_id, manager_id
FROM employees
START WITH employee_id = 100
CONNECT BY PRIOR manager_id = employee_id;
```

> ##### SIBLINGS(Oracle 계층형 질의에서 ORDER BY 절과 함께 사용되는 키워드로, 같은 레벨(형제 노드)에 있는 데이터들을 정렬할 때 사용)
+ 같은 부모를 가진 형제 노드들끼리만 정렬
+ 계층 구조를 유지하면서 정렬 수행

> #### SQL Server 계층형 질의
[기본 문법 (CTE 사용)]
```SQL
WITH RecursiveCTE AS (
    -- 기준 멤버(Anchor)
    SELECT column, 0 AS Level
    FROM table
    WHERE condition

    UNION ALL

    -- 재귀 멤버(Recursive)
    SELECT t.column, r.Level + 1
    FROM table t
    INNER JOIN RecursiveCTE r ON condition
)
SELECT * FROM RecursiveCTE;
```

#### Oracle, SQL Server 계층형 질의의 주요 차이점
+ 레벨 시작값 <br>
Oracle: LEVEL은 1부터 시작 <br>
SQL Server: 사용자가 정의 (일반적으로 0부터 시작) <br>
+ 구현 방식 <br>
Oracle: 전용 키워드 제공 <br>
SQL Server: 재귀 CTE 사용 <br>
+ 기능 <br>
Oracle: 더 많은 계층형 전용 기능 제공 <br>
SQL Server: CTE의 유연성을 활용한 구현 필요 <br>
+ 성능 <br>
Oracle: 계층형 질의에 최적화 <br>
SQL Server: 재귀 제한 있음 (기본 100회)

#### 셀프 조인

### 7. PIVOT 절과 UNPIVOT절

### 8. 정규 표현식

## 관리 구문

### 1. DML

### 2. TCL

### 3. DDL

### 4. DCL
