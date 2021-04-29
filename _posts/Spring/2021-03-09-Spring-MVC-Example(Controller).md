---
title: "Spring : MVC 예제(Controller작성)"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Controller"
  - "Spring : MVC"
toc: true
toc_sticky: true
toc_label: 목차
---

## MVC 예제(Controller작성)



## Controller작성 1/3

1. 웹 브라우저에서 http://localhost:8080/mvcexam/plusform 이라고 요청을 보 내면 서버는 웹 브라우저에게 2개의 값을 입력받을 수 있는 입력 창과 버튼이 있는 화면을 출력합니다.
2. 웹 브라우저에 2개의 값을 입력하고 버튼을 클릭하면 http://localhost:8080/mvcexam/plus URL로 2개의 입력값이 POST방식으로 서버에게 전달한다. 서버는 2개의 값을 더한 후, 그 결과 값을 JSP에게 request scope으로 전달하여 출력합니다.

 

### **Spring MVC가 지원하는 Controller메소드 인수 타입**

bold한 것은 앞으로 사용할 것들 입니다.

- javax.servlet.ServletRequest
- **javax.servlet.http.HttpServletRequest**
- org.springframework.web.multipart.MultipartRequest
- org.springframework.web.multipart.MultipartHttpServletRequest
- javax.servlet.ServletResponse
- **javax.servlet.http.HttpServletResponse**
- **javax.servlet.http.HttpSession**
- org.springframework.web.context.request.WebRequest
- org.springframework.web.context.request.NativeWebRequest
- java.util.Locale
- java.io.InputStream
- java.io.Reader
- java.io.OutputStream
- java.io.Writer
- javax.security.Principal
- java.util.Map
- org.springframework.ui.Model
- org.springframework.ui.ModelMap
- **org.springframework.web.multipart.MultipartFile**
- javax.servlet.http.Part
- org.springframework.web.servlet.mvc.support.RedirectAttributes
- org.springframework.validation.Errors
- org.springframework.validation.BindingResult
- org.springframework.web.bind.support.SessionStatus
- org.springframework.web.util.UriComponentsBuilder
- org.springframework.http.HttpEntity<?>
- Command 또는 Form 객체

 

### Spring MVC가 지원하는 메소드 인수 애노테이션

- **@RequestParam**
- **@RequestHeader**
- **@RequestBody**
- @RequestPart
- **@ModelAttribute**
- **@PathVariable**
- @CookieValue

#### @RequestParam

- Mapping된 메소드의 Argument에 붙일 수 있는 어노테이션
- `@RequestParam`의 name에는 `http parameter`의 name과 멥핑
- `@RequestParam`의 `required`는 필수인지 아닌지 판단

#### @PathVariable

- `@RequestMapping`의 path에 변수명을 입력받기 위한 place holder가 필요함
- place holder의 이름과 `PathVariable`의 name 값과 같으면 mapping 됨
- `required` 속성은 default true 임

#### @RequestHeader

- 요청 정보의 헤더 정보를 읽어들 일 때 사용
- `@RequestHeader(name="헤더명") String 변수명`

 

### Spring MVC가 지원하는 메소드 리턴 값

- **org.springframework.web.servlet.ModelAndView**
- org.springframework.ui.Model
- java.util.Map
- org.springframework.ui.ModelMap
- org.springframework.web.servlet.View
- **java.lang.String**
- java.lang.Void
- org.springframework.http.HttpEntity<?>
- org.springframework.http.ResponseEntity<?>
- **기타 리턴 타입**



### 예제



#### plusForm.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<%-- "plus"라는 url을 이용해 post방식으로 데이터를 전달할 수 있습니다. --%>
<form method="post" action="plus">  
value1 : <input type="text" name="value1"><br>
value2 : <input type="text" name="value2"><br>
<input type="submit" value="확인">  

</body>
</html>
```



#### plusResult.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
${value1} 더하기 ${value2 } (은/는) ${result } 입니다.
</body>
</html>
```



### PlusController.java

