# SQL_BASIC 3주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_3rd_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**3주차 과제는 문제 풀이를 중심으로**, 강의에서 제시된 예제 문제 중 **7 문제 이상을 선택하여 직접 풀어본 뒤**, 강의 영상의 풀이와 비교해 **틀린 부분, 맞은 부분, 새롭게 배운 개념**을 구체적으로 정리해주세요. (적어도 3문제는 정리해야 합니다.) 완성된 과제는 Gihub에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_3rd

### 섹션 3. 데이터 탐색 - 조건, 추출, 요약

### 2-6. 연습문제 1~3번

### 2-6. 연습문제 7~9번

### 2-6. 연습문제 10~12번

### 2-6. 연습문제 13~17번

### 2-7. 정리 

### 2-8. 새로운 집계함수



## 섹션 4. 쿼리 잘 작성하기, 쿼리 작성 템플릿 및 오류를 잘 디버깅하기

### 3-1. INTRO

### 3-2. SQL 쿼리 작성하는 흐름

### 3-3. 쿼리 작성 템플릿과 생산성 도구 



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | 🍽️         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리

## 2-6. 연습문제

~~~
✅ 학습 목표 :
* 연습문제(7문제 이상) 푼 것들 정리하기
~~~

문제 1)
- 조건: type2가 없는
- 어떤 테이블?: pokemon
- 어떤 컬럼: 따로 없음. 포켓만 수만 남기면 됨.
- 어떻게 집계: 포켓몬의 수 => COUNT
- NULL은 IS 연산자 사용
 ~~~sql
SELECT
  COUNT(id) AS cnt
FROM basic.pokemon
WHERE
  type2 IS NULL
~~~
=> IS NULL 사용 형태 확인  

문제2)
- 테이블: pokemon
- 조건: type2가 없는 포켓몬
- 칼럼: type1
- 집계: 포켓몬 수=>COUNT
- 정렬: type1의 포켓몬 수 내림차순
~~~sql
SELECT
  type1,
  COUNT(id) AS pokemon_cnt
FROM basic.pokemon
WHERE
  type2 IS NULL
GROUP BY
  type1
ODER BY
  pokemon_cnt DESC
~~~
=> select에 쉼표 주의  
=> 집계함수를 사용할 때 집계 기준이 있다면 GROUP BY에 써줘야함  

문제5)
- 테이블: trainer
- 조건: 같은 이름이 2개 이상(동명이인)
- 컬럼: 이름
- 집계: COUNT

~~~sql
SELECT
  name,
  COUNT(name) AS trainer_cnt
FROM basic.trainer
GROUP BY
  name
HAVING
  trainer_cnt>=2
~~~
=> 집계 후 조건 => HAVING  

문제7)
- 테이블: trainer
- 조건: 이름 = "Iris","Whitney","Cynthia" 중에 있으면 추출
- 컬럼: 정보->*
- 집계: 없음

~~~sql
SELECT
  *
FROM basic.trainer
WHERE
  name = IN("Iris","Cynthia","Whitney")
~~~
=> IN: 괄호 안에 값이 있는 행만 추출, or로 연결하면 너무 길어 사용  

문제10)
- 테이블: pokemon
- 조건: type2 IS NOT NULL
- 컬럼: x
- 집계: 포켓몬 수=>COUNT

~~~sql
SELECT
  *
FROM basic.pokemon
WHERE
  type2 IS NOT NULL
~~~


문제11)
- 테이블: pokemon
- 조건: type2가 있는
- 컬럼: type1
- 집계: 제일 많은 COUNT

~~~sql
SELECT
  type1,
  COUNT(id) AS pokemon_cnt
FROM basic.pokemon
WEHRE
  type2 IS NOT NULL
GROUP BY
  type1
ORDER BY
  pokemon_cnt DESC
~~~
=> DESC: 내림차순  

