---
title: "Java : 개요, 환경, 명명법"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true
toc_label: 목차
---

## 개요
- 자바의 전체적인 정리가 아니라 자바할 때 내가 몰랐던 부분을 정리한 것이다.



## 환경
- JDK(java development kit)는 openjdk를 IDE(Integration Development Environment, tool)는 eclipse를 사용할 것이다.



## 명명법
- 프로그램을 구현하면서 지켜야 할 규칙이나, 오류가 발생하지는 않는다.
- 하지만 프로그래머들의 규약이라서 반드시 지키지 않으면 혼난다!

- 파스칼(pascal) : 처음에 시작할 때 대문자 그다음 소문자 의미 바뀌면 대문자 소문자
  - ex) class CoffeeMachine 클래스, 인터페이스
- 카멜(camel) : 처음에 시작할 때 소문자 의미 바뀔때 첫글자 대문자 소문자 
  - ex) public void makeCoffee(); 메소드, 변수명
- 헝가리언(hungarian) : 타입 축약 + 이름
  - ex) txtName, lblNumber 타입을 줄여서 이름과 붙여사용, GUI(Swing)
- 전체 대문자 : 상수, 변하지 않는 값을 지정할 때
  - ex) Math.PI = 3.14, System.out.println(Math.PI); 3.14가 나온다.
- 전체 소문자 : 예약어, 키워드, 패키지
  - ex) public, new, void, class , com.sk.sales

