---
title: "Spring : ResponseEntity를 사용해야 하는 이유"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : ResponseEntity"
toc: true
toc_sticky: true
toc_label: 목차
---

# ResponseEntity를 사용해야 하는 이유

우리는 왜 `Restful API`를 만드는 것일까요? **Restful API를 만드는 가장 큰 이유는 Client Side를 정형화된 플랫폼이 아닌 모바일, PC, 어플리케이션 등 플랫폼에 제약을 두지 않는 것을 목표**로 했기 때문입니다.

조금 더 쉽게 설명하자면, 2010년 이전만 해도 Server Side에서 데이터를 전달해주는 Client 프로그램은 PC 브라우저 뿐이었습니다. 하지만 스마트 기기들이 등장하면서 TV, 스마트 폰, 테블릿 등 Client 프로그램이 다양화되고 그에 맞춰서 Server Side를 일일이 만드는 것이 비효율적인 일이 되었습니다.

이런 과정에서 개발자들은 Client Side를 전혀 고려하지 않고 메시지 기반, XML, JSON과 같은 Client에서 바로 객체로 치환 가능한 형태의 데이터 통신을 지향하게 되면서 Server와 Client의 역할을 분리하게 되었습니다.

이런 변화를 겪으면서 필요해진 것은 `HTTP의 표준 규약`을 지켜서 API를 만드는 것입니다.

이 Restful API를 개발하다보면 HTTP의 Response 규약을 지키지 않고 본인들이 만들어낸 Json 규약으로 응답하는 경우를 많이 볼 수 있습니다. **하지만 표준을 지키지 않으면, Client Side가 정형화되어있지 않은 환경에서 개발속도가 저하**됩니다. **표준을 지키지 않게 되었을 때 발생하는 가장 큰 이슈는 HTTP Status 코드를 제대로 응답하지 않게 되는 것**이고 이런 경우 클라이언트에서는 별도의 방어코드를 넣는 수고가 발생하며, 이런 수고들이 모여 프로젝트의 속도를 저하시키는 요소가 되는 것입니다. 그렇기 때문에 표준을 준수하는 것이 중요합니다.

위와 같은 이유로 Controller에서 결과를 반환할 때, **ResponseEntity객체를 이용해 규약에 맞는 HTTP Response를 반환**하여야 합니다.



### HTTP

HTTP 는 `HyperText Transfer Protocol`의 약자로, **Client 와 Server 사이에 요청과 응답을 처리하기 위한 규약**입니다. 해당 규약을 지키게 된다면 살펴보는 것만으로도 어떤 요청을 하는지에 대해서 간략하게 알 수가 있습니다. HTTP의 **Request**와 **Response** 모두 크게 세 가지 요소로 구성됩니다. 



#### HTTP Request

HTTP 요청은 `Start Line`, `Headers` 그리고 `Body` 세 가지 요소로 구성됩니다.

1. `Start Line` : method, URL, 그리고 version으로 이루어져있으며, 서버에서 요청을 받아들이는 첫 줄 입니다.
2. `Headers` : 요청에 대한 접속 운영체제, 브라우저, 인증 정보와 같은 부가적인 정보를 담고 있습니다.
3. `Body` : 요청에 관련된 Json, html 과 같은 구체적인 내용을 포함합니다.



#### HTTP Response

HTTP 응답은 다른 요소인 `Status Line` 과 Request도 가지고 있는 `Headers`, `Body`로 구성됩니다.

- `Status Line` : HTTP 버전과 함께 헤딩 요청에 대한 처리의 상태를 나타납니다. 200, 404와 같은 숫자 코드로 동시에 나타냅니다.



Spring에서는 HTTP Response를 만드는 것이 주요한 관심사입니다. 200, 404 등 각각의 응답의 상태 코드뿐만이 아니라, Body에 들어갈 내용도 넣어주어야 합니다. 이런 **세 가지 요소를 받아서 자동으로 구성해주는 @ResponseBody Annotation과 ResponseEntity객체**가 있습니다.





## Controller vs RestController

ResponseEntity를 알아보기 전에 먼저 Controller와 RestController의 차이점을 알아보겠습니다. **Spring MVC Controller와 Restful Web Service Controller의 가장 큰 차이점은 HTTP Response Body가 생성되는 방식**입니다.



### Controller(Spring MVC Controller)

<img src="https://user-images.githubusercontent.com/79291114/122781355-af4a6280-d2ea-11eb-8091-c27da97e6e0b.png" alt="SpringMVC" style="zoom:80%;" />

