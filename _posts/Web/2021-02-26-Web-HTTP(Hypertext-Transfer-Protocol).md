---
title: "Web : HTTP(Hypertext Transfer Protocol)"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Web : HTTP(Hypertext Transfer Protocol)"
toc: true
toc_sticky: true
toc_label: 목차
---

# HTTP (Hypertext Transfer Protocol)란?

- 팀 버너스리(Tim Berners-Lee)와 그가 속한 팀은 CERN에서 HTML뿐만 아니라 웹 브라우저 및 웹 브라우저 관련 기술과 HTTP를 발명하였습니다.
- 문서화된 최초의 HTTP버전은 HTTP v0.9(1991년)입니다.
- HTTP는 서버와 클라이언트가 인터넷상에서 데이터를 주고받기 위한 프로토콜(protocol)입니다.
- HTTP는 계속 발전하여 HTTP/2까지 버전이 등장한 상태입니다.

 

## HTTP의 장단점

- HTTP는 서버/클라이언트 모델을 따릅니다.
  - 클라이언트가 서버에 요청을 보내면 서버는 클라이언트에게 응답을 보내는 방식입니다.
- 장점
  - 불특정 다수를 대상으로 하는 서비스에는 적합합니다.
  - 클라이언트와 서버가 계속 연결된 형태가 아니기 때문에 클라이언트와 서버 간의 최대 연결 수보다 훨씬 많은 요청과 응답을 처리할 수 있습니다.
- 단점
  - 연결을 끊어버리기 때문에, 클라이언트의 이전 상황을 알 수가 없습니다.
    - 이러한 특징을 무상태(Stateless)라고 말합니다.
    - 이러한 특징 때문에 정보를 유지하기 위해서 Cookie와 같은 기술이 등장하게 되었습니다.

 

### URL (Uniform Resource Locator)

- 인터넷 상의 자원의 위치
- 특정 웹 서버의 특정 파일에 접근하기 위한 경로 혹은 주소



