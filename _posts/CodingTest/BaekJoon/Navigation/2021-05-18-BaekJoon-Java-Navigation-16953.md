---
title: "BaekJoon : 16953번(A -> B)"
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



## 16953

정수 A를 B로 바꾸려고 한다. **가능한 연산은 다음과 같은 두 가지**이다.

- 2를 곱한다.
- 1을 수의 가장 오른쪽에 추가한다. 

A를 B로 바꾸는데 필요한 **연산의 최솟값**을 구해보자.



### 입력

**첫째 줄에 A, B (1 ≤ A < B ≤ 10<sup>9</sup>)**가 주어진다.

```java
2 162	// A,B 입력
```



### 출력

**A를 B로 바꾸는데 필요한 연산의 최솟값에 1을 더한 값을 출력**한다. **만들 수 없는 경우에는 -1을 출력**한다.

```java
5
```

2 → 4 → 8 → 81 → 162



### BFS를 이용한 상향식 방법

`A`가 `2*A(임시로 S라고 부름), A*10+1(임시로 T라고 부름)` 으로 나누어 지고 나누어진 `S`와 `T`가 각각 `2*S, S*10+1` / `2*T, T*10+1` 로 나누어지기 때문에 **Tree와 모양을 같이 한다**고 볼 수 있습니다. 그럼 이 **나누어지는 숫자들을 Tree라고 했을 때, A가 B가 되는 최소 경로를 구하는 것이기 때문에 BFS를 이용**할 수 있습니다.

`boolean[]` 배열을 만들어 방문을 체크해도 되지만, 모든 숫자를 방문한다는 보장이 없기 때문에 `Set`을 이용해서 공간복잡도를 줄일 수 있습니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Set;
import java.util.StringTokenizer;

public class Main {
	final static int MAX = 1000000000;
	
	public static class Num {
		long num;
		int count;
		
		public Num(long num, int count) {
			this.num = num;
			this.count = count;
		}
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			// 입력 시작
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			int a = Integer.parseInt(stk.nextToken());
			int b = Integer.parseInt(stk.nextToken());
			// 입력 끝
			
			Queue<Num> q = new LinkedList<>();
			Set<Long> visit = new HashSet<>();
			
			Num start = new Num(a, 1);
			visit.add((long)a);
			
			q.add(start);
			
			int ans = -1;
			// Queue를 이용한 BFS
			while(!q.isEmpty()) {
				Num num = q.poll();
				
				if(num.num > MAX)
					continue;
				
				// 큐에서 나온 객체의 num이 b와 같다면 루프 종료
				if(num.num == b) {
					ans = num.count;
					break;
				}
				
				long one = num.num * 10 + 1;
				long mul = num.num * 2;
				
				// 방문한 적이 없는 수만 넣어 줍니다.
				if(!visit.contains(one)) {
					visit.add(one);
					q.add(new Num(one, num.count + 1));
				}
				if(!visit.contains(mul)) {
					visit.add(mul);
					q.add(new Num(mul, num.count + 1));
				}
			}
			
			System.out.println(ans);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### B에서 A로 내려가면서 체크하는 하향식 방법

BFS를 이용하지 않고 두 가지 조건을 반대로

- 2를 곱한다. -> `2를 나눈다.`
- 1을 수의 가장 오른쪽에 추가한다. -> `1을 오른쪽에서 뺀다.`

위와 같이 이용해 B에서 A로 내려가면서 체크하는 방법입니다. **반대로 바꾼 두 조건에 해당하지 않으면 만들 수 없는 경우**가 됩니다. 

**B에서 A로 내려가는 것이기 때문에 최대 체크 범위는 B**가 되어, 더 빠르게 답을 구할 수 있습니다.

빼는 것을 어떻게 처리하는지가 관건인데, 문자열로 처리하게 되면 쉽게 처리할 수 있습니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			// 입력 시작
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			int a = Integer.parseInt(stk.nextToken());
			int b = Integer.parseInt(stk.nextToken());
			// 입력 끝
			
			int ans = 1;
			
			// b -> a로 가기 때문에 b가 같거나 작아지는 순간 종료합니다.
			while(b > a) {
                // 숫자 b를 문자열로 만들어 줍니다.
				String tmp = "" + b;
				
				// 마지막 자리 숫자가 1이면 문자열을 잘라서 1을 빼줍니다.
				if(tmp.charAt(tmp.length()-1) == '1') {
					b = Integer.parseInt(tmp.substring(0, tmp.length()-1));
					++ans;
				// 짝수면 2로 나눌 수 있기 때문에 2로 나눕니다.
				} else if(b % 2 == 0) {
					b >>= 1;
					++ans;
				// 두 조건 모두 충족할 수 없고, b가 a에 도달하지 못했다면 성립할 수 없는 숫자입니다.
				} else
					break;
			}
			
			if(b != a)
				ans = -1;
			
			System.out.println(ans);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.