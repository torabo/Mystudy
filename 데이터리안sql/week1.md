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
# 2. 데이터 필터링하기

#### 1. 비교연산자 (=, > , >=, <, <=, <>, !=)
- 파이썬등등의 언어랑 다르게 '=='이 아닌 '='를 사용한다. 
- <>는 !=랑 같다. (차이는없는듯?)

#### 2. 논리연산자(AND, OR)

#### 3. 여러개의 데이터 한번에 찾기 (IN / BETWEEN)
- between을 쓸때 (between 값1 and 값2) 값1, 값2도 포함시킨다
#### 4. 값이 없는 데이터 찾기 (IS NULL / IS NOT NULL)
#### 5. 패턴에 맞는 문자 찾기(LIKE / NOT LIKE)
- 실행속도가 오래걸리므로 최대한 지양하는게 맞을것 같다

> tip2 : 패턴에 맞는 문자 찾을때 sql 정규표현식('sql regular expression') 쓰면 복잡한 문자열 패턴 추출가능하다. 