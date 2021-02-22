---
title: "Java : pbv vs pbr, 접근제한자, 지역변수, 멤버변수"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true
toc_label: 목차
---

## pbv(pass by value) vs pbr(pass by reference)



### pbv(pass by value)

- 기본타입이라고 하며, value를 주고 value를 받는다.
- pass by value
- assign by value
- immutable

```java
int a = 10;
int b = a;
c = a + 20;

System.out.println(a); // a의 값 10이 변하지 않는다.
// a 값이 변하려면 재할당을 해야한다.
```



### pbr(pass by reference)

- 참조타입이라고 하며, reference를 주고 reference를 받는다.
- pass by reference
- assign by reference
- mutable

```java
int[] c = {1,2,3,4,5};
int[] d = new int[5];
d = c;
c[2] = 100;

System.out.println(d[2]); // d가 c를 참조하기 때문에 100이 나온다.
```



## 접근제한자

- default(접근제한자를 입력하지 않으면 default로 봄) : 같은 패키지 내에서만 접근가능
- private : 같은 클래스 내에서만 접근가능 (이 접근제한자로 클래스 생성불가)
- protected : 상속관계에서는 어디서든 접근가능, 상속하지 않으면 같은 패키지내에서만 접근가능 (이 접근제한자로 클래스 생성불가)
- public : 모든 곳에서 접근 가능



## 지역변수/멤버변수
- 지역변수는 초기값을 설정해 주어야 한다.
- 멤버변수는 초기값을 설정해 주지 않으면 숫자는 0 문자는 null로 설정된다.



## String class(문자열 클래스)
- 참조타입이지만 기본타입의 성질을 갖고 있는 클래스



### String class 특징
- immutable : 값을 재할당 하기 전까지는 값이 변하지 않는다.
- concatenation : String 타입을 만나는 순간 String 타입이 된다.



#### concatenation 예제
```java
System.out.println(1+2+3+"자바"); // 6자바
System.out.println("자바"+1+2);   // 자바12
System.out.println("자바"+1+(2+3)); // 자바15    
System.out.println(1+"자바"+2+3);   // 1자바23
System.out.println((1+2)+3+"자바"); // 6자바
```
