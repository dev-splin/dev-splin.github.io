---
title: "Web : Request/Response"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Web : Request/Response"
toc: true
toc_sticky: true
toc_label: 목차
---

# Request/Response

웹 브라우저에 URL을 입력하고 Enter를 입력하면 도메인과 포트번호를 이용해서 서버에 접속합니다. 그리고 나서 path정보, 클라이언트의IP, 클라이언트의 다양한 정보를 포함한 요청 정보를 서버에게 전송하게 됩니다.



[![img](https://cphinf.pstatic.net/mooc/20180124_79/15167843899250uB2H_PNG/1_5_4_request_response.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258511?isDesc=false#)



## 요청과 응답

- WAS는 웹 브라우저로부터 Servlet요청을 받으면 요청할 때 가지고 있는 정보를 **HttpServletRequest**객체를 생성하여 저장합니다.

- 웹 브라우저에게 응답을 보낼 때 사용하기 위하여 **HttpServletResponse**객체를 생성합니다.
- 생성된 HttpServletRequest, HttpServletResponse 객체를 path로 매핑된 서블릿에게 전달합니다.
- 이렇게 전달한 객체는 `service()`, `doGet()`, `doPost()`같은 메서드에 파라미터로 전달돼서 사용하게 됩니다.

 

### HttpServletRequest

- http프로토콜의 request정보를 서블릿에게 전달하기 위한 목적으로 사용합니다.
- 헤더정보, 파라미터, 쿠키, URI, URL 등의 정보를 읽어 들이는 메소드를 가지고 있습니다.
  - 요청할 때 가지고 있는 모든 정보들을 메서드로 담습니다. (심지어 요청한 사용자가 어떤 언어를 사용하고 있느냐 같은 정보들 까지)
- Body의 Stream을 읽어 들이는 메소드를 가지고 있습니다.



### HttpServletResponse

- WAS는 어떤 클라이언트가 요청을 보냈는지 알고 있고, 해당 클라이언트에게 응답을 보내기 위한 HttpServleResponse객체를 생성하여 서블릿에게 전달합니다.
- 서블릿은 해당 객체를 이용하여 content type, 응답코드, 응답 메시지등을 전송합니다.



#### 예제1

Request객체에 들어있는 모든 정보값을 가져와 출력합니다.

```java
package examples;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/header")
public class HeaderServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

    public HeaderServlet() {
        super();
    }


	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<html>");
		out.println("<head><title>form</title></head>");
		out.println("<body>");

		Enumeration<String> headerNames = request.getHeaderNames();
		// getHeaderNames는 모든 헤더이름을 Enumeration객체로 반환해줍니다. 
		while(headerNames.hasMoreElements()) {
			String headerName = headerNames.nextElement();
			// 헤더의 name을 가져오고
			String headerValue = request.getHeader(headerName);
			// 헤더의 이름을 통해서 값 정보를 가져올 수 있습니다.
			out.println(headerName + " : " + headerValue + " <br> ");
		}		
		
		out.println("</body>");
		out.println("</html>");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
	// 기본적으로 doPost를 override하면 doGet을 호출하고 있기 때문에 Get에 다가 내용을 구현하면 Post를 해도 같은 내용이 나옵니다.
}
```

```html
<!-- 결과를 보면 헤더 : 헤더의 값 형식으로 나오는 것을 볼 수 있습니다. -->
accept : image/gif, image/jpeg, image/pjpeg, application/x-ms-application, application/xaml+xml, application/x-ms-xbap, */* 
 accept-language : ko 
 cache-control : no-cache 
 ua-cpu : AMD64 
 accept-encoding : gzip, deflate 
 user-agent : Mozilla/5.0 (Windows NT 6.2; Win64; x64; Trident/7.0; rv:11.0) like Gecko 
 host : localhost:8080 
 connection : Keep-Alive 
```



#### 예제2

URL주소의 파라미터 정보를 읽어 출력합니다.

만약 http://localhost:8080/firstweb/param?name=kim&age=5 이 있다면? 뒤에 있는 `name=kim&age=5`가 파라미터입니다.  &를 계속 사용하면 더 많은 파라미터를 전달할 수 있습니다.

```java
package examples;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/Param")
public class ParameterServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public ParameterServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<html>");
		out.println("<head><title>form</title></head>");
		out.println("<body>");

		String name = request.getParameter("name");
		String age = request.getParameter("age");
		
		out.println("name : " + name + "<br>");
		out.println("age : " +age + "<br>");
		// 그냥 실행하게 되면 값이 null이 나옵니다.
		// form의 input태그를 사용하거나 url에 파라미터를 넘겨주어야 값이 나옵니다.
		
		out.println("</body>");
		out.println("</html>");
	}
}
```



#### 예제3

Request가 가지고 있는 다양한 값들을 출력합니다.

```java
// firstweb프로젝트의 info.java
package examples;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/Info")
public class InfoServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public InfoServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<html>");
		out.println("<head><title>info</title></head>");
		out.println("<body>");

		String uri = request.getRequestURI();
		// 도메인과 포트 이하에(오른쪽) 있는 값을 가져옵니다.
		StringBuffer url = request.getRequestURL();
		// 요청 주소 전체를 볼 수 있습니다.
		String contentPath = request.getContextPath();
		// 웹 어플리케이션과 매핑된 path를 가져옵니다.
		String remoteAddr = request.getRemoteAddr();
		// 클라이언트의 IP주소값을 가져옵니다.
		
		
		out.println("uri : " + uri + "<br>");
		out.println("url : " + url + "<br>");
		out.println("contentPath : " + contentPath + "<br>");
		out.println("remoteAddr : " + remoteAddr + "<br>");
		
		out.println("</body>");
		out.println("</html>");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
}
```

```html
<!-- 결과 -->
uri : /firstweb/Info
url : http://localhost:8080/firstweb/Info
contentPath : /firstweb
remoteAddr : 0:0:0:0:0:0:0:1
```

결과를 보면 uri와 url의 차이를 알 수 있습니다. 또, 기본적으로 현재 프로젝트 이름이contextPath로 지정이 되는 것을 볼 수 있습니다.

ip가 0:0 형식으로 나오고 있는 이유는 로컬 컴퓨터에서 접속했기 때문입니다. 실제 로컬 컴퓨터에서 접속하면 운영체제에 따라서 `127.0.0.1` 혹은 `localhost` 가 출력될 수도 있지만, 운영체제가 IPv6같은 경우는 지금 처럼 `0:0:0:0:0:0:0:1` 이 출력됩니다.



---

참고 : https://www.boostcourse.org/web326/lecture/258514?isDesc=false