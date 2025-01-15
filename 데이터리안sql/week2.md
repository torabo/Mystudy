# 1. SQL 숫자 연산
```sql
+ 
- 
*
/ : 나누기
DIV : 몫 나누기
% , MOD : 나머지
```

```sql
ABS() : 절대값
CEIL() : 올림
FLOOR() : 내림
ROUND() : 반올림
POW(값,x), POWER() : x거듭제곱
SQRT() : 제곱근
```
>tip1 

[사칙연산 공식 설명](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html)

[수학 관련 함수 공식 설명](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html)

# 2. 집계함수 
```sql
COUNT() -- 개수
SUM() -- 합계
AVG() -- 평균
MIN() -- 최소값
MAX() -- 최대값
```

>tip2
Null 값이 존재할때 :

NULL을 제외하고 집계함

# 3. 데이터 요약하기
## 1. GROUP BY()
```sql
-- 예시
SELECT day
     , time
     , size
     , COUNT(*)
FROM tips
GROUP BY day, time, size
```

## 2. HAVING

group by 의 where 같은 느낌임

```sql
-- 사용 예시
SELECT day
     , time
     , COUNT(*)
FROM tips
WHERE sex = 'Female'   -- 여성이 결제한 데이터만 뽑아 보겠다
GROUP BY day, time     -- 요일 시간대별로 GROUP BY 해서 보겠다
HAVING COUNT(*) >= 5   -- GROUP BY 후 데이터가 5개 이상인 그룹의 데이터만 보겠다
```


# 4. 조건문

## 1. CASE 문법
```sql
-- CASE 쿼리 예시
SELECT *
     , CASE
           WHEN size >= 5 THEN '5인 이상'
           ELSE '5인 미만'
       END size_category
FROM tips
```
else 안쓰면 나머지는 NULL로 처리한다.

CASE 쓸때 when은 순서대로 필터해서 값 매겨준다

다음 두개의 코드는 같은 코드임

```sql
-- 1
SELECT *
      , CASE 
          WHEN total_bill > = 40 THEN '큰 금액'
          WHEN total_bill > = 10 THEN  '평균 금액' -- 첫 번째 조건에서 걸러진 나머지 데이터
          ELSE '적은 금액'
        END
FROM tips
```

```sql
-- 2)
SELECT *
      , CASE 
          WHEN total_bill > = 40 THEN '큰 금액'
          WHEN total_bill > = 10 AND total_bill < 40 THEN  '평균 금액'
          ELSE '적은 금액'
        END
FROM tips
```


## 2. IF() 문법
IF(조건, True, Else)
```sql
-- IF() 쿼리 예시
SELECT *
     , IF(size >= 5, '5인 이상', '5인 미만') AS size_category
FROM tips
```