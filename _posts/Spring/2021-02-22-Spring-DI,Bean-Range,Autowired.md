---
title: "Spring : DI, 빈(Bean)의 범위, 의존객체 자동주입/체크 "
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : DI(Dependency Injection)"
toc: true
toc_sticky: true
toc_label: 목차
---

## DI(Dependency injection)

- 의존 주입
- 어떠한 객체에 의존하지 않고 분리해서 여기저기서 사용하게 할 수 있는것
  - 내장배터리를 사용하는 자동차장난감, 따로 배터리를 사용할 수 있는 로봇 장난감이 있다고 가정
  - 자동차장난감은 배터리가 다 달면 새로 사야하지만 로봇은 배터리만 바꿔 껴주면 된다.
  
  

#### DI 예제
- 성적표 프로그램이 있다고 가정
  - 입력, 출력, 삭제, 업데이트 등의 기능을 가지는 인터페이스 객체가 있다.
- 모든 기능에는 StudentDao(Data Access Ojbect)라는 데이터베이스와 통신을 위한 객체가 필요하다.
- 이 객체를 각각의 기능안에서 만들어주는 것이아니고 하나를 만들어서 각각의 객체들에게 주입해준다.
- 생성자, setter, list, map을 통한 주입이 있다.

```java
// applicationContext.xml 파일안에서 Dao객체를 생성
<bean id="studentDao" class="ems.member.dao.StudentDao"></bean>
```
```java
<bean id="registerService" class="ems.member.service.StudentRegister">
  <constructor-arg ref="studentDao"></constructor-arg>  
</bean>
  // StudentRegister를 만들때 StudentDao를 참조한다. (생성자를 통한 주입)
```

```java
<bean id="registerService" class="ems.member.service.StudentRegister">
  <property name="userId" value="scott" /> 
</bean>
  // setUserId라는 setter가 있다고 가정할 때,
  // setUserId의 set을 빼고 U를 소문자로 바꾼 값을 property name으로 그 인자에 들어갈 값을 value에 적는다 (setter를 이용한 주입)
```

```java
<bean id="registerService" class="ems.member.service.StudentRegister">
   <property name="userId">
      <list>
         <value>cat</value>
         <value>dog</value>
         <value>bird</value>
      </list>
    </property> 
</bean
// setUserId가 list를 인자로 갖는 setter일 때, 
// 마찬가지로 setUserId의 set을 빼고 U를 소문자로 바꾼 값을 property name에 넣은후 리스트의 값을든 <list> <value> 로 넣는다.
```

```java
<bean id="registerService" class="ems.member.service.StudentRegister">
   <property name="userId"> 
      <map>
        <entry>
          <key>
             <value>Yun</value>
          </key>
             <value>ChangHyun</value>
        </entry>
      </map>
    </property>  
</bean>
// setUserId가 map을 인자로 갖는 setter일 때,
// map 키와 값을 <entry>로 묶고 키는 <key>로 묶고 <value>에 키 값을 입력, 값은 <value>에 입력해준다.            
```


## 빈(Bean)의 범위

