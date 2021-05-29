---
title: "Spring : 생성자 주입을 사용해야 하는 이유"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "DI(Dependency Injection)"
  - "Spring : Constructor Injection"
toc: true
toc_sticky: true
toc_label: 목차
---

# 생성자 주입을 사용해야 하는 이유

`@Autowired` 를 사용하는 필드 주입이나 수정자 주입 방법보다 생성자 주입을 사용해야 하는 이유에 대해 알아보겠습니다. 



## DI(Dependency Injection) 방법

등록된 빈을 사용하기 위한 스프링 프레임워크의 `DI(Dependency Injection)` 방법은 `생성자 주입(Constructor Injection)`, `@Autowired`를 이용한 `필드 주입(Field Injection)`, Setter를 이용한 `수정자 주입(Setter Injection)` 3가지가 있습니다.



#### 강한 결합

객체 내부에서 다른 객체를 생성하는 것은 강한 결합도를 가지는 구조입니다. A 클래스 내부에서 B 라는 객체를 직접 생성하고 있다면, **B 객체를 C 객체로 바꾸고 싶은 경우에 A 클래스도 수정**해야 하는데, 이런 경우를 `강한 결합`이라고 합니다.

#### 느슨한 결합

객체를 주입 받는다는 것은 **외부에서 생성된 객체를 인터페이스를 통해서 넘겨받는 것**입니다. 즉, **주입 받는 객체가 바뀌어도 같은 인터페이스를 상속받은 객체라면 따로 코드를 수정하지 않아도 되기 때문에 좀 더 유연하게 사용할 수 있다**는 것입니다.

이렇게 하면 결합도를 낮출 수 있고, 런타임 시에 의존관계가 결정되기 때문에 유연한 구조를 가질 수 있습니다.



### 필드 주입(Field Injection)

필드에 `@Autowired` 어노테이션을 붙여주면 자동으로 의존성이 주입이 됩니다. 편리하기 때문에 가장 많이 접할 수 있는 방법입니다.

```java
@Component
public class MadExample {

    @Autowired
    private HelloService helloService;
}
```



### 수정자 주입(Setter Injection)

`수정자(Setter)`를 이용한 주입 방법입니다. 꼭 setter 메서드일 필요는 없고 메서드 이름이 수정자 네이밍 패턴(setXXXX)이 아니어도 동일한 기능을 하면 됩니다. 그래도 일관성과 명확한 코드를 만들기 위해서 정확한 이름을 사용하는 것을 추천합니다.

의존 관계 주입을 런타임 시에 설정할 수 있기 때문에 `낮은 결합도`를 가집니다. 다만, 의존 관계를 주입하지 않아도 객체를 생성할 수 있기 때문에 의존 관계를 가지는 메서드 호출 시 오류가 발생할 수 있습니다.

```java
@Component
public class MadExample {

    private HelloService helloService;

    @Autowired
    public void setHelloService(HelloService helloService) {
        this.helloService = helloService;
    }
}
```



### 생성자 주입(Constructor Injection)

스프링 팀에서 권장하는 방식입니다. 스프링 프레임워크 4.3 버전부터는 **의존성 주입으로부터 클래스를 완벽하게 분리**할 수 있습니다. 단일 생성자인 경우에는 `@Autowired`를 붙이지 않아도 되지만 생성자가 2개 이상인 경우에는 생성자에 어노테이션을 붙여주어야 합니다.

객체를 생성할 때 의존 관계를 주입하기 때문에 `낮은 결합도`를 가집니다. 의존 관계를 주입하지 않으면 객체를 생성할 수 없기 때문에 의존 관계를 가지는 메서드 호출 시에도 문제없이 작동할 수 있습니다.

```java
@Component
public class MadExample {

    // final로 선언할 수 있습니다
    private final HelloService helloService;

    // 단일 생성자인 경우는 추가적인 어노테이션이 필요 없습니다.
    public MadExample(HelloService helloService) {
        this.helloService = helloService;
    }
}
```





대부분 코드에서 `@Autowired` 어노테이션을 필드에 붙여 사용하는 필드 주입 코드를 많이 사용합니다. 그런데 필드 주입을 사용하다보면 

> Spring Team recommends: “Always use constructor based dependency injection in your beans. Always use assertions for mandatory dependencies”.

라는 메세지를 볼 수 있습니다. 핵심은 생성자 주입 방법을 사용하라는 것입니다. 그렇다면 **다른 주입 방법보다 생성자 주입을 권장하는 이유는 무엇일까요?**



## 생성자 주입을 권장하는 이유

필드 주입이나 수정자 주입 방법에는 어떤 문제가 있기에 생성자 주입을 권장하는지에 대해 알아보겠습니다.



### 순환 참조 방지

개발을 하다 보면 여러 컴포넌트 간에 의존성이 생깁니다. 그중에서도 A가 B를 참조하고, B가 다시 A를 참조하는 순환 참조도 발생할 수 있습니다.



