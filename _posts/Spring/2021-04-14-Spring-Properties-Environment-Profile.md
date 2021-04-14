---
title: "Spring : Properties / Environment / Profile"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Properties"
  - "Spring : Environment"
  - "Spring : Profile"
toc: true
toc_sticky: true
toc_label: 목차
---

# Properties / Environment / Profile



## Properties

`Properties`파일은 보통 값을 유동적으로 사용하기 위해 많이 사용합니다. **파일 형식은 `key=value`형식**으로 이루어져 있습니다. 만약 **파일내에 키값이 중복되면 마지막에 읽혀진 것이 출력**됩니다. 또, **여러 프로퍼티들은 우선 순위가 존재**합니다.

예를 들어 시스템 프로퍼티와 환경 변수 프로퍼티가 있고 시스템 프로퍼티가 우선순위가 더 높다고 가정했을 때, 환경 변수에 있는 이름이 `JAVA_HOME`인 환경변수의 값을 구하는 경우 먼저 시스템 프로퍼티 `PropertySource`로 부터 프로퍼티 값을 찾습니다.



**db.properties**

```properties
db.driver=com.mysql.Driver
db.jdbcUrl=jdbc:mysql://host/test
db.user=madvirus
db.password=bkchoi
```

`Properties`파일의 키로 이루어진 값들을 다양한 방법을 이용해 `Spring`에서 사용할 수 있습니다.



## Environment

`Environment`는 외부에서 입력한 정보(`Properties` 같은)를 이용해서 설정값을 변경하는 방법들을 제공합니다. `Environment`는 `Spring Framework`가 자동으로 `inject`하는 `Bean`입니다. `@PropertySource`로 `Property` 파일을 `Environment`로 로딩할 수 있습니다.



### 기능

`Environment`는 두가지의 기능을 제공합니다.

- 프로퍼티 통합관리
  - `시스템 환경변수 , JVM 시스템 프로퍼티 , 프로퍼티 파일 등의 프로퍼티`를 `PropertySource`라는 것으로 통합관리 합니다.
  - **설정 파일이나 클래스 수정없이 시스템 프로퍼티나 프로퍼티 파일 등을 이용해서 설정 정보의 일부를 변경** 할 수 있다.
- 프로필을 이용해서 선택적으로 설정 정보를 사용할 수 있는 방법을 제공
  - 이 기능을 사용하면 **개발환경, 통합 테스트 환경, 실 서비스 환경에 따라 서로 다른 스프링 빈 설정을 선택**할 수 있습니다.
  - **서로 다른 환경을 위한 설정 정보를 편리하게 관리**할 수 있습니다.

 

#### Environment 구하기

`org.springframework.core.env.Environment`를 직접 사용할 일이 많지않지만, **코드에서 프로필을 선택하거나 Environment에 새로운 PropertySource를 직접 추가해주어야 한다면 `ConfigurableApplicationContext`에 정의된 `getEnvironment()`메서드를 이용해서 `Environment`를 구할 수 있습니다.**

```java
import org.springframework.core.env.ConfigurableEnvironment;
  ...
  ConfigurableApplicationContext context = new AnnotationConfigApplicationContext();
  ConfiguralbeEnvironment environment = context.getEnvironment();
  environment.setActiveProfiles("dev");
```

`getEnvironment()`메서드가 리턴하는 타입은 `Environment`의 하위 타입인 `org.springframework.core.env.ConfigurableEnvironment` 타입입니다. **`ConfigfurableEnvironment` 타입은 사용할 프로필을 선택하는 기능과 `PropertySource`를 추가하는데 필요한 기능을 제공**합니다.



`Spring`은 `Spring 3.1` 에서 부터 `Environment interface`를 추가했습니다. **이 Environment (구체적으로 ConfigurableEnvironment)는 아래 세가지 기능을 지원**합니다.

- Profile 에 대한 선택 (active profile)

- Property 에 대한 통합된 접근 제공

- Conversion 서비스에 대한 접근 제공



