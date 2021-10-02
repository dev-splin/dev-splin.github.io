---
title: "BaekJoon : 4256번 (트리)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : DataStructure"
  - "BaekJoon : Tree"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon DataStructure

BaekJoon `DataStructure`(자료구조)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 4256번 (트리)

이진 트리는 매우 중요한 기본 자료 구조이다. 아래 그림은 루트 노드가 유일한 이진 트리이다. 모든 노드는 최대 2개의 자식 노드를 가질 수 있으며, 왼쪽 자식이 순서가 먼저이다. 노드 n개로 이루어진 이진 트리를 BT라고 하자. BT의 노드는 1부터 n까지 유일한 번호가 매겨져 있다.

아래 그림에 나와있는 BT의 루트는 3번 노드이다. 1번 노드는 오른쪽 자식만 가지고 있고, 4와 7은 왼쪽 자식만 가지고 있다. 3과 6은 왼쪽과 오른쪽 자식을 모두 가지고 있다. 나머지 노드는 모두 자식이 없으며, 이러한 노드는 리프 노드라고 부른다.

![img](https://www.acmicpc.net/upload/images/tree(2).png)

BT의 모든 노드를 순회하는 방법은 `전위 순회(preorder), 중위 순회(inorder), 후위 순회(postorder)`로 총 세 가지가 있다. 이 세 방법은 아래에 C 스타일의 의사 코드로 나와 있다. BT의 노드 v에 대해서, v.left는 왼쪽 자식, v.right는 오른쪽 자식을 나타낸다. v가 왼쪽 자식이 없으면 v.left는 ∅와 같고, 오른쪽 자식이 없으면 v.right는 ∅와 같다.

![img](https://www.acmicpc.net/upload/images/treeorder.png)

**BT를 전위 순회, 중위 순회한 결과**가 주어진다. 즉, **위의 함수 중 preorder(root node of BT)와 inorder(root node of BT)를 호출해서 만든 리스트**가 주어진다. 두 **순회한 결과를 가지고 다시 BT를 만들 수 있다.** **BT의 전위, 중위 순회한 결과가 주어졌을 때, 후위 순회했을 때의 결과를 구하는 프로그램**을 작성하시오.

예를 들어, 위의 그림을 전위 순회하면 3,6,5,4,8,7,1,2, 중위 순회하면 5,6,8,4,3,1,2,7이 된다. 이를 이용해 후위 순회하면 5,8,4,6,2,1,7,3이 된다.



### 입력

첫째 줄에 **테스트 케이스의 개수 T**가 주어진다. 각 **테스트 케이스의 첫째 줄에는 노드의 개수 n**이 주어진다. **(1 ≤ n ≤ 1,000)** BT의 모든 노드에는 1부터 n까지 서로 다른 번호가 매겨져 있다. 다음 줄에는 **BT를 전위 순회한 결과**, 그 다음 줄에는 **중위 순회한 결과**가 주어진다. 항상 두 순회 결과로 유일한 이진 트리가 만들어지는 경우만 입력으로 주어진다.

```java
2	// 테스트 케이스 T
4 // 첫 번째 테스트 케이스, 노드의 개수 n
3 2 1 4	// 전위 순회 결과
2 3 4 1	// 중위 순회 결과
8	// 두 번째 테스트 케이스
3 6 5 4 8 7 1 2
5 6 8 4 3 1 2 7
```



### 출력

각 테스트 케이스마다 **후위 순회한 결과를 출력** 한다.

```java
2 4 1 3	// 첫 번째 테스트 케이스의 후위 순회 결과
5 8 4 6 2 1 7 3	// 두 번째 테스트 케이스의 후위 순회 결과
```





### 전위 순회, 중위 순회를 이용한 후위 순회 구하기

**전위 순회와 중위 순회의 특징을 이용하여 후위 순회의 결과를 도출**해낼 수 있습니다.

`전위 순회는 Root -> Left -> Right` 순으로, `중위 순회는 Left -> Root -> Right` 순으로 트리를 순회 합니다. 즉, **Root가 어느 위치에 있느냐에 따라서, 앞에 있으면 전위(pre), 중간은 중위(in), 끝은 후위(post) 순회라고 하는 것**입니다.

전위 순회, 중위 순회를 각각 따로 보면 이걸 도대체 어떻게 해야하지.. 라는 생각이 들 수 있지만, Root 값을 찾으면서 차근 차근 진행하다보면 해결법에 도달할 수 있습니다

위의 전위 순회(3 6 5 4 8 7 1 2)와 중위 순회(5 6 8 4 3 1 2 7)로 그림과 함께 예를 들어 보겠습니다.

![img](https://www.acmicpc.net/upload/images/tree(2).png)

전위는 `pre`, 중위는 `in`으로 표현하고 Root를 찾으면 그 값은 `/`로 바꾸는 방식으로 진행하겠습니다.

**pre(3 6 5 4 8 7 1 2), in(5 6 8 4 3 1 2 7)** : 시작

**pre(/ 6 5 4 8 7 1 2), in(5 6 8 4 / 1 2 7)** : pre에서 Root 값(맨 앞의 3)을 찾아 제거, pre에서 찾은 Root 값을 in에서 제거
이 때, 중위(in) 순회는 `Left -> Root -> Right`이기 때문에 in에서 Root 3의 왼쪽 4개(5 6 8 4)는 노드의 왼쪽, 오른쪽 3개(1 2 7)는 노드의 오른쪽인 것을 알 수 있습니다. 즉, **pre에서 찾은 Root를 이용해 in에서 현재 루트 노드의 왼쪽과 오른쪽을 찾을 수 있는 것**입니다. 이 오른쪽 트리와 왼쪽 트리로 나눠지는 방법을 이용해 후위 순회인 경우를 찾을 수 있는 것입니다.

**pre(/ / 5 4 8 7 1 2), in(5 / 8 4 / 1 2 7)** : pre에서 Root 값(맨 앞의 6) 제거, in에서 Root 값(6 제거)

**pre(/ / / 4 8 7 1 2), in(/ / 8 4 / 1 2 7)** : pre에서 Root 값(맨 앞의 5) 제거, in에서 Root 값(5 제거)

**pre(/ / / / 8 7 1 2), in(/ / 8 / / 1 2 7)** : pre에서 Root 값(맨 앞의 4) 제거, in에서 Root 값(4 제거)

**pre(/ / / / / 7 1 2), in(/ / / / / 1 2 7)** : pre에서 Root 값(맨 앞의 8) 제거, in에서 Root 값(8 제거)

**pre(/ / / / / / 1 2), in(/ / / / / 1 2 /)** : pre에서 Root 값(맨 앞의 7) 제거, in에서 Root 값(7 제거)

**pre(/ / / / / / / 2), in(/ / / / / / 2 /)** : pre에서 Root 값(맨 앞의 1) 제거, in에서 Root 값(1 제거)

**pre(/ / / / / / / /), in(/ / / / / / / /)** : pre에서 Root 값(맨 앞의 2) 제거, in에서 Root 값(2 제거)

위의 경우를 그림과 같이 보게 되면 트리가 나눠지는 모습을 볼 수 있습니다.



### 코드

pre에서 Root노드를 찾아 **in을 이용해 left의 개수를 구한 후, left를 이용해 right 개수까지 구할 수 있습니다.**

배열에 pre와 in을 저장해 놓고 배열을 left, right 개수를 이용해 자른 후 재귀 함수를 진행하면 생각보다 간단하게 결과를 도출할 수 있습니다. 자세한 설명은 주석을 통해 진행하겠습니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	static StringBuilder sb = new StringBuilder();

	// post(후위) 구하기
	public static void postorder(int pre[], int in[]) {
		// 트리의 길이 구하기
		int length = pre.length;
		
		if(length == 0)
			return;

		// pre의 맨 앞은 Root
		int root = pre[0];
		
		int left = 0;

		// in에서 Root 찾기, Root까지의 개수가 왼쪽의 개수(left) -> 배열이 0부터 시작하기 때문
		for (int i = 0; i < length; i++)
			if(in[i] == root) {
				left = i;
				break;
			}

		// Left
		// left가 0이 아닐 때만, 재귀(left가 0이면 왼쪽이 없다는 의미이기 때문)
		// 구한 left 개수로 배열을 잘라서 재귀 함수를 진행
		if(left != 0)
			postorder(slice(pre, 1, left), slice(in, 0, left-1));

		// Right
		// length - left - 1 은 오른쪽을 의미(전체 크기 - 왼쪽 크기 - 1(루트))
		// 구한 left 개수로 배열을 잘라서 재귀 함수를 진행
		if(length - left -1 != 0)
			postorder(slice(pre, left+1, length-1), slice(in, left+1, length-1));

		// Root 출력
		sb.append(root).append(' ');
	}

	// 배열 자르기
	public static int[] slice(int arr[], int start, int end) {
		int result[] = new int[end-start+1];
		
		int index = 0;
		
		for (int i = start; i <= end; i++) {
			result[index] = arr[i];
			++index;
		}
		
		return result;
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			int t = Integer.parseInt(br.readLine());
			
			for (int i = 0; i < t; i++) {
				int n = Integer.parseInt(br.readLine());
				
				int pre[] = new int[n];
				int in[] = new int[n];
				
				StringTokenizer preStk = new StringTokenizer(br.readLine());
				StringTokenizer inStk = new StringTokenizer(br.readLine());
				
				for (int j = 0; j < n; j++) {
					pre[j] = Integer.parseInt(preStk.nextToken());
					in[j] = Integer.parseInt(inStk.nextToken());
				}
				
				postorder(pre, in);
				sb.append('\n');
			}
			
			System.out.println(sb.toString());
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

