---
title: "Spring : Annotation을 이용한 DI(Dependency Injection) 예제"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : DI(Dependency Injection)"
toc: true
toc_sticky: true
toc_label: 목차
---

# Annotation을 이용한 DI(Dependency Injection) 예제



## Java config를 이용한 설정을 위한 어노테이션

- **@Configuration**

  스프링 설정 클래스를 선언하는 어노테이션

- **@Bean**

  bean을 정의하는 어노테이션

- **@ComponentScan**

  `@Controller`, `@Service`, `@Repository`, `@Component` 어노테이션이 붙은 클래스를 찾아 컨테이너에 등록

- **@Component**

  컴포넌트 스캔의 대상이 되는 애노테이션 중 하나로써 주로 유틸, 기타 지원 클래스에 붙이는 어노테이션

- **@Autowired**

  주입 대상이되는 bean을 컨테이너에 찾아 주입하는 어노테이션

  

## Java Config를 이용해 설정하기



### ApplicationConfig.java

```java
package kr.or.connect.diexam01;
import org.springframework.context.annotation.*;

@Configuration
public class ApplicationConfig {
	@Bean
	public Car car(Engine e) {
		Car c = new Car();
		c.setEngine(e);
		return c;
	}
	
	@Bean
	public Engine engine() {
		return new Engine();
	}
}
```

`@Configuration` 은 스프링 설정 클래스라는 의미를 가집니다. JavaConfig로 설정을 할 클래스 위에는 `@Configuration`가 붙어 있어야 합니다.

`ApplicationContext`중에서 `AnnotationConfigApplicationContext`는 JavaConfig클래스를 읽어들여 **IoC와 DI를 적용**하게 됩니다. 이때, 설정파일 중에 `@Bean`이 붙어 있는 메소드들을 `AnnotationConfigApplicationContext`는 **자동으로 실행하여 그 결과로 리턴하는 객체들을 기본적으로 싱글턴으로 관리**를 하게 됩니다.

`ApplicationContext`는 파라미터를 **받아들이지 않는 bean 생성 메서드(Engine)를 먼저 다 실행**해서 반환받은 객체를 관리합니다. 그리고 **파라미터에 생성된 객체들과 같은 타입이 있는 객체(Car)가 있을 경우에 파라미터로 전달해서 객체를 생성**합니다.



### ApplicationContextExam03.java

```java
package kr.or.connect.diexam01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ApplicationContextExam03 {

	public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);
		   
		Car car = ac.getBean(Car.class);
		car.run();
		
	}
}
```

xml을 이용했을 때는 `Bean Factory`를 생성할 때  `ClassPathXmlApplicationContext`를 이용했다면 자바 Config를 이용할 때는 `AnnotationConfigApplicationContext`를 이용하면 됩니다.

`getBean()`에 "car" 이렇게 메서드 이름으로 Bean을 불러올 경우에는 오타가 나기도 쉽고 형 변환하는 것도 번거롭기 때문에 요청하는 클래스 타입을 넣어주면 **메서드 이름과 관계없이 실행**되는 것을 볼 수 있습니다.

---



### ApplicationConfig2.java

```java
package kr.or.connect.diexam01;
import org.springframework.context.annotation.*;

@Configuration
@ComponentScan("kr.or.connect.diexam01")
public class ApplicationConfig2 {
}
```

기존 JavaConfig에서 **빈을 생성하는 메소드를 모두 제거**했습니다. 단, `@Configuration`아래에 `@ComponentScan`이라는 어노테이션을 추가했습니다.

`@ComponentScan`어노테이션은 파라미터로 들어온 패키지 이하에서 `@Controller`,` @Service`, `@Repository`, `@Component` 어노테이션이 붙어 있는 클래스를 찾아 메모리에 몽땅 올려줍니다.

`@ComponentScan`은 굉장히 광범위하기 때문에 수행할 때는 반드시 패키지명을 알려주어야 합니다.



기존의 Car클래스와 Engine클래스 위에 @Component를 붙이도록 하겠습니다.

### Engine.java

```java
package kr.or.connect.diexam01;

import org.springframework.stereotype.Component;

@Component
public class Engine {
	public Engine() {
		System.out.println("Engine 생성자");
	}
	
	public void exec() {
		System.out.println("엔진이 동작합니다.");
	}
}
```



### Car.java

```java
package kr.or.connect.diexam01;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class Car {
	@Autowired
	private Engine v8;
	
	public Car() {
		System.out.println("Car 생성자");
	}
	
	public void run() {
		System.out.println("엔진을 이용하여 달립니다.");
		v8.exec();
	}
}
```

수정된 JavaConfig를 읽어들여 실행하는 클래스를 보도록 하겠습니다.

 

### ApplicationContextExam04.java

```java
package kr.or.connect.diexam01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ApplicationContextExam04 {

	public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig2.class);
		   
		Car car = ac.getBean(Car.class);
		car.run();
		
	}
}
```

Spring에서 사용하기에 알맞게 `@Controller`, `@Service`, `@Repository`, `@Component` 어노테이션이 붙어 있는 객체들은 `@ComponentScan`을 이용해서 읽어들여 메모리에 올리고 DI를 주입하도록 하고, 이러한 어노테이션이 붙어 있지 않은 객체는 @Bean어노테이션을 이용하여 직접 생성해주는 방식으로 클래스들을 관리하면 편리합니다.

하지만 ComponentScan만 다 하면 굉장히 편한데 bean은 언제 쓸까요? 

나중에 Spring JDBC를 쓴다든가 다른 라이브러리가 갖고있는 개체들을 사용할 때는 `@ComponentScan`이 인식할 수 있는 어노테이션을 직접 붙일 수 없기 때문에 bean으로 등록하는 방법을 이용해서 사용할 수 있습니다.



---

참고 : https://www.boostcourse.org/web326/lecture/58972?isDesc=false

