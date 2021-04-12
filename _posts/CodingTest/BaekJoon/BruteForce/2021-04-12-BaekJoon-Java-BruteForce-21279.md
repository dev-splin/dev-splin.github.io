---
title: "BaekJoon : 21279번(광부 호석)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Two Pointers"
  - "BaekJoon : Priority Queue"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Brute Force

BaekJoon Brute Force(브루트포스)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 21279

***N* 개의 광물**이 있다. **i 번째 광물은 (*Xi* , *Yi* )**에 있으며 **캐내는 비용은 1**이고, 이것의 **아름다운 정도는 *Vi*** 이다.

**호석이는 지금 (0, 0)**에 있다. 타고난 광부인 호석이는 시그니쳐 스킬인 "광산 뒤집기"를 쓰려고 한다. 이 스킬을 쓰면 **자신이 있는 위치를 꼭지점으로 하며, 원하는 높이 *H(H ≥ 0)*, 너비 *W(W ≥ 0)* 인 직사각형 영역 안에 있는 모든 광물을 캘 수 있다. 영역의 테두리에 존재하는 광물도 캐야한다.**

주의할 점은, **직사각형 영역 안에 들어오는 광물은 무조건** 캐야 하며, **영역에 속한 광물들의 캐내는 비용의 총합이 현재 가진 돈 *C* 보다 크면 파산**을 하게 된다.

호석이가 파산하면 광물을 못 캐고, 이로 인해 상상을 초월하는 나비효과가 발생해서 한국의 취직율이 떨어진다!!!!!!! 대신 호석이가 얻을 수 있는 광물들의 아름다운 정도의 합이 높아질수록 한국의 취직율은 올라간다!!!!!!!! 모두의 행복을 위해서 **호석이가 파산하지 않고 얻을 수 있는 광물들의 아름다운 정도를 최대화**하자.



### 입력

**첫 줄에 광물의 개수 *N* 과 호석이가 가진 돈 *C*** 가 주어진다.

이어서 **N개의 줄에 걸쳐서, i번째 줄에 3개의 정수 *Xi* , *Yi* , *Vi*** 가 주어진다. 각 숫자는 i 번째 광물이 위치한 X, Y 좌표와 아름다운 정도를 의미한다.

```java
5 3		// 광물의 개수(N)과 호석이가 가진 돈(C)을 입력합니다.
1 10 10	// 여기부터
2 4 1
3 8 10
4 5 5
5 7 6	// 여기까지 X,Y,V를 입력합니다.
```



### 출력

호석이가 파산하지 않으면서 얻을 수 있는 광물들의 아름다운 정도의 합의 최댓값을 출력하라.

```java
21	// V합의 최대값을 출력합니다.
```



### 제한

**1 ≤ *N* ≤ 500,000** 

**0 ≤ Xi , Yi ≤ 100,000**

**1 ≤ Vi ≤ 10<sup>8</sup>**

**1 ≤ C ≤ *N***

**광물들은 모두 서로 다른 위치에 존재하며, (0, 0) 에는 광물이 존재하지 않음이 보장된다. 주어지는 모든 수는 정수**이다.



### 투 포인터

