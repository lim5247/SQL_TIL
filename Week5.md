# SQL_BASIC 5주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_5th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**5주차 과제는 문제 풀이를 중심으로**, 강의에서 제시된 예제 문제 중 **3 문제 이상을 선택하여 직접 풀어본 뒤**, 강의 영상의 풀이와 비교해 **틀린 부분, 맞은 부분, 새롭게 배운 개념**을 구체적으로 정리해주세요. (적어도 4문제는 정리해야 합니다.) 완성된 과제는 Gihub에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀(수행 인증샷은 필수입니다.)** 



## SQL_BASIC_5th

### 섹션 5. 데이터 탐색 - 변환

### 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

### 4-5. 시간 데이터 연습문제 1~2번

### 4-5. 시간 데이터 연습문제 3~5번

### 4-6. 조건문 (CASE WHEN, IF)

### 4-7. 조건문 연습 문제

### 4-8. 정리

### 4-9. BigQuery 공식 문서 확인하는 법

(강의에서 연습문제가 많아서 따로 프로그래머스 문제 과제는 없습니다.)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>



<!-- 여기까진 그대로 둬 주세요-->

---

# 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

~~~
✅ 학습 목표 :
* 날짜 및 시간 데이터에 대해서 더 자세히 설명할 수 있다. 
* CURRENT_TIME, EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME 을 설명할 수 있다. 
~~~

- CURRENT_DATE([time_zone]) AS ...: 현재 날짜
- CURRENT_DATETIME([time_zone]) AS ...: 현재 날짜 + 시간
- EXTRACT({part} FROM datetime_expression) AS ...: DATETIME에서 일부만 추출하고 싶을 때
  - {part}: DATE, TEAR, MONTH, DAY, HOUR, MINUTE, DAYOFWEEK 등 시간표현
- DATETIME_TRUNC(A, B): A에서 B 뒤에 부분을 자름 -> 기본 꼴로 정리
- PARSE_DATETIME('문자열의 형태', 'DATETIME문자열') AS datetime: 문자열DTETIME을 DATETIME 타입으로 바꾸기
  - ex) 문자열의 형태: '%y-%m-%d'
- FORMAT_DATETIME('%C', DATETIME"2024-01-11 12:35:35") AS formatted: DATETIME타입 데이터를 특정 형태의 문자열 데이터로 변환
- LAST_DAY(DATETIME): 월의 마지막 값을 반환, 인자를 주면 월이 아니라 기준값의 마지막날을 줌
- DATETIME_DIFF(첫 DATETIME, 두번째 DATETIME, 궁금한 차이) : 두 DATETIME의 차이를 알고 싶은 경우


# 4-6. 조건문(CASE WHEN, IF)

~~~
✅ 학습 목표 :
* 조건문 함수의 기능을 이해하고, 설명할 수 있다. 
~~~

- 조건문: 조건이 충족되면 특정 동작 수행 / 조건에 따라 다른 값을 표시하고 싶을 때 사용
  - CASE WHEN / IF 문
- 사용하는 경우 ex) 특정 카테고리를 하나로 합치는 전처리

**CASE WHEN** 여러 조건이 있을 경우 유용  
~~~sql
SELECT
 CASE
  WHEN 조건1 THEN 조건1이 참일 때 결과
  WHEN 조건2 THEN 조건2가 참일 때 결과
  ELSE 그 외 조건일 경우 결과
 END AS 새로운_컬럼_이름
FROM
~~~
* 조건 순서 주의!!!

**IF** 단일 조건일 떄 유용
~~~sql
IF(조건문, TRUE일 때의 값, FALSE일 때의 값) AS 새로운_컬럼_이름
~~~




 # 4-5. 시간 데이터 연습문제 & 4-7. 조건문 연습 문제

~~~
✅ 학습 목표 :
* 4-5, 4-7 각각에서 두 문제 이상 (최소 4문제) 푼 내용 정리하기
~~~
# 2. 배틀이 일어난 시간(battle_datetime)을 기준으로, 오전 6시에서 오후 6시 사이에 일어난 배틀의 수를 계산해주세요
```Sql
# SOL 1
SELECT 
   COUNT(DISTINCT id) AS battle_cnt
FROM Basic.battle
WHERE
   EXTRACT(HOUR FROM battle_datetime) >= 6
   AND EXTRACT(HOUR FROM battle_datetime) <= 18
```
```Sql
# SOL 2
SELECT 
   COUNT(DISTINCT id) AS battle_cnt
FROM Basic.battle
WHERE
   EXTRACT(HOUR FROM battle_datetime) BETWEEN 6 and 18
```
-> SOL 2 확인, EXTRACT문 반복을 BETWEEN으로 해결 가능

