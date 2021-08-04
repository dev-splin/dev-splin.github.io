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

`TCP(Transmission Control Protocol)`와 `UDP(User Datagram Protocol)`는 전송 계층(Transport layer)에서 사용되는 프로토콜 입니다. TCP와 UDP는 패킷을 한 컴퓨터에서 다른 컴퓨터로 전달해주는 `IP 프로토콜`을 기반으로 구현되어 있고, 포트 번호를 이용하여 주소를 지정하는것과 데이터 오류검사를 위한 체크섬이 존재하는 두가지 공통점을 가지고 있지만 **정확성(TCP)을 추구할지 신속성(UDP)을 추구할지를 구분**하여 나뉩니다.

> **전송계층**은 TCP/IP에서 IP에 의해 전달되는 패킷의 오류를 검사하고 재전송 요구 등의 제어를 담당하는 계층입니다. 데이터의 전달을 담당한다고 생각하면 됩니다.

> **포트 번호**
> TCP와 UDP는 ‘포트 번호’라는 숫자를 이용하여 컴퓨터 안의 어떤 서비스(애플리케이션)에게 데이터를 전달하면 좋은지를 식별합니다. 포트 번호는 ‘0~65535’(16비트 분)까지의 숫자로 되어 있으며, 범위에 따라 용도가 정해져 있습니다. ‘0~1023’은 ‘잘 알려진 포트(well-known port)’라고 해서 웹 서버나 메일 서버 등과 같이 일반적인 서버 소프트웨어가 클라이언트의 서비스 요청을 대기할 때 사용합니다. ‘1024~49151’은 ‘등록된 포트(registered port)’로, 제조업체의 독자적인 서버 소프트웨어가 클라이언트의 서비스 요청을 대기할 때 사용합니다. ‘49152~65535’는 ‘동적 포트(dynamic port)’로, 서버가 클라이언트를 식별하기 위해 사용합니다.



### 간단한 설명

일단 간단하게 TCP와 UDP를 표현하자면 아래와 같습니다.



**TCP**

<img src="https://madplay.github.io/img/post/2018-02-04-network-tcp-udp-tcpip-2.png" alt="img" style="zoom: 50%;" />



**UDP**

<img src="https://madplay.github.io/img/post/2018-02-04-network-tcp-udp-tcpip-3.png" alt="img"  />



두 그림을 보면 **신뢰성이 요구되는 애플리케이션에서는 TCP**를 사용하고 **간단한 데이터를 빠른 속도로 전송하고자하는 애플리케이션에서는 UDP**를 사용하는 것이 좋아보입니다.





## TCP(Transmission Control Protocol)

TCP는 네트워크 계층 중 전송 계층에서 사용하는 프로토콜로서, 장치들 사이에 논리적인 접속을 성립(establish)하기 위하여 연결을 설정하여 **신뢰성을 보장하는 연결형 서비스** 입니다. TCP는 네트워크에 연결된 컴퓨터에서 실행되는 프로그램 간에 **일련의 옥텟(데이터, 메세지, 세그먼트라는 블록 단위)를 안정적으로, 순서대로, 에러없이 교환**할 수 있게 합니다.

> **옥텟** : 8개의 비트가 한데 모인 것을 말합니다. 초기 컴퓨터들은 1 바이트가 꼭 8 비트만을 의미하지 않았으므로, 8 비트를 명확하게 정의하기 위해 **옥텟**이라는 용어가 필요했던 것입니다. 그러나 요즘에는 **바이트하고 같은 의미**가 되었다.



### 특징

- **연결형 서비스** : 연결형 서비스로 가상 회선 방식을 제공합니다.
  - 3-way handshaking 과정을 통해 연결을 설정
  - 4-way handshaking 을 통해 연결을 해제

> **가상 회선 방식** : 패킷이 지나갈 경로에 가상의 회선을 배정하는 방식입니다. 가상회선을 통과하는 패킷들은 모두 같은 경로를 거치므로 패킷의 전송 순서를 계속 유지하여 목적지에 도착하게 됩니다. 

