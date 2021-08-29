---
title: "Network : OSI 1계층(물리 계층)"
excerpt_separator: <!--more-->
categories:
  - CS(Computer Science)
  - Network
tags:
  - CS(Computer Science)
  - Network
  - "Network : OSI Layer1 - Physical Layer"
toc: true
toc_sticky: true
toc_label: 목차
---

# 물리 계층(Physical Layer)

물리계층은 OSI 참조 모델 하위 Layer 1계층이며, 통신하는 네트워크 장비로 **데이터를 전기 신호로 출력하는 일과 통신하는 네트워크 장비 사이의 물리적 링크 연결과 링크 활성화 및 비활성화를 담당**합니다.

- **Encoding** : 컴퓨터에서 만든 데이터를 다른 장비로 전달하기 위해서 전기적 신호로 변환하는 것
- **Decoding** : 전기적 신호를 받은 상대방 장비에서 신호를 데이터화 하기 위해 변환하는 것
- **Codec** : Encoding / Decoding 하는 장치를 말함





## 장비

물리 계층에서 사용하는 장비와 추가로 연관된 장비까지 알아보겠습니다.



### 리피터(Repeater)

`리피터(repeater)`는 구부러진 **전기 신호를 복원하고 증폭하는 기능을 가진 네트워크 중계장비**입니다. 통신하는 상대방이 멀리 있을 경우 리피터를 사이에 둠으로써 통신 거리를 연장할 수 있습니다. 하지만 요즘은 다른 네트워크 장비가 리피터 기능을 지원하기 때문에 리피터를 쓸 필요가 없어졌습니다.

<img src="https://user-images.githubusercontent.com/79291114/131204510-9c506d9d-3331-4a9a-883b-68834eb409a7.png" alt="repeater" style="zoom: 67%;" />



### 허브(Hub)

`허브(hub)`는 리피터 외에도 물리 계층에서 동작하는 네트워크 장비입니다. 허브는 **컴퓨터 여러 대를 서로 연결하는 장치**이기도 하며, 리피터와 마찬가지로 **전기신호를 증폭하는 기능**을 가지고 있습니다. 

허브의 **장점은 직접 컴퓨터끼리 연결하지 않아도, 여러 대가 데이터를 주고 받는 다는 점**입니다. 하지만 **데이터를 전송할때, 특정 포트로 데이터전송이 되지않고, 연결되어 있는 모든 포트(컴퓨터)에 데이터가 전송되는 단점**을 가지고 있습니다. (불필요한 데이터도 전송이 됩니다.)

허브는 스스로 데이터를 받는 걸 판단할 수 없기 때문에 `더미 허브`라고도 불리웁니다.

<img src="https://user-images.githubusercontent.com/79291114/131204508-d57dce2c-e312-422a-a06c-f122609e9fe8.png" alt="hub" style="zoom: 50%;" />

리피터와 허브를 이용해 컴퓨터 여러대가 데이터를 보내면 데이터들이 서로 부딪히는데, 이를 `충돌(Collision)`이라고 합니다.

이런 충돌을 방지하기 위하여 여러 컴퓨터가 동시에 데이터를 전송해도 충돌이 일어나지 않는 구조로 되어있는 `이더넷(Ethernet)`의  `CSMA/CD`을 사용했는데, 효율이 좋지 않아 현재는 사용하지 않습니다.

현재는 데이터 충돌을 방지하기 위하여 `스위치(Switch)`를 사용합니다.

> **CSMA/CD** : 이더넷이 충돌을 방지하기 위하여 데이터를 보내는 시점을 늦추는 것



### 스위치(Switch)

`스위치(Switch)`는 전이중 통신방식, 즉, **충돌이 일어나지 않는 구조**로 되어 있기 때문에 효율이 높은 장비입니다. 사용 목적은 허브와 유사하지만, 훨씬 더 향상된 속도를 제공합니다. 하지만 허브와 다른점은 허브처럼 다른 모든 컴퓨터에 데이터를 전송하는게 아닌, 데이터를 필요를 하는 컴퓨터에만 전송됩니다. 

> **전이중 통신(Full Duplex)** : 두 대의 단말기가 데이터를 송수신하기 위해 각각 독립된 회선(송신을 위한 회선, 수신을 위한 회선)을 사용하는 통신 방식 

<img src="https://user-images.githubusercontent.com/79291114/131204512-38e2fd19-47c0-4dd8-8ec4-79b761aea6fa.jpg" alt="switch" style="zoom:50%;" />

**충돌(Collision)이 발생할 때 그 영향이 미치는 범위를 충돌 도메인**이라고 합니다. 

**만약 충돌이 일어난다면, 허브는 그 영향을 연결되어 있는 모든 컴퓨터에게 영향**을 미칩니다. 하지만 **스위치는 접속되어 있는 모든 컴퓨터에 영향을 미치지 않습니다.** (충돌 도메인의 범위가 넓을 수록 네트워크는 지연됩니다.) 

네트워크를 지연시키지 않기 위해서는 충돌 도메인의 범위는 좁히는 것이 매우 중요합니다.