#### 필드 주입의 경우

우선 두 개의 서비스 레이어 컴포넌트를 정의합니다. 그리고 서로 참조하고, 서로의 메서드까지 호출하도록 합니다. 즉, 빈이 생성된 후에 비즈니스 로직으로 인하여 서로의 메서드를 순환 참조하는 형태인 것입니다. (실제로 이러한 구조가 되어서는 안됩니다.)

```java
@Service
public class MadPlayService {

    // 순환 참조
    @Autowired
    private MadLifeService madLifeService;

    public void sayMadPlay() {
        madLifeService.sayMadLife();
    }
}
```

---

```java
@Service
public class MadLifeService {
    
    // 순환 참조
    @Autowired
    private MadPlayService madPlayService;

    public void sayMadLife() {
        madPlayService.sayMadPlay();
    }
}
```



##### 실행 결과

```bash
java.lang.StackOverflowError: null
	at com.example.demo.GreetService.sayGreet(GreetService.java:12) ~[classes/:na]
	at com.example.demo.HelloService.sayHello(HelloService.java:12) ~[classes/:na]
	at com.example.demo.GreetService.sayGreet(GreetService.java:12) ~[classes/:na]
	at com.example.demo.HelloService.sayHello(HelloService.java:12) ~[classes/:na]
	at com.example.demo.GreetService.sayGreet(GreetService.java:12) ~[classes/:na]
```

코드를 작성하고 실행해보면, 오류가 날 거라고 예상하지만, 서로의 메서드를 호출하기 전 까지는 아무런 오류 없이 정상적으로 구동됩니다. 문제는 애플리케이션이 아무런 오류나 경고없이 구동된다는 것은 실제 코드가 호출되기 전까지 문제를 발견할 수 없다는 의미가 됩니다. 



#### 생성자 주입의 경우

```java
@Service
public class MadPlayService {
    private final MadLifeService madLifeService;

    public MadPlayService(MadLifeService madLifeService) {
        this.madLifeService = madLifeService;
    }

    // 생략
}
```

---

```java
@Service
public class MadLifeService {
    private final MadPlayService madPlayService;

    public MadLifeService(MadPlayService madPlayService) {
        this.madPlayService = madPlayService;
    }

    // 생략
}
```



##### 실행 결과



```bash
Description:
The dependencies of some of the beans in the application context form a cycle:
┌─────┐
|  madLifeService defined in file [~~~/MadLifeService.class]
↑     ↓
|  madPlayService defined in file [~~~/MadPlayService.class]
└─────┘
```

`BeanCurrentlyInCreationException`이 발생하며 애플리케이션이 구동조차 되지 않습니다. 따라서 발생할 수 있는 오류를 사전에 알 수 는 것입니다.



#### 실행 결과가 차이나는 이유

위와 같이 실행 결과에 차이가 발생하는 이유는 생성자 주입 방법은 필드 주입이나 수정자 주입과는 **빈을 주입하는 순서가 다르기 때문 입니다.**

- **수정자 주입(Setter Injection)** : 우선 주입(inject) 받으려는 빈의 생성자를 호출하여 빈을 찾거나 빈 팩터리에 등록하고 수정자 인자에 사용하는 빈을 찾거나 만듭니다. 그 이후에 주입 받으려는 빈 객체의 수정자를 호출하여 주입합니다.

- **필드 주입(Field Injection)** : 수정자 주입 방법과 동일하게 먼저 빈을 생성한 후에 어노테이션이 붙은 필드에 해당하는 빈을 찾아서 주입하는 방법입니다. 즉 먼저 빈을 생성한 후에 필드에 대해서 주입하는 것입니다.
- **생성자 주입(Constructor Injection)** : 생성자로 객체를 생성하는 시점에 필요한 빈을 주입합니다. 먼저 생성자의 인자에 사용되는 빈을 찾습니다. 그 후에 찾은 인자 빈의 생성자를 호출합니다. 즉 먼저 빈을 생성하지 않습니다. 수정자 주입, 필드 주입처럼 빈을 생성해 놓고 주입하는 방식과는 다른 것입니다.

그렇기 때문에 **순환 참조는 생성자 주입에서만 문제가 됩니다.** 객체 생성 시점에 빈을 주입하기 때문에 서로 참조하는 객체가 생성되지 않은 상태에서 그 빈을 참조하기 때문에 오류가 발생하는 것입니다.

그렇다면 순환 참조 오류를 피하기 위해서 수정자 또는 필드 주입을 사용해야 할까요? 아닙니다. 순환 참조가 있는 객체 설계는 잘못된 설계이기 때문에 **오히려 생성자 주입을 사용하여 순환 참조되는 설계를 사전에 막아야 합니다.**



### 테스트에 용이

