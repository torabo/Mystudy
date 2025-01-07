# 1. 데이터 추출하기
SELECT, FROM, LIMIT
```sql 

SELECT --특정 열 선택하기
columnsname

FROM --테이블 이름
tablename

LIMIT --특정 개수의 데이터 가져오기
5

--SELECT *(asterisk) :  모든 열 선택하기

```
DISTINCT
```sql

SELECT DISTINCT name  -- 중복 제거하기 
FROM station

```
AS
```sql
SELECT address AS station_address  -- 별칭 붙이기
FROM station

/*
별칭 붙일때 as는 생략 가능함, 띄어쓰기 포함하려면 '' 사용해서 묶어주면 됌
*/
```
tip1: [가독성을 높이는 습관](https://datarian.io/blog/good-sql-code)
# 2.