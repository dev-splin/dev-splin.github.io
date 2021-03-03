---
title: "Web : JSTL(JSP Standard Tag Library)"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Web : JSTL(JSP Standard Tag Library)"
toc: true
toc_sticky: true
toc_label: 목차
---

# JSTL(JSP Standard Tag Library)

JSTL(JSP Standard Tag Library)은 **JSP 페이지에서 조건문 처리, 반복문 처리 등을 html tag형태로 작성할 수 있게 도와줍니다.**

JSP는 스크립트릿의 자바코드와 HTML태그가 섞여있어서 편의성은 높았지만 프론트 개발자가 해당코드를 수정하기가 힘들었기 때문에 유지보수가 어렵다는 문제를 가지게 되었습니다.이런 문제를 해결하기위해 등장한것이 JSTL 입니다.



[![img](https://cphinf.pstatic.net/mooc/20180130_149/1517289583487Ac0YJ_PNG/2_6_2_jstl.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258521?isDesc=false#)



## JSTL을 사용하려면?

- [http://tomcat.apache.org/download-taglibs.cgi](http://tomcat.apache.org/download-taglibs.cgi)
- 위의 사이트에서 3가지 jar파일을 다운로드 한 후 WEB-INF/lib/ 폴더에 복사를 합니다.

[![img](https://cphinf.pstatic.net/mooc/20180130_248/1517289861733CmzUv_PNG/2_6_2_jstl_.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258521?isDesc=false#)



## JSTL이 제공하는 태그의 종류

[![img](https://cphinf.pstatic.net/mooc/20180130_273/1517290494334HrB7S_PNG/2_6_2_jstl___.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258521?isDesc=false#)



### 코어 태그

[![img](https://cphinf.pstatic.net/mooc/20180130_226/1517290578353rKRbE_PNG/2_6_2_jstl_.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258521?isDesc=false#)



#### set, remove

[![img](https://cphinf.pstatic.net/mooc/20180226_240/1519633482313pWfP8_PNG/1.png?type=w760)](https://www.boostcourse.org/web326/lecture/258521?isDesc=false#)



##### 예제

set으로 값을 설정하고 출력합니다. 또, remove로 값을 지운 다음에도 출력해봅니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
<!-- 라이브러리를 추가하고 JSP페이지에서 taglib로 설정을 알려주어야 합니다. -->
<!-- JSTL은 커스텀 태그를 만들어서 사용할 수도 있는데 태그를 알수없기 때문에 prefix를 지정해서 사용할 때 쓸 수 있게끔 도와줍니다. -->
<c:set var="value1" scope="request" value="kang"/>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
성 : ${value1}<br>
<c:remove var="value1" scope="request"/>
성 : ${value1}<br>
</body>
</html>
```



[
![img](https://cphinf.pstatic.net/mooc/20180226_103/1519633640114VKW2d_PNG/2.png?type=w760)](https://www.boostcourse.org/web326/lecture/258522/?isDesc=false#)

자바를 공부할 때 객체에 `property`라는 용어가 나오면, 그 **객체의 값을 변경하거나 읽어들이기 위한 `getter`, `setter` 메서드를 생각**하면 됩니다.

set 태그를 이용해서 특정한 객체의 메서드에 값을 전달할 수도 있습니다.

`property="propertyName`을 보면 속성명하고 똑같이 나오는데, setter메서드는 앞에 `set`을 붙이고 필드의 `첫 글자를 대문자`로 바꿔서 사용해야 하는 규칙이 있기 때문에 그 규칙을 이용해 속성명만 적어주어도 setPropertyName으로 작동할 수 있게 해줍니다. (이런 규칙들을 반드시 지켜야되는 이유가 여기에 있습니다.)



#### if

자바의 문법과는 다르게 else 처리는 없습니다.

[![img](https://cphinf.pstatic.net/mooc/20180226_83/1519633710402BlJ2W_PNG/3.png?type=w760)](https://www.boostcourse.org/web326/lecture/258522/?isDesc=false#)



#### choose

java의 if-else와 비슷하다고 볼 수 있습니다.

[![img](https://cphinf.pstatic.net/mooc/20180130_4/1517292532220uxSVD_PNG/2_6_2__choose.PNG?type=w760)
](https://www.boostcourse.org/web326/lecture/258522/?isDesc=false#)



##### 예제

set으로 점수를 설정하고 choose를 이용해 학점을 출력해줍니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
<c:set var="score" scope="request" value="83"/>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<c:choose>
	<c:when test="{$score >= 90}">
		A학점입니다.
	</c:when>
	<c:when test="${score >=80 }">
   		B학점입니다.
    </c:when>
    <c:when test="${score >=70 }">
    	C학점입니다.
    </c:when>
    <c:when test="${score >=60 }">
    	D학점입니다.
    </c:when>
    <c:otherwise>
    	F학점입니다.
    </c:otherwise>    
</c:choose>
</body>
</html>
```



#### forEach

forEach를 이용하면 배열이나 리스트 같은 자료구조에서 정보를 하나씩 가져올 수 있습니다. 마치 for문처럼 특정 조건만큼 반복을 하게도 만들 수 있습니다.

begin, end는 생략가능하고 생략 시에는 처음부터 끝까지 출력해줍니다.

[![img](https://cphinf.pstatic.net/mooc/20180130_218/1517292735244tmWgM_PNG/2_6_2__forEach.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258523/?isDesc=false#)



##### 예제

스크립트릿으로 리스트를 하나 만들고 값을 저장해준 다음, forEach를 이용해 출력해봅니다.

```jsp
<%@page import="java.util.ArrayList"%>
<%@page import="java.util.List"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
<%
	List<String> list = new ArrayList<>();
	list.add("hello");
	list.add("world");
	list.add("!!!!");
	
	request.setAttribute("list", list);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<c:forEach items="${list}" var="item" begin="1">
<!-- bigin을 넣으면 1번 인덱스부터, 넣지 않으면 0번부터 시작하는 것을 볼 수 있습니다. -->
	${item } <br>
</c:forEach>
</body>
</html>
```



#### import

[![img](https://cphinf.pstatic.net/mooc/20180130_93/1517293018908uGgzT_PNG/2_6_2__import.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258523/?isDesc=false#)



##### 예제

jstlValue라는 jsp 페이지의 정보를 가져옵니다.

```jsp
<%-- jstlValue.jsp --%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
Splin
```



```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 

<c:import url="http://localhost:8080/firstweb/jstlValue.jsp" var="urlValue" scope="request" />

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
${urlValue }
<%-- Splin을 가져오는 것을 볼 수 있습니다. --%>
</body>
</html>
```



#### redirect

[![img](https://cphinf.pstatic.net/mooc/20180130_170/1517293246119dFJ4F_PNG/2_6_2__redirect.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258524/?isDesc=false#)



##### 예제

페이지를 실행시키면 jstlValue 페이지로 redirect합니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
<c:redirect url="http://localhost:8080/firstweb/jstlValue.jsp" />
```



#### out

[![img](https://cphinf.pstatic.net/mooc/20180130_55/1517293404340WP4J3_PNG/2_6_2__out.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258524/?isDesc=false#)



##### 예제

escapeXml을 true로 사용하면 표에 나와있는 `value`에 있는 특수문자들을  자바스크립트로 인식하지 않고 그냥 문자로 인식합니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<c:set var="t" value="<script type ='text/javascript'>alert(1);</script> "/>

<c:out value="${t }" escapeXml="true"/>
</body>
</html>
```



---

참고 : https://www.boostcourse.org/web326/lecture/258524/?isDesc=false