### 라우터(Router)

네트워크 계층에는 IP라는 프로토콜이 있는데, **IP에 헤더를 붙여 데이터를 다른 네트워크 목적지에 보낼지 결정하는 것을 라우팅(rounting)**이라 하며, **라우팅은 `라우터(Router)` 라는 장비와 L3(Layer3 Switch) 가 라우팅**을 합니다. 즉 서로 다른 네트워크 간에 통신을 하려면 라우터가 필요한 것입니다.

라우터를 쉽게 설명하자면 밑에 그림과 같이 연결된 스위치 혹은 네트워크들을 **분리**할 수 있습니다.

<img src="https://user-images.githubusercontent.com/79291114/131204511-09d8a70b-badb-4b3b-bc72-f259d05c2138.jpg" alt="router" style="zoom:50%;" />

**물리계층과 데이터링크 계층으로 이루어지는 이더넷의 범위에 있는 독립적인 네트워크들을 네트워크 계층의 기능으로 분리하며 중계하는것이 라우터**입니다.

라우터의 기능은 가장 신속하고 효율적인 경로로 배정해주고 제어하는 일을하며, 위에 설명했듯이 독립된 각 네트워크를 연결 시켜주고 최적의 경로 설정 기능과 패킷스위치 기능 등 하나의 데이터가 망가졌을때 다른 길로 전송될 수 있도록 구성시켜주는 일을 합니다.  

- **게이트웨이** : 다른 네트워크로 데이터를 전송하기 위해 설정해야하는 것

- **라우팅 테이블** : 경로 정보가 등록되어 있는 테이블

- **라우팅 프로토콜** : 라우팅 정보를 서로 교환하기 위한 프로토콜





## 케이블

전기신호로 변경된 데이터를 전달하기 위해서는 케이블이 필요합니다.



### 랜선(Twisted Pair)

흔히 랜선이라고 불리우는 이 케이블은 **LAN 구간을 연결하는 케이블 중에 가장 보편적으로 사용하는 케이블**입니다. **총 8가닥의 얇은 구리선으로 구성되어 있으며, 두 가닥씩 쌍을 이루어 꼬여있는 모습**을 본따서 이름을 지정하였습니다.

<img src="https://user-images.githubusercontent.com/79291114/131236123-ae9dc2fa-84f7-4a78-a2f6-72f6a11ac607.png" alt="utp" style="zoom:80%;" />

<img src="https://user-images.githubusercontent.com/79291114/131236126-e513ed2b-ee50-4656-90b4-56d8a219965d.png" alt="stp" style="zoom:80%;" />

