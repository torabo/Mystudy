# 1. RFM 고객 세분화 분석

고객들의 등급을 나눌때, 또는 고객을 그룹별로 나누어 별도의 푸시등을 보내고 싶을때 '고객 세분화 모형' 이용함. RFM은 그 모형 중 하나

- Recency : 얼마나 최근에 구매했는지
- Frequency : 얼마나 자주 구매했는지
- Monetary : 얼마나 많은 금액을 구매했는지

[RFM에 관해서](https://velog.io/@c1035cc/RFM)

```sql
SELECT CASE 
					 WHEN last_order_date >= '2020-12-01' THEN 'recent' 
					 ELSE 'past' 
			 END AS Recency
     , CASE 
	 				 WHEN cnt_orders >= 3 THEN 'high' 
					 ELSE 'low' 
			 END AS Frequency
     , CASE 
					 WHEN sum_sales >= 500 THEN 'high' 
					 ELSE 'low'
			 END AS Monetary
     , COUNT(customer_id) AS Customers
FROM customer_stats
GROUP BY Recency, Frequency, Monetary
```
GROUP BY 에 쓰는거는 ' '로 묶으면 오류 나더라

# 2. 피봇테이블
Pivot : 축을 바꾼다는 것

```sql
SELECT region AS Region
     , COUNT(DISTINCT CASE WHEN category = 'Furniture' THEN order_id END) AS Furniture
FROM records
GROUP BY region
```

# 3. 데이터 연결하기

## 3.1 UNION , UNION ALL
```sql
SELECT *
FROM customers_before_2021

UNION ALL

SELECT *
FROM customers_after_2021
```

## 3.2 INNER JOIN
```sql
SELECT *
FROM orders AS o
	   INNER JOIN customers AS c ON o.customer_id = c.customer_id
WHERE c.country = 'France'
```

INNER JOIN 쓸때 WHERE 처럼 BETWEEN 같은것도 쓸 수 있음~!

예)
```sql
SELECT *
FROM students s
    INNER JOIN grades g ON s.marks BETWEEN g.min_mark AND g.max_mark
```