- **흐름제어(Flow control)** : 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지합니다.
  - 송신하는 곳에서 감당이 안되게 많은 데이터를 빠르게 보내 수신하는 곳에서 문제가 일어나는 것을 방지
  - 수신자가 `윈도우크기(Window Size)` 값을 통해 수신량을 설정

- **혼잡제어(Congestion control)** : 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지합니다.
  - 정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는 것을 방지

- **신뢰성이 높은 전송(Reliable transmission)**
  - **Dupack-based retransmission** : 정상적인 상황에서는 ACK 값이 연속적으로 전송되어야 합니다. 그러나 ACK값이 중복으로 올 경우 패킷 이상을 감지하고 재전송을 요청합니다.
  - **Timeout-based retransmission** : 일정시간동안 ACK 값이 수신을 못할 경우 재전송을 요청합니다.

- **전이중, 점대점 방식**
  - **전이중 (Full-Duplex)** : 전송이 양방향으로 동시에 일어날 수 있습니다.
  - **점대점 (Point to Point)** : 각 연결이 정확히 2개의 종단점(소켓)을 가지고 있습니다.
  - 멀티캐스팅이나 브로드캐스팅을 지원하지 않습니다.

> **멀티 캐스팅** : UDP를 기반으로 하나 이상의 송신자들이 특정한 하나 이상의 수신자들에게 패킷을 전송하는 방식 (데이터를 수신 받기를 원하는 특정한 호스트들에게만 전송)
> **브로드 캐스팅** : UDP를 기반으로 자신의 호스트가 속해 있는 네트워크 전체를 대상으로 패킷을 전송하는 일대다 통신방식 (데이터를 수신할 필요가 없는 호스트들에게도 데이터가 전송)



#### TCP의 연결 및 해제 과정

