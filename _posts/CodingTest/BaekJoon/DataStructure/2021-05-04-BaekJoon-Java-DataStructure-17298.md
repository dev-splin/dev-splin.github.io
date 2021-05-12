---
title: "BaekJoon : 17298번(오큰수)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Data Structure"
  - "BaekJoon : Stack"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon DataStructure

BaekJoon `DataStructure`(자료구조)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 17298

크기가 N인 수열 A = A1, A2, ..., AN이 있다. 수열의 각 원소 Ai에 대해서 **오큰수 NGE(i)**를 구하려고 한다. **Ai의 오큰수는 오른쪽에 있으면서 Ai보다 큰 수 중에서 가장 왼쪽에 있는 수를 의미**한다. **그러한 수가 없는 경우에 오큰수는 -1**이다.

예를 들어, **A = [3, 5, 2, 7]인 경우 NGE(1) = 5, NGE(2) = 7, NGE(3) = 7, NGE(4) = -1**이다. **A = [9, 5, 4, 8]인 경우에는 NGE(1) = -1, NGE(2) = 8, NGE(3) = 8, NGE(4) = -1**이다.



### 입력

**첫째 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000,000)**이 주어진다. **둘째에 수열 A의 원소 A1, A2, ..., AN (1 ≤ Ai ≤ 1,000,000)**이 주어진다.

```java
4			// 수열의 개수(N) 입력
3 5 2 7		// 수열의 개수만큼 수열을 입력합니다.
```



### 출력

**총 N개의 수 NGE(1), NGE(2), ..., NGE(N)을 공백으로 구분해 출력**한다.

```java
5 7 7 -1
```



### 오큰수

오큰수를 구한다는 것은 **배열의 현재 인덱스의 오른쪽에서 현재 인덱스의 값보다 큰 수 중 가장 가까운 수를 구하는 것**이라고 할 수 있습니다. 숫자들을 배열에 넣은 후,  `Stack`을 이용해서 **처음부터 차례대로 검사하면서 구하는 방법**과 **끝에서 부터 구하는 방법** 2가지가 있습니다. 

이 때, `Stack`안에 인덱스를 넣어 해당 **인덱스에 해당하는 값들이 Stack의 입구를 기준으로 오름차순으로 되어있게 유지**해야 가장 가깝고 큰 수를 구할 수 있습니다. <br>`유지`란 의미 때문에 조금 헷갈릴 수 있습니다. **여기서 유지의 의미는 Stack이 무조건 항상 오름차순으로 유지해야 한다가 아니라 Stack을 유지하기 위해 인덱스를 넣었다 뺐다할 수 있지만 최종적으로 유지하게 만들어야 한다는 의미**입니다. 이 말의 의미는 뒤의 문제해결 방법을 보면 이해할 수 있다고 생각합니다.



### 처음부터 검사하면서 구하는 방법

처음부터 검사하는 방법의 **오름차순 유지는 `검사하는 인덱스의 값`이 `Stack에 들어있는 인덱스의 값`보다 크면 Stack에서 빼주는 방식**으로 할 수 있는데, 이 때 **빼는 인덱스의 오큰수가 현재 검사하고있는 인덱스의 값**이 됩니다. 즉, **Stack에 인덱스를 쌓아가다가 처음으로 스택의 인덱스 값보다 큰 값을 만나면 이 큰 값보다 작은 값들의 인덱스를 Stack에서 빼주면서 오큰수를 설정**해주는 것입니다. 

이런 방식으로 처리하고나면, 자연스럽게 오름차순으로 유지가 되고 **Stack에 남은 인덱스 값들은 배열의 가장 오른쪽 인덱스 값이거나, 가장 큰 수**가 됩니다.<br>`Stack`을 한 번만 검사하면 되기 때문에 `시간 복잡도는 O(n)`입니다.



#### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		StringBuilder sb = new StringBuilder();
		
		try {
			int n = Integer.parseInt(br.readLine());
			
			int arr[] = new int[n];
			
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			for (int i = 0; i < n; i++)
				arr[i] = Integer.parseInt(stk.nextToken());
			
			ArrayDeque<Integer> stack = new ArrayDeque<>();
			
			for (int i = 0; i < n; i++) {
				// Stack에 값이 있을 때, Stack의 맨 위 인덱스의 값이 현재 검사하는 인덱스의 값보다 작으면 빼면서 오큰수 설정
				// 5,4,3, 8(현재 값)같은 경우가 있기 때문에  큰 값이 나오거나 비어있을 때 까지 반복해줍니다.
				while(!stack.isEmpty() && arr[stack.peek()] < arr[i])
					arr[stack.pop()] = arr[i];
				
				// Stack에 값을 쌓습니다.
				stack.push(i);
			}
			
			// 전체를 검사하고 남은 인덱스의 값들을 -1로 설정합니다.
			while(!stack.isEmpty())
				arr[stack.pop()] = -1;
			
			Arrays.stream(arr).forEach(i -> sb.append(i).append(' '));
			
			System.out.println(sb.toString());
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 끝에서부터 구하는 방법

끝에서부터 구하는 방법의 오름차순 유지는 **배열에서 제일 가까운 큰 수를 알게되면 그 뒤는 볼필요 없다는 특징을 이용**한 것입니다. **오름차순 유지는 끝에서 부터 검사하기 때문에 `Stack에 들어있는 인덱스의 값`이 `검사하는 인덱스의 값`보다 작으면 Stack에서 빼주는 방식**으로 할 수 있는데, 이 때 빼는 인덱스의 오큰수가 현재 검사하고있는 인덱스의 값이 되는 것이 아니라, **현재 검사하는 인덱스의 오큰수가 Stack에서 빼고 남아있는 맨 위의 인덱스 값**이 되는 것입니다. 즉, **Stack에 배열의 끝에서부터 인덱스를 쌓아가다가 처음으로 스택의 인덱스의 값보다 작은 값을 가지는 검사하는 인덱스를 만나면 검사하는 인덱스의 값보다 작은 값을 Stack에서 제외하고 남아있는 Stack에서 맨위의 인덱스를 오큰수로 설정**해주는 것입니다. 그렇기 때문에 Stack의 더 안쪽에 있는 값(가까운 큰 수 뒤에 있는 수)들은 볼 필요가 없는 것입니다.

이런 방식으로 처리하고나면, 자연스럽게 오름차순으로 유지가 되고 **배열의 가장 오른쪽 인덱스 값이나, 가장 큰 수는 Stack에 자신을 추가하기 전에는 값이 없는 것**을 알 수 있습니다. <br>동일하게 `Stack`을 한 번만 검사하면 되기 때문에 `시간 복잡도는 O(n)`입니다.



> `처음부터 구하는 방법`과 다른 점은 **처음부터 구할 때는 검사가 끝난 값들은 이용하지 않기 때문**에 배열 값을 바꿔도 되고 따로 **-1이 오는 경우를 처리**해주어야 하지만, `끝에서부터 구하는 방법`은 **-1이 오는 경우를 동시에 처리**하게 되지만  **Stack에 들어있는 인덱스의 값들이 오큰수가 될 수 있기 때문**에 배열 값을 바꿀 수 없고, 따로 결과를 저장할 배열을 만들어주어야 합니다.



#### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		StringBuilder sb = new StringBuilder();
		
		try {
			int n = Integer.parseInt(br.readLine());
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			int arr[] = new int[n];
			
			for (int i = 0; i < n; i++)
				arr[i] = Integer.parseInt(stk.nextToken());
			
			ArrayDeque<Integer> stack = new ArrayDeque<>();
			
			// 결과를 저장할 배열
			int ans[] = new int[n];
			
			// 뒤에서부터 검사
			for (int i = n-1; i >= 0; --i) {
				// Stack에 검사하는 인덱스의 값보다 작은 값을 가진 인덱스는 뺍니다.
				while(!stack.isEmpty() && arr[stack.peek()] <= arr[i])
					stack.pop();
				
				// 검사하는 값보다 작은 값들을 전부 뺐을 때, Stack에 값이 없으면 가장 큰 값이거나, 가장 오른쪽의 값이기 때문에 -1
				if(stack.isEmpty())
					ans[i] = -1;
				// 값이 있다면 검사하는 값보다 큰 값이기 때문에 오큰수 설정
				else
					ans[i] = arr[stack.peek()];
				stack.push(i);		
			}
			
			Arrays.stream(ans).forEach(i -> sb.append(i).append(' '));
			
			System.out.println(sb.toString());
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

