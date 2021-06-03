---
title: "Spring : Lombok의 사용법 및 주의점"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Lombok"
toc: true
toc_sticky: true
toc_label: 목차
---

#  Lombok의 사용법 및 주의점

**Lombok은 Java 기반 애플리케이션에서 VO,DTO,Entity 등을 보다 쉽게 작성하기 위해 사용되는 라이브러리**입니다. `Lombok`은 `Getter,Setter,ToString` 등을 어노테이션을 이용하여 만들 수 있기 때문에 가독성이 좋습니다.

하지만 특정 Annotation의 무분별한 사용은 오히려 문제가 될수 있고 협업 중이라면 모든 팀원들이 lombok을 설치해야 됩니다.



## 사용법



### 설치 및 의존성 설정

1. [Lombok 다운로드 사이트](https://projectlombok.org/download)에 들어가 Lombok을 설치 해줍니다.
2. 의존성을 주입합니다.

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.20</version>
    <scope>provided</scope>
</dependency>
```



**의존성을 설정해도 사용되지 않는 경우**

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.2</version>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
        <encoding>UTF-8</encoding>
        <!-- 이 부분을 추가 -->
        <annotationProcessors>
            <annotationProcessor>lombok.launch.AnnotationProcessorHider$AnnotationProcessor</annotationProcessor>
        </annotationProcessors>
    </configuration>
</plugin>
```

`maven-compiler-plugin`에 `<annotationProcessors> </annotationProcessors>`부분을 추가해줍니다.



### @Getter / Setter

- `getter`와 `setter`를 생성해줍니다. 
- 클래스와 필드에서  모두 사용 가능합니다. (클래스에 사용하면 static을 제외한 전체 변수에 적용)
- 생성된 메소드들은 기본적으로 **public**입니다.



#### 속성

- `AccessLevel` : 접근제한자를 설정해줄 수 있습니다. 
  - `AccessLevel`은 `PUBLIC, PROTECTED, PACKAGE, PRIVATE`이 있습니다.
  - `AccessLevel.NONE`을 붙이면 해당 필드는 lombok이 메소드를 생성하지 않습니다.



#### 주의할 점

- `Setter` 같은 경우 해당 클래스가 무결성(변경하면 안되는)을 보장해야 하는 경우 사용을 지양해야 됩니다.



```java
@Getter		// 모든 변수에 대해서 Getter를 생성합니다.
public class User {
    private String id;
    @Setter(AccessLevel.PRIVATE)	// password의 Setter가 private으로 생성됩니다.
    private String password;
}
```





### @NonNull

- 변수나 파라미터에 선언하게 되면, 해당 값에 null이 올 수 없습니다.
- Lombok에서 null-check 로직을 자동으로 생성해줍니다.
- `Setter`에 null값이 들어오면 `NullPointException`예외를 일으킵니다.
- 변수에 @NonNull이 달려있으면 해당 변수에 값을 설정하는 메소드들에도 null-check 코드를 생성시킵니다.
- null값이 들어왔을 때 exception이 기본은 `NullPointerException`이 발생하지만 `lombok.nonNull.exceptionType` 설정값을 `IllegalArgumentException`으로 변경할 수 있습니다.



#### 주의할 점

-  Lombok이 생성한 메소드나 생성자에만 효과가 있습니다.

- 불필요하게 branch coverage를 증가시킵니다.



#### 권장하는 방법



- `Guava, Preconditions`로 null을 검증후 오류 처리하는것을 권장합니다. (`Preconditions.checkNotNull`은 branch를 만들지 않습니다.)



```java
public class User{
    @NonNull	// id 값에 null이 올 수 없습니다.
    private String id;
    private String password;
}
```





### @NoArgsConstructor

- parameter가 없는 디폴트 생성자를 생성해줍니다.



#### 속성

- `acces` : `AccessLevel`을 이용하여 접근 제한자를 설정해줄 수 있습니다.



#### 주의할 점

-  기본 생성자를 public(default)로 열어두면 안전성이 심각하게 저하됩니다.
- static 변수들은 스킵됩니다.
- 필드들이 final로 생성되어 있는 경우에는 필드를 초기화 할 수 없기 때문에 생성자를 만들 수 없고 에러가 발생하게 됩니다.
  - `@NoArgsConstructor(force = true)` 옵션을 이용해서 final 필드를 0, false, null 등으로 초기화를 강제로 시켜서 생성자를 만들 수 있습니다. (`@NonNull`같은 제약조건은 무시됩니다.)

- `@NonNull` 같이 필드에 제약조건이 설정되어 있는 경우, 생성자 내 null-check 로직이 생성되지 않습니다.
  - 후에 초기화를 진행하기 전까지 null-check 로직이 발생하지 않는 점을 염두하고 코드를 개발해야 합니다.



#### 권장하는 방법

- `@NoArgsConstructor(access = AccessLevel.PROTECTED)`를 사용하여 객체 생성시 안전성을 보장해주는것을 권장합니다.



```java
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class User{
    @NonNull
    private String id;
    private String password;
}
```





### @AllArgsConstructor와  @RequiredArgsConstructor (사용 지양)

- `@AllArgsConstructor` : 모든 변수를 parameter로 받는 생성자를 생성해줍니다.
- `@RequiredArgsConstructor` : final 혹은 @NonNull인 변수만 parameter로 받는 생성자를 생성해줍니다.



#### 속성

- `acces` : `AccessLevel`을 이용하여 접근 제한자를 설정해줄 수 있습니다.
- `staticName` : 생성자 관련 어노테이션은 모두 사용할 수 있습니다. 생성자를 private으로 생성하고, private 생성자를 감싸고 있는 static factory 메소드를 추가할 수 있습니다. 
  - 하지만, 기본 생성자를 이용하여 생성해 줄 일이 잘 없고 `@AllArgsConstructor`와  `@RequiredArgsConstructor`도 잘 안쓰이기 때문에 사용할 일이 없을 것 같습니다.



#### 주의할 점

```java
@AllArgsConstructor
public class User{
    private String id;
    private String pwd;
}

User user = new User("userId","userPwd");
```

- 위와 같이 초기설정을 했는데, **개발자가 변수의 위치가 마음에 안들어서 바꾸게 된다면, 생성자의 위치도 바뀌지만 입력에는 둘다 String이라 오류가 발생하지 않습니다.** 이를 개발자가 인지하지 못하게 되는 경우가 생길 수 있습니다.
- 그렇기 때문에 `@AllArgsConstructor`, `@RequiredArgsConstructor` 두 어노테이션들은 사용하지 않는것이 좋습니다.



#### 권장하는 방법

```java
public static class User{
    private String pwd;
    private String id;

    @Builder
    public User(String pwd, String id){
        this.pwd = pwd;
        this.id = id;
    }
}

User user = User.builder().pwd("userPwd").id("userId").build();
```

- `@Builder`어노테이션은 순서가 아닌 이름으로 입력받기 때문에 개발자가 실수하는 것을 최대한 방지할 수 있습니다.





### @Builder

- 빌더 패턴을 사용할 수 있도록 코드를 생성



#### 주의할 점

- `@Builder`도 private으로 만들긴 하지만 `@AllArgsConstructor`를 내포합니다.
  - 해당 클래스의 다른 메소드에서 이렇게 자동으로 생성된 생성자를 사용하거나 할 때 문제 소지가 있습니다.



#### 권장하는 방법

- `@Builder`를 Class보다는 **직접 만든 생성자** 혹은 **static 객체 생성 메소드**에 붙이는 것을 권장합니다.
- private 생성자를 구현하여 `@Builder` 를 지정합니다.
  -  `@Builder` 를 Class에 적용시키면 생성자의 접근 레벨이 default이기 때문에, 동일 패키지 내에서 해당 생성자를 호출할 수 있는 문제가 있기 때문입니다.



```java
public class User {
    private String pwd;
    private String id;
    private String name;

    @Builder
    private UserProfile(String pwd, String id, String name) {
        this.pwd = pwd;
        this.id = id;
        this.name = name;
    }
}
```





### @ToString

- `ToString()` 메서드를 생성해줍니다.
- 기본적으로 static을 제외한 전체 변수에 대한 `ToString()`을 생성합니다.



#### 속성

- `exclude` : `ToString()`을 만들 때 제외할 변수를 설정합니다.
- `of` : `ToString()`을 만들 때 포함할 변수를 설정합니다.
- `callSuper` : true로 설정할 경우, 상속받은 클래스의 정보까지 출력합니다. (Default = false)



#### 주의할 점

- JPA 사용 시, 객체가 양방향 영관 관계 일 경우 `@ToString`을 호출하게 되면 무한 순환 참조 발생할 수 있습니다.



#### 권장하는 방법

- `@ToString(exclude={"필드명"})`을 권장합니다.
  - `of`보다 비용이 적음 (새로 객체를 추가해 줄 때마다 수정 비용이 적습니다.)



```java
@ToString(exclude = "password")		// 토큰값, 비밀번호 등과 같은 민감한 정보를 exclude를 통하여 제외할 수 있습니다.
public class User {
    private String id;
    private String password;
    private String Name;
}
```





### @EqualsAndHashCode

- `hashcode()`와 `equals()`를 생성해줍니다.



#### 속성

- `exclude` : `hashcode()`와 `equals()`를 만들 때 제외할 변수를 설정합니다.
- `of` : `hashcode()`와 `equals()`를 만들 때 포함할 변수를 설정합니다.



#### 주의할 점

- 상속받은 클래스가 있다면, `callSuper=true` 명시가 필요합니다.

  

#### 권장하는 방법

- 항상 `@EqualsAndHashCode(of={“변수명”})` 형태로 동등성 비교에 필요한 필드를 명시하는게 좋습니다.
  - 실무에서는 누군가는 이에 대해 실수하기 마련인지라 차라리 사용을 완전히 금지시키고 IDE 자동생성으로 꼭 필요한 필드를 지정하는 것이 나을 수도 있다고 합니다.





### @Data (사용 지양)

- `@Getter, @Setter, @ToString, @EqualsAndHashCode, @RequiredArgsConstructor` 등을 자동으로 생성해줍니다. 



#### 주의할 점

- 무분별하게 `@Getter,@Setter`를 사용하게 되고 `@RequiredArgsConstructor`등을 포함하기 때문에 사용하지 않는게 좋습니다.



#### 권장하는 방법

- `@Data`를 사용하는 것 보다 `@Getter,@Setter` 등 필요한 어노테이션을 각각 선언하는것을 권장합니다.





### @Value (사용 지양)

- Immutable(불변성) 객체를 선언합니다.
- 해당 **어노테이션을 사용할 경우 setter 메소드는 사용이 불가능**합니다.



#### 주의할 점

- `@Value`역시 `@AllArgsConstructor`등을 포함하기 때문에 사용하지 않는게 좋습니다.



#### 권장하는 방법

- Immutable(불변성) 객체를 만들어야 할 때는 직접 만들어 주고 `@Getter`등을 붙여주는 것이 좋습니다.





### lombok.config 설정

Lombok에서도 일부 기능에 대해 사용금지가 필요하다고 느꼈는지, [Configuration System lombok.config](https://projectlombok.org/features/configuration)를 통해 다양한 설정이 가능하게 해두었습니다.

`lombok.config`파일을 작성한뒤 **Proejct root path에 위치**시킵니다

```properties
lombok.Setter.flagUsage = error
lombok.AllArgsConstructor.flagUsage = error
lombok.ToString.flagUsage = warning
lombok.data.flagUsage= error
```

`lombok.{해당어노테이션}.flagUsage = [warning or error]` 이러한 규칙으로 lombok 어노테이션들을 설정 할 수 있습니다.





다른 어노테이션들은 [Lombok 공식 홈페이지](https://projectlombok.org/)에서 확인할 수 있습니다.



---

참고 : [https://ksshlee.github.io/spring/java/lombok/#requiredargsconstructor-%EC%82%AC%EC%9A%A9-%EA%B8%88%EC%A7%80-%EA%B6%8C%EC%9E%A5](https://ksshlee.github.io/spring/java/lombok/#requiredargsconstructor-%EC%82%AC%EC%9A%A9-%EA%B8%88%EC%A7%80-%EA%B6%8C%EC%9E%A5)

[http://wonwoo.ml/index.php/post/1607](http://wonwoo.ml/index.php/post/1607)

[https://kwonnam.pe.kr/wiki/java/lombok/pitfall](https://kwonnam.pe.kr/wiki/java/lombok/pitfall)

[https://velog.io/@dahye4321/%EC%8A%A4%ED%94%84%EB%A7%81-%EA%B0%80%EC%9D%B4%EB%93%9C-2#%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5](https://velog.io/@dahye4321/%EC%8A%A4%ED%94%84%EB%A7%81-%EA%B0%80%EC%9D%B4%EB%93%9C-2#%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5)

[https://dingue.tistory.com/13?category=727003](https://dingue.tistory.com/13?category=727003)

[https://dingue.tistory.com/14?category=727003](https://dingue.tistory.com/14?category=727003)

[https://hyoj.github.io/blog/java/basic/lombok/#allargsconstructor-requiredargsconstructor](https://hyoj.github.io/blog/java/basic/lombok/#allargsconstructor-requiredargsconstructor)
