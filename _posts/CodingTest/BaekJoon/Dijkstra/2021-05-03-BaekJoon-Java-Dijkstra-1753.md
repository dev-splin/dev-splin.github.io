---
title: "BaekJoon : 1753번(최단 경로)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Dijkstra"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Dijkstra

BaekJoon Dijkstra(다익스트라)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 1753

방향그래프가 주어지면 주어진 **시작점에서 다른 모든 정점으로의 최단 경로를 구하는 프로그램을 작성**하시오. 단, **모든 간선의 가중치는 10 이하의 자연수**이다.



### 입력

**첫째 줄에 정점의 개수 V와 간선의 개수 E**가 주어진다. **(1≤V≤20,000, 1≤E≤300,000)** 모든 정점에는 1부터 V까지 번호가 매겨져 있다고 가정한다. **둘째 줄에는 시작 정점의 번호 K(1≤K≤V)**가 주어진다. **셋째 줄부터 E개의 줄에 걸쳐 각 간선을 나타내는 세 개의 정수 (u, v, w)가 순서대로** 주어진다. 이는 **u에서 v로 가는 가중치 w인 간선이 존재한다는 뜻**이다. **u와 v는 서로 다르며 w는 10 이하의 자연수**이다. 서로 다른 두 정점 사이에 여러 개의 간선이 존재할 수도 있음에 유의한다.

```java
5 6		// 정점의 개수 V와 간선의 개수 E
1		// 시작 정점 번호
5 1 1	// 여기 부터
1 2 2
1 3 3
2 3 4
2 4 5
3 4 6	// 여기까지 u에서 v로가는 가중치 w인 간선 입력
```



### 출력

**첫째 줄부터 V개의 줄에 걸쳐, i번째 줄에 i번 정점으로의 최단 경로의 경로값을 출력**한다. **시작점 자신은 0으로 출력**하고, **경로가 존재하지 않는 경우에는 INF를 출력**하면 된다.

```java
0	
2
3
7
INF
```



### 다익스트라

하나의 정점에서 다른 모든 정점으로 가는 최단 경로를 구하는 문제이기 때문에 `Dijkstra(다익스트라)`를 사용해 문제를 해결할 수 있습니다.

**다익스트라 알고리즘은 다이나믹 프로그래밍을 활용한 대표적인 최단 경로 탐색 알고리즘**입니다.<br>다익스트라 알고리즘이 다이나믹 프로그래밍 문제인 이유는 **하나의 최단 거리를 구할 때 그 이전까지 구했던 최단 거리 정보를 그대로 사용**한다는 특징이 있기 때문입니다. 즉, 최단 거리는 여러 개의 최단 거리로 이루어져있는 것입니다.

간단하게 설명하자면, **최단 거리를 저장하는 배열과 인접 노드를 담은 리스트를 정점의 개수만큼 선언**하고<br>시작 노드의 **인접 노드 리스트를 순회하면서 검사**하게 되는데, 이 때, **최단 거리를 구해야 하기 때문에 제일 간선의 값이 작은 노드 부터 체크하면서 최단 거리를 저장하는 배열을 갱신**해줍니다.. (**우선 순위 큐**에 검사할 노드를 하나씩 넣으면 간선의 값이 작은 것 부터 체크하기 편합니다.)<br>

더 자세한 것은 코드의 주석을 보면서 설명하겠습니다.

다익스트라에 대한 더 자세한 설명은 [나동빈님 블로그](https://blog.naver.com/ndb796/221234424646)에서 확인할 수 있습니다.

### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class d_1753 {
	
	public static class Point implements Comparable<Point>{
		int v;
		int w;
		
		public Point() {
		}
		
		public Point(int v, int w) {
			this.v = v;
			this.w = w;
		}

		@Override
		public int compareTo(Point o) {
			return this.w - o.w;
		}
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		StringBuilder sb = new StringBuilder();
		
		final int INF = 10000000;
		
		try {
			StringTokenizer stk = new StringTokenizer(br.readLine());
			int v = Integer.parseInt(stk.nextToken());
			int e = Integer.parseInt(stk.nextToken());
			int k = Integer.parseInt(br.readLine());
			
			List<Point> list[] = new ArrayList[v+1];
			
			// 인접 노드를 리스트에 저장합니다.
			for (int i = 0; i < e; i++) {
				stk = new StringTokenizer(br.readLine());
				
				int u = Integer.parseInt(stk.nextToken());
				if(list[u] == null)
					list[u] = new ArrayList<>();
				
				Point point = new Point();
				point.v = Integer.parseInt(stk.nextToken());
				point.w = Integer.parseInt(stk.nextToken());
				
				list[u].add(point);
			}
			
			// 다익스트라는 가중치가 가장 적은 노드부터 방문하기 때문에 우선 순위 큐를 만드는데,
			// compareTo를 정의하여 가중치 기준 오름차순으로 해주었습니다.
			PriorityQueue<Point> pq = new PriorityQueue<>();
			
			// 시작노드를 만들고 큐에 넣습니다.
			Point point = new Point(k,0);
			pq.add(point);
			
			// 인덱스의 값이 "시작 노드 -> 해당 인덱스 노드로 갈 수있는 가중치 최소값"을 의미합니다.
			// 최소값이 새로 생기면 계속 갱신해줍니다.
			int d[] = new int[v+1];
			for (int i = 1; i <= v; i++)
				d[i] = INF;
			
			d[k] = 0;
			
			while(!pq.isEmpty()) {
				point = pq.poll();
				
				// 해당 노드로 이동하는 값이 최소값 보다 크거나 인접 리스트가 없다면 검사하지 않습니다.
				if(d[point.v] < point.w || list[point.v] == null)
					continue;
				
				for (int i = 0; i < list[point.v].size(); i++) {
					
					Point cmpPoint = list[point.v].get(i);
					
					int dist = point.w + cmpPoint.w;
					
					// point를 거쳐 cmpPoint에 도착할 때의 가중치(dist)가 최소값보다 작다면 갱신해주고 큐에 넣습니다.
					
					// 그리고 cmpPoint까지의 가중치 값을 dist로 바꿔줍니다. 
					// 그 이유는 해당 객체가 큐에서 나오게 될 때 해당 객체가 point가 될 것이고 해당 객체의 인접 리스트들이 cmpPoint가 되는데,
					// 이 때, 해당 객체까지 도달한 경로의 가중치 + cmpPoint의 가중치로 dist를 구하기 때문입니다.
					// "해당 객체 노드 번호"를 a라고 했을 때, d[a]와 point.w는 다릅니다.
					// d[a]는 모든 경로 중에서 a까지의 최소 경로의 가중치가 들어가게 되고,
					// point.w는 시작노드에서 point객체의 노드까지 도달한 경로의 가중치 값이기 때문입니다.  
					
					// dist가 최소값보다 크다면 굳이 큐에 넣어서 검사하지 않아도 됩니다.
					if(dist < d[cmpPoint.v]) {
						d[cmpPoint.v] = dist;
						cmpPoint.w = dist;
						pq.add(cmpPoint);		
					}
				}
			}
			
			for (int i = 1; i <= v; i++) {
				if(d[i] == INF)
					sb.append("INF\n");
				else
					sb.append(d[i]+"\n");
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



[https://blog.naver.com/ndb796/221234424646](https://blog.naver.com/ndb796/221234424646)