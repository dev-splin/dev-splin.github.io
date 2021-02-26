---
title: "Web : "
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Web : Browser"
toc: true
toc_sticky: true
toc_label: 목차
---

# 브라우저

브라우저는 월드와이드웹(WWW)에서 정보를 검색, 표현하고 탐색하기 위한 소프트웨어입니다.

인터넷에서 특정 정보로 이동할 수 있는 주소 입력창이 있고 서버와 HTTP로 정보를 주고 받을 수 있는 네트워크 모듈도 포함하고 있습니다. 그리고 서버에서 받은 문서(HTML, CSS, Javascript)를 해석하고 실행하여 화면에 표현하기 위한 해석기(Parser)들을 가지고 있습니다.



## 사파리 브라우저 webkit렌더링엔진의 처리과정

[![img](https://cphinf.pstatic.net/mooc/20171231_32/1514692895834EoHUo_PNG/webkitflow.png?type=w760)](https://www.boostcourse.org/web326/lecture/258496/?isDesc=false#)



브라우저마다 서로 다른 엔진을 포함하고 있습니다. 여기서는 사파리 브라우저에서 처리되는 webkit렌더링엔진의 처리과정을 살펴보겠습니다.

1. HTML을 해석해서 DOM Tree를 만들고, CSS를 해석해서 역시 CSS Tree(CSS Object Model)을 만듭니다. 
   - 이 과정에서 Parsing 과정이 필요하며 토큰 단위로 해석되는 방식은 일반적인 소스코드의 컴파일 과정이라고 보시면 됩니다.
2. DOM Tree와 CSS Tree, 이 두 개는 연관되어 있으므로 Render Tree로 다시 조합됩니다.
   - 이렇게 조합된 결과는 화면에 어떻게 배치할지 크기와 위치 정보를 담고 있습니다.

3. 이후에 이렇게 구성된 Render Tree정보를 통해서 화면에 어떤 부분에 어떻게 색칠을 할지 Painting과정을 거치게 됩니다.



---

참고 : https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/

https://www.boostcourse.org/web326/lecture/258496/?isDesc=false