**기존의 Spring MVC는 view을 사용하기 때문에 주로 view(화면)를 return** 합니다. 이 때, 데이터는 ModelAndView객체를 이용해 Controller에서 Client로 전달되는 것을 알 수 있습니다. 단, 아래와 같이 `@ResponseBody` 어노테이션을 사용하면 View를 return하지 않고 Controller에서 직접 데이터를 return할 수 있습니다.

<img src="https://user-images.githubusercontent.com/79291114/122781358-af4a6280-d2ea-11eb-96ed-8241fef157f0.png" alt="SpringMVCResponseBody" style="zoom:80%;" />

`@ResponseBody`을 사용하면, Spring은 HTTP 응답에 리턴 값을 자동으로 변환해줍니다. 대신 Controller 클래스의 각 메서드에 `@ResponseBody` 어노테이션을 작성해줘야 합니다.



### RestController(Restful Web Service Controller)

<img src="https://user-images.githubusercontent.com/79291114/122781352-ae193580-d2ea-11eb-8319-d65af7bb2973.png" alt="RestfulWebService" style="zoom:80%;" />

Spring4.0에서는 `@RestController` 어노테이션을 선언해주면서 **컨트롤러 클래스의 각 메서드마다 @ResponseBody을 추가할 필요가 없어졌고**, **모든 메서드는 @ResponseBody 애노테이션이 기본으로 작동**이 됩니다.





## @ResponseBody

`@ResponseBody` 는 HTTP 규격에 맞는 응답을 만들어주기 위한 Annotation입니다. 

HTTP 요청을 객체로 변환하거나, 객체를 응답으로 변환하는 HttpMessageConverter를 사용합니다. 따라서 Controller에서 반환할 객체나 Method에 `@ResponseBody` 를 붙히는 것만으로 HTTP 규격에 맞는 값을 반환할 수 있습니다.

> `HttpMessageConverter` : **HTTPMessageConverter는 해당 Annotation이 붙은 대상을 Jackson라이브러리를 통해 Json으로 response body 에 직렬화를 하는 방식으로 작동**됩니다. 
> 다른 Converter를 등록하지 하지 않았으면 기본적으로 HttpMessageConvter의 구현체인 `MappingJackson2HttpMessageConverter`를 사용합니다. 



```java
@ResponseBody
@ResponseStatus(HttpStatus.OK)
public MoveResponseDto move(@PathVariable String name, @RequestBody MoveDto moveDto) {
  String command = makeMoveCmd(moveDto.getSource(), moveDto.getTarget());
    springChessService.move(name, command, new Commands(command));
    
  MoveResponseDto moveResponseDto = new MoveResponseDto(springChessService
      .continuedGameInfo(name), name);
      
  return moveResponseDto;
}
```

하지만 단점으로는 **HTTP 규격 구성 요소 중 하나인 Header에 대해서 유연하게 설정을 할 수 없다**는 점입니다. 또한 Status도 메서드 밖에서 `@ResponseStatus` Annotation을 사용하여 따로 설정을 해주어야한다는 점이 있습니다. 즉, **HTTP 규격에 따라 사용하기 위해서는 @ResponseBody와 @ResponseStatus를 조합**해 사용해야 한다는 것입니다. 이는 **@ResponseBody만 사용 시에 별도의 뷰를 제공하지 않고, 데이터만 전송하는 형식이기 때문**입니다. 이와 같은 점들을 해결해 줄 수 있는 것이 `ResponseEntity`객체입니다.

> 리턴값을 성공적으로 반환할 때, @ResponseStatus를 사용하지 않으면 HttpStatusCode는 기본적으로 200으로 응답하게 됩니다.





## ResponseEntity

`ResponseEntity`도 마찬가지로 HTTP 응답을 빠르게 만들어주기 위한 객체입니다. `@ResponseBody`와 달리 Annotation이 아닌 객체로 사용이 됩니다. 즉, **응답으로 변환될 정보를 모두 담은 요소들을 객체로 만들어서 반환**해줍니다. 객체의 구성요소에서 응답이 되는 본문을 HttpMessageConverter가 변환해준 뒤에, 나머지 구성 요소를 넘겨주게 됩니다. 이를 통하여 앞서 언급한 Header와 Status에 관련된 문제점들을 해결할 수가 있는데, 일단은 선언된 구조부터 살펴보도록 하겠습니다.



