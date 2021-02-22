---
title: "Git : Remote"
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
  - "Git : Remote"
toc: true
toc_sticky: true
toc_label: 목차
---

# Remote

![remote](C:\Users\1\Desktop\Study\사진\remote.jpg)

- 나만의 PC에만 git repository를 만들고 간직한다면 내 PC에 문제가 생기거나 다른 환경에서 작업하고 싶을 때 repository에 접근이 불가능합니다.

- Remote라는 server를 이용해 나의 git repository를 업로드해두면, 다른 PC에서도 접근 할 수 있고 다른 개발자들과 함께 일을 할 수 있습니다.

- clone

  ​	server에 이미 존재하고 있는 git repository를 가지고 올 때 사용하는 명령어

- push

  ​	 local에서 commit한 내용을 server에 올려주고 싶을 때 사용하는 명령어

- pull

  ​	server에 업데이트 된 내용을 local에 업데이트 하고 싶을 때 사용하는 명령어

  ​	<u>server에서 업데이트된 내용이 local에서 작업하고 있는 파일과 동일하면 merge conflict이 발생할 수 있습니다.</u>



### 이미 만들어진 프로젝트 Git Hub에 추가하기

1. Git Hub에 들어가서 Repository를 하나 만듭니다.
2. `...or push an existing repository from the command line` 부분에 `git remote add origin git@github.com:dev-splin/git-undo.git`를 복사한 후 git에 붙여 넣으면 origin이라는 remote가 생긴 것을 볼 수 있습니다.
3. `git push`를 이용해서 push를 해주면 Git Hub에 작업했던 모든 파일이 올라가게 됩니다!



## fetch와 pull의 차이점

- 두 명령어 모두 server에 업데이트 된 내용을 local에 업데이트 하고 싶을 때 사용하는 명령어라는 것은 같습니다.

- **fetch**

  ![fetch](C:\Users\1\Desktop\Study\사진\fetch.jpg)

  ​	내가 현재 작업하고 있는 HEAD는 그대로 유지하면서 server에 업데이트된 history 정보만 가지고 옵니다.

  ​	<u>server에서 어떤 일들이 지금 발생하고 있는지, 누가 어떤 일을 했는지 확인하고 싶은 경우에 많이 쓰입니다.</u>

- **pull**

  ![pull](C:\Users\1\Desktop\Study\사진\pull.jpg)

  server에 있는 내용을 받아와서 나의 local 버전도 server와 함께 동일하게 만들고 싶을 때 사용합니다. 

  HEAD를 보면 server에 있는 HEAD와 동일한 commit을 가리키고 있기 때문에 synchronized가 잘 된것을 볼 수 있습니다.



### 명령어 

**git clone**

- https://github.com/dev-splin/test.git (github에서 복사해온 주소) test-project(원하는 이름)

  ​	해당하는 repository를 원격에서 받아옵니다. 

  ​	주소까지만 입력할 시 repository 이름 그대로 해당 경로에 새로운 폴더가 생성됩니다.

  ​	원하는 이름을 입력하면 repository 이름 대신 원하는 이름으로 폴더가 생성됩니다.

---

**git remote**

​	서버에 관련된 정보들을 확인할 수 있습니다. (기본적으로 server있는 이름은 origin이라고 설정되어 있습니다.)

- -v

  ​	서버에 있는 정보들을 조금 더 자세히 확인할 수 있습니다.

- add server(원하는 이름) https://github.com/dev-splin/test.git (github 에서 받아온 주소 <u>동일한 링크는 하면 안 됩니다!</u>)

  ​	오픈소스 프로젝트에 참가할 때, fork한 repository에서는 다수의 origin을 원하는 이름으로 설정할 수 있습니다.

- show origin(정보를 확인할 이름)

  ​	origin(정보를 확인할 이름)의 관련된 정보를 자세히 볼 수 있습니다.

---

**git push**

​	ID와 Password를 입력한 후 commit한 것들을 server에 올릴 수 있습니다.

​	<u>git환경에서 설정된 이름과 이메일을 따라 github에 올라가기 때문에 신경써주어야 합니다.</u>

- -f

  ​	만약 서버에 a라는 파일의 변경사항이 있는데 local에서 rebase하지 않고 a라는 파일을 변경했을 때, `git push`  를 이용하면 conflict이 발생합니다. 하지만, force(f)를 사용하면 서버에 있는 변경사항과는 상관없이  local에 있는 변경사항을 적용합니다. 

  <u>웬만하면  server에 변동된 사항이 있다면 `git pull` 또는 `git fetch`를 이용해 server에 맞게 업데이트 한 다음에 push를 하는게 맞습니다. 하지만 간혹 기존의 git history를 rebase를 이용해서 변경하거나 history를 변경했다면 부득이하게 force라는 옵션을 이용해서 push를 해야 합니다.</u>

---

**git fetch**

​	내가 현재 작업하고 있는 HEAD는 그대로 유지하면서 server에 업데이트된 history 정보만 가지고 옵니다.

​	<u>server에서 어떤 일들이 지금 발생하고 있는지, 누가 어떤 일을 했는지 확인하고 싶은 경우에 많이 쓰입니다.</u>

- origin (server의 이름)

  ​	해당하는 server의 history를 가지고 옵니다.

  - master (branch의 이름)

    ​	해당하는 origin server의 master branch만 가지고 옵니다.

---

**git pull**

​	server에 있는 내용을 받아와서 나의 local 버전도 server와 함께 동일하게 만들고 싶을 때 사용합니다.

​	<u>default로 merge를 이용하기 때문에 conflict이 났을 때 conflict을 해결하면 두개의 commit을 통합한 새로운 commit이 생깁니다.</u>

- --rebase

  ​	server에 있는 내용을 받아온 후 나의 local 버전을 적용시킵니다.

  ​	<u>conflict이 났을 때 conflict을 해결하면 rebase를 이용해 local의 commit만 새로운 commit 되고 server에서 받아온 commit은 그대로 유지 되어 fast-foward 방식으로 merge할 수 있게됩니다.</u>

---

**git blame**

- fixtures/dom/src/tags.js (원하는 파일명)

  ​	해당 파일에 존재하는 코드들이 어떤 commit에 의해서 도입 되었는지 자세하게 확인해서 디버깅할 수 있습니다.

  ​	<u>다양한 IDE에서 extension(vscode의 경우는 Git Lens)을 이용해서  IDE에서 바로 확인해 볼 수도 있습니다!</u>

---

출처 : https://academy.dream-coding.com/

