---
title: "Web : Swagger"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Web : Swagger"
toc: true
toc_sticky: true
toc_label: 목차
---

# 스웨거(Swagger)

스웨거는 `Web API` 문서화를 위한 도구입니다. 스웨거 홈페이지([https://swagger.io](https://swagger.io))에서는 스웨거를 `OAS(Open API Specification)`이라고 소개하고 있습니다. 말그대로 **API들이 가지는 명세(Spec)을 관리하기 위한 프로젝트**라고 말할 수 있습니다. `Web API`를 수동으로 문서화 하는 것은 굉장히 힘든 작업입니다. `Web API`의 스펙이 변경되었을 때 문서 역시 변경이 되야 하는데 이를 유지하는 것이 쉽지가 않습니다. `Swagger`를 사용하면 `Web API`가 수정되더라도 상관 없습니다. 문서가 자동으로 갱신이 되기 때문입니다.



## 스웨거의 기능

스웨거 홈페이지를 가보면 아래와 같은 기능이 있습니다.

1. **API Design**

2. **API Development**

3. **API Documentation**

4. **API Testing**

5. **API Mocking and Virtualization**

6. **API Governance**

7. **API Monitoring**

8. **OpenAPI & Swagger**

`Web API`를 만드는 개발자와 `Web API`를 이용하는 개발자가 있다고 생각해 보겠습니다. `Web API`를 이용하는 개발자는 `Web API`가 만들어질 때까지 기다린다면 작업이 상당히 느려질 수 있을 것입니다. `Web API`를 만드는 사람과 `Web API`를 사용하는 사람 간에 미리 명세를 정의하고 공유할 수 있다면 개발이 상당히 편리해 질 것입니다. 지금 이야기 한 것들을 편하게 해주는 도구 중에 하나가 `스웨거`라고 말할 수 있습니다.



## 스웨거 허브를 이용하여 API를 명세화 하고 테스트하기

**스웨거 허브 사이트를 이용하면 `Web API`를 만들지 않더라도 `Web API`를 명세화**할 수 있습니다. 또한, `Web API`를 명세화만 하는게 아니라 간단히 테스트도 할 수 있다는 장점을 가지고 있습니다.



[![img](https://cphinf.pstatic.net/mooc/20200211_84/1581406321817kgh4X_PNG/0.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

[https://swagger.io](https://swagger.io) 사이트의 `Why Swagger`메뉴를 선택하고 `API Design`을 클릭합니다.

---

[![img](https://cphinf.pstatic.net/mooc/20200211_15/1581406342360PbUlX_PNG/1.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

API Design에 대한 설명이 나옵니다. `Design APIs in SwaggerHub` 버튼을 클릭합니다.

---

[![img](https://cphinf.pstatic.net/mooc/20200211_151/1581406358328hrley_PNG/2.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

[![img](https://cphinf.pstatic.net/mooc/20200211_134/15814063859880ffmC_PNG/5.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

Swagger Hub를 사용하려면 먼저 회원가입을 해야 합니다. `Github` 계정을 이용해 회원가입을 진행하도록 하겠습니다. `Github`에 로그인되어있다면 위와 같은 화면이 뜹니다. `Authorize SmartBear` 버튼을 클릭합니다.

---

[![img](https://cphinf.pstatic.net/mooc/20200211_163/1581406453530Ov7Ch_PNG/6.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

사용할 조직정보를 입력합니다. 사실, 조직정보를 입력하지 않아도 사용가능 합니다. 팀으로 이용하려면 조직정보를 입력하고 `Continue to my account`를 클릭합니다.

---

[![img](https://cphinf.pstatic.net/mooc/20200211_245/1581406466399u9NwS_PNG/7.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

함께 사용할 사용자의 email을 입력합니다. 함께 사용할 사용자가 없다면, `Continue My Account`를 클릭합니다.

---

[![img](https://cphinf.pstatic.net/mooc/20200211_136/1581406486221NXQ5a_PNG/8.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

새로운 API를 등록하기 위해 `CREATE API`를 클릭합니다.

---

[![img](https://cphinf.pstatic.net/mooc/20200211_128/1581406504634xl9vt_PNG/9.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

위와 같은 창이 뜨면 `Template`에는 `Simple API`를 선택하고, `Name`엔 `calculator`를 선택합니다. 위에서 조직을 생성했다면 `Owner`에는 조직이름을 입력하고 조직을 선택하지 않았다면 본인 아이디를 선택합니다. `Visibility`는 `Public`을 선택합니다. 여기까지 입력했다면 `CREATE API`를 클릭합니다. 

---

[![img](https://cphinf.pstatic.net/mooc/20200211_38/1581406534147C2COB_PNG/10.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

```yml
paths:
  /plus:
    post:
      tags:
      - developers
      summary: plus value
      operationId: plus
      parameters:
      - in : query
        name: value1
        type: integer
      - in : query
        name: value2
        type: integer
      responses:
        200:
          description: plus result
          schema:
            $ref: '#/definitions/CalResult'
        400:
          description: bad input parameter

definitions:
  CalResult:
    type: object
    properties:
      value1:
        type: integer
        example: 5
      value2:
        type: integer
        example: 10
      operation:
        type: string
        example: +
      result:
        type: integer
        example: 15
```

좌측 메뉴중에서 `Code Editor`를 클릭합니다. `paths` 부분에 위와 같은 코드를 입력합니다. `paths`항목에는 `Web API` 명세 정보를 입력합니다. `schema`부분에는 응답에서 사용할 정보를 적습니다. 아래쪽에 `definitions/CalResult`에 설정된 정보를 사용하라는 의미입니다.  아래부분의 `definitions` 항목에 다음과 같은 내용을 추가합니다. `CalResult`는 응답 메시지의 형태를 설정합니다.

[![img](https://cphinf.pstatic.net/mooc/20200211_79/1581406695283zWcW6_PNG/11.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

위와 같이 설정을 하고, 코드에 문제가 없다면 우측에 아래와 같은 화면이 보여집니다.

---

[![img](https://cphinf.pstatic.net/mooc/20200211_103/1581406716231Wbz4z_PNG/12.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

`examples`로 주어진 값이 설정되어 있는 것을 확인할 수 있습니다. `Execute`버튼을 클릭해보도록 하겠습니다.

[![img](https://cphinf.pstatic.net/mooc/20200211_107/1581406740372CcYhg_PNG/13.png?type=w760)](https://www.boostcourse.org/web326/lecture/58988?isDesc=false#)

`Execute`버튼을 클릭하면 위와 같은 형태의 결과가 출력되는 것을 알 수 있습니다.  `Swagger Hub`를 이용해 간단히 덧셈 기능을 제공하는 `Web API`를 정의해 보았습니다. `Web API` 제작자는 이렇게 정의한 명세대로 개발을 해야 하고, `Web API` 사용자는 위와 같은 명세가 있을 거라 예상하고 개발할 수 있게 됩니다.



---

참고 : [https://www.boostcourse.org/web326/lecture/58988/?isDesc=false](https://www.boostcourse.org/web326/lecture/58988/?isDesc=false)