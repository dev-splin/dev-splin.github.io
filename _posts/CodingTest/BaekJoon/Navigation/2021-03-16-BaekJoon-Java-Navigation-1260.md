---
title: "BaekJoon : 1260번(DFS와 BFS)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Navigation"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Greedy

BaekJoon 탐색 저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 1260

그래프를 `DFS`로 탐색한 결과와 `BFS`로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, **방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문**하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. **정점 번호는 1번부터 N번**까지이다.



첫째 줄에 `정점의 개수 N(1 ≤ N ≤ 1,000)`, `간선의 개수 M(1 ≤ M ≤ 10,000)`, `탐색을 시작할 정점의 번호 V`가 주어진다. 다음 M개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.

```java
4 5 1		// 정점의 개수(N), 간선의 개수(M), 시작할 번호(V)를 입력합니다.
1 2
1 3
1 4
2 4
3 4			// 간선의 개수(M)만큼 간선을 이루는 두 번호를 입력합니다.
1 2 4 3		// DFS의 결과
1 2 3 4		// BFS의 결과를 출력합니다.
```



### 내코드

`DFS`는 `stack`을, `BFS`는 `queue`를 이용해 풀었습니다.

[예시로 보는 DFS와 BFS](https://dev-splin.github.io/cs(computer%20science)/datastructure/DataStructure-%EC%98%88%EC%8B%9C%EB%A1%9C-%EB%B3%B4%EB%8A%94-DFS%EC%99%80-BFS/) 여기의 방식과 비슷하게 풀었지만 하나 다른 점은 stack에 이미 노드가 있더라도 넣어주는 것 입니다. 그 이유는 `(1,2)(1,3)(1,4)(2,4)(3,4)`와 같은 경우에 stack에 있을 때 저장하지 않게 되면 `1->2->3->4` 순으로 찾게 되지만 (2에서 4로 바로 가지 않고 1로올라온 다음, 남아 있는 노드 중 작은 노드인 3부터 찾게 하는 방식입니다.)

문제의 출력결과는 `1->2->4->3`순으로 찾게 되기 때문입니다. (2에서 1로올라가지 않고 바로 4로 가는 방식입니다.)	

`DFS`는 재귀함수로도 풀 수 있는데,  재귀함수는 stack과 다르게 먼저 들어오는 노드를 함수로 호출하기 때문에 주의해주어야 합니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Stack;
import java.util.StringTokenizer;

public class Main {
	static InputStreamReader isr = new InputStreamReader(System.in);
	static BufferedReader br = new BufferedReader(isr);
	
	
	// 노드 클래스, 이인접 노드들을 리스트로 가지고 있습니다.
	public static class Node {
		int nodeNum;
		List<Node>  adjacentNodes;
		
		public Node(int num) {
			this.nodeNum = num;
			adjacentNodes = new ArrayList<>();
		}
		
		public int getNodeNum() {
			return nodeNum;
		}
		
		public void setNodeNum(int nodeNum) {
			this.nodeNum = nodeNum;
		}
		
		public void add(Node node) {
			adjacentNodes.add(node);
		}		
	}
	
	// 인접노드들을 넣어주는 함수입니다.
	public static void insertMainLine(Node[] nodes, int m) throws Exception {
		for (int i = 0; i < m; i++) {
			String nodeNums = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(nodeNums);
			
			int nodeNum1 = Integer.parseInt(stk.nextToken()) - 1;
			int nodeNum2 = Integer.parseInt(stk.nextToken()) - 1;
			
			nodes[nodeNum1].add(nodes[nodeNum2]);
			nodes[nodeNum2].add(nodes[nodeNum1]);
		}				
	}
	
	// stack을 이용한 DFS입니다. 
	public static List<Node> DFS(Node[] nodes, int v) {
		Stack<Node> stack = new Stack<>();
		Node startNode = nodes[v-1];
		List<Node> DFSNodes = new ArrayList<>();
		
		stack.add(startNode);
		
		while(!stack.empty()) {
			Node checkNode = stack.pop();
			
			// DFS리스트에 노드가 있으면 스택에 추가할지 말지 결정하는 부분을 건너 뜁니다.
			if(DFSNodes.contains(checkNode))
				continue;
			
			DFSNodes.add(checkNode);
			
			for(Node node : checkNode.adjacentNodes) {
				if(!DFSNodes.contains(node))
					stack.add(node);
			}
		}
				
		return DFSNodes;
	}
	
	// 재귀 함수를 이용한 DFS 구현입니다. 이미 탐색해서 리스트안에 있는 노드면 함수를 끝내주고
	// 없으면 리스트에 추가해준 후, 인접 노드들을 재귀를 이용해 체크해 줍니다.
	public static void recursionDFS(List<Node> nodes, Node startNode) {
		if(nodes.contains(startNode))
			return;
		
		nodes.add(startNode);
		
		for(Node node : startNode.adjacentNodes) {
			recursionDFS(nodes, node);
		}
	}

	// queue을 이용한 DFS입니다. 
	public static List<Node> BFS(Node[] nodes, int v) {	
		Queue<Node> queue = new LinkedList<>();
		Node startNode = nodes[v-1];
		List<Node> BFSNodes = new ArrayList<>();
		
		queue.add(startNode);
		
		while(!queue.isEmpty()) {
			Node checkNode = queue.poll();
			
			if(BFSNodes.contains(checkNode))
				continue;
			
			BFSNodes.add(checkNode);
			
			for(Node node : checkNode.adjacentNodes) {
				if(!BFSNodes.contains(node))
					queue.add(node);
			}
		}
		
		return BFSNodes;
	}
	
	public static void main(String[] args) {
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String nums = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(nums);
			
			int n = Integer.parseInt(stk.nextToken());
			int m = Integer.parseInt(stk.nextToken());
			int v = Integer.parseInt(stk.nextToken());
			
			Node[] nodes = new Node[n];
			
			for (int i = 0; i < n; i++) {
				nodes[i] = new Node(i+1);
			}
			
			insertMainLine(nodes,m);
			
			// 재귀함수를 이용한 
//			for(Node node : nodes)
//				Collections.sort(node.adjacentNodes,(a,b)->a.nodeNum-b.nodeNum);
//			List<Node> recusionNodes = new ArrayList<>();
//			recursionDFS(recusionNodes, nodes[v-1]);
			
			// DFS와 BFS 리스트를 반환 받을 변수입니다.
			List<Node> NavigationNodes;
			// DFS가 실행되기 전 인접 노드가 여러개 있을 때 작은 노드 부터 들어가야 하므로 내림차순으로 해주었습니다.
			// 내림차순으로 한 이유는 DFS는 스택을 이용하는데, 스택은 후입선출이기 때문에 마지막에 들어간 제일 작은 숫자가 제일 먼저 나오기 때문입니다. 
			for(Node node : nodes)
				Collections.sort(node.adjacentNodes,(a,b)->b.nodeNum-a.nodeNum);
			NavigationNodes = DFS(nodes,v);
			
			// DFS 출력(재귀함수를 사용할 땐, NavigationNodes를 recusionNodes로 바꿔주어야 합니다.)
			for(Node node : NavigationNodes) {
				bw.write(Integer.toString(node.nodeNum) + " ");
			}
			bw.newLine();
			
			// BFS의 큐는 선입 선출이기 때문에 오름차순으로 인접노드들을 정렬해주었습니다.
			for(Node node : nodes)
				Collections.sort(node.adjacentNodes,(a,b)->a.nodeNum-b.nodeNum);
			NavigationNodes = BFS(nodes,v);
			
			// BFS 출력
			for(Node node : NavigationNodes) {
				bw.write(Integer.toString(node.nodeNum) + " ");
			}
			bw.newLine();
			
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.