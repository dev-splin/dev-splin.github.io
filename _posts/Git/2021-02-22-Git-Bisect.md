---
title: "Git : Bisect "
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
  - "Git : Bisect"
toc: true
toc_sticky: true
toc_label: 목차
published: false
---

# Bisect

- 이제까지 제품에 아무런 문제가 없이 정상적으로 동작하는 기능이 최근부터 조금씩 이상한 문제가 발생한다면, 개발 tool에 있는 디버깅 tool을 이용해 소스코드로 또는 성능을 분석해서 문제상황을 해결하는것이 좋습니다.

- 그럼에도 문제가 잘 파악되지 않는다면 그 때 쓸 수 있는 것이 바로 git bisect 입니다.
- commit history에서 3주 전까지는 괜찮았지만 3주부터 최근까지 문제가 도입이 된 것 같다고 가정했을 때 3주 전의 commit은 good, 최근 commit은 bad라고 설정을 해 놓으면 이진탐색알고리즘을 이용해서 많은 commit이 중간에 있더라도 빠르게 문제의 commit을 찾아낼 수 있는 유용한 명령어입니다.



**사용 방법**

1. ![bisect exam1](https://user-images.githubusercontent.com/79291114/108801116-322b0d00-75d8-11eb-9d2d-f3d59fcd1019.jpg)

   hashcode가 75726fadf 인 commit까지는 잘 동작하는 것 같다면 그 부분으로 `git checkout 75726fadf `를 이용해 해당 commit으로 이동하고 프로그램 테스트를 해봅니다.

2. `git bisect start` 를 이용해 bisect을 시작합니다. 잘 동작한다면 `git bisect good` 이라고 마크를 해줍니다.

3. 최신 master 포인터로 돌아가서 최신버전 프로그램 테스트를 해보고 문제가 발생한다면 `git bisect bad`로 마크를 해줍니다.

4. good 마크와 bad 마크를 설정해 놓으면 bisecting 을 시작합니다.

   ![bisect exam2](https://user-images.githubusercontent.com/79291114/108801117-32c3a380-75d8-11eb-841d-7d815bbaf0d7.jpg)

   good과 bad 사이의 12개의 commit이 존재하는데 4번만 확인하면 된다고 나오고 그 중간 지점 즈음에 commit에 checkout 한 상태가 됩니다.

5. 현재 commit에서 프로그램 테스트를 해보고 문제가 발생하지 않는다면 `git bisect good` 마크를 해주면 그 다음 commit으로 이동하게 됩니다.

6. 다음 commit으로 이동하게 되면 똑같이 프로그램 테스트 후 문제가 발생해서 `git bisect bad` 마크를 해줄 때 까지 반복합니다.

7. bad 마크를 해준 commit이 발생하면 그 commit과 바로 직전 good 마크를 해준 commit 중간지점 어딘가에 commit에 문제가 있는 것이므로 그 중간 commit으로 이동합니다. 이 중간 commit에서 프로그램 테스트를 하고 마크를 해줍니다. 만약 4번의 포인트가 다 완료가 되면 결과물이 나오게 됩니다.

8. ![bisect exam3](https://user-images.githubusercontent.com/79291114/108801115-30f9e000-75d8-11eb-9e9f-678dbbcf36e4.jpg)

   어떤 commit이 bad commit인지 알려주는 것을 볼 수가 있습니다.

9. bisect을 다 이용했다면 `git bisect reset` 명령어를 이용해 원래 branch로 돌아갈 수 있습니다.

---

### 명령어

**git bisect**

​	개발tool로 디버깅하기가 힘든 경우 버그를 포함하고 있는 commit을 이진탐색알고리즘을 이용해 빠르게 찾아낼 수 있도록 도와주는 명령어입니다.

- start

  ​	bisect를 시작합니다.

- bad

  ​	문제가 있는 commit이라고 마크해둡니다.

- good

  ​	문제가 없는 commit이라고 마크해둡니다.

- reset

  ​	bisect를 다 이용했을 때, 원래 branch로 돌아갑니다.

---

출처 : https://academy.dream-coding.com/

