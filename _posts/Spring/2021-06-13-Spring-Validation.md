---
title: "Spring : 유효성 검사(Validation) 방법"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Validation"
toc: true
toc_sticky: true
toc_label: 목차
---

# 유효성 검사(Validation) 방법

**Validation이란 어떤 데이터의 값이 유효한지, 타당한지 확인하는 것을 의미**합니다. 예를 들어, 이메일 주소 양식은 admin@example.com인데, 회원 가입을 할 때 이메일 양식이 일치하지 않으면 유효하지 않은 이메일이므로 회원 가입을 막을 수 있습니다.

UI에서 javascript로 "이메일 양식이 일치하지 않는다"는 것은 UX 측면에서 사용자에게 편의를 주기 위함입니다. 사용자가 잘못 입력하여 오타가 발생 했으니 다시 한 번 확인을 하라는 의미가 강합니다. 즉, UI단에서 유효성 검사를 하는 것은 보안적인 측면에서 아무 효과가 없다는 것입니다.

**보안적인 측면에서 유효성 검사란 올바르지 않은 데이터가 서버로 전송되거나, DB에 저장되지 않도록 하는 것**입니다.



### 의존성 설정

```xml
<dependency>
	    <groupId>org.hibernate</groupId>
	    <artifactId>hibernate-validator</artifactId>
	    <version>6.1.0.Final</version>
</dependency>
```

의존성을 추가하는 부분을 몇 개 검색해보면 `validationa-api`도 추가해주는 것을 볼 수 있는데, **hibernate-validator의존성을 추가하면  validationa-api의존성도 함께 추가되므로 validation-api 의존을 생략해도 됩니다.**



### 기본적인 사용법

기본적으로 유효성 검사는 `@Valid` 또는 `@Validated` 어노테이션을 이용할 수 있습니다. 다만, 두 어노테이션만으로는 유효성을 검사할 수 없고, 클래스에 여러가지 제약조건에 관한 어노테이션을 추가해야 `@Valid, @Validated`가 붙은 객체를 제약조건 어노테이션들을 이용하여 유효성 검사를 해주는 것입니다. `@Valid, @Validated `어노테이션은 기본적으로 기능이 같지만, `@Validated`는 **필드에다 제약조건에 대한 그룹을 만들어 적용**시킬 수 있습니다.

또, `BindingResult` 객체를 파라미터에 넣어주면 유효성 검사에 실패할 때, 에러 내용을 `BindingResult`객체에 바인딩할 수 있습니다. 즉, `BindingResult`를 이용해 유효성 검사에 실패했을 경우에 따른 다양한 처리를 해줄 수 있고, **DispatcherServlet이 BindingResult를 JSP에 넣어주기 때문**에 JSP에서 유효성 검사에 실패했을 경우를 처리해줄 수 있습니다.

> 만약 `@Valid, @Validated`를 사용 중 유효성 검사에 실패할 때, 컨트롤러 메서드가 BindingResult 객체를 받지 않는다면 **검증에러 발생 시 MethodArgumentNotValidException**을 내보냅니다.

**참고 링크**

