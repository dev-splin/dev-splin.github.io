---
title: "BaekJoon : 1010(다리 놓기)"
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



## 1010

재원이는 한 도시의 시장이 되었다. 이 도시에는 도시를 동쪽과 서쪽으로 나누는 큰 일직선 모양의 강이 흐르고 있다. 하지만 재원이는 다리가 없어서 시민들이 강을 건너는데 큰 불편을 겪고 있음을 알고 다리를 짓기로 결심하였다. 강 주변에서 다리를 짓기에 적합한 곳을 사이트라고 한다. 재원이는 강 주변을 면밀히 조사해 본 결과 **강의 서쪽에는 N개의 사이트**가 있고 **동쪽에는 M개의 사이트**가 있다는 것을 알았다. (N ≤ M)

재원이는 서쪽의 사이트와 동쪽의 사이트를 다리로 연결하려고 한다. (이때 한 사이트에는 최대 한 개의 다리만 연결될 수 있다.) 재원이는 다리를 최대한 많이 지으려고 하기 때문에 **서쪽의 사이트 개수만큼 (N개) 다리를 지으려고 한다.** **다리끼리는 서로 겹쳐질 수 없다고 할 때 다리를 지을 수 있는 경우의 수를 구하는 프로그램을 작성**하라.

![img](https://www.acmicpc.net/upload/201003/pic1.JPG)



### 입력

**입력의 첫 줄에는 테스트 케이스의 개수 T**가 주어진다. **그 다음 줄부터 각각의 테스트케이스에 대해 강의 서쪽과 동쪽에 있는 사이트의 개수 정수 N, M (0 < N ≤ M < 30)이 주어진다.**

```java
3		// 테스트 케이스(T)의 개수를 입력하고
2 2
1 5
13 29	// 사이트의 개수(N,M)을 입력합니다.
```



### 출력

각 테스트 케이스에 대해 주어진 조건하에 다리를 지을 수 있는 경우의 수를 출력한다.

```java
1
5
67863915	// 다리를 지을 수 있는 최대 경우의 수를 출력합니다.
```



### 규칙을 찾아 해결하는 방법

N을 2라고 가정하고 경우의 수 규칙을 찾아보겠습니다. `(N,M) -> (N의 번호, M의 번호 / N의 번호, M의 번호) ` 방식으로 표현합니다.

`(2,2) -> (1,1/2,2)`

`(2,3) -> (1,1/2,2)(1,1/2,3)(1,2/2,3)`

`(2,4) -> (1,1/2,2)(1,1/2,3)(1,1/2,4) (1,2/2,3)(1,2/2,4) (1,3/2,4)`

여기 까지보면 뭔가 패턴이 보이려고 합니다. **M이 4일 때 경우**에서 **M이 3일 때의 경우**가 보입니다. **M이 4인 경우**에서 **M이 3인 경우**를 뺀 `(1,1/2,4)(1,2/2,4)(1,3/2,4)`를 보면 **N의 두번째와 M의 네번째를 고정**해놓고 **N의 1, M이 3을 가져온 것**과 같습니다. 그림으로 보면 더 이해가 빠릅니다.

<img src="https://blog.kakaocdn.net/dn/bH7im6/btqEBP6JiOV/nwwwTgpGkwo6wrck9OeM9k/img.png" alt="img" style="zoom:80%;" />

<img src="https://blog.kakaocdn.net/dn/x7gqu/btqECv7LNFP/m3K7w43KR1KPCctq5NEgRK/img.png" alt="img" style="zoom:80%;" />

이 경우에서 (2,4) = (2,3) + (1,3) 즉, (N,M) = (N,M-1) + (N-1,M-1)인 것을 알 수 있습니다. 그럼 이 식을 이용해 문제를 해결하면 되겠습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {
	static int memo[][] = new int[30][30];
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			int t = Integer.parseInt(br.readLine().trim());
			
			// 1-> i는 i만큼 만들 수 있으니 i로 초기화 해줍니다.
			for (int i = 1; i <= 29; i++)
				memo[1][i] = i;
						
			for (int i = 0; i < t; i++) {
				String nums = br.readLine();
				StringTokenizer stk = new StringTokenizer(nums);
				
				int n = Integer.parseInt(stk.nextToken());
				int m = Integer.parseInt(stk.nextToken());
				
				// 경우의 수를 구합니다.
				for (int j = 2; j <= n; j++) {
					for (int k = j; k <= m; k++) {
						// memo에 이미 값이 있다면 건너뜁니다.
						if(memo[j][k] > 0)
							continue;
						
						// j == k의 경우의 수는 1개 밖에 없습니다.
						if(j == k)
							memo[j][k] = 1;
						else
							memo[j][k] = memo[j-1][k-1] + memo[j][k-1];
					}
				}
				
				System.out.println(memo[n][m]);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 조합 공식을 사용한 방법

결국 문제는 M개의 사이트에 N개를 배치하는 것 즉, **M개 중 N개를 선택**해야 한다는 것입니다. 이 때, 사용할 수 있는 것이 [조합공식](https://st-lab.tistory.com/159#%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)입니다. 

`조합공식`의 파스칼의 삼각형을 보면 **1번성질**( $_nC_r = _{n-1}C_{r-1} + _{n-1}C_r$ )과 **2번성질**( $_nC_0 = _nC_n = 1$ ) 을 알 수 있는데, 이 때, **n이 M, r이 N을 나타내고** 이 `조합공식`의 성질을 이용하면 문제를 쉽게 해결할 수 있습니다.

```java
package Dynamic_Programming;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class d1010_2 {
	static int memo[][] = new int[30][30];
	
	// 조합 공식을 이용해 경우의 수를 찾습니다. 
	public static int dp(int n, int r) {
				
		// 조합 공식의 성질 2
		if(n == r || r == 0)
			return memo[n][r] = 1;
		
		// 조합 공식의 성질 1
		if(memo[n][r] == 0)
			return memo[n][r] = dp(n-1,r-1) + dp(n-1,r);
		
		return memo[n][r];
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			int t = Integer.parseInt(br.readLine().trim());
			
			for (int i = 0; i < t; i++) {
				String nums = br.readLine();
				StringTokenizer stk = new StringTokenizer(nums);
				
				int n = Integer.parseInt(stk.nextToken());
				int m = Integer.parseInt(stk.nextToken());
				
				System.out.println(dp(m,n));
				}
				
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



참고 : [https://yeoeun-ji.tistory.com/60](https://yeoeun-ji.tistory.com/60)

[https://st-lab.tistory.com/194](https://st-lab.tistory.com/194)



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

