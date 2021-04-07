---
title: "Java : String / StringBuffer / StringBuilder"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : String / StringBuffer / StringBuilde"
toc: true
toc_sticky: true
toc_label: 목차
---

# String / StringBuffer / StringBuilde

Java 에서 문자열을 다루를 대표적인 클래스로 `String` , `StringBuffer`, `StringBuilder` 가 있습니다. 

연산이 많지 않을때는 위에 나열된 어떤 클래스를 사용하더라도 이슈가 발생할 가능성은 거의 없습니다. 하지만 **연산횟수가 많아지거나 멀티쓰레드, Race Condition** 등의 상황이 자주 발생 한다면 **각 클래스의 특징을 이해하고 상황에 맞는 적절한 클래스를 사용**해야 합니다.

> Race Condition : 두 개 이상의 프로세스가 공통 자원을 병행적으로(concurrently) 읽거나 쓰는 동작을 할 때, 공용 데이터에 대한 접근이 어떤 순서로 이루어졌는지에 따라 그 실행 결과가 같지 않고 달라지는 상황



## String

`String`과 `StringBuffer/StringBuilder` 클래스의 가장 큰 차이점은 **String은 불변(immutable)의 속성**을 갖는다는 점입니다. `String`을 생성하면 `Heap 영역`에서 특수한 `String Pool`이라는 공간에 생성됩니다. 이 메모리 공간에 생성된 문자열 값은 절대 변하지 않는다는 얘기입니다.

```java
String str = "hello";   // String str = new String("hello");
str = str + " world";  // [ hello world ]
```

위의 예제를 보면, `hello` 값을 가지고 있던 `String` 클래스의 참조변수 `str`이 가리키는 곳에 저장된 `hello`에 `world` 문자열을 더해 `hello world`로 변경한 것으로 착각할 수 있습니다.

그러나 실제 동작은 기존의 문자열을 `hello world`로 변경하는 것이 아니라, 기존에 `hello` 값이 들어가있던 영역을 가리키고 있는 클래스의 참조변수 `str`이 `Heap 메모리 영역`에서`hello world`라는 값을 가지고 있는 **새로운 메모리영역을 가리키게 변경**되고 **처음 선언했던 `hello`로 값이 할당되어 있던 메모리 영역은 `Garbage`로 남아있다가 `GC(garbage collection)`에 의해 사라지게 되는 것** 입니다. **String 클래스는 불변하기 때문에 문자열을 수정하는 시점에 새로운 String 인스턴스가 생성된 것**입니다.



![img](https://t1.daumcdn.net/cfile/tistory/99948B355E2F13350F)

위와 같이 `String`은 불변성을 가지기 때문에 **변하지 않는 문자열을 자주 읽어들이는 경우(조회, 참조 등) String을 사용**해 주시면 좋은 성능을 기대할 수 있습니다. 또,  `Immutable`한 객체여서 간단하게 사용가능하고, 동기화에 대해 신경쓰지 않아도 되기때문에(`Thread-safe`), 내부 데이터를 자유롭게 공유 가능합니다.

그러나 **문자열 추가,수정,삭제 등의 연산이 빈번하게 발생**하는 알고리즘에 `String` 클래스를 사용하면 **힙 메모리(Heap)에 많은 가비지(Garbage)가 생성**되어 힙메모리가 부족으로 어플리케이션 성능에 치명적인 영향을 끼치게 됩니다.

이를 해결하기 위해 `Java`에서는 `가변(mutable)성`을 가지는 `StringBuffer/StringBuilder` 클래스를 도입했습니다.



## StringBuffer / StringBuilder

`String` 과는 반대로 `StringBuffer/StringBuilder` 는 `가변성`을 가지기 때문에 `append()`, `delete()` 등의 `API`를 이용하여 **동일 객체내에서 문자열을 변경하는 것이 가능**합니다. 문자열 연산 등으로 기존 객체의 공간이 부족하게 되는 경우, **기존의 버퍼 크기를 늘리며 유연하게 동작**합니다. 따라서 **문자열의 추가,수정,삭제가 빈번하게 발생할 경우라면 String 클래스가 아닌 StringBuffer/StringBuilder를 사용**해야 합니다.

```java
StringBuffer sb= new StringBuffer("hello");
sb.append(" world");
```

![img](https://t1.daumcdn.net/cfile/tistory/9923A9505E2F133608)



### StringBuffer vs StringBuilder

그렇다면 동일한 `API`를 가지고 있는 `StringBuffer`, `StringBuilder`의 차이점은 무엇일까요?

가장 큰 차이점은 **동기화의 유무**로써 **StringBuffer는 동기화 키워드를 지원하여 멀티쓰레드 환경에서 안전합니다.(thread-safe)**  참고로 **String도 불변성을 가지기때문에 마찬가지로  멀티쓰레드 환경에서의 안정성(thread-safe)**을 가지고 있습니다. 

반대로 **StringBuilder는 동기화를 지원하지 않기때문에 멀티쓰레드 환경에서 사용하는 것은 적합하지 않지만** 동기화를 고려하지 않는 만큼 **단일쓰레드에서의 성능은 StringBuffer 보다 뛰어납니다.**



## 정리

사실 `JDK 1.5버전 이전`에서는 `문자열연산(+, concat)`을 할때에는 조합된 문자열을 새로운 메모리에 할당하여 참조함으로 인해서 성능상의 이슈가 있었습니다. 그러나 **JDK1.5 버전 이후에는 컴파일 단계에서 String 객체를 사용하더라도 StringBuilder로 컴파일 되도록 변경**되었습니다. 그리하여 `JDK 1.5 이후 버전`에서는 `String` 클래스를 활용해도 `StringBuilder`와 성능상으로 차이가 없어졌습니다. 하지만 **반복 루프를 사용해서 문자열을 더할 때에는 새로운 객체를 계속 생성한다는 사실에는 변함이 없습니다.** 그러므로 `String` 클래스를 쓰는 대신, 스레드와 관련이 있으면 `StringBuffer`를, 스레드 안전 여부와 상관이 없으면 `StringBuilder`를 사용하는 것을 권장합니다.

컴파일러에서 분석 할때 최적화에 따라 다른 성능이 나올 수도 있지만 일반적인 경우에는 아래와 같은 경우에 맞게 사용하면 됩니다.

![img](https://t1.daumcdn.net/cfile/tistory/99BE23375E2F133722)

- **String** : 문자열 연산이 적고 멀티쓰레드 환경일 경우

- **StringBuffer** : 문자열 연산이 많고 멀티쓰레드 환경일 경우

- **StringBuilder** : 문자열 연산이 많고 단일쓰레드이거나 동기화를 고려하지 않아도 되는 경우 

단순히 성능만 놓고 본다면 연산이 많은 경우, `StringBuilder > StringBuffer >>> String` 입니다.



---

참고 : [https://ifuwanna.tistory.com/221](https://ifuwanna.tistory.com/221)

[https://12bme.tistory.com/42](https://12bme.tistory.com/42)

[https://jeong-pro.tistory.com/85](https://jeong-pro.tistory.com/85)