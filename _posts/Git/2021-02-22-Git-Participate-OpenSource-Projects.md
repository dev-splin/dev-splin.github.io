---
title: "Git : 오픈소스 프로젝트 참여하는 방법"
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
toc: true
toc_sticky: true
toc_label: 목차
published: false
---

# 오픈소스 프로젝트

![open source project](https://user-images.githubusercontent.com/79291114/108800999-e9735400-75d7-11eb-83dc-ae8e803aab62.jpg)

- 만약 오픈소스 프로젝트에 참여하고 싶을 때 당장은 나에게 그 repository에 대한 접근과 쓰기 권환이 없으므로 그 repository를 fork 해올 수 있습니다.

- fork

  ​	기존의 repository에 바로 commit해 나가는 것이 아니라 나의 repository에 동일하게 복사해서 가지고 오는 것을 말합니다.

- PR (Pull Request)

  ​	나의 repository에 commit을 하고 난 후 PR을 작성해서 오픈소스 프로젝트에 제출할 수 있습니다.

  ​	PR을 신청하게 되면 오픈소스 프로젝트 관리자가 PR를 리뷰한 후 승인하거나 거절할 수 있습니다.

- rebase

  ​	만약 승인이 되고 난 뒤에 오프소스 프로젝트에 다른 업데이트 된 commit이 있다면 rebase를 이용해서 나의 repository를 최신 버전으로 유지할 수 있습니다.



## 참여 방법

1. Git Hub에서 마음에 드는 오픈소스 프로젝트를 자기 프로젝트로 fork 해옵니다.

   ![open source project exam1](https://user-images.githubusercontent.com/79291114/108801015-f42de900-75d7-11eb-81ba-d3ee02ba697e.jpg)

2. 자신의 repository를 clone 해옵니다. (SSH가 있다면 SSH로 기본은 HTTPS 입니다.)

   ![open source project exam2](https://user-images.githubusercontent.com/79291114/108801009-f1cb8f00-75d7-11eb-8a7f-5d2f1865c106.jpg)

3. `git clone git@github.com:YunChangHyunIT/pull-request-test.git` 을 이용해 현재 경로에 repository 폴더를 만듭니다.

4. branch를 하나 만들고 가져온 파일을 수정한 후 commit 합니다. (merge는 권한이 없으므로 불가능합니다.)

5. push하고 Git Hub에 들어가보면 branch가 업로드 되었다는 배너가 생깁니다.

   ![open source project exam3](https://user-images.githubusercontent.com/79291114/108801010-f2fcbc00-75d7-11eb-8a38-72f085d0b6d0.jpg)

6. 내용을 확인하고 원하는 내용이 들어가 있으면 `Compare & pull request`를 눌러 pull request create 페이지로 이동합니다.

7. Title은 요악해서 정확하게, description은 자세하게 적어주는게 좋습니다.

   아래를 보면 변경사항도 확인할 수 있습니다. 

   그리고  `Allow edits by maintainer(오픈소스 프로젝트 담당자가 수정할 수 있게 해줍니다.)` 에 체크해주세요! (보통 체크를 많이 한다고 합니다!)

8. `Create pull request`를 누르고 오픈소스 프로젝트 Git Hub에 가서 Pull requests Tab을 확인하면 pull request가 만들어진 것을 볼 수 있습니다.

9. ![open source project exam4](https://user-images.githubusercontent.com/79291114/108801011-f2fcbc00-75d7-11eb-80fc-bac4d1f87d7e.jpg)

   - pull requests의 Commits Tab은 어떤 commit이 있는 지 볼 수 있습니다.
   - Files changed Tab은 변경된 코드를 볼 수 있고 오픈소스 프로젝트 담당자는 리뷰를 작성할 수 있습니다.

10. ![open source project exam5](https://user-images.githubusercontent.com/79291114/108801012-f3955280-75d7-11eb-9bcf-4d1eed3a75ed.jpg)

    - 메세지를 입력하고 Commnet만 남기고 싶다면 그냥 `Submit review` 버튼을 누르면 됩니다.
    - Approve를 선택하면 승인이 되면서 오픈소스 프로젝트에 merge할 준비가 된 것입니다.

11. 승인이 완료되면 Conversation Tab에서 merge를 눌러 프로젝트에 적용시킬 수 있습니다.

    ![open source project exam6](https://user-images.githubusercontent.com/79291114/108801014-f3955280-75d7-11eb-96d1-bd977b888433.jpg)

---

출처 : https://academy.dream-coding.com/