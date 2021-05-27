---
title: "Spring : Spring Security에서 DB를 이용한 로그인 및 회원가입"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Spring Security"
toc: true
toc_sticky: true
toc_label: 목차
---

# Spring Security에서 DB를 이용한 로그인 및 회원가입

[Spring Security를 이용한 로그인 및 로그아웃](https://dev-splin.github.io/spring/Spring-Security-Login-And-Logout/) 포스트에서는 DB를 이용하지 않았기 때문에 **Spring Security를 DB를 이용해 로그인 및 회원가입**을 해보겠습니다.

그리고 로그인 후 **사용자 정보를 확인하는 방법**도 알아보겠습니다.



## 데이터 베이스 모델링, 테이블 생성, 데이터 추가

`회원 정보(member 테이블)`와 `회원 권한(member_role 테이블)`은 아래 그림과 같이 1:N 구조로 되어 있습니다.


![img](https://cphinf.pstatic.net/mooc/20200301_80/1583074079670AURal_PNG/mceclip0.png)



### 테이블 생성

```mysql
-- -----------------------------------------------------
-- Table `member`
-- -----------------------------------------------------
CREATE TABLE `member` (
  `id` INT(11) NOT NULL AUTO_INCREMENT COMMENT 'member id',
  `name` VARCHAR(255) NOT NULL COMMENT 'member name',
  `password` VARCHAR(255) NOT NULL COMMENT '암호회된 password',
  `email` VARCHAR(255) NOT NULL UNIQUE COMMENT 'login id, email',
  `create_date` DATETIME NULL DEFAULT NULL COMMENT '등록일',
  `modify_date` DATETIME NULL DEFAULT NULL COMMENT '수정일',
  PRIMARY KEY (`id`)) ENGINE=InnoDB DEFAULT CHARSET=utf8;


-- -----------------------------------------------------
-- Table `member_role`
-- -----------------------------------------------------
CREATE TABLE `member_role` (
  `id` INT(11) NOT NULL AUTO_INCREMENT COMMENT 'role id',
  `member_id` INT(11) NOT NULL COMMENT 'member id fk',
  `role_name` VARCHAR(100) NOT NULL COMMENT 'role 이름 ROLE_ 로 시작하는 값이어야 한다.',
  PRIMARY KEY (`id`),
  FOREIGN KEY (`member_id`)
  REFERENCES `member` (`id`)
)  ENGINE=InnoDB DEFAULT CHARSET=utf8;
```



### 데이터 추가

```mysql
insert into member (id, name, password, email, create_date, modify_date) values ( 1, '강경미', '$2a$10$G/ADAGLU3vKBd62E6GbrgetQpEKu2ukKgiDR5TWHYwrem0cSv6Z8m', 'carami@example.com', now(), now());
insert into member (id, name, password, email, create_date, modify_date) values ( 2, '이정주', '$2a$10$G/ADAGLU3vKBd62E6GbrgetQpEKu2ukKgiDR5TWHYwrem0cSv6Z8m', 'toto@example.com', now(), now());

insert into member_role (id, member_id, role_name) values (1, 1, 'ROLE_USER');
insert into member_role (id, member_id, role_name) values (2, 1, 'ROLE_ADMIN');
insert into member_role (id, member_id, role_name) values (3, 2, 'ROLE_USER');
```



### 데이터 베이스 사용을 위해 pom.xml 수정

`pom.xml` 파일에 아래와 같이 Spring JDBC와 관련된 라이브러리를 추가합니다.

```xml
		<!-- mysql jdbc driver -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.19</version>
        </dependency>

        <!-- 커넥션 풀 라이브러리를 추가한다. spring boot 2의 경우는 hikariCP가 기본으로 사용된다.-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-dbcp2</artifactId>
            <version>2.6.0</version>
        </dependency>

        <!-- DataSource, Transaction등을 사용하려면 추가한다. spring-tx를 자동으로 포함시킨다.-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>
```



### 데이터 베이스 사용을 위해 설정 파일 수정

**원래는 따로 DBConfig를 만드는 게 좋지만**, 간단한 테스트를 위한 것이기 때문에 기존 스프링 설정 파일인 `ApplicationConfig.java`파일에 `DataSource`와 트랜잭션에 대한 설정을 합니다. 데이터베이스 접속 정보는 본인에 맞게 수정합니다.

```java
package org.edwith.webbe.securityexam.config;

import org.apache.commons.dbcp2.BasicDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;
import org.springframework.transaction.annotation.TransactionManagementConfigurer;

import javax.sql.DataSource;

// 레이어드 아키텍처에서 Controller가 사용하는 Bean들에 대해 설정을 합니다.
// dao, service를 컴포넌트 스캔하여 찾도록 합니다.
// 어노테이션으로 트랜잭션을 관리하기 위해 @EnableTransactionManagement를 설정하였습니다.
@Configuration
@ComponentScan(basePackages = {"org.edwith.webbe.securityexam.dao", "org.edwith.webbe.securityexam.service"})
@EnableTransactionManagement
public class ApplicationConfig implements TransactionManagementConfigurer {

    // mysql 드라이버 클래스 이름은 "com.mysql.jdbc.Driver"에서 버전 6이후에는 "com.mysql.cj.jdbc.Driver"로 변경되었습니다.
    private String driverClassName = "com.mysql.cj.jdbc.Driver";    
    private String url = "jdbc:mysql://localhost:3306/connectdb?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC";
    private String username = "connectuser";
    private String password = "connect123!@#";

     // 커넥션 풀과 관련된 Bean을 생성합니다.
    @Bean
    public DataSource dataSource(){
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);

        return dataSource;
    }

    // 트랜잭션 관리자를 생성합니다.
    @Bean
    public PlatformTransactionManager transactionManager(){
        return new DataSourceTransactionManager(dataSource());
    }

    @Override
    public PlatformTransactionManager annotationDrivenTransactionManager() {
        return transactionManager();
    }

}
```

만약 DB연결시 아래와 같은 오류가 발생한다면 `&serverTimezone=UTC` 를 url에 붙여줘야 합니다.

 `java.sql.SQLException: Cannot create PoolableConnectionFactory (The server time zone value 'KST' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the 'serverTimezone' configuration property) to use a more specifc time zone value if you want to utilize time zone support.)`





## 로그인



### 데이터베이스로 부터 읽어오기 위한 DTO와 DAO   



#### Member.java

회원 정보 한 건의 정보를 저장하는 `Member DTO 클래스`를 작성합니다.

```java
package org.edwith.webbe.securityexam.dto;

import java.util.Date;

public class Member {
    private Long id;
    private String name;
    private String password;
    private String email;
    private Date createDate;
    private Date modifyDate;

    public Member() {
        createDate = new Date();
        modifyDate = new Date();
    }

    public Member(Long id, String name, String password, String email) {
        this();
        this.name = name;
        this.password = password;
        this.email = email;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    public Date getModifyDate() {
        return modifyDate;
    }

    public void setModifyDate(Date modifyDate) {
        this.modifyDate = modifyDate;
    }
}
```



#### MemberDaoSqls.java

**Member DAO에서 읽어들일 SQL정보를 가지고 있는 MemberDaoSqls클래스**를 작성합니다. email정보와 일치하는 회원 정보를 읽어들이는 것을 알 수 있습니다.

```java
package org.edwith.webbe.securityexam.dao;

public class MemberDaoSqls {
	public static final String SELECT_ALL_BY_EMAIL = "SELECT id, name, password, email, create_date, modify_date FROM member WHERE email = :email";
}
```



#### MemberDao.java

MemberDao클래스를 작성합니다. **데이터베이스의 정보를 읽어 들이는 레이어에 속하기 때문에 @Component가 아니라 @Repository가 붙은 것을 확인**할 수 있습니다.

```java
package org.edwith.webbe.securityexam.dao;

import org.edwith.webbe.securityexam.dto.Member;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.stereotype.Repository;

import javax.sql.DataSource;
import java.util.HashMap;
import java.util.Map;

@Repository
public class MemberDao {
	private NamedParameterJdbcTemplate jdbc;
	// BeanPropertyRowMapper는 Role클래스의 프로퍼티를 보고 자동으로 칼럼과 맵핑해주는 RowMapper객체를 생성합니다.
	// roleId 프로퍼티는 role_id 칼럼과 맵핑이 됩니다.
	private RowMapper<Member> rowMapper = BeanPropertyRowMapper.newInstance(Member.class);

	public MemberDao(DataSource dataSource){
		this.jdbc = new NamedParameterJdbcTemplate(dataSource);
	}

	public Member getMemberByEmail(String email){
		Map<String, Object> map = new HashMap<>();
		map.put("email", email);

		return jdbc.queryForObject(MemberDaoSqls.SELECT_ALL_BY_EMAIL, map, rowMapper);
	}
}
```



#### MemberRole.java

회원의 권한(Role)정보를 저장하기 위한 `MemberRole DTO클래스`를 작성합니다.

```java
package org.edwith.webbe.securityexam.dto;

public class MemberRole {
	private Long id;
	private Long memberId;
	private String roleName;

	public MemberRole() {
	}

	public MemberRole(Long memberId, String roleName) {
		this.memberId = memberId;
		this.roleName = roleName;
	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public Long getMemberId() {
		return memberId;
	}

	public void setMemberId(Long memberId) {
		this.memberId = memberId;
	}

	public String getRoleName() {
		return roleName;
	}

	public void setRoleName(String roleName) {
		this.roleName = roleName;
	}

}
```



#### MemberRoleDaoSqls.java

**email에 해당하는 권한 정보를 읽어들이기 위해서 member테이블과 member_role테이블을 조인(JOIN)하여 결과를 얻는 SQL을 가진 MemberRoleDaoSqls 클래스**를 작성합니다.

```java
package org.edwith.webbe.securityexam.dao;

public class MemberRoleDaoSqls {
	public static final String SELECT_ALL_BY_EMAIL = "SELECT mr.id, mr.member_id, mr.role_name FROM member_role mr JOIN member m ON mr.member_id = m.id WHERE m.email = :email";
}
```



#### MemberRoleDao.java

권한 정보를 읽어들이는 `MemberRole Dao 클래스`를 작성합니다.

```java
package org.edwith.webbe.securityexam.dao;

import org.edwith.webbe.securityexam.dto.MemberRole;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.stereotype.Repository;

import javax.sql.DataSource;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Repository
public class MemberRoleDao {
	private NamedParameterJdbcTemplate jdbc;
	// BeanPropertyRowMapper는 Role클래스의 프로퍼티를 보고 자동으로 칼럼과 맵핑해주는 RowMapper객체를 생성합니다.
	// roleId 프로퍼티는 role_id 칼럼과 맵핑이 됩니다.
	private RowMapper<MemberRole> rowMapper = BeanPropertyRowMapper.newInstance(MemberRole.class);

	public MemberRoleDao(DataSource dataSource){
		this.jdbc = new NamedParameterJdbcTemplate(dataSource);
	}

	public List<MemberRole> getRolesByEmail(String email){
		Map<String, Object> map = new HashMap<>();
		map.put("email", email);

		return jdbc.query(MemberRoleDaoSqls.SELECT_ALL_BY_EMAIL, map, rowMapper);
	}
}
```



### 데이터베이스 접속 및 Dao클래스 테스트

**데이터베이스 접속, MemberDao, MemberRoleDao클래스가 알맞게 동작하는지 테스트 클래스를 작성**합니다. 해당 클래스를 실행했을 때 아무 문제도 발생하지 않으면 다음 단계로 진행합니다.

```java
package org.edwith.webbe.securityexam.dao;

import org.edwith.webbe.securityexam.config.ApplicationConfig;
import org.edwith.webbe.securityexam.dto.Member;
import org.edwith.webbe.securityexam.service.security.UserEntity;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.sql.DataSource;
import java.sql.Connection;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {ApplicationConfig.class})
public class MemberDaoTest {
    @Autowired
    DataSource dataSource;

    @Autowired
    MemberDao memberDao;

    @Autowired
    MemberRoleDao memberRoleDao;

    @Test
    public void configTest() throws Exception{
        // 아무 작업도 하지 않습니다. 실행이 잘된다는 것은 Spring 설정이 잘 되어 있다는 것을 의미합니다.
    }

    @Test
    public void connnectionTest() throws Exception{
        Connection connection = dataSource.getConnection();
        Assert.assertNotNull(connection);
    }

    @Test
    public void getUser() throws Exception{
        Member member = memberDao.getMemberByEmail("carami@example.com");
        Assert.assertNotNull(member);
        Assert.assertEquals("강경미", member.getName());
    }
}
```



### Dao를 이용한 서비스 수정 및 실행



#### MemberServiceImpl.java

carami사용자에 대해 무조건 리턴하던 `MemberServiceImpl`클래스를 수정합니다.

```java
package org.edwith.webbe.securityexam.service;

import org.edwith.webbe.securityexam.dao.MemberDao;
import org.edwith.webbe.securityexam.dao.MemberRoleDao;
import org.edwith.webbe.securityexam.dto.Member;
import org.edwith.webbe.securityexam.dto.MemberRole;
import org.edwith.webbe.securityexam.service.security.UserEntity;
import org.edwith.webbe.securityexam.service.security.UserRoleEntity;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.ArrayList;
import java.util.List;

@Service
public class MemberServiceImpl implements MemberService {
    // 생성자에 의해 주입되는 객체이고, 해당 객체를 초기화할 필요가 이후에 없기 때문에 final로 선언하였습니다.
    // final로 선언하고 초기화를 안한 필드는 생성자에서 초기화를 해줍니다.
    private final MemberDao memberDao;
    private final MemberRoleDao memberRoleDao;

    // @Service가 붙은 객체는 스프링이 자동으로 Bean으로 생성하는데
    // 기본생성자가 없고 아래와 같이 인자를 받는 생성자만 있을 경우 자동으로 관련된 타입이 Bean으로 있을 경우 주입해서 사용하게 됩니다.
    public MemberServiceImpl(MemberDao memberDao, MemberRoleDao memberRoleDao) {
        this.memberDao = memberDao;
        this.memberRoleDao = memberRoleDao;
    }

    @Override
    @Transactional
    public UserEntity getUser(String loginUserId) {
        Member member = memberDao.getMemberByEmail(loginUserId);
        return new UserEntity(member.getEmail(), member.getPassword());
    }

    @Override
    @Transactional
    public List<UserRoleEntity> getUserRoles(String loginUserId) {
        List<MemberRole> memberRoles = memberRoleDao.getRolesByEmail(loginUserId);
        List<UserRoleEntity> list = new ArrayList<>();

        for(MemberRole memberRole : memberRoles) {
            list.add(new UserRoleEntity(loginUserId, memberRole.getRoleName()));
        }
        return list;
    }

}
```



#### 실행

웹 애플리케이션을 재시작한 후 DB에서 저장한 정보 `ID : carami@example.com`, `암호 : 1234`를 이용하면 로그인, 로그아웃이 잘 수행되는 것을 볼 수 있습니다.





## 회원가입

`SecurityConfig`에서 `members/joinform`은 회원가입을 위한 폼, `members/join`은 form 데이터들을 DB에 등록해 주는 API 입니다.



### 회원가입을 위한 jsp 생성 및 수정



#### joinform.jsp

회원가입을 위한 `joinform.jsp`를 생성합니다.

```jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
  <html>
    <head>
      <title>회원가입 </title>
    </head>
  <body>
    <div>
      <div>
        <form method="post" action="/securityexam/members/join">
          <div>
            <label>ID(email 형식)</label>
            <input type="text" name="email">
          </div>
          <div>
            <label>암호</label>
            <input type="password" name="password">
          </div>
          <div>
            <label>이름</label>
            <input type="text" name="name">
          </div>
          <div>
            <label></label>
            <input type="submit" value="회원가입등록">
          </div>
        </form>
      </div>
    </div>
  </body>
</html>
```



#### loginform.jsp

로그인 화면에서 joinform.jsp로 이동하는 회원가입 버튼을 만듭니다.

```jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
  <html>
    <head>
      <title>로그인 </title>
    </head>
  <body>
    <div>
      <div>
        <form method="post" action="/securityexam/authenticate">
          <div>
            <label>ID</label>
            <input type="text" name="userId">
          </div>
          <div>
            <label>암호</label>
            <input type="password" name="password">
          </div>
          <div>
            <label></label>
            <input type="submit" value="로그인">
          </div>
        </form>
      </div>
       <a href="/securityexam/members/joinform">회원가입</a>
    </div>
  </body>
</html>
```



### 데이터베이스에 데이터를 넣기 위한 메서드 추가

회원가입을 하게 되면 `member` 테이블에 회원정보를 넣고, `member_role` 테이블에 해당 회원의 권한을 넣어주어야 하기 때문에 `MemberDao`, `MeberRoleDao`에 데이터를 삽입하는 메서드를 만들어 줍니다.



#### MemberDao.java

`SimpleJdbcInsert`로 간단하게 insert를 할 수 있습니다. INSERT가 성공하면 key(id)를 반환합니다.

```java
@Repository
public class MemberDao {
	private SimpleJdbcInsert insertAction;
	
    ...

	public MemberDao(DataSource dataSource){
		this.jdbc = new NamedParameterJdbcTemplate(dataSource);
		this.insertAction = new SimpleJdbcInsert(dataSource)
				.withTableName("member")
				.usingGeneratedKeyColumns("id");
	}

	...
	
	public Long insert(Member member) {
		SqlParameterSource params = new BeanPropertySqlParameterSource(member);
		return insertAction.executeAndReturnKey(params).longValue();
	}
}
```



#### MemberRoleDao.java

마찬가지로 `SimpleJdbcInsert`로 간단하게 insert를 할 수 있습니다.  INSERT가 성공하면 key(id)를 반환합니다.

```java
@Repository
public class MemberRoleDao {
	private SimpleJdbcInsert insertAction;
	
    ...

	public MemberRoleDao(DataSource dataSource){
		this.jdbc = new NamedParameterJdbcTemplate(dataSource);
		this.insertAction = new SimpleJdbcInsert(dataSource)
				.withTableName("member_role")
				.usingGeneratedKeyColumns("id");
	}

	...
	
	public Long insert(MemberRole memberRole) {
		SqlParameterSource parmas = new BeanPropertySqlParameterSource(memberRole);
		return insertAction.executeAndReturnKey(parmas).longValue();
	}
}
```



#### MemberServiceImpl.java

`member_role테이블`은 `member테이블`의 id를 외래키로 가지기 때문에 `memberDao`에서 insert 후 반환된 id값을 `MemberRole`을 만들 때 이용합니다.  **Member의 id는 member테이블에 생성될 때 자동으로 만들어지기 때문에 INSERT 후 따로 설정**해주어야 합니다.

INSERT이기 때문에 `readOnly = false`로 해줍니다.

```java
@Service
public class MemberServiceImpl implements MemberService {
   
    ...
    
	@Override
    @Transactional(readOnly = false)
    public Member addMember(Member member) {
    	
    	Long memberId = memberDao.insert(member);
    	member.setId(memberId);
    	
    	MemberRole memberRole = new MemberRole();
    	memberRole.setMemberId(memberId);
    	memberRole.setRoleName("ROLE_USER");
    	memberRoleDao.insert(memberRole);
    	
    	return member;
    }
}
```



### Controller 작성 및 실행

`joinform.jsp`로 이동하는 GET요청 API와 `joinform.jsp`에서 POST 요청으로 들어온 데이터를 처리하는 API를 만듭니다.





#### MemberController.java

데이터를 처리할 때 `PasswordEncoder`를 이용해서 비밀번호를 인코딩해주어야 합니다.

```java
@Controller
@RequestMapping(path = "/members")
public class MemberController {
	 // 스프링 컨테이너가 생성자를 통해 자동으로 주입합니다.
    private final MemberService memberService;
    private final PasswordEncoder passwordEncoder;

    public MemberController(MemberService memberService, PasswordEncoder passwordEncoder){
        this.memberService = memberService;
        this.passwordEncoder = passwordEncoder;
    }

    ...
    
    @GetMapping("/joinform")
    public String registerform() {
    	return "members/joinform";
    }
    
    @PostMapping("/join")
    public String register(@ModelAttribute Member member) {
    	member.setPassword(passwordEncoder.encode(member.getPassword()));
    	
    	Member insertMember = memberService.addMember(member);
    	
    	return "redirect:/loginform";
    }
}
```



#### 실행

웹 애플리케이션을 재시작한 후 로그인 폼에서 회원가입 페이지에 들어갑니다.

회원가입 페이지에서 아이디(이메일), 이름, 비밀번호를 입력 후 DB를 확인해보면 데이터가 제대로 인코딩되어서 들어간 것을 볼 수 있고, 로그인 로그아웃도 잘 되는 것을 볼 수있습니다.



## 로그인 한 사용자 정보 읽어 오기

사용자가 로그인을 한 상태라면, 스프링 MVC는 컨트롤러 메소드에 회원정보를 저장하고 있는 Principal객체(Security에서 지원)를 파라미터로 받을 수 있습니다. 이 Principal 객체를 이용해 로그인 한 사용자의 정보를구할 수 있습니다.



### MemberServiceImpl.java

데이터베이스로 부터 아이디(Email)을 이용해 사용자 정보를 가져오는 메서드를 만듭니다.

```java
@Service
public class MemberServiceImpl implements MemberService {
    
    ...

	@Override
	public Member getMemberByEmail(String email) {
		return memberDao.getMemberByEmail(email);
	}
}
```



### MemberController.java

Principal 객체의 `getName()`메서드를 이용해 로그인 아이디를 구할 수 있습니다. 로그인 아이디를 구하고 해당 아이디를 이용해 데이터베이스로부터 회원 정보를 읽어 옵니다.

```java
@Controller
@RequestMapping(path = "/members")
public class MemberController {
    
	...
    
    @GetMapping("/memberinfo")
    public String memberInfo(Principal principal, ModelMap modelMap) {
    	String email = principal.getName();
    	Member member = memberService.getMemberByEmail(email);
    	modelMap.addAttribute("member",member);
    	
    	return "members/memberinfo";
    }
}
```



### memberinfo.jsp

사용자 정보를 화면에 보여줄 jsp 페이지 입니다.

```jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
  <head>
    <title>회원 가입폼 </title>
  </head>
  <body>
    <div>
      <div>
        <h1>회원정보</h1>
        <p>로그인한 회원 정보를 표기합니다.</p>
      </div>

        <div>
          <label>id</label>
          <p>${member.id}</p>
        </div>
        <div>
          <label>이름</label>
          <p>${member.name}</p>
        </div>
        <div>
          <label>암호</label>
          <p>${member.password}</p>
        </div>
        <div>
          <label>등록일</label>
          <p>${member.createDate}</p>
        </div>
        <div>
          <label>수정일</label>
          <p>${member.modifyDate}</p>
        </div>

    </div>
  </body>
</html>
```



### 실행

웹 애플리케이션을 재시작한 후 로그인 합니다. 로그인 후 members/memberinfo 페이지에 들어가보면 사용자 정보가 잘 나오는 것을 볼 수 있습니ㅏ.



---

참고 : [https://www.boostcourse.org/web326/lecture/60591?isDesc=false](https://www.boostcourse.org/web326/lecture/60591?isDesc=false)

[https://www.boostcourse.org/web326/lecture/60592?isDesc=false](https://www.boostcourse.org/web326/lecture/60592?isDesc=false)

