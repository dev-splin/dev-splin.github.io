---
title: "BaekJoon : 12865번(평범한 배낭)"
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



## 12865

이 문제는 아주 평범한 배낭에 관한 문제이다.

한 달 후면 국가의 부름을 받게 되는 준서는 여행을 가려고 한다. 세상과의 단절을 슬퍼하며 최대한 즐기기 위한 여행이기 때문에, 가지고 다닐 배낭 또한 최대한 가치 있게 싸려고 한다.

준서가 여행에 필요하다고 생각하는 **N개의 물건**이 있다. **각 물건은 무게 W와 가치 V**를 가지는데, 해당 물건을 배낭에 넣어서 가면 준서가 V만큼 즐길 수 있다. 아직 행군을 해본 적이 없는 준서는 **최대 K만큼의 무게만을 넣을 수 있는 배낭만 들고 다닐 수 있다.** 준서가 최대한 즐거운 여행을 하기 위해 **배낭에 넣을 수 있는 물건들의 가치의 최댓값**을 알려주자.



### 입력

**첫 줄에 물품의 수 N(1 ≤ N ≤ 100)과 준서가 버틸 수 있는 무게 K(1 ≤ K ≤ 100,000)**가 주어진다. **두 번째 줄부터 N개의 줄에 거쳐 각 물건의 무게 W(1 ≤ W ≤ 100,000)와 해당 물건의 가치 V(0 ≤ V ≤ 1,000)**가 주어진다.

입력으로 주어지는 모든 수는 정수이다.

```java
4 7		// 물품의 수(N)과 버틸 수 있는 무게(K) 입력
6 13	// 여기부터
4 8
3 6
5 12	// 여기까지 각 물건의 무게(W)와 가치(V)를 입력
```



### 출력

한 줄에 **배낭에 넣을 수 있는 물건들의 가치합의 최댓값을 출력**한다.

```java
14
```



### knapsack

배낭 문제(knapsack)로 유명한 문제입니다. **배낭 문제는 문제 설명처럼 배낭에 넣을 수 있는 최댓값이 정해지고 해당 한도 물건을 넣어 가치의 합이 최대가 되도록 고르는 방법을 찾는 것**입니다. 즉, 조합 최적화 문제입니다.

배낭문제, 일명 `knapsack 알고리즘`은 크게 두 가지 문제로 분류 될 수 있는데, `짐을 쪼갤 수 있는 경우`와 `짐을 쪼갤 수 없는 경우`로 나눌 수 있습니다. **짐을 쪼갤 수 있는 배낭문제를 Fraction Knapsack Problem** 이라 하고, **짐을 쪼갤 수 없는 배낭문제를 0/1 Knapsack Problem** 이라 합니다. 알고리즘 또한 다르게 적용하는데, `Fraction Knapsack Problem`의 경우 `탐욕 알고리즘(Greedy)`을 쓰며, `0/1 Knapsack Problem`의 경우 `DP` 법을 씁니다.  

이번 문제는 짐을 쪼갤 수 없기 때문에 `다이나믹 프로그래밍(DP)`로 해결해야합니다.



위의 예제에서 `물건의 번호가 i`일 때, `물건의 무게(w)`와 `물건의 가치(v)`를 **(w<sub>i</sub>,v<sub>i</sub>)**라고 표현하면,

**(w<sub>1</sub>,v<sub>1</sub>) -> (6,13)**<br>**(w<sub>2</sub>,v<sub>2</sub>) -> (4,8)**<br>**(w<sub>3</sub>,v<sub>3</sub>) -> (3,6)**<br>**(w<sub>4</sub>,v<sub>4</sub>) -> (5,12)** 

가 되고, **최대 수용가능한 무게 K = 7** 입니다.

이 때, `행을 물건의 번호(i)`로, `열을 수용가능한 무게(0~7)(k)`로 표를 그려보면 문제를 이해할 수 있습니다. 

| `dp[i][k]` |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |
| :--------: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|   **1**    |  0   |  0   |  0   |  0   |  0   |  0   |  13  |  13  |
|   **2**    |  0   |  0   |  0   |  0   |  8   |  8   |  13  |  13  |
|   **3**    |  0   |  0   |  0   |  6   |  8   |  8   |  13  |  14  |
|   **4**    |  0   |  0   |  0   |  6   |  8   |  12  |  13  |  14  |

