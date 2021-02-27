---
title: "Java : block 변수/member field, Wrapper Class, boxing/unboxing"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Boxing/UnBoxing"
toc: true
toc_sticky: true
toc_label: 목차
---

## block 변수 / member field
- block 변수(메소드와 같이 블락안에 있는 변수)와 member field의 이름이 같으면 block 변수가 우선순위가 높다.



## Wrapper Class
- 참조타입을 기본타입으로 바꿀때 사용하는 클래스
- 기본타입의 앞글자를 대문자로 만들면 클래스명이 된다.
- ex) Boolean, Byte, Integer, Float, Character...



## Boxing / UnBoxing
래퍼 클래스(Wrapper class)는 산술 연산을 위해 정의된 클래스가 아니므로, 인스턴스에 저장된 값을 변경할 수 없습니다.<br/>단지, 값을 참조하기 위해 새로운 인스턴스를 생성하고, 생성된 인스턴스의 값만을 참조할 수 있습니다.

![boxing,unboxing](https://user-images.githubusercontent.com/79291114/109388233-3d1fcd80-7949-11eb-9f11-3739f9ac0974.png)

위의 그림과 같이 기본 타입의 데이터를 래퍼 클래스의 인스턴스로 변환하는 과정을 **박싱(Boxing)**이라고 합니다.

반면 래퍼 클래스의 인스턴스에 저장된 값을 다시 기본 타입의 데이터로 꺼내는 과정을 **언박싱(UnBoxing)**이라고 합니다.



### AutoBoxing / AutoUnBoxing

JDK 1.5부터는 박싱과 언박싱이 필요한 상황에서 자바 컴파일러가 이를 자동으로 처리해 줍니다.<br/>이렇게 자동화된 박싱과 언박싱을 **오토 박싱(AutoBoxing)**과 오토 언박싱(AutoUnBoxing)이라고 부릅니다.



```java
Integer num = new Integer(17); // 박싱 (17 자체를 기본타입으로 봅니다.)
int n = num.intValue();        // 언박싱
System.out.println(n);

Character ch = 'X'; // Character ch = new Character('X'); : 오토박싱
char c = ch;        // char c = ch.charValue();           : 오토언박싱

System.out.println(c);
```

위의 예제에서 볼 수 있듯이 래퍼 클래스인 `Interger` 클래스와 `Character` 클래스에는 각각 언박싱을 위한 `intValue()` 메소드와 `charValue()` 메소드가 포함되어 있습니다.

반대로 `charValue()` 메소드를 사용하지 않고도, 오토 언박싱을 이용하여 인스턴스에 저장된 값을 바로 참조할 수 있습니다.



#### 기본 타입과 Wrapper클래스 간의 연산

```java
public class Wrapper02 {
    public static void main(String[] args) {
        Integer num1 = new Integer(7); // 박싱
        Integer num2 = new Integer(3); // 박싱

        int int1 = num1.intValue();    // 언박싱
        int int2 = num2.intValue();    // 언박싱

1      Integer result1 = num1 + num2; // 10
2      Integer result2 = int1 - int2; // 4
3      int result3 = num1 * int2;     // 21
    }
}
```

1. num1과 num2를 autounboxing 후 연산한 다음 autoboxing 합니다.
2. int1과 int2를 연산 후 autoboxing 합니다.
3. num1과 num2를 autounboxing 후 연산합니다.



#### Wrapper클래스 간의 비교

```java
public class Wrapper03 {
    public static void main(String[] args) {
        Integer num1 = new Integer(10);
        Integer num2 = new Integer(20);
        Integer num3 = new Integer(10);

        System.out.println(num1 < num2);       // true
1       System.out.println(num1 == num3);      // false
2       System.out.println(num1.equals(num3)); // true

    }

}
```

래퍼 클래스의 비교 연산도 오토언박싱을 통해 가능해지지만, 인스턴스에 저장된 값의 동등 여부 판단은 1번 라인처럼 비교 연산자인 동등 연산자(==)를 사용해서는 안 되며, 2번 라인처럼 equals() 메소드를 사용해야만 합니다. 

래퍼 클래스도 객체이므로 동등 연산자(==)를 사용하게 되면, 두 인스턴스의 값을 비교하는 것이 아니라 두 인스턴스의 주소값을 비교하게 됩니다.<br/>따라서 서로 다른 두 인스턴스를 동등 연산자(==)로 비교하게 되면, 언제나 false 값을 반환되게 됩니다.

그러므로 인스턴스에 저장된 값의 동등 여부를 정확히 판단하려면 equals() 메소드를 사용해야만 합니다.

---

참고 : http://www.tcpschool.com/java/java_api_wrapper

