# 1 WHERE절 서브쿼리

## 1. 단일행 서브쿼리
 Q. 평균 영수 금액보다 더 많이 지불한 경우를 모두 출력해 주세요.
> 서브쿼리없이 풀기
```sql
-- 계산 결과: 19.785943
SELECT AVG(total_bill)
FROM tips

SELECT *
FROM tips
WHERE total_bill > 19.785943
```
> 서브쿼리 사용
```sql
SELECT *
FROM tips
WHERE total_bill > (SELECT AVG(total_bill) FROM tips) -- 서브쿼리의 결과값은 1개
```

## 2. 다중행 서브쿼리
Q. 요일별 판매 금액이 1500불 이상인 날의 결제 내역을 모두 출력해 주세요.
> 서브쿼리없이 풀기
```sql 
-- 계산 결과: Sun, Sat
SELECT day
FROM tips
GROUP BY day
HAVING SUM(total_bill) >= 1500

SELECT *
FROM tips
WHERE day IN ('Sun', 'Sat')
```
> 서브쿼리 사용
```sql
SELECT *
FROM tips
WHERE day IN (
  SELECT day
  FROM tips
  GROUP BY day
  HAVING SUM(total_bill) >= 1500
) -- 서브쿼리의 결과값은 컬럼 1개, 로우 N개
```

## 3. 다중컬럼 서브쿼리

> 서브쿼리없이 풀기
```sql 
SELECT day, MAX(total_bill)
FROM tips
GROUP BY day

SELECT *
FROM tips
WHERE (day, total_bill) IN (('Thur', 43.11), ('Fri', 40.17), ('Sat', 50.81), ('Sun', 48.17))
```
> 서브쿼리 사용
```sql
SELECT *
FROM tips
WHERE (day, total_bill) IN (
  SELECT day, MAX(total_bill)
  FROM tips
  GROUP BY day
)
```
# 2. FROM 절 서브쿼리
> FROM 사용
```sql
-- 괄호 안의 쿼리 결과물을 daily라는 이름을 가진 테이블처럼 활용할 수 있음
-- 괄호를 닫고 Alias를 적어줘야 함 AS는 생략 가능

SELECT ROUND(AVG(daily.sales), 2) daily_sales_avg
FROM (
  SELECT day
       , SUM(total_bill) sales
  FROM tips
  GROUP BY day
) AS daily
```
> WITH문 사용
```sql
-- FROM 절 서브쿼리와 마찬가지로, 쿼리 결과물을 daily라는 이름을 가진 테이블처럼 활용할 수 있음

WITH daily AS (
  SELECT day
       , SUM(total_bill) sales
  FROM tips
  GROUP BY day
)

SELECT ROUND(AVG(daily.sales), 2) daily_sales_avg
FROM daily
```
# 3.심화

## SELECT절 서브쿼리
Q. 각 영수 금액이, 이 레스토랑의 전체 매출액에서 차지하는 비율을 계산해 주세요. 비율은 반올림하여 소수점 둘째 자리까지만 출력해 주세요. 영수 금액이 높은 것부터 출력해 주세요.
> 서브쿼리 없이
```sql
-- 계산 결과: 4827.77
SELECT SUM(total_bill) 
FROM tips

SELECT day
     , total_bill
     , total_bill * 100.0 / 4827.77 AS pct
FROM tips
ORDER BY total_bill DESC
```
> 서브쿼리 사용
```sql
SELECT t1.day
     , t1.total_bill
     , ROUND(t1.total_bill * 100.0 / (SELECT SUM(total_bill) FROM tips t2), 2) sales
FROM tips t1
ORDER BY t1.total_bill DESC
```