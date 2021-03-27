---
title: "BaekJoon : 9205번(맥주 마시면서 걸어가기)"
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



## 9205

송도에 사는 상근이와 친구들은 송도에서 열리는 펜타포트 락 페스티벌에 가려고 한다. 올해는 맥주를 마시면서 걸어가기로 했다. 출발은 상근이네 집에서 하고, 맥주 한 박스를 들고 출발한다. **맥주 한 박스에는 맥주가 20개** 들어있다. 목이 마르면 안되기 때문에 **50미터에 한 병씩 마시려고 한다.**

상근이의 집에서 페스티벌이 열리는 곳은 매우 먼 거리이다. 따라서, 맥주를 더 구매해야 할 수도 있다. 미리 인터넷으로 조사를 해보니 다행히도 맥주를 파는 편의점이 있다. **편의점에 들렸을 때, 빈 병은 버리고 새 맥주 병을 살 수 있다. 하지만, 박스에 들어있는 맥주는 20병을 넘을 수 없다.**

**편의점, 상근이네 집, 펜타포트 락 페스티벌의 좌표가 주어진다.** 상근이와 친구들이 행복하게 페스티벌에 도착할 수 있는지 구하는 프로그램을 작성하시오.



### 입력

**첫째 줄에 테스트 케이스의 개수 t**가 주어진다. (t ≤ 50)

**각 테스트 케이스의 첫째 줄에는 맥주를 파는 편의점의 개수 n**이 주어진다. (0 ≤ n ≤ 100).

**다음 n+2개 줄에는 상근이네 집, 편의점, 펜타포트 락 페스티벌 좌표**가 주어진다. 각 **좌표는 두 정수 x와 y**로 이루어져 있다. (두 값 모두 미터, -32768 ≤ x, y ≤ 32767)

송도는 직사각형 모양으로 생긴 도시이다. **두 좌표 사이의 거리는 x 좌표의 차이 + y 좌표의 차이 이다. (맨해튼 거리)**

```java
2			// 테스트 케이스(t)를 입력합니다.
2			// 1번째 테스트케이스의 편의점 개수(n)을 입력합니다.
0 0			// 집
1000 0
1000 1000	// 여기까지 편의점
2000 1000	// 페스티벌 좌표를 입력합니다.
2			// 2번째 테스트케이스도 동일한 방식으로 입력합니다.
0 0
1000 0
2000 1000
2000 2000
```



### 출력

각 테스트 케이스에 대해서 상근이와 친구들이 행복하게 **페스티벌에 갈 수 있으면 "happy", 중간에 맥주가 바닥나면 "sad"를 출력**한다. 

```java
happy	// 페스티벌에 도착할 때,
sad		// 도착하지 못하였을 때
```



### 내코드

문제의 핵심을 찾는 것이 중요한데, 이 문제의 핵심은 **집, 편의점, 페스티벌을 하나의 노드로 보고 맥주를 20잔 마실 수 있는 거리 즉, 50m X 20 = 1,000m(문제에서 맨해튼 거리라고 힌트를 주었습니다.) 안에 있으면 인접한 노드로 생각한다는 것**입니다. 

문제를 노드를 이용한 그래프 형태로 표현할 수 있기 때문에<br>**집에서 페스티벌까지 단계별로 접근하는 `BFS`**로 풀 수도 있고(DFS도 됩니다!)<br>집과 편의점에서 페스티벌까지 즉, **다수의 정점이 모든 정점으로 갈 수있는 최소 경우를 찾는 `플로이드 와샬`**을 이용해서 집 -> 페스티벌로 갈 수 있는 경우가 있는지  확인하여 문제를 풀 수도 있습니다. 

여기서는 `플로이드 와샬`로 풀어보겠습니다. `플로이드 와샬`은 기준점 즉, 정점을 설정해주는 것이 중요합니다. 여기서는 집, 편의점, 페스티벌을 거쳐가는 정점으로 설정해주면 생각보다 쉽게 `플로이드 와샬`을 사용할 수 있습니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	static InputStreamReader isr = new InputStreamReader(System.in);
	static BufferedReader br = new BufferedReader(isr);
	
	static int n;
	final static int INF = 3300000;
	// 정점(집 + 편의점 + 페스티벌)을 저장할 배열
	static Node[] stores;
	// 갈 수 있는 경로를 저장할 배열
	static int songdo[][];
	
	// x,y를 저장할 노드 클래스
	public static class Node {
		int x;
		int y;
	}
	
	// 집, 편의점, 페스티벌 장소가 담긴 송도 맵을 만듭니다.
	public static void makeSongdo() throws Exception{
		
		n = Integer.parseInt(br.readLine());
		
		songdo = new int[n+2][n+2];
		
		// 편의점 + 집 + 페스티벌 장소이기 때문에 n+2
		stores = new Node[n+2];
		
		for (int i = 0; i <= n+1; i++) {
			String position = br.readLine();
			StringTokenizer stk = new StringTokenizer(position);
			
			Node node = new Node();
			
			node.x = Integer.parseInt(stk.nextToken());
			node.y = Integer.parseInt(stk.nextToken());
			
			stores[i] = node; 
		}
		
		// 송도맵을 순회해서 거리가 1000이하 즉, 맥주를 50미터 씩 20번 마실 수 있는 거리면 이동할 수 있으므로 1을 없으면 INF를 넣어줍니다.
		// j=i+1을 해준 이유를 예를 들어 설명하자면, i=0,j=1일 때, [0][1],[1][0]값 둘 다 바꿔주기 때문에 i=1,j=0인 경우는 확인하지 않아도 됩니다.
		for (int i = 0; i <= n+1; i++) {
			for (int j = i + 1; j <= n+1; j++) {
				int dist = Math.abs(stores[i].x - stores[j].x) + Math.abs(stores[i].y - stores[j].y);
				
				if(dist <= 1000) {
					songdo[i][j] += 1;
					songdo[j][i] += 1;
				}
				else {
					songdo[i][j] = INF;
					songdo[j][i] = INF;
				}
			}
		}
		
	}
	
	// 페스티벌까지 갈 수 있는지 확인하는 메서드
	public static String goFestival() {
		
		// 플로이드 와샬
		for (int k = 0; k <= n + 1; k++) {
			for (int i = 0; i <= n + 1; i++) {
				for (int j = 0; j <= n + 1; j++) {
					if(songdo[i][j] > songdo[i][k] + songdo[k][j])
						songdo[i][j] = songdo[i][k] + songdo[k][j];
				}
			}
		}
		
		boolean arrivalCheck = false;
		
		// [0][n+1] 즉, 집에서 페스티벌 장소까지 가는 경우가 있는지 체크합니다.
		if(songdo[0][n+1] > 0 && songdo[0][n+1] != INF)
			arrivalCheck = true;
		
		if(arrivalCheck)
			return "happy";
		
		return "sad";
	}
	
	public static void main(String[] args) {
		
		try {
			int t = Integer.parseInt(br.readLine());
			
			List<String> list = new ArrayList<>();
			
			for (int i = 0; i < t; i++) {
				makeSongdo();
				
				list.add(goFestival());
			}
			
			for(String result : list) {
				System.out.println(result);
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

