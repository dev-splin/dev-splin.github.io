---
title: "BaekJoon : 14501(퇴사)"
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

BaekJoon Dynamic Programming(동적 프로그래밍)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 14501

상담원으로 일하고 있는 백준이는 퇴사를 하려고 한다.

오늘부터 N+1일째 되는 날 퇴사를 하기 위해서, 남은 N일 동안 최대한 많은 상담을 하려고 한다.

백준이는 비서에게 최대한 많은 상담을 잡으라고 부탁을 했고, 비서는 하루에 하나씩 서로 다른 사람의 상담을 잡아놓았다.

각각의 상담은 상담을 완료하는데 **걸리는 기간 Ti**와 상담을 했을 때 **받을 수 있는 금액 Pi**로 이루어져 있다.

N = 7인 경우에 다음과 같은 상담 일정표를 보자.

|      | 1일  | 2일  | 3일  | 4일  | 5일  | 6일  | 7일  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Ti   | 3    | 5    | 1    | 1    | 2    | 4    | 2    |
| Pi   | 10   | 20   | 10   | 20   | 15   | 40   | 200  |

1일에 잡혀있는 상담은 총 3일이 걸리며, 상담했을 때 받을 수 있는 금액은 10이다. 5일에 잡혀있는 상담은 총 2일이 걸리며, 받을 수 있는 금액은 15이다.

상담을 하는데 필요한 기간은 1일보다 클 수 있기 때문에, 모든 상담을 할 수는 없다. 예를 들어서 **1일에 상담을 하게 되면, 2일, 3일에 있는 상담은 할 수 없게 된다.** 2일에 있는 상담을 하게 되면, 3, 4, 5, 6일에 잡혀있는 상담은 할 수 없다.

또한, **N+1일째에는 회사에 없기 때문에, 6, 7일에 있는 상담을 할 수 없다.**

퇴사 전에 할 수 있는 상담의 최대 이익은 1일, 4일, 5일에 있는 상담을 하는 것이며, 이때의 이익은 10+20+15=45이다.

상담을 적절히 했을 때, **백준이가 얻을 수 있는 최대 수익을 구하는 프로그램을 작성**하시오.



### 입력

첫째 줄에 N (1 ≤ N ≤ 15)이 주어진다.

둘째 줄부터 N개의 줄에 Ti와 Pi가 공백으로 구분되어서 주어지며, 1일부터 N일까지 순서대로 주어진다. (1 ≤ Ti ≤ 5, 1 ≤ Pi ≤ 1,000)

```java
10		// 남은 일수(N)를 입력합니다.
5 50
4 40
3 30
2 20
1 10
1 10
2 20
3 30
4 40
5 50	// 남은 일수(N) 만큼 상담에 걸리는기간(T)와 받을 수 있는 금액(P)를 입력합니다.
```



### 출력

첫째 줄에 백준이가 얻을 수 있는 최대 이익을 출력한다.

```java
90		// 최대 이익을 출력합니다.
```



### 내코드

엄청나게 까다로운 조건은 없습니다!! 하지만 대부분의 DP문제가 그렇듯이 규칙성을 찾고 그 규칙성을 반복문이나 재귀로 구현할 수 있어야 합니다.

여기서 주의할 점은 위의 입출력 예제로 봤을 때, `(5,50) -> (1,10) -> (2,20) = 80(최대이익)` 처럼 끝나는 날 다음 날에 바로 상담을 시작하는 것 보다 `(5,50) -> (1,10) -> (3,30) = 90(최대이익)` 더 큰 이익이 있는 날이 있는 지 체크해야 하는 것입니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {
	static int t[];
	static int p[];
	static int memo[];
	static int n;
	
	public static int dp(int day) {
		if(day > n)
			return 0;
		
		if(memo[day] == 0) {
			
			// "현재 일 수 + 상담에 걸리는 기간 - 1"을 해서 끝나는 날을 구합니다. (끝나는 날 다음부터 상담가능)
			int possibleDay = day + t[day] - 1;
			
			// 상담이 퇴사(n) 전에 끝난다면 (현재 금액 + 상담이 끝나는 다음날의 경우를 체크) 와 
			// 다음날 체크(더 큰경우가 있을 수 있기 때문)를 해서 최대 값을 넣어 줍니다.
			if(possibleDay <= n) 
				memo[day] = Math.max(p[day] + dp(day + t[day]),dp(day+1));
			// 다음 날이 퇴사 전에 끝나지 않으면 다시 다음날 (dp(day+1))을 호출하기 때문에 다른 경우의 수들을 구할 수 있습니다.
			else
				return dp(day+1);
		}
		
		return memo[day];
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			n = Integer.parseInt(br.readLine().trim());
			
			t = new int[n+1];
			p = new int[n+1];
			memo = new int[n+1];
			
			for (int i = 1; i <= n; i++) {
				String nums = br.readLine().trim();
				StringTokenizer stk = new StringTokenizer(nums);
				
				t[i] = Integer.parseInt(stk.nextToken());
				p[i] = Integer.parseInt(stk.nextToken());
			}
			
			int max = 0;
			// 해당하는 날에서 나올 수 있는 최대 이익을 비교한 다음 제일 큰 이익을 출력합니다.
			for (int i = 1; i <= n; i++) {
				int pay = dp(i);
				if(max < pay)
					max = pay;
			}
			
			bw.write(Integer.toString(max));
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

