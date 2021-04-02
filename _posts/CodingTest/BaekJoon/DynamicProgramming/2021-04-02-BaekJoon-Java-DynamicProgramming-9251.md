---
title: "BaekJoon : 9251번(LCS)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Dynamic Programming"
  - "BaekJoon : LCS(Longest Common Subsequence)"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Dynamic Programming

BaekJoon Dynamic Programming(동적 프로그래밍)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 9251

`LCS(Longest Common Subsequence, 최장 공통 부분 수열)`문제는 두 수열이 주어졌을 때, 모두의 부분 수열이 되는 수열 중 가장 긴 것을 찾는 문제이다.

예를 들어, `ACAYKP`와 `CAPCAK`의 `LCS`는 `ACAK`가 된다.



### 입력

**첫째 줄과 둘째 줄에 두 문자열**이 주어진다. **문자열은 알파벳 대문자로만 이루어져 있으며, 최대 1000글자**로 이루어져 있다.

```java
ACAYKP		// 두개의 문자열을 입력합니다.
CAPCAK
```



### 출력

첫째 줄에 입력으로 주어진 두 문자열의 LCS의 길이를 출력한다.

```java
4	// LCS 길이를 출력합니다.
```



### LCS

`LCS`는 `Longest Common Subsequence` 의 줄임말로, **공통 부분 문자열 중 가장 길이가 긴 문자열**을 말합니다.

이 때, `Substring`과 `Subsequence`와의 차이점을 알 필요가 있습니다.

- `Substring` : 전체 문자열에서 연속된 부분 문자열
  - [예] `ABCDEFGHI` 에서 `Substring`은 `ABC`, `DEFG`, `ABCDEF`, `GHI`, … 등
- `Subsequence` : 전체 문자열에서 꼭 연속된 문자열인 것은 아닌 부분 문자열
  - [예] `ABCDEFGHI` 에서 `Subsequence` 는 `ABD`, `AEFGH`, `AFI`, … 등이 있다. 
    - 단, 앞에서부터가 아니라 순서가 뒤바뀐 `IHE`, `BIDEHF` 와 같은 문자열은 부분 문자열이 될 수 없다



이제 `LCS`는, **서로 다른 문자열 중에서 공통 Subsequence를 찾는데,** 그 중 **길이가 가장 긴 Subsequence**를 찾으려 하는 것입니다.

- [예] `ACAYKP`와 `CAPCAK`와의 공통 Subsequence 중 가장 길이가 긴 것
  - **ACA**Y**K**P / C**A**P**CAK** -> `ACAK`
    - 이렇게 두개의 문자열을 비교할 때는 A~Z까지의 순서보다는 공통된 문자열의 순서(`A->C->A->K`)가 더 중요합니다.
    - 부분 수열이기 때문에 문자를 대응시킬 때, 교차가 일어날 수 없습니다.

표를 보면서 한 번 찾아보겠습니다. 표는 각각의 문자열을 나눠 행과 열로 두고 해당 행,열의 값은 공통 `Subsequence`의 최대 값이 됩니다.

예를 들면, 행이 `A(CA)`이고 열이 `A(ACA)`면  공통 부분문자열인 **CA**와 A**CA**의 개수인 2가 나옵니다.

|               |   0   |   A   | C(AC) | A(ACA) | Y(ACAY) | K(ACAYK) | P(ACAYKP) |
| :-----------: | :---: | :---: | :---: | :----: | :-----: | :------: | :-------: |
|     **0**     | **0** | **0** | **0** | **0**  |  **0**  |  **0**   |   **0**   |
|     **C**     | **0** |   0   |   1   |   1    |    1    |    1     |     1     |
|   **A**(CA)   | **0** |   1   |   1   |   2    |    2    |    2     |     2     |
|  **P**(CAP)   | **0** |   1   |   1   |   2    |    2    |    2     |     3     |
|  **C**(CAPC)  | **0** |   1   |   2   |   2    |    2    |    2     |     3     |
| **A**(CAPCA)  | **0** |   1   |   2   |   3    |    3    |    3     |     3     |
| **K**(CAPCAK) | **0** |   1   |   2   |   3    |    3    |    4     |     4     |

