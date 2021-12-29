---
title: "Java : final"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : final"
toc: true
toc_sticky: true
toc_label: 목차

---

# final

Java 디자인 패턴 스터디 중 `final` 키워드 활용법을 찾아보다가 정리가 필요할 거 같아, 포스팅을 시작하였습니다.



## 1. 개요

`final` 키워드를 떠올릴 때면 그냥 상수로만 생각할 수도 있지만, final을 `클래스, 메서드, 변수`에 선언하면 조금씩 할 수 있는 부분들이 제안되기 때문에 다양한 방식의 final 활용법을 알아야할 필요성이 있습니다.





## 2. 정의

자바에서 `final` 키워드는 **여러 컨텍스트에서 단 한 번만 할당될 수 있는 entity를 정의할 때 사용**됩니다.



## 3. final 변수

### 3-1. 원시 타입

로컬 원시 변수에 `final`로 선언하면 **한 번 초기화된 변수는 변경할 수 없는 상수값**이 됩니다. 

```java
public static void main(String[] args) {
        final int test = 1;
        // test = 2; // 변수 변경 불가능
    }
```



### 3-2. 객체 타입

객체 변수에 `final`로 선언하면 원시 타입과 동일하게 한번 쓰여진 변수는 재변경 불가능하기 때문에 **해당 변수에 다른 참조 값을 지정할 수 없습니다.** 

**단, 객체 자체가 immutable하다는 의미는 아닙니다**. 참조 값만 변경 불가능한 것이지 객체 안의 속성은 변경 가능합니다.

```java
public static void main(String[] args) {
        final Shoes shoes = new Shoes();

        // shoes = new Shoes(); // 다른 객체로 변경 불가능

        shoes.setSize(260); // 객체 속성은 변경 가능
    }
```



### 3-3. 메서드 인자

메서드 인자에 `final` 키워드를 붙이면, 메서드 안에서 변수값을 변경할 수 없습니다. 

```java
public void setSize(final int size) {
        // size = 265;	// 불가능
        this.size = size;
    }
```



### 3-4. 멤버 변수

클래스의 맴버 변수에 `final`로 선언하면 상수값이 되거나 `write-once` 필드로 한 번만 쓰이게 됩니다.

멤버 변수를 `final`로 선언할 때, static이냐 일반 멤버 변수냐에 따라서 초기화 시점이 아래와 같이 달라집니다.

- static final 맴버 변수
  - 값과 함께 선언시 - ex) `static final int x = 1`
  - 정적 초기화 블록에서 (`static initialization block`)
- instance final 맴버 변수
  - 값과 함께 선언시 - ex) `final int x = 1`
  - 인스턴스 초기화 블록에서 (`instance initialization block`)
  - 생성자 메서드에서

**참고 링크**

- [Static Block과 Instance Block](https://dev-splin.github.io/java/Java-Static-Block,Instance-Block/)





## 4. final 메서드

메서드를 `final`로 선언하면 아래와 같이 상속받은 클래스에서 오버라이드가 불가능하게 됩니다. 때문에 **구현한 코드의 변경을 원하지 않을 때 사용**합니다. **side-effect가 있으면 안 되는 자바 코어 라이브러리에서 final로 선언된 부분을 많이 찾을 수 있습니다.**

```java
public class Shoes {
    int size;
    int name;

    public final void make() {
        System.out.println("사이즈가 " + this.size + "인 " + this.name + "를 만들었습니다.");
    }
}

public class nike extends Shoes {
    // override 불가능
    public void make() {}
}
```





## 5. final 클래스

클래스에 `final`을 선언하면 상속 자체가 안됩니다. 그냥 클래스 그대로 사용해야 합니다. **보통 Util 형식의 클래스나 여러 상수 값을 모아둔 Constants 클래스을 final로 선언**합니다. 

```java
public final class Shoes {
    int size;
    int name;
}

// final 클래스 상속 불가능
public class nike extends Shoes {
}
```



### 5-1. Util 형식 클래스

JDK에서 **String도 final 클래스**로 선언되어 있습니다. 자바의 코어 라이브러리이고 `side-effect`가 있으면 안 되서 다른 개발자가 상속을 해서 새로운 SubString을 만들어 라이브러리로 다른 곳에서 사용하게 되면 유지보수, 정상 실행 보장이 어려워질 수 있기 때문입니다.

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
}
```



### 5-2 상수 클래스

상수 값을 모아준 클래스는 굳이 상속해서 사용할 필요가 없기 때문에 애초에 방지해주는 것이 좋습니다.

```
public final class ShoesName {
	public static final String NIKE_DUNK_ROW = "Nike Dunk Row"
    public static final String NIKE_DUNK_HIGH = "Nike Dunk High"
}
```





## 6. 마치며...

사실 final을 안 쓰더라도 기존 코드를 잘 이해하고 작성하면 문제없이 코딩이 가능합니다. 하지만 **사람은 누구나 실수를 하기 때문에 적절한 final 사용을 통해 위에서 설명한 여러 방법을 제한해 줌으로써 다른 사람의 실수를 방지**할 수 있습니다.

*다만 무차별적으로 final 사용 시 가독성을 해칠 수 있으므로 주의하여야 합니다.*



----

참고 : [https://advenoh.tistory.com/13](https://advenoh.tistory.com/13)
