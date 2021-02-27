---
title: "Cmder : Cmder 시작 경로 바꾸기"
excerpt_separator: <!--more-->
categories:
  - Cmder
tags:
  - Cmder
toc: true
toc_sticky: true
toc_label: 목차
---

# Cmder 시작 경로 바꾸기

cmder을 사용하여 Git을 사용하는데 시작 위치가 cmder 설치위치여서 실행할 때 마다 `cd`로 위치바꿔주는게 여간 귀찮은게 아니어서 시작경로 바꾸는 방법을 찾아보았습니다!



1. 시작을 눌러 cmder을 검색합니다.
2. `cmder - 바로가기`파일을 오른쪽마우스버튼으로 누른 후 해당 파일이 존재하는 폴더로 이동합니다.

<img src="https://user-images.githubusercontent.com/79291114/109373957-467f4a80-78f5-11eb-9b5a-43ebe4f01e09.jpg" alt="cmder바로가기" style="zoom: 80%;" />

3. `cmder - 바로가기` 파일을 마우스 오른쪽 버튼을 이용해 속성창을 열어줍니다.

4. `바로가기` 탭으로 이동해 `대상(T)` 부분에 나와있는 경로 뒤쪽에 `/start` 와 함께 자신이 시작하고 싶은 경로를 적어 줍니다.

   `ex) /start C:\Users\1\projects\git\dev-splin`

![cmder속성창](https://user-images.githubusercontent.com/79291114/109373953-454e1d80-78f5-11eb-986e-88a33b874503.jpg)

5. 짠! 시작경로가 바뀐 것을 볼 수 있습니다!

![cmder시작경로바꾸기결과](https://user-images.githubusercontent.com/79291114/109374040-e6d56f00-78f5-11eb-903e-8794ceaf5c8c.jpg)



## 그 외의 다양한 옵션들

이런 속성창에서 사용할 수 있는 다양한 옵션들이 있는데요. 한번 알아보겠습니다!

![cmder다양한옵션](https://user-images.githubusercontent.com/79291114/109374096-33b94580-78f6-11eb-8453-0fc28e6ab264.jpg)

- /c : 개별 사용자 cmder 루트 폴더를 설정합니다. `ex %userprofile%\cmder_config`
- /m : ConEmu 설정 저장소 `user_conemu.xml` 대신 `conemu-%computername%.xml`를 사용합니다.
- /register [ALL, USER] : Windows 쉘 메뉴 바로가기를 등록합니다.
- /unregister [ALL, USER] : Windows 쉘 메뉴 바로가기를 등록을 취소합니다.
- /single : single 모드로 cmder을 시작합니다.
- /task [task_name] : 프로그램 실행 후 해당작업을 실행합니다.
- /x [ConEmu extras pars] : 매개변수를 ConEmu에 전달합니다. 

---

참고 : https://github.com/cmderdev/cmder/blob/master/README.md

https://qyutdyutt.tistory.com/entry/%EC%9C%88%EB%8F%84%EC%9A%B0-cmd%EB%AA%85%EB%A0%B9%ED%94%84%EB%A1%AC%ED%94%84%ED%8A%B8-%EA%B8%B0%EB%B3%B8-%EA%B2%BD%EB%A1%9C%EB%94%94%EB%A0%89%ED%86%A0%EB%A6%AC-%EB%B0%94%EA%BE%B8%EA%B8%B0-%EC%8B%9C%EC%9E%91-%EC%9C%84%EC%B9%98-%EB%B0%94%EA%BE%B8%EA%B8%B0

