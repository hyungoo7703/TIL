# 윈도우 함수
윈도우 함수는 행과 행 간의 관계를 쉽게 정의하고 계산하기 위한 함수

<hr>

### 윈도우 함수의 주요 특징

#### 행 단위 처리
→ 집계함수는 그룹당 하나의 결과만 반환하지만, 윈도우 함수는 모든 행이 유지되며 각 행마다 결과 반환한다. <br>
　(집계 함수도 OVER 절과 함께 사용하면 윈도우 함수처럼 행 단위로 결과를 반환할 수 있다.)

#### GROUP BY와의 차이
→ GROUP BY는 행을 그룹화하여 집약(축소)하지만, 윈도우 함수는 행 수가 줄어들지 않는다.

### 윈도우 함수의 장점
1. 복잡한 행간 계산을 단순화
2. 서브쿼리 없이 데이터 분석 가능
3. 성능 최적화된 데이터 처리

### 기본 문법
```SQL
함수(컬럼) OVER (
    PARTITION BY 그룹화할_컬럼
    ORDER BY 정렬할_컬럼
    ROWS BETWEEN 시작행 AND 끝행
)
```

### 실제 예시1
[상황] <br>
부서별 급여 순위 계산
```SQL
SELECT 
    employee_name,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as salary_rank
FROM employees;
```
[결과 예시]
```SQL
employee_name | department | salary | salary_rank
--------------+------------+--------+-------------
김철수        | 영업부      | 5000   | 1
이영희        | 영업부      | 4500   | 2
박지민        | 개발부      | 5500   | 1
최동욱        | 개발부      | 5000   | 2
```

### 실제 예시2
[상황] <br>
누적 합계 계산
```SQL
SELECT 
    sale_date,
    amount,
    SUM(amount) OVER (ORDER BY sale_date) as running_total
FROM sales;
```
[결과 예시]
```SQL
sale_date  | amount | running_total
-----------+--------+--------------
2024-01-01 | 100    | 100
2024-01-02 | 150    | 250
2024-01-03 | 200    | 450
```