1행부터 보게 되면 **i가 1일 때 무게가 6, 가치가 13인 물건**이 올 수 있기 때문에 **k가 0~5일 때까지는 아무것도 올 수 없으므로 0**,  `dp[1][6]`에 13이 들어가게 됩니다. <br>이 때, k가 7(`dp[1][7]`)이면  `7=6+1` 즉, **현재 번호의 무게 6의 경우(`dp[1][6]`)와 이전 번호(현재 번호는 이용하고 있기 때문)가 1의 무게일 때의 경우(`dp[0][1]`)를 더한 것으로 표현**할 수 있기 때문에 `dp[1][6]+dp[0][1]`인 `13+0=13`이 오게 됩니다.



2행에서는 **i가 2일 때, 무게가 4, 가치가 8인 물건**이 올 수 있으므로 **k가 0~3일 때까지는 0**, `dp[2][4]`에 8이 들어갑니다.<br>k가 5이면 `dp[2][4]+dp[1][1]`이기 때문에 `8+0=8`,<br>k가 6이면 `dp[2][4]+dp[1][2]`인 `8+0=8`이 올수도 있지만, **현재까지 체크한 i 1,2 중 i가 1일 때 무게가 6, 가치가 13인 물건`dp[1][6]`이 가치가 더 크기 때문에 13이 들어가게 됩니다.**<br>k가 7일 때도 마찬가지로 `dp[2][4]+dp[1][3]`인 `8+0=8`보다 `dp[1][7] = dp[1][6]+dp[0][1]`인 `13+0=13`이 더 크기 때문에 13이 옵니다.

2행의 k가 6,7인 경우를 보면, 이 때까지 0이 들어갔던 부분도 그 전인 `dp[i-1][k]`가 0이었기 때문에 0의 값이 들어왔다는 것을 알 수 있습니다.

즉, i에 올 수 있는 `물건의 무게(w)`와 `물건의 가치(v)`가 있을 때, `dp[i][k]`에서 **k가 물건의 무게(w)보다 작으면 i 이전의 물건 중 최대값인 `dp[i-1][k]`가** 오게 되고,<br> **k가 물건의 무게(w)보다 크거나 같으면 i 이전의 물건 중 최대값인 `dp[i-1][k]`와  k에서 물건의 무게(w)를 뺀 i 이전의 물건 중 최대값인 `dp[i-1][k-w]`,  물건의 가치(v) 합(`dp[i-1][k-w] + v`) 중 더 큰 수가** 들어가게 됩니다.

> `dp[i-1][k]`는 `dp[i-2][k]`와 비교를, `dp[i-2][k]`는 `dp[i-3][k]`와 비교하기 때문에 결국 `dp[i-1][k]`는 i-1 이전의 물건 중 최대값이 오게 됩니다.



### 재귀를 이용한 하향식

