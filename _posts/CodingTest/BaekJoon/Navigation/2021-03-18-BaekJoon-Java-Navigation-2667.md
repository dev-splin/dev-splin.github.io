---
title: "BaekJoon : 2667번(단지번호붙이기)"
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



## 2667

<그림 1>과 같이 정사각형 모양의 지도가 있다. **1은 집이 있는 곳을, 0은 집이 없는 곳**을 나타낸다. 철수는 이 지도를 가지고 연결된 집의 모임인 단지를 정의하고, 단지에 번호를 붙이려 한다. 여기서 **연결되었다는 것은 어떤 집이 좌우, 혹은 아래위로 다른 집이 있는 경우**를 말한다. **대각선상에 집이 있는 경우는 연결된 것이 아니다.** <그림 2>는 <그림 1>을 단지별로 번호를 붙인 것이다. 지도를 입력하여 단지수를 출력하고, 각 단지에 속하는 집의 수를 오름차순으로 정렬하여 출력하는 프로그램을 작성하시오.

![img](https://www.acmicpc.net/upload/images/ITVH9w1Gf6eCRdThfkegBUSOKd.png)



첫 번째 줄에는 지도의 크기 **N(정사각형이므로 가로와 세로의 크기는 같으며 5≤N≤25)**이 입력되고, 그 **다음 N줄에는 각각 N개의 자료(0혹은 1)가 입력**된다.

```java
7			// 지도의 크기(N)을 입력
0110100
0110101
1110101
0000111
0100000
0111110
0111000		// 집이 있으면 1 없으면 0을 입력해서 지도를 만들어 줍니다.
3
7
8
9			// 단지 내 집의 수를 오름차순으로 정렬해서 출력합니다.
```



### 내코드

연결 된 모든 집(아파트)의 수를 구해주는 문제이기 때문에, 모든 맵을 순회하면서 체크되지 않은 집들을 `BFS`를 이용해 연결된 집들을 체크해주고 수를 구합니다.

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

public class Main {
	static InputStreamReader isr = new InputStreamReader(System.in);
	static BufferedReader br = new BufferedReader(isr);
	static int map[][];
	static boolean check[][];
	static int n;
	
	// 맵을 입력받고 아스키코드를 이용해 맵을 만들어 줍니다.
	public static void makeMap() throws Exception{
		map = new int[n][n];
		check = new boolean[n][n];
		
		for (int i = 0; i < map.length; i++) {
			char houses[] = br.readLine().trim().toCharArray();
			
			for (int j = 0; j < houses.length; j++) {
				map[i][j] = houses[j] - 48;
			}
		}
	}
	
	// 인자로 받은 행과열을 이용해 BFS를 진행하고 해당 BFS 결과의 개수를 반환합니다.
	// 이 때, 인자로 받은 행렬이 이미 체크되었거나 빈 땅이면 0을 반환합니다.
	public static int BFS(int startRow, int startCol) {
		
		int count = 0;
		
		if(check[startRow][startCol] || map[startRow][startCol] != 1)
			return count;
		
		// 상하좌우 체크를 위한 배열
		int dirRow[] = {-1, 1, 0, 0};
		int dirCol[] = {0, 0, -1, 1};
		
		Queue<Integer> rowQueue = new LinkedList<>();
		Queue<Integer> colQueue = new LinkedList<>();
		rowQueue.add(startRow);
		colQueue.add(startCol);
		
		check[startRow][startCol] = true;
		++count;
		
		while(!rowQueue.isEmpty()) {
			int row = rowQueue.poll();
			int col = colQueue.poll();
			
			for (int i = 0; i < dirRow.length; i++) {
				
				int tmpRow = row + dirRow[i];
				int tmpCol = col + dirCol[i];
				
				// 상하좌우를 체크하기 때문에 맵의 범위를 벗어나지 않았는 지 체크해야합니다.
				if(tmpRow >= 0 && tmpCol >= 0
					&& tmpRow < n && tmpCol < n)
					if(!check[tmpRow][tmpCol] && map[tmpRow][tmpCol] == 1) {
						
						rowQueue.add(tmpRow);
						colQueue.add(tmpCol);
						
						check[tmpRow][tmpCol] = true;
						++count;
					}
			}
		}
		return count;
	}
	
	public static void main(String[] args) {
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			n = Integer.parseInt(br.readLine().trim());
			
			makeMap();
			
            // 단지 내의 집(아파트)수를 저장할 리스트
			List<Integer> countApartment = new ArrayList<>();
			
			// 맵 전체를 순회하면서 BFS를 진행합니다.
			for (int i = 0; i < map.length; i++) {
				for (int j = 0; j < map[i].length; j++) {
					
					int count = BFS(i,j);
					
					if(count != 0)
						countApartment.add(count);
				}
			}
			
			Collections.sort(countApartment);
			
			bw.write(Integer.toString(countApartment.size()));
			bw.newLine();
			
			for(int count : countApartment) {
				bw.write(Integer.toString(count));
				bw.newLine();
			}
			
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