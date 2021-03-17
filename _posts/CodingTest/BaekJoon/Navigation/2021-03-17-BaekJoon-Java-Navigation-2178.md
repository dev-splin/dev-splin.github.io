---
title: "BaekJoon : 2178(미로 탐색)"
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

# Java : BaekJoon Navigation

BaekJoon Navigation(탐색)  저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 2178

N×M크기의 배열로 표현되는 미로가 있다.

| 1     | 0     | 1     | 1     | 1     | 1     |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **1** | **0** | **1** | **0** | **1** | **0** |
| **1** | **0** | **1** | **0** | **1** | **1** |
| **1** | **1** | **1** | **0** | **1** | **1** |

미로에서 1은 이동할 수 있는 칸을 나타내고, 0은 이동할 수 없는 칸을 나타낸다. 이러한 미로가 주어졌을 때, **(1, 1)에서 출발하여 (N, M)의 위치로 이동할 때 지나야 하는 최소의 칸 수를 구하는 프로그램**을 작성하시오. **한 칸에서 다른 칸으로 이동할 때, 서로 인접한 칸으로만 이동**할 수 있다.

위의 예에서는 15칸을 지나야 (N, M)의 위치로 이동할 수 있다. 칸을 셀 때에는 시작 위치와 도착 위치도 포함한다.



첫째 줄에 두 정수 **N, M(2 ≤ N, M ≤ 100)**이 주어진다. 다음 N개의 줄에는 M개의 정수로 미로가 주어진다. 각각의 수들은 **붙어서** 입력으로 주어진다.

```java
4 6		// 미로의 크기 n,m을 입력합니다
101111
101010
101011
111011	// 각각의 수들을 붙여서 미로를 만듭니다.
15		// (n,m)의 위치로 이동할 수 있는 최단 거리를 출력합니다
```



### 방법1

