---
title: "Java : 제네릭 타입, 제네릭 메소드, 와일드 카드"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Generic Type"
toc: true
toc_sticky: true
toc_label: 목차
---

## 제네릭 타입 (Generic Type)

- Java5 부터 새로 추가된 내용
- 타입을 파라미터로 가지는 클래스와 인터페이스를 말한다.
- 타입을 파라미터화 해서 컴파일 시 구체적인 타입이 결정되도록 하는 방법
- 선언시 클래스 또는 인터페이스 이름 뒤에 "<>" 기호가 붙는다.
  - "<>" 사이에는 타입 파라미터가 위치한다.
- 컬렉션, 람다식, 스트림, NIO에서 널리 사용된다.



### 타입 파라미터

- 일반적으로 대문자 알파벳 한문자로 표현한다.
- 개발 코드에서는 타입 파라미터 자리에 구체적인 타입을 지정해야 한다.



### 제네릭을 사용해서 얻는 이점

- 미리 타입을 지정해서 casting을 하지 않고 사용할 수 있다.
- 컴파일시 강한 타입 체크를 할 수 있다.
- 실행시 타입 에러가 나는 것 보다 컴파일시에 미리 타입을 체크해서 에러를 사전에 방지할 수 있다.

```java
// 대표적인 제네릭 타입 사용 예 (Card 라는 클래스가 있다고 가정)
ArrayList<Card> list = new ArrayList<Card>(); 
// ArrayList<Card> list = new ArrayList<>();
list.add(Card o);
String card2 = list.get(i).getCard();

// 제네릭 타입을 사용하지 않은 예 (일일이 형변환을 해주어야 한다.)
ArrayList list = new ArrayList(); 
list.add(Object o);
Object o = list.get(i); 
String card = ((Card)list.get(i)).getCard();

// 클래스에도 제네릭 타입을 사용하는 예
  public class Box<T>{
       private T t; 
       public T get(){
          return t;
       } 
       public void set(T t){
          this.t = t;
       }
   }
   
// 여기에 String을 대입한다고 가정
 Box<String> box = new Box<String>(); 
 box.set("hi~");
 String str = box.get();
 
 // T 부분에 String이 들어가는 것이다.
 Box<String> box = new Box<String>(); 
   public class Box<String>{
       private String t;
       public void set(String t){
          this.t = t;
       }
       public String get(){
          return t;
       }
   }
```



## 와일드 카드(?)의 타입

- 제네릭 타입을 파라미터변수나 리턴타입으로 사용할 때 타입 파라미터를 제한할 목적으로 사용



### 와일드 카드의 세가지 타입

![상속이미지](https://user-images.githubusercontent.com/58713853/74127153-1b0fbc00-4c1d-11ea-8e73-88171a6ad427.jpg)
<br> 이미지 출처 : https://zbomoon.tistory.com/13

- 제네릭 타입<?> : Unbounded Wildcards(제한없음)
  - 타입 파라미터를 대치하는 구체적인 타입으로 모든 클래스나 인터페이스 타입이 올 수 있다.
- 제네릭 타입<? extends 상위타입> : Upper Bounded Wildcards (상위 클래스 제한)
  - 타입 파라미터를 대치하는 구체적인 타입으로 명시한 상위타입이나 그 상위타입을 상속받는 하위타입만 올 수 있다.
  - 제한된 타입 파라미터랑 비슷한 기능을 한다.
  - ex) <? extends 공룡> 을 하게되면 공룡, 투슬리스, 둘리 가 올 수 있다.
- 제네릭 타입<? super 하위타입> : Lower bounded Wildcards (하위 클래스 제한)
  - 타입 파라미터를 대치하는 구체적인 타입으로 명시한 하위타입이나 그 하위타입이 상속받은 상위타입이 올 수 있다.
  - ex) <? extends 멍멍이>을 하게되면 포유류, 동물이 올 수 있다.



## 제네릭 메소드

- 파라미터 변수 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드를 말한다.



### 제네릭 메소드 선언방법

- 리턴 타입 앞에 "<>" 기호를 추가하고 타입 파라미터를 기술한다.
- 타입 파라미터를 리턴타입과 파라미터 변수에 사용한다.



### 제네릭 메소드를 호출하는 두가지 방법

- 명시적으로 구체적인 타입을 지정하는 방법

  - 리턴타입 변수 = <구체적 타입> 메소드명(파라미터 값)

- 파라미터값을 보고 구체적 타입을 추정하는 방법

  - 리턴타입 변수 = 메소드명(파라미터 값);

  

#### 예제 코드

```java
public class Box<T>{
  private T t; 

  public void set(T t){
     this.t = t;
  }
  public T get(){
     return t;
  }
}

public class Util {
  public static <T> Box<T> boxing<T t) {  
  // public static <타입 파라미터,...> 리턴타입 메소드명(파라미터 변수,...) {}
  // Box<T>와 boxing(T t)에 들어갈 T를 명시해주는 것
    Box<T> box = new Box<T>();
    box.set(t);
    return box; // 리턴타입이 Box<T>기 때문에 box를 리턴
  }
}

public static void main(String[] args) {
  Box<Integer> box = <Integer>boxing(100);  // 구체적으로 타입지정
  Box<Integer> box = boxing(100); // 타입 파라미터를 Integer로 추정
}
```