```java
//ResponseEntity 선언 구조
public class ResponseEntity extends HttpEntity {

  private final Object status;
}
```

먼저, ResponseEntity의 구조를 보게 되면, 다음과 같이 `Status`만 필드값으로 가지고 있습니다. **ResponseEntity에서 직접적으로 Status Code 를 지정할 수 있다는 것을 의미**합니다. 나머지 부분은 HttpEntity 에 구현이 되어있는데, 이는 RequestEntity와 여러 설정들을 공유하기 때문입니다. 다음은 HttpEntity 의 구현 부분을 보도록 하겠습니다.



```java
//HttpEntity 선언 구조
public class HttpEntity<T> {
    public static final HttpEntity<?> EMPTY = new HttpEntity<>();
  
  
    private final HttpHeaders headers;
  
    @Nullable
    private final T body;
}
```

이와 같이 ResponseEntity는 `HttpEntity`를 상속하여 구현이 됩니다. 

**HttpEntity에서는 Generic 타입으로 Body가 될 필드값**을 가질 수 있습니다. Generic 타입으로 인하여 바깥에서 Wrapping 될 타입을 지정할 수 있습니다. Wrapping 된 객체들은 자동으로 HTTP 규격에서 Body 에 들어갈 수 있도록 변환 됩니다. 

또한, 필드 타입으로 `HttpHeaders`를 가지고 있는데, 이는 **ResponseBody와 다르게 객체 안에서 Header를 설정해 줄 수 있음을 의미**합니다. 생성자(Constructor)를 활용하여 ResponseEntity를 사용한 예시는 다음과 같습니다.



```java
public ResponseEntity<MoveResponseDto> move(@PathVariable String name,
    @RequestBody MoveDto moveDto) {
    HttpHeaders headers = new HttpHeaders();
    headers.set("Game", "Chess");
    
    String command = makeMoveCmd(moveDto.getSource(), moveDto.getTarget());
    springChessService.move(name, command, new Commands(command));
    
    MoveResponseDto moveResponseDto = new MoveResponseDto(springChessService
        .continuedGameInfo(name), name);

    return new ResponseEntity<MoveResponseDto>(moveResponseDto, headers, HttpStatus.valueOf(200)); // ResponseEntity를 활용한 응답 생성
}
```

Spring 에서 다음과 같이 HTTP 응답으로 반환할 메서드를 만들게 되었습니다. 이 때, 타입은 ResponseEntity<반환할 타입>으로 지정합니다. **생성자를 사용 시에는  Body 부분, Header 그리고 상태로 지정될 Status 를 차례로 입력하여 생성**하면 됩니다. 예시 코드에서는 moveResponseDto라는 객체가 Body부분에 들어가서 응답으로 전송이 됩니다. 즉, HTTP 응답에 필요한 요소들 중 대표적인 `Status, Header , Body`를 지정하여 응답을 만들 수 있습니다.



### Builder 사용 권장

ResponseEntity를 사용할 때, 생성자를 사용하기보다는 Builder를 사용하는 것을 권장하고 있습니다. 그 이유는 숫자로 된 상태 코드를 넣을 때, 잘못된 숫자를 넣는 실수가 발생할 수 있기 때문입니다. 따라서, Builder Pattern을 사용하면 다음과 같이 코드를 변경할 수 있습니다.

```java
  return new ResponseEntity<MoveResponseDto>(moveResponseDto, headers, HttpStatus.valueOf(200));

  return ResponseEntity.ok()
        .headers(headers)
        .body(moveResponseDto);
```

이렇게 Builder Pattern을 활용하면 각 상태에 매칭되는 숫자 코드를 외울 필요 없이 Builder 메소드를 선택하면 됩니다.

다만, **Status와 관련된 메서드와 headers메서드는 BodyBuilder를 리턴하기 때문에 ResponseEntity를 리턴하는 body()메서드를 꼭 마지막으로 호출하여야 합니다.**



---

참고 : [https://steemit.com/kr-dev/@igna84/spring-boot-responseentity](https://steemit.com/kr-dev/@igna84/spring-boot-responseentity)

[https://2ham-s.tistory.com/279](https://2ham-s.tistory.com/279)

[https://woowacourse.github.io/javable/post/2021-05-10-response-entity/](https://woowacourse.github.io/javable/post/2021-05-10-response-entity/)

[https://www.baeldung.com/spring-response-entity](https://www.baeldung.com/spring-response-entity)

[https://woodcock.tistory.com/19](https://woodcock.tistory.com/19)

