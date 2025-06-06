---
title: "Network : Socket"
excerpt_separator: <!--more-->
categories:
  - CS(Computer Science)
  - Network
tags:
  - CS(Computer Science)
  - Network
  - "Network : Socket"
toc: true
toc_sticky: true
toc_label: 목차
---

# 소켓(Socket)

소켓(Socket)은 사전적인 의미로는 구멍, 연결, 콘센트등을 의미합니다. 이와 마찬가지로 네트워크에서의 소켓을 간단히 말해보자면, 프로세스가 네트워크를 통해서 데이터를 주고받으려면 반드시 열어야 하는 출입문 같은 것이라고 할 수 있습니다.



### 호스트(Host)

네트워크에 연결된 모든 종류의 장치를 `노드(Node)`라고 부르는데, **노드 중에서도 네트워크 주소(IP 주소)가 할당된 애들을 호스트(Host)**라고 부릅니다. 스마트폰이든 데스크톱이든 노트북이든 인터넷에 연결되어 있으면 다 호스트라고 보면 됩니다. 

이 호스트들끼리 서로 데이터를 주고 받게 되는데, 이 때, **네트워크 공간 상에서 데이터를 주고 받는다는 것은 호스트의 프로세스까지 데이터가 오고가는 것을 의미**합니다.



### 포트(Port)

데이터가 네트워크를 타고 목적지 호스트에 도착했다고 끝이 아니라, 호스트에서 동작하는 여러 프로세스 중에 실제로 이 데이터를 받아야 하는 프로세스 까지 전달 되어야 합니다. 이 때, 포트가 필요하게 됩니다.

포트는 **네트워크를 통해 데이터를 주고받는 프로세스를 식별하기 위해 호스트 내부적으로 프로세스가 할당받는 고유한 값**입니다.



### 소켓 vs 포트

소켓과 포트는 좀 다른데, **포트는 보통 프로세스당 고유의 포트 하나만 할당**받게 되고, 

**소켓은 하나의 프로세스에서 같은 프로토콜, 같은 IP 주소, 같은 포트 번호를 가지는 수십 혹은 수만 개의 소켓을 가질 수 있습니다.**

이런 이유 때문에 하나의 프로세스는 하나의 포트만으로도 다른 여러 호스트에 있는 프로세스의 요청을 처리할 수 있고, 게임 서버의 동시 접속자 수가 수십 수백만이 될 수 있는 것입니다.





## 소켓의 종류

소켓의 종류는 아래와 같습니다.



### 스트림(TCP에서 사용)

**스트림 소켓은 양뱡향으로 바이트 스트림을 전송할 수 있는 연결 지향형 소켓으로 양쪽 어플리케이션이 모두 데이터를 주고받을 수 있습니다.**

- 스트림 소켓은 오류수정 , 전송처리 , 흐름제어등을 보장해 주며 송신된 순서에 따른 중복되지 않은 데이터를 수신하게 됩니다.
-  이 소켓은 각 메시지를 보내기 위해 별도의 연결을 맺는 행위를 하므로 약간의 오버헤드가 존재합니다.
- 소량의 데이터보다는 대량의 데이터를 보내는 경우에 적당합니다.
-  스트림 소켓은 이러한 품질의 통신을 수행하기 위해서 `TCP`를 사용합니다.



### 데이터그램(== 메시지 , UDP에서 사용)

**명시적으로 연결을 맺지 않으므로 비 연결형 소켓이라고 합니다.** 

- 메시지는 대상 소켓으로 전송되며 대상 소켓은 메시지를 적절히 수신합니다.
- `스트림 소켓`을 사용하는 것이 `데이터그램 소켓을` 사용하는 것보다 신뢰성이 높은 방법이지만 연결을 수립하는데 드는 `오버헤드`는 무시할 수 없습니다.

- 데이터그램 소켓을 사용하려면 클라이언트에서 서버로 데이터를 전송할 때 `UDP`를 사용합니다.
- 이 프로토콜에서는 메시지의 크기에 약간의 제한이 있으며 메시지의 확실한 전달 역시 보장하지 않으며 통신 중 데이터를 잃어버려도 오류가 발생하지 않습니다.



### RAW

**RAW소켓은 패킷을 가져오면 TCP/IP 스택상의 TCP,UDP 계층을 우회하여 바로 어플리케이션으로 송신하는 소켓**입니다. 

