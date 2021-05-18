---
title: "BaekJoon : 12851(숨바꼭질 2)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Navigation"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Navigation

BaekJoon `Navigation(탐색)`  저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 12851

수빈이는 동생과 숨바꼭질을 하고 있다. **수빈이는 현재 점 N(0 ≤ N ≤ 100,000)**에 있고, **동생은 점 K(0 ≤ K ≤ 100,000)**에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, **수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동**하게 된다. **순간이동을 하는 경우에는 1초 후에 2*X의 위치로 이동**하게 된다.

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 **가장 빠른 시간이 몇 초 후인지 그리고, 가장 빠른 시간으로 찾는 방법이 몇 가지 인지 구하는 프로그램을 작성**하시오.



### 입력

**첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K**가 주어진다. N과 K는 정수이다.

```java
5 17	// N과 K를 입력
```



### 출력

**첫째 줄에 수빈이가 동생을 찾는 가장 빠른 시간을 출력**한다.

**둘째 줄에는 가장 빠른 시간으로 수빈이가 동생을 찾는 방법의 수를 출력**한다.

```java
4		// 동생을 찾는 가장 빠른 시간
2		// 가장 빠른 시간으로 동생을 찾는 방법의 수
```



### 중복 방문을 허용한 BFS

동생을 찾는 가장 빠른 시간만 구하는 것이 아니고, 가장 빠른 시간으로 찾는 방법의 수 까지 구해야 하기 때문에 조금 까다로울 수 있습니다.

가장 빠른 시간에 도착하는 방법이 여러가지 있을 수 있는데, 가장 보편적인 예는 `1 4`가 주어졌을 때 입니다.

`1 -> 2(+1) -> 4(*2)`<br>`1 -> 2(*2) -> 4(*2)`

위와 같이 **`같은 시간`에 `같은 숫자`를 똑같이 방문하지만 연산하는 방식이 다르기 때문에 두 방식을 다른 방법으로 인식**해야 합니다. 즉, **중복 방문을 허용**해야 한다는 것입니다.



### 큐에서 뺄 때 Set으로 방문체크하는 방식

`BFS`에서 큐를 이용할 때, 큐에 값을 넣을 때 방문체크하는 것이 아니고, 큐에서 빼면서 방문체크하는 방식입니다. 

다만, 큐에 넣으면서 결과를 체크하는 것이 아니고 빼면서 체크하기 때문에 조금 더 많은 공간과 시간이 필요합니다.

#### 큐에 값을 넣을 때 방문체크

위의 `1 4`의 경우에 큐에 값을 넣을 때 방문체크하게 되면 `1 -> 2(+1)`는 큐에 들어가지만, `1 -> 2(*2)`는 방문체크를 했기 때문에 큐에 들어가지 않아, 방법의 수를 제대로 구할 수 없습니다.

#### 큐에서 빼면서 방문체크

큐에서 빼면서 방문체크를 하게 되면 `1 -> 2(+1)`와 `1 -> 2(*2)` 둘 다 큐에 들어가, 방법의 수를 제대로 구할 수 있습니다.

> 즉, 1을 0초라고 했을 때 2는 1초, 4는 2초에 방문하게 되는데,
>
> 큐에서 빼면서 방문체크를 할 시, 최초로 2가 나오는 1초의 경우에 방문체크 하기 때문에 1초 이후(2초, 3초...)에는 2를 큐에 넣을 수 없습니다.



#### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Set;
import java.util.StringTokenizer;

public class Main {
	
	public static class Point {
		int position;
		int time;
		
		public Point(int position, int time) {
			this.position = position;
			this.time = time;
		}
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			int n = Integer.parseInt(stk.nextToken());
			int k = Integer.parseInt(stk.nextToken());
			
			Set<Integer> visit = new HashSet<>();
			Queue<Point> q = new LinkedList<>();
			
			q.add(new Point(n, 0));
			
			int dstTime = Integer.MAX_VALUE;
			int count = 0;
			
			// 큐에 위치, 시간으로 이루어진 Point객체를 넣고 큐에서 뺄 때, 방문체크를 합니다.
			while(!q.isEmpty()) {
				Point cur = q.poll();
				
				// 방문하지 않았다면 방문 체크
				if(!visit.contains(cur.position))
					visit.add(cur.position);
				
				// 범위를 넘어갈 시에는 스킵
				if(cur.position < 0 || cur.position > 100000)
					continue;
				
				// BFS이기 때문에 현재 시간이 찾은 시간보다 커지는 순간 루프를 종료합니다.
				if(cur.time > dstTime)
					break;
				
				// 동생의 위치(k)와 같아지면 찾은 시간을 설정하고 count를 늘립니다.
				if(cur.position == k) {
					dstTime = cur.time;
					++count;
				}
				
				int nextTime = cur.time + 1;
				int next = cur.position + 1;
				
				// 방문하지 않았을 시 큐에 넣어줍니다.
				if(!visit.contains(next))
					q.add(new Point(next, nextTime));
				
				next = cur.position - 1;
				if(!visit.contains(next))
					q.add(new Point(next, nextTime));
				
				next = cur.position * 2;
				if(!visit.contains(next))
					q.add(new Point(next, nextTime));
			}
			
