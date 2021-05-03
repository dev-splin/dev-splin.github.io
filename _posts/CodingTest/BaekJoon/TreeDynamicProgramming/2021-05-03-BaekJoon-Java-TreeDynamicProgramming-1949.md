---
title: "BaekJoon : 1949번(우수 마을)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Tree Dynamic Programming"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Tree Dynamic Programming

BaekJoon Tree Dynamic Programming(동적 프로그래밍)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 1949

N개의 마을로 이루어진 나라가 있다. 편의상 **마을에는 1부터 N까지 번호**가 붙어 있다고 하자. 이 나라는 트리(Tree) 구조로 이루어져 있다. 즉 **마을과 마을 사이를 직접 잇는 N-1개의 길**이 있으며, 각 **길은 방향성이 없어서 A번 마을에서 B번 마을로 갈 수 있다면 B번 마을에서 A번 마을로 갈 수 있다.** 또, **모든 마을은 연결되어 있다.** 두 마을 사이에 직접 잇는 길이 있을 때, 두 마을이 인접해 있다고 한다.

이 나라의 주민들에게 성취감을 높여 주기 위해, 다음 세 가지 조건을 만족하면서 **N개의 마을 중 몇 개의 마을을 '우수 마을'로 선정**하려고 한다.

1. **'우수 마을'로 선정된 마을 주민 수의 총 합을 최대**로 해야 한다.
2. 마을 사이의 충돌을 방지하기 위해서, 만일 두 마을이 인접해 있으면 두 마을을 모두 '우수 마을'로 선정할 수는 없다. 즉 **'우수 마을'끼리는 서로 인접해 있을 수 없다.**
3. 선정되지 못한 마을에 경각심을 불러일으키기 위해서, **'우수 마을'로 선정되지 못한 마을은 적어도 하나의 '우수 마을'과는 인접**해 있어야 한다.

각 마을 주민 수와 마을 사이의 길에 대한 정보가 주어졌을 때, 주어진 조건을 만족하도록 '우수 마을'을 선정하는 프로그램을 작성하시오.



### 입력

**첫째 줄에 정수 N**이 주어진다. **(1≤N≤10,000)** **둘째 줄에는 마을 주민 수를 나타내는 N개의 자연수**가 빈칸을 사이에 두고 주어진다. **1번 마을부터 N번 마을까지 순서대로 주어지며, 주민 수는 10,000 이하**이다. **셋째 줄부터 N-1개 줄에 걸쳐서 인접한 두 마을의 번호**가 빈칸을 사이에 두고 주어진다.

```java
7		// 정수 N이 주어집니다.
1000 3000 4000 1000 2000 2000 7000	// 마을의 주민 수
1 2		// 여기부터
2 3
4 3
4 5
6 2
6 7		// 여기까지 N-1개의 인접한 두 마을의 번호
```



### 출력

첫째 줄에 '우수 마을'의 주민 수의 총 합을 출력한다.

```java
14000
```



### Tree DP

**Tree를 이용한 Dynamic Programming** 입니다. 

문제의 마을이 `Tree` 구조로 되어 있다고 했기 때문에, `Dynamic Programming`의 특징인 하나의 문제를 여러 작은 문제로 나누는 방법처럼, **하나의 큰 Tree를 여러 작은 Tree로 나눌 수 있습니다.** 그렇기 때문에 **Tree에서 부모 노드와 자식노드 간의 관계만 잘 설정**해주면 문제를 해결할 수 있습니다.

유의할 조건은  밑의 두 가지 조건입니다.

- **'우수 마을'끼리는 서로 인접해 있을 수 없다.**

- **'우수 마을'로 선정되지 못한 마을은 적어도 하나의 '우수 마을'과는 인접**해 있어야 한다.

이 두 가지 조건을 만족하면서, **마을 주민 수가 최대**가 되게 해야합니다.  **핵심은 부모 노드가 우수마을 일 때와 우수마을이 아닐 때를 나누어 memoization에 저장**하는 것입니다. 이 때, **Tree의 리프노드부터 검사해야하기 때문에 DFS를 이용**합니다.

`memo[a][0]`은 a노드가 우수마을이 아닐 때, `memo[a][1]`은 a노드가 우수마을 일 때를 나타냅니다.



#### 부모노드가 우수마을 일 때

부모노드가 우수마을 일 때는 바로 위의 첫 번째 조건(지문의 1,2,3번 과는 다릅니다.)을 만족해야 하기 때문에 자식노드는 우수마을이 올 수 없기 때문에 자식 노드가 우수마을이 아닌 경우와 부모노드 자신의 주민수를 더해줍니다.

```java
memo[cur][1] += memo[child][0];		// 부모노드 자신은 주민수를 입력받을 때 memo[][1]에 초기화 해주었습니다.
```



#### 부모노드가 우수마을이 아닐 때

부모노드가 우수마을 아닐 때는 두 번째 조건을 만족해야 하기 때문에 자식노드는 우수마을이거나 우수마을이 아닌 경우 중 최대 값을 더해줍니다.

이 때, `자식마을 중 우수마을이 없는 경우는 어떻게 처리할까?` 라는 의문이 들 수 있습니다. 하지만 리프노드부터 처리해주게 되면 리프노드(`memo[leaf][1]`)가 제일 큰 주민수를 가지고 있다고 하더라도 부모노드가 우수마을(`memo[parent][0]`)이 아닌 경우에서 리프노드를 포함하게 되고, 이 부모노드의 부모노드 즉, 조부모 노드(`memo[grand][1]`)가 우수마을인 경우에서 부모노드를 포함하게 되기 때문에 **굳이 없는 경우를 처리해 주지 않아도 자식마을 중 우수마을이 없는 경우는 나오지 않습니다.**



### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	static Integer memo[][];
	static List<Integer> childs[];
	
	// dfs를 이용해 리프노드부터 체크합니다.
	public static void dfs(int cur, int parent) {
		
		for(int child : childs[cur]) {
			// 리스트의 인접 노드중 부모노드가 나오면 건너뜁니다.
			if(child == parent)
				continue;
			
			dfs(child, cur);
			
			// 현재 노드가 우수마을이 아닌 경우에는 자식 노드 중 더 큰 값을 더해줍니다.
			memo[cur][0] += Math.max(memo[child][0], memo[child][1]);
			// 현재 노드가 우수마을인 경우는 자식 노드 중 우수마을이 아닌 경우만 더해줍니다.
			memo[cur][1] += memo[child][0];
		}
	}
		
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			int n = Integer.parseInt(br.readLine());
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			memo = new Integer[n+1][2];
			childs = new ArrayList[n+1];
			
			// 인접노드를 저장할 리스트를 초기화하면서 memo[][1] 즉, 우수마을인 경우에는 그 마을의 주민수를 넣어놓습니다.
			for (int i = 1; i <= n; i++) {
				memo[i][0] = 0;
				memo[i][1] = Integer.parseInt(stk.nextToken());
				childs[i] = new ArrayList<>();
			}
			
			// 리스트에 노드 추가
			for (int i = 0; i < n-1; i++) {
				stk = new StringTokenizer(br.readLine());
				int node1 = Integer.parseInt(stk.nextToken());
				int node2 = Integer.parseInt(stk.nextToken());
				childs[node1].add(node2);
				childs[node2].add(node1);
			}
			
			dfs(1,0);
			
			System.out.println(Math.max(memo[1][0], memo[1][1]));
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

