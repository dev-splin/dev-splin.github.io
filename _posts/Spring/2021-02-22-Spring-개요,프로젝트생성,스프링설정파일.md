---
title: "Spring : 개요, 프로젝트 생성, 스프링설정파일"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
toc: true
toc_sticky: true
toc_label: 목차
---

## 스프링 개요

- 스프링 프레임워크는 주요기능으로 DI, AOP, MVC, JDBC 등을 제공한다.
  - 스프링 프레임워크에서 등장한 용어가 아니고 프로그래밍에서 이떠한 구조를 만드는 방법들의 하나이다. (앞으로 천천히 알아갈 것이다.)
- MVC와 JDBC는 JSP에서 많이 등장한 용어이다.



### 스프링 프레임워크에서 제공하고 있는 모듈
- 모듈은 코드로 구성되어있는 라이브러리로 볼 수 있다.
- 개발 프로젝트의 환경설정 부분에 XML 파일등을 이용해서 개발자가 필요한 모듈을 명시만해주면 해당 모듈을 자동으로 다운받는다.
![Spring_Moudule](https://user-images.githubusercontent.com/58713853/74433575-0002ad80-4ea4-11ea-98ec-28df5698a286.PNG)



### 스프링 컨테이너 (IoC)

- 스프링에서 객체가 담겨져있는 큰 그릇을 스프링 컨테이너라고 한다.
- 스프링에서 객체를 생성하고 조립하는 것을 컨테이너(Container)라고 한다.
- 컨테이너를 통해 생성된 객체를 빈(Bean)이라고 부른다.
![컨테이너](https://user-images.githubusercontent.com/58713853/74433966-ce3e1680-4ea4-11ea-852f-08f10cd23655.PNG)



## 스프링 프로젝트 생성

- 스프링 프레임워크는 Maven Project를 이용한다.
  - Group Id : 전체적인 그룹의 아이디 ex) 전체 지하철 노선도
  - Artifact Id : 개별적인 프로젝트의 아이디 ex) 4호선 노선도
- 스프링 프로젝트를 생성하면 src/main안에 java와 resources 폴더로 이루어져 있다.
  - java : 실제로 자바언어를 이용해서 기능구현을 하는 부분 ( 자바파일 관리)
  - resources : 기능구현을 해나아가는 데 있어서 보조적인 역할의 파일을 모아놓는 곳 (스프링 설정 파일 (XML) 또는 프로퍼티 파일 관리)



### pom.xml

- 필요한 모듈을 가져오기 위한 파일
- groupId와 artifactId의 version에 해당하는 모듈을 가져올 수 있다.
- build태그에는 build에 필요한 기능들을 명시해 놓는다.
- pom.xml에 의해서 필요한 라이브러리만 다운로드해서 사용한다.



### 스프링 설정 파일 (XML) 사용법

- pom.xml과는 다른 resources에서 관리하는 보조적인 역할의 파일이다. 
- new를 이용해 객체를 생성하지 않고 스프링컨테이너에 xml에 명령어를 이용하여 객체를 생성한다.(메모리에 로딩이 된다.)
  - 메모리에서 특별하게 관리되는 스프링 컨테이너에 로딩이 된다.
  

```java
// resources폴더에 applicationContext.xml 이라는 파일을 만들었다고 가정
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
 		http://www.springframework.org/schema/beans/spring-beans.xsd"> 
// 스프링 설정 파일을 사용하기 위한 기본적인 네임스페이스 스키마로 보통 복사해서 사용한다.
		
// <bean id="사용할 아이디" class="패키지.클래스명(풀네임)" />
<bean id="twalk" class="lec03Pjt001.TransportationWalk" />  // 객체를 컨테이너에 로딩 한다.(객체를 생성)
```

```java
public static void main(String[] args) {
    // 기존의 객체 생성 방식
    //		TransportationWalk transportationWalk = new TransportationWalk(); 
    //		transportationWalk.move();
	}
```

```java
public static void main(String[] args) {
		
    // 스프링 객체 생성 방식
		GenericXmlApplicationContext ctx = 
				new GenericXmlApplicationContext("classpath:applicationContext.xml"); // 컨테이너를 가져온다.
		
		TransportationWalk transportationWalk = ctx.getBean("tWalk", TransportationWalk.class); // id를 이용해서 컨테이너의 특정 객체를 가져온다.
		transportationWalk.move();
		
		ctx.close();  // 자바에서는 외부리소스를 쓰면 반환을 해주어야한다.
	}
```



#### 스프링 설정 파일 분리

- 하나의 스프링 파일에 모든 객체들을 명시하게 되면 파일의 길이가 너무 길어진다.
	- 기능별로 분류하는 것이 좋다.

```java
// resources폴더에 applicationContext.xml 이라는 파일에 기능 부분, DB부분, 기타 정보 부분이 있다고 가정
// 이 파일을 기능/DB/기타정보 로 나누어서 appCtx1.xml, appCtx2.xml, appCtx3.xml로 분리 해준다.


public static void main(String[] args) {
	String[] appCtxs = {"classpath:appCtx1.xml", "classpath:appCtx2.xml", "classpath:appCtx3,xml"};	
	// 배열을 이용해 설정파일 이름을 저장한다.
	GenericXmlApplicationContext ctx = new GenericXmlApplicationContext(appCtxs) // 그 배열을 인자로 넣어준다.

	// 다른 부분은 동일
	// appCtx1.xml에 appCtx2와 appCtx3을 import해서 appCtx1만 가져와 사용할 수도 있지만 위의 방법이 더 많이 쓰인다고 한다.
}
```
