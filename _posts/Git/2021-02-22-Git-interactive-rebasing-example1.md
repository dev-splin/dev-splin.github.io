---
title: "Git : Interactive Rebasing 예제(필요없는 commit 삭제하기)"
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
  - "Git : Interactive Rebasing"
toc: true
toc_sticky: true
toc_label: 목차
published: false
---

# Interactive Rebasing 예제1



## 필요없는 commit 삭제하기

![interactive rebasing exam1_1](https://user-images.githubusercontent.com/79291114/108801500-3c99d680-75d9-11eb-9135-2e4eeb92891b.jpg)

- commit message가 .인 커밋을 삭제해봅시다! 어떻게 하면 될까요? 작업을 하다보면 충돌이 날 수도 있는데, 그 부분도 git status에 나오는 힌트를 잘 활용하셔서 해결해봅시다!

---

**해결방법**

1. `git rebase -i fa7bbd6` 를 이용해 rebase창으로 이동합니다.

2. 삭제하고싶은 . commit의 명령어를 삭제 명령어인 d나 drop으로 바꿔주고 저장 후 종료합니다.

3. ![interactive rebasing exam1_2](https://user-images.githubusercontent.com/79291114/108801503-3d326d00-75d9-11eb-9b03-126c0ad9b38a.jpg)

   . commit에서 payment-ui.txt 파일을 삭제 했는데, Add payment UI commit에서 파일을 수정했기 때문에 conflict이 발생합니다. (이런경우에는 수동적으로 관리를 해주어야 합니다.)

4. `git status`를 실행해 상태를 확인해 보면

![interactive rebasing exam1_3](https://user-images.githubusercontent.com/79291114/108801495-3b68a980-75d9-11eb-94a5-d8f73ade0773.jpg)

​	interactive rebase가 실행 중 (No commands remaining 이므로 현재 0dd7ab 즉, Add payment UI commit에 있는 것입니다.) 인데, 그 이전에서 이 파일이 삭제되었다!라고 나옵니다.

5. status에 나와있는 `git add .` 명령어를 이용하면 payment-ui.txt 파일이 생성됩니다.
6. 수정이 완료되고 `git rebase --continue`를 입력하면 commit message를 수정할 수 있는 창이 나타나는데, message를 수정해도 되고 이전 message를 그대로 사용하고 싶다면 그냥 저장하고 종료하면 됩니다.
7. git history를 확인해보면 . commit이 삭제된것을 볼 수 있습니다.

![interactive rebasing exam1_4](https://user-images.githubusercontent.com/79291114/108801498-3c99d680-75d9-11eb-92a7-a121d726a94a.jpg)

---

출처 : https://academy.dream-coding.com/