![img](https://t1.daumcdn.net/cfile/tistory/277696495830265C2F)



#### Properties 사용하기

resources 디렉토리에 `~.properties` 파일을 만들어줍니다.

<img src="https://media.vlpt.us/post-images/max9106/51e7f460-3c44-11ea-9119-39c2819698f6/-2020-01-21-8.50.39.png" alt="img" style="zoom: 50%;" />

---

아래와 같이 `key=value` 꼴로 값을 미리 정해놓을 수 있습니다.
<img src="https://media.vlpt.us/post-images/max9106/0e07bea0-3c45-11ea-8ffc-814f6f5e3208/-2020-01-21-8.55.55.png" alt="img" style="zoom: 67%;" />

---

그 후, `@PropertySource` 라는 어노테이션을 사용해서 이 어플리케이션이 만들어준 `app.properties` 파일을 참조할 수 있도록 합니다.
<img src="https://media.vlpt.us/post-images/max9106/9bd503a0-3c45-11ea-8ffc-814f6f5e3208/-2020-01-21-8.59.34.png" alt="img" style="zoom: 67%;" />

이 `Properties`를 가져올 수 있는 방법 중 2가지를 알아보겠습니다.



##### getProperty

**`Environment` 객체의 `getProperty` 메소드를 이용**해서 `Properties` 값을 가져올 수 있습니다. `.properties` 파일의 `key`에 해당하는 값을 가져옵니다.

<img src="https://media.vlpt.us/post-images/max9106/160253d0-3c46-11ea-8826-e59fef59340a/-2020-01-21-9.02.38.png" alt="img" style="zoom: 67%;" />



##### @Value

`@Value` 어노테이션을 사용해서 값을 가져올 수도 있습니다. `.properties` 파일의 `key`에 해당하는 값을 가져옵니다.

<img src="https://media.vlpt.us/post-images/max9106/21c9c4f0-3c46-11ea-8826-e59fef59340a/-2020-01-21-9.03.01.png" alt="img" style="zoom: 67%;" />

<img src="https://media.vlpt.us/post-images/max9106/43f3f460-3c46-11ea-8826-e59fef59340a/-2020-01-21-9.04.32.png" alt="img" style="zoom: 67%;" />



##### 주의 사항

`Environment` 객체를 주입받아 사용하려 할 때, `null` 값이 들어가는 경우가 있습니다. 이 경우 **해당 클래스가 Bean으로 등록되어 있는 지 확인해보는 것을 추천합니다.**



#### Profile 사용하기

`Profile`은 `Bean`들의 묶음입니다. 예를 들면, A와 B 묶음의 `Profile`이 있다고 했을 때, 테스트 환경에서는 A묶음의 `Bean`들을 쓰고, 실제 배포에서는 B묶음의 `Bean`들을 쓰는 것과 같이 **환경에 따라 다르게 `Bean` 묶음을 사용하는 것**입니다.

`Environmet`는 활성화 할 Profile을 확인하고 설정해 줍니다. 기본적으로는 `Default Profile`로 설정 되어있습니다.

`Default Profile`은 직접 `Profile` 설정하지 않아도 적용되는 `Profile`입니다. 직접 생성한 `Bean`들도 따로 어떤 `Profile`에 들어가는지 설정하지 않으면 전부 `Default Profile`에 속합니다.



`Bean`에 특정 `Profile`을 설정하려면 `@Profile("Profile이름")` 형식으로 지정해 줍니다.

<img src="https://media.vlpt.us/post-images/max9106/26074560-3c37-11ea-aaa0-a3acb80bcd7f/-2020-01-21-7.16.22.png" alt="img" style="zoom:67%;" />

---

또한 클래스가 아닌 메소드 자체에도 사용 가능합니다.

<img src="https://media.vlpt.us/post-images/max9106/eecf7570-3c38-11ea-8e40-051bfb77729e/-2020-01-21-7.28.40.png" alt="img" style="zoom:67%;" />

---

`Profile` 이름을 줄 때, `표현식(!, &, |)`도 사용 가능합니다.
<img src="https://media.vlpt.us/post-images/max9106/1df21bf0-3c39-11ea-8e40-051bfb77729e/-2020-01-21-7.30.27.png" alt="img" style="zoom:67%;" />



##### JVM 설정

이후 이 `Bean`을 사용하려고 하면 실행오류가 납니다. 그 이유는 `test Profile`을 적용해주지 않았기 때문입니다. `test Profile`을 적용하기 위해서는 아래와 같이 해주어야합니다.

`VSCode : 실행 버튼 좌측에 Edit Configuration -> VM options에 -Dspring.profiles.active="Profile명1, Profile명2..."을 입력`

`Eclipe :  프로젝트 오른쪽 클릭 -> Properties -> Maven -> Active Maven Profiles에 컴마(,)로 나누어서 입력`



##### 현재 적용중인 Profile과 default Profile을 확인해볼때 사용하는 코드

<img src="https://media.vlpt.us/post-images/max9106/c76e78d0-3c44-11ea-8ffc-814f6f5e3208/-2020-01-21-8.53.50.png" alt="img" style="zoom: 67%;" />

<img src="https://media.vlpt.us/post-images/max9106/d67fbdc0-3c44-11ea-8ffc-814f6f5e3208/-2020-01-21-8.54.20.png" alt="img" style="zoom: 67%;" />



---

참고 : [https://backback.tistory.com/160](https://backback.tistory.com/160)

[https://velog.io/@max9106/Spring-ApplicationContext%EC%9D%98-%EA%B8%B0%EB%8A%A5%EB%93%A4-jnk5npcm3d](https://velog.io/@max9106/Spring-ApplicationContext%EC%9D%98-%EA%B8%B0%EB%8A%A5%EB%93%A4-jnk5npcm3d)