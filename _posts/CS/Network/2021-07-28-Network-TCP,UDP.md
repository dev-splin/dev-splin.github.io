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

`TCP(Transmission Control Protocol)`와 `UDP(User Datagram Protocol)`는 전송 계층(Transport layer)에서 사용되는 프로토콜 입니다. TCP와 UDP는 포트 번호를 이용하여 주소를 지정하는것과 데이터 오류검사를 위한 체크섬 존재하는 두가지 공통점을 가지고 있지만 **정확성(TCP)을 추구할지 신속성(UDP)을 추구할지를 구분**하여 나뉩니다.

> **전송계층**은 TCP/IP에서 IP에 의해 전달되는 패킷의 오류를 검사하고 재전송 요구 등의 제어를 담당하는 계층입니다. 데이터의 전달을 담당한다고 생각하면 됩니다.



## TCP(Transmission Control Protocol)





### TCP(Transmission Control Protocol)

대부분의 **인터넷 응용 분야들은 신뢰성 과 순차적인 전달 을 필요**로 합니다. UDP 로는 이를 만족시킬 수 없으므로 다른 프로토콜이 필요하여 탄생한 것이 `TCP`입니다. **`TCP(Transmission Control Protocol, 전송제어 프로토콜)`는 신뢰성이 없는 인터넷을 통해 종단간에 신뢰성 있는 바이트 스트림을 전송** 하도록 특별히 설계되었습니다. `TCP` 서비스는 **송신자와 수신자 모두가 소켓이라고 부르는 종단점을 생성**함으로써 이루어집니다. `TCP` 에서 **연결 설정(connection establishment)는 `3-way handshake`를 통해 행해집니다.**

모든 `TCP` 연결은 `전이중(full-duplex)`, `점대점(point to point)`방식입니다. **전이중이란 전송이 양방향으로 동시에 일어날 수 있음을 의미**하며 **점대점이란 각 연결이 정확히 2 개의 종단점을 가지고 있음을 의미**합니다. `TCP` 는 **멀티캐스팅이나 브로드캐스팅을 지원하지 않습니다.**



## UDP(User Datagram Protocol)

`UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)`는 **비연결형 프로토콜** 입니다. IP **데이터그램을 캡슐화하여 보내는 방법과 연결 설정을 하지 않고 보내는 방법을 제공**합니다. `UDP`는 **흐름제어, 오류제어 또는 손상된 세그먼트의 수신에 대한 재전송을 하지 않습니다.** 이 모두가 사용자 프로세스의 몫입니다. `UDP`가 행하는 것은 **포트들을 사용하여 IP 프로토콜에 인터페이스를 제공**하는 것입니다.

종종 클라이언트는 서버로 짧은 요청을 보내고, 짧은 응답을 기대합니다. 만약 요청 또는 응답이 손실된다면 클라이언트는 `time out` 되고 다시 시도할 수 있으면 됩니다. 코드가 간단할 뿐만 아니라 **TCP 처럼 초기설정`(initial setup)`에서 요구되는 프로토콜보다 적은 메시지가 요구**됩니다.

`UDP`를 사용한 것들에는 `DNS`가 있습니다. 어떤 **호스트 네임의 IP 주소를 찾을 필요가 있는 프로그램**은, `DNS` 서버로 호스트 네임을 포함한 UDP 패킷을 보냅니다. 이 **서버는 호스트의 IP 주소를 포함한 UDP 패킷으로 응답**합니다. 사전에 설정이 필요하지 않으며 그 후에 해제가 필요하지 않습니다.





### TCP 헤더 정보



![img](https://blog.kakaocdn.net/dn/cIt86U/btqNiVx6GmY/nPEo5ZZsFq71gFGqAxtvxK/img.png)



| **필드**                      | **크기** | **내용**                                                     |
| ----------------------------- | -------- | ------------------------------------------------------------ |
| 송수신자의 포트 번호          | 16       | TCP로 연결되는 가상 회선 양단의 송수신 프로세스에 할당되는 포트 주소 |
| 시퀀스 번호 (Sequence Number) | 32       | 송신자가 지정하는 순서 번호, 전송되는 바이트 수를 기준으로 증가 SYN = 1 : 초기 시퀀스 번호가 된다. ACK 번호는 이 값에 1을 더한값 SYN = 0 : 현재 세션의 이 세그먼트 데이터의 최초 바이트 값의 누적 시퀀스 번호 |
| 응답 번호 (ACK Number)        | 32       | 수신 프로세스가 제대로 수신한 바이트 수를 응답하기 위해 사용 |
| 데이터 오프셋 (Data Offset)   | 4        | TCP 세그먼트의 시작 위치를 기준으로 데이터의 시작 위치를 표현(TCP 헤더의 크기) |
| 예약 필드(Reserved)           | 6        | 사용을 하지 않지만 나중을 위한 예약 필드이며 0으로 채워져야한다. |
| 제어 비트(Flag Bit)           | 6        | SYN, ACK, FIN 등의 제어 번호                                 |
| 윈도우 크기(Window)           | 16       | 수신 윈도우의 버퍼 크기를 지정할 때 사용. 0이면 송신 프로세스의 전송 중지 |
| 체크섬(Checksum)              | 16       | TCP 세그먼트에 포함되는 프로토콜 헤더와 데이터에 대한 오류 검출 용도 |
| 긴급 위치(Urgent Pointer)     | 16       | 긴급 데이터를 처리하기 위함, URG 플래그 비트가 지정된 경우에만 유효 |

 

### TCP 제어비트 (Flag Bit) 정보

| 종류 | 내용                                                         |
| ---- | ------------------------------------------------------------ |
| ACK  | 응답 번호 필드가 유효한지 설정할때 사용하며 상대방으로부터 패킷을 받았다는 걸 알려주는 패킷. 클라이언트가 보낸 최초의 SYN 패킷 이후에 전송되는 모든 패킷은 이 플래그가 설정되어야 한다. |
| SYN  | 연결 설정 요구. 동기화 시퀀스 번호. 양쪽이 보낸 최초의 패킷에만 이 플래그가 설정되어 있어야 한다.TCP 에서 세션을 성립할 때 가장먼저 보내는 패킷, 시퀀스 번호를 임의적으로 설정하여 세션을 연결하는 데에 사용되며 초기에 시퀀스 번호를 보내게 된다. |
| PSH  | 수신 애플리케이션에 버퍼링된 데이터를 상위 계층에 즉시 전달할 때 사용 |
| RST  | 연결의 리셋이나 유효하지 않은 세그먼트에 대한 응답용으로 사용 |
| URG  | 긴급 위치를 필드가 유효한지 설정 (긴급한 데이터는 다른 데이터에 비해 우선순위가 높음) |
| FIN  | 세션 연결을 종료시킬 때 사용되며 더 이상 전송할 데이터가 없을 때 연결 종료 의사 표시 |







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

**[https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4](https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4)**

[https://coding-factory.tistory.com/614](https://coding-factory.tistory.com/614)

[https://www.stevenjlee.net/2020/06/29/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-tcp-%EC%99%80-udp-tcp-vs-udp/](https://www.stevenjlee.net/2020/06/29/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-tcp-%EC%99%80-udp-tcp-vs-udp/)

[https://hack-cracker.tistory.com/111](https://hack-cracker.tistory.com/111)

[https://coding-factory.tistory.com/613](https://coding-factory.tistory.com/613)