![img](https://nesoy.github.io/assets/posts/20181010/2.png)

##### TCP Connection (3-way handshake)

1. 먼저 open()을 실행한 클라이언트가 `SYN`을 보내고 `SYN_SENT` 상태로 대기한다.
2. 서버는 `SYN_RCVD` 상태로 바꾸고 `SYN`과 응답 `ACK`를 보낸다.
3. `SYN`과 응답 `ACK`을 받은 클라이언트는 `ESTABLISHED(설정)` 상태로 변경하고 서버에게 응답 `ACK`를 보낸다.
4. 응답 `ACK`를 받은 서버는 `ESTABLISHED` 상태로 변경한다.



##### TCP Disconnection (4-way handshake)

1. 먼저 close()를 실행한 클라이언트가 FIN을 보내고 `FIN_WAIT1` 상태로 대기한다.
2. 서버는 `CLOSE_WAIT`으로 바꾸고 응답 ACK를 전달한다. 동시에 해당 포트에 연결되어 있는 어플리케이션에게 close()를 요청한다.
3. ACK를 받은 클라이언트는 상태를 `FIN_WAIT2`로 변경한다.
4. close() 요청을 받은 서버 어플리케이션은 종료 프로세스를 진행하고 `FIN`을 클라이언트에 보내 `LAST_ACK` 상태로 바꾼다.
5. FIN을 받은 클라이언트는 ACK를 서버에 다시 전송하고 `TIME_WAIT`으로 상태를 바꾼다. `TIME_WAIT`에서 일정 시간이 지나면 `CLOSED`된다. ACK를 받은 서버도 포트를 `CLOSED`로 닫는다.

> #### 주의
>
> - 반드시 서버만 `CLOSE_WAIT` 상태를 갖는 것은 아니다.
> - 서버가 먼저 종료하겠다고 `FIN`을 보낼 수 있고, 이런 경우 서버가 `FIN_WAIT1` 상태가 됩니다.
> - 누가 먼저 `close`를 요청하느냐에 따라 상태가 달라질 수 있다.



### TCP 헤더 정보

응용 계층으로부터 데이터를 받은 TCP는 `헤더`를 추가한 후에 이를 IP로 보냅니다. 헤더에는 아래 표와 같은 정보가 포함됩니다.



![img](https://blog.kakaocdn.net/dn/cIt86U/btqNiVx6GmY/nPEo5ZZsFq71gFGqAxtvxK/img.png)



| **필드**                      | **크기** | **내용**                                                     |
| ----------------------------- | -------- | ------------------------------------------------------------ |
| 송수신자의 포트 번호          | 16       | TCP로 연결되는 가상 회선 양단의 송수신 프로세스에 할당되는 포트 주소 |
| 시퀀스 번호 (Sequence Number) | 32       | 송신자가 지정하는 순서 번호, 전송되는 바이트 수를 기준으로 증가<br />SYN = 1 : 초기 시퀀스 번호. ACK 번호는 이 값에 1을 더한 값<br />SYN = 0 : 현재 세션의 이 세그먼트 데이터의 최초 바이트 값의 누적 시퀀스 번호 |
| 응답 번호 (ACK Number)        | 32       | 수신 프로세스가 제대로 수신한 바이트 수를 응답하기 위해 사용 |
| 데이터 오프셋 (Data Offset)   | 4        | TCP 세그먼트의 시작 위치를 기준으로 데이터의 시작 위치를 표현(TCP 헤더의 크기) |
| 예약 필드(Reserved)           | 6        | 사용을 하지 않지만 나중을 위한 예약 필드이며 0으로 채워져야 함 |
| 제어 비트(Flag Bit)           | 6        | SYN, ACK, FIN 등의 제어 번호                                 |
| 윈도우 크기(Window)           | 16       | 수신 윈도우의 버퍼 크기를 지정할 때 사용. 0이면 송신 프로세스의 전송 중지 |
| 체크섬(Checksum)              | 16       | TCP 세그먼트에 포함되는 프로토콜 헤더와 데이터에 대한 오류 검출 용도 |
| 긴급 위치(Urgent Pointer)     | 16       | 긴급 데이터를 처리하기 위함, URG 플래그 비트가 지정된 경우에만 유효 |

 

### TCP 제어비트 (Flag Bit) 정보

| 종류 | 내용                                                         |
| ---- | ------------------------------------------------------------ |
| ACK  | 응답 번호 필드가 유효한지 설정할때 사용하며 상대방으로부터 패킷을 받았다는 걸 알려주는 패킷. 클라이언트가 보낸 최초의 SYN 패킷 이후에 전송되는 모든 패킷은 이 플래그가 설정되어야 함 |
| SYN  | 연결 설정 요구. 동기화 시퀀스 번호. 양쪽이 보낸 최초의 패킷에만 이 플래그가 설정되어 있어야 한다. TCP에서 세션을 성립할 때 가장먼저 보내는 패킷, 시퀀스 번호를 임의적으로 설정하여 세션을 연결하는 데에 사용되며 초기에 시퀀스 번호를 보내게 된다. |
| PSH  | 수신 애플리케이션에 버퍼링된 데이터를 상위 계층에 즉시 전달할 때 사용 |
| RST  | 연결의 리셋이나 유효하지 않은 세그먼트에 대한 응답용으로 사용 |
| URG  | 긴급 위치를 필드가 유효한지 설정 (긴급한 데이터는 다른 데이터에 비해 우선순위가 높음) |
| FIN  | 세션 연결을 종료시킬 때 사용되며 더 이상 전송할 데이터가 없을 때 연결 종료 의사 표시 |



#### ACK 제어비트

- ACK는 송신측에 대하여 **수신측에서 긍정 응답**으로 보내지는 전송 제어용 비트
- ACK 번호를 사용하여 패킷이 도착했는지 확인합니다.
  - 송신한 패킷이 제대로 도착하지 않았으면 **재송신**을 요구합니다.

![img](https://image.slidesharecdn.com/tcp-150426214109-conversion-gate01/95/tcp-12-1024.jpg?cb=1430085440)





## UDP(User Datagram Protocol)

응용 계층으로부터 데이터 받은 UDP도 UDP 헤더를 추가한 후에 이를 IP로 보냅니다.

`UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)`는 **비연결형 프로토콜** 입니다. IP **데이터그램을 캡슐화하여 보내는 방법과 연결 설정을 하지 않고 보내는 방법을 제공**합니다. `UDP`는 **흐름제어, 오류제어 또는 손상된 세그먼트의 수신에 대한 재전송을 하지 않습니다.** 이 모두가 사용자 프로세스의 몫입니다. `UDP`가 행하는 것은 **포트들을 사용하여 IP 프로토콜에 인터페이스를 제공**하는 것입니다.

종종 클라이언트는 서버로 짧은 요청을 보내고, 짧은 응답을 기대합니다. 만약 요청 또는 응답이 손실된다면 클라이언트는 `time out` 되고 다시 시도할 수 있으면 됩니다. 코드가 간단할 뿐만 아니라 **TCP 처럼 초기설정`(initial setup)`에서 요구되는 프로토콜보다 적은 메시지가 요구**됩니다.

`UDP`를 사용한 것들에는 `DNS`가 있습니다. 어떤 **호스트 네임의 IP 주소를 찾을 필요가 있는 프로그램**은, `DNS` 서버로 호스트 네임을 포함한 UDP 패킷을 보냅니다. 이 **서버는 호스트의 IP 주소를 포함한 UDP 패킷으로 응답**합니다. 사전에 설정이 필요하지 않으며 그 후에 해제가 필요하지 않습니다.





## 표로 비교하는 TCP vs UDP

| TCP                                                          | UDP                                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Connection-oriented protocol (연결지향형 프로토콜)           | Connection-less protocol (비 연결지향형 프로토콜)            |
| Connection by **byte** stream (바이트 스트림을 통한 연결)    | Connection by **message** stream (메세지 스트림을 통한 연결) |
| Congestion / Flow control (혼잡제어, 흐름제어)               | NO Congestion / Flow control (혼잡제어와 흐름제어 지원 X)    |
| Ordered, Lower speed (순서 보장, 상대적으로 느림)            | Not ordered, Higer speed (순서 보장되지 않음, 상대적으로 빠름) |
| Reliable data transmission (신뢰성 있는 데이터 전송 - 안정적) | Unreliable data transmission (데이터 전송 보장 X)            |
| TCP packet : Segment (세그먼트 TCP 패킷)                     | UDP packet : Datagram (데이터그램 UDP 패킷)                  |
| HTTP, Email, File transfer 에서 사용                         | DNS, Broadcasting (도메인, 실시간 동영상 서비스에서 사용)    |



## 공통점

| TCP(Transfer Control Protocol) \| UDP(User Datagram Protocol) |
| ------------------------------------------------------------ |
| 포트 번호를 이용하여 주소를 지정                             |
| 데이터 오류 검사를 위한 체크섬 존재                          |

## 차이점

| TCP(Transfer Control Protocol)                     | UDP(User Datagram Protocol)                             |
| -------------------------------------------------- | ------------------------------------------------------- |
| 연결이 성공해야 통신 가능(연결형 프로토콜)         | 비연결형 프로토콜(연결 없이 통신이 가능)                |
| 데이터의 경계를 구분하지 않음(Byte-Stream Service) | 데이터의 경계를 구분함(Datagram Service)                |
| 신뢰성 있는 데이터 전송(데이터의 재전송 존재)      | 비신뢰성 있는 데이터 전송(데이터의 재전송 없음)         |
| 일 대 일(Unicast) 통신                             | 일 대 일, 일 대 다(Broadcast), 다 대 다(Multicast) 통신 |



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

