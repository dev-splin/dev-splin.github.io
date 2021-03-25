---
title: "Web : Rest API란?"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Rest API"
toc: true
toc_sticky: true
toc_label: 목차
---

# Rest API

HTTP 프로토콜을 이용하여 브라우저는 요청을 보내고, 그 결과를 받아 랜더링하여 화면에 보여지게 됩니다. HTTP의 이런 특징을 브라우저가 아닌 다른 클라이언트도 사용할 수 없을까? 라는 고민을 개발자들이 하게 되었습니다. 즉, 안드로이드 앱, 모바일 앱, Javascript 등에서 HTTP로 요청을 보내고 데이터를 받아올 수 없을까? 하는생각을 하게 된 것입니다. 

예를 들어, 정부에서 버스 운행 시간표를 제공한다고 생각해 보겠습니다. 정부에서는 300번 버스에 대한 시간표를 제공하는 URL을 아래와 같은 형태로 제공하는 것입니다. **`http://ip주소/api/buses/300`같은 URL을 호출 하게 되었을 때 시간표 정보를 특정한 형태로 제공한다면, 그 정보를 다양한 클라이언트에서 사용할 수 있을 것**입니다. 

HTTP 프로토콜을 이용해 요청을 받고 알맞은 데이터를 제공하는 방식중에서 `REST API`라는 것이 있습니다. 2000년도에 로이 필딩(Roy Fielding) 박사의 학위 논문에 `REST`라는 개념이 처음 등장하였습니다. `REST`는 "**Re**presentational **S**tate **T**ransfer"의 약자로 자원을 이름으로 구분하여 **해당 자원의 상태(정보)를 주고 받는 모든 것을 의미**합니다. <br>로이 필딩의 논문에는 먼저 소프트웨어 아키텍처 스타일(architectural style)에 대해 설명합니다. **아키텍처 스타일이란 아키텍처가 지켜야 하는 제약 조건들의 집합**이라고 말할 수 있습니다. 로이 필딩은 웹 아키텍처의 요구사항과 해결해야할 문제를 설명하고 이를 해결하기 위한 접근 방법을 논문에서 제시하면서, 이러한 문제를 해결하기 위한 아키텍처 스타일인 `REST`를 소개합니다.  **`REST`는 기본적으로 웹의 기존 기술과 HTTP프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일**이라고 말할 수 있습니다.



## API란?

`API`는 "**A**pplication **P**rogramming **I**nterface"의 약자로 **응용 프로그램에서 사용할수 있도록, 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만드는 인터페이스**를 뜻합니다. 주로 파일 제어, 창 제어, 화상 처리, 문자 제어 등 **자신이 원하는 기능을 제어할 수 있도록 하는 인터페이스를 제공**합니다. 따라서 원하는 기능을 제어하는 `API`를 직접 만들 수도 있습니다.



## REST 구성

REST API는 다음과 같은 3가지 부분으로 구성됩니다.

- 자원(Resource) : 자원은 Data, Meta Data, HATEOAS로 나뉩니다.

- 행위(Verb) : HTTP Method로 표현됩니다.

- 표현(Representations)

 

## REST의 특징

### 1) Uniform Interface(유니폼 인터페이스)

**구성 요소(클라이언트, 서버 등) 사이의 인터페이스는 균일(uniform)**해야합니다.

인터페이스를 일반화함으로써, 전체 시스템 아키텍처가 단순해지고, 상호 작용의 가시성이 개선되며, 구현과 서비스가 분리되므로 독립적인 진화가 가능해질 수 있습니다.

 

### 2) Stateless (무상태성)

**클라이언트와 서버의 통신에는 상태가 없어야합니다. 모든 요청은 필요한 모든 정보를 담고 있어야합니다.**

요청 하나만 봐도 바로 뭔지 알 수 있으므로 가시성이 개선되고, task 실패시 복원이 쉬우므로 신뢰성이 개선되며, 상태를 저장할 필요가 없으므로 규모 확장성이 개선될 수 있습니다.



### 3) Cacheable (캐시 가능)

**캐시가 가능**해야합니다. 즉, 모든 서버 응답은 캐시가 가능한지 그렇지 아닌지 알 수 있어야합니다. 

효율, 규모 확장성, 사용자 입장에서의 성능이 개선됩니다.



### 4) Self-descriptiveness (자체 표현 구조)

REST의 또 다른 큰 특징 중 하나는 **REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조**로 되어 있다는 것입니다.



### 5) Client - Server 구조

클라이언트-서버 스타일은 사용자 인터페이스에 대한 관심(concern)을 데이터 저장에 대한 관심으로부터 분리함으로써 **클라이언트의 이식성과 서버의 규모확장성을 개선**할 수 있습니다.



### 6) Layered System(계층형 구조)

**REST 서버는 다중 계층으로 구성**될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 합니다.

 

## REST API 설계 가이드

REST API 설계 시 가장 중요한 항목은 다음과 같습니다.



**1) URI는 정보의 자원을 표현해야 합니다.**

- resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.

- resource의 도큐먼트 이름으로는 단수 명사를 사용해야 한다.

- resource의 컬렉션 이름으로는 복수 명사를 사용해야 한다.

- resource의 스토어 이름으로는 복수 명사를 사용해야 한다.

예 : GET /members/1

 

**2) 자원에 대한 행위는 HTTP Method (GET, POST, PUT, DELETE)로 표현합니다.**



**3) URI에 HTTP Method가 들어가면 안됩니다.**

예) GET /books/delete/1 -> DELETE /books/1



**4) URI에 행위에 대한 동사 표현이 들어가면 안됩니다.**

