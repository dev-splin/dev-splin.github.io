---
title: "BaekJoon : 17070(파이프 옮기기 1)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Dynamic Programming"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Dynamic Programming

BaekJoon `Dynamic Programming(동적 프로그래밍)`  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 17070

유현이가 새 집으로 이사했다. 새 집의 크기는 N×N의 격자판으로 나타낼 수 있고, 1×1크기의 정사각형 칸으로 나누어져 있다. 각각의 칸은 (r, c)로 나타낼 수 있다. 여기서 r은 행의 번호, c는 열의 번호이고, 행과 열의 번호는 1부터 시작한다. 각각의 칸은 빈 칸이거나 벽이다.

오늘은 집 수리를 위해서 파이프 하나를 옮기려고 한다. 파이프는 아래와 같은 형태이고, 2개의 연속된 칸을 차지하는 크기이다.

<img src="https://upload.acmicpc.net/3ceac594-87df-487d-9152-c532f7136e1e/-/preview/" alt="img" style="zoom: 67%;" />

파이프는 회전시킬 수 있으며, 아래와 같이 3가지 방향이 가능하다.

<img src="https://upload.acmicpc.net/b29efafa-dbae-4522-809c-76d5c184a231/-/preview/" alt="img" style="zoom: 67%;" />

파이프는 매우 무겁기 때문에, 유현이는 파이프를 밀어서 이동시키려고 한다. 벽에는 새로운 벽지를 발랐기 때문에, 파이프가 벽을 긁으면 안 된다. 즉, 파이프는 항상 빈 칸만 차지해야 한다.

**파이프를 밀 수 있는 방향은 총 3가지가 있으며, →, ↘, ↓ 방향**이다. 파이프는 밀면서 회전시킬 수 있다. 회전은 45도만 회전시킬 수 있으며, 미는 방향은 오른쪽, 아래, 또는 오른쪽 아래 대각선 방향이어야 한다.

파이프가 가로로 놓여진 경우에 가능한 이동 방법은 총 2가지, 세로로 놓여진 경우에는 2가지, 대각선 방향으로 놓여진 경우에는 3가지가 있다.

아래 그림은 파이프가 놓여진 방향에 따라서 이동할 수 있는 방법을 모두 나타낸 것이고, 꼭 빈 칸이어야 하는 곳은 색으로 표시되어져 있다.

**가로**

<img src="https://upload.acmicpc.net/0f445b26-4e5b-4169-8a1a-89c9e115907e/-/preview/" alt="img" style="zoom:67%;" />

---

**세로**

<img src="https://upload.acmicpc.net/045d071f-0ea2-4ab5-a8db-61c215e7e7b7/-/preview/" alt="img" style="zoom:67%;" />

---

**대각선**

<img src="https://upload.acmicpc.net/ace5e982-6a52-4982-b51d-6c33c6b742bf/-/preview/" alt="img" style="zoom:67%;" />



**가장 처음에 파이프는 (1, 1)와 (1, 2)**를 차지하고 있고, **방향은 가로**이다. **파이프의 한쪽 끝을 (N, N)로 이동시키는 방법의 개수**를 구해보자.



### 입력

**첫째 줄에 집의 크기 N(3 ≤ N ≤ 16)**이 주어진다. **둘째 줄부터 N개의 줄에는 집의 상태**가 주어진다. **빈 칸은 0, 벽은 1로 주어진다. (1, 1)과 (1, 2)는 항상 빈 칸**이다.

```java
3		// 집의 크기를 입력합니다.	
0 0 0	// 여기부터
0 0 0
0 0 0	// 여기까지 집의 상태를 입력합니다.
```



### 출력

**첫째 줄에 파이프의 한쪽 끝을 (N, N)으로 이동시키는 방법의 수를 출력**한다. **이동시킬 수 없는 경우에는 0을 출력**한다. 방법의 수는 항상 1,000,000보다 작거나 같다.

```java
1
```



### DP와 DFS

2가지 방법으로 해결할 수 있는 문제입니다. 

**모든 방법의 수는 작은 방법의 수**로 나눌 수 있습니다. 즉, **큰 문제를 작은 문제**로 나누는 `DP`를 이용하여 해결할 수 있습니다.

또, **처음 파이프의 끝(1,2)이 (N,N)에 도달하는 모든 방법의 수를 출력**해야하기 때문에 모든 경로를 탐색하는 `DFS`를 이용할 수 있습니다. (하향식 DP는 재귀 함수를 이용하기 때문에 어쩌면 DP로 볼 수도 있을 거 같습니다.)

물론 `BFS`도 가능하지만, 대각선, 가로, 세로를 체크해주어야 하고 각각의 방향에서 밀 수 있는 방향까지 체크해주어야 합니다. 때문에 **BFS는  DP와 DFS에 비해 너무 많은 공간과 시간을 사용**하게 되므로 보지 않겠습니다.



### DFS

**파이프의 끝만 (N,N)에 도달하면 되기 때문에, 파이프 전체를 하나로 생각하지 않고 끝 부분만 생각**하면 됩니다.

움직이는 방법을 `현재 방향[행][열] -> 이동할 방향[행][열]`로 표현해 보겠습니다.

`가로[r][c] -> 가로[r][c+1], 대각선[r+1][c+1]` <br>`세로[r][c] -> 세로[r+1][c], 대각선[r+1][c+1]`<br>`대각선[r][c] -> 가로[r][c+1], 세로[r+1][c], 대각선[r+1][c+1]`

각각의 방향은 위와 같이 움직이기 때문에 **재귀 함수를 이용할 때 방향을 생각**해 주어야 합니다. 또, 방향에 공통된 부분이 있는데, **모든 방향에 대각선으로 이동하는 경우가 포함**되고, **대각선은 모든 방향으로 이동**할 수 있습니다. 이 특징을 이용해 중복하는 부분을 줄일 수 있습니다.

