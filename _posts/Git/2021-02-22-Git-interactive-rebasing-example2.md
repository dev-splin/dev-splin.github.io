---
title: "Git : Interactive Rebasing 예제(commit 분할하기)"
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
  - "Git : Interactive Rebasing"
toc: true
toc_sticky: true
toc_label: 목차
---

# Interactive Rebasing 예제2



## commit 분할하기

![interactive rebasing exam1_1](https://user-images.githubusercontent.com/79291114/108801554-62bf7680-75d9-11eb-97e6-cf6c25a002fa.jpg)

- 'Add payment library and Add payment service' commit을 보면 두 가지의 내용이 한 가지의 commit에 들어 있는 걸 확인할 수가 있습니다.

- commit 하나에는 하나의 기능만 있는 것이 좋습니다. 그래야 history 관리하기도 쉽고 나중에 문제를 발견했을 때 revert 하기에도 좋습니다.
- 이번 예제는 'Add payment library and Add payment service'의 commit을 두 가지로 나누는 것 입니다!

---

**해결 방법**

1. `git rebase -i 707de7d` 를 이용해 rebase창으로 이동합니다.

2. 'Add payment library and Add payment service' commit의 명령어를 e나 edit로 바꾸고 저장 후 종료합니다.

3. 로그를 확인해 보면

   ![interactive rebasing exam2_2](https://user-images.githubusercontent.com/79291114/108801560-64893a00-75d9-11eb-8e6d-db8c504abfd0.jpg)

   'Add payment library and Add payment service' commit으로 HEAD가 이동한 것을 볼 수 있습니다.

4. `git reset HEAD~1` 을 이용해 'Add payment library and Add payment service' commit의 내용을 working directory로 옮길 수 있습니다.

5. `git status`를 통해 확인해보면

   ![interactive rebasing exam2_3](https://user-images.githubusercontent.com/79291114/108801561-6521d080-75d9-11eb-8d80-31ab58b7e64e.jpg)

   'package.json'파일과 'payment-service.txt' 파일이 working directory로 옮겨간 것을 볼 수 있습니다.

6. `git add package.json` 과 `git commit -m "Add payment library"` 명령어를 사용해 하나의 파일로 하나의 commit을 만듭니다.

7. 'payment-service.txt' 파일도 같은 방법으로 commit 합니다.

8. 로그를 확인해보면 

   ![interactive rebasing exam2_4](https://user-images.githubusercontent.com/79291114/108801562-6521d080-75d9-11eb-8ec7-6c22f8a06a7c.jpg)

   'Setup Dependecies' 시점에서 기존의 commit과 방금 만든 commit이 갈라진 것을 볼 수 있습니다.

9. `git rebase --continue` 명령어를 이용해 rebase를 완료하면

   ![interactive rebasing exam2_5](https://user-images.githubusercontent.com/79291114/108801559-64893a00-75d9-11eb-8296-412e3d4eaf58.jpg)

   'Add payment library and Add payment service' commit이 나누어진 것을 볼 수 있습니다.

---

출처 : https://academy.dream-coding.com/

