---
title: "BaekJoon : 11399번(ATM)"
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



## 11399

인하은행에는 ATM이 1대밖에 없다. 지금 이 ATM앞에 N명의 사람들이 줄을 서있다. 사람은 1번부터 N번까지 번호가 매겨져 있으며, i번 사람이 돈을 인출하는데 걸리는 시간은 Pi분이다.

사람들이 줄을 서는 순서에 따라서, 돈을 인출하는데 필요한 시간의 합이 달라지게 된다. 예를 들어, 총 5명이 있고, P1 = 3, P2 = 1, P3 = 4, P4 = 3, P5 = 2 인 경우를 생각해보자. [1, 2, 3, 4, 5] 순서로 줄을 선다면, 1번 사람은 3분만에 돈을 뽑을 수 있다. 2번 사람은 1번 사람이 돈을 뽑을 때 까지 기다려야 하기 때문에, 3+1 = 4분이 걸리게 된다. 3번 사람은 1번, 2번 사람이 돈을 뽑을 때까지 기다려야 하기 때문에, 총 3+1+4 = 8분이 필요하게 된다. 4번 사람은 3+1+4+3 = 11분, 5번 사람은 3+1+4+3+2 = 13분이 걸리게 된다. 이 경우에 각 사람이 돈을 인출하는데 필요한 시간의 합은 3+4+8+11+13 = 39분이 된다.

줄을 [2, 5, 1, 4, 3] 순서로 줄을 서면, 2번 사람은 1분만에, 5번 사람은 1+2 = 3분, 1번 사람은 1+2+3 = 6분, 4번 사람은 1+2+3+3 = 9분, 3번 사람은 1+2+3+3+4 = 13분이 걸리게 된다. 각 사람이 돈을 인출하는데 필요한 시간의 합은 1+3+6+9+13 = 32분이다. 이 방법보다 더 필요한 시간의 합을 최소로 만들 수는 없다.

줄을 서 있는 사람의 수 N과 각 사람이 돈을 인출하는데 걸리는 시간 Pi가 주어졌을 때, 각 사람이 돈을 인출하는데 필요한 시간의 합의 최솟값을 구하는 프로그램을 작성하시오.



첫째 줄에 사람의 수 N(1 ≤ N ≤ 1,000)이 주어진다. 둘째 줄에는 각 사람이 돈을 인출하는데 걸리는 시간 Pi가 주어진다. (1 ≤ Pi ≤ 1,000)

첫째 줄에 각 사람이 돈을 인출하는데 필요한 시간의 합의 최솟값을 출력한다.

```java
5	// 사람 수를 입력하고
3 1 4 3 2	// 인출하는데 걸리는 시간을 입력하면
32	// 인출하는데 필요한 시간의 최소값을 출력합니다.
```



### 내코드

`list`에 저장해서 `Collections.sort()`를 이용해 풀었으나, 입력할 개수가 정해져 있기 때문에 배열로 저장해서 `Arrays.sort()`를 이용하는 게 더 좋을 것 같습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.LinkedList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			
			// 범위를 벗어나면 프로그램 종료
			if(1 > n || 1000 < n)
				return;
			
			// 문자열을 입력받고
			String times = br.readLine().trim();
			// 공백 단위로 문자열을 토큰으로 쪼갠 후
			StringTokenizer stk = new StringTokenizer(times);
			
			// 토큰의 개수가 입력될 개수와 맞지 않으면 프로그램 종료
			if(stk.countTokens() != n)
				return;
			
			// 입력받은 수를 저장할 리스트
			List<Integer> list = new ArrayList<>();
			
			// 토큰이 있는 지 체크하고 있으면 정수로 변환 후 범위 체크를 해주고 list에 추가합니다.
			while(stk.hasMoreTokens())
			{
				int time = Integer.parseInt(stk.nextToken());
				if(time < 1 || time > 1000)
					return;
				list.add(time);				
			}
			
			// list 정렬합니다 ( 기본 오름차순)
			Collections.sort(list);
			
			// 최소 시간의 합을 저장할 변수
			int sum = 0;
			// 이전 출금의 시간을 저장할 변수
			int preNum = 0;
			
			// list를 순회하여 preNum에 현재 시간을 더한 후 그 더한 값을 sum에 더해 총 시간을 구해줍니다.
			for(int time : list)
			{
				preNum += time;
				sum += preNum;
			}
			
			// 출력
			bw.write(Integer.toString(sum));
			
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