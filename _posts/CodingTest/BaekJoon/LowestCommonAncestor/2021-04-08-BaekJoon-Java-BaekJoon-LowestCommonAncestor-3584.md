---
title: "BaekJoon : 3584번(가장 가까운 공통 조상)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Lowest Common Ancestor(LCA)"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Lowest Common Ancestor (LCA)

BaekJoon Lowest Common Ancestor (LCA) 저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 3584

루트가 있는 트리(rooted tree)가 주어지고, 그 트리 상의 두 정점이 주어질 때 그들의 `가장 가까운 공통 조상(Nearest Common Anscestor)`은 다음과 같이 정의됩니다.

- **두 노드의 가장 가까운 공통 조상은, 두 노드를 모두 자손으로 가지면서 깊이가 가장 깊은(즉 두 노드에 가장 가까운) 노드를 말합니다.**

<img src="https://upload.acmicpc.net/4f2eae58-31bf-445f-a7a3-625505e7102c/-/preview/" alt="nca.png" style="zoom:67%;" />

예를 들어 15와 11를 모두 자손으로 갖는 노드는 4와 8이 있지만, 그 중 깊이가 가장 깊은(15와 11에 가장 가까운) 노드는 4 이므로 가장 가까운 공통 조상은 4가 됩니다.

**루트가 있는 트리가 주어지고, 두 노드가 주어질 때 그 두 노드의 가장 가까운 공통 조상을 찾는 프로그램을 작성**하세요



### 입력

**첫 줄에 테스트 케이스의 개수 T**가 주어집니다.

각 **테스트 케이스마다, 첫째 줄에 트리를 구성하는 노드의 수 N**이 주어집니다. `(2 ≤ N ≤ 10,000)`

그리고 그 다음 **N-1개의 줄에 트리를 구성하는 간선 정보**가 주어집니다. **한 간선 당 한 줄에 두 개의 숫자 A B 가 순서대로** 주어지는데, 이는 **A가 B의 부모라는 뜻**입니다. (당연히 **정점이 N개인 트리는 항상 N-1개의 간선**으로 이루어집니다!) A와 B는 1 이상 N 이하의 정수로 이름 붙여집니다.

**테스트 케이스의 마지막 줄에 가장 가까운 공통 조상을 구할 두 노드**가 주어집니다.

```java
2		// 테스트 케이스(T)의 개수를 입력합니다.
16		// 노드의 수(N)을 입력합니다.
1 14	// 여기부터
8 5
10 16
5 9
4 6
8 4
4 10
1 13
6 15
10 11
6 7
10 2
16 3
8 1
16 12	// 여기까지 간선 N-1개를 입력합니다.
16 7	// 가장 가까운 공통 조상을 구할 두 노드를 입력합니다.
5		// 2번째 테스트 케이스 시작
2 3
3 4
3 1
1 5
3 5
```



### 출력

각 테스트 케이스 별로, 첫 줄에 **입력에서 주어진 두 노드의 가장 가까운 공통 조상을 출력**합니다.

```java
4	// 가장 가까운 공통 조상을 출력합니다.
3
```



### Lowest Common Ancestor (LCA)

문제에서 보다시피 `가장 가까운 공통 조상(Nearest Common Anscestor)`이라고도 합니다. `LCA`를 사용할 때, `DP`와 `DFS`도 이용하게 됩니다. `LCA`의 구현은 5가지로 나누어 집니다.

1. 모든 노드에 대한 깊이를 구합니다. (가까운 조상을 판별하기 위해 깊이가 필요, 보통 `DFS`를 이용합니다.)
2. 모든 노드에 대한 2<sup>i</sup>번째(2<sup>0</sup> ~ 2<sup>i</sup>) 부모 노드를 구합니다. (시간 복잡도가 logN이 됩니다.)
3. 최소 공통 조상을 찾을 두 노드를 설정합니다.
4. 깊이가 더 깊은 노드(루트에서 더 멀리있는 노드)의 깊이를 깊이가 더 얕은 노드와 동일하게 맞춰줍니다.
5. 최상단 노드(대부분 루트노드)부터 내려오는 방식으로 두 노드의 공통 부모를 찾아냅니다.

