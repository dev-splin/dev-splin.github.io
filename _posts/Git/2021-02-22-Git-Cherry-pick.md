---
title: "Git : Cherry pick"
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
  - "Git : Cherry pick"
toc: true
toc_sticky: true
toc_label: 목차
published: false
---

# Cherry pick

![cherry pick](https://user-images.githubusercontent.com/79291114/108801161-50910880-75d8-11eb-929f-94758441ebdc.jpg)

- service branch에서 서비스를 개발하고 있다가 필요한 기능이 있는 f라는 commit만 따로 master branch에 merge 하고 싶을 때 사용할 수 있습니다.
- 가지고 오고자하는 branch로 이동한 후 `git cherry-pick f2b9178(가져올 기능이 있는 commit의 해쉬코드)` 명령어를 사용하면 됩니다.



---

### 명령어

**git cherry-pick** 

- f2b9178 (가져올 기능이 있는 hashcode)

  ​	현재 branch에 f2b9178에 해당하는 commit을 가져옵니다.

---

출처 : https://academy.dream-coding.com/