이 때, 주의할 점은 대각선을 이동할 때 `(r+1, c+1)`만 체크하는 것이 아니고 `(r+1, c), (r, c+1)`까지 체크해주어야 한다는 것입니다.



#### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	static int n;
	static int arr[][];
	// 대각선 방향 체크할 배열 (r,c+1), (r+1,c), (r+1,c+1)
	static int dirRow[] = {0,1,1};
	static int dirCol[] = {1,0,1};
	static int count = 0;
	
	// status가 0 = 가로, 1 = 세로, 2 = 대각선 
	public static void DFS(int status, int row, int col) {
        // n,n에 도달하면 count 증가
		if(row == n && col == n) {
			++count;
			return;
		}
		
		// 왼쪽 방향으로는 이동하지 않기 때문에 오른쪽 방향의 끝(n)만 체크
		if(status == 0 || status == 2) {
			if(col+1 <= n && arr[row][col+1] != 1)
				DFS(0,row,col+1);
		} 
		
		if(status == 1 || status == 2) {
			if(row+1 <= n && arr[row+1][col] != 1)
				DFS(1,row+1,col);
		}
		
		if(checkDiagonal(row, col))
			DFS(2,row+1,col+1);
	}
	
	// 대각선으로 이동할 수 있는지 체크
	public static boolean checkDiagonal(int row, int col) {
		
		for (int i = 0; i < 3; i++) {
			int nextRow = row+dirRow[i];
			int nextCol = col+dirCol[i];
			if(nextRow > n || nextCol > n || arr[nextRow][nextCol] == 1)
				return false;
		}
		
		return true;
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			n = Integer.parseInt(br.readLine());
			
			arr = new int[n+1][n+1];
			
			for (int i = 1; i <= n; i++) {
				StringTokenizer stk = new StringTokenizer(br.readLine());
				for (int j = 1; j <= n; j++)
					arr[i][j] = Integer.parseInt(stk.nextToken());
			}
			
			DFS(0,1,2);
			
			System.out.println(count);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### DP

`DP`를 이용하는 방법은 현재 `현재 방향 -> 이동할 방향`으로 생각하는 것이 아니고, **현재 위치에 도달할 수 있는 경우를 구해서 더하는 것**입니다.

현재 위치에 도달할 수 있는 경우를 `현재 위치[행][열] -> 현재 위치에 도달할 수 있는 경우[행][열]`로 표현해 보겠습니다.

`가로[r][c] -> 가로[r][c-1] + 대각선[r-1][c-1]` <br>`세로[r][c] -> 세로[r-1][c] + 대각선[r-1][c-1]`<br>`대각선[r][c] -> 가로[r][c-1] + 세로[r-1][c], 대각선[r-1][c-1]`

위와 같은 방식으로 **(1,1)부터 (n,n)까지 순회하면서 memoization에 값을 저장**한 후, `가로[n][n] + 세로[n][n] + 대각선[n][n]`을 하게 (N,N)에 도달할 수 있는 모든 방법의 수를 구할 수 있습니다.( **1부터 n까지 순회하기 때문에 따로 새 집의 범위를 넘어서는지 체크하지 않아도 됩니다.**) 다만, **행,열에 방향까지 알아야 하기 때문**에 **2차원 memoization이 아닌 `[행][열][방향]`으로 이루어진 3차원 memoization을 사용**해야 합니다.

이 때 주의할 점은, **(1,1)의 위치를 구할 때, 0행 0열을 이용하기 때문에 배열을 선언할 때 n+1만큼 선언**하여야 한다는 것이고, 현재 방향이 대각선일 때, 현재 위치`(r,c)`의 `위(i-1,j)`와 `왼쪽(i,j-1)`에 벽이 없는지 체크해야 한다는 것입니다.



#### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Iterator;
import java.util.StringTokenizer;

public class Main {
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			int n = Integer.parseInt(br.readLine());
			
			int arr[][] = new int[n+1][n+1];
			// dp[행][열][방향] 방향 0 = 가로, 1 = 세로, 2 = 대각선
			int dp[][][] = new int [n+1][n+1][3];
			
			for (int i = 1; i <= n; i++) {
				StringTokenizer stk = new StringTokenizer(br.readLine());
				for (int j = 1; j <= n; j++)
					arr[i][j] = Integer.parseInt(stk.nextToken());
			}
			
			// 처음 파이프 끝의 위치에 값을 넣어줍니다. (1행 2열에 올 수 있는 경우의 수)
			dp[1][2][0] = 1;
			
			// n,n까지 순회하기 때문에 따로 arr범위를 넘어가는지 체크해주지 않아도 됩니다.
			for (int i = 1; i <= n; i++) {
				for (int j = 1; j <= n; j++) {
					if(arr[i][j] == 1)
						continue;
					
					// 가로
					dp[i][j][0] += dp[i][j-1][0] + dp[i][j-1][2];
					// 세로
					dp[i][j][1] += dp[i-1][j][1] + dp[i-1][j][2];
					
					// 현재 방향이 대각선이면, 현재 위치의 위(i-1,j)와 왼쪽(i,j-1)에 벽이 없는지 체크해야 합니다.
					if(arr[i-1][j] == 0 && arr[i][j-1] == 0)
						dp[i][j][2] += dp[i-1][j-1][0] + dp[i-1][j-1][1] + dp[i-1][j-1][2];
				}
			}
			
			// 모든 방향의 경우의 수를 더해 답을 구합니다.
			int sum = dp[n][n][0] + dp[n][n][1] + dp[n][n][2];
			
			System.out.println(sum);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}


```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