최단 거리를 구하는 것이기 때문에 하나를 깊숙히 탐색하는 `DFS`보다는 **레벨 단위로 체크를 해서 레벨의 깊이를 이용해 거리를 구할 수 있는 `BFS`를 이용**해야 합니다.  **Node 클래스를 만들고 인접리스트로 `BFS`를 수행하면서 깊이를 체크하는 방식**으로 풀었습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
	static InputStreamReader isr = new InputStreamReader(System.in);
	static BufferedReader br = new BufferedReader(isr);
	
	// 행과열정보, 길인지 벽인지 체크하는 변수, 인접노드 리스트를 가지고 있는 노드 클래스
	public static class Node {
		boolean checkRoad;
		int row;
		int col;
		List<Node> adjacentNodes;
		
		public Node(int row, int col, boolean checkRoad) {
			adjacentNodes = new ArrayList<>();
			this.row = row;
			this.col = col;
			this.checkRoad = checkRoad;
		}
		
		public void add(Node node) {
			adjacentNodes.add(node);
		}
	}
	
	// 미로를 만들어주면서 노드 클래스 인접노드 리스트에 노드를 넣어줍니다.
	public static Node[][] MakeMaze(int n, int m) throws Exception {
		Node maze[][] = new Node[n][m];
		
		for (int i = 0; i < maze.length; i++) {
			char wallsOrRoads[] = br.readLine().trim().toCharArray();  
			
			for (int j = 0; j < wallsOrRoads.length; j++) {
				boolean checkRoad = false;
				if(wallsOrRoads[j] == '1')
					checkRoad = true;
					
				maze[i][j] = new Node(i, j, checkRoad);
				
				// i 즉, 행이 0이 아니면 현재 행열 위에 있는 길 노드를 인접노드에 저장합니다.
				// 위에만 저장한 이유는 인접리스트에 서로 추가해 주기 때문입니다. 
				if(i != 0)
					if(maze[i-1][j].checkRoad && maze[i][j].checkRoad) {
						maze[i-1][j].add(maze[i][j]);
						maze[i][j].add(maze[i-1][j]);
					}
				
				// j 즉, 열이 0이 아니면 현재 행열 왼쪽에 있는 길 노드를 인접노드에 저장합니다.
				if(j != 0)
					if(maze[i][j-1].checkRoad && maze[i][j].checkRoad) {
						maze[i][j-1].add(maze[i][j]);
						maze[i][j].add(maze[i][j-1]);
					}
			}
		}
		return maze;
	}
	
	// Node클래스안에 인접리스트를 이용한 BFS입니다. int형을 반환하는데, 이 때 반환 값은 depth를 의미합니다.
	public static int BFS(Node startNode,Node findNode,int n, int m) {
		boolean checkNodes[][] = new boolean[n][m];
		
		Queue<Node> queue = new LinkedList<>();
		queue.add(startNode);
		checkNodes[startNode.row][startNode.col] = true;	
		Node checkNode; 
		
		// depth를 체크하기 위한 변수 처음에 startNode가 들어갔기 때문에 1로시작합니다.
		int depth = 1;
		// 한 레벨에서 체크하지 않은 노드의 수 입니다.
		int countNodesInLevel = 1;
		// 현재 레벨의 다음레벨의 노드 수를 저장합니다.
		int countAdjacentNodes = 0;
		
		while(!queue.isEmpty()) {
			checkNode = queue.poll();	
			// 큐에서 하나를 빼면서 체크할 것이기 때문에 --를 해줍니다.
			--countNodesInLevel;
					
			if(checkNode == findNode) {
				break;
			}

			// 체크노드의 인접노드들을 순회하면서 체크되지 않은 노드들은 큐에 넣어주고 체크해줍니다.
			// 이 때, 큐에 넣는 순간 countAdjacentNodes값을 ++ 해줍니다.
			// 이렇게 해주는 이유는 특정 레벨의 다음 레벨의 수를 구해주기 위함입니다.
			for(Node node : checkNode.adjacentNodes) {
				if(!checkNodes[node.row][node.col]) {
					queue.add(node);
					checkNodes[node.row][node.col] = true;
					++countAdjacentNodes;
				}
			}
			
			// countNodesInLevel이 0이 될 때, 즉 현재 레벨을 다 체크하였으면 다음레벨의 노드 수를 넣어주고
			// 다음 레벨의 노드수는 0으로 다시 초기화하고 depth를 1올려줍니다.
			if(countNodesInLevel == 0) {
				countNodesInLevel = countAdjacentNodes;
				countAdjacentNodes = 0;
				++depth;
			}
		}
		return depth;
	}
	
	public static void main(String[] args) {
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String mazeSize = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(mazeSize);
			
			int n = Integer.parseInt(stk.nextToken());
			int m = Integer.parseInt(stk.nextToken());
			
			Node maze[][] = MakeMaze(n, m);
			
			int result = BFS(maze[0][0],maze[n-1][m-1], n, m);
			
			bw.write(Integer.toString(result));
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 방법2

**미로 행렬의 값을 레벨로 표시해서 값을 구하는 방식**입니다. `BFS`에서 상하좌우를 체크해서 인접행렬을 체크하고 다음레벨의 행렬의 값을 올려주면서 **해당행렬의 값이 레벨(깊이)**을 가지게 해서 최단거리를 구합니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
	static InputStreamReader isr = new InputStreamReader(System.in);
	static BufferedReader br = new BufferedReader(isr);
	// static 함수에서 사용할 수 있게 static으로 정의하였습니다.
	static int maze[][];
	static boolean check[][];
	static int n,m;
		
	// 미로를 입력 받고 만들어줄 함수
	public static void MakeMaze() throws Exception {
		maze = new int[n][m];
		check = new boolean[n][m];
		
		for (int i = 0; i < maze.length; i++) {
			char wallsOrRoads[] = br.readLine().trim().toCharArray();  
			
			for (int j = 0; j < wallsOrRoads.length; j++) {
					maze[i][j] = wallsOrRoads[j] - 48;
			}
		}
	}
	
	// BFS 함수. 찾는 행열의 레벨(최단 거리)를 반환해줍니다. 
	// 다음 레벨로 가면 미로안의 숫자를 +1 해서 레벨 표시를 해주는 방식입니다.
	// 예를들어 (0,0)부터 시작하고 (1,0)이 갈수 있는 미로(1)이면
	//  maze[1][0] = maze[0][0] + 1 을 해주어서 레벨 2라고 표현해 주는 것입니다.
	public static int BFS(int findRow, int findCol) {
		// 미로의 상하좌우를 체크할 때 사용할 배열 입니다.
		int dirRow[] = {-1, 1, 0, 0};
		int dirCol[] = {0, 0, -1, 1};
		
		Queue<Integer> rowQueue = new LinkedList<>();
		Queue<Integer> colQueue = new LinkedList<>();
		
		rowQueue.add(0);
		colQueue.add(0);
		check[0][0] = true;
		
		while(!rowQueue.isEmpty()) {
			
			int row = rowQueue.poll();
			int col = colQueue.poll();
			
			// 상하좌우를 체크해줍니다.
			for (int i = 0; i < dirRow.length; i++) {
				
				int tmpRow = row + dirRow[i];
				int tmpCol = col + dirCol[i];
				
				// 범위 안의 미로만 체크할 수 있게 하는 것입니다. 
				// 예를 들어, (-1,0) 이나 행이나 열의 값이 n,m을 넘지 않게 하는 것입니다.
				if(tmpRow >= 0 && tmpCol >= 0
						&& tmpRow < n && tmpCol < m) {
					// 아직 체크되지 않는 행열이고 갈 수 있는 미로면 큐에 넣어주고 체크 후, 레벨을 올려줍니다.
					if(!check[tmpRow][tmpCol] && maze[tmpRow][tmpCol] == 1) {
						
						rowQueue.add(tmpRow);
						colQueue.add(tmpCol);
						
						check[tmpRow][tmpCol] = true;
						maze[tmpRow][tmpCol] = maze[row][col] + 1;
					}
				}
				
			}
		}
		return maze[findRow][findCol];
	}
	
	public static void main(String[] args) {
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String mazeSize = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(mazeSize);
			
			n = Integer.parseInt(stk.nextToken());
			m = Integer.parseInt(stk.nextToken());
			
			MakeMaze();
			
			int result = BFS(n-1,m-1);
			
			bw.write(Integer.toString(result));
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