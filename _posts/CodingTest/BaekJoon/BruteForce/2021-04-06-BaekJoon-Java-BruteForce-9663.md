---
title: "BaekJoon : 9663(N-Queen)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Brute Force"
  - "BaekJoon : Back Tracking"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Brute Force

BaekJoon Brute Force(브루트포스)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 9663

N-Queen 문제는 크기가 **N × N인 체스판 위에 퀸 N개를 서로 공격할 수 없게 놓는 문제**이다.

**N이 주어졌을 때, 퀸을 놓는 방법의 수를 구하는 프로그램을 작성**하시오.



### 입력

첫째 줄에 N이 주어진다. (1 ≤ N < 15)

```java
8	// N을 입력합니다.
```



### 출력

첫째 줄에 퀸 N개를 서로 공격할 수 없게 놓는 경우의 수를 출력한다.

```java
92
```



### 백 트래킹(Back Tracking)

이 문제는 `백트래킹`으로 해결할 수 있습니다. `백 트래킹(Back Tracking)`은 해를 찾아가는 도중, 지금의 경로가 해가 될 것 같지 않으면 그 경로를 더이상 가지 않고 되돌아갑니다. 코딩에서는 반복문의 횟수를 줄일 수 있으므로 효율적입니다. 이를 `가지치기`라고 하는데, **불필요한 부분을 쳐내고 최대한 올바른 쪽으로 간다는 의미**입니다. 즉, `백트래킹`은 모든 가능한 경우의 수 중에서 **특정한 조건을 만족하는 경우만 살펴보는 것**입니다.

#### 백트래킹 기법의 유망성 판단

어떤 노드의 유망성, 즉 **해가 될 만한지 판단한 후 유망하지 않다고 결정되면 그 노드의 이전(부모)로 돌아가(`Backtracking`)** 다음 자식 노드로 갑니다.<br>**해가 될 가능성이 있으면 `유망하다(promising)`고 하며**, 유망하지 않은 노드에 가지 않는 것을 `가지치기(pruning)` 한다고 하는 것입니다.

주로 문제 풀이에서는

1. `DFS` 등으로 모든 경우의 수를 탐색하는 과정에서
2. 조건문 등을 걸어 답이 절대로 될 수 없는 상황을 정의(`유망성판단`)하고
3. 그러한 상황일 경우에는 탐색을 중지시킨 뒤 그 이전으로 돌아가서 다시 다른 경우를 탐색하게끔(`Backtracking`) 구현할 수 있습니다.



### 문제 풀이

체스판에서 퀸은 가로, 세로, 대각선을 공격할 수 있기 때문에, **놓여있는 퀸을 기준으로 일직선상(가로,세로)과 대각선 방향에는 아무것도 놓여있으면 안됩니다.** <br>이 때 **행과열로 이루어진 체스판에서 한 행에는 하나의 퀸만 위치**할 수 있으므로, 2차원 배열의 체스판에서 퀸을 표시하지 않고 **1차원 배열의 인덱스를 행으로, 값을 열로 해서 퀸의 위치를 표현**할 수 있습니다. 표를 보면 이해가 더 쉽습니다.

|       |  1   |  2   |  3   |  4   |
| :---: | :--: | :--: | :--: | :--: |
| **1** |      |      |  O   |      |
| **2** |  O   |      |      |      |
| **3** |      |      |      |  O   |
| **4** |      |  O   |      |      |

표에서 퀸의 위치는 

`1행 -> 3열` <br>`2행 -> 1열` <br>`3행 -> 4열`<br>`4행 -> 2열`

길이가 4인 1차원 배열 `{3,1,4,2}`로 표현할 수 있습니다.

**문제의 핵심은 공격받지 않는 위치를 찾는 것**입니다. <br>`[1][3]`에 퀸이 놓여있을 때,  

1. 같은 행에는 다른 퀸이 올 수 없습니다.
2. 같은 열에는 다른 퀸이 올 수 없습니다.
3. 대각선에는 다른 퀸이 올 수 없습니다.

3가지의 조건 중 1번, 2번은 쉽게 알 수 있기 때문에 <br>`[1][3]`의 대각선 방향만 살펴보면<br>`[2][2]`<br>`[3][1]`<br>`[2][4]`

대각선에 위치한 곳과 퀸이 놓여있는 곳의 **행의 차이의 절댓값과 열의 차이의 절대값이 같은 것**을 볼 수 있습니다.<br> `ex) [1][3],[2][2]를 비교하면,  |행의 차이(2-1)| == |열의 차이(2-3)|`

이제 이 조건들을 이용해 유망성을 판단하면서 완전탐색(Brute Force)을 하면 되겠습니다.

### 코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	static int queens[];
	static int n;
	static int count = 0;
	
	// DFS를 이용해 유명상을 판단하여 백 트래킹을 하게 됩니다.
	public static void DFS(int row) {
		
		for (int i = 1; i <= n; i++) {
			// 유망성 판단
			if(checkQueen(row, i)) {
				// 유망성 판단을 통과하고 마지막 행일 경우 count를 올려주고 종료
				if(row == n) {
					++count;
					return;
				}
				
				queens[row] = i;
				DFS(row+1);
			}
		}
	}
	
	// 해당 행,열의 위치가 다른 퀸에게 공격받는 위치인지 확인하는 메서드(유망성 판단)
	public static boolean checkQueen(int row, int col) {
		
		for (int i = 1; i < row; i++)
			if(col == queens[i] || row - i == Math.abs(col - queens[i]))
				return false;
			
		return true;
		
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			n = Integer.parseInt(br.readLine());
			
			queens = new int[n+1];
			DFS(1);
			System.out.println(count);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

참고 : [https://chanhuiseok.github.io/posts/algo-23/](https://chanhuiseok.github.io/posts/algo-23/)

