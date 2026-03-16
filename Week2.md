# SQL_BASIC 2주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_2nd_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**2주차 과제**는 1주차 과제처럼 SQL의 필요성이나 느낀점 위주가 아닌, **실제 강의 내용을 바탕으로 개념을 정리하고 학습한 내용을 집중적으로 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요. 

**👀(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_2nd

### 섹션 3. 데이터 탐색 - 조건, 추출, 요약

### 2-3. 데이터 탐색 (SELECT, FROM, WHERE)

### 2-4. SELECT 연습문제

### 2-5. 집계 (Group By + Having + Sum/Count)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | 🍽️         |
| 4주차 | 섹션 **3-4** ~ **4-4** | 🍽️         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>


---

# 1️⃣ 개념정리 

## 2-3. 데이터 탐색 (SELECT, FROM, WHERE)

~~~
✅ 학습 목표 :
* SQL 쿼리 구조를 이해할 수 있다. 
* SELECT, FROM, WHERE의 핵심 문법을 설명할 수 있다. 
~~~

<예시) 포켓몬>  
어떤 포켓몬을 선택할지 고민: 이름, 공격력, 타입 등  
해당 데이터를 데이터베이스 테이블(컬럼과 로우)로 표현  

<br>

**SQL 쿼리 구조**  
- SELECT: 테이블의 어떤 컬럼을 선택(출력)할 것인가
  ex) Col1 AS new_name,  
      Col2,  
      Col3
- FROM (데이터셋).(테이블): 어떤 테이블에서 데이터를 확인할 것인가
- WHERE: 만약 원하느 조건이 있다면 선택할 수 있음 ex) Col1 = 1: name = "꼬부기"

~~~
예시)  
SELECT
  * (모든 컬럼을 출력하겠다, SELECT * EXCEPT도 가능)
FROM basic.pokemon
WHERE
  type1 = 'Fire'
~~~
**[데이터 추출 방법]**
**데이터가 여러 장소에 저장되어 있는 경우**  
특정 테이블 데이터 각각 추출 후 연결하기, JOIN  
TIP) 집합으로 그려놓고 시작하기

**빅커리에 실습해보기**  
프로젝트를 여러 개 사용한다면 프로젝트명을 명시하는 것이 좋음 
프로젝트명을 제외하고 작성할 때는 파일명에 '' 없어도 괜찮음  
세미콜론으로 쿼리문을 끊을 수 있음
SQL은 FROM, WHERE, SELECT 순서대로 인지하면 좋음

~~~
SELECT
  id AS pokemon_id, #AS는 별칭을 지어줄 때 / 따옴표 사용X
  kor_name, 
  type1,
  total
FROM basic.pokemon
WHERE
 type1 = "Fire"
~~~

## 2-5. 집계 (Group By / HAVING / SUM,COUNT)

~~~
✅ 학습 목표 :
* 데이터를 집계하고 그룹화하는 방법을 설명할 수 있다.
* GROUP BY, HAVING, ORDER BY, 집계함수(SUM/COUNT 등)을 활용하는 방법을 설명할 수 있다.
* having과 where의 차이에 대해서 설명할 수 있다.
~~~
**[요약하기]**  
**집계**: 그룹화해서 계산(+, -, max, min, 평균, 갯수)하기 ex) COUNT, COUNTIF, MAX, MIN, AVG, SUM
**GROUP BY**: 같은 값끼리 모아서 그룹화, 특정 칼럼을 기준으로 모으면서 다른 컬럼에서 집계 가능
- HAVING: 집계 후 조건 처리
집계할 컬럼을 SELECT에 명시하고 그 컬럼을 GROUP BY에 작성
**DISTINCT**: 고유값을 알고 싶은 경우, 중복 제거 ex) COUNT(DISTINCT 컬럼)
  GROUP BY도 비슷한 상황에서 사용할 수 있음
~~~
SELECT
  type1.AVG(attack) As avg_attack,
  count(id) AS cnt
FROM pokemon
GROUP BY
  type1
HAVING
  cnt > 3
~~~
**그룹화 활용 포인트**
- 일자별 집계
- 연령대별 집계
- 특정 타입별 집계
- 앱 화면별 집계  
등등

**조건을 설정하고 싶은 경우**
- WHERE: 테이블에 바로 조건을 설정하고 싶은 경우
- HAVING: GROUP BY한 후 조건을 설정하고 싶은 경우
  <br>
* HAVING대신 WHERE을 쓰고 싶을 때:  
  FROM절 아래에 서브 쿼리를 넣을 수 있음, 더 길어짐

**서브쿼리**  
- SELECT문 안에 존재하는 SELECT쿼리  
- FROM절에 넣을 수 있음
- 괄호로 묶어서 사용
- 서브쿼리를 작성하고 서브쿼리 바깥에서 WHERE 조건 설정 = 서브쿼리에서 HAVING으로 하는 것

**ORDER BY [컬럼] [순서]**: 정렬, 쿼리의 맨 마지막에 작성
ex) DESC(내림차순), OSC(오름차순, 보통 디폴트 값)

**LIMIT**: 출력 개수 제한하기, 결과 ROW 수 제한, 쿼리의 맨 마지막에 작성
  
# 2️⃣ 학습 인증란

![학습인증](./image2-1.jpg)



<br><br>



---

# 3️⃣ 확인문제

## 문제 1

> **🧚Q. 포켓몬 마스터 진아는 포켓몬 데이터 조회하는 SQL문에 재미를 느껴서 혼자서 데이터를 조회하는 쿼리문을 짰습니다. 하지만 세 가지의 오류로 다음 코드가 실행이 안된다고 하는데, 각 오류의 위치와 이유를 설명하고, 올바른 쿼리문으로 수정해보세요.**

~~~sql
# 진아의 SQL Query문 
SELECT name. type
FROM pokemon;
WHERE type = Electric;
~~~



~~~
1. SELECT할 컬럼을 나열할 때 쉼표를 사용해야 함  
2. WHERE문 전에 ;가 오면 쿼리문을 끊는 역할을 하기에 오류가 생긴다  
3. Electric은 문자열이기에 따옴표를 넣어야 함
~~~
~~~sql
SELECT name, type
FROM pokemon
WHERE type = 'Electric';
~~~



## 문제 2

> **🧚Q. 앞서 SQL Query의 오류를 해결한 진아는 기분 좋게 이번에는 포켓몬 데이터에서 타입별 평균 공격력이 60 이상인 타입만 조회하려는 쿼리를 작성하려고 했습니다. 하지만 이번에도 실수를 하여 쿼리문이 실행되지 않거나 잘못된 결과가 나오고 있는데, 쿼리에서 잘못된 부분이 무엇인지 설명하고, 올바르게 수정한 쿼리를 작성해보세요.**

~~~sql
SELECT type, AVG(attack) AS avg_attack
FROM pokemon
WHERE AVG(attack) >= 60
GROUP BY type;
~~~

~~~
평균값을 집계한 후 평균 공격력이 60이상인 타입만 추출해야한다. 이를 위해서는 WHERE문이 아니라 HAVING을 써야 한다.
~~~
~~~sql
SELECT type, AVG(attack) AS avg_attack
FROM pokemon
GROUP BY type
HAVING AVG(attack) >= 60;
~~~



### 🎉 수고하셨습니다.
