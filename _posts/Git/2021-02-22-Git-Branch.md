---
title: "Git : Branch"
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
  - "Git : Branch"
toc: true
toc_sticky: true
toc_label: 목차
---

# Branch

![branch](https://user-images.githubusercontent.com/79291114/108801142-440cb000-75d8-11eb-9289-15aa235aadd1.jpg)

- Git에서 따로 지정하지 않는 이상 master branch가 기본적으로 쓰입니다. 보통 master branch는 코드가 검증이되고 기능에 문제가 없는 내용들만 포함이 되어져 있습니다.
- 만약, 새로운 기능인 Feature A를 개발한다고 한다면 master branch에서 바로 작업을 하기 보다는 Feature A라는 branch를 새로 만들어서 commit을 하는 것이 중요합니다.

- 기능 별로 리팩토링 별로 버그픽스 별로 branch를 만들어서 일을 하게 되면 동시다발적으로 다수의 branch로 다수의 개발자가 개발을 할 수 있기 때문에 병렬적으로 업무를 진행할 수 있는 장점이 있습니다.
- Feature A에 branch가 제품에 포함이 될 준비가 되었다면 Feature A의 commit들을 master branch로 merge 할 수가 있습니다. merge가 완벽하게 이루어졌다면 Feature A branch는 삭제를 해줍니다.
- branch 위에서 commit들을 그대로 master branch에 전부 다 merge 하는 경우도 있지만 대부분은 이런 Feature A에서 작업했던 히스토리가 전부 다 merge될 필요가 없기 때문에 e와 f의 commit을 합해서 하나의 commit을 만든 후 그 commit만 깔끔하게 master branch로 가져오는 방법도 있습니다.



## 명령어

**git branch**

​	지금 현재 Repository에 있는 branch들을 확인할 수 있습니다.
- --all

  ​	원격 GIthub와 같은 서버와 연결된 Repository일 경우 서버에 있는 branch들의 모든 정보를 보여줍니다.

- new-branch(만들 branch 이름)

  ​	'new-branch'라는 이름의 branch를 만들어줍니다.

- -v

  ​	간단하게 최신 commit들도 같이 확인할 수 있습니다.

- --merged

  ​	현재 branch에 merge가 된 branch들을 확인해 볼 수 있습니다.

- --no-merge

  ​	현재 branch에 merge가 되지 않은 branch들을 확인해 볼 수 있습니다.

- -d new-branch2(삭제할 branch 이름)

   new-branch2라는 barnch를 삭제할 수 있습니다.

- --move fix(변경할 branch 이름) fix-welcome(변경될 branch 이름)

  ​	`fix`라는 brach를 `fix-welcome`이라는 이름으로 변경한다.

---

**git switch** 

- new-branch(branch 이름)

  ​	new-branch(branch 이름)로 이동합니다.

- -C new-branch2(만들 branch 이름)

  ​	new-branch2라는 branch를 만들고 동시에 이동합니다.

---

**git checkout**

-   fix(branch이름 or tag이름 or hashcode)

  ​	해당하는 branch나 tag로 이동하거나 hashcode에 해당하는 버전으로 이동합니다.

- -b testing v2.0.0

  ​	testing이라는 branch를 만들면서 v2.0.0으로 이동한다.

---

**git push**

- origin
  - --delete new-branch2

    ​	만약 지금 Repository가 서버에도 있다면, 그래서 branch를 삭제한 것을 원격에도 업데이트하고 싶다면 git push 명령어를 이용해서 origin(원격이 있는 곳)에서 --delete(삭제) 할 수 있습니다. (**뒤에 다시 다룸**)

  - --set-upstream origin fix-welcome

    ​	fix-welcome으로 이름이 변경된 branch를 원격에 업데이트 할 수있습니다. (**다시 다룸**)

---

출처 : https://academy.dream-coding.com/