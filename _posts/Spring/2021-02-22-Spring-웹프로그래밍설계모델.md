---
title: "Spring : 웹 프로그래밍 설계 모델 "
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

#  웹 프로그래밍 설계 모델



## Model1

- WAS (Web Application Server)에서 Service(사용자에게 제공할 기능)와 DAO(데이터베이스와 관계되는 것), JSP(사용자에게 실제로 보여줄 코드)를 모듈화 시키지 않고 하나의 파일로 처리 하는 것
- 장점 : html안에 Java코드나 각종 태그들이 한 곳에 모여있기 때문에 개발속도가 빠르다.
- 단점 : 여러가지 언어를 하나의 문서에 작성하기 때문에 유지보수가 힘들고 가독성이 좋지 않다.

![Model1](https://user-images.githubusercontent.com/58713853/75864574-94c64e80-5e45-11ea-9abf-e3dc1e51c17d.PNG)



## Model2

- 기능은 Service에 데이터베이스와 관계되는 것은 DAO, 사용자에게 보여지는 것은 View, 이런것들을 컨트롤 해주는 것을 Controller로 각각의 기능들을 분리해 모듈화 시켜 사용하는 것
- 각각의 기능이 분리되어 있기 때문에 유지보수가 수월하다.

![Model2](https://user-images.githubusercontent.com/58713853/75864661-b7f0fe00-5e45-11ea-834c-963ab276ed7d.PNG)





## 스프링 MVC 프레임워크 설계 구조

- DispatcherServlet :  클라이언트에서 받은 요청을 가장 먼저 HandlerMapping에거 넘겨준다.
- HandlerMapping : 많은 컨트롤러 중에 요청에 알맞는 컨트롤러를 선택해준다.
- HandlerAdapter : 컨트롤러 안에 있는 메소드 중에 요청에 적합한 메소드를 찾아준다.
  - 이 기능을 통해 Model,View 라는 데이터를 가져온다.
- ViewResolver : View데이터에 가장 적합한 JSP 문서를 찾아준다.

![설계구조](https://user-images.githubusercontent.com/58713853/75899291-b1cb4380-5e7e-11ea-9dac-5eecd5c7656b.PNG)



### DispatcherServlet 설정

- init-param 안에 스프링 설정파일을 초기 파라미터로 설정할 수 있다. (명시적인 방법으로, 많이쓰인다)
- 초기 파라미터를 설정하지 않으면 스프링 프레임워크가 자동으로 appServlet-context.xml을 자동으로 설정하게 된다. ( 물론 만들어 주어야함)

![webxml](https://user-images.githubusercontent.com/58713853/75904032-d5de5300-5e85-11ea-94e2-b5487e545ce8.PNG)

![초기설정](https://user-images.githubusercontent.com/58713853/75904051-dd056100-5e85-11ea-8f09-95b0d76557ca.PNG)



### Controller 객체

컨트롤러에 쓰이는 어노테이션을 사용하려면 servlet-context파일에 <annotation-driven /> 태그를 넣는다.



#### @Controller

클래스 이름 위에 @Controller라는 어노테이션을 사용한다.

![Controller객체](https://user-images.githubusercontent.com/58713853/75904054-dd9df780-5e85-11ea-80bd-6b6957793032.PNG)



#### RequestMapping

Controller객체의 메소드에 @RequestMapping 어노테이션을 사용 후 그 안에 요청한 값을 넘겨준다.

![RequestMapping](https://user-images.githubusercontent.com/58713853/75904056-de368e00-5e85-11ea-9033-e519414d8438.PNG)



### Model 타입의 파라미터

![Model타입](https://user-images.githubusercontent.com/58713853/75904044-db3b9d80-5e85-11ea-8560-fc1dbaad611d.PNG)



#### View 객체

- 해당하는 적합한 뷰를 찾아주는 InternalResourceViewResolver라는 Bean 객체를 스프링 설정파일에 만든다.
- ReqeustMapping에서 매핑 되는 리턴값과 prefix, suffix 값을 합쳐 jsp 파일로 만들어준다.
  - prefix + retur값 + suffix
    - 이 때, 리턴 타입이 void 일 경우 ViewResolver가 url과 똑같은 이름으로 view이름을 설정한다.

![View객체](https://user-images.githubusercontent.com/58713853/75904046-dc6cca80-5e85-11ea-9211-7edd6a331d6b.PNG)



## 전체적인 웹프로그래밍 구조

![전체적인구조](https://user-images.githubusercontent.com/58713853/75904048-dd056100-5e85-11ea-945e-df1abd4ccf4d.PNG)
