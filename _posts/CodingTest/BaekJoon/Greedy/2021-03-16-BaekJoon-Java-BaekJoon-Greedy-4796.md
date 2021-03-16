---
title: "BaekJoon : 4796번(캠핑)"
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



## 4796

등산가 김강산은 가족들과 함께 캠핑을 떠났다. 하지만, 캠핑장에는 다음과 같은 경고문이 쓰여 있었다.

캠핑장은 **연속하는 20일 중 10일동**안만 사용할 수 있습니다.

강산이는 이제 막 28일 휴가를 시작했다. 이번 휴가 기간 동안 강산이는 캠핑장을 며칠동안 사용할 수 있을까?

강산이는 조금 더 일반화해서 문제를 풀려고 한다. 

캠핑장을 **연속하는 P일 중, L일동안만 사용**할 수 있다. 강산이는 이제 막 V일짜리 휴가를 시작했다. 강산이가 캠핑장을 최대 며칠동안 사용할 수 있을까? (1 < L < P < V)



입력은 여러 개의 테스트 케이스로 이루어져 있다. 각 테스트 케이스는 한 줄로 이루어져 있고, L, P, V를 순서대로 포함하고 있다. 모든 입력 정수는 int범위이다. 마지막 줄에는 0이 3개 주어진다.

```java
5 8 20	// L와 P,V를 입력합니다
5 8 17
0 0 0	// 0,0,0을 입력하면 입력종료
Case 1: 14	// Case 1(5,8,20)의 결과
Case 2: 11	// Case 2(5,8,17)의 결과가 출력됩니다.
```



### 내코드

연속해서 5일만 사용할 수 있기 때문에 사용할 수 있는 날을 1이라고 할 때, `5(L)` `8(P)` `10(V)` 이 들어오면

`11111 000 11` 처럼 반드시 0이 중간에 들어가게 되는데, 이 때 **연속된날 8(P)로 나눠진다는 것**을 알 수 있습니다. <br>최종적으로 사용할 수 있는 날은 `10(V)`를 `8(P)`로 나누고 `남은날(2)`가 `5(L)`보다 작으면 그냥 더해주고, 크면 그냥 `5(L)`를 더해서 구할 수 있습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			List<Integer> testCases = new ArrayList<>();
			
			while(true) {
				String testCase = br.readLine().trim();
				StringTokenizer stk = new StringTokenizer(testCase);
				
				// l,p,v를 토큰으로 나눈 후 받아옵니다.
				int l = Integer.parseInt(stk.nextToken());
				int p = Integer.parseInt(stk.nextToken());
				int v = Integer.parseInt(stk.nextToken());
				
				// 0이 입력되면 종료
				if (l == 0 && p == 0 && v == 0)
					break;
				
				// 총 사용할 수 있는 날을 저장하는 변수
				int availableDays = (v / p) * l;
				// 남은 날을 저장하는 변수
				int restDays = v % p;
				
				// 남은 날이 사용할 수 있는 날(l)보다 작으면 바로 총사용할 날에 더하고 아니면 l을 더합니다. 
				if(restDays < l)
					availableDays += restDays;
				else
					availableDays += l;

				testCases.add(availableDays);
			}
			
			// 출력
			for (int i = 0; i < testCases.size(); i++) {
				bw.write("Case " + Integer.toString(i+1) + ": " + Integer.toString(testCases.get(i)));
				bw.newLine();
			}
			
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