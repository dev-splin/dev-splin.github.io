---
title: "BaekJoon : 10844(쉬운 계단 수)"
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



## 10844

45656이란 수를 보자.

이 수는 **인접한 모든 자리수의 차이가 1이 난다. 이런 수를 계단 수**라고 한다.

세준이는 수의 길이가 N인 계단 수가 몇 개 있는지 궁금해졌다.

N이 주어질 때, 길이가 N인 계단 수가 총 몇 개 있는지 구하는 프로그램을 작성하시오. (0으로 시작하는 수는 없다.)



### 입력

첫째 줄에 N이 주어진다. N은 1보다 크거나 같고, 100보다 작거나 같은 자연수이다.

```java
2	// 자리 수(n)를 입력합니다.
```



### 출력

첫째 줄에 정답을 1,000,000,000으로 나눈 나머지를 출력한다.

```java
17	// 자리 수(n)에서 나올 수 있는 계단 수를 출력합니다.
```



### 코드

**핵심은 인접한 모든 자리수의 차이가 1이 난다는 것**입니다. 이 때, 자리수는 자연수와 다릅니다. 123456이 있다면 **자연수는 오른쪽부터 첫번째 자리** 라고 하지만, **자리수는 왼쪽부터 첫번째 자리**라고 할 수 있습니다. 그렇기 때문에 자연수는 0으로 시작하는게 불가능 하지만, 자리수는 123450 처럼 0으로 시작할 수 있습니다. 

인접한 자리수와의 차이가 1이어야 하기 때문에, 특정 **자리수에 0이나 9가 오게 되면 인접 자리수에는 1과 8밖에 올 수 없습니다.** n의 자리수는이전 자리수(n-1)를 이용할 수 있습니다. 예를 들어, 3으로 시작하는 3자리수는 `123, 323, 343, 543` 처럼 1의 차이가 나는 이전 자리수(n-1) 즉, 2자리수의 `12,32(2로시작), 34,54(4로시작)`를 이용할 수 있습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	static Long nums[][];
	final static long MOD = 1000000000; 
	
	public static long dp(int num, int digit) {
		if(digit == 1)
			return nums[num][1];
		
		if(nums[num][digit] == null) {
			// 자리수가 0으로 시작하면 다음 자리수는 1밖에 올 수 없습니다.
			if(num == 0)
				nums[0][digit] = dp(1, digit - 1);
			// 자리수가 9으로 시작하면 다음 자리수는 8밖에 올 수 없습니다.
			else if(num == 9)
				nums[9][digit] = dp(8, digit - 1);
			// 자리수가 1~8로 시작하면 1의 차이가나는 이전 자리수(n-1,n+1)의 합만큼 올 수 있습니다.
			else
				nums[num][digit] = dp(num - 1, digit - 1) + dp(num + 1, digit - 1);
		}
		
		return nums[num][digit] % MOD;
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			
			nums = new Long[10][n+1];
			
			// 자리수가 1개일 때, 0~9 로시작하는 자리수의 값을 넣어줍니다.
			// 이 때, 0으로 시작할 수 없지만 자리수가 2개 일 때 0을 이용하게 되므로 1을 넣어줍니다.
			for (int i = 0; i <= 9; i++) {
				nums[i][1] = 1L;
			}
			
			long result = 0;
			// 특정 숫자로 시작하는 n의 자리수의 경우의 수를 전부 더해줍니다.
			for (int i = 1; i <= 9; i++) {
				result += dp(i,n);
			}
			
			bw.write(Long.toString(result % MOD));
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

