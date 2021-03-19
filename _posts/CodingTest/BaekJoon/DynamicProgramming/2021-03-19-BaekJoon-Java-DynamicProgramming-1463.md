---
title: "BaekJoon : 1463번(1로 만들기)"
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



## 1463

정수 X에 사용할 수 있는 연산은 다음과 같이 세 가지 이다.

1. X가 3으로 나누어 떨어지면, 3으로 나눈다.
2. X가 2로 나누어 떨어지면, 2로 나눈다.
3. 1을 뺀다.

정수 N이 주어졌을 때, 위와 같은 연산 세 개를 적절히 사용해서 1을 만들려고 한다. 연산을 사용하는 횟수의 최솟값을 출력하시오.



### 입력

첫째 줄에 1보다 크거나 같고, 10<sup>6</sup>보다 작거나 같은 정수 N이 주어진다.

```java
10
```



### 출력

첫째 줄에 연산을 하는 횟수의 최솟값을 출력한다.

```java
3
```



### 내코드

3가지 방식으로 풀었습니다. 코드를 보면서 주석으로 설명하겠습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	// memoization
	static Integer memo[];
	
	// memoization(memo 배열)을 이용한 Top-Down 방식입니다.
	// memo의 해당 인덱스의 값이 그 숫자를 연산할 수 있는 최솟값 입니다.
	// 해당 인덱스에 값이 없으면 재귀를 이용해 나올 수 있는 경우의 수 중 최소의 값을 구해 저장해둡니다. 
	// 해당 인덱스의 값이 있으면 저장되어져 있는 값을 불러 옴으로써 연산의 양을 줄일 수 있습니다.
	public static int recursive1(int n) {
		
		if(memo[n] == null) {
			// 2와 3의 최소공배수인 6으로 나누어 떨어질 경우에는 1을 빼는 경우, 3으로 나누는 경우, 2로 나누는 경우가 나올 수있습니다.
			// 재귀를 이용해 1을 빼는 경우, 3으로 나누는 경우, 2로 나누는 경우로 시작해 1까지의 경우의 수를 구한 후 최소값을 memo[n]에 저장합니다.
			if(n % 6 == 0) {
				memo[n] = Math.min(recursive1(n-1), Math.min(recursive1(n/3), recursive1(n/2))) + 1;
			} else if(n % 3 == 0) {
				memo[n] = Math.min(recursive1(n/3), recursive1(n-1)) + 1;
			} else if(n % 2 == 0) {
				memo[n] = Math.min(recursive1(n/2), recursive1(n-1)) + 1;
			} else {
				memo[n] = recursive1(n-1) + 1;
			}
		}
				
		// 처음 재귀를 호출할 때는 값이 있는 인덱스는 1과 0뿐(처음에 초기화)이므로
		// 인덱스가 1인, 즉 목표에 도달하게 됩니다. 이 때, 인덱스 1의 값인 0부터 리턴 하면서
		// +1을 더하면서 memo[n]의 값(n의 경우의수 최솟값)을 정해줍니다.(ex) memo[1] = 0, memo[2] = 1)
		// 처음 재귀에는 값이 있는 인덱스가 인덱스 1과 0뿐이지만 재귀를 진행할수록 값이 채워지는 인덱스가 많이지기 때문에 연산의 양이 줄어듭니다.
		return memo[n];
	}
	
	// memoization을 사용하지 않고 count를 누적해서 결과를 구해줍니다.
	// 연산속도가 빠르나 코드가 직관적이지 않아 조금 이해하기 힘든 경향이 있습니다.
	public static int recursive2(int n, int count) {
		if(n < 2)
			return count;
		
		// "recursive2(n/2, count + 1 + n % 2)" 는 n/2로 나누어 떨어지지 않으면 나머지(n%2)가 count에 추가됩니다.
		// 즉, n/2로 나누어 떨어질 때까지 -1 해주고 n/2 해주는 것과 같습니다. 
		// ex) 11 -> 10(11-1, count 1(11%2)증가) -> 5(10/2, count 1증가) 
		
		// "recursive2(n/3, count + 1 + n % 3)"도 마찬가지입니다. 
		// ex) 11 -> 10 -> 9(11-1-1, count 2(11%3)증가) -> 3(9/3, count1증가)
		
		// 이런 경우 중 최소값을 가지는 경우를 찾기 때문에 memoization을 쓰지 않고 연산도 짧아 지기 때문에
		// 시간복잡도, 공간복잡도 모두 줄어드는 것을 볼 수 있습니다.
		return Math.min(recursive2(n/2, count + 1 + n % 2), recursive2(n/3, count + 1 + n % 3));
	}
	
	// Bottom-Up 푸는 방식입니다.
	// 전체 memoization을 순회하면서 일단 바로 전의 인덱스(i-1)에서 +1을 해주고 (i에서 i-1로 가려면 연산을 한번 해야하기 때문)
	// 임의의 수 i가 3이나 2로 나누어 떨어지면 i/3, i/2 인덱스의 최솟값에 +1을 한 값을 넣고 비교 후 제일 작은 수를 인덱스의 값으로 합니다. 
	public static void recursive3() {
		
		for (int i = 2; i < memo.length; i++) {
			memo[i] = memo[i-1] +1;
			
			if(i % 3 == 0 && memo[i/3] < memo[i])
				memo[i] = memo[i/3] + 1;
			
			if(i % 2 == 0 && memo[i/2] < memo[i])
				memo[i] = memo[i/2] + 1;
		}
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			
			int n = Integer.parseInt(br.readLine().trim());
			
			// memoization을 사용하기 위한 배열을 초기화 해주고
			// 0,1번 인덱스에 0을 넣어줍니다. (1번 인덱스에서 1까지 가는 경우는 0(자기자신)이기 때문, 0의 경우 해당X)
			memo = new Integer[n + 1];
			memo[0] = memo[1] = 0;
			
			//recursive1 사용법
//			bw.write(Integer.toString(recursive1(n)));
			//recurscive2 사용법
//			bw.write(Integer.toString(recursive2(n,0)));	
			
			// recursive3 사용법
			recursive3();
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

참고 : https://st-lab.tistory.com/133

https://odysseyj.tistory.com/19



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.