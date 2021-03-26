---
title: "Spring : RestController"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : RestController"
toc: true
toc_sticky: true
toc_label: 목차
---

# @RestController

- Spring 4 에서 `Rest API` 또는 `Web API`를 개발하기 위해 등장한 어노테이션입니다.
- 이전 버전의 `@Controller`와 `@ResponseBody`를 포함합니다.

 

## MessageConvertor

**외부에서 전달받은 JSON 메서드를 내부에서 사용할 수 있는 객체로 변환**하거나 **컨트롤러를 리턴한 객체가 클라이언트에게 JSON으로 변환**해서 전달할 수 있도록하는 역할을 합니다.

- 자바 객체와 HTTP 요청 / 응답 바디를 변환하는 역할
- `@ResponseBody`, `@RequestBody`
- `@EnableWebMvc` 로 인한 기본 설정
- `WebMvcConfigurationSupport` 를 사용하여 Spring MVC 구현
- `Default MessageConvertor` 를 제공
- [링크 바로가기](https://github.com/spring-projects/spring-framework/blob/master/spring-webmvc/src/main/java/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.java) 의 `addDefaultHttpMessageConverters`메소드 항목 참조

 

### MessageConvertor 종류

[![img](https://cphinf.pstatic.net/mooc/20180219_44/1519025088215YszqO_PNG/1.png?type=w760)](https://www.boostcourse.org/web326/lecture/58984?isDesc=false#)



**JSON 응답하기**

- 컨트롤러의 메소드에서는 JSON으로 변환될 객체를 반환합니다.
- `jackson라이브러리`를 추가할 경우 객체를 JSON으로 변환하는 메시지 컨버터가 사용되도록 `@EnableWebMvc`에서 기본으로 설정되어 있습니다.
- `jackson라이브러리`를 추가하지 않으면 JSON메시지로 변환할 수 없어 500오류가 발생합니다.
- 사용자가 임의의 `메시지 컨버터(MessageConverter)`를 사용하도록 하려면 `WebMvcConfigurerAdapter`의 `configureMessageConverters`메소드를 오버라이딩 하도록 합니다.



---

참고 : https://www.boostcourse.org/web326/lecture/58985?isDesc=false