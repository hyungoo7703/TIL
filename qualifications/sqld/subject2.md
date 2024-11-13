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
