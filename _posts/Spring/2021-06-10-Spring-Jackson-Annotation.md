---
title: "Spring : Jackson Annotation 사용법"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Jackson"
toc: true
toc_sticky: true
toc_label: 목차
---

#  Jackson Annotation 사용법

jackson은 자바진영 Json 라이브러리로 잘 알려져 있지만 Json 뿐만 아니라 XML, YAML, CSV 등 **다양한 형식의 데이타를 지원하는 data-processing 라이브러리**입니다. 스트림 방식이므로 속도가 빠르며 유연하며, 다양한 third party(제 3자) 데이터 타입을 지원하며 annotation 방식으로 메타 데이타를 기술할 수 있으므로 Json 의 약점 중 하나인 문서화와 데이타 validation(검토) 문제를 해결할 수 있습니다.



클라이언트와 서버 사이에서 Json을 이용할 때, ` Jackson 어노테이션`을 이용하면 편리한 점이 많기 때문에 다양한 `Jackson 어노테이션`에 대해 알아보겠습니다.

여기서 `Serialization(직렬화)와 Deserialization(역직렬화)`란 개념이 나오는데, <br>`Serialization(직렬화)`이란, 자바 객체를 전송가능한 Json형태로 만들어주는 것을 의미하고<br>`Deserialization(역직렬화)`이란, 직렬화한 Json 데이터들을 자바 객체로 변환하는 것을 의미합니다.



### 참고사항

