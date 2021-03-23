---
title: "Java : 함수형 프로그래밍(람다식), 제네릭 타입"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Functional Programming(Lambda)"
toc: true
toc_sticky: true
toc_label: 목차
---

## 함수형 프로그래밍 (functional programming)

- y = f(x) 형태의 함수로 구성된 프로그래밍 기법
- 데이터를 파라미터로 전달하고 결과를 받는 코드로 구성
- 객체지향 프로그래밍보다 효율적인 경우
  - 대용량 데이터 처리에 유리하다.
    - 데이터를 포장해서 객체를 생성하는 것 보다는 데이터를 바로 처리하는 것이 속도에 유리하다.
    - 멀티코어 CPU에서 데이터를 병렬 처리하고 취합할 때 객체보다는 함수가 유리함.
  - 이벤트 지향 프로그래밍
    - 반복적인 이벤트 처리는 핸들러 객체보다는 핸들러 함수가 적합함.
- 현대 프로그래밍 기법
  - 객체지향 프로그래밍 + 함수형 프로그래밍
  
  
### Java8에서부터 함수형 프로그래밍 지원

- 람다식(Lambda Expressions)을 언어 차원에서 제공
- 람다 계산법에서 사용된 식을 프로그래밍 언어에 접목
- 익명함수(Anonymous function)을 생성하기 위한 식
- (타입 매개변수) -> { 실행문;}
- 자바에서 람다식을 수용한 이유
  - 코드가 매우 간결하다
  - 컬렉션 요소(대용량 데이터)를 필터링 또는 매핑해서 쉽게 집계할 수 있다.
  
  
## 함수형 인터페이스 (functional interface)
- 모든 인터페이스는 람다식의 타겟 타입이 될 수 없다.
  - 람다식은 하나의 메소드를 정의하기 때문
  - 하나의 추상메소드만 선언된 인터페이스만 타겟 타입이 될 수 있음
- @FunctionalInterface 어노테이션(주석)을 기재한다.



### 타겟 타입
- 람다식이 대입되는 인터페이스를 말한다.
- 익명 구현 객체를 만들 때 사용할 인터페이스이다.



### @FunctionalInterface 어노테이션(주석)
- 하나의 추상 메소드만을 가지는지 컴파일러가 체크 하도록 함
- 명시적으로 지정하지 않더라도 추상 메소드가 한개라면 함수형 인터페이스라고 보면된다.
- 하지만 이 주석을 사용한다면 추상 메소드가 2개 이상 있을경우 컴파일 타임 에러가 난다.



### 람다식 (Lambda Expressions)
- Java는 람다식을 함수적 인터페이스의 익명 구현 객체로 취급
- 함수적 인터페이스 : 한개의 메소드를 가지고 있는 인터페이스를 말한다.
- 람다식 -> 파라미터를 가진 코드 블록 -> 익명 객체 구현
- (타입 + 파라미터 변수) -> { 실행문; }

```java
@FunctionalInterface
public interface MyFunctionalInterface {
	public void print();
//	public void print2(); // 만약 추상메소드 2개를 만들게 되면 에러가 난다.
}

public static void main(String[] args) {
  MyFunctionalInterface mfi;
  
  mfi = () -> { System.out.println("메소드 호출!!"); } // 파라미터가 없기 때문에 기재하지 않는다.
  
  // 객체지향 방식 (같은 결과가 나온다.)
  mfi = new MyFunctionalInterface() {
			@Override
			public void print() {
				System.out.println("메소드 호출!!");
			}
		};
  
  // 이 아래는 int a라는 파라미터가 있다고 가정하에 작성한다.
  mfi = (a) -> { System.out.println(a); } // 파라미터 타입은 런타임시에 대입값에 따라 자동으로 인식하기 때문에 생략 가능
  mfi = a -> { System.out.println(a); } // 하나의 파라미터만 있을 경우에는 괄호() 생략 가능 (단, 파라미터 변수가 없으면 생략할 수 없다.)
  mfi = a -> System.out.println(a); // 하나의 실행문만 있다면 중괄호{} 생략 가능 (단, 실행문이 두줄이상이 되면 생략할 수 없다.)
  mfi = a -> return a; // 리턴값이 있는 경우, return문을 사용
  mfi = a -> a; // 중괄호{}에 return문만 있을 경우, return문 생략 가능
}
```



