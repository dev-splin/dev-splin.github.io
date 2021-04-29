---
title: "Spring : DI(Dependency Injection) 예제"
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

# DI(Dependency Injection) 예제

Car와 Engine이라는 클래스 2개를 생성합니다.



### Car

```java
package kr.or.connect.diexam01;

public class Car {
	private Engine v8;
	
	public Car() {
		System.out.println("Car 생성자");
	}
	
	public void setEngine(Engine e) {
		this.v8 = e;
	}
	
	public void run() {
		System.out.println("엔진을 이용하여 달립니다.");
		v8.exec();
	}
```



### Engine

```java
package kr.or.connect.diexam01;

public class Engine {
	public Engine() {
		System.out.println("Engine 생성자");
	}
	
	public void exec() {
		System.out.println("엔진이 동작합니다.");
	}
}
```



---

위의 Car 클래스가 제대로 동작하도록 하려면 보통 다음과 같은 코드가 작성되어야 합니다.

```java
Engine e = new Engine();
Car c = new Car();
c.setEngine( e );
c.run();
```



1,2 번째 줄을 Spring 컨테이너에게 맡기기 위해 설정파일에 다음과 같은 코드를 입력합니다.

```xml
<bean id="e" class="kr.or.connect.diexam01.Engine"></bean>
<bean id="car" class="kr.or.connect.diexam01.Car">
	<property name="engine" ref="e"></property>
</bean>
```

egine이라는 `property(getter or setter)`를 사용해서 e(bean id)를 가져올 수 있습니다. 하지만 bean태그 안에서는 모두 값을 설정하는 것이기 때문에 여기서 `property`는 `setter`가 됩니다.



위에 XML 설정은 다음과 같은 의미를 가집니다.

```java
Engine e = new Engine();
Car c = new Car();
c.setEngine( e );
```



###  ApplicationContextExam02.java

```java
package kr.or.connect.diexam01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class ApplicationContextExam02 {

	public static void main(String[] args) {
		ApplicationContext ac = new ClassPathXmlApplicationContext( 
				"classpath:applicationContext.xml"); 

		Car car = (Car)ac.getBean("car");
		car.run();
	}
```

`applicationContext`에서 `Engine`객체를 `Car`에다 이미 주입시켜주었기 때문에 위의 실행과정과 똑같이 작동하는 것을 볼 수 있습니다. 이렇게 **어떤 객체에게 객체를 주입하는 것을 DI**라고 합니다.

DI를 사용했을 때 장점은 `Engine`이라는 클래스가 실행 클래스에서 등장하지 않기 때문에 사용자는 `Car`라는 클래스만 알고 있으면 됩니다. 만약 나중에 `Car`를 상속받는 `Bus`라는 클래스가 생성되도록 xml파일만 바꿔주고 `Bus`가 상속받고 있는 `Engine` 클래스도 `Electric Engine`으로 주입받도록 바꿔준다면 실행 클래스의 코드는 하나도 바뀌지 않고 전기 버스가 동작하는 코드가 될 수도 있습니다.

이렇게 xml파일을 이용한 방법말고 Annotation이라는 것을 이용하는 방법도 있습니다.

[Annotation을 이용한 DI예제](https://dev-splin.github.io/spring/Spring-Annotation%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-DI-%EC%98%88%EC%A0%9C/)

---

참고 : https://www.boostcourse.org/web326/lecture/258526/?isDesc=false

