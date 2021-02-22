---
title: "Spring : 어노테이션을 사용한 스프링 설정"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
toc: true
toc_sticky: true
toc_label: 목차
---

# 어노테이션을 사용한 스프링 설정

​	설정파일을 자바코드의 어노테이션을 이용해서 만드는 방법이다.



## @Configuration

​	**java코드를 xml파일과 같은 스프링 설정파일로** 작용하게 해주는 어노테이션



## @Bean

​	**@Configuration**을 사용한 클래스의 메소드가 **Bean**객체를 나타낸다는 의미로 사용하는 어노테이션



### 예제

​	**일반 Bean객체** 생성

```java
// applicationContext.xml 에 Bean객체들이 선언되어 있다고 가정한다.

// MemberConfig.java
@Configuration	// 클래스앞에 @Configuration 어노테이션을 붙여준다.
public class MemberConfig {
    
    // <bean id="studentDao" class="ems.member.dao.StudentDao" ></bean>
    @Bean	// Bean 객체를 나타낸다
	public StudentDao studentDao() {
		return new StudentDao();
	}

```

---

**생성자**를 이용한 Bean객체 생성

```java
//<bean id="registerService" class="ems.member.service.StudentRegisterService">
//		<constructor-arg ref="studentDao" ></constructor-arg>
//</bean>

	@Bean
	public StudentRegisterService registerService() {
		return new StudentRegisterService(studentDao());
	}
```

---

**setter**를 이용한 Bean객체 생성

```java
//<bean id="dataBaseConnectionInfoDev" class="ems.member.DataBaseConnectionInfo">
//		<property name="jdbcUrl" value="jdbc:oracle:thin:@localhost:1521:xe" />
//		<property name="userId" value="scott" />
//		<property name="userPw" value="tiger" />
//</bean>

	@Bean
	public DataBaseConnectionInfo dataBaseConnectionInfoDev() {
		DataBaseConnectionInfo infoDev = new DataBaseConnectionInfo();
		infoDev.setJdbcUrl("jdbc:oracle:thin:@localhost:1521:xe");
		infoDev.setUserId("scott");
		infoDev.setUserPw("tiger");
		
		return infoDev;
	}
```

---

**list/map**을 이용한 Bean객체 생성

```java
//<bean id="informationService" class="ems.member.service.EMSInformationService">
//			<list>
//				<value>Cheney.</value>
//				<value>Eloy.</value>
//				<value>Jasper.</value>
//				<value>Dillon.</value>
//				<value>Kian.</value>
//			</list>
//		</property>
//		<property name="administrators">
//			<map>
//				<entry>
//					<key>
//						<value>Cheney</value>
//					</key>
//					<value>cheney@springPjt.org</value>
//				</entry>
//				<entry>
//					<key>
//						<value>Jasper</value>
//					</key>
//					<value>jasper@springPjt.org</value>
//				</entry>
//			</map>
//		</property>
//</bean>

	@Bean
	public EMSInformationService informationService() {
		EMSInformationService info = new EMSInformationService();
        
		ArrayList<String> developers = new ArrayList<String>();
		developers.add("Cheney.");
		developers.add("Eloy.");
		developers.add("Jasper.");
		developers.add("Dillon.");
		developers.add("Kian.");
		info.setDevelopers(developers);
		
		Map<String, String> administrators = new HashMap<String, String>();
		administrators.put("Cheney", "cheney@springPjt.org");
		administrators.put("Jasper", "jasper@springPjt.org");
		info.setAdministrators(administrators);
				
		return info;
	}
}
```

---

**main클래스**에서 이 자바 파일을 사용하는 방법

```java
// 기존에 사용하던 방법
//GenericXmlApplicationContext ctx =
//				new GenericXmlApplicationContext("classpath:applicationContext.xml"); 

// @Configuration을 사용한 자바파일을  사용하는 방법 AnnotationConfigApplicationContext라는 클래스를 이용 (나머지 getBean을 이용해 객체를 가져오는 것은 동일)
AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(MemberConfig.class);
```

---



## Java 파일 분리

- Java파일도 xml 파일처럼 용도를 나눠 분리하면 좋다.
- MemberConfig를 용도를 분리해 MemberConfig1, MemberConfig2, MemberConfig3으로 나눴다고 가정



### 용도에 맞게 나눠 각각 불러주는 방법

```java
// main클래스
// 기존에 MemberConfig.java만 넣는 것이 아니고 1,2,3 전부 넣어준다.
AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(MemberConfig1.class, MemberConfig2.class, MemberConfig3.class);
```



### 나눈 파일들을 하나에 모아 하나만 불러주는 방법

```java
// MemberConfigImport.java

// 1번 내용을 적어주고 MemberConfig2, MemberConfig3을 Import 해준다.
@Configuration
@Import({MemberConfig2.class, MemberConfig3.class})	
public class MemberConfigImport {
 	// MemberConfig1 에 있는 내용
}
```

```java
// main 클래스
AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(MemberConfigImportclass);
```



