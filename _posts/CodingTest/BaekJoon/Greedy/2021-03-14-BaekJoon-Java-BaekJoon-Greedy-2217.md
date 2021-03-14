---
title: "BaekJoon : 2217번(로프)"
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



## 2217

N(1 ≤ N ≤ 100,000)개의 로프가 있다. 이 로프를 이용하여 이런 저런 물체를 들어올릴 수 있다. 각각의 로프는 그 굵기나 길이가 다르기 때문에 들 수 있는 물체의 중량이 서로 다를 수도 있다.

하지만 여러 개의 로프를 병렬로 연결하면 각각의 로프에 걸리는 중량을 나눌 수 있다. k개의 로프를 사용하여 중량이 w인 물체를 들어올릴 때, 각각의 로프에는 모두 고르게 w/k 만큼의 중량이 걸리게 된다.

각 로프들에 대한 정보가 주어졌을 때, 이 로프들을 이용하여 들어올릴 수 있는 물체의 최대 중량을 구해내는 프로그램을 작성하시오. 모든 로프를 사용해야 할 필요는 없으며, 임의로 몇 개의 로프를 골라서 사용해도 된다.



첫째 줄에 정수 N이 주어진다. 다음 N개의 줄에는 각 로프가 버틸 수 있는 최대 중량이 주어진다. 이 값은 10,000을 넘지 않는 자연수이다.

```java
2		// 로프의 개수 입력합니다.
10		
15		// 로프가 버틸 수 있는 중량을 입력합니다.
20		// 각 로프가 버틸 수 있는 최대 중량이 주어집니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.Arrays;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			Integer ropes[] = new Integer[n];
			
			// 로프가 버틸 수 있는 중량을 입력받습니다.
			for (int i = 0; i < n; i++) {
				ropes[i] = Integer.parseInt(br.readLine().trim());
			}
			
			// 오름차순으로 정렬 후
			Arrays.sort(ropes);
			
			int maxWeight = 0;
			// 각 로프에 고르게 중량이 걸리게 되므로 (물건의 중량/로프의 개수)가 최소로프무게 이하여야 합니다.
			// 오름차순으로 된 배열에서 현재로프의 무게 * 현재위치에 ~ 최대로프(n - i) 까지의 숫자를 곱해서 최대 무게를 구합니다.
			// ex) 로프의 무게가 30,60,90 일 때, 30*3, 60*2, 90*1 중에 제일 최대의 무게는 60*2 입니다.
			for (int i = 0; i < ropes.length; i++) {
				int sum = ropes[i] * (ropes.length - i);
				if(maxWeight < sum)
					maxWeight = sum;
			}	
			
			bw.write(Integer.toString(maxWeight));
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