재귀를 이용한 하향식 방법입니다. 주의할 점은 i가 1일 때 i가 0인 경우를 이용하게 되므로 `w, v, memo`를 선언할 때 +1씩 더해주어야 한다는 것입니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
	static Integer memo[][];
	static int w[];
	static int v[];
	
	// 재귀를 이용한 하향식
	public static int dp(int i, int k) {
		if(i < 0)
			return 0;
		
		if(memo[i][k] == null) {
			// k가 i번의 물건의 무게보다 작으면 i 이전의 물건 중 최대값인 dp(i-1,k)이 들어가게 됩니다.
			if(w[i] > k)
				memo[i][k] = dp(i-1,k);
			// k가 i번의 물건의 무게보다 크거나 같으면 i 이전의 물건 중 최대값인 dp(i-1,k)와
			// k에서 물건의 무게(w)를 뺀 i 이전의 물건 중 최대값인 dp(i-1, k-w[i]), 물건의 가치 v[i]를 더한 값 중 큰 값을 넣습니다.
			else
				memo[i][k] = Math.max(dp(i-1, k), dp(i-1, k-w[i]) + v[i]);
		}
		
		return memo[i][k];
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			int n = Integer.parseInt(stk.nextToken());
			int k = Integer.parseInt(stk.nextToken());
			
			w = new int[n+1];
			v = new int[n+1];
			memo = new Integer[n+1][k+1];
			
			for (int i = 1; i <= n; i++) {
				stk = new StringTokenizer(br.readLine());
				
				w[i] = Integer.parseInt(stk.nextToken());
				v[i] = Integer.parseInt(stk.nextToken());
			}
			
			System.out.println(dp(n,k));
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 반복문을 이용한 상향식

반복문을 이용한 상향식 방법입니다. 처음부터 차례대로 순회하면서 `memo`에 값을 넣어 줍니다. 마찬가지로 `w, v, memo`를 선언할 때 +1씩 더해주어야 합니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	static int memo[][];
	static int w[];
	static int v[];
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			int n = Integer.parseInt(stk.nextToken());
			int k = Integer.parseInt(stk.nextToken());
			
			w = new int[n+1];
			v = new int[n+1];
			memo = new int[n+1][k+1];
			
			for (int i = 1; i <= n; i++) {
				stk = new StringTokenizer(br.readLine());
				
				w[i] = Integer.parseInt(stk.nextToken());
				v[i] = Integer.parseInt(stk.nextToken());
			}
			
			// 처음 부터 순회하면서 memo에 값을 넣어줍니다.
			for (int i = 1; i <= n; i++) {
				for (int j = 1; j <= k; j++) {
					// j가 i번의 물건의 무게보다 작으면 i 이전의 물건 중 최대값인 memo[i-1][j]가 들어가게 됩니다.
					if(w[i] > j)
						memo[i][j] = memo[i-1][j];
					// j가 i번의 물건의 무게보다 크거나 같으면 i 이전의 물건 중 최대값인 memo[i-1][j]와
					// j에서 물건의 무게(w)를 뺀 i 이전의 물건 중 최대값인 memo[i-1][j-w[i]], 물건의 가치 v[i]를 더한 값 중 큰 값을 넣습니다.
					else
						memo[i][j] = Math.max(memo[i-1][j], memo[i-1][j-w[i]] + v[i]);
				}
			}
			
			System.out.println(memo[n][k]);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 1차원배열을 이용한 상향식 

기존의 상향식은 j가 i번 째의 물건의 무게보다 작으면 `memo[i-1][j]`의 값을 이용했는데, 상향식에서 **j를 k의 최대값부터 i번 째의 물건의 무게까지 거꾸로 순회**하면서 1차원 배열을 갱신해주게 되면, **i번 째의 물건의 무게가 i-1번 째의 물건의 무게보다 클 시 기존의 i-1번 째의 물건의 값을 그대로 가지고 있기 때문에 불필요한 탐색을 줄일 수 있습니다.** 

예를 들어, `k=7`, `i-1의 무게가 3`, `i의 무게가 5`이면 **i-1번 째는 memo[3] ~ memo[7]을 갱신**하게 되고, **i번 째는 memo[5] ~ memo[7]을 갱신**하게 되기 때문에 **memo[3],memo[4]의 값을 그대로 이용**할 수 있습니다. (재귀를 이용한 하향식 방법에서는 처음부터 차례대로 갱신하지 않기 때문에 불가능합니다.)

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	static int memo[];
	static int w[];
	static int v[];
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			int n = Integer.parseInt(stk.nextToken());
			int k = Integer.parseInt(stk.nextToken());
			
			w = new int[n+1];
			v = new int[n+1];
			memo = new int[k+1];
			
			for (int i = 1; i <= n; i++) {
				stk = new StringTokenizer(br.readLine());
				
				w[i] = Integer.parseInt(stk.nextToken());
				v[i] = Integer.parseInt(stk.nextToken());
			}

			for (int i = 1; i <= n; i++) {
				// K부터 i번 째 물건의 무게까지 거꾸로 탐색하면서 1차원 배열을 갱신해줍니다.
				for (int j = k; j >= w[i]; --j) {
					memo[j] = Math.max(memo[j], memo[j-w[i]] + v[i]);
				}
			}
			
			System.out.println(memo[k]);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

참고 : [https://st-lab.tistory.com/141](https://st-lab.tistory.com/141)

