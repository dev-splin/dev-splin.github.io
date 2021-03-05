---
title: "Spring : JUnit"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : JUnit"
toc: true
toc_sticky: true
toc_label: 목차
---

# JUnit

프로그래밍 언어마다 **테스트를 위한 프레임워크가** 존재합니다. 이러한 도구들을 **보통 xUnit**이라고 말합니다. **자바언어의 경우는 JUnit**이라고 말합니다.

각 언어마다 사용되는 xUnit은 다음과 같습니다.



[![img](https://cphinf.pstatic.net/mooc/20200211_139/1581397814787LQDlX_PNG/1.png?type=w760)](https://www.boostcourse.org/web326/lecture/58976?isDesc=false#)

 

##  JUnit 사용하기

`JUnit`을 사용하려면 JUnit 라이브러리가 클래스패스(CLASSPATH)에 존재해야 합니다.

직접 다운로드를 받는 것은 번거롭기 때문에 보통 빌드 도구인 Maven이나 Gradle을 이용해 다운로드 받아 사용합니다.

Maven을 사용할 경우 pom.xml에 다음을 추가합니다.

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>버전</version>
  <scope>test</scope>
</dependency> 
```

scope가 test인 이유는 해당 라이브러리가 테스트 시에만 사용된다는 뜻입니다. 테스트가 아닌 상황에선 해당 라이브러리가 사용되지 않습니다.

  

### JUnit을 이용한 자바 어플리케이션 테스트

1. **`Group Id가 org.apache.maven.archetypes`이고  `Artifact Id가  maven-archetype-quickstart`인 Archetype으로 Maven 프로젝트를 만듭니다.**
   - src/main/java 폴더에 만들어야 할 코드를 작성하고, src/test/java 폴더에는 테스트 코드를 작성합니다. 
2. **pom.xml 파일을 다음과 같이 수정합니다**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.edwith.webbe</groupId>
    <artifactId>calculatorcli</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- junit 4.12 라이브러리를 추가합니다. -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <!-- 사용할 JDK버전을 입력합니다. JDK 11을 사용할 경우에는 1.8대신에 11로 수정합니다.--><!-- 사용할 JDK버전을 입력합니다. JDK 11을 사용할 경우에는 1.8대신에 11로 수정합니다.-->
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.7.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>utf-8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

생성된 pom.xml파일에는 불필요한 부분이 많습니다. 아래와 같이 pom.xml파일을 수정합니다. 라이브러리 의존성에 Junit이 있는 것을 확인할 수 있습니다. 그리고 JDK를 1.8로 사용하도록 Maven 플러그인을 설정하였습니다.



3. **사칙연산을 구하는 CalculatorService 클래스를 작성합니다.**

테스트를 할 CalculatorService 클래스를 작성합니다.

나누기의 경우 0으로 나누게 되면 `ArithmeticException`이 발생하기 때문에 divide() 메소드에는 `ArithmeticException`을 `throws` 하고 있는 것을 확인할 수 있습니다.

```java
package org.edwith.webbe.calculatorcli;

public class CalculatorService {
    public int plus(int value1, int value2) {
        return value1 + value2;
    }

    public int minus(int value1, int value2) {
        return value1 - value2;
    }

    public int multiple(int value1, int value2) {
        return value1 * value2;
    }

    public int divide(int value1, int value2) throws ArithmeticException {
        return value1 / value2;
    }
}
```

 

4. **CalculatorService클래스를 테스트하는 CalculatorServiceTest클래스를 /src/test/java 폴더 아래에 작성합니다.**

```java
package org.edwith.webbe.calculatorcli;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;

public class CalculatorServiceTest {
	CalculatorService calculatorService;
	
	@Before
	public void init() {
		this.calculatorService = new CalculatorService();
	}
	
	@Test
	public void plus() throws Exception {
		int value1 = 10;
		int value2 = 5;
		
		int result = calculatorService.plus(value1, value2);
		
		Assert.assertEquals(15, result); // 결과와 15가 같을 경우에만 성공
	}
	
	@Test
    public void divide() throws Exception{
        int value1 = 10;
        int value2 = 5;

        int result = calculatorService. divide (value1, value2);

        Assert.assertEquals(2,result); // 결과와 2가 같을 경우에만 성공
    }

    @Test
    public void divideExceptionTest() throws Exception{
        // given
        int value1 = 10;
        int value2 = 0;

        try {
            calculatorService.divide(value1, value2);
        }catch (ArithmeticException ae){
            Assert.assertTrue(true); // 이부분이 실행되었다면 성공
            return; // 메소드를 더이상 실행하지 않는다.
        }
        Assert.fail(); // 이부분이 실행되면 무조건 실패다.
    }
}
```

코드를 살펴보면 `@Before`, `@Test`와 같은 어노테이션이 붙은 것을 확인할 수 있습니다.

Junit을 이용하는 테스트 클래스는 다음과 같은 어노테이션이 주로 이용됩니다.

[![img](https://cphinf.pstatic.net/mooc/20200211_238/1581397925591vY1gC_PNG/2.png?type=w760)](https://www.boostcourse.org/web326/lecture/58976?isDesc=false#)



테스트 클래스에 테스트 메소드가 3개 있다면, 각각의 메소드에는 `@Test`가 붙어 있어야 합니다. 

테스트 클래스가 실행되기 전에 `@BeforeClass`가 붙은 메소드가 실행됩니다.

그리고 테스트 메소드가 실행되기 전에 `@Before`가 붙은 메소드가 실행됩니다.

그다음은 `@Test`가 붙은 메소드가 실행되고 `@After`가 붙은 메소드가 실행됩니다.

이렇게 3개의 메소드가 실행된 후에 `@AfterClass`가 붙은 메소드가 실행되고 프로그램은 종료됩니다.



[![img](https://cphinf.pstatic.net/mooc/20200211_163/1581397986058LHXHo_PNG/12.png?type=w760)](https://www.boostcourse.org/web326/lecture/58976?isDesc=false#)

`JUnit`테스트 클래스 실행은 이클립스를 사용할 경우 이클립스에 내장된 JUnit에 의해 실행됩니다. 이클립스에 내장된 `Junit`에 `@Test`가 붙은 메소드를 실행하는 main(String[] args)메소드가 있다고 생각하면 됩니다.



 

5. **`Run As -> Junit Test`로 테스트를 실행합니다.**

[![img](https://cphinf.pstatic.net/mooc/20200211_34/1581398140746bfNk2_PNG/8.png?type=w760)](https://www.boostcourse.org/web326/lecture/58976?isDesc=false#)

`@Test`가 붙어 있는 3가지 메소드가 실행됩니다. Runs부분에 **3/3** 이라고 써 있는데  **3개의 테스트 메소드 중에서 3개의 메소드의 실행이 종료되었다는 것을 의미**합니다. (실제로 테스트 메소드가 하나씩 실행해 나가면서 숫자가 증가하는 것을 알 수 있습니다.)

이 때 성공했다는 것은 무슨 의미일까요? 

첫번째, **Exception이 발생하지 않고 테스트 메소드 메소드가 잘 실행되었다는 것을 의미**합니다. 두번째, **Assert.assertOOO() 메소드가 문제 없이 실행되었다는 것을 의미**합니다.



테스트 메소드를 만들 땐 `assert메소드`라고 불리는 메소드(메소드 이름이 assert로 시작합니다.)를 이용해 값을 체크하도록 하는 방법을 주로 사용합니다.

```markup
Assert.assertEquals(result, 2); // 결과와 2가 같을 경우에만 성공
```

위의 코드의 2를 3으로 수정한 후 테스트 클래스를 실행해 봅니다.

[![img](https://cphinf.pstatic.net/mooc/20200211_166/1581398631343MVLKT_PNG/9.png?type=w760)](https://www.boostcourse.org/web326/lecture/58976?isDesc=false#)

결과값이 틀리기 때문에 `Failure` 숫자가 1로 증가한 걸 알 수 있습니다. 그리고 그래프가 갈색으로 표시됩니다. 만약 `Exception`이 발생했다면 `Errros` 숫자가 1로 표시되게 됩니다.



[![img](https://cphinf.pstatic.net/mooc/20200211_121/1581398659584eABit_PNG/10.png?type=w760)](https://www.boostcourse.org/web326/lecture/58976?isDesc=false#)

만약 특정 테스트 메소드만 실행하고 싶다면, 위 그림처럼 테스트 클래스를 펼치고 특정 테스트 메소드를 선택한 후에 `Run as -> Junit Test`를 선택합니다.



[![img](https://cphinf.pstatic.net/mooc/20200211_163/15813988127516WdN3_PNG/11.png?type=w760)](https://www.boostcourse.org/web326/lecture/58976?isDesc=false#)

1개의 메소드가 테스트 되는 것을 확인 할 수 있습니다.



### JUnit의 중요 assert

JUnit의 Assert클래스는 다양한 assert메소드를 가집니다. 그 중에서 자주 사용하는 메소드에 대해 알아보도록 하겠습니다.

[![img](https://cphinf.pstatic.net/mooc/20200211_44/1581398989118HbTys_PNG/3.png?type=w760)
  ](https://www.boostcourse.org/web326/lecture/58976?isDesc=false#)

여기서 assertNotSame(ox, oy)는 ox와 oy과 같은 객체를 참조하고 있지 않다면 테스트 성공입니다. 즉 ox와 oy가 다른 객체인지를 확인하는 것입니다.



### 스프링 테스트 어노테이션 사용하기

사칙연산을 계산하는 CalculatorService클래스를 스프링 프레임워크에서 빈(Bean)으로 관리되도록 수정해보겠습니다. 그리고 나서 기존 테스트 클래스도 수정하여 빈(Bean)을 테스트 하겠습니다.

 

1. **스프링 프레임워크를 사용하도록 기존 코드 변경하기**

스프링 프레임워크를 사용하려면 관련 라이브러리들이 프로젝트에 추가 되야 합니다.  `pom.xml `파일을 다음과 같이 수정합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.edwith.webbe</groupId>
    <artifactId>calculatorcli</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <failOnMissingWebXml>false</failOnMissingWebXml>
        <spring.version>5.2.3.RELEASE</spring.version>
    </properties>

    <dependencies>
        <!-- junit 4.12 라이브러리를 추가합니다. -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <!-- spring-context와 spring-test를 의존성에 추가합니다.-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>

    </dependencies>

    <!-- 사용할 JDK버전을 입력합니다. JDK 11을 사용할 경우에는 1.8대신에 11로 수정합니다.--><!-- 사용할 JDK버전을 입력합니다. JDK 11을 사용할 경우에는 1.8대신에 11로 수정합니다.-->
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.7.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>utf-8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

수정을 하고 나서 Maven update를 수행합니다.

 

2. **설정파일 만들고 Component 만들어주기**

스프링 프레임워크를 사용하려면 설정 파일을 작성해야 합니다. 스프링 설정 파일은 `xml`파일이나 `Java Config`로 작성할 수 있다고 하였습니다. `Java Config`파일로 다음과 같이 작성해 보도록 하겠습니다.

```java
package org.edwith.webbe.calculatorcli;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = {"org.edwith.webbe.calculatorcli"})
public class ApplicationConfig {
}
```

클래스 위에 `@Configuration` 어노테이션이 붙어 있으면 스프링 설정 파일이라는 것을 의미합니다.

스프링 설정 파일은 스프링 빈 컨테이너인 `ApplicationContext`에서 읽어 들인다고 하였습니다. `@ComponentScan`은 특정 패키지 이하에서 컴포넌트를 찾도록 합니다. 해당 어노테이션에 설정된 패키지 이하로부터 `@Component`, `@Repository`, `@Service`, `@Controller`, `@RestController` 등의 어노테이션이 붙은 클래스를 찾아 빈으로 등록합니다.

 

```java
package org.edwith.webbe.calculatorcli;

import org.springframework.stereotype.Component;

@Component
public class CalculatorService {
    public int plus(int value1, int value2) {
        return value1 + value2;
    }

    public int minus(int value1, int value2) {
        return value1 - value2;
    }

    public int multiple(int value1, int value2) {
        return value1 * value2;
    }

    public int divide(int value1, int value2) throws ArithmeticException {
        return value1 / value2;
    }
}
```

스프링 빈 컨테이너에서 관리한다는 것은 개발자가 직접 인스턴스를 생성하지 않는다는 것을 의미합니다. **스프링 빈 컨테이너가 인스턴스를 생성해 관리**한다는 것을 뜻합니다. 스프링 빈 컨테이너가 `CalculatorService`클래스를 찾아 빈으로 등록할 수 있도록 클래스 위에 `@Component`를 붙입니다.

위와 같이 작성했다면, 기존 클래스를 스프링 프레임워크에서 사용될 준비가 끝난 것입니다. `CalculatorService` 클래스를 이용하려면 다음과 같은 클래스를 작성해야 합니다.

```java
package org.edwith.webbe.calculatorcli;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args){
        // ApplicationConfig.class 설정파일을 읽어들이는 ApplicationContext객체를 생성합니다.
        // 아래 한줄이 실행되면서 컴포넌트 스캔을 하고, 컴포넌트를 찾으면 인스턴스를 생성하여 ApplicationContext가 관리하게 됩니다.
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(ApplicationConfig.class);

        // ApplicationContext가 관리하는 CalculatorService.class타입의 객체를 요청합니다.
        CalculatorService calculatorService = applicationContext.getBean(CalculatorService.class);
        
        // ApplicationContext로 부터 받은 객체를 잉요하여 덧셈을 구합니다.
        System.out.println(calculatorService.plus(10, 50));
    }
}
```

 Main클래스를 실행하면 결과는 60이 출력됩니다.

  

3. **테스트 클래스를 스프링 빈 컨테이너를 사용하도록 수정하기**

기존 테스트 클래스는 테스트할 객체를 `@Before`가 붙은 메소드에서 초기화 하였습니다. 스프링 빈 컨테이너를 사용할 때는 개발자가 직접 인스턴스를 생성하면 안됩니다.

 **스프링 빈 컨테이너가 빈을 생성하고 관리**하도록 하고, **그 빈을 테스트 해야합니다.** 이를 위해서 스프링 프레임워크는 몇가지 특별한 기능을 제공합니다. 소스 코드를 아래와 같이 수정하도록 합니다.

```java
package org.edwith.webbe.calculatorcli;


