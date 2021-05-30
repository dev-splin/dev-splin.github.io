---
title: "Spring : WebApplicationInitializer / AbstractAnnotationConfigDispatcherServletInitializer"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : WebApplicationInitializer"
  - "Spring : AbstractAnnotationConfigDispatcherServletInitializer"
toc: true
toc_sticky: true
toc_label: 목차
---

# WebApplicationInitializer / AbstractAnnotationConfigDispatcherServletInitializer 

[기존의 xml을 이용한 설정](https://dev-splin.github.io/spring/Spring-MVC-Example(DispatcherServlet-FrontController-Set)/)과는 다른 어노테이션만을 이용한 설정 방법입니다.

**Spring MVC는 ServletContainerInitializer를 구현하고 있는 SpringServletContainerInitializer를 제공**합니다. **SpringServletContainerInitializer는 `WebApplicationInitializer` 구현체를 찾아 인스턴스를 만들고 해당 인스턴스의 `onStartup` 메소드를 호출하여 초기화**합니다.

`AbstractAnnotationConfigDispatcherServletInitializer`는 `WebApplicationInitializer `를 상속받았기 때문에 `AbstractAnnotationConfigDispatcherServletInitializer`도 구현체라고 할 수 있습니다.



## 차이점

`WebApplicationInitializer`는 `onStartup()`함수 안에 `DispatcherServlet`과 `ContextLoaderListener`를 모두 구현해야합니다. 

하지만 `AbstractAnnotationConfigDispatcherServletInitializer`는 내부적으로 `startUp`, `registDispatcherServlet`을 진행합니다.



코드를 보면 딱 차이점이 느껴질 것이기 때문에 코드를 보며 알아보겠습니다.

`ApplicationConfig`는 **[Layered Architecture](https://dev-splin.github.io/spring/Spring-Layered-Architecture/#%EB%A0%88%EC%9D%B4%EC%96%B4%EB%93%9C-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98)에서 `Service Layer`와 `Repository Layer`를 모아놓은 것**입니다.(`root-conifg` 관련 파일, 안에 DB설정 관련 파일인 `DBConfig`를 `import`하고 있습니다.)

`WebMvcContextConfiguration`는 **DispatcherServlet 등의 servlet-config 관련 파일**입니다.



### Listner 설정

#### XML방식

기존의 `XML`을 이용한 `Listner` 설정 방법입니다.

```xml
  <!-- context가 로딩될 때 이부분을 읽어서 수행하게 됩니다. -->
  <listener>
  	<listener-class>
  		org.springframework.web.context.ContextLoaderListener
  	</listener-class>
  </listener>

  <!-- listener에서 java Configuration을 이용하려면 이부분이 꼭 필요합니다!! -->
  <context-param>
  	<param-name>contextClass</param-name>
  	<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
  </context-param>

  <!-- 공통으로 사용할 Config 파일을 가져옵니다. -->
  <context-param>
  	<param-name>contextConfigLocation</param-name>
  	<param-value>kr.or.connect.mvcguestbook.config.ApplicationConfig</param-value>
  </context-param>
```



#### WebApplicationInitializer 

`DispatcherServlet`과 `Listner`를 사용할 때, `AnnotationConfigWebApplicationContext` 객체를 만들어야 하기 때문에 `AnnotationConfigWebApplicationContext`객체를 만드는 부분을 따로 함수(`createContext`)로 만들었습니다.

```java
	// 인자로 받은 클래스로 AnnotationConfigWebApplicationContext를 만듭니다. 
	private AnnotationConfigWebApplicationContext createContext(final Class<?>... annotatedClasses) {
		AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
		context.register(annotatedClasses);
		return context;
	}

	// 리스너를 생성하는 함수입니다. Override된 onStartup() 함수에서 호출해야 합니다.
	private void registerListener(ServletContext servletContext) {
		// ApplicationConfig(java Configuration)를 이용해 AnnotationConfigWebApplicationContext을 만들고 
		AnnotationConfigWebApplicationContext context = createContext(ApplicationConfig.class);
        // Listener에 context-param을 등록해 줍니다.
		servletContext.addListener(new ContextLoaderListener(context));
	}
```



#### AbstractAnnotationConfigDispatcherServletInitializer

`AbstractAnnotationConfigDispatcherServletInitializer`의 `getRootConfigClasses()`를 `Override`하여, `Config`파일만 지정해주면 내부적으로 알아서 `root-config`를 설정해줍니다.

```java
	@Override
	protected Class<?>[] getRootConfigClasses() {
        // 배열로 되어있기 때문에 ','로 여러개의 Config파일을 지정할 수 있습니다.
		return new Class<?>[] {ApplicationConfig.class};
	}
```



### DispatcherServlet 설정

#### XML방식

```xml
<servlet>
    <servlet-name>mvc</servlet-name>
    <!-- Spring이 제공하고 있는 DispatcherServlet을 FrontController로 할 것이라는 <servlet-class>가 등록되어 있습니다. -->
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
    	<param-name>contextClass</param-name>
    	<!-- DispatcherServlet이 실행될 때 AnnotationConfigWebApplicationContext를 사용하기 위해 등록되어 있습니다. -->
    	<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
    </init-param>
    <init-param>
     	<param-name>contextConfigLocation</param-name>
    	<!-- DispatcherServlet이 실행될 때 설정들을 읽어야 하기 때문에 -->
    	<!-- <init-param>부분에 우리가 만든 WebMvcContextConfiguration를 설정하고 있는 것을 볼 수 있습니다. -->
    	<param-value>kr.or.connect.mvcguestbook.config.WebMvcContextConfiguration</param-value>
    </init-param>    
    <!-- 프로그램 시작과 동시에 서블릿의 생성과 초기화를 진행할 때 사용합니다. -->
    <!-- 여러 서블릿들간에 우선순위가 필요한 경우 0에 가까울수록 먼저 초기화가 진행됩니다. -->
    <load-on-startup>1</load-on-startup>
</servlet>
	<servlet-mapping>
        <!-- "/"요청이 들어오면 <servlet>안에 있는 <servlet-name>이 "mvc"인 이름을 찾아갑니다. -->
    	<servlet-name>mvc</servlet-name>
	    <url-pattern>/</url-pattern>
  </servlet-mapping>
```



#### WebApplicationInitializer 

`setLoadOnStartup()`함수를 이용해 우선순위를 지정해주고, `addMapping()`함수로 `url-pattern`을 지정해줍니다.

```java
	// DispatcherServlet를 만들어 줍니다.
	private void registerDispatcherServlet(ServletContext servletContext) {
        // WebMvcContextConfiguration(java Configuration)를 이용해 AnnotationConfigWebApplicationContext을 만들고
		AnnotationConfigWebApplicationContext context = createContext(WebMvcContextConfiguration.class);
		
        // DispatcherServlet를 만든 후, 서블릿들 간의 우선순위를 정해주고, url-pattern을 지정해줍니다.
		ServletRegistration.Dynamic dispatcher = servletContext.addServlet("dispatcher", new DispatcherServlet(context));
		dispatcher.setLoadOnStartup(1);
		dispatcher.addMapping("/");
	}
```



#### AbstractAnnotationConfigDispatcherServletInitializer 

`AbstractAnnotationConfigDispatcherServletInitializer`의 `getServletConfigClasses()`를 `Override`하여, `Config`파일만 지정해주면 내부적으로 알아서 `DispatcherServlet`설정을 해줍니다.

`getServletMappings()`을 `Override`하여 `url-pattern`을 지정해줍니다.

```java
	@Override
	protected Class<?>[] getServletConfigClasses() {
		return new Class<?>[] {WebMvcContextConfiguration.class};
	}

	@Override
	protected String[] getServletMappings() {
		return new String[] {"/"};
	}
```



### Filter 설정

데이터를 주고 받을 때 한글이 제대로 나오게 하기위한 `Filter설정`도 해주어야합니다.

#### XML방식

```xml
<filter>
	<filter-name>encodingFilter</filter-name>
    <filter-class>
        org.springframework.web.filter.CharacterEncodingFilter
    </filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <!-- 이 필터의 적용범위를 설정해줍니다. "/*"는 모든 범위, 특정 URL에만 지정할 수도 있습니다. -->
    <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 데이터를 주고 받을 때 한글이 제대로 나오게 해줍니다. -->
```



#### WebApplicationInitializer 

```java
	// 필터를 만듭니다.
	private void registerFilter(ServletContext servletContext) {
        // CharacterEncodingFilter 인자의 true, true는 request, response이 존재할 때 encoding을 적용한다는 것입니다.
		CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter("UTF-8", true, true);
		FilterRegistration.Dynamic webEncodingFilter = servletContext.addFilter("webEncodingFilter", encodingFilter);
		webEncodingFilter.addMappingForUrlPatterns(null, false, "/*");
	}
```



#### AbstractAnnotationConfigDispatcherServletInitializer 

```java
	@Override
	protected Filter[] getServletFilters() {
		CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter();
        encodingFilter.setEncoding("UTF-8");
        // request와 response에 encoding을 적용한다는 것입니다.
        encodingFilter.setForceEncoding(true);

        return new Filter[]{encodingFilter};
	}
```



### 전체 코드

코드를 비교했을 때, 딱 봐도 `AbstractAnnotationConfigDispatcherServletInitializer`를 사용하는 게 편하고 간단하다는 것을 알 수 있습니다.

#### XML방식

```java
<?xml version="1.0" encoding="UTF-8"?>

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  
  <!-- listener를 선언해줍니다. -->
  <!-- context가 로딩될 때 이부분을 읽어서 수행하게 됩니다. -->
  <listener>
  	<listener-class>
  		org.springframework.web.context.ContextLoaderListener
  	</listener-class>
  </listener>
  
  <!-- listener가 실행될 때 context-param에 등록되어 있는 것들을 참고하게 됩니다. -->
  
  <!-- listener에서 java Configuration을 이용하려면 이부분이 꼭 필요합니다!! -->
  <context-param>
  	<param-name>contextClass</param-name>
  	<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
  </context-param>
  
  <!-- 공통으로 사용할 Config 파일을 가져옵니다. -->
  <context-param>
  	<param-name>contextConfigLocation</param-name>
  	<param-value>kr.or.connect.mvcguestbook.config.ApplicationConfig</param-value>
  </context-param>
  
  	  
   <servlet>
    <servlet-name>mvc</servlet-name>
    <!-- Spring이 제공하고 있는 DispatcherServlet을 FrontController로 할 것이라는 <servlet-class>가 등록되어 있습니다. -->
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextClass</param-name>
      <!-- DispatcherServlet이 실행될 때 AnnotationConfigWebApplicationContext를 사용하기 위해 등록되어 있습니다. -->
      <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
    </init-param>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <!-- DispatcherServlet이 실행될 때 설정들을 읽어야 하기 때문에 -->
      <!-- <init-param>부분에 우리가 만든 WebMvcContextConfiguration를 설정하고 있는 것을 볼 수 있습니다. -->
      <param-value>kr.or.connect.mvcguestbook.config.WebMvcContextConfiguration</param-value>
    </init-param>    
    <!-- 프로그램 시작과 동시에 서블릿의 생성과 초기화를 진행할 때 사용합니다. -->
    <!-- 여러 서블릿들간에 우선순위가 필요한 경우 0에 가까울수록 먼저 초기화가 진행됩니다. -->
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>mvc</servlet-name>
    <url-pattern>/</url-pattern>
    <!-- "/"요청이 들어오면 <servlet>안에 있는 <servlet-name>이 "mvc"인 이름을 찾아갑니다. -->
  </servlet-mapping>
  
  
  <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>
            org.springframework.web.filter.CharacterEncodingFilter
    </filter-class>
    <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
</filter>
<filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <!-- 이 필터의 적용범위를 설정해줍니다. "/*"는 모든 범위, 특정 URL에만 지정할 수도 있습니다. -->
        <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 데이터를 주고 받을 때 한글이 제대로 나오게 해줍니다. -->
  
  
</web-app>

```



#### WebApplicationInitializer 

```java
public class WebApplicationInitializer implements WebApplicationInitializer  {
		// 웹 어플리케이션이 시작할 때 자동으로 이 메서드가 실행 됩니다.
	@Override
	public void onStartup(ServletContext servletContext) throws ServletException {
		registerListener(servletContext);
		registerDispatcherServlet(servletContext);
		registerFilter(servletContext);
	}
	
	// 리스너를 생성하는 함수입니다. Override된 onStartup() 함수에서 호출해야 합니다.
	private void registerListener(ServletContext servletContext) {
		// ApplicationConfig(java Configuration)를 이용해 AnnotationConfigWebApplicationContext을 만들고 
		AnnotationConfigWebApplicationContext context = createContext(ApplicationConfig.class);
        // Listener에 context-param을 등록해 줍니다.
		servletContext.addListener(new ContextLoaderListener(context));
	}
	
	// DispatcherServlet를 만들어 줍니다.
	private void registerDispatcherServlet(ServletContext servletContext) {
        // WebMvcContextConfiguration(java Configuration)를 이용해 AnnotationConfigWebApplicationContext을 만들고
		AnnotationConfigWebApplicationContext context = createContext(WebMvcContextConfiguration.class);
		
        // DispatcherServlet를 만든 후, 서블릿들 간의 우선순위를 정해주고, url-pattern을 지정해줍니다.
		ServletRegistration.Dynamic dispatcher = servletContext.addServlet("dispatcher", new DispatcherServlet(context));
		dispatcher.setLoadOnStartup(1);
		dispatcher.addMapping("/");
	}
	
	// 필터를 만듭니다.
	private void registerFilter(ServletContext servletContext) {
		// CharacterEncodingFilter 인자의 true, true는 request, response이 존재할 때 encoding을 적용한다는 것입니다. 
		CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter("UTF-8", true, true);
		FilterRegistration.Dynamic webEncodingFilter = servletContext.addFilter("webEncodingFilter", encodingFilter);
		webEncodingFilter.addMappingForUrlPatterns(null, false, "/*");
	}
	
	// 필터를 만듭니다.
	private void registerFilter(ServletContext servletContext) {
      // CharacterEncodingFilter 인자의 true, true는 request, response이 존재할 때 encoding을 적용한다는 것입니다.
		CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter("UTF-8", true, true);
		FilterRegistration.Dynamic webEncodingFilter = servletContext.addFilter("webEncodingFilter", encodingFilter);
		webEncodingFilter.addMappingForUrlPatterns(null, false, "/*");
	}
}
```



#### AbstractAnnotationConfigDispatcherServletInitializer 

```java
public class WebApplicationInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

	@Override
	protected Class<?>[] getRootConfigClasses() {
        // 배열로 되어있기 때문에 ','로 여러개의 Config파일을 지정할 수 있습니다.
		return new Class<?>[] {ApplicationConfig.class};
	}

	@Override
	protected Class<?>[] getServletConfigClasses() {
		return new Class<?>[] {WebMvcContextConfiguration.class};
	}

	@Override
	protected String[] getServletMappings() {
		return new String[] {"/"};
	}
	
	@Override
	protected Filter[] getServletFilters() {
		CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter();
        encodingFilter.setEncoding("UTF-8");
        // request와 response에 encoding을 적용한다는 것입니다.
        encodingFilter.setForceEncoding(true);

        return new Filter[]{encodingFilter};
	}
```



---

참고 : [https://hashmap27.tistory.com/6](https://hashmap27.tistory.com/6)

[https://joont92.github.io/spring/WebApplicationInitializer/](https://joont92.github.io/spring/WebApplicationInitializer/)