![URL](https://user-images.githubusercontent.com/79291114/109289854-28203d00-786a-11eb-982b-4cb5dc1517d0.jpg)

URL은 크게  세 부분으로 나누어집니다.

1. 접근 프로토콜 : 프로토콜의 종류 
2. IP주소 또는 도메인 이름, 포트번호
   - 물리적인 서버를 찾기 위해서 반드시 필요한 것은 IP주소나 도메인 주소입니다.
   - 물리적인 컴퓨터를 찾은 후에 해당 컴퓨터 안에 등장하는 소프트웨어 서버를 찾기 위해서는 포트 값이 필요합니다.
   - 하나의 물리적 컴퓨터에는 여러개의 소프트웨어 서버가 동작할 수 있는데, 이 서버는 포트 값이 다르게 동작해야 됩니다.
3. 자원의 위치



## HTTP 작동방식

[![img](https://cphinf.pstatic.net/mooc/20180119_25/1516354290022wUY3x_PNG/http_-_.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/58942/?isDesc=false#)

1. 클라이언트가 먼저 원하는 서버에 접속합니다. (connect)
2. 클라이언트가 서버에 요청합니다. (request)
3. 서버가 응답결과를 클라이언트한테 줍니다. (response)
4. 서버가 응답결과를 주었다면 연결이 끊깁니다. (close)



### 응답/요청 데이터 포멧

- 응답/요청 데이터 포멧은 헤더,빈줄(공백),바디로 이루어져 있습니다.
- 헤더 부분에 요청 메서드, 요청 URI, HTTP 프로토콜 버전이 있습니다. 

- 요청 메서드 : GET, PUT, POST, PUSH, OPTIONS 등의 요청 방식이 옵니다.
- 요청 URI : 요청하는 자원의 위치를 명시합니다.
- HTTP 프로토콜 버전 : 웹 브라우저가 사용하는 프로토콜 버전입니다.



#### 요청 메서드 종류

첫번째 줄의 요청메소드는 서버에게 요청의 종류를 알려주기 위해서 사용됩니다. 각각의 메소드 이름은 다음과 같은 의미를 가집니다.<br/>참고로 최초의 웹 서버는 GET방식만 지원해줬습니다.

- GET : 정보를 요청하기 위해서 사용합니다. (SELECT)
  - 요청할 때 가지고 가야되는 자원을 URI에 붙여서 가지고 가기 때문에 요청 바디가 없습니다.
- POST : 정보를 밀어넣기 위해서 사용합니다. (INSERT)
- PUT : 정보를 업데이트하기 위해서 사용합니다. (UPDATE)
- DELETE : 정보를 삭제하기 위해서 사용합니다. (DELETE)
- HEAD : (HTTP)헤더 정보만 요청합니다. 해당 자원이 존재하는지 혹은 서버에 문제가 없는지를 확인하기 위해서 사용합니다.
- OPTIONS : 웹서버가 지원하는 메서드의 종류를 요청합니다.
- TRACE : 클라이언트의 요청을 그대로 반환합니다. 예컨데 echo 서비스로 서버 상태를 확인하기 위한 목적으로 주로 사용합니다.



#### 응답 요청 포멧

**헤더 :  `GET /servlet/query?a=10&b=90 HTTP/1.1`**

- GET : 요청메서드

- servlet/query?a=10&b=90 : 요청 URI

- HTTP/1.1 : HTTP 프로토콜 버전

- 바디가 없습니다



#### 응답 데이터 포멧

**헤더 : HTTP/1.1 200ok**

첫 줄에 응답 HTTP 프로토콜의 버전, 그 다음은 응답 코드 그리고 응답 메세지 등으로 나올 수 있습니다. 나머지 헤더 부분에는 날짜, 웹 서버 이름과 버전, 콘텐츠 타입, 캐시 제어 방식, 콘텐츠 길이 등의 값이 나오게 됩니다. 빈 줄 다음, 바디에 나오는 것은 실제 응답 리소스 데이터가 나오는 부분 입니다.



## **HTTPS**(HyperText Transfer Protocol over Secure Socket Layer, **HTTP** over TLS, **HTTP** over SSL, **HTTP** Secure)

**HTTPS**(**H**yper**T**ext **T**ransfer **P**rotocol over **S**ecure Socket Layer, **HTTP over [TLS](https://ko.wikipedia.org/wiki/전송_계층_보안)**,[[1]](https://ko.wikipedia.org/wiki/HTTPS#cite_note-HTTP_Over_TLS-1)[[2]](https://ko.wikipedia.org/wiki/HTTPS#cite_note-HTTPS-ranking-signal-2) **HTTP over SSL**,[[3]](https://ko.wikipedia.org/wiki/HTTPS#cite_note-Enabling_HTTP_Over_SSL-3) **HTTP Secure**[[4]](https://ko.wikipedia.org/wiki/HTTPS#cite_note-Secure_your_site_with_HTTPS-4)[[5]](https://ko.wikipedia.org/wiki/HTTPS#cite_note-What_is_HTTPS?-5))는 [월드 와이드 웹](https://ko.wikipedia.org/wiki/월드_와이드_웹) 통신 프로토콜인 [HTTP](https://ko.wikipedia.org/wiki/HTTP)의 보안이 강화된 버전입니다. HTTPS는 통신의 인증과 암호화를 위해 [넷스케이프 커뮤니케이션즈 코퍼레이션](https://ko.wikipedia.org/wiki/넷스케이프_커뮤니케이션즈_코퍼레이션)이 개발했으며, [전자 상거래](https://ko.wikipedia.org/wiki/전자_상거래)에서 널리 쓰입니다. 

HTTPS는 소켓 통신에서 일반 텍스트를 이용하는 대신에, [SSL](https://ko.wikipedia.org/wiki/SSL)이나 [TLS](https://ko.wikipedia.org/wiki/트랜스포트_레이어_보안) 프로토콜을 통해 [세션](https://en.wikipedia.org/wiki/Session_(computer_science)) 데이터를 암호화합니다. 따라서 데이터의 적절한 보호를 보장합니다. HTTPS의 기본 [TCP/IP](https://ko.wikipedia.org/wiki/TCP/IP) 포트는 443입니다.

HTTPS를 사용하는 웹페이지의 [URI](https://ko.wikipedia.org/wiki/통합_자원_식별자)는 'http://'대신 'https://'로 시작합니다.



---

참고 : https://www.boostcourse.org/web326/lecture/58942/?isDesc=false