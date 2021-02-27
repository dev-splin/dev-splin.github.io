---
title: "Database : DDL(Data Definition Language)"
excerpt_separator: <!--more-->
categories:
  - Database
tags:
  - Database
  - "Database : DDL(Data Definition Language)"
toc: true
toc_sticky: true
toc_label: 목차
---

# DDL (Data Definition Language)



## MySQL 데이터 타입

[![img](https://cphinf.pstatic.net/mooc/20180131_89/1517386938999sf3SM_PNG/2_8_3_MySQL__-1.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/58936#)

---

[![img](https://cphinf.pstatic.net/mooc/20180131_46/1517386952668I67cM_PNG/2_8_3_MySQL__-2.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/58936#)



## 테이블 생성



```sql
create table 테이블명( 
          필드명1 타입 [NULL | NOT NULL][DEFAULT ][AUTO_INCREMENT], 
          필드명2 타입 [NULL | NOT NULL][DEFAULT ][AUTO_INCREMENT], 
          필드명3 타입 [NULL | NOT NULL][DEFAULT ][AUTO_INCREMENT], 
          ........... 
          PRIMARY KEY(필드명) 
          );
```



- 데이터 형 외에도 속성값의 빈 값 허용 여부는 NULL 또는 NOT NULL로 설정
- DEFAULT 키워드와 함께 입력하지 않았을 때의 초기값을 지정
- 입력하지 않고 자동으로 1씩 증가하는 번호를 위한 AUTO_INCREMENT

 

#### 테이블 생성 실습

- EMPLOYEE와 같은 구조를 가진 EMPLOYEE2 테이블을 생성합니다.



```sql
CREATE TABLE EMPLOYEE2(   
            empno      INTEGER NOT NULL PRIMARY KEY,  
           name       VARCHAR(10),   
           job        VARCHAR(9),   
           boss       INTEGER,   
           hiredate   VARCHAR(12),   
           salary     DECIMAL(7, 2),   
           comm       DECIMAL(7, 2),   
          deptno     INTEGER);
```

 

## 테이블 수정 (컬럼 추가 / 삭제)



```sql
alter table 테이블명
          add  필드명 타입 [NULL | NOT NULL][DEFAULT ][AUTO_INCREMENT];

alter table 테이블명
         drop  필드명;
```

 

#### 테이블 수정 실습 (컬럼 추가)

- EMPLOYEE2 테이블에 생일(birthdate)칼럼을 varchar(12)형식으로 추가합니다.



```sql
alter table EMPLOYEE2

add birthdate varchar(12);
```



#### 테이블 수정 실습 (컬럼 삭제)

- EMPLOYEE2 테이블의 생일(birthdate)칼럼을 삭제합니다.



```sql
alter table EMPLOYEE2

drop birthdate;
```



## 테이블 수정 (컬럼 수정)



```sql
alter table  테이블명
     change  필드명  새필드명 타입 [NULL | NOT NULL][DEFAULT ][AUTO_INCREMENT];
```



- change 키워드를 사용하고 칼럼을 새롭게 재정의 합니다 (이름부터 속성까지 전부)

 

#### 테이블 수정 실습 (컬럼 수정)

- EMPLOYEE2 테이블의 부서번호(deptno)를 dept_no로 수정합니다.



```sql
alter table EMPLOYEE2

change deptno dept_no int(11);
```



## 테이블 이름 변경



```sql
alter table  테이블명 rename 변경이름
```

 

#### 테이블 이름 변경 실습

- EMPLOYEE2 테이블의 이름을 EMPLOYEE3로 변경합니다.



```sql
alter table EMPLOYEE2

rename EMPLOYEE3;
```



## 테이블 삭제하기



```sql
drop table 테이블이름;
```



- 참고로, 제약 조건이 있을 경우에는 drop table 명령으로도 테이블이 삭제되지 않을 수 있습니다. 그럴 경우는 테이블을 생성한 반대 순서로 삭제를 해야합니다.

 

**테이블 삭제 실습**

\* 테이블 삭제 후 desc 명령을 수행하면, 존재하지 않는 테이블이라고 표시됩니다.

-  EMPLOYEE2 테이블을 삭제합니다.

  

```sql
drop table EMPLOYEE2;
```



---

출처 : https://www.boostcourse.org/web326/lecture/58936