- 싱글톤(Singleton)
  - 스프링 컨테이너에서 생성된 빈(Bean)객체의 경우 기본적으로 한 개만 생성이 된다.
  - getBean() 메소드로 호출될 때 동일한 객체가 반환된다.
  ![싱글톤](https://user-images.githubusercontent.com/58713853/74734779-9a8f3200-5292-11ea-9177-6bcb5def62f2.PNG)
- 프로토타입(Prototype)
  - 싱글톤 범위와 반대의 개념
  - 스프링 설정 파일에서 빈(Bean)객체를 정의할 때 scope속성을 명시해 주어야한다.

```java
<bean id="dependencyBean" class="scope.ex.DependencyBean" scope="prototype">
```



## 의존객체 자동 주입

- 스프링 설정 파일에서 의존 객체를 주입할 때 <constructor-org> 또는 <property> 태그로 의존 대상 객체를 명시하지 않아도 스프링 컨테이너가 자동으로 필요한 의존 대상 객체를 찾아서 주입해 주는 기능
- @Autowired, @Resource 어노테이션을 이용해서 쉽게 구현할 수 있다.



### @Autowired

- 주입하려고 하는 객체의 타입이 일치하는 객체를 자동으로 주입한다.
- 해당하는 코드위에 @Autowired를 명시해 준다.
- xml파일에 xmlns:context="http://www.springframework.org/schema/context" 네임스페이스 스키마를 명시해준다.
- xml파일에 <context:annotation-config /> 태그를 명시해준다.
- 생성자뿐만 아니라 속성,메소드에도 붙일 수 있다.
  - 속성과 메소드에 @Autowired를 사용할 때는 디폴트 생성자가 반드시 있어야 한다.
  



### @Resource

- @Autowired와 비슷하지만 객체의 타입을 보는 것이 아니라, 객체의 id를 보고 메소드인자나 속성이름과 같은 객체를 주입해준다.
- 생성자에는 사용할 수 없고 속성,메소드에만 사용할 수 있다.
  - 마찬가지로 디폴트 생성자가 반드시 필요하다.



#### @Autowired, @Resource 예제

- appCtxUseAutowired.xml이라는 스프링 설정 파일과 WordDao를 생성자로 갖는 WordRegisterService.java라는 자바파일이 있다고 가정한다.



```java
// appCtxUseAutowired.xml
xmlns:context="http://www.springframework.org/schema/context" 
// Autowired,Resource를 사용하기위한 네임스페이스를 명시해준다.

xsi:schemaLocation= http://www.springframework.org/schema/context 
    http://www.springframework.org/schema/context/spring-context.xsd">
// xsi:schemaLocation=에도 이 네임스페이스를 명시해준다.
// 나머지 네임스페이스는 생략
```



```java
<context:annotation-config /> // 이 태그를 씀으로써 Autowired,Resource를 사용할 수 있다.
<bean id="wordDao" class="com.word.dao.WordDao" />  
// @Resource를 사용하면 WordRegisterService클래스의 wordDao속성과 id가 같은 객체를 주입해주게 된다.
// @Autowired를 사용할 땐, 생성자 인자인 WordDao객체와 타입이 일치하는 객체를 주입해주게 된다.

<bean id="registerService" class="com.word.service.WordRegisterService">
// <constructor-arg ref="wordDao" />  // 기존 생성자에 직접 명시해주는 방법, Autowired,Resource를 사용시 명시해주지 않아도 된다.
```



```java
// WordRegisterService.java
public class WordRegisterServiceUseAutowired {
  private WordDao wordDao;
  
	@Autowired	// @Autowired를 사용할 윗부분에 명시해준다.
	public WordRegisterService(WordDao wordDao) {
		this.wordDao = wordDao;
	}
  ....  
}
```

```java
// WordRegisterService.java
public class WordRegisterServiceUseAutowired {
  @Resource  // @Resource 사용할 속성,메소드 윗부분에 명시해준다.
  private WordDao wordDao;
  
  	public WordRegisterService() {} // 속성,메소드에 @Autowired나 @Resource를 사용하려면 반드시 디폴트 생성자를 만들어 주어야한다.
	public WordRegisterService(WordDao wordDao) {
		this.wordDao = wordDao;
	}
  ....  
}
```



## 의존객체 선택 주입

- @Autowired를 사용할 때, 동일한 객체가 2개 이상인 경우 스프링 컨테이너는 자동주입 대상 객체를 판단하지 못해서 Exception을 발생시킨다.
- 속성이나 메소드,생성자의 인자 이름과 xml에서의 id를 맞춰주면 해당 객체를 주입하게 되지만 그다지 좋은 방법은 아니다.
- @Qualifier를 사용한다.



### @Qualifier

- @Qualifier를 사용해 이름을 명시해줌으로써 어떤 객체를 사용할지 선택할 수 있다.

```java
// WordRegisterService.java
public class WordRegisterServiceUseAutowired {
  private WordDao wordDao;
  
	@Autowired
	@Qualifier("usedDao")	// @Qualifier를 사용해 이름을 명시해준다.
	public WordRegisterService(WordDao wordDao) {
		this.wordDao = wordDao;
	}
  ....  
}
```

```java
<context:annotation-config />

// <bean id="wordDao" class="com.word.dao.WordDao" />  // 생성자의 인자와 id가 같으면 이 객체를 선택하게 되지만 좋은 방법이 아니다.

<bean id="wordDao1" class="com.word.dao.WordDao">  
	<qualifier value="usedDao"/>	// @Qualifier에서 설정한 이름과 맞춰 qualifier 태그를 명시해준다.
</bean>
<bean id="wordDao2" class="com.word.dao.WordDao" />  
<bean id="wordDao3" class="com.word.dao.WordDao" />  

<bean id="registerService" class="com.word.service.WordRegisterService">
```



#### 의존객체 자동주입 체크

- @Autowired를 사용해서 의존객체 자동주입을 할 때, 스프링 설정파일에 해당하는 객체가 없을 경우 사용한다.
	- 초보적인 프로그래밍 실수라 거의 사용하진 않는다.
- @Autowired(required=false)로 명시해준다.

```java
// WordRegisterService.java
public class WordRegisterServiceUseAutowired {
  private WordDao wordDao;
  
	@Autowired(required=false) // xml 스프링 설정파일에 해당 객체타입이 없어도 실행이 되게 해준다.
	public WordRegisterService(WordDao wordDao) {
		this.wordDao = wordDao;
	}
  ....  
}
```


### @inject

- @Autowired와 동일하게 주입하려고 하는 객체의 타입이 일치하는 객체를 자동으로 주입한다.
- @Autowired와 다르게 required 속성을 지원하지 않는다.



#### @Named

- @inject에서 의존객체를 선택할 때 사용하는 어노테이션
- @Qualifier와 동일한 기능을 한다.

```java
// WordRegisterService.java
public class WordRegisterServiceUseAutowired {
  private WordDao wordDao;
  
	@inject			// @Autowired와 같은 기능 (다만, required 속성 없음)
	@Named("usedDao")	// @Named 사용해 이름을 명시해준다.
	public WordRegisterService(WordDao wordDao) {
		this.wordDao = wordDao;
	}
  ....  
}
```
