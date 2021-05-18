---
title: "Spring : Cookie / Session"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Cookie"
  - "Spring : Session"
toc: true
toc_sticky: true
toc_label: 목차
---

# 쿠키(Cookie)와 세션(Session)

**HTTP프로토콜은 상태 유지가 안되는 프로토콜**입니다. 이전에 무엇을 했고, 지금 무엇을 했는지에 대한 정보를 갖고 있지 않습니다. 웹 브라우저(클라이언트)의 요청에 대한 응답을 하고 나면 해당 클라이언트와의 연결을 지속하지 않습니다. 이 때, 웹에서의 상태 유지를 위해 `Cookie`와 `Session`기술이 등장합니다.

 

## 쿠키

- 사용자 컴퓨터에 저장
- 저장된 정보를 다른 사람 또는 시스템이 볼 수 있는 단점
- 유효시간이 지나면 사라짐

 

[<img src="https://cphinf.pstatic.net/mooc/20180221_5/1519187850598AmEe1_PNG/1.png?type=w760" alt="img" style="zoom: 80%;" />](https://www.boostcourse.org/web326/lecture/58991?isDesc=false#)



[<img src="https://cphinf.pstatic.net/mooc/20180221_188/1519187853247UDkY0_PNG/2.png?type=w760" alt="img" style="zoom:80%;" />](https://www.boostcourse.org/web326/lecture/58991?isDesc=false#)



### 정의

- **클라이언트 단에 저장되는 작은 정보의 단위**입니다.
- 클라이언트에서 생성하고 저장될 수 있고, 서버 단에서 전송한 쿠키가 클라이언트에 저장될 수 있습니다.

 

### 이용 방법

- **서버에서 클라이언트의 브라우저로 전송되어 사용자의 컴퓨터에 저장**합니다.
- 저장된 쿠키는 다시 해당하는 웹 페이지에 접속할 때, 브라우저에서 서버로 쿠키를 전송합니다.
- 쿠키는 **이름(name)과 값(value) 쌍으로 정보를 저장**합니다.
  - 이름-값 쌍 외에도 `주석(comment), 도메인(Domain), 경로(Path), 유효기간(Max-Age, Expires), 보안(Secure), HttpOnly 속성`을 저장할 수 있습니다.

 

### 제한

- **하나의 쿠키는 4K Byte 크기로 제한**

- 브라우저는 각각의 웹사이트 당 20개의 쿠키를 허용
  - 모든 웹 사이트를 합쳐 최대 300개를 허용
  - 클라이언트 당 쿠키의 최대 용량이 1.2M Byte가 됨

 

### javax.servlet.http.Cookie

**서버에서 쿠키 생성, Response의 addCookie메소드를 이용해 클라이언트에게 전송**할 수 있습니다.

```java
Cookie cookie = new Cookie(이름, 값);
response.addCookie(cookie);
```

- **쿠키는 (이름, 값)의 쌍 정보를 입력하여 생성**합니다.
- 쿠키의 이름은 일반적으로 알파벳과 숫자, 언더바로 구성합니다. 
  - 공백, 괄호, 등호, 콤마, 콜론, 세미콜론 등은 포함 불가능
  - 정확한 정의를 알고 싶다면 [RFC 6265 문서](https://tools.ietf.org/html/rfc6265) [4.1.1 Syntax] 항목을 참조

 

#### 클라이언트가 보낸 쿠키 정보 읽기

```java
// 하나의 서버가 쿠키를 여러 개 보낼 수 있기 때문에 배열로 받습니다.
Cookie[] cookies = request.getCookies();
```

- **쿠키 값이 없으면 null이 반환**됩니다.
- **Cookie가 가지고 있는 getName()과 getValue()메소드를 이용해서 원하는 쿠키정보를 찾아 사용**합니다.

 

#### 클라이언트에게 쿠키 삭제 요청

- **쿠키의 관리는 웹 클라이언트가 하기 때문에 쿠키를 삭제하는 명령은 따로 없고, maxAge가 0인 같은 이름의 쿠키를 전송**합니다.
  - **클라이언트에서 같은 이름을 가진 쿠키가 두 개 존재할 수 없기 때문**에 쿠키 자체를 유지 시간이 0인 쿠키로 교체하게 되고, 유지 시간이 0이기 때문에 해당 쿠키가 없어집니다.

```java
Cookie cookie = new Cookie("이름", null);
cookie.setMaxAge(0);
response.addCookie(cookie);
```

 

##### 쿠키의 유효기간 설정

[![img](https://cphinf.pstatic.net/mooc/20180221_109/1519193077699vJM62_PNG/1.png?type=w760)](https://www.boostcourse.org/web326/lecture/58992?isDesc=false#)



###### setMaxAge() 메소드

- 인자는 유효기간을 나타내는 초 단위의 정수형
  - 유효기간 10분 : cookie.setMaxAge(10 * 60); //초 단위 : 10분
  - 유효기간 1주일 : cookie.setMaxAge(7*24*60*60);
- 만일 유효기간을 0으로 지정하면 쿠키의 삭제
- 음수를 지정하면 브라우저가 종료될 때 쿠키가 삭제



#### Spring MVC에서의 Cookie 사용

- `@CookieValue` 어노테이션 사용
- 컨트롤러 메소드의 파라미터에서 `@CookieValue`어노테이션을 사용함으로써 원하는 쿠키정보를 파라미터 변수에 담아 사용할 수 있습니다.
- 컨트롤러메소드(@CookieValue(value="쿠키이름", required=false, defaultValue="기본값") String 변수명)

 



## 세션

- 서버에 저장
- 서버가 종료되거나 유효시간이 지나면 사라집니다.



[<img src="https://cphinf.pstatic.net/mooc/20180221_246/15191878577834bPNF_PNG/3.png?type=w760" alt="img" style="zoom:80%;" />](https://www.boostcourse.org/web326/lecture/58991?isDesc=false#)



[<img src="https://cphinf.pstatic.net/mooc/20180221_236/15191878600705qUuz_PNG/4.png?type=w760" alt="img" style="zoom:80%;" />](https://www.boostcourse.org/web326/lecture/58991?isDesc=false#)



### 정의

- 클라이언트 별로 서버에 저장되는 정보입니다.

 

### 이용 방법

- 웹 클라이언트가 서버측에 요청을 보내게 되면 서버는 클라이언트를 식별하는 `session id`를 생성합니다.
- 서버는 `session id`를 이용해서 `key와 value`를 이용한 저장소인 `HttpSession`을 생성합니다.
- 서버는 `session id`를 저장하고 있는 쿠키를 생성하여 클라이언트에 전송합니다.
- 클라이언트는 서버측에 요청을 보낼때 `session id`를 가지고 있는 쿠키를 전송합니다.
- 서버는 쿠키에 있는 `session id`를 이용해서 그 전 요청에서 생성한 `HttpSession`을 찾고 사용합니다.

 

### 세션 생성 및 얻기

```java
HttpSession session = request.getSession();
HttpSession session = request.getSession(true);
```

- **request의 getSession()메소드는 서버에 생성된 세션이 있다면 세션을 반환하고 없다면 새롭게 세션을 생성하여 반환**합니다.
- 새롭게 생성된 세션인지는 `HttpSession`이 가지고 있는 `isNew()`메소드를 통해 알 수 있습니다.

```java
HttpSession session = request.getSession(false);
```

- **request의 getSession()메소드에 파라미터로 false를 전달하면, 이미 생성된 세션이 있다면 반환하고 없으면 null을 반환**합니다.

 

### 세션에 값 저장

```java
setAttribute(String name, Object value)
```

- name과 value의 쌍으로 객체 Object를 저장하는 메소드입니다.
- 세션이 유지되는 동안 저장할 자료를 저장합니다.

```java
session.setAttribute(이름, 값)
```

 

### 세션에 값 조회

`getAttribute(String name)` 메소드

- 세션에 저장된 자료는 다시 `getAttribute(String name)` 메소드를 이용해 조회합니다.
- **반환 값은 Object 유형이므로 저장된 객체로 자료유형 변환이 필요**합니다.
- 메소드 **setAttribute()에 이용한 name인 “id”를 알고 있다면 바로 다음과 같이 바로 조회**합니다.

```javascript
String value = (String) session.getAttribute("id");
```

 

### 세션에 값 삭제

- `removeAttribute(String name)` 메소드
  - name값에 해당하는 세션 정보를 삭제합니다.
- `invalidate()` 메소드
  - 모든 세션 정보를 삭제합니다.

 

**javax.servlet.http.HttpSession**

[![img](https://cphinf.pstatic.net/mooc/20180221_274/15191943441196AM5W_PNG/1.png?type=w760)](https://www.boostcourse.org/web326/lecture/58994?isDesc=false#)

[![img](https://cphinf.pstatic.net/mooc/20180221_271/1519194381710ssK9b_PNG/2.png?type=w760)](https://www.boostcourse.org/web326/lecture/58994?isDesc=false#)



### javax.servlet.http.HttpSession

**세션은 클라이언트가 서버에 접속하는 순간 생성**

- 특별히 지정하지 않으면 세션의 유지 시간은 **기본 값으로 30분 설정**합니다.
- **세션의 유지 시간이란 서버에 접속한 후 서버에 요청을 하지 않는 최대 시간**입니다.
- 30분 이상 서버에 전혀 반응을 보이지 않으면 세션이 자동으로 끊어집니다.
- 이 세션 유지 시간은 `web.xml`파일에서 설정 가능합니다.

```javascript
<session-config>
  <session-timeout>30</session-timeout>
</session-config>
```



### Spring MVC에서의 Session 사용



#### @SessionAttributes & @ModelAttribute

`@SessionAttributes` 파라미터로 지정된 이름과 같은 이름이 `@ModelAttribute`에 지정되어 있을 경우 메소드가 반환되는 값은 세션에 저장됩니다.

아래의 예제는 세션에 값을 초기화하는 목적으로 사용되었습니다.

```java
@SessionAttributes("user")
public class LoginController {
  @ModelAttribute("user")
  public User setUpUserForm() {
  return new User();	// 반환되는 값이 user라는 이름의 세션에 저장됩니다.
  }
}
```



`@SessionAttributes`의 파라미터와 같은 이름이 `@ModelAttribute`에 있을 경우 세션에 있는 객체를 가져온 후, 클라이언트로 전송받은 값을 설정합니다.

```java
@Controller
@SessionAttributes("user")
public class LoginController {
......
  @PostMapping("/dologin")
  // 세션에 user라는 이름의 객체의 정보를 User객체에 넣어줍니다.
  public String doLogin(@ModelAttribute("user") User user, Model model) {
......
  }
}
```

 

#### @SessionAttribute

메소드에 `@SessionAttribute`가 있을 경우 파라미터로 지정된 이름으로 등록된 세션 정보를 읽어와서 변수에 할당합니다.

```java
@GetMapping("/info")
public String userInfo(@SessionAttribute("user") User user) { // user라는 이름을 가진 세션을 User 객체에 할당
//...
//...
return "user";
}
```

 

#### SessionStatus

`SessionStatus` 는 컨트롤러 메소드의 파라미터로 사용할 수 있는 스프링 내장 타입입니다.

이 오브젝트를 이용하면 현재 컨트롤러의 `@SessionAttributes`에 의해 저장된 오브젝트를 제거할 수 있습니다.

```java
@Controller
@SessionAttributes("user")
public class UserController {
...... 
    @RequestMapping(value = "/user/add", method = RequestMethod.POST)
    public String submit(@ModelAttribute("user") User user, SessionStatus sessionStatus) {
  ......
      // 세션에서 전달받은 값을 삭제할 수 있습니다.
  sessionStatus.setComplete();
                                   ......

   }
 }
```

 

**Spring MVC - form tag 라이브러리**

`modelAttribute`속성으로 지정된 이름의 객체를 세션에서 읽어와서 form태그로 설정된 태그에 값을 설정합니다.

```java
<form:form action="login" method="post" modelAttribute="user">
Email : <form:input path="email" /><br>
Password : <form:password path="password" /><br>
<button type="submit">Login</button>
</form:form>
```



---

참고 : [https://www.boostcourse.org/web326/lecture/58996?isDesc=false](https://www.boostcourse.org/web326/lecture/58996?isDesc=false ) 

[https://www.boostcourse.org/web326/lecture/58994?isDesc=false](https://www.boostcourse.org/web326/lecture/58994?isDesc=false)

[https://www.boostcourse.org/web326/lecture/58992?isDesc=false](https://www.boostcourse.org/web326/lecture/58992?isDesc=false)

[https://www.boostcourse.org/web326/lecture/58991?isDesc=false](https://www.boostcourse.org/web326/lecture/58991?isDesc=false)

