---
title: "Spring : 로직 단위테스트"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Mockito"
toc: true
toc_sticky: true
toc_label: 목차
---

# 로직 단위테스트

빈들 간에는 다양한 관계를 맺고 있는 경우가 많습니다.  **하나의 빈을 사용한다는 것은 관계된 빈들도 함께 동작한다는 것을 의미**합니다. 

하나의 빈을 테스트할 때 관련된 빈들이 모두 잘 동작하는지 테스트하는 것을 `통합 테스트(integration test)`라 합니다. 

관계된 다른 클래스와는 상관 없이 특정 빈이 가지고 있는 기능만 잘 동작하는지 확인하는 것을 `단위 테스트(unit test)`라 합니다.



## 예제

[JUnit](https://dev-splin.github.io/spring/Spring-JUnit/)에서 실습했던 내용에 다음의 클래스를 추가해보도록 하겠습니다.

```java
package org.edwith.webbe.calculatorcli;

import org.springframework.stereotype.Service;

@Service
public class MyService {
    private final CalculatorService calculatorService;

    public MyService(CalculatorService calculatorService) {
        this.calculatorService = calculatorService;
    }

    public int execute(int value1, int value2){
        return calculatorService.plus(value1, value2) * 2;
    }
}
```



`MyService`클래스는 아래의 클래스 다이어그램처럼 `CalculatorService`를 필드로 가지고 있습니다. 해당 필드는 생성자를 통해 초기화를 합니다. 

[![img](https://cphinf.pstatic.net/mooc/20200214_134/1581664575293HUkdg_PNG/1.png?type=w760)](https://www.boostcourse.org/web326/lecture/58978/?isDesc=false#)

`MyService`가 가지고 있는 `execute`메소드는 `calculatorService.plus()`메소드를 사용합니다. 2개의 값을 받아 plus()메소드를 이용해 더한 후 2를 더한 값을 반환합니다.



이렇게 작성한 `MyService`객체는 다음과 같은 방법으로 테스트를 할 수 있습니다.

```java
package org.edwith.webbe.calculatorcli;

import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {ApplicationConfig.class})
public class MyServiceTest {
    @Autowired
    MyService myService;

    @Test
    public void execute() throws Exception{
        int value1 = 5;
        int value2 = 10;

        int result = myService.execute(value1, value2);

        Assert.assertEquals(30, result);
    }
}
```

 `@RunWith(SpringJUnit4ClassRunner.class)` 는 `JUnit`에서 내부적으로 스프링 빈 컨테이너를 생성한다고 하였습니다. 스프링 빈 컨테이너는 빈들을 찾아 메모리에 올리게 됩니다. 그리고 나서 `myService`필드에 객체를 주입하게 됩니다.

`assertEquals()`메소드로 비교한 결과, 결과는 맞게 나올 거라고 예상할 수 있습니다. 



## Mockito

그런데, `CalculatorService`의 `plus`()메소드에 버그가 있다면 어떻게 될까요? `plus()`메소드가 더하는 것이 아니라 곱한 결과를 반환한다면 어떻게 될까요? 아마 내가 생각한 값이 나오지 않을 것입니다.

`MyService`를 테스트하려고 하였지만 `CalculatorService`의 버그로 인해 에러가 날 수도 있는 것입니다. 이런 문제를 해결하려면 어떻게 해야할까요? 이런 문제를 해결하는 방법 중 하나는 `목(Mock)객체`를 이용하는 방법이 있습니다.



`Mockito`는 모기의 영어단어를 줄인 말이 아니고 유명한 영화 대사인 “모히또 한 잔하러 가자”의 모히토에서 이름을 따왔다고 합니다. `mockito`는 오픈소스 목 프레임워크입니다. **테스트를 위해 가짜 객체를 쉽게 만들어 줄 수 있는 프레임워크**라고 생각하면 편합니다.



[![img](https://cphinf.pstatic.net/mooc/20200214_113/1581664887928KKzQy_PNG/mockito.png?type=w760)](https://www.boostcourse.org/web326/lecture/58978/?isDesc=false#)

`Mockito` 프레임워크 자체만 하더라도 다뤄야 할 양이 많아서 `Mockito`에 대해 모두 다루기는 어렵습니다. 



### 문제 해결

[![img](https://cphinf.pstatic.net/mooc/20200214_263/1581664722932hBxQi_PNG/2.png?type=w760)](https://www.boostcourse.org/web326/lecture/58978/?isDesc=false#)

`MyService`가 사용하던 `CalculatorService`를 사용하는 대신, **가짜 객체를 하나 생성**하도록 하는 것입니다. **내가 원하는 동작을 하는 `Mock객체`로 `CalculatorService`를 사용**함으로써 **`MyService`의 내용만 테스트를 수행**할 수 있습니다.

이를 위해 pom.xml파일에 다음을 추가합니다.

```xml
<!-- test mock을 위한 라이브러리를 추가합니다. -->
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>1.9.5</version>
            <scope>test</scope>
        </dependency>
```

 

`MyserviceTest`를 수정해줍니다.

```java
package org.edwith.webbe.calculatorcli;

import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;

import static org.mockito.BDDMockito.given;
import static org.mockito.Matchers.anyInt;
import static org.mockito.Mockito.verify;

@RunWith(MockitoJUnitRunner.class)
public class MyServiceTest {
    @InjectMocks
    MyService myService;

    @Mock
CalculatorService calculatorService;

    @Test
    public void execute() throws Exception{
        // given
        int value1 = 5;
        int value2 = 10;
        given(calculatorService.plus(5, 10)).willReturn(15);
        //when

        // when
        int result = myService.execute(value1, value2);

        // then
        verify(calculatorService).plus(anyInt(), anyInt());
        Assert.assertEquals(30, result);
    }
}
```



```java
@RunWith(MockitoJUnitRunner.class)
```

`@RunWith(MockitoJUnitRunner.class)`는 Mockito가 제공하는 Junit 확장 클래스 `MockitoJunitRunner`를 이용해 테스트 클래스를 실행하도록 합니다.



```java
@Mock
CalculatorService calculatorService;
```

`@Mock` 어노테이션은 `calculatorService`가 목 객체를 참조하도록 합니다. 즉, 객체를 생성하지 않아도 **자동으로 객체가 생성되고 해당 필드가 초기화 된다는 것을 의미**합니다.



```java
@InjectMocks
MyService myService;
```

`@InjectMocks` 어노테이션이 붙은 필드는 목 객체를 사용하는 `MyService` 객체를 생성하여 초기화하라는 의미를 가집니다.  `myService` 역시 직접 **초기화하지 않아도 자동으로 `MyService` 객체가 생성되어 초기화** 됩니다.



```java
given(calculatorService.plus(5, 10)).willReturn(15);
```

`given()`은 static메소드입니다. import문을 보면 **`org.mockito.BDDMockito` 클래스의 static 메소드**인것을 알 수 있습니다. 이클립스에서 `Ctrl + Space` 를 누르면 Code Assist가 동작하여 처음 몇자만 입력하면 필요한 클래스의 import 나 사용 가능한 메소드를 추천 또는 자동완성을 해 줍니다. 하지만 static import 는 기본적으로 자동으로 인식하지 못합니다.

`calculatorService`는 가짜 객체입니다. 이 가짜 객체가 동작하는 방법을 규정할 수 있는 것이 given()메소드입니다. **`calculatorService.plus(5,10)`을 호출하면 plus메소드가 15를 반환하도록 하라는 의미**를 가집니다. 

실제로 `calculatorService.plus()`의 연산을 곱셈으로 바꿔도 15가 나오는 것을 볼 수 있습니다.



```java
int result = myService.execute(value1, value2);
```

`execute()`메소드는 내부적으로 `calculatorService`의 `plus()`메소드를 호출합니다. 해당 메소드는 위에서 설정한대로만 동작합니다. value1과 value2가 무슨 값이든지 간에 result는 15의 2배인 30을 반환할 것입니다.



```java
verify(calculatorService).plus(anyInt(), anyInt());
```

`verify()`메소드는 **`org.mockito.Mockito`의 static한 메소드**입니다. 

`anyInt()`메소드는 **`org.mockito.Matchers`의 static한 메소드**입니다. 

`verify()`메소드는 **파라미터로 들어온 객체의 `plus()`메소드가 호출된 적이 있는지 검증**합니다. 좀 더 자세히 살펴보자면 `plus(anyInt(), anyInt())`는 어떤 정수든지 2개를 파라미터로 넣어서 `plus()`메소드를 호출했는지를 검증합니다. 

`myService.execute()`메소드 내부적으로 `plus()` 메소드를 호출했다면 해당 메소드는 검증을 성공한 것입니다. 만약, `execute()`메소드에서 `plus(anyInt(), anyInt())`를 호출하지 않았다면 오류가 발생하게 됩니다.



```java
public int execute(int value1, int value2){
        int value = 0;
        // value = calculatorService.plus(value1, value2); // plus메소드가 호출되지 않도록 주석문 처리하였다.
        return  value * 2;
    }
```

`execute()` 메소드를 위와 같이 수정한 후 테스트를 실행해보면 `JUnit`에서 `Failures` 가 발생한 것을 볼 수 있습니다.



---

참고 : https://www.boostcourse.org/web326/lecture/58978/?isDesc=false

