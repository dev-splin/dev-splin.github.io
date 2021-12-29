---
title: "Java : JVM과 연관지어 보는 Static Block과 Instance Block"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Static Block,Instance Block"
toc: true
toc_sticky: true
toc_label: 목차
---

# 스태틱 블록(Static Block)과 인스턴스 블록(Instance Block)

# JVM과 연관지어 보는 스태틱 블록(Static Block)과 인스턴스 블록(Instance Block)

Java로 디자인 패턴 스터디 중 Class 안에 `static { }`와 `{ }`로만 이루어진 처음보는 블록이 있어 이 글을 포스팅하게 되었습니다.

JVM으로 자바 프로그램 실행하는 과정과 연관되어 있기 때문에 아래의 JVM 글을 먼저 보고 오시면 더 좋을 거 같습니다 :)

**참고 링크**

- [JVM(Java Virtual Machine)](https://dev-splin.github.io/java/Java-JVM(Java-Virtual-Machine)/)





## 1. 스태틱 블록(Static Block)

- 클래스가 로딩되고 해당 클래스가 사용 중이라면 자동으로 실행되는 블록
- 클래스 안에 여러 개의 static 블록을 넣을 수 있다.
- 주로 클래스의 static 멤버 변수를 초기화시키는 용도로 사용

해당 클래스가 여러 군데 사용되고 있다면 스태틱 블록이 여러 번 실행된다고 생각할 수 있지만, **static 관련 메모리는 모든 쓰레드가 공유하는 JVM의 Method Area에서 관리하기 때문에 각 Class 당 한 번만 실행**됩니다.



### 1-1. 실행 순서

1. 클래스 로드
2. 해당 클래스의 **static 속성이나 메서드를 사용하거나 최초로 객체를 생성한다면, 해당 static 메모리나 객체 크기 만큼 메모리를 할당**해줍니다.
   - 해당 Class File이 **static으로 되어 있다고 무조건 메모리를 할당하는 것은 아닙니다.**
3. static 블록이 선언된 순서대로 실행됩니다.



#### static으로 되어 있다고 무조건 메모리를 할당하지 않는 이유??

static으로 되어 있다고 무조건 메모리를 할당하지 않는 이유는 **JVM의 Class Loader가 jar파일 내 저장된 클래스들을 JVM위에 탑재하고 사용하지 않는 클래스들은 메모리에서 삭제하기 때문**입니다.



### 1-2. 코드

Main Class를 보면 `Font.name`처럼 해당 클래스를 사용하고 있어야 스태틱 블록이 실행됩니다.

만약 주석으로 된 `Font font = null`도 스태틱 블록이 실행되어야 한다고 생각할 수도 있지만, 클래스 명만 작성 즉, **메모리에 Font 객체의 크기 만큼 공간을 할당해준 것이지 실질적으로 Font Class를 사용하지 않았기 때문에 스태틱 블록이 실행되지 않습니다.**

```java
// Static Block을 가지는 Font Class
public class Font {
    static String name;
    int size;

    static {
        System.out.println("Static Block 1");
    }
    static {
        System.out.println("Static Block 2");
    }
}

// Main
public class Main {
    public static void main(String[] args) {
        Font.name = "test";
        // Font font = null; // 해당 클래스를 사용하지 않았기 때문에 아무 것도 실행되지 않음.
    }
}
```

#### 실행 결과

```
Static Block 1
Static Block 2
```





## 2. 인스턴스 블록(Instance Block)

- 인스턴스가 생성된 후(**JVM의 JVM Stack에 메모리가 쌓인 후**) 자동으로 실행하는 블록
- 한 클래스 안에 여러 개의 인스턴스 블록을 넣을 수 있음
- 인스턴스 변수를 초기화시키는 용도로 사용
  - 어떤 생성자가 호출되든 그 전에 공통으로 초기화시키고 싶은 작업이 있다면 인스턴스 블록에서 처리하기 유용



### 2-1. 실행 순서

1. 인스턴스를 생성합니다.
2. 생성자가 호출되기 전에 인스턴스 블록이 선언된 순서대로 실행됩니다.



### 2-2. 코드

인스턴스 블록은 생성자 호출 전에 실행되기 때문에 Main Class를 보면 `new Font()`처럼 **해당 클래스로 객체를 만들어주어야  실행되고 객체를 만들어 줄 때마다 실행되는 것**을 볼 수 있습니다.

```java
// Instance Block을 가지는 Font Class
public class Font {
    static String name;
    int size;

    {
        System.out.println("Instance Block 1");
    }
    {
        System.out.println("Instance Block 2");
    }
}

// Main
public class Main {
    public static void main(String[] args) {
        Font font = new Font();

        font = new Font();
    }
}
```

#### 실행 결과

```
Instance Block 1
Instance Block 2
Instance Block 1
Instance Block 2
```





## 3. 스태틱 블록과 인스턴스 블록의 혼합 사용

그렇다면 스태틱 블록과 인스턴스 블록을 혼합해서 사용하면 어떻게 될까요??

**스태틱 블록은 각 Class 당 한 번만 실행**된다고 하였고, 인스턴스 블록은 객체를 만들 때 마다 실행된다고 하였습니다. 정말 그런지 예제로 확인해보겠습니다.



### 코드

```java
// Instance Block과 Static Block을 가지는 Font Class
public class Font {
    static String name;
    int size;

    {
        System.out.println("Instance Block 1");
    }
    {
        System.out.println("Instance Block 2");
    }
    static {
        System.out.println("Static Block 1");
    }
    static {
        System.out.println("Static Block 2");
    }
}

// Main
public class Main {
    public static void main(String[] args) {
        Font.name = null;
        Font font = new Font();

        font = new Font();
    }
}
```



### 실행 순서

1. 클래스 로드
2. 해당 클래스의 **static 속성이나 메서드를 사용하거나 최초로 객체를 생성한다면, 해당 static 메모리나 객체 크기 만큼 메모리를 할당**해줍니다.
   - static 속성이나 메서드를 사용 : `스태틱 블록 실행`
   - 최초 객체 생성 : `스태틱 블록 실행` -> `인스턴스 블록 실행`
3. 만약 추가적인 객체 생성이 있을 시 `인스턴스 블록 실행`



#### 실행 결과

예상했던 대로 결과가 잘 나오는 것을 볼 수 있습니다.

```
Static Block 1
Static Block 2
Instance Block 1
Instance Block 2
Instance Block 1
Instance Block 2
```





## 4. Inner Class 소유 시 스태틱 블록과 인스턴스 블록

만약 Class가 Inner Class를 가지고 있다면 어떻게 될 지 알아보겠습니다.



### 코드

```
public class Font {
    static String name;
    private int size;
    Owner owner;

    public Font() {
        size = 12;
        owner = new Owner();
    }

    {
        System.out.println("Instance Block 1");
    }
    {
        System.out.println("Instance Block 2");
    }
    static {
        System.out.println("Static Block 1");
    }
    static {
        System.out.println("Static Block 2");
    }

    public class Owner {
        static String name;
        int tel;

        {
            System.out.println("Inner Class Instance Block 1");
        }

        static {
            System.out.println("Inner Class Static Block 1");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Font.name = null;
//        Font.Owner.name = null;

        Font font = new Font();


        font = new Font();
    }
}
```



---

참고 : [https://uoonleen.tistory.com/6](https://uoonleen.tistory.com/6)

