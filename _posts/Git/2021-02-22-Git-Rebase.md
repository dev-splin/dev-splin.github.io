---
title: "Git : Rebase"
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
  - "Git : Rebase"
  - "Git : Interactive Rebasing"
toc: true
toc_sticky: true
toc_label: 목차
published: false
---

# Rebase

- fast-forward merges가 아니라 three-way merges를 사용한다면 히스토리가 복잡해 질 수 있는데, 그것을 피할 수 있는 방법이 rebase를 이용하는 방법입니다.

![rebase](https://user-images.githubusercontent.com/79291114/108801691-bb8f0f00-75d9-11eb-8dcd-7a97b30b0cf5.jpg)

- feature A branch가 파생된 시점이 d라고 했을 때 e가 가리키고 있는게 d가 아니라 g로 변경하게 된다면, 즉 feature A branch를 master branch의 최신 버전으로 rebase하게 된다면 fast-forward가 가능해집니다.



**주의사항**

![rebase precautions](https://user-images.githubusercontent.com/79291114/108801693-bd58d280-75d9-11eb-8c05-58a9060b8ca7.jpg)

- 혼자서 feature A branch에서 작업을 하고 있다면 언제든지 rebase를 자유롭게 할 수 있지만, 다른 개발자와 함께 feature A에서 작업을 하고 있다면 위험해 질 수 있습니다.
- 그 이유는 rebase를 하게 될 때 기존의 commit을 유지하는 것이 아니라 commit은 변하지 않기 때문에 변경사항이 발생하면 새로운 commit을 만들게 되는데, 그렇기 때문에 겉으로는 똑같아 보이지만 실제로는 다른 e와 f에 commit이 생겨 내가 rebase를 하고 push를 해서 서버에 변경된 정보를 업데이트하게 된다면, 다른 개발자가 가지고 있는 **feature A의 e와 f는 전혀 다른 commit이 되기 때문**에 나중에 merge confilict이 발생할 수 있습니다.
- 그래서 rebase는 정말 유용하지만 다른 개발자와 함께 branch 위에서 작업을 하고 있고 이미 히스토리가 서버에 업로드되어 있는 경우라면 업로드된 히스토리는 절대 rebase하면 안됩니다.



#### 예제

![rebase exam](https://user-images.githubusercontent.com/79291114/108801718-cb0e5800-75d9-11eb-84f0-97e3646a62f9.jpg)

- master가 a, b, c, d를 merge하고 feature-b가 d에서 파생되고 난 후 master에 feature-a가 e,f를 merge했고 그 이후에 feature-b가 g, h를 merge 한 것을 볼 수 있습니다.
- 여기에서 만약 내가 fast-forward를 하고 싶다면 feature-b branch를 최신 master branch인 f에 rebase하면 간단하게 fast-forward를 할 수 있습니다.

1. `git checkout feature-b` 명령어를 이용해 feature-b branch로 이동합니다.
2. `git rebase master` 명령어를 이용해 feature-b를 mastre branch에 rebase 합니다.
3. `git checkout master` 명령어를 이용해 다시 master branch로 이동합니다.
4. `git merge feature-b` 명령어를 이용해 feature-b branch를 merge 합니다.
5. `git branch -d feature-a` 와 `git branch -d feature-b` 명령어를 이용해 두 개의 branch를 삭제해주면 fast-forward 할 수 있습니다.

![rebase exam2](https://user-images.githubusercontent.com/79291114/108801717-ca75c180-75d9-11eb-9f2d-b5d241237e5f.jpg)



## rebase --onto

- 여러가지의 branch를 만드는, 특히 여러가지를 체이닝해서 branch 위에 또 다른 branch를 만드는 경우에 유용하게 쓸 수 있는 옵션입니다.

![rebase onto](https://user-images.githubusercontent.com/79291114/108801728-d792b080-75d9-11eb-9c31-8ad38ef15d49.jpg)

- 이렇게 master branch에서 service branch를 만든 다음에 이 serivce를 쓰면서 만들어야 되는 UI가 있다면 별도로 UI branch를 만들어서 일을 진행하는 것 같은 경우가 많다고 합니다.
- 새로 개발하고 있는 service에 의존적인 UI를 만들다가 굳이 이 service가 필요가 없어서 service가 없이도 이 UI를 master branch에 merge를 하고 싶을 때 사용할 수 있습니다.
- merge를 간편하게 하기 위해서 UI branch가 master branch를 가리키고 있으면 편리하기 때문에 `git rebase --onto master service ui` 명령어를 사용해, service에서 파생된 UI branch만 master branch에 rebase를 할 수 있습니다.



## interactive rebasing

- `git commit --amend` 명령어를 이용해서 최신 commit의 타이틀을 변경하거나 수정사항을 업데이트 할 수 있었습니다.
- 만약 최신 commit이 아니라 이전의 commit을 업데이트 하고 싶을 때 interactive rebasing를 사용합니다.
- interactive rabasing을 하는 순간 뒤에 있는 모든 포인터들이 업데이트 되기 때문에 주의해야합니다. 즉, 기존에 존재하는 history를 변경하는 것으므로 때문에 서버에 업로드된 경우라면 피하는게 좋습니다.
- 혼자서 작업 하는 경우 history를 변경해야 한다면 강추하는 명령어입니다!



#### 예제

![interactive rebasing](https://user-images.githubusercontent.com/79291114/108801744-e4170900-75d9-11eb-8120-dd26945d4dcd.jpg)

- WIP가 message로 입력된 commit의 message를 바꾸고 싶다면 WIP 이전 해시코드(98955fc)부터 시작해야합니다. 즉, 지정한 hashcode부터 interactive하게 계속 뒤에 이어지는 commit까지 함께 rebase를 한다는 의미입니다.

- `git rebase -i 98955fc` 명령어로 사용할 수 있습니다.

![interactive rebasing1](https://user-images.githubusercontent.com/79291114/108801745-e4af9f80-75d9-11eb-9a0e-ba063b16c6ac.jpg)

- 명령어를 실행해보면 창이 하나 나오는데, 우리가 지정한 hashcode 다음에 이어지는 모든 commit들이 나와 있는 걸 확인할 수 있습니다.
- hashcode 앞의 명령어를 바꿔 기능을 수행할 수 있습니다. (p 나 pick이라고 적습니다.)
  - p, pick
    - 해당commit을 사용합니다.
  - r, reword
    - 해당commit을 사용하지만, commit message를 바꿉니다.
  - e, edit
    - 해당commit을 사용하긴하지만, 안에 commit 내용을 바꿉니다.
  - s, squash
    - 이전에 merge를 할 때 하나로 묶어 주는 three-way merges 처럼 여러가지의 commit을 하나로 묶어주는 방법입니다. (이전의 commit과 묶어줍니다.)
  - f, fixup
    - squash와 비슷하지만 message를 남기지 않습니다.
  - x, exec
    - 해당commit부터 shell 명령어를 직접적으로 이용하고 싶을 때 사용할 수 있습니다.
  - b, break
    - 해당commit에서 rebase를 멈춥니다.
  - d, drop
    - 해당commit을 제거합니다.
- WIP message를 바꾸고 싶은 것이기 때문에 pick대신 r이나 reword라고 작성한 다음 저장하고 파일을 종료하면, 다시 commit message를 수정할 수 있는 창이 나옵니다. 여기서 원하는 message를 작성한 다음 저장하고 종료한 후 history를 확인해 보면 WIP 대신 작성한 message가 나오는 것을 볼 수 있습니다.

![interactive rebasing2](https://user-images.githubusercontent.com/79291114/108801743-e37e7280-75d9-11eb-9d1a-6a33f8f1221c.jpg)



#### 더 다양한 예제

- [필요없는 commit 삭제하기](https://dev-splin.github.io/git/Git-interactive-rebasing-example1/)
- [commit 분할하기](https://dev-splin.github.io/git/Git-interactive-rebasing-example2/)
- [commit 묶기](https://dev-splin.github.io/git/Git-interactive-rebasing-example3/)



---

### 명령어

- **git rebase**

  - master

    ​	현재 branch를 최신 master branch 위치에 rebase 합니다.

  - --onto master service(branch 이름) ui(branch 이름)

    ​	master branch에 service branch에서 파생된 ui branch를 rebase 합니다.

  - -i HEAD or hashcode

    ​	지정한 HEAD 나 hashcode의 다음 commit부터 최신 commit까지를 interactive rebasing 합니다

  - --abort
  
    ​	rebase를 취소합니다.

---

출처 : https://academy.dream-coding.com/

