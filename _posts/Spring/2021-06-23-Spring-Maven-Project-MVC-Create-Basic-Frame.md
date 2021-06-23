---
title: "Spring : Maven Project MVC 기본적인 틀 만들기"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
toc: true
toc_sticky: true
toc_label: 목차
---

# Maven Project MVC 기본적인 틀 만들기

boostcourse 강의를 듣고 혼자 실습하다가.. 틀 만드는 것 부터 너무 복잡해서 정리를 해놔야겠단 생각에 포스팅합니다..!!



## 프로젝트 만들기

1. `File -> New -> Maven Project`
2. `Use default Workspace location`에 체크되어 있는 상태로 놔두고 `Next`

3. `Grup Id`가 `org.apache.maven.archetypes`이고 `Artifact Id`가 `maven-archetype-webapp`인 Archetype을 선택하고 `Next`

4. 원하는 `Group Id`와 `Artifact Id`를 설정후 `Finish`



### pom.xml 설정하기

다양한 라이브러리를 가져와서 사용할 수 있지만 일단은 제가 사용했던 pom.xml을 가져오겠습니다.

설정을 추가하면 `프로젝트 오른쪽 클릭 -> Maven -> Update Project` 를 반드시 해주야합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>kr.or.connect</groupId>
  <artifactId>reservation</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>reservation Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <!-- pom.xml 내에서 상수처럼 사용할 수 있습니다. -->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <!-- Eclipse에서는 web.xml 파일을 작성하지 않고, Java Config를 사용할 때 failOnMissingWebXml를 false로 설정합니다. -->
    <failOnMissingWebXml>false</failOnMissingWebXml>
    <spring.version>5.2.3.RELEASE</spring.version>
    <jackson2.version>2.8.6</jackson2.version>
    <org.mapstruct.version>1.4.2.Final</org.mapstruct.version>  
  </properties>

  
  <dependencies>
      <!-- junit  -->
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
          <scope>test</scope>
      </dependency>
      
      
      <!-- Mock -->
      <dependency>
          <groupId>org.mockito</groupId>
          <artifactId>mockito-core</artifactId>
          <version>1.9.5</version>
          <scope>test</scope>
      </dependency>
      <!-- Mock에서 jsonpath를 이용할 수 있게 합니다. -->
      <dependency>
          <groupId>com.jayway.jsonpath</groupId>
          <artifactId>json-path</artifactId>
          <version>2.4.0</version>
      <dependency>
          
      <!-- spring -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>${spring.version}</version>
      </dependency>
      <!-- spring webmvc에 대한 의존성을 추가합니다.
      spring webmvc에 대한 의존성을 추가하게 되면 spring-web, spring-core등이 자동으로 의존성이 추가됩니다.-->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>${spring.version}</version>
      </dependency>
      <!-- jUnit을 확장한 스프링의 테스트 라이브러리 -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>${spring.version}</version>
      </dependency>
          
          
      <!-- Spring Security -->
      <!-- Spring Security Core -->
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-core</artifactId>
          <version>${spring.version}</version>
      </dependency>
      <!-- Spring Security Config -->
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-config</artifactId>
          <version>${spring.version}</version>
      </dependency>
      <!-- Spring Security Web -->
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-web</artifactId>
          <version>${spring.version}</version>
      </dependency>
      <!-- Spring Security JSP Custom Tags -->
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-taglibs</artifactId>
          <version>${spring.version}</version>
      </dependency>
      
          
      <!-- servlet -->
      <!-- servlet-api이다. tomcat에 배포될 경우엔 사용되지 않도록 하기 위해서 scope를 provided로 설정하였습니다. -->
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
          <scope>provided</scope>
      </dependency>
      <!-- jsp-api이다. tomcat에 배포될 경우엔 사용되지 않도록 하기 위해서 scope를 provided로 설정하였습니다. -->
      <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>javax.servlet.jsp-api</artifactId>
          <version>2.3.1</version>
          <scope>provided</scope>
      </dependency>
          
          
      <!-- jstl -->
      <!-- jstl은 tomcat이 기본 지원하지 않습니다. 그렇기 때문에 tomcat에도 배포가 되야 합니다.-->
      <dependency>
          <groupId>jstl</groupId>
          <artifactId>jstl</artifactId>
          <version>1.2</version>
      </dependency>
          
          
      <!-- JDBC -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>${spring.version}</version>
      </dependency>
      <!-- 트랜잭션을 사용하기 위한 설정입니다. -->
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
      <!-- JDBC에서 mysql을 사용하기 위한 설정입니다. -->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.45</version>
      </dependency>
          
          
      <!-- jackson -->
      <!-- RestController의 json 변환을 위해 필요합니다 -->
      <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-core</artifactId>
          <version>2.9.8</version>
      </dependency>
      <!-- jackson-core 및 jackson-annotation 라이브러리의 의존성을 포함 -->
      <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-databind</artifactId>
          <version>${jackson2.version}</version>
      </dependency>
      <dependency>
          <groupId>com.fasterxml.jackson.datatype</groupId>
          <artifactId>jackson-datatype-jdk8</artifactId>
          <version>${jackson2.version}</version>
      </dependency>
          
          
      <!-- Lombok과 mapstruct를 같이 사용할 경우, Lombok이 먼저 와야 합니다. -->
      <!-- Lombok -->
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>1.18.20</version>
          <scope>provided</scope>
      </dependency>
          
          
      <!-- mapstruct -->
      <dependency>
          <groupId>org.mapstruct</groupId>
          <artifactId>mapstruct</artifactId>
          <version>${org.mapstruct.version}</version>
      </dependency>

          
      <!-- hibernate-validator -->
      <dependency>
          <groupId>org.hibernate</groupId>
          <artifactId>hibernate-validator</artifactId>
          <version>6.1.0.Final</version>
      </dependency> 
          
          
      <!-- java 9 이상에서 추가해줘야 합니다. @PostConstruct 등을 사용하려면 필요합니다-->
      <dependency>
          <groupId>javax.annotation</groupId>
          <artifactId>javax.annotation-api</artifactId>
          <version>1.3.2</version>
      </dependency>
          
          
      <!-- swagger2 의존성 추가. Swagger 사용을 위해서는 구현체인 springfox-swagger2 가 필요 -->
      <dependency>
          <groupId>io.springfox</groupId>
          <artifactId>springfox-swagger2</artifactId>
          <version>2.6.1</version>
      </dependency>
      <!--  또 가장 중요한 (사용목적이라해도 과언이 아닌) UI 적으로 확인을 위해서는 springfox-swagger-ui 라이브러리가 필요합니다. -->
      <dependency>
          <groupId>io.springfox</groupId>
          <artifactId>springfox-swagger-ui</artifactId>
          <version>2.6.1</version>
      </dependency>
  </dependencies>

  <build>
    <finalName>reservation</finalName>
      <plugins>
      
      	<!-- maven 버전 설정을 위한 플러그인 입니다.-->
      	<plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.6.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                
                <!-- mapstruct-processor을 설정해줍니다. -->
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${org.mapstruct.version}</version>
                    </path>
                    <!-- other annotation processors -->
                </annotationProcessorPaths>
                
                <!-- Lombok 의존성을 설정해도 사용되지 않을 경우. 아래의 부분을 추가 -->
                <annotationProcessors>
                    <annotationProcessor>lombok.launch.AnnotationProcessorHider$AnnotationProcessor</annotationProcessor>
                </annotationProcessors>
            </configuration>
          </plugin>
      	
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
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
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
  </build>
