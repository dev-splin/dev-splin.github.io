---
title: "Spring : DTO(Data Transfer Object), DAO(Data Transfer Object), Connection Pool, Data Source"
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

# DTO(Data Transfer Object), DAO(Data Transfer Object), Connection Pool, Data Source



## DTO(Data Transfer Object)

`DTO`란 `Data Transfer Object`의 약자입니다. **계층간 데이터 교환을 위한 자바빈즈**입니다. 여기서의 계층이란 **컨트롤러 뷰, 비지니스 계층, 퍼시스턴스 계층을 의미**합니다. 일반적으로 `DTO`는 로직을 가지고 있지 않고, **순수한 데이터 객체**입니다. 필드와 `getter`, `setter`를 가집니다. 추가적으로 `toString()`, `equals()`, `hashCode()`등의 Object 메소드를 오버라이딩 할 수 있습니다.

 

### DTO의 예

```java
public class ActorDTO {
    private Long id;
    private String firstName;
    private String lastName;
    public String getFirstName() {
        return this.firstName;
    }
    public String getLastName() {
        return this.lastName;
    }
    public Long getId() {
        return this.id;
    }
    // ......
}
```



## DAO(Data Access Object)

`DAO`란 `Data Access Object`의 약자로 **데이터를 조회하거나 조작하는 기능을 전담하도록 만든 객체**입니다. 보통 데이터베이스를 조작하는 기능을 전담하는 목적으로 만들어집니다.



## ConnectionPool

DB연결은 비용이 많이 듭니다. **커넥션 풀은 미리 커넥션을 여러 개 맺어 둡니다. 커넥션이 필요하면 커넥션 풀에게 빌려서 사용한 후 반납**합니다. 커넥션을 반납하지 않으면 어떻게 될까요?

`ConnectionPool`을 사용할 때는 `Connection`을 되도록 빨리 사용하고 빨리 반납을 해야 합니다. 그렇지 않으면 `ConnectionPool`에서 사용 가능한 `Connection`이 없어서 프로그램이 늦어지거나 심할 경우에는 장애를 발생시킬 수도 있습니다.

[![img](https://cphinf.pstatic.net/mooc/20180208_14/15180684447693OANG_JPEG/3_8_2_ConnectionPool.jpg?type=w760)](https://www.boostcourse.org/web326/lecture/258527/?isDesc=false#)



## DataSource

`DataSource`는 **커넥션 풀을 관리하는 목적으로 사용되는 객체**입니다. `DataSource`를 이용해 커넥션을 얻어오고 반납하는 등의 작업을 수행합니다.

 



## Spring JDBC를 이용한 DAO작성 예제를 위한 다이어그램

[![img](https://cphinf.pstatic.net/mooc/20180208_103/1518068520531pRbvK_PNG/3_8_2_Spring_JDBC__DAO_.png?type=w760)
  ](https://www.boostcourse.org/web326/lecture/258527/?isDesc=false#)

먼저 Spring 컨테이너인 `ApplicationContext`는 설정 파일로 `ApplicationConfig`라는 클래스를 읽어들입니다.

`ApplicationConfig`의 `componentScan` 어노테이션이 `DAO` 클래스를 찾습니다. 찾은 `DAO` 클래스는 스프링 컨테이너가 관리하게 됩니다.

`ApplicationContext`는 `DBConfig`를 `import`하게 되고 `DBConfig` 클래스에서는 `Data Source`와 트랜잭션 매니저 객체를 생성합니다.

`DAO`는 필드로 `NamedParameterJdbcTemplate`과 `SimpleJdbcInsert`를 가지게 됩니다. 두 개의 객체 모두 **SQL의 실행을 편리하게 하도록 `Spring JDBC`에서 제공하는 객체**이기 때문에 DB연결을 위해서 내부적으로 `Data Source`를 필요로 합니다. 이 두개의 객체는 `RoleDao` 생성자에서 초기화를 하고 `RoleDao`의 메서드들을 구현하게 됩니다.

`Spring JDBC`를 사용하는 사용자는 파라미터와 `SQL`을 가장 많이 신경 써야 됩니다. `SQL`은 `RoleDao`에 `SQL`의 **상수로 정의를 해놓음으로써 나중에 `SQL`이 변경될 경우에 좀 더 편하게 수정**할 수 있도록 합니다.

한 건의 Role 정보를 저장하고 전달하기 위한 목적으로 `Role DTO`가 사용되고 있는 것을 볼 수 있습니다.



---

참고 : https://www.boostcourse.org/web326/lecture/258528/?isDesc=false