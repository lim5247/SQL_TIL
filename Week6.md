# SQL_BASIC 6주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_6th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**6주차 과제는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_6th

### 섹션 6. 다량의 자료를 연결 : JOIN 

### 5-1. Intro

### 5-2. JOIN 이해하기

### 5-3. 다양한 JOIN 방법

### 5-4. JOIN 쿼리 작성하기 

### 5-5. JOIN을 처음 공부할 때 헷갈렸던 부분

### 5-6. JOIN 연습문제 1~2번

### 5-6. JOIN 연습문제 3~5번

### 5-7. 정리



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | ✅         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<!-- 여기까진 그대로 둬 주세요-->

<br>

---

# 1️⃣ 개념정리

## 5-2. JOIN 이해하기

~~~
✅ 학습 목표 :
* JOIN에 대한 정의와 필요성에 대해 설명할 수 있다.
~~~

**SQL JOIN**: 서로 다른 데이터 테이블을 연결  
<br>
두 데이터를 연결할 수 있는 공통값이 없음  
-> 두 개를 연결할 수 있는 테이블 추가로 이용  
<br>
연결할 수 있는 Key를 기준으로 데이터 연결 -> 오른쪽에 붙이기  
주로 id나 특정범위를 key로 이용  
<br>

관계형 데이터 베이스 설계시 정규화(중복을 최소화하도록 구조화) 과정을 거침  
-> 필요해서 JOIN
<br>
대신 데이터웨어하우스에서 JOIN 및 필요한 연산을 해서 데이터 마트를 만들어서 사용

## 5-3. 다양한 JOIN 방법

~~~
✅ 학습 목표 :
* JOIN 방법들의 종류를 설명할 수 있다. 
* 각 JOIN 방법들의 차이점에 대해서 설명할 수 있다. 
~~~

* **(INNER) JOIN**: 두 테이블의 공통 요소(행)만 연결, 교집합
* **LEFT/RIGHT(OUTER) JOIN**: 왼쪽이나 오른쪽 테이블 기준으로 연결
  * LEFT: 테이블 A의 KEY를 보면서 연결
  * RIGJT: 테이블 B의 KEY를 보면서 연결
* **FULL (OUTER) JOIN**: 양쪽 기준으로 연결, 합집합
  * 전체 KEY 다 확인, 상호 연결
* **CROSS JOIN**: 두 테이블에 각각의 요소를 곱하기  
-> LEFT JOIN이 가장 기본 


## 5-4. JOIN 쿼리 작성하기 

~~~
✅ 학습 목표 :
* JOIN을 사용한 문법에 대해 이해하여 적용할 수 있다.
* JOIN 을 활용한 쿼리를 작성할 수 있다. 
~~~

**SQL JOIN 쿼리 작성 흐름**
1. 테이블 확인
2. 기준 테이블 정의: 가장 많이 참고할 기준(base) 테이블 정의
   - ROW수 적으면서 내가 원하는 것을 다포함한 테이블
3. JOIN KEY 찾기: 여러 테이블과 연결할 KEY(ON) 정리
4. 결과 예상하기: 예상해서 손이나 엑셀로 작성
5. 쿼리 작성/검증
<br>
* CROSS JOIN 제외 나머지 조인 방식
~~~SQL
SELECT
  tp.*,
  t.* EXCEPT(id), # id_1: id 라는 결과가 중복
  p.* EXCEPT(id) #따라서 EXCEPT를 사용해서 각각의 id를 tp에 있는 것으로 활용하기
FROM basic.trainer_pokemon AS tp
LEFT JOIN basic.trainer AS t
ON tp.pokemon_id = t.id
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id = p.id #ON에서 별칭 사용 가능
~~~

* CROSS JOIN: ON필수X, 즉 조인키가 없어도 됨
<br>


## 5-6. JOIN 연습문제 1~5번 

~~~
✅ 학습 목표 :
* 연습문제(3문제 이상) 푼 것들 정리하기
~~~

# 1. 트레이너가 보유한 포켓몬들은 얼마나 있는지 알 수 있는 쿼리 작성
~~~sql
SELECT
  kor_name,
  COUNT(tp.id) AS pokemon_cnt
FROM(
SELECT
  id,
  trainer_id,
  pokemon_id,
  status
FROM basic.trainer_pokemon
WHERE
  status IN ("Active", "Training")
) AS tp
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id=p.id
GROUP BY
  kor_name
ORDER BY
   pokemon_cnt DESC
~~~
* 필터링을 먼저 해서 row 수를 줄이고 JOIN하기
* JOIN에서 사영하는 테이브렝 중복된 컬럼 이름이 있다면 어떤 테이블의 컬럼인지 명시하기

# 2. 각 트레이너가 보유한 포켓몬 중에서 'GRASS' 타입의 포켓몬 수를 계산해주세요.
~~~sql
SELECT
  tp.*,
  p.type1
  COUNT(tp.id) AS pokemon_Cnt
FROM (
  SELECT
    id,
    trainer_id,
    pokemon_id,
    status
  FROM basic.trainer_pokemon
  WHERE
    status IN ("Active","Training")
) AS tp
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id=p.id
WHERE
  type1="Grass"
GROUP BY
  type1
ORDER BY
  2 DESC
~~~
* 기준이 되는 테이블을 무엇으로 잡을까 -> NULL 안 추가되는 방안으로, 내가 구하고자 하는 데이터가 어디에 잘 저장되어 있는가?

<br>

<br>

---

# 2️⃣ 확인문제 & 문제 인증

## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/131533

> 상품 별 오프라인 매출 구하기

https://school.programmers.co.kr/learn/courses/30/lessons/133027

> 주문량이 많은 아이스크림들 조회하기

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. --> 



---

# 3️⃣ 참고자료

JOIN 에 대해서 그림으로 쉽게 이해할 수 있는 자료들도 있어서 첨부합니다. 아래의 블로그도 학습할 때 같이 참고해주세요.

1. https://data-marketing-bk.tistory.com/entry/SQL-JOIN-%ED%95%9C-%EB%B0%A9%EC%97%90-%EC%A0%95%EB%A6%AC-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%BD%94%EB%93%9C%EA%B9%8C%EC%A7%80-%EC%9D%B4%EA%B2%83%EB%A7%8C-%EB%B3%B4%EC%9E%90



2. https://velog.io/@wijoonwu/JOIN

<br>

### 🎉 수고하셨습니다.