문제를 보면서 적용시켜보면 더 이해가 빠를 것 같습니다.

더 자세한 설명을 알고싶으시다면, [나동빈님 블로그](https://blog.naver.com/ndb796/221282478466)와 [Crocus님 블로그](https://www.crocus.co.kr/660)를 참고하시면 될 것 같습니다.



### 내코드

문제의 해결은 `LCA`를 이용하면 됩니다. 하지만, `DFS`는 루트노드 부터 출발해야 하기 때문에 루트노드가 필요한데, 문제에서 루트노드를 따로 정해주지 않기 때문에 **부모 노드만을 저장하는 배열을 하나 만들어 루트노드**를 구해주어야합니다. 이 부모노드 배열만으로도 더 간단하게 문제를 풀 수 있지만, `LCA`를 정확히 파악하기 위해 `LCA`로 문제를 풀겠습니다. 문제를 보기 전에 1~5번 조건을 한번 더 봐주세요!

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	// 최대 노드 수
	final static int MAX_NODE = 10001;
	// 2^i번째 부모 노드를 표현할 때, i에 올 수 있는 최대값 (노드 수 보다 커야함) 
	final static int MAX_LEVEL = 14;
	static int n;
	
	// 깊이를 저장할 배열
	static int level[];
	// [노드번호][i]로 이루어질 2차원 배열입니다.
	// i는 2^i번째 부모 노드를 표현할 때, i입니다. 즉, [1][3]이면 노드 1의 8번째(2^3) 부모를 말합니다.
	static int parents[][];
	// 연결된 노드를 저장할 리스트(노드가 n개 만큼 있으므로 n+1(노드1이 배열1에 올 수 있게 + 1)개의 리스트를 생성할 것입니다.
	static List<Integer> adjacentNodes[];
	// 해당 인덱스를 노드번호, 값을 부모노드로 사용할 배열입니다.
	static int parentNode[];
	
	// 1번 조건
	// DFS를 이용해 루트노드부터 깊이를 구해줍니다.
	public static void DFS(int node, int depth) {
		
		level[node] = depth;
		
		for(int adjacentNode : adjacentNodes[node]) {
			
			// 이미 깊이가 설정되어 있으면 스킵
			if(level[adjacentNode] != 0)
				continue;
			
			// 현재 노드(node)의 인접 노드들은 자식 노드이기 때문에
			// [자식노드][0] 즉, 자식노드의 1번째(2^0) 부모는 현재 노드(node)가 됩니다.
			parents[adjacentNode][0] = node;
			DFS(adjacentNode,depth + 1);
		}
	}
	
	// 입력된 간선 정보를 이용해 트리를 만듭니다.
	public static void setParent() {
		int startNode = 0;
		
		// 부모노드를 저장한 배열을 이용해 시작노드(루트)를 구해줍니다.
		for (int i = 1; i <= n; i++) {
			if(parentNode[i] == 0)
				startNode = i;
		}
		
		DFS(startNode,1);
		
		// 2번 조건
		// DFS를 수행하고 나면 각 노드들의 1번째(2^0)부모만이 저장되었기 때문에 저장할 수 있는(루트노드를 넘지 않는) 2^i번째 부모를 구합니다.
		for (int i = 1; i <= MAX_LEVEL; i++)
			for (int j = 1; j <= n; j++)
				// 2^i번째 부모를 구하기 위해서 2^(i-1)의부모의 2^(i-1)부모를 이용할 수 있습니다. (DP)
				// 문제의 그래프로 예를 들면, 
				// [3][1] 3번노드의 2번째(2^1)부모노드 10은 [16][1] 즉, 3번노드의 1번째(2^0) 부모노드[16]의 1번째(2^0) 부모노드[0] 와 같습니다.
				// 또, [3][2]는 [10][1]과 같은 것을 볼 수 있습니다.
				
				// 반복문 시작을 1로 둔 것은 DFS를 수행하고 나면 각 노드들의 1번째(2^0)부모만이 저장되었기 때문에
				// 위의 DP성질(i-1)을 이용해 차근차근 부모노드를 구할 수 있기 때문입니다.
				// 예를 들면, 각 노드(j)의 2번째(2^1) 부모노드[n][1]을 구하면 부모노드도 n에 속해있기 때문에
				// 다음 반복문 [n][2]를 부모노드 [n][1]을 이용해 차근차근 구할 수 있습니다.
				parents[j][i] = parents[parents[j][i-1]][i-1];

	}
	
	// 최소 공통 조상을 구합니다.
	public static int LCA(int a, int b) {
		
		// 4번 조건
		// 어떤 인자가 깊이가 깊은지 알 수 없기 때문에 b를 깊이가 깊은 변수로 설정합니다.
		if(level[a] > level[b]) {
			int tmp = a;
			a = b;
			b = tmp;
		}
		
		// 가장 상단에 있는 부모노드 부터 내려오면서 깊이를 체크해 a보다 깊이가 더 깊으면 b를 해당 부모노드로 변경해줍니다.
		// i가 1씩 감소하기 때문에 b로 변한 부모노드의 1번째 (2^0)부모노드 까지 체크하게 되므로 깊이가 같아질 수 있습니다.
		// 예를 들면, 4(a)과 3(b)의 노드가 있으면 3의 상단 노드부터 체크하다가 4보다 깊이가 같거나 깊은 10을 만나게되면 b가 10으로 변하게 됩니다.
		// 4로 변하지 않는 이유는 부모는 2^i 번째만 저장하기 때문에 3번째 부모인 4는 저장되어 있지 않기 때문입니다.
		// 하지만, b가 10인 상태에서 i가 0까지 내려오기 때문에 b의 1번째(2^0) 부모인 4가 될 수 있습니다.
		for (int i = MAX_LEVEL; i >= 0; --i) 
			if(level[a] <= level[parents[b][i]])
				b = parents[b][i];
		
		// 위와 같이 4와 3의 경우처럼 깊이가 같으면서 공통 조상을 갖게 되므로 바로 종료하면 됩니다.
		if(b == a)
			return a;
		else {
			// 5번 조건
			// 깊이는 같지만 공통 조상이 아직 아닐경우
			// 위와 같이 최상단 노드 부터 탐색합니다.
			for (int i = MAX_LEVEL; i >= 0; --i) 
				// 깊이를 맞춰주는 부분과 비슷합니다. 최상단에 있을 수록 공통 조상(최소는 아닐수도)이 많기 때문에
				// 두 부모가 다를 때(최소 공통 조상 보다 깊이가 깊어짐) a와 b를 해당 부모로 교체하게 됩니다.
				// 하지만 위와 같이 결국 i가 0에 도달하기 때문에 a와 b는 최소 공통 조상의 바로 아래의 노드가 됩니다.(a와 b가 다를 때만 바꿔주기 때문)
				if(parents[a][i] != parents[b][i]) {
					a = parents[a][i];
					b = parents[b][i];
				}
					
			// a와 b가 최소 공통 조상의 바로 아래의 노드이기 때문에 1번째(2^0)부모가 최소 공통 조상입니다.
			return parents[a][0];
		}
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		StringBuilder sb = new StringBuilder();
		
		try {
			// 입력 시작
			int t = Integer.parseInt(br.readLine());
			
			for (int i = 0; i < t; i++) {
				n = Integer.parseInt(br.readLine());
				
				level = new int[n+1];
				parents = new int[n+1][MAX_LEVEL+1];
				adjacentNodes = new ArrayList[n+1];
				parentNode = new int[n+1];
				
				for (int j = 1; j <= n; j++)
					adjacentNodes[j] = new ArrayList<>();
				
				for (int j = 1; j <= n; j++) {
					String nodes = br.readLine();
					StringTokenizer stk = new StringTokenizer(nodes);
					
					int a = Integer.parseInt(stk.nextToken());
					int b = Integer.parseInt(stk.nextToken());
					// 입력 끝
					
					// 3번 조건
					// 간선은 n-1개고 n번째에서는 LCA를 찾을 두 노드를 입력하기 때문
					if(j == n) {
						setParent();
						System.out.println(LCA(a,b));
						break;
					}
					
					adjacentNodes[a].add(b);
					adjacentNodes[b].add(a);
					parentNode[b] = a;
				}
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.