# 3. 각 트레이너별로 그들이 포켓몬을 포획한 첫날(catch_date)를 찾고, 그 날짜를 'DD/MM/YYYY' 형식으로 출력해주세요
~~~sql
SELECT
  trainer_id,
  FORMAT_DATE("%d/%m/%Y", minday)
FROM(
  SELECT 
    trainer_id, 
    MIN(DATE(catch_datetime, "Asia/Seoul")) AS minday
  FROM Basic.trainer_pokemon
  GROUP BY trainer_id
)
ORDER BY trainer_id;  
~~~
-> minday 이용 확인
<br>
________________________________________________________________

<br>
# 1.포켓몬의 Speed가 70이상이면 빠름, 그렇지 않으면 느림으로 표시하는 새로운 컬럼 Spee_Category 생성
~~~sql
SELECT
  *, 
  IF(speed >= 70, "빠름", "느림") AS Speed_Category
FROM Basic.pokemon;
~~~

# 4. 각 트레이너의 배지 개수(badge_count)를 기준으로 5개 이하면 Bigginer, 6개에서 8개 사이면 Intermediate 그 이상이면 Advanced로 분류
~~~sql
SELECT 
  name, 
  badge_count,
  CASE 
    WHEN badge_count >= 9 THEN "Advanced"
    WHEN badge_count BETWEEN 6 and 8 THEN "Intermediate"
    ELSE "Bigginer"
  END AS level
FROM Basic.trainer;
~~~


<br>

<br>

---

# 확인문제

## 문제 1

> **🧚Q. 광윤이는 카페 주문 로그 데이터(order_log)를 분석하여, '오전(0시-11시)'과 '오후(12시-23시)'의 주문 건수를 집계하려고 합니다. 광윤이가 작성한 다음 SQL 쿼리 중 문법적으로 틀렸거나 의도한 결과가 나오지 않는 것을 모두 골라보세요. (복수 선택 가능)**

~~~sql
1. SELECT 
   IF(EXTRACT(HOUR FROM order_time) < 12, '오전', '오후') AS time_type,
   COUNT(*)
   FROM order_log
   GROUP BY time_type;

2. SELECT 
   DATETIME_TRUNC(order_time, HOUR) AS truncated_hour,
   COUNT(*)
   FROM order_log
   WHERE order_time BETWEEN '2021-01-01' AND '2021-12-31'
   GROUP BY order_time;

3. SELECT 
   FORMAT_DATETIME(order_time, '%H') AS order_hour,
   COUNT(*)
   FROM order_log
   GROUP BY 1;

4. SELECT 
    CASE 
      WHEN EXTRACT(HOUR FROM order_time) BETWEEN 0 AND 11 THEN '오전'
      ELSE '오후'
    AS time_group,
    COUNT(*)
   FROM order_log
   GROUP BY time_group;
~~~

<!-- 틀린쿼리에 대한 오류의 원인도 같이 작성해주세요. 문제에서 제공된 order_time 컬럼은 DATETIME type의 데이터를 가지고 있다고 가정합니다. -->

~~~
2. 
~~~



## 문제 2

> **🧚Q. 예운이는 포켓몬 타입에 따라 설명을 부여하는 쿼리를 작성했습니다. type 1 컬럼의 값에 따라 조건을 분기했으며, 다음 SQL 쿼리를 실행했습니다.**

~~~sql
SELECT name,
       CASE 
         WHEN type1 = 'Fire' THEN 'Hot'
         WHEN type1 = 'Water' THEN 'Cool'
         ELSE 'Normal'
       END AS type_description
FROM pokemon;
~~~

> **다음 중 type_description의 결과가 'Normal'로 출력될 포켓몬은?**

| **name**   | **type1** |
| ---------- | --------- |
| Pikachu    | Electric  |
| Charmander | Fire      |
| Squirtle   | Water     |
| Bulbasaur  | Grass     |

<!-- 근거와 함께 답을 작성해주세요 -->

~~~
여기에 답을 작성해주세요!
~~~



<br>

### 🎉 수고하셨습니다.