			System.out.println(dstTime);
			System.out.println(count);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



### 큐에 넣을 때 배열로 방문체크하는 방식

`BFS`에서 큐를 이용할 때, 큐에 값을 뺄 때 방문체크하는 것이 아니고, 큐에 넣으면서 방문체크하는 방식입니다. 다만, 큐에 넣으면서 결과를 체크하기 때문에 더 빨리 결과를 체크할 수 있기 때문에 공간과 시간이 더 적게 필요합니다.

배열의 인덱스를 숫자 위치로 두고, **해당 숫자 위치에 도착하는 가장 빠른 시간 담는 배열 `time[]`**와 **해당 숫자 위치에 도착하는 방법의 개수를 담는 배열 `count[]`**을 이용합니다. 

#### time[]

`time[]`배열의 초기값이 0이고, 시간이 0인 경우는 시작할 때 밖에 없기 때문에 **`time[]`배열에서 해당 인덱스의 값이 0이면, 방문한 적이 없는 곳이라는 의미**가 됩니다. 즉, 따로 방문 체크하는 배열을 만들지 않아도 되기 때문에 공간을 절약할 수 있습니다.

#### count[]

`1 -> 2(+1) -> 4(*2)`<br>`1 -> 2(*2) -> 4(*2)`

위의 경우에서 **현재 숫자가 2라고 가정하면, 현재 숫자(2)에 도착하는 여러가지 방법이 있으면 다음 숫자(4)도 현재 숫자에 도착한 방법의 개수 만큼 올 수 있습니다.** 

그렇기 때문에 **방문한 적이 없는 곳을 도착했을 때**, 단순히 count를 증가 시켜주는 것이 아니라 **현재 숫자에 도착한 개수 만큼(`count[4] = count[2]`) 넣어줍니다.** 또, **같은 시간에 같은 곳을 도착하게 되면**, 위와 같은 이유로 **현재 숫자에 도착한 개수 만큼(`count[4] += count[2]`) 더 해줍니다.**



#### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			int n = Integer.parseInt(stk.nextToken());
			int k = Integer.parseInt(stk.nextToken());
			
			// 해당 인덱스(숫자)를 방문한 시간과 개수
			int time[] = new int[100001];
			int count[] = new int[100001];
			
			Queue<Integer> q = new LinkedList<>();
			
			// 처음 시간은 0
			time[n] = 0;
			// 처음 숫자(n)을 방문한 숫자는 1개(자기 자신)
			count[n] = 1;
			q.add(n);
			
			// 
			while(!q.isEmpty()) {
				int cur = q.poll();
				
                // 큐에 넣으면서 방문 체크를 했기 때문에 목표 위치와 같으면 루프 종료
				if(cur == k)
					break;
				
				int nexts[] = {cur - 1, cur + 1, cur * 2};
				
				for(int next : nexts) {
					// 범위를 넘으면 스킵
					if(next < 0 || next > 100000)
						continue;
					
					// 시간이 0 즉, 방문한 적이 없는 곳
					if(time[next] == 0) {
						q.add(next);
						time[next] = time[cur] + 1;
                        
						// 1->2(*2)->4(*2), 1->2(+1)->4(*2)의 경우에서 현재 숫자가 2라고 가정하면,
						// 현재 숫자(2)에 도착하는 여러가지 방법(count[2])이 있으면
						// 다음 숫자(4)도 현재 숫자에 도착한 방법의 개수만큼(count[4] = count[2]) 올 수 있습니다.
						// 때문에 1을 넣는 것이 아니고 현재 숫자에 도착한 개수 만큼 넣어줍니다.
						count[next] = count[cur];
                        
					// 방문한 적이 있지만, 같은 시간에 도착했을 때도 위와 같은 이유로 현재 숫자에 도착한 개수 만큼 더해줍니다.
					} else if(time[next] == time[cur] + 1)
						count[next] += count[cur];
				}
			}
			
			System.out.println(time[k]);
			System.out.println(count[k]);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