</project>
```

MapStruct와 Lombok을 같이 사용하게 되면, 의존성 설정할 때 순서를 신경써주어야 합니다. 

또, pluginManagement와 plugins의 차이와 주의할 점에 대해서도 알아야합니다.

[MapStruct 의존성 설정 포스트 링크(MapStruct 사용법 및 ModelMapper와의 비교 내용도 포함)](https://dev-splin.github.io/spring/Spring-ModelMapper,MapStruct/#mapstruct)

[Lombok의 사용법 및 주의점 포스트 링크](https://dev-splin.github.io/spring/Spring-Lombok/)

[pluginManagement와 plugins의 차이와 주의할 점 포스트 링크](https://dev-splin.github.io/spring/Spring-pluginManagement,plugins/)



### Servlet 버전 설정하기

자바 어노테이션을 사용하기위해 Servlet 버전을 3.0이상으로 바꿔줍니다.

1. web.xml 위쪽에 버전이 있는 부분을 삭제합니다.

```xml
<!--<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" > 이 부분을 삭제--> 
<?xml version="1.0" encoding="UTF-8"?> <!-- 이부분을 추가해줍니다. -->
```

2. `Navigator -> .settings -> org.eclipse.wst.common.project.facet.core.xml`에서 `jst.web` 부분의 버전을 원하는 버전으로 바꿔줍니다.
3. Eclipse를 재시작합니다.



### DataBase 만들기

[DataBase 명령어](https://dev-splin.github.io/database/Database-cmd-Command(DB)/)를 참고해 DB 계정을 만듭니다.

`src/main/resources/application.properties` 파일에 다음과 같이 설정합니다.

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://domain:port/dbName?useUnicode=true&characterEncoding=utf8
spring.datasource.username=dbUserName
spring.datasource.password=dbPasswd
```

`application.properties`는 스프링 사용시 필요한 옵션들을 작성하여 추가할 수 있습니다. 예를 들면 메일 서버를 구축한다던가 혹은 db의 종류를 설정, 로그사용여부 등을 설정이 가능합니다. 이 후, 아래와 같이 사용할 수 있습니다.



