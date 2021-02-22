---
title: "Java : 멀티 타입파라미터, 제한된 타입파라미터"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true
toc_label: 목차
---

## 멀티타입 파라미터

- 두개 이상의 타입 파라미터를 사용해서 제네릭타입을 선언할 수도 있다.

```java
class A<K,V,...> {}
interface IB<K,V,...> {}

public class Product<T,M> {
  private T kind;
  private M model;
  
  public T getKind() { return this.kind; }
  public M getModel() { return this.model; }
  
  public void setKind(T kind) { this.kind = kind; }
  public void setModel(M model) { this.model = model; }
}

// Tv클래스가 있다고 가정
Product<Tv,String> product = new Product<Tv,String>();
Product<Tv,String> product = new Product<>();
```



## 제한된 타입 파라미터

- 상속 및 구현관계를 이용해서 타입을 제한 할 수 있다.
- 타입 파라미터를 대체할 구체적인 타입
  - 상위타입이거나 하위 또는 구현 클래스만 지정할 수 있다.
- 메소드의 중괄호{}안에서 타입 파라미터 변수로 사용 가능한 것은 상위 타입의 멤버(필드,메소드)로 제한된다.
- 하위 타입에만 있는 필드와 메소드는 사용할 수 없다.



### 선언방법
- public <T extends 상위타입> 리턴타입 메소드명(파라미터 변수, ...) {}
  - 상위타입은 클래스 뿐만 아니라 인터페이스도 가능하다.
  - 인터페이스라고 해서 implements를 사용하지 않고 extends로 사용한다.
  
  

#### 예제 코드
```java
public class Util {
  public <T extends Number> int compare(T t1, T t2) {
    double v1 = t1.doubleValue(); // Number클래스에 있는 doubleValue() 메소드 사용가능
    double v2 = t2.doubleValue();
    return Double.compare(v1,v2);
  }
}

public static void main(String[] args) {
  int result = Util.compare(10,20); // 문자열을 넣을 시 오류가 발생한다.
  System.out.println(result);
}
```
