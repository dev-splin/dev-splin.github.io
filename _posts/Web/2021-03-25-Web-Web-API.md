---
title: "Web : Web API란?"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Web API"
toc: true
toc_sticky: true
toc_label: 목차
---

# Web API

**웹 서버 또는 웹 브라우저를 위한 애플리케이션 프로그래밍 인터페이스**입니다. HTTP 서비스이고 다양한 클라이언트에서 접근이 가능하도록 설계되어있습니다. **Web 환경을 통해 제공되는 데이터 CRUD인터페이스를 제공**하며,  HTTP 표준 접근 방식을 이용하며 플랫폼 환경, 클라이언트 환경의 제한이 없는 서비스 구현이 가능합니다. 그리고 보통 `REST API`를 완벽하게 구현 하지 못할 경우 `Web API`라고 합니다. `REST`의 `HATEOAS`와 `self-descriptive`를 만족 못하는 경우 대부분을 `Web API`라고 칭합니다.



## 예제

Spring MVC를 이용한 간단한 Web 어플리케이션이 있습니다.

http://localhost:8080/mvcexam/plusform 이라는 URL주소를 브라우저의 주소창에 입력하면, **2개의 값을 입력받는 폼(Form)**이 보여졌고 해당 **폼에 값을 입력하고 버튼을 누르면 2개의 숫자의 합이 출력되는 프로그램**입니다. mvcexam 프로젝트에 덧셈을 구하는 Web API를 추가해봄으로써 Web API가 무엇인지 간단히 알아보도록 하겠습니다.



mvcexam 프로젝트에3개의 정수 값을 저장할 수 있는 `PlusResult` 클래스를 추가합니다.

```java
package kr.or.connect.webmvc.dto;

public class PlusResult {
    private int value1;
    private int value2;
    private int result;

    public int getValue1() {
        return value1;
    }

    public void setValue1(int value1) {
        this.value1 = value1;
    }

    public int getValue2() {
        return value2;
    }

    public void setValue2(int value2) {
        this.value2 = value2;
    }

    public int getResult() {
        return result;
    }

    public void setResult(int result) {
        this.result = result;
    }
}
```



컨트롤러를 만듭니다.

```java
package kr.or.connect.webmvc.controller;

import kr.or.connect.webmvc.dto.PlusResult;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class PlusApiController {
    @GetMapping("/api/plus")
    @ResponseBody
    public PlusResult plus(@RequestParam("value1") int value1, @RequestParam("value2") int value2){
        PlusResult plusResult = new PlusResult();
        plusResult.setValue1(value1);
        plusResult.setValue2(value2);
        plusResult.setResult(value1 + value2);
        return plusResult;
    }
}
```

컨트롤러가 가지는 메소드는 String을 반환합니다. 컨트롤러 메소드가 반환하는 String값은 뷰(View)이름이나 리다이렉트되는 경로를 의미했었습니다. 그런데, 지금 작성하는 컨트롤러는 String이 아닌 `PlusResult` 객체를 리턴하고 있습니다. 그리고 `@ResponseBody` 라는 어노테이션이 붙어습니다. `@ResponseBody` 어노테이션이 붙게 되면 해당 **메소드는 뷰이름을 리턴하는 것이 아니라, 리턴한 객체를 출력하라는 의미**를 가집니다.

여기까지 작성을 하고 서버를 재시작한 후, http://localhost:8080/mvcexam/api/plus?value1=10&value2=20 이라고 브라우저에서 입력하게 되면 다음과 같은 에러가 발생하게 됩니다.

```java
org.springframework.web.util.NestedServletException: Request processing failed; nested exception is java.lang.IllegalArgumentException: No converter found for return value of type: class kr.or.connect.webmvc.dto.PlusResult
org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:982)
org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:861)
javax.servlet.http.HttpServlet.service(HttpServlet.java:634)
org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:846)
javax.servlet.http.HttpServlet.service(HttpServlet.java:741)
org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52)
// 근본 원인 (root cause)

java.lang.IllegalArgumentException: No converter found for return value of type: class kr.or.connect.webmvc.dto.PlusResult
org.springframework.web.servlet.mvc.method.annotation.AbstractMessageConverterMethodProcessor.writeWithMessageConverters(AbstractMessageConverterMethodProcessor.java:187)
org.springframework.web.servlet.mvc.method.annotation.RequestResponseBodyMethodProcessor.handleReturnValue(RequestResponseBodyMethodProcessor.java:174)
org.springframework.web.method.support.HandlerMethodReturnValueHandlerComposite.handleReturnValue(HandlerMethodReturnValueHandlerComposite.java:81)
org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:132)
org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:827)
org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:738)
org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:85)
org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:963)
org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:897)
org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:970)
org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:861)
javax.servlet.http.HttpServlet.service(HttpServlet.java:634)
org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:846)
javax.servlet.http.HttpServlet.service(HttpServlet.java:741)
org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52)
// 비고 근본 원인(root cause)의 풀 스택 트레이스를, 서버 로그들에서 확인할 수 있습니다.
```

에러메시지를 보면 `No converter found for return value of type: class kr.or.connect.webmvc.dto.PlusResult`와 같은 내용이 보입니다. 

`PlusResult`에 대한 컨버터가 없다는 것입니다. `DispathcerServlet`은 컨트롤러 메소드를 실행하고 해당 메소드가 리턴한 객체를 변환시키려고 합니다. 변환을 시킬 때 `메시지 컨버터(MessageConverter)`를 사용하게 되는데 메시지 컨버터가 `Bean`으로 등록 되어 있지 않으면 위와 같은 오류를 출력하게 됩니다. 

위와 같은 오류가 발생하지 않도록 하려면 `MessageConverter`를 `Bean`으로 등록해줘야 합니다. 보통 **Web API는 JSON, XML 과 같은 데이터를 표현하기에 알맞은 형태로 결과를 출력**합니다. `PlusResult`를 JSON메시지로 변환하려면 pom.xml 파일에 다음의 라이브러리를 추가합니다.

```xml
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.10.2</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.10.2</version>
        </dependency>
```

jackson 라이브러리는 객체를 JSON으로 또는 JSON을 객체로 변환시킬 때 주로 사용합니다. 

2개의 라이브러리를 포함시킨 후 서버를 재시작 하고, http://localhost:8080/mvcexam/api/plus?value1=10&value2=20라고 다시 입력합니다.

이번엔 다음과 같이 JSON메시지가 브라우저에 출력되는 것을 확인할 수 있습니다. 에러가 발생하지 않았다는 것은 메시지 컨버터가 자동으로 등록되었다는 것을 의미합니다.

```json
{"value1":10,"value2":20,"result":30}
```

이렇게 **Web 환경을 통해 제공되는 데이터 CRUD**를 할 수 있는, 2개의 정수값을 전달하여 결과를 구하는 `Web API`를 만들 수 있습니다.



---

참고 : https://www.boostcourse.org/web326/lecture/58987/?isDesc=false