```java
@Configuration
@PropertySource("classpath:application.properties")
public class DBConfig implements TransactionManagementConfigurer {
	
	@Autowired
	Environment env;
	
	private String driverClassName = env.getProperty("spring.datasource.driver-class-name");
	private String url = env.getProperty("spring.datasource.url");
	private String username = env.getProperty("spring.datasource.username");
	private String password = env.getProperty("spring.datasource.password");
    
    ...
}
```

`Environment`는 `시스템 환경변수 , JVM 시스템 프로퍼티 , 프로퍼티 파일 등의 프로퍼티`를 `PropertySource`라는 것으로 통합관리합니다.

[Environment란?](https://dev-splin.github.io/spring/Spring-Properties-Environment-Profile/)

### DispatcherServlet을 FrontController로 설정하기

설명이 필요하기 때문에 [링크](https://dev-splin.github.io/spring/Spring-MVC-Example(DispatcherServlet-FrontController-Set)/)로 대체하겠습니다. 

[WebApplicationInitializer / AbstractAnnotationConfigDispatcherServletInitializer](https://dev-splin.github.io/spring/Spring-WebApplicationInitializer,AbstractAnnotationConfigDispatcherServletInitializer/)



### Controller 작성하기

간단한 컨트롤러를 만들어서 테스트 해봅니다.

[Controller작성하기](https://dev-splin.github.io/spring/Spring-MVC-Example(Controller)/)



### JDBC 연결하기

[JDBC 연결하기](https://dev-splin.github.io/spring/Spring-Spring-JDBC-Exam/)

[Layered Architecture](https://dev-splin.github.io/spring/Spring-Layered-Architecture/)를 보면 `DispatcherServlet`과  나머지 부분을 분리하여 사용하는게 좋다고 했습니다.

`DispatcherServlet`을 경우에 따라서 2개 이상 설정할 수 있는데 이 경우에는 각각의 `DispathcerServlet`의 `ApplicationContext`가 각각 독립적이기 때문에 각각의 설정 파일에서 생성한 빈을 서로 사용할 수 없습니다.

위의 경우와 같이 동시에 필요한 빈은 `ContextLoaderListener`를 사용함으로써 공통으로 사용하게 할 수 있습니다.

`ContextLoaderListener`의 설정은 `DispatcherServlet을 FrontController로 설정하기`에 포함되어 있습니다.





## 그 외에 프로젝트 진행 시 유용한 자료들



### Mapping 및 객체

[Entity, VO, DTO의 차이](https://dev-splin.github.io/spring/Spring-Entity-DTO-VO/)

[생성자 주입을 사용해야 하는 이유](https://dev-splin.github.io/spring/Spring-Constructor-Injection/)

[Lombok의 사용법 및 주의점](https://dev-splin.github.io/spring/Spring-Lombok/)

[MapStruct의 사용법 및 ModelMapper와의 비교](https://dev-splin.github.io/spring/Spring-ModelMapper,MapStruct/)

[ResponseEntity를 사용해야 하는 이유](https://dev-splin.github.io/spring/Spring-ResponseEntity/)



### 직렬화, 역직렬화

[Jackson Annotation 사용법](https://dev-splin.github.io/spring/Spring-Jackson-Annotation/)

> 직렬화 : 자바 객체를 전송가능한 Json형태로 만들어주는 것을 의미
>
> 역직렬화 : 직렬화한 Json 데이터들을 자바 객체로 변환하는 것을 의미



### 유효성 검사

[유효성 검사(Validation) 방법 및 Custom Annotation](https://dev-splin.github.io/spring/Spring-Validation/)



### 예외처리

[Exception 전략](https://cheese10yun.github.io/spring-guide-exception/)



### Security

[Spring Security란? (설정 포함)](https://dev-splin.github.io/spring/Spring-Security/)

[Spring Security를 이용한 로그인 및 로그아웃](https://dev-splin.github.io/spring/Spring-Security-Login-And-Logout/)

[Spring Security에서 DB를 이용한 로그인 및 회원가입](https://dev-splin.github.io/spring/Spring-Security-UsedDB-Login-And-Register/)



### 테스트

[MockMvc를 이용한 단위 테스트](https://dev-splin.github.io/spring/Spring-MockMVC-WebAPI-Test/)

[Given-When-Then 패턴](https://dev-splin.github.io/spring/Spring-Given-When-Then-Pattern/)

[Web API 문서화를 위한 Swagger](https://dev-splin.github.io/spring/Web-Swagger/)

[Spring MVC에서 Swagger 사용하기](https://dev-splin.github.io/spring/Spring-Swagger/)



### 기타

[Cookie / Session](https://dev-splin.github.io/spring/Spring-Cookie,Session/)





---

여기까지 했으면 안에 내용을 구현해서 멋진 프로젝트를 만듭니다! 추후에 알게되는 것이 있으면 계속 추가하겠습니다!

