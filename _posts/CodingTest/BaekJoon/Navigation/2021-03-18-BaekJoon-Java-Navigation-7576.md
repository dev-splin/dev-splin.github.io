---
title: "BaekJoon : 7576번(토마토)"
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



## 7576

철수의 토마토 농장에서는 토마토를 보관하는 큰 창고를 가지고 있다. 토마토는 아래의 그림과 같이 격자 모양 상자의 칸에 하나씩 넣어서 창고에 보관한다. 

![img](https://www.acmicpc.net/upload/images/tmt.png)

창고에 보관되는 토마토들 중에는 **잘 익은 것**도 있지만, **아직 익지 않은 토마토**들도 있을 수 있다. **보관 후 하루가 지나면, 익은 토마토들의 인접한 곳에 있는 익지 않은 토마토들은 익은 토마토의 영향을 받아 익게 된다.** 하나의 토마토의 인접한 곳은 왼쪽, 오른쪽, 앞, 뒤 네 방향에 있는 토마토를 의미한다. 대각선 방향에 있는 토마토들에게는 영향을 주지 못하며, **토마토가 혼자 저절로 익는 경우는 없다**고 가정한다. 철수는 **창고에 보관된 토마토들이 며칠이 지나면 다 익게 되는지, 그 최소 일수**를 알고 싶어 한다.

토마토를 창고에 보관하는 **격자모양의 상자들의 크기**와 **익은 토마토들과 익지 않은 토마토들의 정보**가 주어졌을 때, 며칠이 지나면 토마토들이 모두 익는지, 그 최소 일수를 구하는 프로그램을 작성하라. 단, **상자의 일부 칸에는 토마토가 들어있지 않을 수도 있다.**



### 입력

**첫 줄에는 상자의 크기를 나타내는 두 정수 M,N**이 주어진다. **M은 상자의 가로 칸의 수, N은 상자의 세로 칸의 수**를 나타낸다. 단, **2 ≤ M,N ≤ 1,000** 이다. **둘째 줄부터는 하나의 상자에 저장된 토마토들의 정보가 주어진다.** 즉, 둘째 줄부터 **N개의 줄에는 상자에 담긴 토마토의 정보**가 주어진다. **하나의 줄에는 상자 가로줄에 들어있는 토마토의 상태가 M개의 정수**로 주어진다. 정수 **1은 익은 토마토**, 정수 **0은 익지 않은 토마토**, 정수 **-1은 토마토가 들어있지 않은 칸**을 나타낸다.

토마토가 하나 이상 있는 경우만 입력으로 주어진다.

```java
6 4				// 두 정수 M(열),N(행)을 입력합니다
1 -1 0 0 0 0
0 -1 0 0 0 0
0 0 0 0 -1 0
0 0 0 0 -1 1	// 토마토들의 정보를 입력합니다.
```



### 출력

여러분은 토마토가 **모두 익을 때까지의 최소 날짜를 출력**해야 한다. 만약, 저장될 때부터 **모든 토마토가 익어있는 상태이면 0을 출력**해야 하고, **토마토가 모두 익지는 못하는 상황이면 -1을 출력**해야 한다.

```java
6		// 모두 익는 최소 날짜를 출력합니다.
```



### 내코드

레벨 별(날짜 별)로 일수가 늘어나기 때문에  `BFS`를 사용하여 풀었습니다. **행,렬의 값을 날짜로 표현**하였습니다. 이 때, **처음 토마토를 만들 때 전부 익어있는 상태인 지(0이 없는 상태) 체크**해야하고, `BFS`를 이용한 후 **익지 못 한 토마토(값이 0)가 있는지 체크**해야합니다.

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
	static int tomato[][];
	static boolean check[][];
	static int n,m;
	
	// n,m을 입력받고 n과m으로 토마토 배열을 만듭니다. 
	// 이 때, 전부다 익어있는 상태 즉, 0이 없는 토마토배열인지 체크해주고 boolean을 리턴합니다.  
	public static boolean makeTomatoBox() throws Exception{
		String nums = br.readLine().trim();
		StringTokenizer stk = new StringTokenizer(nums);
		
		m = Integer.parseInt(stk.nextToken());
		n = Integer.parseInt(stk.nextToken());
		
		tomato = new int[n][m];
		check = new boolean[n][m];
		
		boolean checkZero = false;
		
		// 행(n)만큼 반복하고 열(m)만큼 입력합니다. 이때 0이 하나라도 있으면 checkZero는 true가 됩니다.
		for (int i = 0; i < tomato.length; i++) {
			nums = br.readLine().trim();
			stk = new StringTokenizer(nums);
			
			int j = 0;
			while(stk.hasMoreTokens()) {
				tomato[i][j] = Integer.parseInt(stk.nextToken());
				
				if(tomato[i][j] == 0)
					checkZero = true;
				
				++j;
			}
		}
		return checkZero;
	}
	
	// BFS를 이용해 레벨이 올라가면 해당 행,열의 값을 현재 레벨 행,열의 값 + 1을 해줍니다.
	// 예를 들어, 처음 익은 토마토 다음 레벨의 토마토들은 값이 2가 되는 것입니다.
	// 이렇게 되면 해당 행열의 값으로 몇 일이 지나야 익을 수 있는지를 나타낼 수 있습니다.
	// 이 때, 처음부터 익어있는 토마토가 여러 개 있을 수 있기 때문에 처음 큐에 익어있는 토마토 전부를 넣고 시작합니다.
	public static void BFS() {
		// 상,하,좌,우
		int dirRow[] = {-1, 1, 0, 0};
		int dirCol[] = {0, 0, -1, 1};
		
		Queue<Integer> rowQueue = new LinkedList<>();
		Queue<Integer> colQueue = new LinkedList<>();
		
		// 1(익어있는 토마트)위치를 모두 찾아 큐에 넣어줍니다.
		for (int i = 0; i < tomato.length; i++) {
			for (int j = 0; j < tomato[i].length; j++) {
				if(tomato[i][j] == 1) {
					rowQueue.add(i);
					colQueue.add(j);
					check[i][j] = true;
				}
			}
		}
		
		while(!rowQueue.isEmpty()) {
			int row = rowQueue.poll();
			int col = colQueue.poll();
			
			for (int i = 0; i < dirRow.length; i++) {
				int tmpRow = row + dirRow[i];
				int tmpCol = col + dirCol[i];
				
				if(tmpRow >= 0 && tmpCol >= 0
					&& tmpRow < n && tmpCol <m)
					if(!check[tmpRow][tmpCol] && tomato[tmpRow][tmpCol] != -1) {
						rowQueue.add(tmpRow);
						colQueue.add(tmpCol);
						
						tomato[tmpRow][tmpCol] = tomato[row][col] + 1;
						check[tmpRow][tmpCol] = true;
					}
			}
		}
	}
	
	public static void main(String[] args) {
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			boolean checkZero = makeTomatoBox();
			
			// 처음 부터 전부 익어있는 상태 (0이 없으면) 0을 출력해주고 프로그램 종료합니다.
			if(!checkZero) {
				bw.write("0");
				bw.flush();
				bw.close();
				return;
			}
			
			BFS();
			
			int day = 0;
			checkZero = false;
			
			// BFS를 마치고 토마토를 전부 검사해 하나라도 0(익지 않은 토마토)이 나오게 되면 -1을 출력하고
			// 아니면 해당 행,열의 값(익은 날)을 출력하는데, 처음 시작이 1이므로 -1 해줍니다.
			// (문제에서는 처음 익은 토마토 다음 부터 1로 표현하기 때문)
			for (int i = 0; i < tomato.length; i++) {
				for (int j = 0; j < tomato[i].length; j++) {
					if(tomato[i][j] == 0)
						checkZero = true;
					
					else if(tomato[i][j] > day)
						day = tomato[i][j];
				}
			}
			
			if(checkZero)
				bw.write("-1");
			else
				bw.write(Integer.toString(day-1));
			
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