- 간단한 설정을 위해 Lombok을 사용하였습니다.
  - [Lombok의 사용법 및 주의점](https://dev-splin.github.io/spring/Spring-Lombok/)

- 테스트는 ObjectMapper를 이용해 할 수 있는데, 테스트까지 작성하게 되면 글이 너무 길어지기 때문에 아래의 링크에서 어노테이션 별 테스트 코드를 확인해 보실 수 있습니다.
  - [https://www.tutorialspoint.com/jackson_annotations/index.htm](https://www.tutorialspoint.com/jackson_annotations/index.htm)

- 사용법을 간단하게 알아보기 위한 글이기 때문에 자세한 설명이 필요한 어노테이션은 링크를 남겨놓거나 추후에 따로 포스팅하겠습니다.



### 의존성 설정

```xml
<properties>
    <jackson2.version>2.9.4</jackson2.version>
</properties>

<dependencies>
	<!-- jackson -->	
	<!-- RestController의 Json 변환을 위해 필요합니다 -->
	<dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-core</artifactId>
        <version>${jackson2.version}</version>
    </dependency>
    <!-- jackson-core 및 jackson-annotation 라이브러리의 의존성을 포함 -->
	<dependency>
	    <groupId>com.fasterxml.jackson.core</groupId>
	    <artifactId>jackson-databind</artifactId>
	    <version>${jackson2.version}</version>
	</dependency>
	<dependency>
	    <groupId>com.fasterxml.jackson.datatype</groupId>
	    <artifactId>jackson-datatype-jdk8</artifactId>
	    <version>${jackson2.version}</version>
	</dependency>
</dependencies>
```



## Jackson General Annotations(일반 어노테이션)

Jackson의 일반적인 어노테이션입니다.



### @JsonProperty

직렬화, 비직렬화할 때 설정할 수 있는 이름을 지정하는 어노테이션입니다. **멤버필드의 이름이나 필드의 Getter/Setter이름이 Json 키와 일치하지 않을 경우 사용**합니다.

```java
public class Name {
    private String name; 
    
    @JsonProperty("name")
    public void setTheName(String name) {
        this.name = name; 
    } 
    
    @JsonProperty("name") 
    public String getTheName() { 
        return name; 
    }
}
```

```json
// Getter의 이름이 TheName이지만 어노테이션을 이용해 Json 키를 name으로 직렬화합니다.
{
    name : "myName"
}
```



### @JsonFormat

직렬화, 비직렬화할 때 **날짜 / 시간 값을 포멧팅**할 수 있습니다.

```java
public static class Event {
    public String name;

    @JsonFormat(pattern = "dd-MM-yyyy hh:mm:ss")
    public Date eventDate;

    public Event(String name, Date date) {
        this.name = name;
        this.eventDate = date;
    }
}
```

```json
// 적용 전
{
  "name":"party",
  "eventDate":1419042600000
}

//적용 후
{
  "name": "party",
  "eventDate": "20-12-2014 02:30:00"	// 설정한 포멧에 맞춰 직렬화됩니다.
}
```



### @JsonUnwrapped

직렬화, 비직렬화할 때 **Unwrapping되어야 하는 객체를 정의하는 데 사용**됩니다.

```java
public static class UnwrappedUser {
    public int id;

    @JsonUnwrapped
    public Name name;

    public UnwrappedUser(int id, Name name) {
        this.id = id;
        this.name = name;
    }

    public static class Name {
        public String firstName;
        public String lastName;

        public Name(String firstName, String lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }
    }
}
```

```json
// 적용 전
{
  "id":1,
  "name":{
    "firstName":"John",
    "lastName":"Doe"
  }
}

// 적용 후
// name객체의 필드가 Unwrapping됩니다.
{
  "id": 1,
  "firstName": "John",
  "lastName": "Doe"
}
```



### @JsonView

직렬화할 때 필드를 선택적으로 직렬화할 수 있습니다.

- **`MapperFeature.DEFAULT_VIEW_INCLUSION` 옵션을 활성화하면 @JsonView가 명시되지 않은 변수들도 Json에 포함**됩니다. 기본값은 활성화되어 있으므로 이를 원치 않을 경우 비활성화 해주어야 합니다. (`mapper.disable(MapperFeature.DEFAULT_VIEW_INCLUSION);`)
- **`writerWithView()`로 적용할 @JsonView를 명시하면 Jackson은 해당 @JsonView가 명시된 프라퍼티만 Json 문자열에 포함**합니다.

**참고 링크**

[테스트 코드](https://www.tutorialspoint.com/jackson_annotations/jackson_annotations_jsonview.htm)

```java
public static class Views {
    public static class Public {
    }

    public static class Internal extends Public {
    }
}

public static class Item {
    @JsonView(Views.Public.class)
    public int id;

    @JsonView(Views.Public.class)
    public String itemName;

    @JsonView(Views.Internal.class)
    public String ownerName;
}
```

```java
String itme = new ObjectMapper()
        .disable(MapperFeature.DEFAULT_VIEW_INCLUSION)	// MapperFeature.DEFAULT_VIEW_INCLUSION 비활성화
        .writerWithView(Views.Public.class)	// View 설정
        .writeValueAsString(item);
```

```json
// 적용 전
{
  "id":2,
  "itemName":"book",
  "ownerName":"John"
}

// 적용 후
// 테스트 코드에서 Views.Public.class를 설정하면 Views.Public.class로 설정한 필드만 직렬화됩니다.
{
  "id":2,
  "itemName":"book"
}
```



### @JsonManagedReference, @JsonBackReference

Jackson 2.0 버전 이전에 순환 참조를 해결하기 위해서 사용했던 어노테이션입니다. 객체의 **부모(@JsonManagedReference) / 자식(@JsonBackReference) 관계를 명시하여 순환 참조 에러를 방지**합니다.

```java
public static class ItemWithRef {
    public int id;
    public String itemName;

    @JsonManagedReference
    public UserWithRef owner;

    public ItemWithRef(int id, String itemName, UserWithRef owner) {
        this.id = id;
        this.itemName = itemName;
        this.owner = owner;
    }
}

public static class UserWithRef {
    public int id;
    public String name;

    @JsonBackReference
    public List<ItemWithRef> itemWithRefs = new ArrayList<>();

    public UserWithRef(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public void addItem(ItemWithRef item) {
        itemWithRefs.add(item);
    }
}
```

```json
// 적용 전
Infinite recursion (StackOverflowError)... 순환 참조 에러

// 적용 후
{
  "id":2,
  "itemName":"book",
  "owner":{
    "id":1,
    "name":"John"
  }
}
```



### @JsonIdentityInfo

Jackson 2.0 이후부터 새롭게 추가된 어노테이션입니다. 객체의 **부모 / 자식 관계가 있을 때,  id를 설정하여 순환참조에러를 방지**합니다.

**참고 링크**

 [Jackson에서 Infinite Recursion 이슈 해결방법]( https://blog.advenoh.pe.kr/java/Jackson%EC%97%90%EC%84%9C-Infinite-Recursion-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95/)

[테스트 코드](https://www.tutorialspoint.com/jackson_annotations/jackson_annotations_jsonidentityinfo.htm)



### @JsonFilter

직렬화, 비직렬화할 때 **어떤 필드를 사용할지 여부와 같은 필터를 적용하는 데 사용**됩니다.

**참고 링크**

[옵션 설명](https://kwonnam.pe.kr/wiki/java/jackson/jsonfilter)

[Jackson, 커스텀 @JsonFilter로 조건에 맞는 필드만 JSON 변환하기](https://jsonobject.tistory.com/258)

[테스트 코드](https://www.tutorialspoint.com/jackson_annotations/jackson_annotations_jsonfilter.htm)



### @JacksonAnnotationsInside (Custom Jackson Annotation)

`@JacksonAnnotationsInside`어노테이션을 사용해서 **사용자 정의 Jackson 어노테이션**을 만들 수 있습니다. 

`@Retention`는 Coustom Annotation을 만들 때 사용되는데, 어노테이션의 라이프 사이클을 설정합니다.

- `RetentionPolicy.SOURCE` : 소스 코드(.java)까지 남아있습니다.

- `RetentionPolicy.CLASS` : 클래스 파일(.class)까지 남아있습니다.(=바이트 코드)
  - class 파일만 존재하는 라이브러리 같은 경우에도 타입체커, IDE 부가기능 등을 사용할수 있으려면 CLASS 정책이 필요

- `RetentionPolicy.RUNTIME` : 런타임까지 남아있습니다.(=사실상 안 사라짐)

**참고 링크**

[@Retention 어노테이션 정리](https://jeong-pro.tistory.com/234)

```java
@CustomAnnotation
public static class BeanWithCustomAnnotation {
    public int id;
    public String name;
    public Date dateCreated;

    public BeanWithCustomAnnotation(int id, String name, Date dateCreated) {
        this.id = id;
        this.name = name;
        this.dateCreated = dateCreated;
    }
}

@Retention(RetentionPolicy.RUNTIME)	// 어노테이션의 라이프 사이클 설정
@JacksonAnnotationsInside	// Custom Annotation을 만들기 위해 사용
@JsonInclude(JsonInclude.Include.NON_NULL)	// null일 시 직렬화 X
@JsonPropertyOrder({"name", "id", "dateCreated"})	// 직렬화할 시 순서 설정
@interface CustomAnnotation {
}
```

```json
// 적용 전
{
  "id": 1,
  "name": "My bean",
  "dateCreated": null
}

// 적용 후
// @JsonPropertyOrder의 순서대로 필드가 직렬화되었고,
// @JsonInclude의 NON_NULL속성이 null값인 dateCreated를 직렬화하지 않았습니다.
{
  "name": "My bean",
  "id": 1
}
```



### Jackson MixIn

MixIn을 사용하면 소스접근이 불가능한 특정 클래스에 Jackson Annotation 을 적용하거나 혹은 특정 인터페이스/클래스 구현체에 대해 항상 동일한 어노테이션 주입이 가능해집니다. 즉, **대상 클래스를 수정하지 않고 주석을 연결하는 방법**입니다. 

간단하게 설명하자면, **MixIn으로 사용할 클래스를 만들고 사용할 클래스 위에 Jackson Annotatoin을 작성하고 ObjectMapper에서 적용**시키는 것입니다. 때문에 소스접근이 불가능한 클래스에서도 적용할 수 있습니다. (Filter와 함께 사용하면 좋다고 합니다.)

**참고링크**

[테스트 코드](https://www.tutorialspoint.com/jackson_annotations/jackson_annotations_mixin.htm)

[Custom Filter와 MixIn 함께 사용하기](https://goni9071.tistory.com/entry/jackson-fasterxml-custom-filter)

[Jackson을 이용해 추상, 인터페이스 객체 별로 구현체 구분하기](https://seongtak-yoon.tistory.com/70)는 다형성 처리 어노테이션에 관한 글이지만 MixIn을 적용한 부분이 있기 때문에 다형성 처리 어노테이션과 함께 보시면 좋을 것 같습니다.



### Jackson Annotation 비활성화

ObjectMapper를 이용할 때, `MapperFeature.USE_ANNOTATIONS` 옵션을 비활성화 하게 되면 모든 Jackson annotation 비활성화 할 수 있습니다.

```java
@JsonInclude(JsonInclude.Include.NON_NULL)
@JsonPropertyOrder({"name", "id"})
 public static class MyBean {
     public int id;
     public String name;

     public MyBean(int id, String name) {
         this.id = id;
         this.name = name;
     }
 }
```

```java
@Test public void disable_jackson_annotation() {
    ObjectMapper mapper = new ObjectMapper();
    mapper.disable(MapperFeature.USE_ANNOTATIONS);
    // 중략
    // 이 mapper를 사용하면, 어노테이션이 전부 비활성화 됩니다 
}
```

```json
// MapperFeature.USE_ANNOTATIONS 비활성화
{
    "id":1,
    "name":null
}

// MapperFeature.USE_ANNOTATIONS 활성화
{
  "id": 1
}
```



## Jackson Serialization Annotation(직렬화 어노테이션)

**Jackson Serialization Annotation**에서 `Serialization(직렬화)`이란, 자바 객체를 전송가능한 Json형태로 만들어주는 것을 의미합니다.



### @JsonAnyGetter

Map변수를 다루는데 유연성을 제공합니다. **객체를 직렬화할 때 Map의 모든 키 / 값 을 일반 Json**으로 직렬화합니다.

```java
@Getter
@Builder
public static class ExtendableBean {
    public String name;
    private Map<String, String> properties;

    @JsonAnyGetter
    public Map<String, String> getProperties() {
        return properties;
    }
}
```

```json
// 적용 전
{
  "name": "yun",
  "properties": {	// Map의 키/값이 Map의 이름으로 감싸져 있습니다.
    "key1": "value1",
    "key2": "value2"
  }
}

// 적용 후
// Map의 이름으로 감싸져있던 키/값을 일반 Json으로 직렬화할 수 있습니다.
{
  "name": "yun",
  "key1": "value1",		
  "key2": "value2"
}
```



### @JsonGetter

기존 getter의 이름 기반으로 Json의 키값이 정해지는 것 대신, **어노테이션 속성의 값으로 Json의 키값을 설정**합니다.

```java
@Builder
public static class MyBean {
    public int id;
    private String name;

    @JsonGetter("name")
    public String getTheName() {
        return name;
    }
}
```

```json
// 적용 전
{
  "id": 1,
  "theName": "yun"	// 기존 getter의 이름인 theName
}

// 적용 후
// 기존 getter의 이름인 theName 어노테이션 속성이 아닌 명시한 name으로 직렬화합니다.
{
  "id": 1,
  "name": "yun"		
}
```



### @JsonPropertyOrder

**객체를 직렬화할 때의 순서**를 정할 수 있습니다.

```java
@JsonPropertyOrder({"name", "id"})
@Builder
public static class PropertyOrder {
    private long id;
    private String name;
}
```

```json
// 적용 전
// 객체의 멤버 필드 순서에 맞게 Json으로 직렬화합니다.
{
  "id": 1,
  "name": "name"
}

// 적용 후
// @JsonPropertyOrder({"name", "id"})으로 정해준 순서에 맞게 직렬화합니다.
{
  "name": "name",
  "id": 1
}
```



### @JsonRawValue

Jackson이 **필드를 그대로 Json으로 직렬화(이스케이프 문자를 인식)**

```java
@Builder
    public static class RawBean {
        public String name;

        @JsonRawValue
        public String json;
    }
```

```json
// 적용 전
{
  "name": "yun",
  "json": "{\n  \"attr\":false\n}"	// 이스케이프 문자를 그대로 Json으로 직렬화합니다.
}

// 적용 후
// 이스케이프 문자를 인식해서 Json으로 직렬화합니다.
{
  "name": "yun",
  "json": {
    "attr": false
  }
}
```



### @JsonValue

객체를 직렬화할 때 사용하는 단일 메서드를 나타냅니다. (**해당 객체를 직렬화 시, 객체의 이름과 필드 값이 나오는 게 아니라 어노테이션을 붙인 메서드의 값**이 나오게 됩니다.)

```java
public enum TypeEnumWithValue {
    TYPE1(1, "Type A"),
    TYPE2(2, "Type 2");

    private Integer id;
    private String name;

    TypeEnumWithValue(Integer id, String name) {
        this.id = id;
        this.name = name;
    }

    @JsonValue
    public String getName() {
        return name;
    }
}
```

```json
// 적용 전
// enum 객체의 이름으로 직렬화합니다.
"TYPE1"

// 적용 후 
// @JsonValue를 붙여준 enum의 name으로 직렬화합니다.
"Type A"
```



### @JsonRootName

직렬화할 때, **@JsonRootName 속성의 이름으로 감쌉니다(Root Wrapper)**.

```java
@Builder
@JsonRootName(value = "user")
public static class UserWithRoot {
    public int id;
    public String name;
}

//objectMapper.enable(SerializationFeature.WRAP_ROOT_VALUE); 반드시 적용해야함
```

```json
// 적용 전
{
  "id": 1,
  "name": "yun"
}

// 적용 후
// @JsonRootName(value = "user")에서 설정한 이름이로 감싸서 직렬화합니다.
{
  "user": {
    "id": 1,
    "name": "yun"
  }
}
```



### @JsonSerialize

**StdSerializer를 상속받아 직렬화를 어떻게 할지 커스터마이징**할 수 있습니다. 내용은 링크로 대체하겠습니다.

**참고 링크**

[jackson custom serializer, deserializer 만들기](https://multifrontgarden.tistory.com/172)

[테스트 코드](https://www.tutorialspoint.com/jackson_annotations/jackson_annotations_jsonserialize.htm)



## Jackson Deserialization Annotation(역직렬화 어노테이션)

**Jackson Deserialization Annotation**에서 `Deserialization(역직렬화)`이란, 직렬화한 Json 데이터들을 자바 객체로 변환하는 것을 의미합니다.



### @JsonCreator (with @JsonProperty)

역직렬화할 때 사용하는 생성자에서 사용합니다. **멤버 필드의 이름이 Json 키와 일치하지 않을 경우 사용**합니다.

```json
{
  "id":1,
  "theName":"My bean"
}
```

```java
public static class BeanWithCreator {
    public int id;
    public String name;

    @JsonCreator	// 생성자 위에 어노테이션을 붙여줍니다.
    public BeanWithCreator(
            @JsonProperty("id") int id,
            @JsonProperty("theName") String name // 이름이 name인 멤버 필드에 Json 키가 theName인 값을 Mapping 합니다.
    ) {
        this.id = id;
        this.name = name;
    }
}
```



### @JacksonInject

**Json 데이터가 아닌 곳에서 값을 주입할 때 사용**합니다.  

**참고 링크**

[테스트 코드](https://www.tutorialspoint.com/jackson_annotations/jackson_annotations_jacksoninject.htm)

```json
{
  "name": "My bean"
}
```

```java
public static class BeanWithInject {
    @JacksonInject
    public int id;
    public String name;
}
```



### @JsonAnySetter

@JsonAnyGetter와 비슷하게 Map변수를 다루는데 유연성을 제공합니다. **객체를 역직렬화할 때 Mapping되지 않은 일반 Json을 Map의 키 / 값**으로 역직렬화합니다.

```json
{
  "name": "My bean",	
  "attr2": "val2",		
  "attr1": "val1"
}
```

```java
@Setter
@Getter
public static class ExtendableBean {
    public String name;		// Json의 name키와 Mapping됩니다.
    private Map<String, String> properties = new HashMap<>();

    @JsonAnySetter	// Json의 attr2, attr1은 Mapping할 수 있는 필드가 없기 때문에 Map의 키/값으로 역직렬화합니다.
    public void setProperties(String key, String value) {
        properties.put(key, value);
    }
}
```



### @JsonSetter

**Json키와 맴버변수의 Setter의 이름이 일치하지 않을 경우 유용하게 사용**할 수 있습니다. 

`@Creator + @JsonProperty`와 혼동할 수 있지만, **@Creator + @JsonProperty는 생성자에, @JsonSetter는 Setter나 필드에 사용**하는 것입니다.

```json
{
  "id": 1,
  "name": "My bean"
}
```

```java
@Setter
@Getter
public static class MyBean {
    public int id;
    private String name;

    @JsonSetter("name")	
    // Setter의 이름이 TheName이지만 어노테이션을 이용해 Json 키 name을 TheName에 역직렬화합니다.
    public void setTheName(String name) {
        this.name = name;
    }
}
```



### @JsonAlias

**역직렬화할 때 한 개 이상의 Json 키를 한 개의 멤버필드에 Mapping되게 설정**할 수 있습니다.

```json
{
  "id": 1,
  "name": "My bean"
}

{
  "id": 1,
  "theName": "My bean"
}

{
  "id": 1,
  "aName": "My bean"
}
```

```java
@Setter
@Getter
public static class MyBean {
    public int id;
    @JsonAlias({"name", "theName", "aName"})	// Json 키가 name, theName, aName인 값을 name에 Mapping 합니다.
    private String name;
}
```



### @JsonDeserialize

**StdDeserializer를 상속받아 역직렬화를 어떻게 할지 커스터마이징**할 수 있습니다.

**참고 링크**

[jackson custom serializer, deserializer 만들기](https://multifrontgarden.tistory.com/172)

[테스트 코드](https://www.tutorialspoint.com/jackson_annotations/jackson_annotations_jsondeserialize.htm)





# Jackson Property Inclusion Annotations

속성을 포함할지 포함하지 않을지 설정해주는 어노테이션들 입니다.



### @JsonIgnoreProperties

**클래스에 붙일 수 어노테이션이며, 무시할 필드나 필드 목록을 표시**하는 데 사용됩니다.

```java
@JsonIgnoreProperties({"id"})
public static class BeanWithIgnore {
    public int id;
    public String name;
}
```

```json
// id 필드를 무시합니다.
{
  "name": "yun"
}
```



### @JsonIgnore

**필드에 붙일 수 어노테이션이며, 무시할 필드를 표시**하는 데 사용됩니다.

```java
public static class BeanWithIgnore {
    @JsonIgnore
    public int id;
    public String name;
}
```

```json
// id 필드를 무시합니다.
{
  "name": "yun"
}
```



### @JsonIgnoreType

**어노테이션이 붙은 클래스와 클래스에 속하는 모든 변수를 무시**하도록 설정하는데 사용됩니다.

```java
public static class User {
public int id;
public Name name;

    @JsonIgnoreType
    public static class Name {
        public String firstName;
        public String lastName;
    }
}
```

```json
// Name 객체를 무시합니다.
{
  "id": 1
}
```



### @JsonInclude

어노테이션의 속성에 따라 포함할 범위를 설정합니다. (default는 ALWAYS)

- `Include.ALWAYS` : 모든 값을 포함합니다.
- `Include.NON_NULL` : null이 아닌 값을 포함합니다.(null 제외)
- `Include.NON_ABSENT` : null이나 참조 유형의 `absent(없음)`값이 아닌 값을 포함합니다. 
  - null값 제외
  - 참조 유형 (Java 8 'Optional'또는 {link java.utl.concurrent.atomic.AtomicReference})의 `absent`값 즉, null이 아닌 값은 제외합니다. 
  - 이 옵션은 대부분 `Optional(Java 8, Guava)`과 함께 작동하는 데 사용됩니다.
- `Include.NON_EMPTY` : null 또는 값이 비어있는 경우가 아니면 포함합니다.
  - null 값 (`NON_NULL` 조건) 제외
  - absent 값 (`NON_ABSENT` 조건) 제외
  - Collections와 Maps 메서드에서 isEmpty()이 true인 경우 제외
  - arrays의 length 0인 경우 제외
  - String의 length()를 호출했을 때 0이거나 빈 String값일 경우 제외
  - timestamp 값이 0인 경우 제외
  - 기본 타입이 디폴트 값인 경우 (int나 Integer가 0이거나 bool/Boolean 값에서 false인 경우) 제외
- `Include.DEFAULT` : `NON_EMPTY`가 아니고 primitive 타입이 디폴트 값이 아니며, Date/time 값이 0L이 아닌 경우를 포함합니다.
  - empty는 제외(`NON_EMPTY` 조건)
  - primitive 타입이 디폴트 값이면 제외(int / Integer : 0 , boolean / Boolean : false 등)
  - Date의 timestamp가 0L이면 제외
- 그 외 : `CUSTOM, USE_DEFAULTS`가 있지만, 잘 안쓰이기 때문에 [공식 문서](https://fasterxml.github.io/jackson-annotations/javadoc/2.9/com/fasterxml/jackson/annotation/JsonInclude.Include.html)를 참고하시면 되겠습니다.

```java
@JsonInclude(Include.NON_NULL)
@AllArgsConstructor
public static class MyBean {
    public int id;
    public String name;
}
```

```json
// NON_NULL 사용시 name이 null인 경우에 제외됩니다.
{
  "id": 1
}
```



### @JsonAutoDetect

필드의 직렬화, 역직렬화 대상을 정할 수 있습니다. 

id 와 name은 private이고 getter가 없다면, 접근할 수 있는 경로가 없습니다. 그래서 **@JsonAutoDetect의 가시성 범위를 설정**합니다. **ANY로 준다면 private 필드까지 모두 직렬화시 접근가능**합니다.

JsonAutoDetect의 가시성 범위는 `ANY, NON_PRIVATE, PROTECTED_AND_PUBLIC` 등등 여러 속성이 있으니 필요한 것들을 공식문서를 참고해 골라쓰면 될 것같습니다.

**참고 링크**

[JsonAutoDetect 공식 문서](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/reference-materials/javadoc/com/fasterxml/jackson/annotation/JsonAutoDetect.html)

[Visibility속성 공식 문서](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/reference-materials/javadoc/com/fasterxml/jackson/annotation/JsonAutoDetect.Visibility.html)

```java
@JsonAutoDetect(fieldVisibility = Visibility.NONE, getterVisibility = Visibility.PUBLIC_ONLY)
public static class PrivateBean {
    private int id;
    private String name;
    
    public String getName() {
        return this.name;
    }
}
```

```json
// 필드는 Json으로 직렬화하지 않고, public Getter 메서드만 Json으로 직렬화합니다.
{
  "name": "yun"
}
```





## Jackson Polymorphic Type Handling Annotations(다형성 처리 어노테이션)

다형성 유형 처리 어노테이션 입니다.

- `@JsonTypeInfo` : 직렬화, 역직렬화에 포함될 유형 정보의 세부 사항을 표시하는 데 사용됩니다.
- `@JsonSubTypes` : 어노테이션이 달린 유형의 하위 유형을 나타냅니다.
- `@JsonTypeName` : 어노테이션이있는 클래스에 사용할 이름을 정의합니다.

**참고링크**

[Jackson을 이용해 추상, 인터페이스 객체 별로 구현체 구분하기](https://seongtak-yoon.tistory.com/70)



---

참고 : [https://pjh3749.tistory.com/281](https://pjh3749.tistory.com/281)

[https://cheese10yun.github.io/jackson-annotation-final/](https://cheese10yun.github.io/jackson-annotation-final/)

[https://recordsoflife.tistory.com/28](https://recordsoflife.tistory.com/28)

[https://sjh836.tistory.com/164](https://sjh836.tistory.com/164)

[https://www.tutorialspoint.com/jackson_annotations/index.htm](https://www.tutorialspoint.com/jackson_annotations/index.htm)

[https://developia.tistory.com/18](https://developia.tistory.com/18)

[https://alwayspr.tistory.com/31](https://alwayspr.tistory.com/31)

[https://www.slideshare.net/topcredu/jackson-annotation-json](https://www.slideshare.net/topcredu/jackson-annotation-json)

[https://tomining.tistory.com/200](https://tomining.tistory.com/200)

[https://blog.advenoh.pe.kr/java/Jackson%EC%97%90%EC%84%9C-Infinite-Recursion-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95/](https://blog.advenoh.pe.kr/java/Jackson%EC%97%90%EC%84%9C-Infinite-Recursion-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95/)

[https://jeong-pro.tistory.com/234](https://jeong-pro.tistory.com/234)

[https://seongtak-yoon.tistory.com/70](https://seongtak-yoon.tistory.com/70)

