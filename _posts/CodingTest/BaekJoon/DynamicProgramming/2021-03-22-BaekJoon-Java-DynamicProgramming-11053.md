---
title: "BaekJoon : 11053(가장 긴 증가하는 부분 수열)"
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

BaekJoon Dynamic Programming(동적 프로그래밍)  저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 11053

수열 A가 주어졌을 때, 가장 긴 증가하는 부분 수열을 구하는 프로그램을 작성하시오.

예를 들어, 수열 A = {10, 20, 10, 30, 20, 50} 인 경우에 가장 긴 증가하는 부분 수열은 A = {**10**, **20**, 10, **30**, 20, **50**} 이고, 길이는 4이다.



### 입력

첫째 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000)이 주어진다.

둘째 줄에는 수열 A를 이루고 있는 Ai가 주어진다. (1 ≤ Ai ≤ 1,000)

```java
6		// 수열의 크기(n)을 입력하고
10 20 10 30 20 50	// 수열의 값을 입력해줍니다.
```



### 출력

첫째 줄에 수열 A의 가장 긴 증가하는 부분 수열의 길이를 출력한다.

```java
4		// 가장 긴 증가하는 부분 수열의 최댓값을 출력합니다.
```



### 내코드

DP라고 해서 재귀로만 풀생각을 하다가 생각보다 시간이 많이 걸렸던 문제입니다.. ㅠ 아직 많이 부족한 것 같습니다.. ㅠㅠ 일단 모든 DP와 마찬가지로 조건을 찾아 주어야 합니다.

**가장 긴 증가하는 부분 수열을 만들기 위한 조건**

1. n이 제일 큰 수라고 가정 했을 때 **n번째 수 보다 작은 순서(n-1,n-2...)는 n의 값보다 작아야 합니다.**
2. n번째 수보다 작은 순서(n-1,n-2...)로 오는 수 중, 1번을 만족하고 최댓값이 제일 큰 수를 찾습니다.

1번은 쉽게 알 수 있는데, 2번을 만족하는 수를 찾는 방법이 핵심입니다!<br>여기서 고정된 숫자는 제일 처음에 오는 수 입니다. 처음에 오는 수는 배열에 자기 자신 밖에 없기 때문에 1의 값을 가지기 때문이죠!

첫 번째 부터 차근차근 경우의 수를 따져가보면 쉽게 찾을 수 있습니다. {10 20 10 30 20 50}이 있을 때 경우의 수를 `순서 -> 올 수 있는 경우의 수`로 표현하겠습니다.

`1 -> 1(10)`

`2 -> 2(20)1(10)`

`3 -> 3(10)`

`4 -> 4(30)2(20)1(10)`

`5 -> 20(5)3(10)`

이렇게 차근차근 보다보면 **n의 값보다 작은 수 중(1번 조건)**에서 **최댓값이 제일 큰 경우(2번 조건)**를 가져와 자기자신을 더해주는 것을 볼 수 있습니다.(10 같이 하나만 입력되는 경우 제외)  그렇기 때문에 처음부터 순회하면서 **n번째 일 때, 1번부터 체크하며 n번째의 값보다 작고, 최댓값을 비교해서 최댓값이 제일 큰 경우를 가져와 +1**을 해주면 되겠습니다.

저 같은 경우 첫 번째부터 순서대로 입력하였기 때문에 입력하는 동시에 체크해주었습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
	static int memo[];
	static int nums[];
	
	// n까지의 수열을 순회하여 n의 값보다 작고 count로 최댓값을 비교해 최댓값이 큰 값을 memo[n]에 담아줍니다.
	public static void dp(int n) {
		if(n == 0)
			return;
		if(n == 1)
			return;
		
		if(memo[n] == 0) {
			int count = 1;
			for (int i = 1; i < n; i++) {
				if(nums[i] < nums[n] && count < memo[i] + 1)
					count = memo[i] + 1;
			}
			memo[n] = count;
		}
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			
			memo = new int[n+1];
			nums = new int[n+1];
			
			String inputNums = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(inputNums);
			
			memo[0] = 0;
			memo[1] = 1;
			
			// 첫 번째 수열 부터 입력하는 동시에 dp를 수행해 줍니다.
			for (int i = 1; i < nums.length; i++) {
				nums[i] = Integer.parseInt(stk.nextToken());
				dp(i);
			}
			
			// 최대값이 섞여 있기 때문에 오름차순 정렬 후 마지막에 있는 값을 출력합니다.
			Arrays.sort(memo);

			bw.write(Integer.toString(memo[n]));
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