표를 보면 두 개의 대상에 대한 원소 별 대응을 처리하기 위해서 2차원 배열로 나타낼 수 있다는 것을 알 수 있습니다.

숫자가 증가하는 부분은  행과 열이 같은 부분 즉, <br>**같은 문자가 나오는 부분**  `[A(CA)][A(ACA)]`에서 값이 증가하고<br>**숫자가 증가 한 뒤에 행과 열이 증가해도 최소 숫자는 증가된 숫자**가 된다는 것을 알 수 있습니다. 

**같은 문자가 나오는 부분**에서 증가하는 이유는 `[A(CA)][A(ACA)]`의 행과 열에서 A가 나오기 전 `[C][C(AC)]`에서 A가 나오는 순간 증가하는 것입니다.<br>즉, **행이 N, 열이 M**이라고 했을 때 **2차원 배열대각선 왼쪽에서 +1**을 한`[N-1][M-1] +1` 로 나타낼 수 있습니다.

**숫자가 증가 한 뒤에 행과 열이 증가해도 최소 숫자는 증가된 숫자**의 경우는 바로 전 열의 수와 바로 전 행의 수를  참조하면 되기 때문에 `[N][M-1]`과 `[N-1][M]`을 비교해 최대값을 구하면 되는 것입니다.

최종적으로 **같은 문자가 나오는 부분**은 `[N-1][M-1] +1`로 나타내고 **같은 문자가 나오지 않는 부분**은 `[N][M-1]`과 `[N-1][M]`을 비교해 최대값을 구하면 되는 것입니다.



### 상향식

반복문을 이용해 1부터 하나씩 채워나가는 상향식 방법입니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String a = br.readLine();
			String b = br.readLine();
			
			int n = a.length();
			int m = b.length();

			int memo[][] = new int[n+1][m+1];
			
			// [1][1]부터 하나씩 채워나갑니다.
			for (int i = 1; i <= n; i++) {
				
				for (int j = 1; j <= m; j++) {
					// 문자가 같게되면 대각선 왼쪽 값에서 +1
					if(b.charAt(j-1) == a.charAt(i-1))
						memo[i][j] = memo[i-1][j-1] + 1;
					// 같지 않으면 행-1 과 열-1 즉, 위와 왼쪽 중 비교해 최대값을 구합니다.
					else
						memo[i][j] = Math.max(memo[i][j-1], memo[i-1][j]);
				}
			}
			
			System.out.println(memo[n][m]);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 하향식

재귀를 이용한 하향식 방법입니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	static Integer memo[][];
	static String a,b;
	
	public static int dp(int n, int m) {
		if(n <= 0 || m <= 0)
			return 0;
		
		if(memo[n][m] == null) {
						
			// 행과 열 즉, 문자들이 같으면 왼쪽 대각선을 참조한 값에 +1을 해줍니다.
			if(a.charAt(n-1) == b.charAt(m-1))
				memo[n][m] = dp(n-1,m-1) + 1;
			// 문자들이 같지 않으면 왼쪽과 위쪽 중 최대값을 넣어줍니다.
			else
				memo[n][m] = Math.max(dp(n-1,m),dp(n,m-1));
		}
		
		return memo[n][m];
		
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			a = br.readLine();
			b = br.readLine();
			
			int n = a.length();
			int m = b.length();

			memo = new Integer[n+1][m+1];
			
			System.out.println(dp(n,m));
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



참고 : [https://chanhuiseok.github.io/posts/algo-34/](https://chanhuiseok.github.io/posts/algo-34/)

[https://edu.goorm.io/lecture/554/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%95%B4%EA%B2%B0%EA%B8%B0%EB%B2%95-%EC%9E%85%EB%AC%B8](https://edu.goorm.io/lecture/554/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%95%B4%EA%B2%B0%EA%B8%B0%EB%B2%95-%EC%9E%85%EB%AC%B8)



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

