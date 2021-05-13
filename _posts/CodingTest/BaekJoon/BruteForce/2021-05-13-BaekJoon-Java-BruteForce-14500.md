---
title: "BaekJoon : 14500(테트로미노)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Brute Force"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Brute Force

BaekJoon Brute Force(브루트포스)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 14500

폴리오미노란 크기가 1×1인 정사각형을 여러 개 이어서 붙인 도형이며, 다음과 같은 조건을 만족해야 한다.

- 정사각형은 서로 겹치면 안 된다.
- 도형은 모두 연결되어 있어야 한다.
- 정사각형의 변끼리 연결되어 있어야 한다. 즉, 꼭짓점과 꼭짓점만 맞닿아 있으면 안 된다.

정사각형 4개를 이어 붙인 폴리오미노는 테트로미노라고 하며, 다음과 같은 5가지가 있다.

[<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/14500/1.png" alt="img" style="zoom: 67%;" />](https://commons.wikimedia.org/wiki/File:All_5_free_tetrominoes.svg)

아름이는 크기가 **N×M인 종이 위에 테트로미노 하나**를 놓으려고 한다. 종이는 1×1 크기의 칸으로 나누어져 있으며, 각각의 칸에는 정수가 하나 쓰여 있다.

**테트로미노 하나를 적절히 놓아서 테트로미노가 놓인 칸에 쓰여 있는 수들의 합을 최대**로 하는 프로그램을 작성하시오.

테트로미노는 반드시 한 정사각형이 정확히 하나의 칸을 포함하도록 놓아야 하며, **회전이나 대칭을 시켜도 된다.**



### 입력

**첫째 줄에 종이의 세로 크기 N과 가로 크기 M**이 주어진다. (4 ≤ N, M ≤ 500)

**둘째 줄부터 N개의 줄에 종이에 쓰여 있는 수**가 주어진다. i번째 줄의 j번째 수는 위에서부터 i번째 칸, 왼쪽에서부터 j번째 칸에 쓰여 있는 수이다. **입력으로 주어지는 수는 1,000을 넘지 않는 자연수**이다.

```java
5 5			// 세로 크기(N)과 가로 크기(M)이 주어집니다.
1 2 3 4 5	// 여기 부터
5 4 3 2 1
2 3 4 5 6
6 5 4 3 2
1 2 1 2 1	// 여기 까지 N개의 줄에 종이에 쓰여있는 수가 주어집니다.
```



### 출력

**첫째 줄에 테트로미노가 놓인 칸에 쓰인 수들의 합의 최댓값**을 출력한다.

```java
19
```



### 폴리오미노 블록 만드는 방법

전형적인 브루트 포스이기 때문에 모양 하나하나를 직접 입력해주어서 해결할 수 있는 문제지만, `ㅜ`모양을 제외한 나머지 모양은 `DFS`로 만들 수도 있습니다.

```java
0000400
0004340
0001234
0004340
0000400
```

**`DFS`를 이용해 깊이가 4가 되면 멈추게 되면 위와 같이(오른쪽 방향만 예시로 듬) 1~4를 하나의 도형으로 생각했을 때,  `ㅜ` 모양을 제외한 나머지 4개 모양의 도형이 나옵니다.** 그리고 남은 `ㅜ` 모양은 따로 구해주면 됩니다. 이 방법을 이용해 하나 하나 입력하는 방법보다는 좀 더 간단하게 문제를 해결할 수 있습니다.



### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	static int n;
	static int m;
	
	static int map[][];
	static boolean visit[][];
	static int max = 0;
	
	// 상하좌우
	static int dirRow[] = {-1,1,0,0};
	static int dirCol[] = {0,0,-1,1};
	
	// 'ㅗ', 'ㅜ', 'ㅓ', 'ㅏ' 모양 순
	static int exRow[][] = {{1,1,1,0}, {0,0,0,1}, {0,1,2,1}, {0,1,2,1}};
	static int exCol[][] = {{0,1,2,1}, {0,1,2,1}, {1,1,1,0}, {0,0,0,1}};
	
	// 상하좌우 방향으로 DFS를 돌다가 깊이가 4가 되면 4가 될 때까지 들린 곳의 합을 이용해 최댓값을 갱신합니다.
	public static void DFS(int row, int col, int sum, int depth) {
		if(depth == 4) {
			max = Math.max(sum, max);
			return;
		} 
		
		for (int i = 0; i < 4; i++) {
			int curRow = row + dirRow[i];
			int curCol = col + dirCol[i];
			
			// 상하좌우방향에서 범위를 벗어나지 않고 지나왔던 길이 아니면 DFS를 진행합니다.
			if(curRow >= 0 && curRow < n &&
					curCol >= 0 && curCol < m && !visit[curRow][curCol]) {
				visit[curRow][curCol] = true;
				DFS(curRow, curCol, sum + map[curRow][curCol], depth + 1);
				visit[curRow][curCol] = false;
			}
		}
	}
	
	// 'ㅗ', 'ㅜ', 'ㅓ', 'ㅏ' 모양을 구해줍니다.
	public static void exception(int row, int col) {
		
		for (int i = 0; i < 4; i++) {
			int sum = 0;
			
			for (int j = 0; j < 4; j++) {
				int curRow = row + exRow[i][j];
				int curCol = col + exCol[i][j];
				
				if(curRow >= 0 && curRow < n &&
						curCol >= 0 && curCol < m && !visit[curRow][curCol]) {
					sum += map[curRow][curCol];
				}
			}
			max = Math.max(max, sum);
		}
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			n = Integer.parseInt(stk.nextToken());
			m = Integer.parseInt(stk.nextToken());
			
			map = new int[n][m];
			visit = new boolean[n][m];
			
			// 종이 만들기
			for (int i = 0; i < n; i++) {
				stk = new StringTokenizer(br.readLine());
				for (int j = 0; j < m; j++)
					map[i][j] = Integer.parseInt(stk.nextToken());
			}
			
			// 종이의 모든 부분을 순회하면서 도형을 찾아 제일 최대 값이 나오는 경우를 찾습니다.
			for (int i = 0; i < n; i++) {
				for (int j = 0; j < m; j++) {
					visit[i][j] = true;
					DFS(i,j,map[i][j],1);
					visit[i][j] = false;
					exception(i, j);
				}
			}
			
			System.out.println(max);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