import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {ApplicationConfig.class})
public class CalculatorServiceTest {
    @Autowired
    CalculatorService calculatorService;

    @Test
    public void plus() throws Exception{
        int value1 = 10;
        int value2 = 5;

        int result = calculatorService.plus(value1, value2);

        Assert.assertEquals(result, 15); // 결과와 15가 같을 경우에만 성공
    }

    @Test
    public void divide() throws Exception{
        int value1 = 10;
        int value2 = 5;

        int result = calculatorService.divide(value1, value2);

        Assert.assertEquals(result, 2); // 결과와 15가 같을 경우에만 성공
    }

    @Test
    public void divideExceptionTest() throws Exception{
        int value1 = 10;
        int value2 = 0;

        try {
            calculatorService.divide(value1, value2);
        }catch (ArithmeticException ae){
            Assert.assertTrue(true); // 이부분이 실행되었다면 성공
            return; // 메소드를 더이상 실행하지 않는다.
        }
        
        Assert.assertTrue(false); // 이부분이 실행되면 무조건 실패다.
    }
}
```

기존 테스트 클래스 위에 `@RunWith(SpringJUnit4ClassRunner.class)`를 붙입니다. `@RunWith` 어노테이션은 `JUnit`이 제공하는 어노테이션입니다.

JUnit은 확장기능을 가지는데, 스프링에서는 `JUnit`을 확장하도록 `SpringJUnit4ClassRunner.class`를 제공합니다. 해당 코드는 `JUnit`이 테스트 코드를 실행할 때 스프링 빈 컨테이너가 내부적으로 생성되도록 합니다.



`@ContextConfiguration(classes = {ApplicationConfig.class})`은 내부적으로 생성된 스프링 빈 컨테이너가 사용할 설정파일을 지정할 때 사용합니다. 

위에서 설명한 2줄이 테스트 클래스 위에 있으면, **테스트 클래스 자체가 빈(Bean)객체가 되어 스프링에서 관리**되게 됩니다. 



```java
@Autowired
CalculatorService calculatorService;
```

`CalcultorServiceTest` 클래스가 빈으로 관리되면서, 스프링 빈 컨테이너는 `CalculatorService`를 **주입(Inject)**할 수 있게 됩니다. 이렇게 주입된 클래스를 테스트하면 됩니다.

테스트 결과는 기존의 클래스와 같은 것을 확인할 수 있습니다.



---

참고 : https://www.boostcourse.org/web326/lecture/58977?isDesc=false