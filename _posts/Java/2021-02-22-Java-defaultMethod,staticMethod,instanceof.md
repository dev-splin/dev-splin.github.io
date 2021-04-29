---
title: "Java : static 메소드, default 메소드, instanceof 연산자"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true
toc_label: 목차
---

# static 메소드와 default 메소드



## static 메소드

- 자식 객체를 구현하지 않고 바로 사용할 수 있다.



## default 메소드

- Java8에서 추가
- 인터페이스에서 직접 사용할 수 없고 자식객체를 구현하고 사용해야 한다.



### static 메소드와 default 메소드 사용방법

```java
public interface IA { 
  void printA();
  
  default void printdefault() {
  	System.out.println("디폴트 메소드 입니다.");
  }
  
  static void printstatic() {
  	System.out.println("스태틱 메소드 입니다.");
  }	
}

public class A implements IA{
	// 오바라이딩 했다고 가정
}

// 메인이라고 가정
IA.printdefault(); // 사용불가능
IA.printstatic(); // 사용가능

IA ia = new A();
ia.printdefault(); // 사용가능
ia.printstatic(); // 사용불가능
```



### instanceof 연산자

- 객체가 어떤 인스턴스를 가지고 있는지 판별하는 연산자.
- 인스턴스 비교를 할 때 부모타입의 인스턴스를 먼저 비교하면 안되고 자식의 인스턴스를 비교하고 마지막에 부모의 인스턴스를 비교해야 한다.
  - 그렇지 않으면 자식 타입으로 비교가 되지 않는다.

```java
public class People{
}

public class Student extends People{
}

public class Teacher extends People{
}

// 메인이라 가정
People p = new People();
		
Student s = new Student();
p=s;
		
if (p instanceof People) {
	System.out.println("직원입니다.");	// People에서 이프문이 완료되기 때문에 Student인지 Teacher인지 더 확실한 체크를 할 수 없다.
}else if(p instanceof Student) {
	System.out.println("학생입니다.");
}else if (p instanceof Teacher) {
	System.out.println("선생 입니다.");
}  
```

