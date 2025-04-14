# TOP N 쿼리
Top N 쿼리는 데이터베이스에서 상위 N개의 행을 조회하는 쿼리이다. <br>
각 데이터베이스 시스템별로 구현 방식이 다르다.

## Oracle

> #### ROWNUM 사용 (12c 이전)
```SQL
-- 단순 조회
SELECT * FROM employees 
WHERE ROWNUM <= 5;

-- 정렬된 결과에서 상위 N개 (서브쿼리 필요)
SELECT * FROM (
    SELECT * FROM employees 
    ORDER BY salary DESC
) WHERE ROWNUM <= 5;
```

> #### FETCH FIRST 사용 (12c 이후)
```SQL
SELECT * FROM employees 
ORDER BY salary DESC
FETCH FIRST 5 ROWS ONLY;

-- 동점 포함
FETCH FIRST 5 ROWS WITH TIES;
```

## SQL Server

> #### TOP 절 사용
```SQL
-- 단순 상위 N개
SELECT TOP 5 * FROM employees;

-- 퍼센트로 지정
SELECT TOP 10 PERCENT * FROM employees;

-- 동점 포함
SELECT TOP 5 WITH TIES * 
FROM employees 
ORDER BY salary DESC;
```

## MySQL

> #### LIMIT 사용
```SQL
-- 상위 N개
SELECT * FROM employees 
ORDER BY salary DESC 
LIMIT 5;

-- 특정 위치에서 시작
SELECT * FROM employees 
ORDER BY salary DESC 
LIMIT 5, 5;  -- 6번째부터 5개
```
