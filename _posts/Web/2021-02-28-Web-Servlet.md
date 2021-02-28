---
title: "Web : Servlet"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Web : Servlet"
toc: true
toc_sticky: true
toc_label: 목차
---

# 자바 웹 어플리케이션(Java Web Application)

WAS에 설치(deploy)되어 동작하는 어플리케이션입니다.

자바 웹 어플리케이션에는 HTML, CSS, 이미지, 자바로 작성된 클래스(Servlet도 포함됨, package, 인터페이스 등), 각종 설정 파일 등이 포함됩니다.

 

**자바 웹 어플리케이션의 폴더 구조**

[![img](https://cphinf.pstatic.net/mooc/20180124_133/15167752967943AqfC_PNG/1_5_1_____.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/58954?isDesc=false#)



## 서블릿이란?

- 자바 웹 어플리케이션의 구성요소 중 동적인 처리를 하는 프로그램의 역할입니다.

- 서블릿을 정의해보면 서블릿(servlet)은 WAS에 동작하는 JAVA 클래스입니다. 

- 서블릿은 HttpServlet 클래스를 상속받아야 합니다.

서블릿과 JSP로부터 최상의 결과를 얻으려면, 웹 페이지를 개발할 때 이 두 가지(JSP, 서블릿)를 조화롭게 사용해야 합니다.

예를 들어, 웹 페이지를 구성하는 화면(HTML)은 JSP로 표현하고, 복잡한 프로그래밍은 서블릿으로 구현합니다.



### Servlet 작성방법

서블릿의 작성방법은 서블릿 버전에 따라서 두 가지로 나누어 집니다.



#### Servlet 3.0 spec 이상에서 사용하는 방법

- web.xml 파일을 사용하지 않습니다.

- 자바 어노테이션(annotation)을 사용합니다.

- `Dynamic Web Project`를 사용합니다.

`Eclipse`에서 `Dynamic Web Project` 를 만들고 `Servlet`을 만듭니다.

```java
//TenServlet.java

package exam;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/Ten")
// 이 부분을 수정하면 url을 변경할 수 있습니다.
public class TenServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public TenServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// HttpServletRequest 요청을 하기 위한 객체,  HttpServletResponse 응답을 하기 위한 객체
		
		response.setContentType("text/html;charset=UTF-8");
		// 브라우저가 응답을 받았을 때 이미지인지 동영상인지 무엇인지를 알려주는 메서드
		
		PrintWriter out = response.getWriter();
		// PrintWriter는 콘솔이아니고 브라우저의 응답결과로 보내줍니다.
        
		out.print("<h1>1-10까지 출력!<h1>");
		// print나 println중 고민하지 않아도 됩니다! -> html은 개행을 '\n'가아닌 <br>로 구분하기 때문에!
		for (int i = 1; i <= 10; i++) {
			out.print(i+"<br>");
		}
		out.close();
	}

}
```



#### Servlet 3.0 spec미만에서 사용하는 방법

- servlet을 등록할 때 web.xml 파일에 등록합니다.

```java
// TenServlet.java
package exam;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


// 2.5 버전은 3.0이상의 버전과 다르게 어노테이션이 없습니다.
public class TenServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

    public TenServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=UTF-8");
		
		PrintWriter out = response.getWriter();
		out.print("<h1>1-10까지 출력!<h1>");
		for (int i = 1; i <= 10; i++) {
			out.print(i+"<br>");
		}
		out.close();
	}
}
```



```xml
<!-- web.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>exam25</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  
  
  <servlet>
    <description></description>
    <display-name>TenServlet</display-name>
    <servlet-name>TenServlet</servlet-name>
    <servlet-class>exam.TenServlet</servlet-class>
    <!-- url을 요청하게 되면 <servlet>이라는 태그 안에서 servlet-name이 같은 서블릿을 찾아 <servlet-class>의 서블릿을 실행시켜줍니다. -->  
  </servlet>
  <servlet-mapping>
    <servlet-name>TenServlet</servlet-name>
    <url-pattern>/Ten</url-pattern>
    <!-- 서블릿은 요청이 들어왔을 때 반드시 서블릿 이름으로 요청하지 않기 때문에 -->
  	<!-- 여기서 url을 찾지못하면 404라는 페이지가 나옵니다. -->
  	<!-- <url-pattern>로 url을 변경할 수 있습니다. -->
  </servlet-mapping>
  <!-- 3.0미만의 버전은 어노테이션을 사용하지 않기 때문에 servlet을 추가하면 web.xml에 이 부분이 만들어집니다. -->
  <!-- web.xml파일이 바뀌면 서버는 restart 되어야 합니다. -->
  
  
</web-app>
```



## Servlet Life Cycle

HttpServlet의 3가지 메소드를 오버라이딩해 `Servlet Life Cycle` 을 알아보겠습니다

- init()
- service(request, response)
- destroy()

```java
// LifecycleServlet.java
package examples;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/LifecycleServlet")
public class LifecycleServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public LifecycleServlet() {
    	System.out.println("LifecycleServlet 생성!!");
    }

	public void init(ServletConfig config) throws ServletException {
		System.out.println("init test 호출!!");
	}

	public void destroy() {
		System.out.println("destroy 호출!!");
	}

	 protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	 System.out.println("service 생성!!"); }
}
```

[![img](https://cphinf.pstatic.net/mooc/20180124_22/1516782982944xjogH_PNG/1_5_3_ServletLifcycle.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258505/?isDesc=false#)



### Servlet 생명주기

1. 서버는 클라이언트가 요청한 URL을 받아 URL에 맵핑된 LifecycleServlet의 정보를 알아냅니다.
2. 그 다음 해당 클래스가 메모리에 존재하는지 체크를 합니다.
3. 메모리에 존재하지 않는다면 생성자가 실행되면서 객체가 만들어지고 생성자 메세지가 출력되는 것을 볼 수 있습니다.
4. 생성자가 실행되고난 후 init메서드가 실행된 것을 확인할 수 있습니다.
5. 브라우저를 새로고침 하면 생성자,init,destroy는 호출되지 않고 service만 호출되는 것을 볼 수 있습니다.
   - 서블릿은 서버에 객체를 여러 개 만들지 않기 때문에 요청이 들어오면 서블릿 객체가 메모리에 있는 지 체크 후 있다면 service 메서드만 호출 합니다.
   - 새로고침하면 service만 실행되기 때문에 응답해야 하는 내용은 service에 넣어주어야 합니다.
6. 서블릿을 조금 수정하고 저장하면 서블릿은 수정됐기 때문에 지금 메모리에 올라가 있는 서블릿 객체는 더 이상 사용할 수 없습니다.
7. 이 때, 콘솔을 확인하면 destroy()라는 메서드가 호출되고 있는 것을 볼 수 있습니다.
8. 그리고 다시 브라우저를 새로고침 해보면 생성자,init을 호출하면서 객체가 다시 생성되는 것을 볼 수 있습니다.



#### service(request, response) 메소드

응답해야 하는 내용은 service에 넣어주어야 한다고 했습니다. 하지만 doget을 사용할 경우에는 service가 없는데 어떻게 실행이 될까요?

실행이 되는 이유는 service라는 메서드가 실제 HttpServlet에 이미 구현이 되어 있기 때문입니다. 그래서 service라는 메소드를 override하지 않았다면 부모클래스인 HttpServlet의 service가 실행되는 것입니다.

HttpServlet의 service메소드는 템플릿 메소드 패턴으로 구현합니다.

- 클라이언트의 요청이 GET일 경우에는 자신이 가지고 있는 doGet(request, response)메소드를 호출
- 클라이언트의 요청이 Post일 경우에는 자신이 가지고 있는 doPost(request, response)를 호출

[템플릿 메소드 패턴에 대해 잘 나와 있는 블로그](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)



##### doGet, doPost 예제

- 생명주기 예제에서 Service(request, response)메소드 주석처리 해서 사용하겠습니다.
- HttpServlet의 doGet(request, response)메소드 오버라이딩
- HttpServlet의 doPost(request, response)메소드 오버라이딩

```java
// LifecycleServlet.java
package examples;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/LifecycleServlet")
public class LifecycleServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public LifecycleServlet() {
    	System.out.println("LifecycleServlet 생성!!");
    }

	public void init(ServletConfig config) throws ServletException {
		System.out.println("init test 호출!!");
	}

	public void destroy() {
		System.out.println("destroy 호출!!");
	}

	/*
	 * protected void service(HttpServletRequest request, HttpServletResponse
	 * response) throws ServletException, IOException {
	 * System.out.println("service 생성!!"); }
	 */
	
	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<html>");
		out.println("<head><title>form</title></head>");
		out.println("<body>");
		out.println("<form method='post' action='/firstweb/LifecycleServlet'>");
		out.println("name : <input type='text' name='name'><br>");
		out.println("<input type='submit' value='ok'><br>");                                                 
		out.println("</form>");
		out.println("</body>");
		out.println("</html>");
		out.close();
	}
	
	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		String name = request.getParameter("name");
		out.println("<h1> hello " + name + "</h1>");
		out.close();
	}

}
```

- URL주소를 직접입력하거나 링크를 클릭하는 것은 GET방식으로 서버에게 요청을 보내는 것입니다.
  - 이런 경우 service() 메서드가 자신의 doGet() 메서드를 호출합니다.

- GET방식으로 호출된 화면에서 post방식으로 서버에 요청하면 service()는 자신의 doPost()를 호출합니다.

---

참고 : https://www.boostcourse.org/web326/lecture/258505/?isDesc=false