# 1. 데이터 연결하기 2 

## 1. LEFT JOIN, RIGHT JOIN 
[벤다이어그램으로 JOIN 설명](https://sql-joins.leopard.in.ua/)

```sql
SELECT r.restaurant_category
     , COUNT(r.id) AS cnts
FROM restaurants AS r --기준
    LEFT JOIN orders AS o ON r.id = o.restaurant_id
WHERE o.id IS NULL
GROUP BY r.restaurant_category
```

## 2. Self JOIN 

```sql
SELECT *
FROM employees AS e
     INNER JOIN employees AS m ON e.manager_id = m.employee_id
ORDER BY e.employee_id
```