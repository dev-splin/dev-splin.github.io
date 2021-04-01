---
title: "Spring : MockMvc"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : MockMvc"
toc: true
toc_sticky: true
toc_label: 목차
---

# MockMVC를 이용한 WebAPI 테스트

원래는 `Web API`를 작성하고 테스트를 수행하기 위해서 스프링 애플리케이션을 실행하고 개발자가 직접 테스트를 수행했습니다. `Web API`를 많이 작성하다보면 웹 어플리케이션을 실행하고 브라우저를 열어서 테스트할 URI를 입력하고 다시 코드를 작성하고 웹 어플리케이션을 재시작하고 등을 반복하게 됩니다. `Web API`를 실행하는 시간보다 웹 어플리케이션을 실행하고 종료하는 시간이 더 오래걸리는 상황이 발생합니다.

여기에는 다음과 같은 문제점이 있습니다.

- 개발자의 수동 테스트
- 코드를 수정한 후에 서버를 재시작하고 테스트

이런 문제를 해결하기 위해 다음과 같은 방법을 사용할 수 있습니다.

-  [JUnit 사용하기](https://dev-splin.github.io/spring/Spring-JUnit/)
- MockMVC 사용하기 



## MockMVC 란?

우리는 웹 애플리케이션을 작성한 후, 해당 웹 애플리케이션을 Tomcat이라는 이름의 `WAS(Web Application Server)`에 배포(deploy)하여 실행을 하였습니다. 브라우저의 요청은 `WAS`에게 전달되는 것이고 응답도 `WAS`에게서 받게 됩니다. `WAS`는 요청을 받은 후, 해당 요청을 처리하는 웹 어플리케이션을 실행하게 됩니다.

즉, `Web API`를 테스트한다는 것은 `WAS`를 실행해야만 된다는 문제가 있습니다. 이런 문제를 해결하기 위해서 스프링 3.2부터 `MockMVC`가 추가되었습니다. 

**`MockMVC`는 `WAS`와 같은 역할을 수행**합니다. 요청을 받고 응답을 받는 `WAS`와 같은 역할을 수행하면서 여러분들이 작성한 웹 애플리케이션을 실행해줍니다.<br> `WAS`는 실행 시 상당한 많은 작업을 수행합니다. 하지만 **`MockMVC`는 웹 어플리케이션을 실행하기 위한 최소한의 기능만을 가지고 있기 때문**에 `MockMVC`를 이용한 웹 어플리케이션 실행은 상당히 빠릅니다.

`MockMVC`를 이용하면 다음과 같은 테스트를 수행할 수 있습니다.

![img](https://cphinf.pstatic.net/mooc/20200305_148/1583391388280d6dWl_PNG/mceclip0.png)
출처 : https://www.slideshare.net/sbcoba/spring-test-mvc (26 page)



### 예제를 통해 알아보는 MockMVC Test

`GuestbookApiController`를 테스트하는 `GuestbookApiControllerTest` 클래스를 작성해보도록 하겠습니다.

먼저, 아래 그림을 보겠습니다.


![img](https://cphinf.pstatic.net/mooc/20200305_99/1583391419153fciVU_PNG/mceclip1.png)



`GuestbookApiController`를 단위 테스트한다는 것은, `GuestbookApiController`가 사용하는 `GuestbookService`에 대한 부분은 함께 테스트하지 않는다는 것을 의미합니다.
이를 위해 `GuestbookService`에 대한 `목(Mock)객체`를 사용할 것이고 `Mokito`를 이용해 목객체를 생성할 것입니다. 

이에 대한 자세한 내용은 [로직 단위테스트](https://dev-splin.github.io/spring/Spring-%EB%A1%9C%EC%A7%81-%EB%8B%A8%EC%9C%84%ED%85%8C%EC%8A%A4%ED%8A%B8/)를 봐주세요~!

그리고, 컨트롤러를 테스트하기 위해 `MockMvc`를 사용하도록 하겠습니다.

#### GestubookApiControllerTest.java

```java
package kr.or.connect.guestbook.controller;

import kr.or.connect.guestbook.config.ApplicationConfig;
import kr.or.connect.guestbook.config.WebMvcContextConfiguration;
import kr.or.connect.guestbook.dto.Guestbook;
import kr.or.connect.guestbook.service.GuestbookService;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.http.MediaType;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.RequestBuilder;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

import java.util.Arrays;
import java.util.Date;
import java.util.List;

import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration
@ContextConfiguration(classes = {WebMvcContextConfiguration.class, ApplicationConfig.class })
public class GestubookApiControllerTest {
    @InjectMocks
    public GuestbookApiController guestbookApiController;

    @Mock
    GuestbookService guestbookService;

    private MockMvc mockMvc;

    @Before
    public void createController() {
        MockitoAnnotations.initMocks(this);
        mockMvc = MockMvcBuilders.standaloneSetup(guestbookApiController).build();
    }

    @Test
    public void getGuestbooks() throws Exception {
        Guestbook guestbook1 = new Guestbook();
        guestbook1.setId(1L);
        guestbook1.setRegdate(new Date());
        guestbook1.setContent("hello");
        guestbook1.setName("kim");

        List<Guestbook> list = Arrays.asList(guestbook1);
        when(guestbookService.getGuestbooks(0)).thenReturn(list);

        RequestBuilder reqBuilder = MockMvcRequestBuilders.get("/guestbooks").contentType(MediaType.APPLICATION_JSON);
        mockMvc.perform(reqBuilder).andExpect(status().isOk()).andDo(print());

        verify(guestbookService).getGuestbooks(0);
    }

    @Test
    public void deleteGuestbook() throws Exception {
        Long id = 1L;

        when(guestbookService.deleteGuestbook(id, "127.0.0.1")).thenReturn(1);

        RequestBuilder reqBuilder = MockMvcRequestBuilders.delete("/guestbooks/" + id).contentType(MediaType.APPLICATION_JSON);
        mockMvc.perform(reqBuilder).andExpect(status().isOk()).andDo(print());

        verify(guestbookService).deleteGuestbook(id, "127.0.0.1");
    }
}
```



```java
@Mock
GuestbookService guestbookService;
```

`@Mock`어노테이션을 붙여서 선언된 `guestbookService`는 `mockito`에 의해 목객체로 생성됩니다. 말그대로 가짜 객체가 됩니다.



```java
@InjectMocks
public GuestbookApiController guestbookApiController;
```

`@InjectMocks`어노테이션이 붙여서 선언된 `guestbookApiController`는 목객체인 `GuestbookService`를 사용하게 됩니다. 스프링에 의해 주입된 객체를 사용하는 것이 아닌 `Mockito` 프레임워크에 의해 생성된 목객체가 주입되어 객체가 생성됩니다.



```java
	@Before
    public void createController() {
          .......
   }
}
```

테스트 메소드가 실행되기 전에 `@Before`어노테이션이 붙은 메소드가 실행됩니다.



```java
MockitoAnnotations.initMocks(this);
```

현재 객체에서 `@Mock`이 붙은 필드를 목객체로 초기화시킵니다.



```java
mockMvc = MockMvcBuilders.standaloneSetup(guestbookApiController).build();
```

`MockMVC`타입의 변수 `mockMvc`를 초기화 합니다. `guestbookApiController`를 테스트 하기 위한
`MockMvc`객체를 생성합니다.



```java
       Guestbook guestbook1 = new Guestbook();
        guestbook1.setId(1L);
        guestbook1.setRegdate(new Date());
        guestbook1.setContent("hello");
        guestbook1.setName("kim");

        List<Guestbook> list = Arrays.asList(guestbook1);
        when(guestbookService.getGuestbooks(0)).thenReturn(list);
```

`List<Guestbook>`타입의 변수 list를 초기화하고 해당 list에 방명록 한 건을 저장합니다.

`when(guestbookService.getGuestbooks(0)).thenReturn(list);`<br>위의 문장은 아래와 같이 동작합니다.
**when(** 목객체.목객체메소드호출() **).threnReturn(**목객체 메소드가 리턴 할 값**)**<br>`guestbookService.getGuestbook(0)` 이 호출되면 위에서 선언된 list객체가 리턴 되도록 설정합니다.



```java
RequestBuilder reqBuilder
 = MockMvcRequestBuilders.get("/guestbooks").contentType(MediaType.APPLICATION_JSON);
```

`MockMvcRequestBuilders`를 이용해 `MockMvc`에게 호출할 URL을 생성합니다.

`get(“/guestbooks”)`
`GET` 방식으로 `/guestbooks` 경로를 호출하라는 의미입니다.

`contentType(MediaType.APPLICATION_JSON);`
`application/json` 형식으로 `api`를 호출합니다.

즉 2가지가 합치면 **application/json형식으로 /guestbooks를 GET방식으로 호출한다는 것을 뜻**합니다. 이러한 URL정보를 가진 `reqBuilder`를 생성합니다.



```java
mockMvc.perform(reqBuilder).andExpect(status().isOk()).andDo(print());
```

`mockMvc.perform(reqBuilder)` 는 `reqBuilder`에 해당하는 URL에 대한 요청을 보냈다는 것을 의미합니다.

`andExpect(status().isOk())` 는 `mockMvc`에 의해 **URL이 실행되고 상태코드값이 200이 나와야 한다는 것을 의미**합니다.
`andDo(print())`는 처리 내용을 출력하게 됩니다.

여기까지 실행되면 화면에 다음과 같은 결과가 출력되면서 테스트가 성공하게 됩니다.



```java
MockHttpServletRequest:
      HTTP Method = GET
      Request URI = /guestbooks
       Parameters = {}
          Headers = {Content-Type=[application/json]}

Handler:
             Type = kr.or.connect.guestbook.controller.GuestbookApiController

           Method = public java.util.Map<java.lang.String, java.lang.Object> 

kr.or.connect.guestbook.controller.GuestbookApiController.list(int)

Async:
    Async started = false
     Async result = null

Resolved Exception:
             Type = null

ModelAndView:
        View name = null
             View = null
            Model = null

FlashMap:
       Attributes = null

MockHttpServletResponse:
           Status = 200
    Error message = null
          Headers = {Content-Type=[application/json;charset=UTF-8]}
     Content type = application/json;charset=UTF-8
             Body =
 {"pageStartList":[],"count":0,"list":[{"id":1,"name":"kim","content":"hello","regdate":1581605377204}]}
    Forwarded URL = null
   Redirected URL = null
          Cookies = []
```

`.andExpect(jsonPath("$.name").value("kim"))` 과 같은 문장을 사용하여 Json 결과에 `“name”:”kim”`이 있을 경우에만 성공이 될 수 있도록 할 수도 있습니다. **이 경우 `jsonPath`에 대한 라이브러리가 pom.xml파일에 추가** 되야 합니다.

```markup
<dependency>
    <groupId>com.jayway.jsonpath</groupId>
    <artifactId>json-path</artifactId>
    <version>2.4.0</version>
</dependency>
```

라이브러리 추가 후 test 클래스에 `import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;` 를 import 해주어야 합니다.



```java
verify(guestbookService).getGuestbooks(0);
```

`Guestbook` 목객체의 `getGuestbooks(0)`메소드가 호출했다면 검증은 성공하게 됩니다.

 

```java
@Test
    public void deleteGuestbook() throws Exception {
        Long id = 1L;

        when(guestbookService.deleteGuestbook(id, "127.0.0.1")).thenReturn(1);

        RequestBuilder reqBuilder = MockMvcRequestBuilders.delete("/guestbooks/" + id).contentType(MediaType.APPLICATION_JSON);
        mockMvc.perform(reqBuilder).andExpect(status().isOk()).andDo(print());

        verify(guestbookService).deleteGuestbook(id, "127.0.0.1");
    }
```

위의 코드는 방명록을 삭제하는 `web api`를 테스트하고 있습니다.



```java
RequestBuilder reqBuilder = MockMvcRequestBuilders.delete("/guestbooks/" + id).contentType(MediaType.APPLICATION_JSON);
```

`“/guestbooks/” + id` 경로를 `DELETE`방식으로 호출하기 위한 경로 정보를 가지고 있는 `reqBuilder`객체를 생성합니다.

 

```java
mockMvc.perform(reqBuilder).andExpect(status().isOk()).andDo(print());
```

**`reqBuilder`에 해당하는 URL을 호출한 후, 상태 코드가 200일 경우 성공**합니다. 그리고 **결과를 출력**하게 됩니다.



```java
verify(guestbookService).deleteGuestbook(id, "127.0.0.1");
```

`guestbookService` 목객체의 `deleteGuestbook(id, “127.0.0.1”)`메소드가 `Web API`가 동작하면서 호출되었다면 성공하게 됩니다.



---

참고 : [https://www.boostcourse.org/web326/lecture/59389?isDesc=false](https://www.boostcourse.org/web326/lecture/59389?isDesc=false)