(즉, CRUD 기능을 나타내는 것은 URI에 사용하지 않습니다.)

예) GET /books/show/1 -> GET /books/1

예) GET /books/insert/2 -> POST /books/2



**5) 경로 부분 중 변하는 부분은 유일한 값으로 대체합니다.**

(즉, id는 하나의 특정 resource를 나타내는 고유값을 의미합니다.)

예) book을 생성하는 URI: POST /students

예) id=10 인 book을 삭제하는 URI: DELETE /students/10



**6) 슬래시 구분자(/ )는 계층 관계를 나타내는데 사용합니다.**

예) http://edwith.org/courses/java



**7) URI 마지막 문자로 슬래시(/ )를 포함하지 않습니다.**

예) http://edwith.org/courses/ (X)



**8) URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며** 

**URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르면 URI도 달라져야 합니다.**

 

**9) 하이픈(- )은 URI 가독성을 높이는데 사용할 수 있습니다.**

 

**10) 밑줄( _ )은 URI에 사용하지 않습니다.**

 

**11) URI 경로에는 소문자가 적합합니다.**

URI 경로에 대문자 사용은 피하도록 합니다. RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는

대소문자를 구별하도록 규정하기 때문입니다.

 

**12) 파일 확장자는 URI에 포함하지 않습니다. Accept header를 사용하도록 합니다.**

예) http://edwith.org/files/java.jpg (X)

예) GET /files/jdk18.exe HTTP/1.1 Host: edwith.org Accept: image/jpg (O)



**13) 리소스 간에 연관 관계가 있는 경우 다음과 같은 방법으로 표현합니다.**

/리소스명/리소스 ID/관계가 있는 다른 리소스명

예) GET : /books/{bookid}/viewers (일반적으로 소유 ‘has’의 관계를 표현할 때)



**14) 자원을 표현하는 컬렉션(Collection)과 도큐먼트(Document)**

컬렉션은 객체의 집합, 도큐먼트는 객체라고 생각하면 됩니다. 컬렉션과 도큐먼트 모두 리소스로 표현할 수 있으며 URI로 표현할 수 있습니다.

예) http://edwith.org/courses/1 

courses는 컬렉션을 나타냅니다. 복수로 표현해야 합니다. courses/1 은 courses중에서 id가 1인 도큐먼트를 의미합니다.

 

### HTTP 응답 상태 코드

**잘 설계된 REST API는 URI만 잘 설계되는 것이 아니라 그 리소스에 대한 응답도 잘 표현**되야 합니다. 정확한 응답의 상태 코드만으로도 많은 정보를 전달할 수 있기 때문입니다. 자주 사용되는 HTTP 상태 코드는 다음과 같습니다. 

[![img](https://cphinf.pstatic.net/mooc/20200211_238/1581404548337vItVO_PNG/_.png?type=w760)](https://www.boostcourse.org/web326/lecture/58986?isDesc=false#)



### HATEOAS

우리는 웹 게시판을 이용한다고 생각해보겠습니다. 게시판 목록보기 화면에서 제목을 누르면 상세 보기 화면으로 이동합니다. 상세 보기 화면에서 글쓰기 버튼을 클릭하면 글쓰기 화면으로 이동합니다. 이렇게 웹이란 하이퍼 링크를 통해 관계된 URL로 이동할 수 있습니다.

REST API를 요청할 경우, 그 결과를 전달 받게 됩니다. 그 결과는 보통 JSON형태로 받게 됩니다. 예를 들어, 다음과 같은 요청을 보냈다고 생각해 보겠습니다.

`GET /books/1`

결과는 다음과 같습니다.

```markup
{
"id" : 1,
"title" : "hello spring",
"author" : "carami"
"price" : 5000 
}
```

이렇게 요청에 해당하는 결과를 보내줄 수 있는데요. **이 결과에 관련된 REST API에 대한 정보를 HATEOAS**라고 말합니다.

**HATEOAS**란 "**H**ypermedia **A**s **T**he **E**ngine **O**f **A**pplication **S**tate" 의 약자입니다.

아래의 JSON문서를 보면 `_links` 부분이 보일 것입니다. 이 부분이 HATEOAS부분입니다. 자기 자신의 URL, book 컬렉션과 관련된 URL, book저장을 위한 URL 등이 표현되어 있는 것을 알 수 있습니다. 이렇게, **DATA와 함께 관련된 URL정보를 제공하는 것을 HATEOAS**라고 말합니다.

```markup
{
    "id" : 1,
    "title" : "hello spring",
    "author" : "carami"
    "price" : 5000,
    "_links":{
        "self":{
        "href":"http://localhost:8080/books/1"
        },
        "query-books":{
        "href":"http://localhost/books"
        },
        "write-books":{
        "href":"http://localhost/books"
        }
    }
}
```



### REST와 관련된 논란

REST API에 대한 명확한 표준은 없습니다. 그런 문제로 REST API와 관련되서 많은 논쟁이 벌어집니다. REST API를 구현하는 개발자들은 아키텍처 스타일을 모두 지키는 것이 힘들게 생각하는 경우가 있는 것 같습니다. 그러다보니, 모든 아키텍처 스타일을 지키지 않고 개발하게 됩니다. **특히 REST의 HATEOAS와 자체 표현 구조(self-descriptiveness)를 만족 못하도록 개발되는 경우가 많습니다.** 이렇게 REST API를 완벽하게 구현하지 못할 경우를 Web API라고 합니다.



---

참고 : https://www.boostcourse.org/web326/lecture/58986?isDesc=false