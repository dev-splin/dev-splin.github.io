---
title: "Java : 추상클래스, 인터페이스"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true
toc_label: 목차
---

## 추상 클래스 (abstract class)

- 공통적으로 사용 되는 메소드를 상속받은 곳에서 다시 구현해야하는 추상메소드로 선언해 1개 이상 가지고 있는 클래스이다.
- body( { } )가 없는 메소드를 추상메소드라고 한다.
- 상속을 강요하고 반드시 구현해야 한다.
  - 구현 하지 않고 오류가 발생되지 않게 하려면 상속받은 클래스도 추상클래스로 만들어야 한다.
- 추상 메소드, 클래스 모두 abstract 키워드를 붙인다.
- 추상 클래스는 추상클래스 생성자로 생성할 수 없다.

```java
public abstract class AbstractA { // 추상 클래스 선언
  int A;
  public abstract void printA();  // 추상 메소드를 가지고 있다.
}

public abstract class B extends AbstractA{  // 상속 받은 클래스도 추상클래스로 만들어 오류가 발생하지 않는다.
  int B;
  public abstract void printB();
}

public class C extends B{ // B를 상속받으면 B가 상속받은 AbstractA, B의 추상클래스 모두 구현해주어야 한다.

  int C;
  
  public void printC();
  
	@Override
	public void printB() {
		
	}

	@Override
	public void printA() {
		
	}

}
```



## interface(인터페이스)

- 추상 메소드로만 이루어진 클래스 (인터페이스의 추상메소드는 abstract를 붙이지 않는다.)
  - 추상 클래스와의 차이 : 추상 클래스는 추상 메소드 외에 다양한 클래스, 변수들을 가질 수 있다.
- 인터페이스의 추상 메소드를 구현하지 못하면 자식 클래스는 추상 클래스가 된다.
- 기본 접근제한자는 보통 public을 사용한다.
- 변수는 자동으로 상수가 된다.
  - int A 를 선언할 때 원래의 멤버필드라면 0으로 디폴트 값이 들어가지만 interface는 선언할 수 없다.
  - int A = 10; 처럼 상수값을 넣어주어야한다.
- 인터페이스는 여러개의 인터페이스를 상속할 수 있다.
- 인터페이스끼리 상속할때는 extends 키워드를 사용한다.
- 인터페이스를 상속하는 키워드는 implements이다.
- 일반 클래스와 인터페이스가 있을 경우에는 일반 클래스가 우선이다.
- 인터페이스도 인터페이스 생성자로 생성할 수 없다.


```java
// 인터페이스 선언 / 상속
public interface IA { // 인터페이스 선언
  void printA();
}

public interface IB extends IA { // 인터페이스 선언, 인터페이스끼리 상속
  void printB();
}

public interface IC extends IA,IB { // 여러개의 인터페이스 상속
  void printC();
}

public class A {  // 클래스 선언 

}

public class B extends A implements IA{  // 일반클래스와 인터페이스가 있을 경우 일반 클래스 우선선언
  @override
  public void printA(){ // IA의 추상메소드 구현
  
  }
}

public class C implements IC {  // 인터페이스 끼리 상속 받은 IC를 상속한다.
 // IC가 IA,IB를 상속받기 때문에 IC를 상속받으면 IA,IB의 추상 메소드도 구현해주어야한다.
  @override
  public void printA(){
  
  }
  
  @override
  public void printB(){
  
  }
  
  @override
  public void printC(){
  
  }
}

```