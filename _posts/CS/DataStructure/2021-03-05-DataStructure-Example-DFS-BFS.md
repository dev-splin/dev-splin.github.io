---
title: "DataStructure : 예시로 보는 DFS(Depth First Search)와 BFS(Breadth First Search)"
excerpt_separator: <!--more-->
categories:
  - CS(Computer Science)
  - DataStructure
tags:
  - CS(Computer Science)
  - DataStructure
  - "DataStructure : DFS(Depth First Search)"
  - "DataStructure : BFS(Breadth First Search)"
toc: true
toc_sticky: true
toc_label: 목차
---

# 예시로 보는 DFS(Depth First Search)와 BFS(Breadth First Search)

`DFS`는 인접한 노드들을 마지막 노드를 만날 때 까지 갔다가 다시 올라와서 형제 노드들을 방문하는 방식으로 탐색합니다.

`BFS`는 시작점에서 자신의 인접한 노드들을 먼저 방문하고 그 다음 level의 노드들을 전부 방문하는 level 단위방식으로 탐색합니다.

`DFS`와 `BFS`는 트리가 아니기 때문에 0에서 시작할 필요 없습니다. 3에서 시작해도 규칙에 맞게 잘 적용됩니다.

![DFS,BFS](https://user-images.githubusercontent.com/79291114/110069884-2996bb80-7dbc-11eb-8990-4d0710e223d4.jpg)

 

## DFS(Depth First Search)

아래와 같은 노드들이 있을 때 Stack을 사용하여 DFS를 해보겠습니다. 보기 쉽게 0부터 시작하겠습니다. (호출했던 노드들은 다시 호출하지 않습니다.)

![DFS](https://user-images.githubusercontent.com/79291114/110069882-28658e80-7dbc-11eb-9789-990b366037a7.jpg)

1. 처음 시작 노드 0을 Stack에 넣습니다. 

   `출력 : X`, `Stack : [0]`

2. 0을 빼서 출력해주고 인접한 노드 1을 넣습니다. 

   `출력 : 0`, `Stack : [1]`

3. 1을 빼서 출력해주고 인접한 노드 2와 3을 넣습니다.

   `출력 : 0,1`, `Stack : [2,3]`

4. Stack은 늦게 넣은 값이 먼저 빠지기 때문에 3을 빼서 출력해주고 인접한 노드인 2,4,5 중 호출하지 않았던 4,5를 넣어줍니다.

   `출력 : 0,1,3`, `Stack : [2,4,5]`

5. 5를 빼서 출력해주고 인접한 노드인 6,7을 넣어줍니다.

   `출력 : 0,1,3,5`, `Stack : [2,4,6,7]`

6. 7을 빼서 출력해주고 호출되지 않았던 노드가 없으니 넘어갑니다.

   `출력 : 0,1,3,5,7`, `Stack : [2,4,6]`

7. 6을 빼서 출력해주고 8을 넣습니다.

   `출력 : 0,1,3,5,7,6`, `Stack : [2,4,8]`

8. Stack에 남은 2,4,8의 인접노드 중 호출되지 않는 노드는 없으므로 8, 4, 2 순으로 빼서 출력해줍니다.

   `출력 : 0,1,3,5,7,6,8,4,2`, `Stack : []`

9. 순회가 종료됩니다.



### 재귀 호출로 DFS사용하기

**재귀 호출로도 DFS가 가능**합니다. 스택과 다른 점은 **스택은 쌓고나서 출력하기 때문에 2개의 노드가 있다고 하면 2번 째로 넣은 노드가 호출** 됩니다. 하지만 **재귀호출은 정방향으로 실행되기 때문에 1번 째의 노드가 호출**됩니다.

그림에서 2는 자식이 4도 있고 3도 있는데 3은 2의 인접한 노드이면서 1과 4의 인접한 노드이기도 합니다. 그렇기 때문에 **호출 순서는 연결관계를 입력할 때 어떤 노드와의 관계를 먼저 입력했느냐에 따라 결정**됩니다.

만약 위의 그림에서 0~8의 숫자 순서대로 입력되었다고 했을 때 출력 순서는 `0,1,2,3,4,5,6,8,7` 이 됩니다.



## BFS(Breadth First Search)

아래와 같은 노드들이 있을 때 Queue를 사용하여 BFS를 해보겠습니다. 보기 쉽게 0부터 시작하겠습니다. (호출했던 노드들은 다시 호출하지 않습니다.)

![BFS](https://user-images.githubusercontent.com/79291114/110069866-20a5ea00-7dbc-11eb-8066-22083d43f61b.jpg)

1. 처음 시작노드인 0을 Queue에 넣습니다.

   `출력 : X`, `Queue : [0]`

2. 0을 빼서 출력해주고 인접한 노드인 1을 넣습니다.

   `출력 : 0`, `Queue : [1]`

3. 1을 빼서 출력해주고 2,3을 넣습니다.

   `출력 : 0,1`, `Queue : [2,3]`

4. Queue는 먼저 들어온 값이 먼저 나가기 때문에 2를 빼서 출력해주고 인접한 노드들 중 호출되지 않은 노드인 4를 넣어줍니다.

   `출력 : 0,1,2`, `Queue : [3,4]`

5. 3을 빼서 출력해주고 5를 넣어줍니다.

   `출력 : 0,1,2,3`, `Queue : [4,5]`

6. 4를 빼서 출력해주고 호출되지 않은 인접한 노드들이 없으므로 넘어갑니다.

   `출력 : 0,1,2,3,4`, `Queue : [5]`

7. 5를 빼서 출력해주고 6,7을 넣어줍니다.

   `출력 : 0,1,2,3,4,5`, `Queue : [6,7]`

8. 6을 빼서 출력해주고 8을 넣어줍니다.

   `출력 : 0,1,2,3,4,5,6`, `Queue : [7,8]`

9. Queue에 남은 7,8의 인접한 노드 중 호출되지 않은 노드는 없으므로 7,8을 순서대로 빼서 출력해줍니다.

   `출력 : 0,1,2,3,4,5,6,7,8`, `Queue : []`

10. 순회가 종료됩니다.



---

참고 : https://www.youtube.com/watch?v=_hxFgg7TLZQ