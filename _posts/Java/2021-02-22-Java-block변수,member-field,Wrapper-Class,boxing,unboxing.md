---
title: "Java : block 변수/member field, Wrapper Class, boxing/unboxing"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true
toc_label: 목차
---

## block 변수 / member field
- block 변수(메소드와 같이 블락안에 있는 변수)와 member field의 이름이 같으면 block 변수가 우선순위가 높다.



## Wrapper Class
- 참조타입을 기본타입으로 바꿀때 사용하는 클래스
- 기본타입의 앞글자를 대문자로 만들면 클래스명이 된다.
- ex) Boolean, Byte, Integer, Float, Character...



## boxing / unboxing
- boxing : 기본타입을 참조타입으로 바꾸는 것
- unboxing : 참조타입을 기본타입으로 바꾸는 것
```java
// 기 : 기본타입 / 참 : 참조타입
int a = 10;
Integer aa = new Integer(a) // 기 -> 참
Integer aa2 = 10; // 기 -> 참
int c = aa2; 참 -> 기
int c1 = aa.intValue(); // 참 -> 기

Object o = 10; // 기 -> 참
int d = (Integer)o; // 참 -> 기
```
