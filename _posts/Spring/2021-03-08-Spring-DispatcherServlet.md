---
title: "Spring : DispatcherServlet"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : DispatcherServlet"
toc: true
toc_sticky: true
toc_label: 목차
---

# DispatcherServlet

- 프론트 컨트롤러 (Front Controller)
- **클라이언트의 모든 요청을 받은 후 이를 처리할 핸들러에게 넘기고 핸들러가 처리한 결과를 받아 사용자에게 응답 결과를 보여줍니다.**
- DispathcerServlet은 여러 컴포넌트를 이용해 작업을 처리합니다.



## DispatcherServlet 내부 동작흐름

[![img](https://cphinf.pstatic.net/mooc/20180219_281/1519003870301bOehw_PNG/2.png?type=w760)](https://www.boostcourse.org/web326/lecture/258533/?isDesc=false#)

요청이 들어오면 요청에 대한 선처리 작업을 합니다. 선처리 작업이 끝나면 HandlerExecutionChain을 결정 후 실행합니다. 실행 과정 중에 예외가 발생하면 예외처리를 하게 되고 예외가 발생하지 않았다면 뷰를 렌더링하고 요청처리를 종료합니다.





## DispatcherServlet 내부 동작흐름 상세 - 요청 선처리 작업

[![img](https://cphinf.pstatic.net/mooc/20180219_91/1519003885824QT31y_PNG/3.png?type=w760)](https://www.boostcourse.org/web326/lecture/258533/?isDesc=false#)

**Locale결정**

`Spring MVC`는 `지역화`라는 것을 지원합니다. `지역화`는 브라우저가 보내는 **헤더 정보에서 언어 설정의 값들을 이용**해서 그림에 나오는 `Locale`을 결정하게 되고 이 **`Locale`에 설정한 값을 한국어, 일본어, 중국어, 영어 등 다양하게 보내지면서 그 부분에 따라 각각 다른 언어로 된 화면을 보여주게 처리하는 것**을 말합니다.

**RequestContextHolder**

**스레드 로컬 객체로 요청을 받아서 응답할 때까지 `HttpServletRequest`, `HttpServletResponse` 등을 Spring이 관리하는 객체 안에서 사용**할 수 있도록 해줍니다. 

예를 들어, 만약 나중에 `Spring MVC`를 사용하다가 컨트롤러가 가진 메서드 안에서 `Request` 객체가 필요해서 **인자에다 `HttpServletRequest request` 를 선언해서 메서드 내에서 `Request`를 자유롭게 사용**할 수 있습니다. 하지만 **이런 방식은 Spring이 웹 기술에 종속되는 문제점**을 가지고 있기 때문에 권장하는 방법은 아닙니다. (Spring이 그에 해당하는 것들을 제공하는데, 이런 것들을 가져다 쓰는게 좋습니다.)

**FlashMap 복원**

`Spring 3`에서 추가된 기능입니다. `redirect`로 값을 전달할 때 `?`뒤에 파라미터를 입력하는 것들을 이용하게 되는데, 이렇게 사용 하다 보면 URL의 길이에 대한 제한도 있고 복잡해지게 됩니다.  이 때, **`FlashMap`을 사용하면 `redirect` 될 때 딱 한 번 값을 유지시킬 수 있게 해줍니다.** 즉, `FlashMap`을 복원은 현재 실행이 `redirect` 되었을 때를 위해 제공해준다고 보면 됩니다.

**멀티 파트 요청**

파일 업로드를 했을 경우에는 **파일 정보를 읽어들이는 특수한 형태의 `Request`객체가 필요**합니다. **이런 객체를 요청하는 것이 멀티 파트 요청**이라고 볼 수 있습니다.

**MultipartResolver가 멀티파트를 결정, 핸들러 결정과 실행**

멀티 파트 요청이 들어오면 **`MultipartResolver`가 파일 업로드를 처리 하기 위한 멀티 파트를 결정**해줍니다. 그리고 나서 **실제 요청을 처리하는 핸들러를 결정하고 실행**하게 됩니다.



### 요청 선처리 작업시 사용된 컴포넌트

**org.springframework.web.servlet.LocaleResolver**

- 지역 정보를 결정해주는 전략 오브젝트입니다.
- 디폴트인 `AcceptHeaderLocalResolver`는 `HTTP` 헤더의 정보를 보고 지역정보를 설정해줍니다.

**org.springframework.web.servlet.FlashMapManager**

- `FlashMap`객체를 조회(retrieve) & 저장을 위한 인터페이스
- `RedirectAttributes`의 `addFlashAttribute`메소드를 이용해서 저장합니다.
- `Redirect` 후 조회를 하면 바로 정보는 삭제됩니다.

**org.springframework.web.context.request.RequestContextHolder**

- 일반 빈에서 `HttpServletRequest`, `HttpServletResponse`, `HttpSession` 등을 사용할 수 있도록 합니다.
- 해당 객체를 일반 빈에서 사용하게 되면, Web에 종속적이 될 수 있습니다.

**org.springframework.web.multipart.MultipartResolver**

- 멀티파트 파일 업로드를 처리하는 전략



## DispatcherServlet 내부 동작흐름 상세 - 요청 전달

[![img](https://cphinf.pstatic.net/mooc/20180219_20/1519003954110F9wyd_PNG/4.png?type=w760)](https://www.boostcourse.org/web326/lecture/258533/?isDesc=false#)

`HandlerMapping`으로 `HandlerExecutionChain`이 결정됩니다. 그 때, `HandlerExecutionChain`을 발견하지 못하면 페이지가 없다는 것이기 때문에 `Http 404`를 전달하게 되고, 발견했다면 `HandlerAdpter`가 결정됩니다. 결정된 `HandlerAdapter`가 없다면 서버가 잘못되었다는 것이기 때문에 `ServletException`이 발생되고 발견했다면 문제없이 요청이 처리됩니다.



### 요청 전달시 사용된 컴포넌트

**org.springframework.web.servlet.HandlerMapping**

- `HandlerMapping`구현체는 어떤 핸들러가 요청을 처리할지에 대한 정보를 알고 있습니다.
- 디폴트로 설정되는 있는 핸들러매핑은 `BeanNameHandlerMapping`과 `DefaultAnnotationHandlerMapping` 2가지가 설정되어 있습니다.

**org.springframework.web.servlet.HandlerExecutionChain**

- `HandlerExecutionChain`구현체는 실제로 호출된 핸들러에 대한 참조를 가지고 있습니다.
- 즉, 무엇이 실행되어야 될지 알고 있는 객체라고 말할 수 있으며, 핸들러 실행전과 실행후에 수행될 `HandlerInterceptor`도 참조하고 있습니다.

**org.springframework.web.servlet.HandlerAdapter**

- 실제 핸들러를 실행하는 역할을 담당합니다.
- 핸들러 어댑터는 선택된 핸들러를 실행하는 방법과 응답을 `ModelAndView`로 변화하는 방법에 대해 알고 있습니다.
- 디폴트로 설정되어 있는 핸들러어댑터는 `HttpRequestHandlerAdapter`, `SimpleControllerHandlerAdapter`, `AnnotationMethodHanlderAdapter` 3가지입니다.
- `@RequestMapping`과 `@Controller` 어노테이션을 통해 정의되는 컨트롤러의 경우 `DefaultAnnotationHandlerMapping`에 의해 핸들러가 결정되고 그에 대응되는 `AnnotationMethodHandlerAdapter`에 의해 호출이 일어납니다.



## DispatcherServlet 내부 동작흐름 상세 - 요청 처리

[![img](https://cphinf.pstatic.net/mooc/20180219_167/1519004040926yL8eC_PNG/5.png?type=w760)](https://www.boostcourse.org/web326/lecture/258533/?isDesc=false#)

`HandlerExecutionChain`이 결정되었다면 사용 가능한 `인터셉터`가 있는지 찾아봅니다.

> 인터셉터를 간단하게 설명하자면, 일종의 필터라고 생각하면 됩니다. 필터처럼 뭔가 **처리하기 전에 한번 거쳐서 지나가게 해주는 것**이라고 생각하면 되겠습니다.

사용 가능한 `인터셉터`가 존재한다면 `인터셉터`의 `preHandle`을 호출해서 요청을 처리하고 핸들러가 실행됩니다. 핸들러가 실행된 후에 값을 리턴하게 되는데, 리턴하는 결과가 `ModelAndView` 객체이고 `ModelAndView` 객체가 뷰를 갖고 있지 않다면 `RequestToViewNameTranslator`가 동작을 하게 됩니다. 그 이외에는 `인터셉터`의 `postHandle`을 호출해서 요청을 처리하고 뷰 렌더링을 해서 결과를 보여주게 됩니다.



### 요청 처리시 사용된 컴포넌트

**org.springframework.web.servlet.ModelAndView**

- `ModelAndView`는 `Controller`의 처리 결과를 보여줄 `view`와 `view`에서 사용할 값을 전달하는 클래스입니다.
- 서블릿에서 모델을 얻고 그 모델을 JSP로 넘길 때 `Request` 객체를 사용했지만 Spring에서는 종속되지 않게 하기 위해 `ModelAndView` 객체를 제공합니다.

**org.springframework.web.servlet.RequestToViewNameTranslator**

- 컨트롤러에서 뷰 이름이나 뷰 오브젝트를 제공해주지 않았을 경우 URL과 같은 요청정보를 참고해서 자동으로 뷰 이름을 생성해주는 전략 오브젝트입니다. 디폴트는 `DefaultRequestToViewNameTranslator`이다.



## DispatcherServlet 내부 동작흐름 상세 - 예외처리

[![img](https://cphinf.pstatic.net/mooc/20180219_26/1519004078279fGdRP_PNG/6.png?type=w760)](https://www.boostcourse.org/web326/lecture/258533/?isDesc=false#)

예외가 발생하면 `HandlerExceptionResolver`에 문의를 한 후, `ModelAndView`가 있다면 요청처리를 재개하고 그렇지 않으면 다시 예외를 던지게 됩니다.



### 예외 처리시 사용된 컴포넌트

**org.springframework.web.servlet.handlerexceptionresolver**

- 기본적으로 `DispatcherServlet`이 `DefaultHandlerExceptionResolver`를 등록합니다.
- `HandlerExceptionResolver`는 예외가 던져졌을 때 어떤 핸들러를 실행할 것인지에 대한 정보를 제공합니다.



## DispatcherServlet 내부 동작흐름 상세 - 뷰 렌더링 과정

[![img](https://cphinf.pstatic.net/mooc/20180219_66/1519004113425TanBR_PNG/7.png?type=w760)](https://www.boostcourse.org/web326/lecture/258533/?isDesc=false#)

뷰 렌더링 요청이 왔을 때 뷰가 `String`이고  `ViewResolver`의 구현체를 찾지 못했다면 `ServletException`을 발생시키게 됩니다. 뷰의 구현체를 찾았다면 렌더링을 하고 요청처리를 계속 진행합니다.



**뷰 렌더링 과정시 사용된 컴포넌트**

**org.springframework.web.servlet.ViewResolver**

- 컨트롤러가 리턴한 뷰 이름을 참고해서 적절한 뷰 오브젝트를 찾아주는 로직을 가진 전략 오프젝트입니다.
- 뷰의 종류에 따라 적절한 뷰 리졸버를 추가로 설정해줄 수 있습니다.



## 내부 동작흐름 상세 - 요청 처리 종료

[![img](https://cphinf.pstatic.net/mooc/20180219_296/1519004150778ofOPV_PNG/8.png?type=w760)
  ](https://www.boostcourse.org/web326/lecture/258533/?isDesc=false#)

`HandlerExecutionChain`이 존재한다면 인터셉터의 `afterCompletion` 메소드를 실행하게 됩니다. 그리고 나서 `RequestHandleEvent`를 발생시키고 요청이 처리됩니다. 만약 `HandlerExecutionChain`이 존재하지 않는다면 바로 `RequestHandleEvent`를 발생시키고 요청이 처리됩니다.



---

참고 : https://www.boostcourse.org/web326/lecture/258533/?isDesc=false