---
title: "Clean Code : 16장(SerialDate 리팩터링)"
excerpt_separator: <!--more-->
categories:
  - Clean Code
tags:
  - Clean Code
  - "Clean Code : Chapter 16"
toc: true
toc_sticky: true
toc_label: 목차 
---

# 16장. SerialDate 리팩터링

자바는 이미 `java.util.Date, java.util.Calendar` 등과 같은 클래스를 제공하는데, 왜 `SerialDate` 가 필요할까요?

`SerialDate`는 `java.util.Date`는 시간이 너무 정밀하기 때문에 특정 날짜(2022년 2월 20일)만을 표현하고 싶을 때 사용하기 위하여 만들었다고 합니다.

리팩토링 과정을 전부 설명하지 않고 저자가 진행하는 **리팩토링 과정에서 제가 중요하다고 생각되는 부분을 위주로 정리**하였습니다.





## 16-1. 첫째, 돌려보자

특정 라이브러리나 클래스를 리팩토링 한다면, 해당 클래스에 대한 이해가 필요합니다.

때문에 먼저, 리팩토링할 대상인 **SerialDate의 단위 테스트**를 진행합니다. 단위 테스트를 진행하면서, 해당 메서드나 멤버 변수 등의 의미를 파악하고 네이밍과 기능이 일치하는지 확인하면서 클래스를 이해하는 과정이 필요합니다.

그 후, 리팩토링 할 부분을 선정하게 됩니다.





## 16-2. 둘째, 고쳐보자

수정하는 과정 중에서도 **단위테스트는 필수** 입니다. 클린 코드답게 여러가지 방법으로 리팩토링하게 되는데, 간략하게 요약하자면 아래와 같습니다.

- 클래스의 책임에 맞는 네이밍 구성
- 자료 구조나 키워드, 상속, 위임을 알맞게 구성
- 클래스의 책임에 알맞지 않은 기능이나 변수,상수를 분리 및 이동
  - 플래그로 메서드 기능을 제어하는 메서드 분리
- 중복된 메서드 제거 및 결합
- 필요하지 않거나 의미없는 기능, 변수, 상수, 키워드 제거
- 잘못된 주석 수정 및 제거
- 연관 클래스 간의 종속성을 파악해 종속성을 제거를 위해 클래스를 분리하고 추상화
- 복잡한 알고리즘은 가독성을 위해 임시 변수를 통해 네이밍 구성
- 테스트 케이스에서 사용하는 메서드를 제거

해당 글에서 종속성제거와 추상화를 위해 다양한 디자인 패턴을 사용하였습니다. 해당 내용은 아래의 링크를 참고하면 좋을 것 같습니다.

위와 같이 계속해서 리팩토링하게 되면 또 다른 문제를 발견할 수 있습니다. 이 때, 그냥 지나치지 않고 보이스카우트 규칙을 따라 발견된 문제들도 같이 해결하게 되면 조금 더 깨끗하고 명확한 코드를 작성할 수 있습니다.



**참고 링크**

- [싱글톤(Singleton) 패턴 ](https://github.com/Nelmm/DesignPattern/tree/main/%EA%B0%9D%EC%B2%B4%EC%83%9D%EC%84%B1/1%EC%A3%BC%EC%B0%A8-%EC%8B%B1%EA%B8%80%ED%86%A4) : 인스턴스를 오직 한 개만 만들어서 제공하는 클래스가 필요한 경우에 사용하는 패턴
- [추상 팩토리(Abstract Factory) 패턴](https://github.com/Nelmm/DesignPattern/tree/main/%EA%B0%9D%EC%B2%B4%EC%83%9D%EC%84%B1/2%EC%A3%BC%EC%B0%A8-%EC%B6%94%EC%83%81%ED%8C%A9%ED%86%A0%EB%A6%AC) : 관련있는 여러개의 인스턴스 생성 방법을 인터페이스로 분리하여 생성하는 패턴
- [데코레이터(Decorator) 패턴](https://github.com/Nelmm/DesignPattern/tree/main/%EA%B5%AC%EC%A1%B0/5%EC%A3%BC%EC%B0%A8-%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0) : 기존 코드를 변경하지 않고 부가 기능을 동적으로(유연하게) 추가하는 패턴





## 16-3. 16장을 마치며...

저자는 어느정도 자신의 주관으로 리팩토링을 진행하였습니다. (실제로 다른 개발자들의 대부분이 반대하는 부분도 있었음)

때문에 우리도 무조건적으로 신뢰하는 것 보다는 적당히 비판적인 시선으로 글을 읽을 필요가 있겠습니다.



---

참고 : Clean Code - 로버트 C. 마틴
