
> [리텐션에 대해서
](https://datarian.io/blog/classic-retention)

# 1. 클래식 리텐션
중간에 진칸 있으면 없는거임
```sql
WITH records_preprocessed AS (
    SELECT r.customer_id
         , DATE_FORMAT(c.first_order_date, '%Y-%m-01') AS first_order_month
         , DATE_FORMAT(r.order_date, '%Y-%m-01') AS order_month
    FROM records AS r
         INNER JOIN customer_stats c ON r.customer_id = c.customer_id
)

SELECT first_order_month
     , COUNT(DISTINCT customer_id) AS month0
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 1 MONTH) = order_month THEN customer_id END) AS month1
from records_preprocessed
GROUP BY first_order_month
ORDER BY first_order_month
```


# 2. 롤링 리텐션
중간에 빈칸 있어도 있는걸로 간주
```sql
WITH customers AS ( 
     SELECT customer_id
          , DATE_FORMAT(first_order_date, '%Y-%m-01') AS first_order_month
          , DATE_FORMAT(last_order_date, '%Y-%m-01') AS last_order_month
     FROM customer_stats c
)

SELECT first_order_month
     , COUNT(DISTINCT customer_id) AS month0
     , COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 1 MONTH) <= last_order_month THEN customer_id END) AS month1
FROM customers

GROUP BY first_order_month
ORDER BY first_order_month
```