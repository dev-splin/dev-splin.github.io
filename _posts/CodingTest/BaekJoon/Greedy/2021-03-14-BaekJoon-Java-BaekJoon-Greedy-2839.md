---
title: "BaekJoon : 2839번(설탕 배달)"
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



## 2839

상근이는 요즘 설탕공장에서 설탕을 배달하고 있다. 상근이는 지금 사탕가게에 설탕을 정확하게 N킬로그램을 배달해야 한다. 설탕공장에서 만드는 설탕은 봉지에 담겨져 있다. 봉지는 3킬로그램 봉지와 5킬로그램 봉지가 있다.

상근이는 귀찮기 때문에, 최대한 적은 봉지를 들고 가려고 한다. 예를 들어, 18킬로그램 설탕을 배달해야 할 때, 3킬로그램 봉지 6개를 가져가도 되지만, 5킬로그램 3개와 3킬로그램 1개를 배달하면, 더 적은 개수의 봉지를 배달할 수 있다.

상근이가 설탕을 정확하게 N킬로그램 배달해야 할 때, 봉지 몇 개를 가져가면 되는지 그 수를 구하는 프로그램을 작성하시오.



첫째 줄에 N이 주어진다. (3 ≤ N ≤ 5000)

상근이가 배달하는 봉지의 최소 개수를 출력한다. 만약, 정확하게 N킬로그램을 만들 수 없다면 -1을 출력한다.

```java
18	// N킬로그램을 입력하면
4	// 제일 봉지수가 적은 수를 출력합니다.

4	// N킬로그램을 입력했는데
-1	// 만들 수 없다면 -1을 출력합니다.
```



### 내코드

```java
package Greedy;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class g2839 {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		// 입력되는 n의 최대 수를 3으로 나눠 제일 많은 봉지가 나오는 경우를 나타냅니다.
		// 최소 봉지를 비교할 때 사용됩니다.
		final int MAX_BAG = 5000 / 3;
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			// n이 범위를 벗어나면 프로그램을 종료합니다.
			if(n < 3 || n > 5000)
				return;
			
			// 3과 5킬로그램 봉지로 만들 수 있는 n의 경우의 수를 구할 때 
			// n을 3킬로와 5킬로로 만들 수 있는 최대 숫자를 구해준 다음에 순회해줍니다.
			int maxThreeKiloBag = n / 3;
			int maxFiveKiloBag = n / 5;
			
			// 3킬로 와 5킬로로 만들 수 있는 경우의 수를 담는 변수
			int TotalBag = 0;
			// 3킬로와 5킬로로 만들 수 있는 최소 봉지의 수를 담는 변수
			int minTotalBag = MAX_BAG;
			
			// n을 3킬로로 만들 수 있는 최대수와, 5킬로로 만들 수 있는 최대수 만큼 반복합니다.
			for (int i = 0; i <= maxThreeKiloBag; i++) {
				for (int j = 0; j <= maxFiveKiloBag; j++) {
					// 3킬로 봉지 i개와 5킬로 봉지 j개를 이용해 n과 비교하기 위한 킬로그램을 만듭니다.
					int sum = 3*i + 5*j;
					
					// 만든 킬로그램 sum과 n이 일치하고 sum의 봉지개수가 현재 minTotalBag보다 작으면 minTotalBag을 갱신해줍니다. 
					if(sum == n && minTotalBag > i + j)
						minTotalBag = i + j;
				}
			}
			
			// 처음에 설정한 minTotalBag이 바뀌지 않았으면 경우의 수가 없다는 뜻이므로 -1을 넣습니다.
			if(minTotalBag == MAX_BAG)
				minTotalBag = -1;
			
			// 출력
			bw.write(Integer.toString(minTotalBag));
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



### 더 좋은 방법

```java
	// 제일 최소의 수가 나오려면 5킬로가 많이 사용되어야 합니다.
	 public static void main(String args[]){
		 	// 입력 받을 변수
	        int input = 0; 
	        // 3킬로 봉지의 개수를 저장할 변수
	        int count = 0;
	        Scanner sc = new Scanner(System.in);
	        input = sc.nextInt();
	        
	        while(true) {
	        	// 입력받은 수가 5로 나누어 떨어지는지 보고 나누어 떨어진다면 
	        	// count(3킬로 봉지의 개수)와 수를 5로나눈 몫(5킬로 봉지의 개수)을 출력합니다.
	        	// 0도 나머지가 0입니다. 5로 나누어 떨어지지 않아도 즉, 3으로만 나누어 떨어져 0이되도 조건이 성립합니다.
	            if (input % 5 ==0) {
	                System.out.println(input/5 + count);	// 
	                break;
	            // 밑에서 5로 나누어 떨어지지않으면 계속해서 3을 빼주게 되므로 input이 0이하가 되면
	            // 5와 3으로 숫자를 표현할 수 없다는 의미가 되므로 불가능하다는 -1을 표시해주고 종료합니다.
	            }else if(input <= 0) {
	                System.out.println(-1);
	                break;
	            }
	            // 위에서 5로 나누어 떨어지지 않으면 -3을 해주고 3킬로 봉지의 개수를 1개 늘립니다.
	            input = input-3;
	            count++;
	        }
	    }
```

참고 : [https://devjjo.tistory.com/40](https://devjjo.tistory.com/40)



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.