문제17)
- 테이블: trainer_pokemon
- 조건: 풀어준 포켓몬의 비율이 20% 넘어야함
- 컬럼: trainer_id
- 집계: COUNTIF(조건)
~~~sql
SELECT
  trainer_id,
  COUNTIF(status = "Released") AS released_cnt,
  COUNT(pokemon_id) AS pokemon_cnt,
  COUNTIF(status="Released")/COUNT(pokemon_id) AS released_ratio
FROM basic.trainer_pokemon
GROUP BY
  trainer_id
HAVING
  released_ratio>=0.2
~~~
=> COUNT IF 사용 확인



## 2-8. 새로운 집계함수

~~~
✅ 학습 목표 :
* SQL 쿼리 구조를 이해할 수 있다. 
* SELECT, FROM, WHERE을 활용하는 방법을 설명할 수 있다. 
~~~

COUNTIF (조건): 조건을 만족하는 데이터만 집계

~~~sql
SELECT
  FirstName AS first_name,
  LastName AS last_name,
  SUM(PointsScored) AS total_points
FROM PlayerStats
GROUP BY ALL
~~~

## 3-2. 쿼리를 작성하는 흐름

~~~
✅ 학습 목표 :
* 쿼리를 작성하는 흐름을 설명할 수 있다.
~~~
**쿼리 작성 흐름**
1. 지표 고민: 어떤 문제를 해결하기 위해 데이터가 필요한가?
2. 지표 구체화: 구체적인 지표 명시
3. 지표 탐색: 유사한 문제를 해결한 케이스가 있나 확인 -> 확인
4. 쿼리 작성: 데이터가 있는 테이블 찾기  
   * if) 2개 이상 -> 연결(join)
6. 데이터 정합성 확인: 예상한 결과와 동일한지 확인  
7. 쿼리 가독성: 나중을 위해 깔끔하게 쿼리 작성  
8. 쿼리 저장: 쿼리는 재사용되므로 문서로 저장  

## 3-3. 쿼리 작성 템플릿과 생산성 도구

~~~
✅ 학습 목표 :
* 생산성 도구를 만들 수 있다.
~~~

<!-- 이어질 주차에서 생산성 도구를 활용한 실습이 있습니다.강의에 맞게 제작하여 화면을 캡쳐하여 이 주석을 지우고 올려주세요. -->



<br>
<br>

---

# 2️⃣ 학습 인증란

<!-- 이 글을 지우고, 여기에 학습한 것을 인증해주세요.-->

<img width="477" height="747" alt="image" src="https://github.com/user-attachments/assets/0b9657e2-f9ce-4034-91da-d68b834f3d34" />

<img width="485" height="273" alt="image" src="https://github.com/user-attachments/assets/7b5c7100-e24b-4482-a576-d88f51fab04f" />

<br><br>



---

# 3️⃣ 확인문제

## 문제 1

> **🧚Q. Q. 포켓몬 연구에 흥미를 느낀 혜인은 각 타입(type1)별 평균 공격력(attack)을 비교해보고 싶었습니다.**
>
> 그래서 다음과 같은 필요한 정보를 미리 정리해보았습니다. 

~~~
조건 : attack이 50 이상인 포켓몬만 포함
보고 싶은 컬럼 : type1
집계 내용 : 각 type1 별 평균 공격력
정렬 기준 : 평균 공격력을 기준으로 내림차순 정렬
~~~

> **이 목표를 바탕으로 혜인은 아래와 같은 쿼리를 작성했지만, 일부 SQL 문법 요소를 빼먹었습니다. 비어 있는 부분인 ㄱ, ㄴ, ㄷ, ㄹ 에 들어갈 알맞은 SQL 구문을 채워보세요:**

~~~sql
SELECT type1, (ㄱ)
FROM pokemon
(ㄴ) attack >= 50
(ㄷ) type1
ORDER BY (ㄱ) (ㄹ);
~~~



~~~
ㄱ: AVG(attack)
ㄴ: WHERE
ㄷ: GROUP BY
ㄹ: DESC
~~~



### 🎉 수고하셨습니다.
