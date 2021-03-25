---
title: "BaekJoon : 21278(호석이 두 마리 치킨)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Floyd Warshall"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Floyd Warshall

BaekJoon Floyd Warshall(플로이드 와샬)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 21278

컴공 출신은 치킨집을 하게 되어있다. 현실을 부정하지 말고 받아들이면 마음이 편하다. 결국 호석이도 2050년에는 치킨집을 하고 있다. 치킨집 이름은 "호석이 두마리 치킨"이다.

이번에 키친 도시로 분점을 확보하게 된 호석이 두마리 치킨은 **도시 안에 2개의 매장**을 지으려고 한다. **도시는 *N* 개의 건물과 *M* 개의 도로**로 이루어져 있다. **건물은 1번부터 *N*번의 번호**를 가지고 있다. **i 번째 도로는 서로 다른 두 건물 *Ai* 번과 *Bi* 번 사이를 1 시간에 양방향으로 이동할 수 있는 도로**이다.

키친 도시에서 2개의 건물을 골라서 치킨집을 열려고 한다. 이 때 아무 곳이나 열 순 없어서 **모든 건물에서의 접근성의 합을 최소화**하려고 한다. 건물 *X* 의 접근성은 *X* 에서 가장 가까운 호석이 두마리 치킨집까지 왕복하는 최단 시간이다. 즉, **"모든 건물에서 가장 가까운 치킨집까지 왕복하는 최단 시간의 총합"을 최소화할 수 있는 건물 2개를 골라**서 치킨집을 열려고 하는 것이다.

컴공을 졸업한 지 30년이 넘어가는 호석이는 이제 코딩으로 이 문제를 해결할 줄 모른다. 알고리즘 퇴물 호석이를 위해서 최적의 위치가 될 수 있는 건물 2개의 번호와 그 때의 "모든 건물에서 가장 가까운 치킨집까지 왕복하는 최단 시간의 총합"을 출력하자. 만약 이러한 건물 조합이 여러 개라면, 건물 번호 중 작은 게 더 작을수록, 작은 번호가 같다면 큰 번호가 더 작을수록 좋은 건물 조합이다.



### 입력

**첫 번째 줄에 건물의 개수 *N*과 도로의 개수 *M*** 이 주어진다. **이어서 *M* 개의 줄에 걸쳐서 도로의 정보 *Ai , Bi* 가 공백으로 나뉘어서 주어진다.** 같은 **도로가 중복되어 주어지는 경우는 없다.** 어떤 두 건물을 잡아도 도로를 따라서 오고 가는 방법이 존재함이 보장된다.

```java
5 4		// 건물의 개수(N)과 도로의 개수(M)을 입력하고
1 3
4 2
2 5
3 2		// M개의 도로를 입력합니다.
```



### 출력

**한 줄에 건물 2개가 지어질 건물 번호를 오름차순으로 출력**하고, 그때 **모든 도시에서의 왕복 시간의 합을 출력**한다.

만약 건**물 조합이 다양하게 가능하면, 작은 번호가 더 작은 것을, 작은 번호가 같다면 큰 번호가 더 작은 걸 출력**한다.

```java
1 2 6	// 치킨집 번호 2개가 오름차순으로 출력되고 치킨집에서 모든 도시까지 왕복 시간의 합을 출력합니다.
```



### 방법1. 너비 우선 탐색(BFS)