[Gravekper](https://www.youtube.com/watch?v=Zt38XizwbAw)님의 방법을 참고했습니다.

`x와 y를 포인터(투 포인터)`로 사용하여 푸는 방법입니다.<br>문제는 사각형을 만들어 그 범위내의 광물을 구하는 것인데,<br>호석이의 위치는 `(0,0)`이기 때문에 광물의 위치를 꼭짓점으로 하는 사각형을 만들 수 있습니다.

<img src="https://upload.acmicpc.net/ea7c9439-01cb-47c1-9759-87d5fef27f69/-/preview/" alt="img" style="zoom: 33%;" />

만약 예제에서 `(5,7)`의 광물을 사각형의 꼭지점으로 하면, **`y가 7`일 때, `x가 6` 이상인 광물들은 절대 포함할 수 없습니다.**(현재 사각형안에 들어갈 수 있는 광물이 최대이기 때문에 더 큰 사각형은 올 수 없습니다.)  **`y가 8`이상인 광물들은 x값을 줄여서 사각형 크기를 줄여 범위 내의 광물 숫자를 줄이게 되면 포함**될 수 있습니다.

정리해보면, **x는 최대값부터 감소하고 y는 0부터 증가**(반대도 가능합니다.)한다고 했을 때, <br>만들 수 있는 사각형의 경우의 수는 **광물이 범위 내에 최대로 들어가기 전 까진 y를 증가**시키고, **광물이 범위 내에 최대로 들어가 있으면 x를 줄여 범위 내의 광물숫자를 감소시킨 후 다시 y를 증가**시키는 방법으로 구할 수 있습니다.<br>즉, **사각형의 꼭짓점은 왼쪽 위로만 증가**하는 것입니다.

**x는 꼭짓점을 비교하는 기준**이 되기 때문에 **y 즉, 행을 기준으로 해당 행에 있는 광물들을 리스트에 저장**합니다.

또, x의 값이 줄어들 때, 줄어든 x좌표에 해당하는 광물들이 포함되어있으면 해당 `광물들의 v값, 비용`을 빼주어야 하기 때문에 **x를 인덱스로 하는 범위 내의 v값 ,비용을 저장할 배열 2개가 필요**합니다.

이 때, 주의할 것은 아래의 그림처럼 **사각형 안에 광물들이 꽉 차지 않는 경우**와 **V의 범위(1 ≤ Vi ≤ 10<sup>8</sup>)**입니다.

<img src="https://upload.acmicpc.net/014e937c-947a-4c9e-96ee-875f26ae2d0e/-/preview/" alt="img" style="zoom: 33%;" />

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

 public class Main {
	 static List<Mineral> minerals[];
	 final static int MAX_NUM = 100000;
	 
	 public static class Mineral{
		 int x;
		 long v;
		 
		 public Mineral() {
		}
		 
		 public Mineral(int x, long v) {
			 this.x = x;
			 this.v = v;
			}
	 }
	 
 	public static void main(String[] args) {
 		InputStreamReader isr = new InputStreamReader(System.in);
 		BufferedReader br = new BufferedReader(isr);
 		OutputStreamWriter osw = new OutputStreamWriter(System.out);
 		BufferedWriter bw = new BufferedWriter(osw);
 		StringBuilder sb = new StringBuilder();
 		
 		try {
 			// 입력
 			String nums = br.readLine();
 			StringTokenizer stk = new StringTokenizer(nums);
 			
 			int n = Integer.parseInt(stk.nextToken());
 			int c = Integer.parseInt(stk.nextToken());
 			
 			// y가 인덱스, y에 해당하는 행에 있는 광물들을 저장할 리스트 배열
 			minerals = new ArrayList[MAX_NUM+1];
 			
 			// x가 인덱스, (x,y)좌표에 인덱스x를 포함하고 있는 광물의 v,c를 저장할 배열들
 			// v의 범위가 크기 때문에 long으로 저장
 			long colV[] = new long[MAX_NUM+1];
 			int colC[] = new int[MAX_NUM+1];
 			
 			for (int i = 0; i < n; i++) {
				nums = br.readLine();
				stk = new StringTokenizer(nums);
				
				int x = Integer.parseInt(stk.nextToken());
				int y = Integer.parseInt(stk.nextToken());
				long v = Long.parseLong(stk.nextToken());
				
				if(minerals[y] == null)
					minerals[y] = new ArrayList<>();
				
				// 리스트 배열의 인덱스가 y이기 때문에 광물은 x,v만 저장
				Mineral mineral = new Mineral(x,v);
				
				minerals[y].add(mineral);
			}
 			// 입력 끝
 			
 			long sum = 0;
 			long max = 0;
 			int count = 0;
 			int y = 0;
 			 
 			// 사각형의 x는 최대값부터 감소 , y는 증가하면서 사각형 범위를 구함
 			// 10만 ~ 0까지 x가 줄어듬
 			for (int x = MAX_NUM; x >= 0; --x) {
 				
 				// y가 범위를 벗어나지 않고 count가 c보다 작을때만 y를 증가
 				while(count < c && y <= MAX_NUM) {
 					
 					// y에 해당하는 행에 광물이 없을 때
 					if(minerals[y] == null) {
 						++y;
 						continue;
 					}
 					
 					// 광물이 있을 때 행에 있는 값 중, 사각형 범위 안에 포함되는 광물(사각형의 x보다 작은)만 연산
 					for(Mineral mineral : minerals[y]) {
 	 					
 	 					if(mineral.x <= x) {
 	 						sum += mineral.v;
 	 						++count;
 	 						
 	 						colV[mineral.x] += mineral.v;
 	 						++colC[mineral.x];
 	 					}
 	 				}
 					// 광물 체크 후 y증가
 					++y;
 				}
 				// count가 c랑 딱 떨어지지 않을 수도 있기 때문에 <=
 				if(count <= c)
 					if(max < sum)
 						max = sum;
 				
 				// x가 계속 줄어들기 때문에 현재 x가 포함된 광물들을 빼줌
				if(colC[x] != 0) 
 					sum -= colV[x];
 					count -= colC[x];
			}
 			
 			System.out.println(max);
 			
 		} catch (Exception e) {
 			e.printStackTrace();
 		}
 	}
 }
```



### 우선순위 큐

[최진영](https://jinyoungchoi95.tistory.com/13)님 블로그를 참고하였습니다.

`우선순위 큐`를 이용한 방법입니다.<br>각 행의 광물들을 **우선순위의 기준을 x의 내림차순(큰 값이 루트)으로 한 `우선순위 큐`에 넣습니다.** <br>그 후, 사각형의 범위를 줄일 때, **큐의 사이즈가 비용(c)을 만족할 때까지 반복**하면서,<br>**우선순위의 최대값(x값, 사각형의 꼭짓점)과 같은 값들을 빼주는 방식을 사용**합니다.<br>

즉, **큐 안에 있는 객체들이 사각형 범위 내의 광물**이 되고,<br>**기준인 x를 최대값 부터 감소시키면서 비교하는 방식을 `우선순위 큐`를 이용하여 처리**하는 것입니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

 public class Main {
	 static List<Mineral> minerals[];
	 final static int MAX_NUM = 100000;
	 
	 public static class Mineral implements Comparable<Mineral>{
		 int x;
		 long v;
		 
		 public Mineral() {
		}
		 
		 public Mineral(int x, long v) {
			 this.x = x;
			 this.v = v;
		}

		@Override
		public int compareTo(Mineral o) {
			return o.x - this.x;
		}
	 }
	 
 	public static void main(String[] args) {
 		InputStreamReader isr = new InputStreamReader(System.in);
 		BufferedReader br = new BufferedReader(isr);
 		
 		try {
 			// 입력
 			String nums = br.readLine();
 			StringTokenizer stk = new StringTokenizer(nums);
 			
 			int n = Integer.parseInt(stk.nextToken());
 			int c = Integer.parseInt(stk.nextToken());
 			
 			// y가 인덱스, y에 해당하는 행에 있는 광물들을 저장할 리스트 배열
 			minerals = new ArrayList[MAX_NUM+1];
 			
 			// 우선순위 큐, Mineral에서 CompareTo로 x기준 내림차순으로 정해주었습니다.
 			PriorityQueue<Mineral> queue = new PriorityQueue<>();
 			
 			for (int i = 0; i < n; i++) {
				nums = br.readLine();
				stk = new StringTokenizer(nums);
				
				int x = Integer.parseInt(stk.nextToken());
				int y = Integer.parseInt(stk.nextToken());
				long v = Long.parseLong(stk.nextToken());
				
				if(minerals[y] == null)
					minerals[y] = new ArrayList<>();
				
				// 리스트 배열의 인덱스가 y이기 때문에 광물은 x,v만 저장
				Mineral mineral = new Mineral(x,v);
				
				minerals[y].add(mineral);
			}
 			// 입력 끝
 			
 			long sum = 0;
 			long max = 0;
 			
 			// y를 증가시키면서, 행에 있는 값들을 가져옵니다.
 			for (int y = 0; y <= MAX_NUM; y++) {
 				if(minerals[y] == null)
 					continue;
 				
 				for(Mineral mineral : minerals[y]) {
 					queue.add(mineral);
 					sum += mineral.v;
 				}
 				
 				int prevX = 0;
 				// 큐에 값이 들어있지 않거나 큐의 사이즈가 c이하가 될 때까지 반복합니다.
 				// 큐에 들어있는 값 중, 해당 행의 x최대 값(사각형의 꼭짓점)을 구한 후 최대 값과 같은 크기를 가지는 광물을 큐에서 뺍니다.
 				// 즉, x의 값을 줄이는 것입니다. (사각형의 범위를 줄이는 것) 
 				while(!queue.isEmpty() && queue.size() > c) {
 					prevX = queue.peek().x;
 					
 					while(!queue.isEmpty() && queue.peek().x == prevX)
 						sum -= queue.poll().v;
 				}
 				
 				max = Math.max(max, sum);
			}
 		
 			System.out.println(max);
 			
 		} catch (Exception e) {
 			e.printStackTrace();
 		}
 	}
 }
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