이런 소켓에서 패킷은 TCP/UDP필터를 통해 전달되지 않으므로 원형 그대로의 패킷을 볼 수 있습니다. **이것의 의미는 모든 데이터를 적절히 처리하거나 헤더를 제거하고 이를 파싱하는 과정을 모두 수신 어플리케이션에서 담당해야 한다는 것**입니다.

실제 RAW소켓을 이용하여 프로그래밍을 하는 일은 거의 드물며 만약 시스템 소프트웨어나 패킷을 분석하는 프로그램을 개발할 경우 필요할 수도 있다고 합니다.





## 다양한 관점의 소켓

소켓 프로그래밍에서 소켓이라는 용어의 의미는 데이터 타입, 통신 종단점, 네트워크 프로그래밍 인터페이스 처럼 3가지 관점에서 볼 수 있습니다.



### 데이터 타입

소켓은 파일 디스크립터(file descriptor) 혹은 핸들(handle)과 유사한 개념으로, 일단 만들고 나면 함수를 호출하여 손쉽게 네트워크 통신을 수행할 수 있습니다. 

> **파일 디스크립터** : 리눅스 혹은 유닉스 계열의 시스템에서 프로세스(process)가 파일(file)을 다룰 때 사용하는 개념으로, 프로세스에서 특정 파일에 접근할 때 사용하는 추상적인 값입니다.



#### 응용 프로그램 통신 시 필요 요소

네트워크는 아래와 같은 필요 요소를 통해 소켓을 구분할 수 있습니다.

- 사용할 프로토콜(TCP, UDP 등)
- 송신 측 IP 주소
- 송신 측 포트 번호
- 수신 측 IP 주소
- 수신 측 포트 번호



### 통신 종단점

소켓은 응용 프로그램 관점에서 통신 종단점(communication end-point), 즉 통신의 출발점과 도착점이라고 할 수 있습니다.



### 네트워크 프로그래밍 인터페이스

TCP/IP 프로토콜의 관점에서 소켓은 네트워크 프로그래밍 인터페이스에 불과합니다. TCP/IP 프로토콜 구조에서 소켓은 응용 계층과 전송 계층 사이에 위치한다고 생각하면 됩니다.



