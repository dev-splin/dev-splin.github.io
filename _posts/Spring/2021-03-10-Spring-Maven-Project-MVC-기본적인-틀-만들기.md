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



## pom.xml 설정하기

다양한 라이브러리를 가져와서 사용할 수 있지만 일단은 제가 사용했던 pom.xml을 가져오겠습니다.

설정을 추가하면 `프로젝트 오른쪽 클릭 -> Maven -> Update Project` 를 반드시 해주야합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>kr.or.connect</groupId>
  <artifactId>selfmvcguestbook</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>selfmvcguestbook Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <!-- pom.xml 내에서 상수처럼 사용할 수 있습니다. -->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <spring.version>4.3.5.RELEASE</spring.version>
    <jackson2.version>2.8.6</jackson2.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>${spring.version}</version>
	</dependency>
	
	<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-webmvc</artifactId>
	    <version>${spring.version}</version>
	</dependency>
	
    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
	<dependency>
	    <groupId>javax.servlet</groupId>
	    <artifactId>javax.servlet-api</artifactId>
	    <version>3.1.0</version>
	    <scope>provided</scope>
	</dependency>
	
	<!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
	<dependency>
	    <groupId>javax.servlet.jsp</groupId>
	    <artifactId>javax.servlet.jsp-api</artifactId>
	    <version>2.3.1</version>
	    <scope>provided</scope>
	</dependency>
		
    
    <!-- https://mvnrepository.com/artifact/jstl/jstl -->
	<dependency>
	    <groupId>jstl</groupId>
	    <artifactId>jstl</artifactId>
	    <version>1.2</version>
	</dependency>
	
	<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
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
	
	<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
	<dependency>
	    <groupId>com.fasterxml.jackson.core</groupId>
	    <artifactId>jackson-databind</artifactId>
	    <version>${jackson2.version}</version>
	</dependency>
	
	<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.datatype/jackson-datatype-jdk8 -->
	<dependency>
	    <groupId>com.fasterxml.jackson.datatype</groupId>
	    <artifactId>jackson-datatype-jdk8</artifactId>
	    <version>${jackson2.version}</version>
	</dependency>
	
  </dependencies>

  <build>
    <finalName>selfmvcguestbook</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
      
      	
      	<!-- maven 버전 설정을 위한 플러그인 입니다.-->
      	<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>3.6.1</version>
			<configuration>
				<source>1.8</source>
				<target>1.8</target>
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
    </pluginManagement>
  </build>
</project>
```



## Servlet 버전 설정하기

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



## DispatcherServlet을 FrontController로 설정하기

설명이 필요하기 때문에 [링크](https://dev-splin.github.io/spring/Spring-MVC-%EC%98%88%EC%A0%9C(DispatcherServlet%EC%9D%84-FrontController%EB%A1%9C-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)/)로 대체하겠습니다. 



## Controller 작성하기

간단한 컨트롤러를 만들어서 테스트 해봅니다.

[Controller작성하기](https://dev-splin.github.io/spring/Spring-MVC-%EC%98%88%EC%A0%9C(Controller%EC%9E%91%EC%84%B1)/)



## JDBC 연결하기

[JDBC 연결하기](https://dev-splin.github.io/spring/Spring-Spring-JDBC-Exam/)

[Layered Architecture](https://dev-splin.github.io/spring/Spring-Layered-Architecture/)를 보면 `DispatcherServlet`과  나머지 부분을 분리하여 사용하는게 좋다고 했습니다.

`DispatcherServlet`을 경우에 따라서 2개 이상 설정할 수 있는데 이 경우에는 각각의 `DispathcerServlet`의 `ApplicationContext`가 각각 독립적이기 때문에 각각의 설정 파일에서 생성한 빈을 서로 사용할 수 없습니다.

위의 경우와 같이 동시에 필요한 빈은 `ContextLoaderListener`를 사용함으로써 공통으로 사용하게 할 수 있습니다.

```xml
<!-- 공통으로 사용할 Config 파일을 가져옵니다. -->
<context-param>
    	<param-name>contextConfigLocation</param-name>
    	<param-value>kr.or.connect.selfmvcguestbook.config.ApplicationConfig</param-value>
</context-param>
<!-- listener를 선언해줍니다. -->
<listener>
    <listener-class>
    		org.springframework.web.context.ContextLoaderListener
    </listener-class>
</listener>
```



여기까지 했으면 안에 내용을 구현해서 멋진 프로젝트를 만듭니다! 추후에 알게되는 것이 있으면 계속 추가하겠습니다!