생성자 주입을 사용하게 되면 테스트 코드를 조금 더 편리하게 작성할 수 있습니다. DI의 핵심은 관리되는 클래스가 DI 컨테이너에 의존성이 없어야 한다는 것입니다. 즉, **독립적으로 인스턴스화가 가능한 POJO(Plain Old Java Ojbect)** 여야 한다는 것입니다. 즉 DI 컨테이너를 사용하지 않고서도 단위 테스트에서 인스턴스화할 수 있어야 한다는 것입니다.

이 부분은 생성자 주입을 사용하면 테스트 방법이 없다기보다는 조금 더 편리하다고 생각하면 좋을 것 같습니다. Mockito(`@Mock`과 `@spy` 같은)를 적절히 섞어서 테스트를 할 수 있지만 아래처럼 생성자 주입을 사용한 경우 매우 간단한 코드를 만들 수 있습니다.

```java
SomeObject someObject = new SomeObject();
MadComponent madComponent = new MadComponent(someObject);
madComponent.someMadPlay();
```

핵심은 **생성자 주입을 사용하게 되면 의존관계가 있을 경우 의존 관계를 설정해주지 않으면 객체생성 자체가 불가능**하기 때문에 의존관계를 가지고 있는 메소드(`someMadPlay()`)를 테스트할 때 문제가 생기지 않지만, **필드 주입이나 수정자 주입을 사용하게 되면, 의존관계를 설정하지 않아도 객체 생성은 가능**하기 때문에 의존관계를 가지고 있는 메소드를 실행할 시 문제가 발생할 수 있다는 것입니다.



### 불변성(Immutability)

**필드 주입과 수정자 주입은 해당 필드를 final로 선언할 수 없습니다.** 따라서 초기화 후에 빈 객체가 변경될 수 있지만 생성자 주입의 경우는 **객체를 final로 선언할 수 있습니다.** 물론 런타임 환경에서 객체를 변경하는 경우가 있을까 싶지만, 이로 인해 발생할 수 있는 오류를 사전에 미리 방지할 수 있습니다.

```java
@Service
public class MadPlayService {
    private final MadPlayRepository madPlayRepository;

    public MadPlayService(MadPlayRepository madPlayRepository) {
        this.madPlayRepository = madPlayRepository;
    }
}
```



### 오류 방지

스프링 레퍼런스에는 강제화되는 의존성의 경우는 생성자 주입 형태를 사용하고 선택적인 경우에는 수정자 주입 형태를 사용하는 것을 권장합니다. **불변 객체나 null이 아님을 보장할 때는** 반드시 생성자 주입을 사용해야 한다는 것입니다. 앞서 제시한 불변성이 주는 혜택을 예제와 함께 살펴보겠습니다.

```java
@Service
public class MadPlayService {
    @Autowired
    private MadPlayRepository madPlayRepository;

    public void someMethod() {
        // final이 아니기 때문에 값을 변경할 수 있습니다.
        madPlayRepository = null;
        madPlayRepository.call();
    }
}
```

필드 주입을 사용했기 때문에 선언된 필드는 `final`이 아닙니다. 따라서 런타임 시점에 변경할 수 있습니다. 위 코드에서는 `null`을 참조하도록 변경했기 때문에 이어지는 코드에서 `NullPointerException`이 발생할 것입니다. 하지만 생성자 주입을 사용한다면 이와 같은 상황을 컴파일 시점에 방지할 수 있습니다.

---

```java
@Service
public class MadPlayService {
    private final MadPlayRepository madPlayRepository;

    public MadPlayService(MadPlayRepository madPlayRepository) {
        this.madPlayRepository = madPlayRepository;
    }

    public void someMethod() {
        // cannot assign a value to final variable
        madPlayRepository = null;
    }
}
```

즉 필드 주입은 생성과정을 DI 컨테이너가 처리해주기 때문에 의존 관계를 외부에 노출시키지 않고, **생성자 주입은 코드로 생성자 주입하는 부분을 작성하였기 때문에 의존 관계를 외부에 노출시킴으로써 컴파일 시에 오류를 잡아낼 수 있게 됩니다.**



## 정리

생성자 주입을 사용해야 하는 이유를 정리하자면

- **순환 참조 방지** : 순환 참조가 발생하는 경우 애플리케이션이 구동되지 않습니다.
- **테스트에 용이** : 단순 POJO를 이용한 테스트 코드를 만들 수 있습니다.
- **불변성** : 객체를 final로 선언하여 객체를 변경해 발생할 수 있는 오류를 사전에 미리 방지할 수 있습니다.
- **오류 방지** : 의존 관계를 외부에 노출시킴으로써 컴파일 시에 오류를 잡아낼 수 있게 됩니다.



---

참고 : [https://yaboong.github.io/spring/2019/08/29/why-field-injection-is-bad/](https://yaboong.github.io/spring/2019/08/29/why-field-injection-is-bad/)

[https://madplay.github.io/post/why-constructor-injection-is-better-than-field-injection](https://madplay.github.io/post/why-constructor-injection-is-better-than-field-injection)
