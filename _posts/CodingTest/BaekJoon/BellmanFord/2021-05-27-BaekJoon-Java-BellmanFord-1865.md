---
title: "BaekJoon : 1865번 (웜홀)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Bellman Ford"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Bellman Ford

BaekJoon `Bellman Ford(벨만 포드)`  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 1865

때는 2020년, 백준이는 월드나라의 한 국민이다. **월드나라에는 N개의 지점**이 있고 **N개의 지점 사이에는 M개의 도로와 W개의 웜홀**이 있다. (**단 도로는 방향이 없으며 웜홀은 방향이 있다.**) **웜홀은 시작 위치에서 도착 위치로 가는 하나의 경로**인데, 특이하게도 **도착을 하게 되면 시작을 하였을 때보다 시간이 뒤로** 가게 된다. 웜홀 내에서는 시계가 거꾸로 간다고 생각하여도 좋다.

시간 여행을 매우 좋아하는 백준이는 한 가지 궁금증에 빠졌다. **한 지점에서 출발을 하여서 시간여행을 하기 시작하여 다시 출발을 하였던 위치로 돌아왔을 때, 출발을 하였을 때보다 시간이 되돌아가 있는 경우가 있는지 없는지** 궁금해졌다. 여러분은 백준이를 도와 이런 일이 가능한지 불가능한지 구하는 프로그램을 작성하여라.



### 입력

**첫 번째 줄에는 테스트케이스의 개수 TC(1 ≤ TC ≤ 5)**가 주어진다. 

그리고 **두 번째 줄부터 TC개의 테스트케이스**가 차례로 주어지는데 **각 테스트케이스의 첫 번째 줄에는 지점의 수 N(1 ≤ N ≤ 500), 도로의 개수 M(1 ≤ M ≤ 2500), 웜홀의 개수 W(1 ≤ W ≤ 200)**이 주어진다. 

그리고 **두 번째 줄부터 M+1번째 줄에 도로의 정보**가 주어지는데 각 도로의 정보는 **S, E, T 세 정수**로 주어진다. **S와 E는 연결된 지점의 번호, T는 이 도로를 통해 이동하는데 걸리는 시간을 의미**한다. 

그리고 **M+2번째 줄부터 M+W+1번째 줄까지 웜홀의 정보**가 **S, E, T 세 정수**로 주어지는데 **S는 시작 지점, E는 도착 지점, T는 줄어드는 시간을 의미**한다. **T는 10,000보다 작거나 같은 자연수 또는 0**이다.

**두 지점을 연결하는 도로가 한 개보다 많을 수도 있다.** 지점의 번호는 1부터 N까지 자연수로 중복 없이 매겨져 있다.

```java
2		// 테스트 케이스(TC)
3 3 1	// 지점의 수(N), 도로의 개수(M), 웜홀의 개수(W)
1 2 2	// 여기 부터 도로의 개수 만큼
1 3 4
2 3 1	// 여기 까지 연결 지점의 번호(S,E), 시간(T)
3 1 3	// 웜홀의 개수 만큼(이 TC에서는 1) 시작 지점(S), 도착 지점(E), 시간(T)
3 2 1	// 두 번째 테스트 케이스 시작
1 2 3
2 3 4
3 1 8
```



### 출력

TC개의 줄에 걸쳐서 만약에 시간이 줄어들면서 **출발 위치로 돌아오는 것이 가능하면 YES, 불가능하면 NO를 출력**한다.

```java
NO
YES
```



### 플로이드 와샬

벨만 포드 문제이지만, **모든 정점에서 모든 정점으로 이동하는 최소 거리를 구하는 플로이드 와샬로 풀 수는 있습니다.** (다만 시간이 아슬 아슬하기 때문에 벨만 포드를 추천합니다..!!)

보통 플로이드 와샬을 이용할 때, 초기에 모든 정점으로 이동하는 경우를 INF로 초기화할 때, 0번 정점이 0번 정점 즉, 자기 자신으로 가는 경우를 0으로 초기화 시켜주는데, **이 문제 같은 경우에는 자기 자신으로 되돌아 가야하기 때문에 그냥 모든 정점을 INF로 초기화** 시켜준 후 플로이드 와샬을 적용하면 됩니다.