![ftp](https://user-images.githubusercontent.com/79291114/131236125-e945bdfd-b1ec-47da-b76b-f3abd9e294ca.jpg)

보호막이 없는 `Unshielded Twisted Pair(UTP)`, 꼬여있는 선을 또 다른 절연체로 감싸서 외부의 간섭을 덜 받도록 한 `Shielded Twisted Pair(STP)`, 보호막 처리는 되어있지 않고, 알루미늄 은박이 4가닥의 선을 감싸고 있는 `Foil Screened Twisted Pair(FTP)`이 있습니다. 짧은 거리에 있는 장비를 연결하는 것이 대부분이기 때문에 주로 UTP를 많이 사용합니다.

*추가로 꼬여있는 선을 Shield로 감싸고 4가닥의 선을 한 번 더 감싸는 `S-FTP` 케이블도 있습니다.*

> **쉴드(Shield)** : 연선으로 된 케이블 겉에 외부 피복, 또는 차폐재가 추가함으로써 외부의 노이즈를 차단하거나 전기적 신호의 간섭을 대폭 줄여줍니다.



#### 종류

##### Category 5

![category5](https://user-images.githubusercontent.com/79291114/131236286-b5ec00e2-bc2c-494a-b2fc-93a23b88e64d.png)

- 별도의 차폐처리 없이 선의 꼬임만으로 외부 간섭 보호
- 최대 100Mbps 지원
- 가정/사무실용으로 적합



##### Category 5E

![category5E](https://user-images.githubusercontent.com/79291114/131236287-fe8b48af-0c8a-4366-ae23-82a2dda8a258.png)

- 현재 가장 많이 사용되는 규격
- 은박쉴드의 유무에 따라 UTP와 FTP로 나뉨
- 네트워크공사장비 대부분이 이 케이블을 사용



##### Category 6

![category6](https://user-images.githubusercontent.com/79291114/131236288-5747877c-fad9-42e9-82ef-e515f94fd7d9.png)

- 빠른 속도의 안정적인 데이터전송을 위해 쓰이는 규격
- 내부 십자형 개재가 각 페어간 간섭을 막아줌
- 가정용보다는 회사의 서버나 전산망, IT 계열 업체 등에 많이 활용



##### Category 7

![category7](https://user-images.githubusercontent.com/79291114/131236284-c8b7d5f4-2b49-4a8f-ad1a-123e4bd0b36f.png)

- S-FTP를 사용하여 외부 간섭을 완벽에 가깝게 차단
- 시공에 어려움이 많고 가격 또한 비싸서 사용이 거의 없음



#### 케이블 연결(Cable Connector)

![T568](https://user-images.githubusercontent.com/79291114/131236635-af1abb3a-0c6a-414e-ba0c-0caa53f7d51f.PNG)

UTP 케이블의 8가닥 중에서 1,2,3,6번을 100Mbps 구간을 연결할 때 사용하는데, 배열을 어떻게 했느냐에 따라 위와 같이 `T568A` 방식과 `T568B` 방식으로 구분됩니다.

> T568 케이블을 만들 때 T568A는 1번부터 녹주파갈, T568B는 주녹파갈 이라고 많이 외웁니다.



#### 





### 동축 케이블(Coaxial)

동축 케이블이라 불리는 이 케이블은 **하나의 구리선으로 통신되며 전기적 간섭을 덜 받도록 하기 위해 구리망으로 한 번 더 감싸고 있는 형태**를 취하고 아날로그와 디지털 신호 모두를 전송할 수 있는 매체입니다. 

![coaxial](https://user-images.githubusercontent.com/79291114/131235848-a6e3af08-cbff-4586-b232-57fd674bd4fd.jpg)

CATV에서는 아날로그 신호를, 근거리통신망에서는 주로 디지털 신호를 전달합니다. 동축케이블은 10 Mbps 이상의 정보 전송량을 갖는데, **중앙의 구리선에 흐르는 전기신호는 그것을 싸고 있는 외부 구리망 때문에 외부의 전기적 간섭을 적게 받고, 전력손실이 적어 고속 통신선로로 많이 이용**되고 있습니다. 또 동축케이블은 바다 밑이나 땅속에 묻어도 그 성능에 큰 지장이 없습니다. 값은 광섬유에 비해서는 싸지만, 전화선보다는 훨씬 비쌉니다.

> **CATV** : 도시 빌딩이나 산간벽지 등 텔레비전 난시청 지역에서, 전파를 각 가정으로 분배하기 위한 공동 청취 안테나 시설을 말합니다.(공동 시청 안테나 텔레비전) 오늘 날에는 다양한 영상통신시스템으로 발전하고 있습니다.



### 광 섬유 케이블(Fiber Optic)

광 케이블이라고 불리며 **빛에 데이터를 실을 수 있는 기술을 사용**하여 위 케이블 보다 훨씬 빠른 속도로 데이터를 전달할 수 있습니다. 광 케이블은 **광섬유의 한쪽 끝에서 전기신호를 따라 점멸하는 발광소자를 써서 빛을 점멸하면 광섬유의 다른 쪽 끝에서 수광소자를 써서 이 점멸하는 빛을 받을 수 있는 현상을 이용**한 것입니다. 

![fiber-optic](https://user-images.githubusercontent.com/79291114/131236628-a70baed4-7536-40a0-b254-3b19124b4079.PNG)

빛을 전달하는 **유리 코어의 크기가 작은 것을 Single Mode, 넓은 것을 Mutil Mode**라고 합니다.

`Single Mode`는 코어가 좁아서 데이터를 한 번에 실어 나를 수 없어 단일경로로 사용되지만, 빛이 거의 일직선으로 전달되어 Multi Mode보다 더 멀리 데이터 전달이 가능합니다.

`Multi Mode`는 코어가 넓어서 여러 데이터를 한 번에 실어나를 수 있어 다중경로로 사용합니다. 빛이 반사되는 각도를 조절하여 많은 데이터를 실을 수 있습니다.



#### 종류

광 케이블에 사용할 수 있는 커넥터는 여러 타입이 있습니다.

![fiber-optic-type](https://user-images.githubusercontent.com/79291114/131236626-f0e372da-bce3-4936-bbe6-7f1e2c83b335.PNG)

이 중에서 SC와 LC가 나오기 전에 많이 사용했던 커넥터는 ST 커넥터이고, 요즘 가장 많이 이용하고 있는 커넥터는 얇고 가볍다는 장점을 가지고 있는 LC 커넥터입니다.



---

참고 : [https://young-duck.tistory.com/15](https://young-duck.tistory.com/15)

[https://bignet.tistory.com/44](https://bignet.tistory.com/44)

[https://handreamnet.tistory.com/501](https://handreamnet.tistory.com/501)

[https://jhnyang.tistory.com/373](https://jhnyang.tistory.com/373)

[https://m.blog.naver.com/itu_itu/220085575495](https://m.blog.naver.com/itu_itu/220085575495)

[https://m.blog.naver.com/kwshop89/221382694617](https://m.blog.naver.com/kwshop89/221382694617)

[https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tear0128&logNo=50072252803](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tear0128&logNo=50072252803)

[https://ko.wikipedia.org/wiki/%EA%B4%91%EC%84%AC%EC%9C%A0](https://ko.wikipedia.org/wiki/%EA%B4%91%EC%84%AC%EC%9C%A0)

[https://m.blog.naver.com/tyghvm100/220249776870](https://m.blog.naver.com/tyghvm100/220249776870)
