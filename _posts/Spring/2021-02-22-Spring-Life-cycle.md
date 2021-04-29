---
title: "Spring : 생명주기(Life Cycle)"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Life Cycle"
toc: true
toc_sticky: true
toc_label: 목차
---

# 생명주기(Life Cycle)



## 스프링 컨테이너 생명주기

1. GenericXmlApplicationContext를 이용한 스프링 컨테이너 초기화
1. getBean() 을 이용한 빈(Bean)객체 이용
1. close를 이용한 스피링 컨테이너 종료



### 빈(Bean)객체 생명주기
- 빈(Bean)객체의 생명주기는 스프링 컨테이너의 생명주기와 같이 한다.
![빈생명주기](https://user-images.githubusercontent.com/58713853/74933549-921c3000-5427-11ea-84b4-c7a425880a77.PNG)



#### afterPropertiesSet(), destroy()

- InitializiongBean 인터페이스의 afterPropertiesSet()을 이용해 생성시점에 특정한 기능을 구현 할 수 있다.
- DisposableBean 인터페이스의 destroy()를 이용해 소멸시점에 특정한 특정한 기능을 구현 할 수 있다.



#### init-method, destroy-method

- 스프링 설정 파일에 init-method로 afterPropertiesSet()와 같이 특정한 기능을 구현할 수 있다.
- 스프링 설정 파일에 destroy-method로 destroy()와 같이 특정한 기능을 구현할 수 있다.



##### 예제

- 도서관리프로그램에서 BookRegister.java라는 객체, appCtx.xml 스프링 설정 파일이 있다고 가정
```java
// BookRegister.java
// afterPropertiesSet(), destroy() 사용
public class BookRegister implements InitializiongBean, DisposableBean{
// InitializiongBean, DisposableBean 인터페이스를 상속 받는다.
  
  @Override
  public void afterPropertiesSet() throws Exception { // InitializiongBean의 메소드를 오버라이딩 한다.
      System.out.println("bean 객체 생성");
  }
  
  @Override
  public void destroy() throws Exception { // DisposableBean 메소드를 오버라이딩 한다.
      System.out.println("bean 객체 소멸");
  }
}
```

```java
// appCtx.xml
// init-method, destroy-method 사용
<bean id="bookRegister" class="com.brms.book.service.BookRegister" 
init-method="initMethod" destroy-method="destroyMethod"/> 
// 스프링 설정파일에서 init-method, destroy-method로 쓰일 메소드를 설정해준다.
```

```java
public class BookRegister { // 따로 상속받지 않아도 된다.
 
  public void initMethod() {
      System.out.println("bean 객체 생성");
  }
  
  public void destroyMethod() {
      System.out.println("bean 객체 소멸");
  }
}
```