```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	static final int INF = 1000000000;
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			// 테스트 케이스 입력
			int tc= Integer.parseInt(br.readLine());
			
            // 입력 시작
			for (int i = 0; i < tc; i++) {
				StringTokenizer stk = new StringTokenizer(br.readLine());
				
				int n = Integer.parseInt(stk.nextToken());
				int m = Integer.parseInt(stk.nextToken());
				int w = Integer.parseInt(stk.nextToken());
				
				int arr[][] = new int[n+1][n+1];
				
				// 자기자신으로 돌아오는 경우도 구해야 하기 때문에 모두 INF로 초기화
				for (int j = 1; j <= n; j++)
					for (int k = 1; k <= n; k++)
						arr[j][k] = INF;
				
				// 도로
				for (int j = 0; j < m; j++) {
					stk = new StringTokenizer(br.readLine());
					
					int node1 = Integer.parseInt(stk.nextToken());
					int node2 = Integer.parseInt(stk.nextToken());
					int time = Integer.parseInt(stk.nextToken());
					
					// 두 지점을 연결하는 도로가 두 개 이상일 수도 있기 때문에 시간이 작은 경우만 넣어줍니다.
					if(arr[node1][node2] < time)
						continue;
					
					arr[node1][node2] = time;
					arr[node2][node1] = time;
				}
				
				// 웜홀은 시간이 줄어들기 때문에 -로 넣어줍니다.
				for (int j = 0; j < w; j++) {
					stk = new StringTokenizer(br.readLine());
					
					int node1 = Integer.parseInt(stk.nextToken());
					int node2 = Integer.parseInt(stk.nextToken());
					int time = Integer.parseInt(stk.nextToken());
					arr[node1][node2] = -time;
				}
				// 입력 끝
				
				// 플로이드 와샬
				for (int k = 1; k <= n; k++)
					for (int j = 1; j <= n; j++)
						for (int l = 1; l <= n; l++)
							if(arr[j][l] > arr[j][k] + arr[k][l])
								arr[j][l] = arr[j][k] + arr[k][l];
				
				boolean isPossible = false;
				
				// 자기자신으로 돌아가는 경우 중 0보다 작은 경우가 있으면 시간이 되돌아가 있는 경우
				for (int j = 1; j <= n; j++)
					if(arr[j][j] < 0)
						isPossible = true;
				
				if(isPossible)
					System.out.println("YES");
				else
					System.out.println("NO");
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 벨만 포드

벨만 포드 알고리즘은 다익스트라와 비슷하지만 다익스트라에서 할 수 없었던 음의 가중치도 계산 할 수 있습니다. 즉 **이 문제에서 웜홀은 음의 가중치를 가지기 때문에 벨만 포드 알고리즘이 적합**한 것입니다. 

벨만 포드 알고리즘을 통해 최단 거리를 구하는 방식은 계속해서 모든 간선을 이용하여 a정점에서 b정점으로 갈 때, **거리가 짧아지는 경우가 생긴다면 계속 업데이트를 해주는 방식**입니다. 즉, **V(정점) * E(간선)번 반복후 종료**됩니다.

다익스트라와 다른 점은 **다익스트라는 이제 까지 왔던 길(DP)을 이용**한다면, **벨만 포드는 현재 까지 검사한 경우 중 가장 짧은 경로를 이용**하는 것입니다.



#### 전제 조건

1. 최단 경로는 사이클을 포함할 수 없기 때문에, **최대 |V|-1개의 간선**만 사용할 수 있습니다. 
   - 3개의 정점 1,2,3이 있다면 1에서 3으로 가는 최단 경로 중 최대로 간선을 사용할 수 있는 경우는 2개이기 때문입니다.

2. 최단 거리가 업데이트 되는 정점이 없어질 때 까지 계속해서 반복(`모든 정점을 간선의 수 만큼 반복`)하여 구해주고 **음의 가중치로 인해 업데이트를 무한히 하게 되는 경우 탈출** 시켜주어야 합니다.



2번 조건에서 **음의 가중치로 인해 업데이트를 무한히 하게 되는 경우라 함은 사이클이 존재**한다는 의미입니다.

만약 3개의 정점 1,2,3의 간선을 `정점 -> 정점(시간)`으로 표현하고 **1에서 3으로가는 최단 경로를 구한다고 가정**해 보겠습니다.

 `1 -> 2(3)`, `2 -> 3(5)`, `3 -> 1(-1)` 같은 경우는 사이클과 음의 가중치가 존재해도 **사이클의 총 합이 7(양수) 이기 때문에 1에서 3으로가는 최단 경로 8에서 더 이상 업데이트하지 않지만**,<br> `1 -> 2(3)`, `2 -> 3(-4)`, `3 -> 1(-1)` 같은 경우는 **사이클의 총 합이 -2(음수) 이기 때문에 1에서 3으로가는 최단 경로 -1에서 사이클의 총 합(-2)를 연산하게 되면 더 짧은 경로가 나오게 되기 때문에 무한히 업데이트**하게 됩니다. 

즉, **사이클의 총 합이 음수일 경우 정점(V) * 간선(E)번 반복하면서 최단 경로를 최종적으로 업데이트 해도 계속 더 짧은 경로가 나오게 되는 것**입니다.



```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	static final int INF = 1000000000;
	static List<Edge> edges;
	
	public static class Edge {
		int start;
		int end;
		int time;
		
		public Edge(int start, int end, int time) {
			this.start = start;
			this.end = end;
			this.time = time;
		}
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			// 테스트 케이스 입력
			int tc= Integer.parseInt(br.readLine());
			
			// 입력 시작
			for (int i = 0; i < tc; i++) {
				StringTokenizer stk = new StringTokenizer(br.readLine());
				
				int n = Integer.parseInt(stk.nextToken());
				int m = Integer.parseInt(stk.nextToken());
				int w = Integer.parseInt(stk.nextToken());
				
				edges = new ArrayList<>();
				
				// 도로
				for (int j = 0; j < m; j++) {
					stk = new StringTokenizer(br.readLine());
					
					int node1 = Integer.parseInt(stk.nextToken());
					int node2 = Integer.parseInt(stk.nextToken());
					int time = Integer.parseInt(stk.nextToken());
					
					// 모든 간선을 저장
					edges.add(new Edge(node1, node2, time));
					edges.add(new Edge(node2, node1, time));
				}
				
				// 웜홀은 시간이 줄어들기 때문에 -로 넣어줍니다.
				for (int j = 0; j < w; j++) {
					stk = new StringTokenizer(br.readLine());
					
					int node1 = Integer.parseInt(stk.nextToken());
					int node2 = Integer.parseInt(stk.nextToken());
					int time = Integer.parseInt(stk.nextToken());
					
					edges.add(new Edge(node1, node2, -time));
				}
				
				// 최단 거리를 저장할 배열
				int d[] = new int[n+1];
				
				Arrays.fill(d, INF);
				
				// 최단 거리 구하기
				for (int j = 1; j <= n; j++)
					for(Edge e : edges)
						d[e.end] = Math.min(d[e.end], d[e.start] + e.time);
				
				boolean isPossible = false;
				
				// 현재 d에 최단 거리가 들어가 있지만, 간선을 순회하면서
				// 간선의 도착 정점까지의 최단거리(d[end])가 
				// 간선의 시작 정점까지의 최단거리(d[start]) + 간선의 가중치 보다 작다면,
				// 즉 업데이트가 된다면, 음의 사이클을 가지게 됩니다.(시간이 되돌아가는 경우가 있다는 것)
				for(Edge e : edges)
					if(d[e.end] > d[e.start] + e.time) {
						isPossible = true;
						break;
					}
				
				if(isPossible)
					System.out.println("YES");
				else
					System.out.println("NO");
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

참고 : [https://www.crocus.co.kr/534?category=209527](https://www.crocus.co.kr/534?category=209527)

[https://ratsgo.github.io/data%20structure&algorithm/2017/11/27/bellmanford/](https://ratsgo.github.io/data%20structure&algorithm/2017/11/27/bellmanford/)

