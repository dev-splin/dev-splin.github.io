---
title: "BaekJoon : 11047번(동전 0)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Greedy"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Greedy

BaekJoon Greedy  저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 11047

준규가 가지고 있는 동전은 총 N종류이고, 각각의 동전을 매우 많이 가지고 있다.

동전을 적절히 사용해서 그 가치의 합을 K로 만들려고 한다. 이때 필요한 동전 개수의 최솟값을 구하는 프로그램을 작성하시오.



첫째 줄에 N과 K가 주어진다. (1 ≤ N ≤ 10, 1 ≤ K ≤ 100,000,000)

둘째 줄부터 N개의 줄에 동전의 가치 Ai가 오름차순으로 주어진다. (1 ≤ Ai ≤ 1,000,000, A1 = 1, i ≥ 2인 경우에 Ai는 Ai-1의 배수)

```java
10 4200	// 동전의 종류 수와 금액을 입력합니다.
1
5
10
50
100
500
1000
5000
10000
50000	// 동전의 종류 수만큼 동전을 입력합니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			// 코인 종류 개수와 가격을 입력받습니다.
			String coinTypeCountAndPrice = br.readLine().trim();
			
			// 공백을 기준으로 토큰으로 나눕니다.
			StringTokenizer stk = new StringTokenizer(coinTypeCountAndPrice);
			
			// 나눈 토큰 개수가 2개가 아니면 프로그램 종료(2개를 입력하지 않으면 종료)
			if(stk.countTokens() != 2)
				return;
			
			// 토큰을 가져와 변수에 담습니다.
			int coinTypeCount = Integer.parseInt(stk.nextToken());
			int price = Integer.parseInt(stk.nextToken());
			
			// 코인 종류 개수만큼 배열을 만들어 줍니다.
			int coin[] = new int[coinTypeCount];
			
			// 배열방에 값 입력
			for (int i = 0; i < coin.length; i++)
				coin[i] = Integer.parseInt(br.readLine().trim());
			
			// 배열방을 순회하기 위한 변수
			int i = coin.length - 1;
			// 가격을 형성하는데 필요한 총 코인 개수
			int totalCoinCount = 0;
			
			// 가격이 0이하가 될 때 까지 루프를 돕니다.
			// "가격/코인종류" 로 나온 몫으로 코인 개수를 판단하고 가격에 코인개수*코인종류를 빼줍니다.
			while(price > 0) {
				int quotient = price / coin[i];
				if(quotient != 0) {
					price -= (coin[i] * quotient);
					totalCoinCount += quotient;
				}
				--i;
			}
			
			bw.write(Integer.toString(totalCoinCount));
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