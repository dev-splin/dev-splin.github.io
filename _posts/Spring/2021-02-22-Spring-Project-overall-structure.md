---
title: "Spring : 프로젝트 전체구조"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : MVC"
toc: true
toc_sticky: true
toc_label: 목차
---

# 프로젝트 전체 구조

![1](https://user-images.githubusercontent.com/58713853/101177159-d614b480-368a-11eb-8a2d-b3158e2facf2.PNG)



## web.xml

![2](https://user-images.githubusercontent.com/58713853/101177164-d745e180-368a-11eb-822c-75c8d14c83f5.PNG)



## DispatcherServlet

![3](https://user-images.githubusercontent.com/58713853/101177166-d745e180-368a-11eb-9aa8-9f84f194bd95.PNG)



## servlet-context.xml

![4](https://user-images.githubusercontent.com/58713853/101177169-d7de7800-368a-11eb-9cef-fd55ed469370.PNG)

***

![5](https://user-images.githubusercontent.com/58713853/101177172-d7de7800-368a-11eb-9590-02c593bb2d5e.PNG)

***

![6](https://user-images.githubusercontent.com/58713853/101177173-d8770e80-368a-11eb-8c5b-bd76d3144a07.PNG)



## Controller (컨트롤러)

![7](https://user-images.githubusercontent.com/58713853/101177174-d8770e80-368a-11eb-889e-6e065cfdca3a.PNG)

***

![8](https://user-images.githubusercontent.com/58713853/101177177-d90fa500-368a-11eb-9e20-6903f7062b44.PNG)



## View (뷰)

![9](https://user-images.githubusercontent.com/58713853/101177179-d9a83b80-368a-11eb-9923-6f6cdefda53c.PNG)



### Controller와 View 만들어보기

- 브라우저에서 "/login" url 을 입력하면 "loginValue" 라는 값이 나오게 한다.

```java
// HomeController와 같은 폴더에 loginController 만들기
// import 부분 생략

@Controller
public class login {

  @RequestMapping(value = "/login", method = RequestMethod.GET) // url에 login으로 인식하게 해준다.
  //@RequestMapping("/login")   // method 부분을 생략할 수 있고, 값이 하나일 경우에는 value도 생략할 수 있다.
  public String login(Model model) {
    model.addAttribute("loginKey", "loginValue"); // model객체에 key와 value를 추가해 넘겨준다.
    
    return "login"; // login.jsp 가 된다.
  }
  
}

```

```jsp
<%-- login.jsp를 만들어준다. --%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Login</title>
</head>
<body>
<h1>
	Hello world!  
</h1>

<P>  Login is ${loginKey}. </P> <%-- 브라우저 상에서는 "loginKey" 라는 키의 값에 들어있는 "loginValue"가 나온다. --%>
</body>
</html>

```