```java
package kr.or.connect.mvcexam.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class PlusController {
	
	// plusform이라는 url요청이 Get방식으로 들어오면 해당 메서드가 실행됩니다.
	@GetMapping(path="/plusform")
	public String plusform() {
		// jsp 파일이름을 리턴하면 "WebMvcContextConfiguration" 파일에서 viewResolver가 경로와 .jsp를 붙여줍니다.
		return "plusForm";
	}
	
	
	@PostMapping(path="/plus")
	// @RequestParam은 Spring MVC가 지원하는 인수의 어노테이션 입니다.
	// name은 http parameter의 name과 매핑되고 required는 필수인지 아닌지 판단 해주는 것입니다.
	// 즉 @RequestParam(name = "value1", required = true) int value1은
	//  http parameter에서 가져온 값을 여기서 int value1로 사용한다는 의미입니다.
	
	// HttpServletRequest를 사용하면 Web에 종속되기 때문에 스프링에서 제공하고 있는 ModelMap 객체를 사용합니다.
	// ModelMap에 넣어주면 알아서 스프링이 request scope에다 알아서 매핑시켜줍니다.
	public String plus(@RequestParam(name = "value1", required = true) int value1,
			@RequestParam(name = "value2", required = true) int value2, ModelMap modelMap) {
		int result = value1 + value2;
		
		modelMap.addAttribute("value1", value1);
		modelMap.addAttribute("value2", value2);
		modelMap.addAttribute("result", result);
		return "plusResult";
	}
}
```



## Controller작성 실습 2/3

1. http://localhost:8080/mvcexam/userform 으로 요청을 보내면 이름, email, 나이를 물어보는 폼이 보여집니다.
2. 폼에서 값을 입력하고 확인을 누르면 post방식으로 http://localhost:8080/mvcexam/regist 에 정보를 전달하게 됩니다.
3. regist에서는 입력받은 결과를 콘솔 화면에 출력합니다.



### 예제

#### userForm.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<form method="post" action="regist">  
name : <input type="text" name="name"><br>
email : <input type="text" name="email"><br>
age : <input type="text" name="age"><br>
<input type="submit" value="확인"> 

</body>
</html>
```

 

#### regist.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h2>등록되었습니다.</h2>
</body>
</html>
```



#### User.java

```java
package kr.or.connect.mvcexam.dto;

public class User {
	// 속성의 이름들이 userForm의 name과 일치하고
	// getter,setter를 만들어줘야 Spring에서 자동으로 값을 넣을 수 있습니다.
	private String name;
	private String email;
	private int age;
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	
	@Override
	public String toString() {
		return "User [name=" + name + ", email=" + email + ", age=" + age + "]";
	}
}
```



#### UserController.java

```java
package kr.or.connect.mvcexam.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import kr.or.connect.mvcexam.dto.User;

@Controller
public class UserController {
	@RequestMapping(path="/userform", method = RequestMethod.GET)
	public String userform() {
		return "userForm";
	}
	
	@RequestMapping(path = "/regist", method = RequestMethod.POST)
	// @ModelAttribute를 이용하면 Spring에서 userForm의 name들을 받아 User객체를 생성해 값을 넣어줍니다. 
	// User라는 DTO를 만들어서 값을 받아줍니다.
	public String regist(@ModelAttribute User user) {
		
		System.out.println("사용자가 입력한 user 정보입니다. 해당 정보를 이용하는 코드가 와야합니다.");
		System.out.println(user);
		return "regist";
	}
}
```



## Controller작성 실습 3/3

1. http://localhost:8080/mvcexam/goods/{id} 으로 요청을 보냅니다. (실제로 입력할 땐 그냥 정수값)
2. 서버는 id를 콘솔에 출력하고, 사용자의 브라우저 정보를 콘솔에 출력합니다.
3. 서버는 HttpServletRequest를 이용해서 사용자가 요청한 PATH정보를 콘솔에 출력합니다.



### 예제

#### goodsById

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
id : ${id } <br>
user_agent : ${userAgent }<br>
path : ${path }<br>
</body>
</html>
```



#### GoodsController.java

```java
package kr.or.connect.mvcexam.controller;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;

@Controller
public class GoodsController {
	// path variable로 받겠다는 의미입니다.
	@GetMapping("/goods/{id}")
	// "@PathVariable(name="id") int id"는 id라는 이름으로 넘어온 값을 int id로 받아서 여기서 사용하겠다는 의미입니다.
	// "@RequestHeader(value="User-Agent", defaultValue = "myBrowser") String userAgent"는 
	// 헤더에서 넘어오는 정보를 꺼내서 String userAgent에 저장합니다.
	// 종속되는 부분 때문에 HttpServletRequest는 사용하지 않는 게 좋지만 사용할 수 있는 것을 볼 수 있습니다.
	public String getGoodsById(@PathVariable(name="id") int id,
								@RequestHeader(value="User-Agent", defaultValue = "myBrowser") String userAgent,
								HttpServletRequest request,
								ModelMap model) {
		String path = request.getServletPath();
				
		System.out.println("id : " + id);
		System.out.println("user_agent : " + userAgent);
		System.out.println("path : " + path);
		
		model.addAttribute("id", id);
		model.addAttribute("userAgent", userAgent);
		model.addAttribute("path", path);
		
		return "goodsById";
	}
}
```



---

참고 : https://www.boostcourse.org/web326/lecture/258536?isDesc=false