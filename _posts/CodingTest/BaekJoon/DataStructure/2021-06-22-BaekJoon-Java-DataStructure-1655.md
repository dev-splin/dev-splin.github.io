---
title: "BaekJoon : 1655번 (가운데를 말해요)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : DataStructure"
  - "BaekJoon : Priority Queue"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon DataStructure

BaekJoon `DataStructure`(자료구조)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 1655번 (가운데를 말해요)

수빈이는 동생에게 "가운데를 말해요" 게임을 가르쳐주고 있다. **수빈이가 정수를 하나씩 외칠때마다 동생은 지금까지 수빈이가 말한 수 중에서 중간값**을 말해야 한다. 만약, 그동안 수빈이가 **외친 수의 개수가 짝수개라면 중간에 있는 두 수 중에서 작은** 수를 말해야 한다.

예를 들어 수빈이가 동생에게 1, 5, 2, 10, -99, 7, 5를 순서대로 외쳤다고 하면, 동생은 1, 1, 2, 2, 2, 2, 5를 차례대로 말해야 한다. 수빈이가 외치는 수가 주어졌을 때, 동생이 말해야 하는 수를 구하는 프로그램을 작성하시오.



### 입력

**첫째 줄에는 수빈이가 외치는 정수의 개수 N**이 주어진다. **N은 1보다 크거나 같고, 100,000보다 작거나 같은 자연수**이다. 그 다음 **N줄에 걸쳐서 수빈이가 외치는 정수가 차례대로** 주어진다. **정수는 -10,000보다 크거나 같고, 10,000보다 작거나 같다.**

```java
7	// 외치는 정수의 개수(N)를 입력합니다
1	// 여기부터
5
2
10
-99
7
5	// 여기 까지 정수를 외칩니다.
```



### 출력

**한 줄에 하나씩 N줄에 걸쳐 수빈이의 동생이 말해야하는 수를 순서대로 출력**한다.

```java
1	// 외치는 개수 만큼 중간값을 출력합니다.
1
2
2
2
2
5
```





### 중간값 구하기

중간 값을 구해야 하는데, 일반적으로 생각할 수 있는 **입력 받을 때 마다 정렬해서 중간값을 구하는 방법은 N의 범위가 100,000까지 이기 때문에 시간초과**가 날 수 밖에 없습니다.

그렇다면 중간값을 어떻게 구해야 할까요?? 중간값은 모든 값 중 중간에 있는 값인데, **중간에 있다는 것은 자기를 기준으로 제일 작은 값 중에서는 제일 크고**, **자기를 기준으로 제일 큰 값 중에서는 제일 작습니다.**

즉, `오름차순 Priority Queue(Min Heap)`에 중간값보다 큰 값을 넣고, `내림차순 Priority Queue(Max Heap)`에는 중간값과 중간값보다 작은 값을 넣어 중간값을 구할 수 있는 것입니다. 



하지만 어떤 방식으로 Queue에 값을 넣어야 할까요? 

중간 값을 `내림차순 Priority Queue(Max Heap)`에 포함한다고 하면, 값의 총 개수가 홀수일 때는 중간값 + 작은 값의 개수가 큰 값의 개수보다 1개 많고<br>값의 총 개수가 짝수일 때는 중간값 + 작은 값의 개수와 큰 값의 개수가 같아지게 됩니다.

즉, **오름차순 Priority Queue(Min Heap)와 내림차순 Priority Queue(Max Heap)에 번갈아가며 값을 넣어야** 하는데, `내림차순 Priority Queue(Max Heap)`는 중간값을 포함하여, Head의 값이 중간값이 되어야하기 때문에 `오름차순 Priority Queue(Min Heap)`의 Head의 값이 `내림차순 Priority Queue(Max Heap)` Head의 값(중간값)보다 반드시 커야합니다. 

그러므로, **번갈아가며 값을 넣으면서 오름차순 Priority Queue(Min Heap)의 Head의 값과 내림차순 Priority Queue(Max Heap) Head의 값을 비교해 내림차순 Priority Queue(Max Heap)의 Head의 값이 더 크다면 서로의 Head의 값을 바꿔주어 내림차순 Priority Queue(Max Heap)의 Head의 값에는 항상 중간값이 오게하는 것**입니다.

> Head : Queue에서 맨 앞의 값



`23 -> 41 -> 13 -> 22 -> -3 -> 24 -> -31`의 순서대로 값을 외친다고 가정하고 이 방식을 이용해 표로 나타내보겠습니다. 

- 표의 Queue에서 왼쪽이 Head라고 가정하겠습니다. 
- `내림차순 Priority Queue(Max Heap)`의 중간 값은 **Bold**해 놓겠습니다. 
- Head의 값이 바뀐다면 값(-> 바뀐 값)으로 표현하겠습니다. ex) 99(-> 1)

| 순서  | 외치는 값 | 내림차순 Priority Queue(Max Heap) | 오름차순 Priority Queue(Min Heap) | 중간값 |
| :---: | :-------: | :-------------------------------: | :-------------------------------: | :----: |
| **1** |    23     |              **23**               |                 -                 |   23   |
| **2** |    41     |              **23**               |                41                 |   23   |
| **3** |    13     |            **23**, 13             |                41                 |   23   |
| **4** |    22     |         **23(-> 22)**, 13         |           22(-> 23), 41           |   22   |
| **5** |    -3     |          **22**, 13, -3           |              23, 41               |   22   |
| **6** |    24     |          **22**, 13, -3           |            23, 24, 41             |   22   |
| **7** |    -31    |        **22**, 13, -3, -31        |            23, 24, 41             |   22   |



### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.PriorityQueue;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			int n = Integer.parseInt(br.readLine());
			
			PriorityQueue<Integer> minPq = new PriorityQueue<>();
			PriorityQueue<Integer> maxPq = new PriorityQueue<>((a,b)->b-a);
			
			StringBuilder sb = new StringBuilder();
			
			for (int i = 0; i < n; i++) {
				int num = Integer.parseInt(br.readLine());
				
				// 두 개의 큐의 크기가 같으면 maxPq에, 아니면 minPq에 값을 번갈아 넣습니다.
				if(minPq.size() == maxPq.size())
					maxPq.add(num);
				else
					minPq.add(num);
				
				// 맨 처음 minPq는 비어 있기 때문에 비어있는지 체크
				if(!minPq.isEmpty()) {
					// maxPq의 Head값은 minPq의 Head값보다 작아야 하기 때문에
					// maxPq의 Head값이 더 크다면 서로 바꿔줍니다.
					if(maxPq.peek() > minPq.peek()) {
						int bigNum = maxPq.poll();
						int smallNum = minPq.poll();
						
						maxPq.add(smallNum);
						minPq.add(bigNum);
					}
				}
				
				sb.append(maxPq.peek()).append('\n');
			}
			
			System.out.println(sb.toString());
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