![socket-interface](https://user-images.githubusercontent.com/79291114/130019942-73e2f295-30b9-4e29-8ab5-0d5ac10d098c.jpg)











## 소켓의 통신

소켓은 OSI 7 Layer의 네 번째 계층인 전송계층의 TCP와 UDP로 전송할 수 있습니다. **TCP로 전송하는 소켓을 TCP 소켓 혹은 TCP/IP 소켓**이라고 하고, **UDP로 전송하는 소켓을 UDP 소켓**이라고 합니다.

보통 **데이터를 능동적으로 보내는 쪽을 클라이언트, 수동적으로 받는 쪽을 서버라고 표현**합니다.



### TCP / IP 소켓 흐름

최초 한 곳에서 무작정 연결을 시도한다고 해서, 그 요청이 무조건 받아들여지고 연결이 만들어져 데이터를 주고 받을 수 있게 되진 않습니다. 한 곳에서 연결 요청을 보낸다고 하더라도 그 대상 시스템이 그 요청을 받아들일 준비가 되어 있지 않다면, 해당 요청은 무시되고 연결은 만들어지지 않습니다.





![sokect-flow](https://user-images.githubusercontent.com/79291114/129993743-b1594f47-f119-4de3-8f86-f662067efa27.png)

#### 클라이언트 측 흐름

1. **socket()** : 소켓 생성
2. **connect()** : 서버 측에 연결 요청
3. **send() / recv()** : 서버 소켓에서 연결을 받으면 데이터를 송수신
4. **close()** : 모든 처리가 완료되면 소켓을 닫음



#### 서버 측 흐름

1. **socket()** : 소켓 생성
   - 이 때의 소켓은 연결 확인 용 소켓
2. **bind()** : 서버가 사용할 IP 주소와 포트 번호를 생성한 소켓에 결합
3. **listen()** : 클라이언트로부터 연결 요청이 수신되는지 주시
4. **accept()** : 요청이 수신되면 accept 후 소켓 생성
   - 이 때의 소켓은 실제 데이터 전송용 소켓
5. **send() / recv()** : 데이터 송수신
6. **close()** : 소켓을 닫음



> 개인적인 생각 : "클라이언트에서도 수동적으로 받는 경우가 있을 수도 있지 않을까?" 싶어서 서버와 클라이언트라고 표현하는 것 보다 TCP / UDP 에서 처럼 active / passive로 표현하는 것이 더 좋지 않을까 생각 해봤는데, 클라이언트에서 서버를 listen() 상태로 대기하는 경우가 생각나질 않아 클라이언트, 서버로 표현하였습니다.



#### 클라이언트 측 흐름 상세 설명

클라이언트 측 흐름에서 각 API의 상세 설명입니다.



##### Socket()

**소켓 통신을 위해 가장 먼저 해야 할 일은 소켓을 생성**하는 것입니다. 이 때 소켓의 종류를 지정할 수 있는데, **TCP 소켓을 위해서는 스트림 타입**, **UDP 소켓을 위해서는 데이터그램 타입**을 지정할 수 있습니다.

최초 소켓이 만들어지는 시점에는 어떠한 연결 대상에 대한 정보도 들어 있지 않습니다. 그러기에 연결 대상 즉, `IP` 와 `Port`를 지정하고 연결 요청을 전달하기 위해서는 생성한 소켓을 사용하여 `connect()` API를 호출해야 합니다.



##### connect()

**connect() API는 IP주소와 포트 번호로 식별되는 대상으로 연결 요청**을 보냅니다. connect() API는 Blocking 방식으로 동작하기에, 연결 요청에 대한 결과가 결정되기 전에는 connect()의 실행이 끝나지않고 대기합니다.

호출이 성공하면 `send() / recv()` API 를 통해 데이터를 송수신 할 수 있습니다.



##### send() / recv()

**연결된 소켓을 통해 데이터를 보낼 때는 `send()`, 수신에는 `recv()` API를 사용**합니다.두 API 모두 connect와 동일하게 Blocking 방식으로 동작합니다. 그러므로 두 호출이 모두 결과가 결정되기 전까지 API가 리턴되지 않습니다.

특히 recv() 같은 경우는 한번 실행되면 언제 어떤 데이터가 전송되어 올 것인지 알 수 없기때문에, 데이터 수신을 위해서 별도의 스레드를 실행하여 데이터 수신을 기다립니다.



##### close()

**데이터 송수신이 필요없게 되면, 소켓을 닫기 위해 close() API를 호출**합니다. 해당 소켓은 닫힌 이후에는 재사용이 불가능합니다.



#### 서버 측 흐름 상세 설명

서버 측 흐름에서 각 API의 상세 설명입니다.



##### Socket()

클라이언트와 동일하게 **소켓을 생성**합니다.



##### bind()

**bind()에서 사용되는 파라미터는 포트 번호 혹은 IP 주소 + 포트 번호**입니다. 

시스템 상에서 많은 수의 프로세스가 동작하기에 서버에 접근할 수 있는 가상화된 포트를 지정해야 합니다. 

운영체제는 소켓들이 중복된 포트 번호를 사용하지 않도록, 내부적으로 포트 번호와 소켓 연결 정보를 관리하는데, bind() 호출과정에서 중복되는 포트 사용이 있으면 운영체제가 포트 할당을 거부하고, API는 에러를 리턴합니다.



##### listen()

서버 소켓에 포트 번호를 결합하고 나면, **서버 소켓을 통해 클라이언트의 요청을 받을 준비가 된 것**입니다. **클라이언트의 연결 요청이 수신될때까지 계속 기다리게 됩니다.** 이 때, 요청이 수신되는 경우와 에러가 발생하는 경우에는 대기 상태를 빠져나오게 됩니다.

클라이언트 연결 요청에 대한 정보는 시스템 내부적으로 관리되는 큐에 쌓이게 됨으로, 대기 중인 연결 요청을 큐로부터 꺼내와서, 연결을 수립하기 위해서는 `accept()` 를 호출하면 됩니다.



##### accept()

최종적으로 accept() 을 호출해서 소켓 간 연결이 수립이 됩니다. 여기서 한가지 독특한 부분은 **accept() 가 호출되어 소켓 간 연결을 수립할 때 새로운 소켓을 만들어 연결하게 된다는 것**입니다.

**즉, 서버 소켓의 역할은 단순히 클라이언트 연결을 대기하고 연결 수립 요청을 하고 소켓을 닫는 것**입니다.



##### send() / recv()

클라이언트와 동일합니다.



##### close()

클라이언트와 동일합니다. 하지만 서버 소켓의 경우 클라이언트와 연결을 수립했던 소켓뿐만 아니라 클라이언트의 연결을 대기하는 소켓 또한 닫아줘야 함으로 유의해야 합니다.





## 기존의 양방향 통신 방법

Web Socket은 웹페이지와 서버 간에 실시간 상호작용을 위해 만들어진 스펙입니다.

http 규격 자체가 클라이언트에서 서버로의 단방향 통신을 위해 만들어진 방법으로, Web Socket 이전에는 실시간 통신을 위해서 일반 http request에 약간의 트릭을 사용해서 실시간인것 처럼 작동하게 하는 아래와 같은 기술들이 있었습니다.



### Polling

**클라이언트가 평범한 http request를 서버로 계속 날려서 이벤트 내용을 전달받는 방식**입니다. 가장 쉬운방법이지만 클라이언트가 계속적으로 request를 날리기때문에 클라이언가 많아지면 서버의 부담이 급증하게 됩니다.

http request connection을 맺고 끊는것 자체가 부담이 많은 방식인데, 그럼에도 불구하고 클라이언트에서 실시간정도의 빠른 응답을 기대하기도 어렵습니다.

![polling](https://user-images.githubusercontent.com/79291114/130024564-0c406b48-abb6-4d82-ba7f-cf8c50871b0a.gif)



### Long Polling

클라이언트에서 서버로 일단 http request를 날립니다. 이상태로 계속 기다리다가 서버에서 해당 클라이언트로 전달할 이벤트가 있다면 그순간 response 메시지를 전달하면서 연결이 종료됩니다. 클라이언트에서는 곧바로 다시 http request를 날려서 서버의 다음 이벤트를 기다리게 되는 방식입니다.

일반 polling 방식보다는 서버의 부담이 줄겠지만 클라이언트로 보내는 이벤트들의 시간간격이 좁다면 polling 과 별 차이가 없게 되며, 다수의 클라이언트에게 동시에 이벤트가 발생될 경우에는 곧바로 다수의 클라이언트가 서버로 접속을 시도하면서 서버의 부담이 급증하게 됩니다.

![long polling](https://user-images.githubusercontent.com/79291114/130024568-fcbde896-3a34-40b2-aec7-e781d1d70af5.gif)



### Streaming

long polling과 마찬가지로 클라이언트에서 서버로 일단 http request를 날립니다. 서버에서 클라이언트로 이벤트를 전달할때 해당 요청을 끊지 않고 필요한 메시지만 보내기를(flush) 반복하는 방식입니다. long polling에 비해 서버에서 메시지를 보내고도 다시 http request 연결을 하지 않아도 되어 부담이 경감될것으로 보입니다.

![streaming](https://user-images.githubusercontent.com/79291114/130024558-a1f8d328-d0bc-4edb-894e-1f415f380d86.gif)





## Web Socket

위와 같은 방법에서 벗어난 방식으로 **클라이언트 서버간 양방향 통신이 가능하게 하기 위해서 HTML5 표준의 일부로 Web Socket**이 만들어지게 되었습니다.

Web Socket이 기존의 일반 TCP Socket과 다른 점은 최초 접속이 일반 http request를 통해 handshaking과정을 통해 이루어 진다는 점입니다. http request를 그대로 사용하기 때문에 기존의 80(HTTP), 443(HTTPS) 포트로 접속을 하므로 추가로 방화벽을 열지 않고도 양방향 통신이 가능하고, http 규격인 CORS적용이나 인증등의 과정을 기존과 동일하게 가져갈 수 있는것이 장점입니다. 하지만 아래와 같은 문제점이 있습니다.

- **Web Socket 미지원 웹 브라우저** : 오래된 버전의 웹 브라우저는 Web Socket을 지원하지 않습니다.

- **웹 브라우저 이외의 클라이언트 지원** : 서버의 입장에서 클라이언트는 웹 브라우저뿐만이 아닙니다.

그래서 이를 해결하기 위해 나온 기술들이 몇가지 있는데 원리는 간단합니다. 웹페이지가 열리는 브라우저가 Web Socket을 지원하면 일반 Web Socket 방식으로 동작하고 지원하지 않는 브라우저라면 위에서 설명한 일반 http 스펙을 이용해서 실시간통신을 흉내낼수 있는 방식으로 통신을 하게 해주는 것입니다. 아래는 이런 방식으로 만들어진 솔루션입니다.



### Socket.io

Node.js 기반으로 만들어진 기술로 자체 스펙으로 만들어진 Socket.io 서버를 만들고 Socket.io 클라이언트와 브라우저에 구애받지 않고 실시간 통신이 가능해집니다. Socket.io는 [Node.js](https://nodejs.org/) 기반이기때문에 모든 코드가 Javascript로 작성되어 있습니다. 서버, 클라이언트 모두 Javascript 기반으로 개발하는 것이 기본입니다. 그러다보니 자바 개발자들은 Socket.io를 쓸 수 없습니다. [자바로 개발이 가능하게 해주는 방법](https://github.com/keesun/mod-socket-io)이 몇가지 있긴한 것 같지만 역시 Javascript 기반 솔루션은 Javascript로 개발해야 문제발생을 줄일 수 있을 것입니다.



### SockJS

Spring Framework에서도 Web Socket을 지원합니다. 스프링 메뉴얼에 Web Socket 부분을 보면 위와 같은 브라우저 문제를 해결하기 위한 방법으로 [SockJS를 솔루션으로 제시](https://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#websocket-fallback)합니다. 역시 자체 스펙으로 Web Scoket 미지원 브라우저를 관리합니다. 서버 개발 시 스프링 설정에서 일반 Web Socket 으로 통신할지 SockJS 호환으로 통신할지 결정할 수 있습니다. 클라이언트쪽은 SockJS Client를 통해 서버와 통신합니다.



### STOMP

`STOMP (Simple Text Oriented Messaging Protocol)`은 메세징 전송을 효율적으로 하기 위해 탄생한 프로토콜 입니다. 기본적으로는 pub / sub 구조로 되어 있어 메세지를 전송하고 메세지를 받아 처리하는 부분이 확실히 정해져 있기 때문에 개발자 입장에서 명확하게 인지하고 개발할 수 있는 이점이 있습니다.

**STOMP 프로토콜은 Web Socket 위에서 동작하는 프로토콜로써 클라이언트와 서버가 전송할 메세지의 유형, 형식 내용들을 정의하는 매커니즘** 입니다. 또한 STOMP를 이용하면 메세지의 헤더에 값을 줄 수 있어 헤더 값을 기반으로 통신 시 인증 처리를 구현하는 것도 가능하며 STOMP 스펙에 정의한 규칙만 잘 지키면 여러 언어 및 플랫폼 간 메시지를 상호 운영할 수 있습니다.



#### pub / sub 구조

![pub,sub](https://user-images.githubusercontent.com/79291114/130039498-17c43100-55f9-4d7e-8dd4-f4f184edbad0.png)



Publish / Subscribe 구조로 발신자(Publisher)는 수신자(Subscriber)에 대한 정보를 몰라도 일단 메세지를 채널이라는 중간 컴포넌트에 보내 놓습니다. 메시지에 맞는 Topic으로 보내놓으면 해당 Topic을 구독중인 수신자에게만 메세지가 가게 됩니다.

즉, **발신자는 특별한 수신자가 정해져 있지 않고 Topic에게로만 메세지를 보내게 되고 수신자는 Topic에서 메시지를 받는 방식**입니다. 때문에 발신자와 수신자간의 종속 관계가 느슨해져 안정적이고 확장에 용이하지만, 중간 컴포넌트인 채널을 통해서 메세지를 전달하기 때문에 부하가 좀 더 걸릴 수 있습니다.



**읽어 보면 좋은 흥미로운 글(Socket.io와 AngularJS 사용)**

[실시간 서비스 경험기(배달 운영 시스템) - 우아한 형제들 기술 블로그](https://techblog.woowahan.com/2547/)



---

참고 : [https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=myca11&logNo=221389847130&categoryNo=24&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView](https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=myca11&logNo=221389847130&categoryNo=24&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)

[https://popbox.tistory.com/66](https://popbox.tistory.com/66)

[https://velog.io/@devsh/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EA%B8%B0%EC%B4%88-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-%EC%86%8C%EC%BC%93%EA%B3%BC-%EC%86%8C%EC%BC%93-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EA%B0%9C%EB%85%90](https://velog.io/@devsh/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EA%B8%B0%EC%B4%88-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-%EC%86%8C%EC%BC%93%EA%B3%BC-%EC%86%8C%EC%BC%93-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EA%B0%9C%EB%85%90)

[https://recipes4dev.tistory.com/153](https://recipes4dev.tistory.com/153)

[https://helloworld-88.tistory.com/215](https://helloworld-88.tistory.com/215)

[https://12bme.tistory.com/62](https://12bme.tistory.com/62)

[https://bcho.tistory.com/896](https://bcho.tistory.com/896)

[https://adrenal.tistory.com/20](https://adrenal.tistory.com/20)

[https://d2.naver.com/helloworld/1336](https://d2.naver.com/helloworld/1336)

[https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications](https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications)

