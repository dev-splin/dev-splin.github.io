---
title: "Database : DML(INSERT, UPDATE, DELETE)"
excerpt_separator: <!--more-->
categories:
  - Database
tags:
  - Database
  - "Database : DML(INSERT, UPDATE, DELETE)"
toc: true
toc_sticky: true
toc_label: 목차
---

## INSERT

- 데이터를 입력할 수 있는 DML 입니다.



```sql
INSERT INTO 테이블명(필드1, 필드2, 필드3, 필드4, … ) 
        VALUES ( 필드1의 값, 필드2의 값, 필드3의 값, 필드4의 값, … )

INSERT INTO 테이블명
        VALUES ( 필드1의 값, 필드2의 값, 필드3의 값, 필드4의 값, … )
```



- 필드명을 지정해주는 방식은 디폴트 값이 세팅되는 필드는 생력할 수 있습니다.
- 필드명을 지정해주는 방식은 추 후, 필드가 추가/변경/수정 되는 변경에 유연하게 대처 가능합니다.
- 필드명을 생략했을 경우에는 모든 필드 값을 반드시 입력해야 합니다.
- PK (Primary Key) 로 되어있는 값은 반드시 입력해 주어야 합니다.

 

#### 예제

- role테이블에 role_id는 200, description에는 'CEO'로 한건의 데이터를 저장합니다.
  ```sql
  insert into role (role_id, description) values ( 200, 'CEO');
  insert into rolevalues ( 200, 'CEO');
  ```



## UPDATE

- 데이터를 수정할 수 있는 DML 입니다.



```sql
 UPDATE  테이블명
        SET  필드1=필드1의값, 필드2=필드2의값, 필드3=필드3의값, …
   WHERE  조건식
```



- 조건식을 통해 특정 row만 변경할 수 있습니다.
- 조건식을 주지 않으면 전체 로우가 영향을 미치니 조심해서 사용하도록 합니다.



#### 예제

- role테이블에 role_id가 200일 경우 description을 'CTO'로 수정합니다.

  ```sql
  update role set description = 'CTO' where role_id = 200;
  ```




## DELETE

- 데이터를 삭제할 수 있는 DML입니다.



```sql
 DELETE
      FROM  테이블명
WHERE  조건식
```

​    

- 조건식을 통해 특정 row만 삭제할 수 있습니다.
- 조건식을 주지 않으면 전체 로우가 영향을 미치니 조심해서 사용하도록 합니다.



#### 예제

- role테이블에서 role_id는 200인 정보를 삭제합니다.

  ```sql
  delete from ROLE where role_id = 200;
  ```



---

출처 : https://www.boostcourse.org/web326/lecture/258487