[victolee93님 블로그(JSP에서 BindingResult 사용하기)](https://victorydntmd.tistory.com/179#recentEntries)



### 제약조건 어노테이션

|             아노테이션              |                             설명                             |
| :---------------------------------: | :----------------------------------------------------------: |
|          **@AssertFalse**           |                 값이 false 인지 확인합니다.                  |
|           **@AssertTrue**           |                  값이 true 인지 확인합니다.                  |
|              **@Null**              |                  값이 null인지 확인합니다.                   |
|            **@NotNull**             |                값이 null이 아닌지 체크합니다.                |
|            **@NotEmpty**            |            값이 null도 빈것도 아닌지 확인합니다.             |
|            **@NotBlank**            | 값이 null이 아니고, 트림 된 길이가 0보다 큰지 확인합니다. @NotEmpty와의 차이점은 이 제약은 CharSequence에만 적용되고 트림된다는 것입니다. |
|             **@Email**              | 지정된 문자 시퀀스가 유효한 전자 메일 주소인지 여부를 확인합니다. 선택적 매개 변수 regexp 및 flags를 사용하면 전자 메일과 일치해야하는 추가 정규 표현식 (정규식 플래그 포함)을 지정할 수 있습니다. |
|          **@Max(value=)**           |       값이 지정된 최대값보다 작거나 같은지 확인합니다.       |
|          **@Min(value=)**           |       값이 지정된 최소값보다 크거나 같은지 확인합니다.       |
|        **@Size(min=, max=)**        |    값이 min=과 max= 사이에 있는지 확인합니다.(경계 포함)     |
|            **@Positive**            |                  값이 양수 인지 확인합니다.                  |
|         **@PositiveOrZero**         |              값이 양수이거나 0인지 확인합니다.               |
|            **@Negative**            |                  값이 음수인지 확인합니다.                   |
|         **@NegativeOrZero**         |              값이 음수이거나 0인지 확인합니다.               |
|              **@Past**              |                값이 과거일자인지 확인합니다.                 |
|         **@PastOrPresent**          |             값이 과거 또는 현재인지 확인합니다.              |
|             **@Future**             |                 날짜가 미래인지 확인합니다.                  |
|        **@FutureOrPresent**         |            날짜가 현재 또는 미래인지 확인합니다.             |
|    **@Pattern(regex=, flags=)**     | 주어진 플래그 매치를 고려하여 값이 정규식 regex와 일치하는지 검사합니다. |
| **@DecimalMax(value=, inclusive=)** | inclusive=false일때 지정된 최대값 보다 작은지 확인합니다. inclusive=true 이면 값이 지정된 최대값보다 작거나 같은지 확인합니다. 매개변수 값은 BigDecimal 문자열 표현에 따라 최대값의 문자열 표현입니다. |
| **@DecimalMin(value=, inclusive=)** | 값이 inclusive=false일때 지정된 최소값 보다 큰지 확인합니다. inclusive=true 이면 값이 지정된 최소값보다 크나 같은지 확인합니다. 매개변수 값은 BigDecimal 문자열 표현에 따라 최소값의 문자열 표현입니다. |
|  **@Digits(integer=, fraction=)**   | 값이 integer=와 fraction=에 의해 지정된 자리수의 숫자인지 확인합니다. |





## @Vaild

Post요청 시 `@RequestBody`를 통해 데이터의 유효성을 검사하는 경우를 예시로 들겠습니다.



### UserDTO.java

```java
public class UserDTO {
	@NotNull
    private Long id;
    @NotEmpty(message = "fail")
    private String password;
    private String name;
    
	// Getter, Setter 생략
}
```

DTO클래스에 제약조건 어노테이션을 붙여줍니다. **message속성을 이용해서 에러가 발생했을 때의 message를 커스터마이징**할 수 있습니다.



### ValidController.java

```java
@RestController
@RequestMapping("/valid")
public class ValidController {

	@PostMapping
	public void valid(@RequestBody @Valid UserDTO userDTO, BindingResult result) {
		
		if(result.hasErrors()) {
			result.getAllErrors()
				.forEach(objectError -> {
					System.err.println("code : " + objectError.getCode());
					System.err.println("message : " + objectError.getDefaultMessage());
					System.err.println("name : " + objectError.getObjectName());
				});
		}
	}
}
```

위치와 상관없이 유효성을 검사할 객체에 `@Vaild`를 붙여주면 됩니다. `@RequestBody` 앞에 붙여도 됩니다.

`BindingResult`에서 모든 에러정보를 얻으려면 `getAllErrors()`메소드를 호출해야 합니다. `getAllErrors()`는 에러정보를 `List<ObjectError>`에 담아 Return 하는데, 위의 경우에선 List를 Stream으로 에러정보를 콘솔에 출력해주었습니다. 기타 메소드들은 공식 문서를 참고해주시기 바랍니다.

**참고 링크**

[BindingResult 공식 문서](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/BindingResult.html)



### 처리순서

1. `@Valid`가 선언되어 있기 때문에 UserDTO 클래스의 제약조건 어노테이션(`@Notnull, @NotEmpty`)에 따라 데이터의 유효성 검사를 진행합니다.
2. 모든 필드의 데이터가 유효하다면 문제없이 작성된 로직을 진행합니다.
3. 데이터가 유효하지 않는 필드가 있으면 그에 대한 에러 정보를 `BindingResult`변수에 담는다.
   - **BindingResult에 값이 있다면 유효성 검사에 실패한 것**이기 때문에 실패한 경우에 실행할 로직을 만들어줄 수 있습니다.



### DTO가 다른 객체를 가지는 경우

만약 **DTO안에 멤버 필드의 타입이 primitive type 이나 String 클래스 외에 또 다른 클래스가 있을 때** 해당 클래스를 `Validation`하려면, 아래와 같이 **또 다른 클래스에 @Valid를 적용하고 해당 클래스에서 다시 annotation을 적용**하면 됩니다.

```java
public class UserDTO {
	@NotNull
    private Long id;
    @NotEmpty(message = "fail")
    private String password;
    private String name;
    @Valid
    private Address address;
}

public class Address {
	@NotEmpty
	private String address;
	@NotNull
	private int addressNum;
}
```





## 테스트 코드

테스트는 `Validator를 사용한 객체의 유효성을 검사하는 테스트`와 `MockMvc를 사용한 Controller로직을 검증하는 테스트`가 있습니다.



### 객체의 유효성 검사 (Validator 사용)

```java
@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration
@ContextConfiguration(classes = {WebMvcContextConfiguration.class, ApplicationConfig.class})
public class ValidTest {
	
	private static Validator validator;
	private static ValidatorFactory factory;
	
	@BeforeClass
	public static void init() {
		factory = Validation.buildDefaultValidatorFactory();
		validator = factory.getValidator();
	}
		
	@Test
	public void validFailTest() throws Exception {
		UserDTO user = new UserDTO();
		user.setName("name");
		
		Set<ConstraintViolation<UserDTO>> set = validator.validate(user);
		
		for(ConstraintViolation<UserDTO> v : set)
			System.out.println(v.getMessage());
	}
}
```

**객체와 @Vaild를 테스트하고 싶을 때 사용**합니다. `ValidatorFactory`의 `getValidator()`로 `Validator`객체를 받아 사용해야합니다. `ValidatorFactory, Validator`는 `@BeforClass`를 이용하여 테스트 클래스가 실행되기 전에 초기화 해주어야 합니다.

`Validator`의 `validate()`에 유효성 검사를 진행할 객체를 넣어주었을 때, 유효성 검사에 실패하게 되면, `Set<ConstraintViolation<검사할 객체>>`에 에러정보를 담아 Return하게 되고, `ConstraintViolation<검사할 객체>`를 이용해 에러정보를 처리할 수 있습니다.

**참고 링크**

[`ConstraintViolation<T>` 공식 문서](https://docs.oracle.com/javaee/7/api/javax/validation/ConstraintViolation.html)



### Controller로직 검증 (MockMvc 사용)

```java
@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration
@ContextConfiguration(classes = {WebMvcContextConfiguration.class, ApplicationConfig.class})
public class ValidTest {
	@Autowired
	public ValidController validController;
	
	private MockMvc mockMvc;
	
	@Before
	public void createController() {
		MockitoAnnotations.initMocks(this);
		mockMvc = MockMvcBuilders.standaloneSetup(validController).build();
	}
	
	@Test
	public void validFailTest() throws Exception {
		UserDTO user = new UserDTO();
		user.setName("name");
				
		ObjectMapper mapper = new ObjectMapper();
		this.mockMvc.perform(post("/valid")
				.characterEncoding("UTF-8")
				.content(mapper.writeValueAsString(user))
				.contentType(MediaType.APPLICATION_JSON))
				.andExpect(status().isOk());
	}
}
```

**Http와 같이 Controller로직을 검증하고 싶을 때 사용**합니다. Controller를 주입받고 `mockMvc`를 해당 Controller로 `build()`해줍니다. 

**ValidController에서 Post요청 시 유효성 검사를 진행**하기 때문에 Json을 전송해 주어야 하는데, `ObjectMapper`의 `writeValueAsString()`에 객체를 넣으면 객체의 필드와 필드의 값을 Json으로 변환해주기 때문에 간단하게 사용할 수 있습니다.

#### 주의할 점

- Json을 전송할 때 `body = <no character encoding set>`처럼 body를 인식하지 못할 수 있기 때문에 `.characterEncoding("UTF-8")`를 넣어주어야 합니다.
- `perform(post("/valid")`에서 `post()`는 `MockMvcRequestBuilders.post()`입니다. 여기서는 간단하게 사용하기 위해 import static을 사용하였습니다.
  - `import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;`

- `javax.validation.ValidationException: HV000183: Unable to initialize 'javax.el.ExpressionFactory'.`에러가 발생하면 아래의 라이브러리 의존성을 추가합니다.

```xml
<dependency>
    <groupId>javax.el</groupId>
    <artifactId>javax.el-api</artifactId>
    <version>3.0.0</version>
</dependency>
<dependency>
    <groupId>org.glassfish</groupId>
    <artifactId>javax.el</artifactId>
    <version>3.0.0</version>
</dependency>
```

**참고 링크**

[MockMvc를 이용한 WebAPI 테스트](https://dev-splin.github.io/spring/Spring-MockMVC-WebAPI-Test/)





## @Validated

`@Validated` 어노테이션은 기본적으로 `@Valid`와 기능이 같지만, **필드에다 제약조건에 대한 그룹을 만들어 적용**시킬 수 있습니다.

`@Valid`는 제약조건을 달아놓은 속성에 대해 전부 유효성 검사를 하게 되는데, 만약 제약조건 중 **원하는 속성만 유효성 검사를 하고싶은 경우**에 사용하는 것이 `@Validated` 입니다.

원하는 속성들만 유효성 검사를 하려면 먼저 그룹핑을 해놓아야 하는데, 필드에 대한 제약조건 그룹핑은 **메소드 선언이 없는 마커 인터페이스**를 이용합니다.



### ValidationGroups.java

```java
public class ValidationGroups {
	public interface group1 {}
	public interface group2 {}
}
```

`ValidationGroups`클래스에 메소드 선언이 없는 마커 인터페이스 `group1, group2`를 만듭니다. 해당 그룹에 원하는 필드를 그룹핑할 수 있습니다.



### UserDTO.java

```java
public class UserDTO {
	@NotNull(groups = {ValidationGroups.group1.class})
    private Long id;
    @NotEmpty(message = "fail", groups = {ValidationGroups.group2.class})
    private String password;
    private String name;
    
	// Getter,Setter 생략
}
```

기존의 UserDTO의 필드를 `groups`속성을 이용해 그룹핑합니다.



### ValidController.java

```java
@RestController
@RequestMapping("/valid")
public class ValidController {

	@PostMapping
	public void valid(@RequestBody @Validated(ValidationGroups.group1.class) UserDTO userDTO, BindingResult result) {
		
		if(result.hasErrors()) {
			result.getAllErrors()
				.forEach(objectError -> {
					System.err.println("code : " + objectError.getCode());
					System.err.println("message : " + objectError.getDefaultMessage());
					System.err.println("name : " + objectError.getObjectName());
				});
		}
	}
}
```

`@Validated`를 선언하고 괄호를 열어 원하는 그룹을 넣어주면 해당 그룹에 속해있는 필드만 유효성 검사를 진행하게 됩니다.(테스트 코드를 그대로 사용할 수 있습니다.)  `@Valid`와 마찬가지로 위치는 상관없습니다. 

#### 주의할 점

- `@Validated`에 그룹을 넣어주지 않으면 Controller에서 유효성 검사를 하지 않습니다. 





## @PathVariable, @RequestParam에서의 유효성 검사

`@PathVariable, @RequestParam`에서 유효성 검사를 하는 방법에 대해 알아보겠습니다. 



### MethodValidationPostProcessor Bean 등록

```java
	@Bean
    public MethodValidationPostProcessor methodValidationPostProcessor() {
         return new MethodValidationPostProcessor();
    }
```

먼저 `@PathVariable, @RequestParam`에서 유효성 검사를 하려면 `MethodValidationPostProcessor`를 Bean으로 등록하여야 합니다.



### ValidController.java

```java
@RestController
@RequestMapping("/valid")
@Validated
public class ValidController {
	
	@GetMapping("/{id}")
	public String parameterValid(@PathVariable @Email String id) {
		return id;
	}
}
```

Controller클래스에 `@Validated`를 붙여주고, 유효성 검사를 진행할 `@PathVariable, @RequestParam`에 제약조건 어노테이션을 붙여주면 됩니다.



### 테스트 코드

```java
	@Test
	public void parameterValidTest() throws Exception {
		
		this.mockMvc.perform(get("/valid/hello")
				.characterEncoding("UTF-8")
				.contentType(MediaType.APPLICATION_JSON))
				.andDo(print());
	}
```

MockMvc방식으로 간단하게 테스트해보면, `ConstraintViolationException` 예외를 발생시키는 것을 알 수 있습니다.

#### 주의할 점

- Controller 클래스에 `@Validated`를 붙여주게 되면, `BindingResult`에 에러정보를 넣어 사용하던 메서드에서도 Exception을 발생시키기 때문에 주의하여야 합니다.
  - 구글링해보니 **@PathVariable과 BindingResult는 같이 사용할 수 없다**고 합니다.
  - 이런 경우에는 `@ExceptionHandler`를 통해 예외를 제어해주어야 합니다.



### ConstraintViolationException과 MethodArgumentNotValidException의 차이점

`@Vaild`를 이용하다 발생한 Exception은 `MethodArgumentNotValidException`입니다. 그런데 같은 제약조건을 처리하는데 왜 `@PathVariable`에서 제약조건 어노테이션(`@Email`)을 이용하면 왜 `ConstraintViolationException`이 발생할까요? 공식 문서를 보면 각 Exception에 대한 설명은 아래와 같습니다.

- `ConstraintViolationException` : 제약조건 위반의 결과를 보고합니다.
- `MethodArgumentNotValidException` : @Valid 주석이 달린 인수에 대한 유효성 검사가 실패하면 예외가 발생합니다. 5.3부터 BindException을 확장합니다.

유효성 검사 시 예외가 발생하는 상황에서 `@Valid`를 사용하지 않았으면 `ConstraintViolationException`, 사용했다면 `MethodArgumentNotValidException`가 발생하는 것 입니다.

> 확인 결과, @Validated도 마찬가지로 `MethodArgumentNotValidException`이 발생합니다.





## Custom Constraint annotation

제약조건 어노테이션으로 모든 제약조건을 설명할 수 있는 것이 아니기 때문에 **필요한 제약조건을 직접 커스터마이징**할 수 있습니다. 

예시를 위해 UserDTO의 name필드에 Splin을 제외한 다른 이름은 올 수 없는 제약조건 어노테이션을 만들어 보겠습니다.



### NameConstraint.java(Custom Constraint annotation)

```java
@Documented
@Constraint(validatedBy = NameConstraintValidator.class)
@Retention(RUNTIME)
@Target({ TYPE, FIELD, PARAMETER })
public @interface NameConstraint {
	String message() default "Not Exist Name";
	Class<?>[] groups() default {};
	Class<? extends Payload>[] payload() default {};
}
```

`@interface`를 사용해 `Custom annotation`을 만들 수 있습니다. 즉, `@NameConstraint`라고 사용할 수 있는 것입니다. 

`@interface`위에 붙은 어노테이션을 메타데이터 어노테이션이라고 합니다.



#### Metadata Annotation

메타데이터는 데이터를 위한 데이터입니다. 즉, **어떤 데이터의 구조화된 정보를 분석, 분류하고 부가적 정보를 추가하기 위해 그 데이터 뒤에 함께 따라가는 정보**를 말합니다. 여기에서 사용된 메타데이터를 간단하게 설명하자면 아래와 같습니다.

- `@Documented` : 해당 어노테이션을 사용하는 클래스가 javadoc와 같이 문서화 될 때, 해당 어노테이션이 적용되었음을 명시하도록 합니다.
- `@Constraint(validatedBy = Class<T>(여기서는 NameConstraintValidator.class))` : 유효성 검사에 사용될 `Validator`를 설정합니다.
- `@Retention(RUNTIME)` : 어노테이션의 라이프 사이클을 설정합니다.
  - `RetentionPolicy.SOURCE` : 소스 코드(.java)까지 남아있습니다.
  - `RetentionPolicy.CLASS` : 클래스 파일(.class)까지 남아있습니다.(=바이트 코드)
    - class 파일만 존재하는 라이브러리 같은 경우에도 타입체커, IDE 부가기능 등을 사용할수 있으려면 CLASS 정책이 필요
  - `RetentionPolicy.RUNTIME` : 런타임까지 남아있습니다.(=사실상 안 사라짐)
  - 여기에서는 import static으로 `RetentionPolicy`를 선언했습니다.
- `@Target({ TYPE, FIELD, PARAMETER })` : 어노테이션이 생성될 수 있는 위치를 지정합니다. (enum으로 된 ElementType의 필드)
  - 여기에서는 import static으로 `ElementType`을 선언했습니다.



#### Constraints Annotation을 만들기 위해서는 3가지 속성

- `message` : 제약 조건에 만족하지 못하는 경우 반환하는 기본 에러 메시지입니다.
- `groups` : 제약 조건 그룹을 지정하는 것으로 추후에 검사 순서를 지정하는 것과 연관이 있습니다. 기본으로 Class<?> 배열로 정의해야 합니다.
- `payload` : client에서 payload 객체를 위해 쓰입니다.

> payload : 전송되는 데이터를 의미합니다. 데이터를 전송할 때, 헤더와 메타데이터, 에러 체크 비트 등과 같은 다양한 요소들을 함께 보내 데이터 전송의 효율과 안정성을 높히게 됩니다. 이 때, **보내고자 하는 데이터 자체**를 payload라고 합니다.



### NameConstraintValidator.java(Validator)

```java
public class NameConstraintValidator implements ConstraintValidator<NameConstraint, String> {
	public static final Set<String> names = new HashSet<>(Arrays.asList("Splin"));

	@Override
	public boolean isValid(String value, ConstraintValidatorContext context) {
		return value != null && names.contains(value);
	}

}
```

`@NameConstraint`를 처리해주려면 반드시 `ConstraintValidator`를 구현해주어야 합니다. 

`ConstraintValidator<NameConstraint, String>`를 보면, **NameConstraint에서 String 객체에 대한 유효성 검사를 진행할 것임을 설정**했고, `isValid` 메서드에서 유효성 검사를 진행합니다.

이렇게 Validator까지 만들어 주게 되면, 아래와 같이 UserDTO에서 사용할 수 있고, Splin이 아닌 다른 값을 넣어주면 에러가 발생하게 됩니다.

```java
public class UserDTO {
	@NotNull
    private Long id;
    @NotEmpty(message = "fail")
    private String password;
    @NameConstraint
    private String name;
    @Valid
    private Address address;
}
```



---

참조 : [https://bbbicb.tistory.com/43](https://bbbicb.tistory.com/43)

[https://woowacourse.github.io/javable/post/2020-09-20-validation-in-spring-boot/](https://woowacourse.github.io/javable/post/2020-09-20-validation-in-spring-boot/)

[https://jeong-pro.tistory.com/203](https://jeong-pro.tistory.com/203)

[https://velog.io/@damiano1027/Spring-Valid-Validated%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%9C%A0%ED%9A%A8%EC%84%B1-%EA%B2%80%EC%A6%9D](https://velog.io/@damiano1027/Spring-Valid-Validated%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%9C%A0%ED%9A%A8%EC%84%B1-%EA%B2%80%EC%A6%9D)

[https://victorydntmd.tistory.com/179](https://victorydntmd.tistory.com/179)

[https://otrodevym.tistory.com/entry/Spring-%EC%9C%A0%ED%9A%A8%EC%84%B1-%EA%B2%80%EC%82%AC](https://otrodevym.tistory.com/entry/Spring-%EC%9C%A0%ED%9A%A8%EC%84%B1-%EA%B2%80%EC%82%AC)

[https://knight76.tistory.com/entry/javaxvalidationValidationException-HV000183-Unable-to-initialize-javaxelExpressionFactory-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0](https://knight76.tistory.com/entry/javaxvalidationValidationException-HV000183-Unable-to-initialize-javaxelExpressionFactory-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)

[https://m.blog.naver.com/varkiry05/222058714706](https://m.blog.naver.com/varkiry05/222058714706)

[https://velog.io/@kwj1270/%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98#documented](https://velog.io/@kwj1270/%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98#documented)

