---
title: "Spring : 레이어드 아키텍처(Layered Architecture)"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Layered Architecture"
toc: true
toc_sticky: true
toc_label: 목차
---

# 레이어드 아키텍처(Layered Architecture)

**Controller에서 중복되는 부분을 처리하려면?**

url은 다르지만 url에 해당하는 웹페이지를 보여주기 위해서 실행되는 부분 중에 중복이 되는 부분이 있을 수 있습니다. 예를 들어 쇼핑몰에서 게시판에서도 회원 정보를 보여주고, 상품 목록 보기에서도 회원 정보를 보여줘야 한다면 회원 정보를 읽어오는 코드는 어떻게 해야 할까요? 

**회원 정보를 읽어 들이는 것만 별도의 객체로 만들고 해당 컨트롤러들에서 이용**하면 됩니다.이런 컨트롤러들이 중복적으로 호출되는 부분들을 별도의 객체인 `서비스`로 구현하게 됩니다.

 

## 컨트롤러와 서비스

비지니스 메서드를 별도의 `Service`객체에서 구현하도록 하고 컨트롤러는 `Service`객체를 사용하도록 합니다.

[![img](https://cphinf.pstatic.net/mooc/20180219_85/1519008848012uvMNx_PNG/1.png?type=w760)](https://www.boostcourse.org/web326/lecture/58982/?isDesc=false#)



### 서비스(Service)객체란?

`비지니스 로직(Business logic)`을 수행하는 메소드를 가지고 있는 객체를 `서비스 객체`라고 합니다. 보통 하나의 비지니스 로직은 하나의 트랜잭션으로 동작합니다.

 

### 트랜잭션(Transaction)이란?

트랜잭션의 특징은 크게 4가지로 구분됩니다.

1. **원자성 (Atomicity)**
2. **일관성 (Consistency)**
3. **독립성 (Isolation)**
4. **지속성 (Durability)**



#### 원자성 (Atomicity)

전체가 성공하거나 전체가 실패하는 것을 의미합니다. 예를 들어 "출금"이라는 기능의 흐름이 다음과 같다고 생각해봅시다.

1. 잔액이 얼마인지 조회합니다.
2. 출금하려는 금액이 잔액보다 작은지 검사합니다.
3. 출금하려는 금액이 잔액보다 작다면 (잔액 - 출금액)으로 수정합니다.
4. 언제, 어디서 출금했는지 정보를 기록합니다.
5. 사용자에게 출금합니다.

위의 작업이 4번에서 오류가 발생했다면 어떻게 될까요? 4번에서 오류가 발생했다면, 앞의 작업을 모두 원래대로 복원을 시켜야 합니다. 이를 `rollback`이라고 합니다.

5번까지 모두 성공했을 때만 정보를 모두 반영해야 합니다. 이를 `commit` 한다고 합니다. 

이렇게 `rollback` 하거나 `commit`을 하게 되면 하나의 트랜잭션 처리가 완료됩니다.

 

#### 일관성 (Consistency)

일관성은 트랜잭션의 작업 처리 결과가 항상 일관성이 있어야 한다는 것입니다.

**트랜잭션이 진행되는 동안에 데이터가 변경되더라도** 업데이트된 데이터로 트랜잭션이 진행되는 것이 아니라, **처음에 트랜잭션을 진행하기 위해 참조한 데이터로 진행**됩니다. 이렇게 함으로써 각 사용자는 일관성 있는 데이터를 볼 수 있는 것입니다.

 

#### 독립성 (Isolation)

독립성은 둘 이상의 트랜잭션이 동시에 병행 실행되고 있을 경우에 어느 하나의 트랜잭션이라도 다른 트랜잭션의 연산을 끼어들 수 없습니다.

**하나의 특정 트랜잭션이 완료될 때까지, 다른 트랜잭션이 특정 트랜잭션의 결과를 참조할 수 없습니다.**

 

#### 지속성 (Durability)

지속성은 트랜잭션이 성공적으로 완료됬을 경우, **결과는 영구적으로 반영**되어야 한다는 점입니다.

 

### JDBC 프로그래밍에서 트랜잭션 처리 방법

DB에 연결된 후 `Connection`객체의 `setAutoCommit`메소드에 `false`를 파라미터로 지정합니다. 입력, 수정, 삭제 SQL이 실행을 한 후 모두 성공했을 경우 `Connection`이 가지고 있는 `commit()`메소드를 호출합니다.

 

### @EnableTransactionManagement

`Spring Java Config`파일에서 트랜잭션을 활성화 할 때 사용하는 어노테이션입니다.

`Java Config`를 사용하게 되면 `PlatformTransactionManager` 구현체를 모두 찾아서 그 중에 하나를 매핑해 사용합니다.

특정 트랜잭션 메니저를 사용하고자 한다면 `TransactionManagementConfigurer`를 `Java Config`파일에서 구현하고 원하는 트랜잭션 매니저를 리턴하도록 합니다.

아니면, 특정 트랜잭션 메니저 객체를 생성시 `@Primary` 어노테이션을 지정합니다.

 

## 레이어드 아키텍처

`Presentation Layer`에서는 `컨트롤러 객체`가 동작을 하게 됩니다.

`Service Layer`에서는 `비즈니스 메서드`를 가지고 있는 `서비스 객체`가 동작하게 됩니다.

`Repository Layer`는 실제 데이터베이스에 접근해서 데이터를 가져오는 등의 `DAO객체`가 동작하게 됩니다.



[![img](https://cphinf.pstatic.net/mooc/20180219_283/1519009121486u3LkD_PNG/2.png?type=w760)](https://www.boostcourse.org/web326/lecture/58982/?isDesc=false#)

현재 지금 `Presentation Layer`는 웹으로 사용하기 때문에 `Presentation Layer`가 웹에서 보여지게 만들게 됩니다. 이 `Presentation Layer` 부분을 `Window프로그래밍`이나 `앱`으로 바꾸면 `Service`와 `DAO`를 재사용해서 만들수도 있습니다.

그렇기 때문에 **재사용 측면이나 유지 보수적인 면에서 봤을 때**, `Presentation Layer` 부분과 뒤쪽에 있는 `Service Layer`, `Repository Layer` 부분은 아예 **설정파일들도 분리를 하는 게 좋습니다.**



### 설정의 분리

`Spring` 설정 파일을 `Presentation Layer`쪽과 나머지를 분리할 수 있습니다.

`web.xml` 파일에서 `Presentation Layer`에 대한 스프링 설정은 `DispathcerServlet`이 읽도록 하고, 그 외의 설정은 `ContextLoaderListener`를 통해서 읽도록 합니다.

`DispatcherServlet`을 경우에 따라서 2개 이상 설정할 수 있는데 이 경우에는 각각의 `DispathcerServlet`의 `ApplicationContext`가 각각 독립적이기 때문에 각각의 설정 파일에서 생성한 빈을 서로 사용할 수 없습니다.

위의 경우와 같이 동시에 필요한 빈은 `ContextLoaderListener`를 사용함으로써 공통으로 사용하게 할 수 있습니다.

`ContextLoaderListener`와 `DispatcherServlet`은 각각 `ApplicationContext`를 생성하는데, `ContextLoaderListener`가 생성하는 `ApplicationContext`가 `root컨텍스트`가 되고 `DispatcherServlet`이 생성한 인스턴스는 `root컨텍스트`를 부모로 하는 `자식 컨텍스트`가 됩니다. 참고로, 자식 컨텍스트들은 root컨텍스트의 설정 빈을 사용할 수 있습니다.



---

참고 : https://www.boostcourse.org/web326/lecture/58982/?isDesc=false

