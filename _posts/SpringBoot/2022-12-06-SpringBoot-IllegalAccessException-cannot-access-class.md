---
title: "Spring Boot : "IllegalAccessException : cannot access class ..." 에러 해결 방법(Intellij)"
excerpt_separator: <!--more-->
categories:
  - "Spring Boot"
tags:
  - "Spring Boot" 
  - "Spring Boot : IllegalAccessException"
  - "Spring Boot : cannot access class"
toc: true
toc_sticky: true
toc_label: 목차
---

# "IllegalAccessException : cannot access class ..." 에러 해결 방법(Intellij)

다양한 라이브러리를 사용하다 보면  `IllegalAccessException : cannot access class ... ` 에러를 마주칠 때가 있습니다.

해당 글은 위 에러를 해결하는 방법에 대하여 기재한 글입니다. (저는 Intellij를 사용했기 때문에 Intellij에서의 해결 방법입니다. 다른 IDE는 비슷한 옵션을,,,)

> 저의 경우에는 Nice 본인인증 라이브러리를 사용하다 발생하였습니다.





## 원인

원인을 간략하게 말하자면, JDK 9 이후의 릴리즈에서 **sun.misc.Unsafe와 같은 중요한 내부 API를 제외하고 기본적으로 JDK의 모든 내부 요소를 강력하게 캡슐화**했기 때문입니다.하지만 이전 버전을 지원하기 위하여 JDK 8에 존재하는 패키지(`java.*`, `sun.*` 등)에 대해 사용자가 캡슐화를 허용할지 말지 선택할 수 있습니다.

대략적으로 읽어봤을 때, 이유는 다른 오픈 라이브러리에서 내부 API를 사용하여 결합도가 높아져 유지보수가 힘들다는 내용인 것 같았습니다. (정확한 내용은 아래 링크를 참고하시면 좋을 것 같습니다.)

- 참고 링크 : https://openjdk.org/jeps/396





## 해결 방법

여러가지의 해결방법이 있는데, 상황에 따라 여러가지를 동시에 적용해야 할 수도 있습니다.



### 1. illegal-access 허용

해당 옵션을 사용하여 위에서 말했던 것 처럼 이전 버전을 지원하기 위하여 JDK 8에 존재하는 패키지(`java.*`, `sun.*` 등)에 대해 사용자가 캡슐화를 허용할지 말지 선택할 수 있습니다.

Intellij에서 `File -> Settings -> Compiler -> Java Compiler`의 `Additional command line parameters`나 `Override compiler parameters per-module -> Compilation options` 에 아래 옵션 중 하나를 추가해주면됩니다.

- **--illegal-access=permit(JDK 9 이후 디폴트)** : JDK 8에 존재하는 모든 패키지가 이름이 지정되지 않은 모듈의 코드에 개방되도록 설정
- **--illegal-access=warn** : `permit과 동일`하지만 `경고 메세지`가 발생
- **--illegal-access=debug** : `permit과 동일`하지만 `경고 메세지와 스택 추적`이 발생
- **--illegal-access=deny** : 다른 명령줄 옵션(--add-opens 등)으로 활성화된 작업을 제외한 모든 액세스 작업을 비활성화



### 2. --add-exports 사용

기본적으로 액세스할 수 없는 내부 API를 사용해야 하는 경우 --add-exports 명령줄 옵션을 사용하여 캡슐화를 해제할 수 있습니다.

Intellij에서 `Run -> Edit Configurations -> Vm options(없다면 상단의 Modify options에서 추가)` 에 아래 옵션을 사용할 패키지에 따라 추가해주면 됩니다.

- **--add-exports=java.base/com.sun.crypto.provider=ALL-UNNAMED**





## 마치며...

생각보다 간단한 것을 여기 저기 검색하느라 꽤나 시간을 사용하였는데, 다른 분들은 이 글을 보고 시간을 단축시켰으면 합니다.





**참고 링크**

----

- https://intellij-support.jetbrains.com/hc/en-us/community/posts/5153987456018-Java-17-cannot-access-class-sun-security-pkcs-PKCS7
- https://openjdk.org/jeps/396
- https://i.imgur.com/j4v0Lcn.jpg
- https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-2C896CA8-927C-4381-A737-B1D81D964B7B

