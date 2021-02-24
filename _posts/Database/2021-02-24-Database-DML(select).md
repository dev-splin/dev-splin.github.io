---
title: "Database : DML(SELECT)"
excerpt_separator: <!--more-->
categories:
  - Database
tags:
  - Database
  - "Database : DML(SELECT)"
toc: true
toc_sticky: true
toc_label: 목차
---

# 데이터 조작어

# (Data Manipulation Language, DML)

- 데이터 조작어는 모두 동사로 시작합니다.
- 시작하는 동사에 따라서 다음과 같은 4가지 조작어가 있습니다.
  - SELECT – 검색
  - INSERT - 등록
  - UPDATE - 수정
  - DELETE - 삭제



## SELECT 

**select 구문의 기본문형**

[![img](https://cphinf.pstatic.net/mooc/20180131_187/1517374752273Ba8n9_PNG/2_8_2_select__.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258484#)



#### 예제

- department 테이블의 모든 데이터를 출력합니다.

  ```sql
  select * from department;
  ```

- employee 테이블에서 직원의 사번(empno), 이름(name), 직업(job)을 출력합니다.

  ```sql
  select empno, name, job from employee;
  ```

- 컬럼에 Alias 부여하기 
  - 컬럼 뒤에 as를 붙여줍니다.

    ```sql
    select empno as 사번, name as 이름, job as 직업 from employee;
    ```

  - as를 생략할수도 있습니다.

    ```sql
    select empno 사번, name 이름, job 직업 from employee;
    ```

  - Alias를 `''(싱글쿼터)` 로 감쌀 수 있다. (Alias에 공백이 들어갈 시에 무조건 붙여주어야 합니다.)

    ```sql
    select empno '사-번', name '이 름', job '직 업' from employee;
    ```

- employee 테이블에서 사번과 부서번호를 하나의 칼럼으로 출력합니다. (문자열 결함함수 concat 사용)

  ```sql
  select concat(empno, '-', deptno) as '사번-부서번호' from employee;
  ```

- 사원 테이블의 부서번호를 중복되지 않게 출력합니다.

  ```sql
  select distinct deptno from employee;
  ```



### ORDER BY

- 정렬을 해줄 때 사용합니다.

[![img](https://cphinf.pstatic.net/mooc/20180227_237/15196955203980m2pE_PNG/2.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258484#)



##### 예제

- employee 테이블에서 직원의 사번(empno), 이름(name), 직업(job)을 출력하시오. 단, 이름을 기준으로 오름차순 정렬합니다.

  ```sql
  select empno, name, job from employee order by name;
  ```

- employee 테이블에서 직원의 사번(empno), 이름(name), 직업(job)을 출력하시오. 단, 이름을 기준으로 내림차순 정렬합니다.

  ```sql
  select empno, name, job from employee order by name desc;
  ```



### where

- 특 정 행을 검색할 때 사용합니다. (다양한 조건을 줄 수 있음)

[![img](https://cphinf.pstatic.net/mooc/20180227_23/1519695801630edbfc_PNG/3.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258485#)



##### 예제

- employee 테이블에서 고용일(hiredate)이 1981년 이전의 사원이름과 고용일을 출력합니다.

  ```sql
  select name, hiredate from employee where hiredate < '1981-01-01';
  ```

- employee 테이블에서 부서번호가 30인 사원이름과 부서번호를 출력합니다.

  ```sql
  select name, deptno from employee where deptno = 30;
  ```

- employee 테이블에서 부서번호가 10또는 30인 사원이름과 부서번호를 출력합니다.

  ```sql
  select name, deptno from employee where deptno 10 or 30;
  ```



#### in 키워드

괄호 안에 있는 숫자를 포함합니다.

- employee 테이블에서 부서번호가 10또는 30인 사원이름과 부서번호를 출력합니다.

  ```sql
  select name, deptno from employee where deptno in (10, 30);
  ```



#### like 키워드

- 와일드 카드를 사용하여 특정 문자를 포함한 값에 대한 조건을 처리
- `%`는 0에서부터 여러 개의 문자열을 나타냄
- `_`는 단 하나의 문자를 나타내는 와일드 카드



##### 예제

- employee 테이블에서 이름에 'A'가 포함된 사원의 이름(name)과 직업(job)을 출력합니다.

  ```sql
  select name, job from employee where name like '%A%';
  ```

- employee 테이블에서 이름 앞글자에 'A'가 포함된 사원의 이름(name)과 직업(job)을 출력합니다.

  ```sql
  select name, job from employee where name like 'A%';
  ```

- employee 테이블에서 이름 두번째 글자에 'S'가 포함된 사원의 이름(name)과 직업(job)을 출력합니다.

  ```sql
  select name, job from employee where name like '_A%';
  ```



### 다양한 함수

mysql은 from 절 없이 select 만으로 함수를 사용할 수 있습니다.

오라클은 from 절이 반드시 와야 하므로 DUAL 이라는 임시테이블을 놓고 사용합니다.

- UCASE(s), UPPER(s) : 전부 대문자로 변환하는 함수
- LCASE(s), LOWER(s) : 전부 소문자로 변환하는 함수
- SUBSTRING(s,n1,n2)  : 문자열을 n1 자리부터 n2개 까지 잘라주는 함수
- LPAD, RPAD(s1,n1,s2) : n1개의 문자열로 만드는데 s1의 문자열을 놓고 L(왼쪽)이나 R(오른쪽)에 s2의 문자열을 채우는 함수
  - select LPAD('hi', 5, '?'); 결과는 `???hi` 로 나옵니다.
- LTRIM, RTRIM(s) : L(왼쪽) 이나 R(오른쪽)의 공백을 제거해줍니다.
- TRIM(s) : 양쪽의 공백을 제거해줍니다.
- TRIM(BOTH 's1' FROM 's2') : s2의 문자열 양쪽에서 s1의 문자열을 제거합니다.
  - select trim(both 'x' from 'xxxxhxixxxx'); 결과는 `hxi`로 나옵니다.
- ABS(x) : x의 절대값을 구합니다.
- MOD(n,m) % : n을 m으로 나눈 나머지 값을 출력합니다.
- FLOOR(x) : x보다 크지 않은 가장 큰 정수를 반환합니다. BIGINT(8)로 자동 변환합니다.(내림)
- CEILING(x) : x보다 작지 않은 가장 작은 정수를 반환합니다.(올림)
- ROUND(x) : x에 가장 근접한 정수를 반환합니다.
- POW(x,y) POWER(x,y) : x의 y 제곱 승을 반환합니다.
- GREATEST(x,y,...) : 가장 큰 값을 반환합니다.
- LEAST(x,y,...) : 가장 작은 값을 반환합니다.
- CURDATE(),CURRENT_DATE : 오늘 날짜를 YYYY-MM-DD나 YYYYMMDD 형식으로 반환합니다.
- CURTIME(), CURRENT_TIME : 현재 시각을 HH:MM:SS나 HHMMSS 형식으로 반환합니다.
- NOW(), SYSDATE() , CURRENT_TIMESTAMP : 오늘 현시각을 YYYY-MM-DD HH:MM:SS나 YYYYMMDDHHMMSS 형식으로 반환합니다. 
- DATE_FORMAT(date,format) : 입력된 date를 format 형식으로 반환합니다.
- PERIOD_DIFF(p1,p2) : YYMM이나 YYYYMM으로 표기되는 p1과 p2의 차이 개월(p1-p2)을 반환합니다.



### CAST 형변환

[![img](https://cphinf.pstatic.net/mooc/20180227_7/1519696097137n1dmo_PNG/4.png?type=w760)](https://www.boostcourse.org/web326/lecture/258486/#)



### SELECT 구문(그룹함수)

- 결과가 하나만 나오는 것을 그룹함수라고 합니다.
  - 전에 concat이라는 함수를 수행했을 때 결과 하나하나마다 함수가 적용되는 것이 단일함수라고 합니다.

[![img](https://cphinf.pstatic.net/mooc/20180131_87/151738015308653Cmb_PNG/2_8_2_select_%28%29.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258486/#)



#### 예제 

- employee 테이블에서 부서번호가 30인 직원의 급여 평균과 총합계를 출력합니다.

  ```sql
  select avg(salary) , sum(salary) from employee where deptno = 30;
  ```

- employee 테이블에서 부서별 직원의 부서번호, 급여 평균과 총합계를 출력합니다.
  ```sql
  select deptno, avg(salary) , sum(salary) from employee group by deptno;
  ```

  - 이 예제에서 select deptno, avg(salary), sum(salary) from employee; 를 하게 되면 <br/>기본적으로 평균값이 전체를 대상으로 하기 때문에  평균값과 합계는 1개의 결과가 나옵니다. 이에 따라 이름도 전체의 평균,합계값과 상관 없는 첫번째 이름이 나오게 됩니다.  <br/> (Oracle 에서는 이 경우 반드시 그룹핑을 해주라는 오류를 발생시키지만 mysql은 그렇지 않습니다.) <br/> 하지만 group by를 해주면 그룹별로 평균값을 구해주기 때문에 `~~별` 이라고 나오는 것은 group by를 생각해주면 됩니다.



## JOIN

- 기본 키(PK)와 외래 키(FK)로 관계된 두 테이블을 결합할 수 있는 명령어이다.
- inner, left, right, full,  cross, self join이 있다.

- 고객 테이블과 주문 테이블이 있다고 가정합니다.
  -  customer_id는 고객테이블의 PK이다. 
  - order_id가 주문테이블의 PK, customer_id는 주문테이블의 FK 이다.



### Inner Join

- 조인 조건이 충족되는 표 A 및 표 B의 모든 레코드가 해당됩니다.

![Select all records from Table A and Table B, where the join condition is met.](https://images.squarespace-cdn.com/content/v1/5732253c8a65e244fd589e4c/1464122775537-YVL7LO1L7DU54X1MC2CI/ke17ZwdGBToddI8pDm48kMjn7pTzw5xRQ4HUMBCurC5Zw-zPPgdn4jUwVcJE1ZvWMv8jMPmozsPbkt2JQVr8L3VwxMIOEK7mu3DMnwqv-Nsp2ryTI0HqTOaaUohrI8PIvqemgO4J3VrkuBnQHKRCXIkZ0MkTG3f7luW22zTUABU/image-asset.png?format=300w)

#### 예제

- 고객테이블에 있는 고객의 이름과 주문테이블에 있는 주문 날짜, 주문 금액 을 함께 출력합니다.

```sql
select first_name, last_name, order_date, order_amount	
-- 만약 주문테이블에도 first_name이 있다면 주문.frist_name, 고객.first_name으로 구분할 수 있습니다. 
from customers c	-- 테이블에 별명을 붙여 간단하게 사용할 수 있습니다.
inner join orders o
on c.customer_id = o.customer_id;	-- PK와 FK를 이용해 결합해줍니다.
```



### Left Join

- 조인 조건이 충족되는 표 B의 레코드와 함께 표 A의 모든 레코드가 해당됩니다.

![Select all records from Table A, along with records from Table B for which the join condition is met (if at all).](https://images.squarespace-cdn.com/content/v1/5732253c8a65e244fd589e4c/1464122797709-C2CDMVSK7P4V0FNNX60B/ke17ZwdGBToddI8pDm48kMjn7pTzw5xRQ4HUMBCurC5Zw-zPPgdn4jUwVcJE1ZvWEV3Z0iVQKU6nVSfbxuXl2c1HrCktJw7NiLqI-m1RSK4p2ryTI0HqTOaaUohrI8PIO5TUUNB3eG_Kh3ocGD53-KZS67ndDu8zKC7HnauYqqk/image-asset.png?format=300w)

#### 예제

- 고객에 주문했는지 여부와 상관없이 주문에 대한 정보를 고객테이블에 추가합니다.

```sql
select first_name, last_name, order_date, order_amount
from customers c
left join orders o
on c.customer_id = o.customer_id;
-- "where order_date is NULL"을 추가해 주문하지 않은 모든 고객목록을 반환할 수 있기 때문에 유용합니다.
```



### Right Join

- 조인 조건이 충족되는 표 A의 레코드와 함께 표 B의 모든 레코드가 해당됩니다. (왼쪽 조인의 미러버전)

![Select all records from Table B, along with records from Table A for which the join condition is met (if at all).](https://images.squarespace-cdn.com/content/v1/5732253c8a65e244fd589e4c/1464122744888-MVIUN2P80PG0YE6H12WY/ke17ZwdGBToddI8pDm48kMjn7pTzw5xRQ4HUMBCurC5Zw-zPPgdn4jUwVcJE1ZvWlExFaJyQKE1IyFzXDMUmzc1HrCktJw7NiLqI-m1RSK4p2ryTI0HqTOaaUohrI8PI-FpwTc-ucFcXUDX7aq6Z4KQhQTkyXNMGg1Q_B1dqyTU/image-asset.png?format=300w)

#### 예제

- 고객의 유무와 상관없이 고객에 대한 정보를 주문테이블에 추가합니다.

```sql
select first_name, last_name, order_date, order_amount
from customers c
right join orders o
on c.customer_id = o.customer_id;
```



### Full Join

- 조인 조건의 충족 여부에 관계없이 표 A와 표 B에서 모든 레코드가 해당됩니다.

![Select all records from Table A and Table B, regardless of whether the join condition is met or not.](https://images.squarespace-cdn.com/content/v1/5732253c8a65e244fd589e4c/1464122981217-RIYH5VL2MF1XWTU2DKVQ/ke17ZwdGBToddI8pDm48kMjn7pTzw5xRQ4HUMBCurC5Zw-zPPgdn4jUwVcJE1ZvWEV3Z0iVQKU6nVSfbxuXl2c1HrCktJw7NiLqI-m1RSK4p2ryTI0HqTOaaUohrI8PIO5TUUNB3eG_Kh3ocGD53-KZS67ndDu8zKC7HnauYqqk/image-asset.png?format=300w)

#### 예제

- 양쪽 테이블의 모든 레코드 목록을 출력합니다.

```sql
select first_name, last_name, order_date, order_amount
from customers c
full join orders o
on c.customer_id = o.customer_id;
```



### Cross Join

- 복수 테이블의 모든 레코드를 M x N 식으로 모두 출력합니다.



#### 예제

```sql
select * from customer, orders;
```



### Self Join

- 하나의 테이블을 Alias를 통하여 두번 참조한 후, 자신의 테이블을 마치 다른 테이블인 것 처럼 조인하는 방식이다.



#### 예제

- Emp 테이블에서 Boss가 Kim인 모든 종업원을 출력합니다.
  - Emp 테이블에는 매니저아이디를 의미하는 MgrId와 모든 사원의 Id인 EmpId가 있습니다.
  - 각 사원을 관리하는 매니저(Boss)가 있습니다.

```sql
select e.empid, e.name, b.name as Boss
from Emp e, Emp b
where e.mgrid = b.empid
and b.name = 'Kim';
-- 각 사원을 관리하는 매니저의 이름이 'Kim'인 데이터가 출력됩니다.
```





---

출처 : https://www.boostcourse.org/web326/lecture/258484

https://www.boostcourse.org/web326/lecture/258485/

https://www.boostcourse.org/web326/lecture/258486/

http://www.sql-join.com/

http://www.sqlprogram.com/Basics/sql-join.aspx