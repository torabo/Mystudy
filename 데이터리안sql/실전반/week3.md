# 1.퍼널분석
>퍼널분석이란?

&nbsp; 퍼널 분석은 사용자들이 우리가 설계한 사용자 경험 루트를 따라 잘 도착하고 있는지 확인해 보기 위해 최초 유입부터 최종 목적지까지 단계를 나누어서 살펴보는 분석 기법. 

&nbsp; 상품을 판매하는 커머스 서비스라면 ‘구매’까지의 퍼널을 볼 수도 있고, ‘회원 가입’까지의 퍼널을 살펴보는 등 실무에서 다양하게 활용할 수 있다.

>예시코드
```sql
WITH pv AS (
  SELECT user_pseudo_id
       , ga_session_id
       , event_timestamp_kst AS pv_at
       , source
       , medium
  FROM ga
  WHERE page_title = '백문이불여일타 SQL 캠프 입문반'
    AND event_name = 'page_view'
), scroll AS (
  SELECT user_pseudo_id
       , ga_session_id
       , event_timestamp_kst AS scroll_at
  FROM ga
  WHERE page_title = '백문이불여일타 SQL 캠프 입문반'
    AND event_name = 'scroll'
), click AS (
  SELECT user_pseudo_id
       , ga_session_id
       , event_timestamp_kst AS click_at
  FROM ga 
  WHERE page_title = '백문이불여일타 SQL 캠프 입문반'
		AND event_name IN ('SQL_basic_form_click', 'SQL_basic_1day_form_click', 'SQL_package_form_click')
)

SELECT pv.source
     , pv.medium
     , COUNT(DISTINCT pv.user_pseudo_id, pv.ga_session_id) AS pv
     , COUNT(DISTINCT scroll.user_pseudo_id, scroll.ga_session_id) AS scroll
     , COUNT(DISTINCT click.user_pseudo_id, click.ga_session_id) AS click
     , COUNT(DISTINCT scroll.user_pseudo_id, scroll.ga_session_id) / COUNT(DISTINCT pv.user_pseudo_id, pv.ga_session_id) AS pv_scroll_rate
     , COUNT(DISTINCT click.user_pseudo_id, click.ga_session_id) / COUNT(DISTINCT scroll.user_pseudo_id, scroll.ga_session_id) AS scroll_click_rate
     , COUNT(DISTINCT click.user_pseudo_id, click.ga_session_id) / COUNT(DISTINCT pv.user_pseudo_id, pv.ga_session_id) AS pv_click_rate
FROM pv
     LEFT JOIN scroll ON pv.user_pseudo_id = scroll.user_pseudo_id
                      AND pv.ga_session_id = scroll.ga_session_id
                      AND pv.pv_at <= scroll.scroll_at
     LEFT JOIN click ON scroll.user_pseudo_id = click.user_pseudo_id
                     AND scroll.ga_session_id = click.ga_session_id
                     AND scroll.scroll_at <= click.click_at
GROUP BY pv.source, pv.medium
ORDER BY click DESC, pv DESC
```

# 2.정규표현식
LIKE로 구할 수 없는 문자열 가져오는 방법, LIKE 쓸 수 있으면 LIKE를 쓰는게 더 효율적이다.

[정규표현식 튜토리얼](https://regexone.com/lesson/introduction_abcs)
>예시모음
- \d : 숫자 나옴
- [^abc] : a랑b랑c는 빼고
- [A-z] : 영어 대문자 A부터 소문자z 까지
- [cmf]an : 제일앞은 c,m,f 중에 하나, 뒤에는 an
- aa+ : a나오고 뒤에 a가 또 반복되는거 찾을때 예(aaaa) , (a는 포함안됨)
- aa* : a나오고 뒤에 a는없다고 생각하고 또 반복되는거 찾음 예(a는 포함됨)
- ^start : start로 시작하는거 찾아줌 (^는 시작하는거찾을때)
- \$end : end로 끝나는거 찾아줌 ($는 끝나는거 찾을때)

나머지는 구글링 하면서풀면됨
---
REGEXP

REGEXP_SUBSTR 사용

>sql 예시
```sql
SELECT *
FROM station
WHERE address REGEXP '망원동'
```
```sql
SELECT page_location
     , REPLACE(REGEXP_SUBSTR(page_location, 'utm_source=[A-Za-z0-9-_]*'), 'utm_source=', '') AS source
FROM ga
LIMIT 100
```