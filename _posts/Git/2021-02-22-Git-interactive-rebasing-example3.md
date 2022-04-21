---
title: "Git : Interactive Rebasing 예제(commit 묶기)"
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

# interactive rebasing 예제3



## 여러개의 commit 묶기

![interactive rebasing exam2_5](https://user-images.githubusercontent.com/79291114/108801987-846d2d80-75da-11eb-8119-ab403b1a1c6d.jpg)

- 'Add payment client' commit부터 'Add payment library' commit까지를 묶어 'Add payment service' 라는 commit을 만들어봅시다!

---

**해결 방법**

1. `git rebase -i 707de7d` 를 이용해 rebase창으로 이동합니다.

2. squash는 이전의 commit과 묶기 때문에 'Add payment client', 'WIP', 'Add payment service' commit의 명령어만 s나 squash로 바꾸고 저장 후 종료합니다.

3. 'Add payment library' commit과 함께 4개의 commit이 합쳐지고 commit message를 작성할 수 있는 창이 나옵니다.

   ![interactive rebasing exam3_1](https://user-images.githubusercontent.com/79291114/108801594-82ef3580-75d9-11eb-9cd7-93596f4f65ca.jpg)

4. 'Add payment service' message만 남기고 다 지웁니다.

   ![interactive rebasing exam3_2](https://user-images.githubusercontent.com/79291114/108801589-81be0880-75d9-11eb-8f4f-a8e9a4a9474a.jpg)

5. 저장 후 종료하면 4개의 commit이 합쳐지고 새로운 commit이 생긴 것을 볼 수 있습니다. (기존의 'Add payment service' commit과 hashcode가 다릅니다.)

   ![interactive rebasing exam3_3](https://user-images.githubusercontent.com/79291114/108801591-82569f00-75d9-11eb-8c54-933b186ebc74.jpg)

---

출처 : https://academy.dream-coding.com/