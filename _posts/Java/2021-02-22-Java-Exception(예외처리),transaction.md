---
title: "Java : Exception(예외처리), Transaction"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : 예외처리"
toc: true
toc_sticky: true
toc_label: 목차
---

# 예외처리 (Exception)
- 예측가능한 오류를 오류 발생없이 처리하기 위한 방법
- 구문 오류 같은 것이 아닌 0으로 어떤 숫자를 나누는 것 같은 경우의 오류를 말한다.
  
  - ex) FileNotFoundException, ArithmeticException 등등...
  
  

## try / catch / finally
- try, catch는 예외처리를 위한 기본 구조이다.
- finally는 어떤 예외가 발생하더라도 반드시 실행되어야 하는 부분이 있을 때 사용한다.

```java
// test() 함수를 꼭 실행시켜야하는 함수라고 가정한다.
int a;

try {
  a = 4 / 0;  // 예외가 발생한 문장
} catch(ArithmeticException e) { // 어떤 숫자를 0으로 나눴을 때의 오류
  a = -1; // 예외가 발생했을 때 처리되는 부분
} finally {
  test(); // 예외가 발생하던 발생하지 않던 꼭 실행되는 구문
}
```



## throw / throws

- throw new : 예외를 발생시키는 키워드
- thorws 예외를 던지는 키워드, 현재상황에서 처리하는 것이 아니라 메소드를 호출하는 곳에서 처리하도록 한다.
  - 호출 하는 부분에서 try/catch 를 해주어야 한다.
- thorw new는 반드시 thorws를 해줘야 하지만 thorws는 반드시 thorw new를 해줄 필요는 없다.

```java
public void divide(int a, int b) throws ArithmeticException { 
// thorw new 부분이 있으면 반드시 필요 하지만 throw new 부분이 없어도 오류나진 않는다.
  if (b==0) {
    throw new ArithmeticException();  // b가 0이면 ArithmeticException 예외를 발생시킨다. 
  }
}

public static void main(String[] args) {
try { // throws로 던졌기 때문에 호출하는 부분에서 try/catch를 해주어야 한다.
  calc.divide(10, 0);
}catch (ArithmeticException e) {
  System.out.println("0으로 나누면 안되요오~~");
}
```



## User Exception (사용자 예외처리)

- 발생할 예외를 개발자가 스스로 만들어서 처리를 하는 예외
- RuntimeException이나 Exception을 상속받아 작성한다.



### Exception
- 프로그램 작성 시 발생하는 예외
- 이미 예측가능한 예외를 작성할 때 사용한다.
- Checked Exception 이라고도 한다.



### RuntimeException

- 실행 시 발생하는 예외
- 사용자가 사용할 때 발생 할수도 발생 안 할수도 있는 경우에 사용한다.
- Unchecked Exception 이라고도 한다.



#### 사용 방법

```java
public class ZeroException extends Exception {	// 실행 되기 전 에러가 발생한다.
	public ZeroException() {
		this("0으로 나누지 말라고");  // 예외가 발생할 시 Console 창에서 해당 텍스트를 볼 수 있다.
	}

	public ZeroException(String msg) {
		super(msg);
	}
}

public void divide(int a, int b) throws ZeroException { 
  if (b==0) {
    throw new ZeroException(); 
  }
}
```



## Transaction

- 트랜잭션은 하나의 작업 단위를 뜻한다.
- 트랜잭션과 예외처리는 매우 밀접한 관련이 있다.



### 상품발송 예제
- 포장, 영수증발행, 발송이라는 3가지 작업이 있는 트랜잭션을 가정해본다.
- 이 3가지 일들 중 하나라도 실패하면 3가지 모두 취소하고 상품발송 전 상태로 되돌리고 싶을 것이다.
	- 이렇게 모두 취소하는 행위를 보통 롤백(Rollback)이라고 말한다.

```java
// 트랜잭션 처리를 잘한 예
상품발송() {
    try {
        포장();
        영수증발행();
        발송();
    }catch(예외) {
       모두취소();
    }
}

포장() throws 예외 {
   ...
}

영수증발행() throws 예외 {
   ...
}

발송() throws 예외 {
   ...
}

// 못한 예
상품발송() {
    포장();
    영수증발행();
    발송();
}

포장(){
    try {
       ...
    }catch(예외) {
       포장취소();
    }
}

영수증발행() {
    try {
       ...
    }catch(예외) {
       영수증발행취소();
    }
}

발송() {
    try {
       ...
    }catch(예외) {
       발송취소();
    }
} 
```

- 잘한 예에서는 try에 함수를 호출할 때 함수중 하나라도 오류가 발생하면 예외처리를 하게 된다.
- 못한 예에서는 try/catch를 함수안에 구현하였기 때문에 포장은 예외처리를 하고있는데 발송이 되는 등 뒤죽박죽한 상황이 연출될 수 있다.



---

참고 : https://wikidocs.net/229
