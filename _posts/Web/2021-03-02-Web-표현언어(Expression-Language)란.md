---
title: "Web : Expression Language"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Web : Expression Language"
toc: true
toc_sticky: true
toc_label: 목차
---

# 표현 언어(Expression Language)란?

- 표현 언어(Expression Language)는 **값을 표현하는 데 사용되는 스크립트 언어**로서 **JSP의 기본 문법을 보완**하는 역할을 합니다.
- 자바, 백엔드를 다루지 않는 개발자가 JSP에 자바 코드가 나오게 되면 혼란스러울 수 있습니다. 때문에 자바 코드보다 조금 더 간단하게 사용할 수 있는, 즉 **프론트엔드 개발자라든가 디자이너가 봐도 크게 이질감이 들지 않게 하기 위해 사용**합니다.



## 표현 언어가 제공하는 기능

- JSP의 스코프(scope)에 맞는 속성 사용
- 집합 객체에 대한 접근 방법 제공
- 수치 연산, 관계 연산, 논리 연산자 제공
- 자바 클래스 메소드 호출 기능 제공
- 표현언어만의 기본 객체 제공



## 표현언어의 표현방법

[![img](https://cphinf.pstatic.net/mooc/20180130_78/1517281954147RNccz_PNG/2_6_1__.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258517?isDesc=false#)



## 표현언어의 기본 객체

[![img](https://cphinf.pstatic.net/mooc/20180130_153/1517281495386qOuqH_PNG/2_6_1__.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258517?isDesc=false#)



### 표현 언어의 기본 객체 사용 예

[![img](https://cphinf.pstatic.net/mooc/20180130_68/1517282068498tAlQM_PNG/2_6_1____.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258517?isDesc=false#)



## 표현 언어의 데이터 타입

- 불리언 타입 - `true`와 `false`
- 정수타입 - `0~9`로 이루어진 정수 값 음수의 경우 `-`가 붙음
- 실수타입 - `0~9`로 이루어져 있으며, 소수점`.`을 사용할 수 있고, `3.24e3`과 같이 지수형으로 표현 가능합니다.
- 문자열 타입 - 따옴표` '` 또는`"`로 둘러싼 문자열. 만약 작은 따옴표`'`를 사용해서 표현할 경우 값에 포함된 작은 따옴표는 `\'` 와 같이 \ 기호와 함께 사용해야 합니다.
- `\` 기호 자체는 `\\` 로 표시합니다.
- 널 타입 - `null`

 

## 객체 접근 규칙

[![img](https://cphinf.pstatic.net/mooc/20180130_27/1517286832617YwnDB_PNG/2_6_1_.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258517?isDesc=false#)

- 표현 1이나 표현 2가 **null**이면 **null**을 반환합니다.
- 표현1이 **Map**일 경우 표현2를 **key로한 값**을 반환합니다.
- 표현1이 **List나 배열**이면 표현2가 **정수일 경우 해당 정수 번째 index에 해당하는 값**을 반환합니다. 만약 **정수가 아닐 경우**에는 **오류가** 발생합니다.
- 표현1이 **객체**일 경우는 표현2에 해당하는 **getter메소드에 해당하는 메소드를 호출한 결과**를 반환합니다.

 

## 표현 언어의 수치 연산자

- `+` : 덧셈
- `\-` : 뺄셈
- `*` : 곱셈
- `/` 또는 `div` : 나눗셈
- `%` 또는 `mod` : 나머지
- 숫자가 아닌 객체와 수치 연산자를 사용할 경우 객체를 숫자 값으로 변환 후 연산자를 수행 : `${"10"+1}` → `${10+1}`
- 숫자로 변환할 수 없는 객체와 수치 연산자를 함께 사용하면 에러를 발생 : `${"열"+1}` → 에러
- 수치 연산자에서 사용되는 객체가 null이면 0으로 처리 : `${null + 1}` → `${0+1}`

 

## 비교 연산자

- `==` 또는 `eq`
- `!=` 또는 `ne`
- `<` 또는 `lt`
- `>` 또는 `gt`
- `<=` 또는 `le`
- `>=` 또는 `ge`
- 문자열 비교: `${str == '값'}`  `str.compareTo("값") == 0` 과 동일

 

## 논리 연산자

- `&&` 또는 `and`
- `||` 또는 `or`
- `!` 또는 `not`

 

## empty 연산자, 비교선택 연산자

[![img](https://cphinf.pstatic.net/mooc/20180130_17/1517287228502gEf9g_PNG/2_6_1_empty_%2C__.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/258517?isDesc=false#)



## 연산자 우선순위

1. `[]` `.`
2. `()`
3. `-` (단일) `not` `!` `empty`
4. `*` `/` `div` `%` `mod`
5. `+` `-`
6. `<` `>` `<=` `>=` `lt` `gt` `le` `ge`
7. `==` `!=` `eq` `ne`
8. `&&` `and`
9. `||` `or`
10. `?` `:`

 

## 표현 언어 비활성화 : JSP에 명시하기

- <%@ page isELIgnored = "true" %>

![img](https://cphinf.pstatic.net/mooc/20180130_20/15172876538779gHtO_PNG/2_6_1___.PNG?type=w760)

---

참고 : https://www.boostcourse.org/web326/lecture/258517?isDesc=false

