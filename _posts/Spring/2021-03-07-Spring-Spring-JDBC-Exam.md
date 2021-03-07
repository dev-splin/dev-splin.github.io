---
title: "Spring : Spring JDBC 예제"
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

# Spring JDBC 예제

Spring JDBC를 예제를 통해 알아보겠습니다!



## Spring JDBC를 이용한 DAO작성 예제를 위한 다이어그램

[![img](https://cphinf.pstatic.net/mooc/20180208_103/1518068520531pRbvK_PNG/3_8_2_Spring_JDBC__DAO_.png?type=w760)
  ](https://www.boostcourse.org/web326/lecture/258527/?isDesc=false#)

먼저 Spring 컨테이너인 `ApplicationContext`는 설정 파일로 `ApplicationConfig`라는 클래스를 읽어들입니다.

`ApplicationConfig`의 `componentScan` 어노테이션이 `DAO` 클래스를 찾습니다. 찾은 `DAO` 클래스는 스프링 컨테이너가 관리하게 됩니다.

`ApplicationContext`는 `DBConfig`를 `import`하게 되고 `DBConfig` 클래스에서는 `Data Source`와 트랜잭션 매니저 객체를 생성합니다.

`DAO`는 필드로 `NamedParameterJdbcTemplate`과 `SimpleJdbcInsert`를 가지게 됩니다. 두 개의 객체 모두 **SQL의 실행을 편리하게 하도록 `Spring JDBC`에서 제공하는 객체**이기 때문에 DB연결을 위해서 내부적으로 `Data Source`를 필요로 합니다. 이 두개의 객체는 `RoleDao` 생성자에서 초기화를 하고 `RoleDao`의 메서드들을 구현하게 됩니다.

`Spring JDBC`를 사용하는 사용자는 파라미터와 `SQL`을 가장 많이 신경 써야 됩니다. `SQL`은 `RoleDao`에 `SQL`의 **상수로 정의를 해놓음으로써 나중에 `SQL`이 변경될 경우에 좀 더 편하게 수정**할 수 있도록 합니다.

한 건의 Role 정보를 저장하고 전달하기 위한 목적으로 `Role DTO`가 사용되고 있는 것을 볼 수 있습니다.



### Pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>kr.or.connect</groupId>
  <artifactId>daoexam</artifactId>
  <version>0.0.1-SNAPSHOT</version>

  <name>daoexam</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <spring.version>4.3.5.RELEASE</spring.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    
    <!-- Spring -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>${spring.version}</version>
	</dependency>
	
	<!-- JDBC -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-jdbc</artifactId>
		<version>${spring.version}</version>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-tx</artifactId>
		<version>${spring.version}</version>
	</dependency>
	
	<!-- basic data source -->
	<dependency>
		<groupId>org.apache.commons</groupId>
		<artifactId>commons-dbcp2</artifactId>
		<version>2.1.1</version>
	</dependency>

	
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.45</version>
	</dependency>
		
  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
      
      	<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>3.6.1</version>
			<configuration>
				<source>1.8</source>
				<target>1.8</target>
			</configuration>
		</plugin>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

먼저 `pom.xml` 에 **maven 플러그인 버전, Spring, JDBC, DataSource, mysql dependency와 properties를 추가**해줍니다.



### Role.java

```java
package kr.or.connect.daoexam.dto;

public class Role {
	private int roleId;
	private String description;
	
	public int getRoleId() {
		return roleId;
	}
	public void setRoleId(int roleId) {
		this.roleId = roleId;
	}

	public String getDescription() {
		return description;
	}
	public void setDescription(String description) {
		this.description = description;
	}
	
	@Override
	public String toString() {
		return "Role [roleID=" + roleId + ", description=" + description + "]";
	}
}
```

 **정보를 저장하고 전달하기 위한 목적**인 `DTO`입니다.



### ApplicationConfig.java

```java
package kr.or.connect.daoexam.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@ComponentScan(basePackages = {"kr.or.connect.daoexam.dao"})
@Import({DBConfig.class}) 
public class ApplicationConfig {

}
```

**`@ComponentScan`를 이용해  `kr.or.connect.daoexam.dao` 패키지안의 파일들을 스캔**합니다. 

**`@Import`라는 어노테이션을 이용해 설정 파일을 여러개로 나눠서 작성**할 수 있습니다. `DBConfig` 설정파일을 import 합니다. 

`{ }`안에 넣으면 여러가지를 지정할 수 있습니다.

### DBConfig.java

```java
package kr.or.connect.daoexam.config;

import javax.sql.DataSource;

import org.apache.commons.dbcp2.BasicDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
public class DBConfig {
	private String driverClassName = "com.mysql.jdbc.Driver";
    private String url = "jdbc:mysql://localhost:3306/connectdb?useUnicode=true&characterEncoding=utf8&useSSL=false";

    private String username = "connectuser";
    private String password = "connect123!@#";
    
    @Bean
    public DataSource dataSource() {
    	BasicDataSource dataSource = new BasicDataSource();
    	dataSource.setDriverClassName(driverClassName);
    	dataSource.setUrl(url);
    	dataSource.setUsername(username);
    	dataSource.setPassword(password);
    	return dataSource;
    }
}
```

`DBConfig`는 DB정보들을 저장해놓고, `DataSource`객체를 생성하고 정보를 설정해줍니다. 이 때, **`DataSource`는 외부 라이브러리 이므로 `@Bean`으로 생성**해야 합니다.



### RoleDaoSqls.java

```java
package kr.or.connect.daoexam.dao;

public class RoleDaoSqls {
	public static final String SELECT_ALL = "SELECT role_id, description FROM role order by role_id";
	public static final String UPDATE = "UPDATE role SET description = :description WHERE ROLE_ID = :roleId";
	public static final String SELECT_BY_ROLE_ID = "SELECT role_id, description FROM role where role_id = :roleId";
	public static final String DELETE_BY_ROLE_ID = "DELETE FROM role WHERE role_id = :roleId";
}
```

**sql문만 static으로 따로 관리해주는 class** 입니다.

JDBC에서 전체를 출력할 때 `*`를 쓰지 않고 컬럼 하나하나 작성해주는게 나중에 더 알아보기 쉽습니다.

이제까지 사용했던 query문과 똑같지만 **바인딩 되는 부분이 `?`가 아니고 `:`과 `속성명`을 붙여 작성**해줍니다.



### RoleDao.java

```java
package kr.or.connect.daoexam.dao;

import java.util.Collection;
import java.util.Collections;
import java.util.List;
import java.util.Map;

import javax.sql.DataSource;

import org.springframework.dao.EmptyResultDataAccessException;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.BeanPropertySqlParameterSource;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;
import org.springframework.stereotype.Repository;

import kr.or.connect.daoexam.dto.Role;

// static import를 사용하면 RoleDaoSqls 객체에 선언된 static 변수를 클래스 이름없이 바로 사용할 수 있습니다.
import static kr.or.connect.daoexam.dao.RoleDaoSqls.*;

// 이런 Dao객체에는 저장소의 역할을 한다는 의미로 "@Repository" 어노테이션을 붙입니다. "ComponentScan"이 스캔해줍니다.
@Repository
public class RoleDao {
	private NamedParameterJdbcTemplate jdbc;

	// DBMS에선 대소문자를 거의 구문하지 않기 때문에 '_'를 이용해 명명 규칙을 'role_id'같은 형태로 사용하고
	// Java는 camel표기법을 사용해 roleId 같은 형식으로 사용합니다.
	// "BeanPropertyRowMapper"는 DBMS와 Java의 이런 이름 규칙을 맞춰주는 기능을 가집니다.
	// 여기서 주의할 점은 "role_Id"가 "roleId"가 되는 것이기 때문에 자바에서 "roleID"같이 명명 규칙이 맞지 않는다면 값을 제대로 불러올 수 없습니다.
	// getter, setter도 마찬가지 입니다.
	private RowMapper<Role> rowMapper = BeanPropertyRowMapper.newInstance(Role.class);
	
	private SimpleJdbcInsert insertAction;
	
	// RoleDao객체를 만들 때 "dataSource"를 이용해 "NamedParameterJdbcTemplate"과 "SimpleJdbcInsert" 객체를 같이 생성해줍니다.
	public RoleDao(DataSource dataSource) {
		this.jdbc = new NamedParameterJdbcTemplate(dataSource);
        
		//  "SimpleJdbcInsert"를 생성할 때 테이블을 지정해줍니다.
		this.insertAction = new SimpleJdbcInsert(dataSource).withTableName("role");
	}
	
	public List<Role> selectAll() {
		
		// 두 번째 파라미터는 sql문(SELECT_ALL)에 바인딩 할 값이 있을 경우에 바인딩 할 값을 전달할 목적으로 "Collections.emptyMap()" 을 사용합니다.
		// 세 번째 파라미터는 "BeanPropertyRowMapper"객체를 이용해서 select 한 건, 한 건의 결과를 DTO에 저장하는 목적으로 사용하게 됩니다.
		// query() 메서드는 결과가 여러 건이었을 때 내부적으로 반복하면서 DTO를 생성하고 List에다 담아준 후 반환해줍니다.
		return jdbc.query(SELECT_ALL, Collections.emptyMap(), rowMapper);
	}
	

	public int insert(Role role) {
		// "BeanPropertySqlParameterSource"는 Role 객체를 받아, Role 객체에 있는 값을 맵으로 바꿔줍니다.
		// 이 때, 선언한 "roleId" 를 DBMS의 컬럼명 "role_id"로 알아서 매핑되는 객체를 생성해줍니다.
		SqlParameterSource params = new BeanPropertySqlParameterSource(role);
        
		// "SimpleJdbcInsert"을 이용하여 INSERT를 하게되면 따로 sql문이 필요없습니다.
		return insertAction.execute(params);
	}
	
	public int update(Role role) {
		SqlParameterSource params = new BeanPropertySqlParameterSource(role);
		return jdbc.update(UPDATE, params);
	}
	
	public int deleteById(Integer id) {
		// Map으로 바꿔주는 "SqlParameterSource"를 사용해도 되지만 값이 딱 하나만 들어오는 경우에는 "Collections.singletonMap"을 사용하는게 좋습니다.
		Map<String, ?> params = Collections.singletonMap("roleId", id);
		return jdbc.update(DELETE_BY_ROLE_ID, params);
	}
	
	public Role selectById(Integer id) {
		try {
			Map<String, ?> params = Collections.singletonMap("roleId", id);
            
			// 한 건 select할 때는 "queryForObject"를 사용합니다.
			return jdbc.queryForObject(SELECT_BY_ROLE_ID, params,rowMapper);
		
		// select 할 때 조건에 맞는 값이 없으면 "EmptyResultDataAccessException" Exception이 발생하게 됩니다.
		} catch (EmptyResultDataAccessException e) {
			return null;
		}
	}
}
```

스프링 JDBC의 주요 부분입니다. 코드가 길기 때문에 주석으로 바로 보는게 편할 것 같아 주석으로 달았습니다.



### JDBCTest.java

```java
package kr.or.connect.daoexam.main;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import kr.or.connect.daoexam.config.ApplicationConfig;
import kr.or.connect.daoexam.dao.RoleDao;
import kr.or.connect.daoexam.dto.Role;

public class JDBCTest {
	public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);
		
		RoleDao roleDao = ac.getBean(RoleDao.class);
		
		Role role = new Role();
        
        // selectAll
//      List<Role> list = roleDao.selectAll();		
//		for(Role role : list) {
//			System.out.println(role);
//		}
		
		// insert
//		role.setRoleId(500);
//		role.setDescription("CEO");
//		int count = roleDao.insert(role);
//		System.out.println(count + "입력하였습니다.");
		
		// update
//		role.setRoleId(102);
//		role.setDescription("PROGRAMMER");
//		int count = roleDao.update(role);
//		System.out.println(count + "건 수정하였습니다..");
		
		// selectById
		Role resultRole = roleDao.selectById(102);
		System.out.println(resultRole);
		
		// delete
		int deletecount = roleDao.deleteById(500);
		System.out.println(deletecount + "건 삭제하였습니다.");
		
		Role resultRole2 = roleDao.selectById(500);
		System.out.println(resultRole2);
		
	}
}

```

`RoleDao`의 기능들을 테스트 해봅니다. 

여기서 어디에도 `roleDao` 내부에서 사용되는 `DataSource`를 생성해주는 부분이 없습니다. 하지만 기능들이 다 실행이 되는데 왜 그럴까요? 그 이유는`ApplicationContext`가 어노테이션들을 찾아서 `Bean Factory`를 만들고, 이 때 **파라미터를 갖지 않는 `Bean(DataSource)`을 먼저 생성**하고 **파라미터를 가지는 `Bean(roleDao)`들에게 자동으로 적합한 파라미터를 전달해주고 생성**해주기 때문입니다. 



---

참고 : https://www.boostcourse.org/web326/lecture/258531/?isDesc=false