[7576번(토마토)](https://dev-splin.github.io/coding%20test/baekjoon/BaekJoon-Java-Navigation-7576/)의 방식과 비슷하게 풀었습니다. **전체 번호를 순회하면서 겹치지 않는 두 개의 건물을 치킨집이라고 가정**하고 다른 건물들과의 거리가 제일 작은 경우를 찾습니다.

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
import java.util.StringTokenizer;

public class Main {
	static Building buildings[];
	
	// 빌딩 클래스 인접 건물번호를 리스트로 가지고 있습니다.
	public static class Building {
		List<Integer> adjacentNums = new ArrayList<>();
	}
	
	// 마지막에 출력해주기 위한 치킨 클래스
	public static class Chicken {
		int count;
		int minNumChicken;
		int maxNumChicken;
		
		@Override
		public String toString() {
			return minNumChicken + " " + maxNumChicken + " " + (count * 2);
		}
	}
	
	// 두개의 건물번호를 받아 BFS를 실행하고 
	// 깊이들을 더 해 입력받은 두 건물과 모든 건물들과의 거리 총합을 반환합니다.  
	public static int BFS(int building1, int building2) {
		
		int length = buildings.length;
		boolean check[] = new boolean[length];
		
		Queue<Building> queue = new LinkedList<>();
		
		int depth = 1;
		// 입력받은 두 건물과 모든 건물들과의 거리 총합을 저장할 변수
		int count = 0;
		
		// 처음에 루트 노드(입력받은 두 개의 건물번호)를 큐에 넣어놓습니다.
		queue.add(buildings[building1]);
		queue.add(buildings[building2]);
		check[building1] = true;
		check[building2] = true;
		// 현재 레벨에 체크안한 개수가 몇개인지 담을 변수
		int countInLevel = 2;
		// 같은 레벨의 인접 리스트 즉, 다음 레벨의 노드 개수를 저장할 변수
		int countAdjacent = 0;
		
		// 큐에서 노드를 하나씩 빼면서 체크해서 거리 총합을 구합니다.
		while(!queue.isEmpty()) {
			Building building = queue.poll();
			
			--countInLevel;
						
			// 노드의 체크하지 않은 인접리스트들을 큐에 넣어줍니다.
			// 큐에 넣을 때마다 countAdjacent을 1씩 누적합니다.(현재 레벨의 체크가 끝날 때의 countAdjacent값이 다음 레벨 노드의 개수가 됩니다.)
			// 거리 총합을 구해야 되기 때문에 깊이(루트 노드와의 거리)를 count에 누적해줍니다.
			for(int adjacentNum : building.adjacentNums) {
				
				if(!check[adjacentNum]) {
					queue.add(buildings[adjacentNum]);
					check[adjacentNum] = true;
					count += depth;
					++countAdjacent;
				}
			}
			
			// 현재 레벨 체크가 끝났으면 countInLevel에 countAdjacent(다음 레벨의 노드들)를 넣어주고
			// countAdjacent를 초기화 후 깊이를 늘려줍니다.	 	
			if(countInLevel == 0) {
				countInLevel = countAdjacent;
				countAdjacent = 0;
				++depth;
			}
		}
		
		return count;
	}
	
	// 치킨집을 찾아 출력해주는 메서드 입니다.
	public static void findChicken() {
		
		Chicken chicken = null;
		
		int minCount = Integer.MAX_VALUE;
		
		// 1,2번 -> 1,3번 -> 1,4번 순으로 치킨집을 가정하고 자리를 찾기 때문에 따로 오름차순 정렬은 하지 않아도 됩니다.
		for (int i = 1; i < buildings.length; i++) {
			for (int j = i + 1; j < buildings.length; j++) {
				
				int count = BFS(i,j);
				
				if(count < minCount) { 
					minCount = count;
					
					chicken = new Chicken();
					chicken.count = count;
					chicken.minNumChicken = i;
					chicken.maxNumChicken = j;
					}
				}
			}
		
		System.out.println(chicken);
	}

	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			// 입력 시작
			String nums = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(nums);
			
			int n = Integer.parseInt(stk.nextToken());
			int m = Integer.parseInt(stk.nextToken());
			
			buildings = new Building[n + 1];
			
			for (int i = 1; i < buildings.length; i++) 
				buildings[i] = new Building();
			
			
			for (int i = 1; i <= m; i++) {
				nums = br.readLine().trim();
				stk = new StringTokenizer(nums);
				
				int a = Integer.parseInt(stk.nextToken());
				int b = Integer.parseInt(stk.nextToken());
				
				buildings[a].adjacentNums.add(b);
				buildings[b].adjacentNums.add(a);
			}
			// 입력 끝
			
			findChicken();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



### 방법2. 플로이드 와샬(Floyd Warshall)

[플로이드 와샬(Floyd Warshall)](https://blog.naver.com/ndb796/221234427842)은 거쳐가는 정점을 기준으로 `모든 정점`에서 `모든 정점`으로의 최단 경로를 구하고 싶을 때 사용합니다. `플로이드 와샬`을 이용해 모든 정점의 최단 경로를 구해 두 개의 치킨집을 가정해서 모든 건물들과의 거리의 총합이 가장 작은 경우를 찾습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {
	final static int INF = 100000;
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			// 입력 시작
			String nums = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(nums);
			
			int n = Integer.parseInt(stk.nextToken());
			int m = Integer.parseInt(stk.nextToken());
			
			int buildings[][] = new int[n+1][n+1];
						
			for (int i = 1; i <= m; i++) {
				nums = br.readLine().trim();
				stk = new StringTokenizer(nums);
				
				int a = Integer.parseInt(stk.nextToken());
				int b = Integer.parseInt(stk.nextToken());
				buildings[a][b] = 1;
				buildings[b][a] = 1;
			}
			// 입력 끝
			
			// i == j인 경우(자기자신을 가르키고 있는 경우는 0이기 때문에)를 제외하고 
			// 값이 0인(노드가 직접적으로 연력되지 않은) 인덱스 값들을 최대 값으로 바꿔줍니다.
			// 최대 값으로 바꾸는 이유는 플로이드 마샬에서 최솟값들을 비교하기 때문입니다.
			for (int i = 1; i < buildings.length; i++)
				for (int j = 1; j < buildings[i].length; j++) 
					if(i != j && buildings[i][j] == 0)
						buildings[i][j] = INF;
			
			// 플로이드 마샬을 실행합니다.
			for (int k = 1; k <= n; k++) {
				for (int i = 1; i < buildings.length; i++) {
					for (int j = 1; j < buildings.length; j++) {
						if(buildings[i][j] > buildings[i][k] + buildings[k][j])
							buildings[i][j] = buildings[i][k] + buildings[k][j];
					}
				}
			}
			
			int minSum = INF;
			String result = "";
			
			// 두 개의 치킨집을 가정하고(i,j) 모든 건물들(k)과의 거리 총합 중 최소 값을 찾습니다.
			// "(j,k) + (j,k+1) + j(k+2) ..." 은 모든 건물들과 거리 총합
			for (int i = 1; i < buildings.length; i++) {
				for (int j = i+1; j < buildings.length; j++) {
					int sum = 0;
					// 건물(k)에서 치킨 집이 더 가까운 쪽으로 가기 때문에 (i,k) (j,k) 중 최소 값을 총합에 넣습니다.
					for (int k = 1; k < buildings.length; k++) {
						sum += Math.min(buildings[i][k],buildings[j][k]);
					}
					
					if(minSum > sum) {
						minSum = sum;
						result = i + " " + j + " " + minSum * 2;
					}
				}
			}
			System.out.println(result);
			
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

