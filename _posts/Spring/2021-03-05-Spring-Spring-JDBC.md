---
title: "Spring : JDBC"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Spring JDBC"
toc: true
toc_sticky: true
toc_label: 목차
---

# Spring JDBC

JDBC 프로그래밍을 보면 반복되는 개발 요소가 있습니다.(connect, statement, resultset 등..) 이러한 반복적인 요소는 개발자를 지루하게 만듭니다. 그래서 Spring JDBC는 개발하기 지루한 **JDBC의 모든 저수준 세부사항을 스프링 프레임워크가 처리**해주기 때문에 개발자는 필요한 부분만 개발하면 됩니다.



## Spring JDBC - 개발자가 해야 할 일은?

[![img](https://cphinf.pstatic.net/mooc/20180205_176/1517797019977L6ygq_PNG/2_11_2_springJDBC.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/58973/?isDesc=false#)



### Spring JDBC 패키지

- **org.springframework.jdbc.core**

  ​	JDBC 템플릿 클래스와 JDBC 템플릿의 다양한 콜백 인터페이스를 포함하는 등 JdbcTemplate 및 관련 Helper 객체 제공합니다.

- **org.springframework.jdbc.datasource**

  ​	DataSource를 쉽게 접근하기 위한 유틸 클래스, 트랜젝션매니져 및 다양한 DataSource 구현을 제공합니다.

- **org.springframework.jdbc.object**

  ​	RDBMS 조회, 갱신, 저장등을 안전하고 재사용 가능한 객제 제공합니다.

- **org.springframework.jdbc.support**

  SQL Exception 변환 기능, jdbc.core 및 jdbc.object를 사용하는 JDBC 프레임워크를 지원합니다.



### JDBC Template

- `org.springframework.jdbc.core`에서 가장 중요한 클래스입니다.
- 리소스 생성, 해지를 처리해서 연결을 닫는 것을 잊어 발생하는 문제 등을 피할 수 있도록 합니다.
- 스테이먼트(Statement)의 생성과 실행을 처리합니다.
- SQL 조회, 업데이트, 저장 프로시저 호출, ResultSet 반복호출 등을 실행합니다.
- JDBC 예외가 발생할 경우 org.springframework.dao패키지에 정의되어 있는 일반적인 예외로 변환시킵니다.



#### 예제



**JdbcTemplate select 예제1**

```java
int rowCount = this.jdbcTemplate.queryForInt("select count(*) from t_actor");
```

`t_actor` 테이블의 모든 레코드 수를 가져옵니다.



**JdbcTemplate select 예제2**

```java
int countOfActorsNamedJoe = this.jdbcTemplate.queryForInt("select count(*) from t_actor where first_name = ?", "Joe"); 
```

  `?`를 바인딩해서 `t_actor` 테이블에서 `first_name`이 "Joe"인 레코드의 수를 가져옵니다.



**JdbcTemplate select 예제3**

```java
String lastName = this.jdbcTemplate.queryForObject("select last_name from t_actor where id = ?", new Object[]{1212L}, String.class); 
```

`t_actor` 테이블에서 `id`가 1212인 레코드의 `last_name`을 가져옵니다. `queryForObject`는 `Object`를 반환하는데, 3번 째 인자를 통해 `String`으로 형 변환해줍니다.



**JdbcTemplate select 예제4**

```java
Actor actor = this.jdbcTemplate.queryForObject(

  "select first_name, last_name from t_actor where id = ?",

  new Object[]{1212L},

  new RowMapper<Actor>() {

    public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {

      Actor actor = new Actor();

      actor.setFirstName(rs.getString("first_name"));

      actor.setLastName(rs.getString("last_name"));

      return actor;

    }

  });
```

`t_actor` 테이블에서 `id`가 1212인 레코드의 `first_name`, `last_name`을 가져옵니다. 3번 째 인자에서 `RowMapper`를 `override`하여 테이블의 Row를 매핑시켜준 후 객체를 만들어 값을 설정하고 반환해줍니다.



**JdbcTemplate select 예제5**

```java
List<Actor> actors = this.jdbcTemplate.query(

  "select first_name, last_name from t_actor",

  new RowMapper<Actor>() {

    public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {

      Actor actor = new Actor();

      actor.setFirstName(rs.getString("first_name"));

      actor.setLastName(rs.getString("last_name"));

      return actor;

    }

  });
```

`t_actor` 테이블에서 모든 레코드의 `first_name`, `last_name`을 가져옵니다. 객체를 만들어 값을 설정하고 반환해줍니다.(`queryForObject`가 아니라 `query`를 이용하면 쿼리문으로 부터 도출된 값만큼 Row를 매핑해 List로 반환해주는 것으로 생각됩니다. )



**JdbcTemplate select 예제6**

```java
public List<Actor> findAllActors() {

  return this.jdbcTemplate.query( "select first_name, last_name from t_actor", new ActorMapper());

}

private static final class ActorMapper implements RowMapper<Actor> {

  public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {

    Actor actor = new Actor();

    actor.setFirstName(rs.getString("first_name"));

    actor.setLastName(rs.getString("last_name"));

    return actor;

  }

}
```

1건 구하기와 여러 건 구하기가 같은 코드에 있을 경우 Row를 매핑하는 부분을 함수로 만들어 사용할 수 있습니다.



**JdbcTemplate insert 예제**

```java
this.jdbcTemplate.update("insert into t_actor (first_name, last_name) values (?, ?)",  "Leonor", "Watling");
```



**JdbcTemplate update 예제**

```java
this.jdbcTemplate.update("update t_actor set = ? where id = ?",  "Banjo", 5276L);
```



**JdbcTemplate delete 예제**

```java
this.jdbcTemplate.update("delete from actor where id = ?", Long.valueOf(actorId));
```



### JdbcTemplate외의 접근방법

JDBC 템플릿만 이용해도 기존의 JDBC 프로그래밍보다 훨씬 간단하게 프로그래밍을 할 수 있습니다. 하지만 아래의 클래스들을 이용한다면 좀 더 편하게 프로그래밍할 수 있습니다.

- **NamedParameterJdbcTemplate**

  ​	JdbcTemplate에서 JDBC statement 인자를 ?를 사용하는 대신 파라미터명을 사용하여 작성하는 것을 지원합니다.

  ​	[NamedParameterJdbcTemplate 예제](https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc-NamedParameterJdbcTemplate)

- **SimpleJdbcTemplate**

  ​	JdbcTemplate과 NamedParameterJdbcTemplate 합쳐 놓은 템플릿 클래스입니다. 이제 JdbcTemplate과 NamedParameterJdbcTemplate에 모든 기능을 제공하기 때문에 삭제 예정될 예정(deprecated)입니다.

  ​	[SimpleJdbcTemplate 예제](https://www.concretepage.com/spring/simplejdbctemplate-spring-example)

- **SimpleJdbcInsert**

​	테이블에 쉽게 데이터 insert 기능을 제공

​	[SimpleJdbcInsert 예제](https://www.tutorialspoint.com/springjdbc/springjdbc_simplejdbcinsert.htm)



---

참고 : https://www.boostcourse.org/web326/lecture/58973/?isDesc=false