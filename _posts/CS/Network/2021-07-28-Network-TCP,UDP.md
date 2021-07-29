---
title: "Network : TCP / UDP"
excerpt_separator: <!--more-->
categories:
  - CS(Computer Science)
  - Network
tags:
  - CS(Computer Science)
  - Network
  - "Network : TCP / UDP"
toc: true
toc_sticky: true
toc_label: 목차
---

# TCP와 UDP

가짜 개발자 CS 스터디를 위해 정리할 예정입니다. 현재는 링크만 모아놓은 상태. (인턴을 하면서 정신이 없지만 이렇게라도 CS지식에 공부하게 되어서 좋은 것 같다.)



## TCP와 UDP

### UDP(User Datagram Protocol)

`UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)`는 **비연결형 프로토콜** 입니다. IP **데이터그램을 캡슐화하여 보내는 방법과 연결 설정을 하지 않고 보내는 방법을 제공**합니다. `UDP`는 **흐름제어, 오류제어 또는 손상된 세그먼트의 수신에 대한 재전송을 하지 않습니다.** 이 모두가 사용자 프로세스의 몫입니다. `UDP`가 행하는 것은 **포트들을 사용하여 IP 프로토콜에 인터페이스를 제공**하는 것입니다.

종종 클라이언트는 서버로 짧은 요청을 보내고, 짧은 응답을 기대합니다. 만약 요청 또는 응답이 손실된다면 클라이언트는 `time out` 되고 다시 시도할 수 있으면 됩니다. 코드가 간단할 뿐만 아니라 **TCP 처럼 초기설정`(initial setup)`에서 요구되는 프로토콜보다 적은 메시지가 요구**됩니다.

`UDP`를 사용한 것들에는 `DNS`가 있습니다. 어떤 **호스트 네임의 IP 주소를 찾을 필요가 있는 프로그램**은, `DNS` 서버로 호스트 네임을 포함한 UDP 패킷을 보냅니다. 이 **서버는 호스트의 IP 주소를 포함한 UDP 패킷으로 응답**합니다. 사전에 설정이 필요하지 않으며 그 후에 해제가 필요하지 않습니다.



### TCP(Transmission Control Protocol)

대부분의 **인터넷 응용 분야들은 신뢰성 과 순차적인 전달 을 필요**로 합니다. UDP 로는 이를 만족시킬 수 없으므로 다른 프로토콜이 필요하여 탄생한 것이 `TCP`입니다. **`TCP(Transmission Control Protocol, 전송제어 프로토콜)`는 신뢰성이 없는 인터넷을 통해 종단간에 신뢰성 있는 바이트 스트림을 전송** 하도록 특별히 설계되었습니다. `TCP` 서비스는 **송신자와 수신자 모두가 소켓이라고 부르는 종단점을 생성**함으로써 이루어집니다. `TCP` 에서 **연결 설정(connection establishment)는 `3-way handshake`를 통해 행해집니다.**

모든 `TCP` 연결은 `전이중(full-duplex)`, `점대점(point to point)`방식입니다. **전이중이란 전송이 양방향으로 동시에 일어날 수 있음을 의미**하며 **점대점이란 각 연결이 정확히 2 개의 종단점을 가지고 있음을 의미**합니다. `TCP` 는 **멀티캐스팅이나 브로드캐스팅을 지원하지 않습니다.**



#### TCP 3-way Handshake

일부 그림이 포함되어야 하는 설명이므로 링크를 대신 첨부합니다.

#### Reference

[http://asfirstalways.tistory.com/356](https://asfirstalways.tistory.com/356)





---

참고 : [https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network)

[https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html](https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html)

[https://gyoogle.dev/blog/computer-science/network/UDP.html](https://gyoogle.dev/blog/computer-science/network/UDP.html)

[https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/TCP.md](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/TCP.md)

[https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/UDP.md](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/UDP.md)

[https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/3%20way%20handshake.md](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/3%20way%20handshake.md)

[https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4](https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4)

[https://coding-factory.tistory.com/614](https://coding-factory.tistory.com/614)

[https://www.stevenjlee.net/2020/06/29/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-tcp-%EC%99%80-udp-tcp-vs-udp/](https://www.stevenjlee.net/2020/06/29/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-tcp-%EC%99%80-udp-tcp-vs-udp/)

[https://hack-cracker.tistory.com/111](https://hack-cracker.tistory.com/111)

