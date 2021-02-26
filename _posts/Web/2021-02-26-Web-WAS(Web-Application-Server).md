---
title: "Web : WAS(Web Application Server)"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Web : WAS(Web Application Server)"
toc: true
toc_sticky: true
toc_label: 목차
---

# WAS (Web Application Server)

WAS를 이해하기 위해서 클라이언트/서버 구조 부터 살펴보겠습니다!



### 클라이언트/서버 구조

클라이언트(Client)는 서비스(Service)를 제공하는 서버(Server)에게 정보를 요청하여 응답 받은 결과를 사용합니다.

[![img](https://cphinf.pstatic.net/mooc/20180213_10/151849899068982T3i_PNG/05.png?type=w760)](https://www.boostcourse.org/web326/lecture/58947/?isDesc=false#)



### DBMS (DataBase Management System)

다수의 사용자가 데이터베이스 내의 데이터에 접근할 수 있도록 해주는 소프트웨어입니다.

[![img](https://cphinf.pstatic.net/mooc/20180122_74/15166087526093WS9P_PNG/1_1_7_DBMS.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/58947/?isDesc=false#)

DMBS가 등장하기 이전에는 개발자들이 파일의 데이터를 저장하고 읽어들이는 등의 기능을 모두 구현해야 했습니다. 이러한 불편함을 해결하기 위한 여러 가지 노력의 결과로 DMBS라는 소프트웨어가 등장하게 되었습니다.

DBMS에 대한 최초의 개념은 IBM에서 논문으로 나왔고 최초의 구현은 Oracle에서 하였습니다.



### 미들웨어 (MiddleWare)

미들웨어는 비즈니스 로직을 클라이언트와 DBMS사이의 미들웨어 서버에서 동작하도록 함으로써 클라이언트는 입력과 출력만 담당하도록 합니다.

[![img](https://cphinf.pstatic.net/mooc/20180122_267/1516608805247GN2hK_PNG/1_1_7_.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/58947/?isDesc=false#)

DBMS는 보통 서버 형태로 서비스를 제공하기 때문에 이러한 DBMS에 접속해서 동작하는 클라이언트 프로그램이 한 때 많이 만들어졌습니다. 그런데 프로그램 로직이 변경되면 클라이언트가 매번 배포되어야 한다는 문제가 있었고 대부분의 로직이 클라이언트에 포함되기 때문에 보안이 나쁘다는 문제가 있었습니다. 그래서 등장한 것이 미들웨어입니다.



## WAS (Web Application Server)

WAS는 일종의 미들웨어로 웹 클라이언트(보통 웹 브라우저)의 요청 중 웹 애플리케이션이 동작하도록 지원하는 목적을 가집니다.

[![img](https://cphinf.pstatic.net/mooc/20180122_270/1516606715302CWRJG_PNG/1_1_7_was.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/58947/?isDesc=false#)

최초의 웹이 등장했을 때는 웹 브라우저는 정적인 데이터만 보여주었습니다. 하지만 웹이 널리 사용되면서 사용자들의 요구사항이 점점 더 복잡한 프로그래밍적인 기능을 요구하게 되었습니다. 보통 이러한 기능들은 DBMS와 연관된 경우가 굉장이 많았기 때문에 브라우저를 클라이언트라고 본다면 브라우저와 DMBS 사이에서 동작하는 미들웨어가 필요하게 되었습니다. 이러한 미들웨어를 WAS라고 합니다.



### WAS가 가지는 중요한 기본 기능 3가지

1. 프로그램 실행 환경과 데이터베이스 접속 기능을 제공
2. 여러 개의 트랜잭션을 관리
3. 업무를 처리하는 비즈니스 로직을 수행



WAS는 다양한 기능을 제공하는데, 웹 서버의 기능도 기본적으로 제공합니다. 보통 톰캣이라는 WAS만 하나 설치하고 이용이 가능한 이유는 톰캣이 가지고 있는 웹 서버가 충분한 기능을 하고 있기 때문에 따로 Apache같은 웹 서버를 따로 설치하지 않는 것입니다.

하지만 현업에서는 다음과 같은 방식으로 사용하고 있다고 합니다.

![웹서버,WAS](https://user-images.githubusercontent.com/79291114/109315776-3d0ec780-788e-11eb-855d-de3c4f389609.jpg)



#### 웹 서버 vs WAS

- 웹 서버는 보통 정적인 콘텐츠를 웹 브라우저에게 전송
- WAS는 보통 프로그램의 동적인 결과를 웹 브라우저에게 전송

- WAS도 보통 자체적으로 웹 서버 기능을 내장하고 있습니다.
- 현재는 WAS가 가지고 있는 웹 서버도 정적인 콘텐츠를 처리하는 데 있어서 성능상 큰 차이가 없습니다.
- 규모가 커질수록 웹 서버와 WAS를 분리합니다.
- 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성을 위해 웹서버와 WAS를 대체로 분리합니다.



##### 장애극복?

WAS에서 동작하도록 개발자가 만든 프로그램이 오작동이 발생해서 WAS 자체에 문제가 발생해 WAS를 재시작해야 되는 경우, 문제가 있는 WAS를 재시작할 때, 앞단의 웹 서버에서 먼저 해당 WAS를 이용하지 못하도록 하고 WAS를 재시작한다면 사용자는 WAS의 문제가 발생하였는지 모르고 이용할 수 있습니다. 

대용량 웹 어플리케이션을 무중단으로 운영하기 위해서 상당히 중요한 기능이라고 합니다. 이러한 기능 때문에 보통 웹 서버가 WAS 앞단에 동작하도록 하는 경우가 많습니다.



---

https://www.boostcourse.org/web326/lecture/58947/?isDesc=false

