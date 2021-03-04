---
title: "Web : Servlet에서 JSP 호출 / 데이터 주고 받을 때 한글 깨짐 현상 해결"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Servlet : Error"
toc: true
toc_sticky: true
toc_label: 목차
---

# Servlet에서 JSP 호출 / 데이터 주고 받을 때 한글 깨짐 현상 해결



## Servlet에서 JSP 호출

Servlet에서 JSP를 호출하는 방법은 두가지가 있습니다.



### Redirect 방식

서버가 클라이언트에게 페이지 호출을 요청하는 방식입니다. 데이터는 문자열 형식만 전송 가능합니다.



#### Servlet에서 JSP에게 데이터를 보내기

```java
String data = request.getParameter("data");
response.sendRedirect("jspFile.jsp?data="+data);
```

파라미터와 함께 보낼 수도 있습니다.



#### JSP에서 Servlet 데이터를 받기

```jsp
<%
String data = request.getParameter("data");
%>
<%-- 스크립트릿으로 받아오는 방법 --%>

${param.data}
<%-- EL(표현언어)로 받아오는 방법 --%>
```



### RequestDispatcher클래스 사용하는 방식

서버가 직접 페이지를 호출하는 방식입니다. 객체 데이터 전송도 가능합니다. (request, response객체 모두 전송 가능합니다.)  RequestDispatcher클래스에서 forward방식과 include방식으로 또 나누어집니다.



#### forward 방식

```java
String data = request.getParameter("data");		
request.setAttribute("dataobject", data); //객체를 request객체에 담음 (data가 문자열이 아니어도 가능)

RequestDispatcher dispatcher = request.getRequestDispatcher("jspFile.jsp");
dispatcher.forward(request, response);
```

forward()메소드를 이용하면 JSP페이지를 호출하는 순간 서블릿 프로그램은 실행을 멈추고 JSP페이지로 넘어가 그곳에서 실행하고 프로그램이 끝나게 됩니다. (서버의 다른 리소스(서블릿, JSP 파일 또는 HTML 파일)로 요청을 전달합니다)



#### include

```java
RequestDispatcher dispatcher = request.getRequestDispatcher("jspFile.jsp");
dispatcher.include(request, response);
```

include()메소드를 이용하면 해당 JSP페이지가 실행되고 다시 나머지 서블릿 프로그램이 실행됩니다. (응답에 리소스 내용(서블릿, JSP 페이지, HTML 파일)을 포함합니다.)



## 데이터 주고 받을 때 한글깨짐 현상 해결

데이터를 요청하고 응답하다가 한글이 깨지는 현상이 발생했습니다.

```html
name :  ì¤ì°½í

ííí... dispatcherê° ìëì..

regdate :  2021-03-03
```



### 해결 방법

위와 같이 서블릿으로 요청을 통해 한글 데이터를 받을 때 GET방식은 깨질수도 아닐수도, POST 방식은 무조건 한글이 깨지는데 이는 아래와 같이 하면 해결됩니다.

```java
// 1. 받을 때 : 한글깨짐 해결(post 방식)
request.setCharacterEncoding("utf-8");    

// 2. 받을 때 : GET방식 처리
String filename = 
    new  String(request.getParameter("parameter").getBytes("8859_1"),"KSC5601");   
// 3. 보낼 때 한글처리
response.setContentType("text/html;charset=utf-8");     
```



---

참고 : https://kyun2.tistory.com/42

https://sourcestudy.tistory.com/294

https://docs.oracle.com/javaee/7/api/javax/servlet/RequestDispatcher.html

https://elephant11.tistory.com/162
