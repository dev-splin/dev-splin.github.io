---
title: "BaekJoon : 1043번 (거짓말)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Union Find"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Union Find

BaekJoon `Union Find(합집합 찿기)`  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 1043

지민이는 파티에 가서 이야기 하는 것을 좋아한다. 파티에 갈 때마다, 지민이는 지민이가 가장 좋아하는 이야기를 한다. **지민이는 그 이야기를 말할 때, 있는 그대로 진실로 말하거나 엄청나게 과장해서 말한다.** 당연히 과장해서 이야기하는 것이 훨씬 더 재미있기 때문에, **되도록이면 과장해서 이야기**하려고 한다. 하지만, 지민이는 거짓말쟁이로 알려지기는 싫어한다. 문제는 **몇몇 사람들은 그 이야기의 진실을 안다**는 것이다. 따라서 **이런 사람들이 파티에 왔을 때는, 지민이는 진실을 이야기**할 수 밖에 없다. 당연히, **어떤 사람이 어떤 파티에서는 진실을 듣고, 또다른 파티에서는 과장된 이야기를 들었을 때도 지민이는 거짓말쟁이로 알려지게 된다.** 지민이는 이런 일을 모두 피해야 한다.

**사람의 수 N**이 주어진다. 그리고 그 **이야기의 진실을 아는 사람**이 주어진다. 그리고 각 **파티에 오는 사람들의 번호**가 주어진다. **지민이는 모든 파티에 참가**해야 한다. 이때, **지민이가 거짓말쟁이로 알려지지 않으면서, 과장된 이야기를 할 수 있는 파티 개수의 최댓값을 구하는 프로그램을 작성**하시오.



### 입력

**첫째 줄에 사람의 수 N과 파티의 수 M**이 주어진다.

**둘째 줄에는 이야기의 진실을 아는 사람의 수와 번호**가 주어진다. **진실을 아는 사람의 수가 먼저 주어지고 그 개수만큼 사람들의 번호가 주어진다.** 사람들의 **번호는 1부터 N까지**의 수로 주어진다.

**셋째 줄부터 M개의 줄에는 각 파티마다 오는 사람의 수와 번호**가 같은 방식으로 주어진다.

**N, M은 50 이하의 자연수**이고, **진실을 아는 사람의 수와 각 파티마다 오는 사람의 수는 모두 0 이상 50 이하의 정수**이다.

```java
4 3			// 사람의 수(N)와 파티의 수(M)를 입력
0			// 진실을 아는 사람의 수 입력 (0이기 때문에 번호는 주어지지 않습니다.)
2 1 2		// 여기부터
1 3
3 2 3 4		// 여기까지 파티에 오는 사람의 수와 번호를 입력
```



### 출력

첫째 줄에 문제의 정답을 출력한다.

```java
3		// 과장된 이야기를 할 수 있는 최댓값
```



### DFS와 Union Find

파티에 진실을 알고 있는 사람이 있으면 그 파티의 사람들은 모두 진실을 알게 됩니다. 즉, **진실을 알게된 순서와 상관없이 나중에 진실을 알게 되어도 이미 입력이 끝난 파티들에 진실을 알게된 사람이 생길 수 있기 때문에 이에 대한 처리**를 잘 해주어야 합니다.

처음부터 진실을 알고 있는 사람들은 애초에 자신이 포함된 파티들은 모두 진실된 파티가 될 수 밖에 없습니다.

**새롭게 진실을 알게된 사람들은 자신이 이전에 참가했던 파티들도 진실된 파티가 되어야 합니다.** 진실을 알게된 현재 파티를 포함한 이전 파티들까지 연쇄적으로 처리를 해주어야 하기 때문에 `DFS`를 생각해볼 수 있습니다.

또, 파티에서 서로 만났던 사람들 중 한 명이라도 진실을 알게되면 모두 진실을 알게 되기 때문에 `Union Find(합집합 찾기)`를 이용해 **같은 부모를 가지는 것을 이어져 있다고 판단**할 수 있습니다.



### DFS

위에서 말했던 것처럼 진실을 알게된 현재 파티를 포함한 이전 파티들까지 연쇄적으로 처리를 해주어야 하기 때문에 `진실을 알고 있는 인덱스를 체크 해주는 배열`과 `파티 번호를 인덱스로 하고 사람들의 번호를 저장하는 List`,  `사람들의 번호를 인덱스로 하고 그 사람들이 포함된 파티를 저장하는 List`가 필요합니다. 코드와 주석을 보면 이해할 수 있을 것이라고 생각합니다.



#### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Set;
import java.util.StringTokenizer;

public class Main {
	static int n;
	// 각 파티에서 과장된 이야기를 들은 인덱스를 저장
	static List<Integer> partys[];
	// 각 인덱스가 과장된 이야기를 들었던 파티를 저장
	static List<Integer> liePartys[];
	// 진실을 알고 있는 인덱스
	static boolean know[];
	
	// 파라미터로 들어온 파티번호에 해당하는 인덱스들이 진실을 알게 되고,
	// 그 인덱스들이 포함된 다른 파티의 인덱스들도 연쇄적으로 진실을 알게 되기 때문에 DFS를 사용합니다.
	public static void DFS (int partyNum) {
			
		if(partys[partyNum] == null)
			return;
			
		for(Integer index : partys[partyNum]) {
			// 이미 진실을 알고 있는 인덱스는 liePartys를 가지고 있지 않고
			// 새롭게 진실을 알게된 인덱스는 know를 true로 바꿔준 후 liePartys를 검사하게 됩니다.
			// 만약, 이 조건이 없다면 새롭게 진실을 알게된 인덱스가 다른 파티에도 존재하기 때문에 무한 루프에 빠지게 됩니다.
			if(know[index])
				continue;
			
			know[index] = true;
			
			for(Integer lieParty : liePartys[index])
				DFS(lieParty);
		}
        // 파티에 있는 인덱스들 체크가 끝났으면 해당 파티는 과장된 파티를 체크할 때 제외하기 위해 null을 넣어줍니다.
		partys[partyNum] = null;
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			// 입력 시작
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			n = Integer.parseInt(stk.nextToken());
			int m = Integer.parseInt(stk.nextToken());
			
			liePartys = new ArrayList[51];
			know = new boolean[51];
			
			for (int i = 1; i <= n; i++)
				liePartys[i] = new ArrayList<>();
			
			stk = new StringTokenizer(br.readLine());
			int k = Integer.parseInt(stk.nextToken());
			
			for (int i = 0; i < k; i++)
				know[Integer.parseInt(stk.nextToken())] = true;
			// 진실을 아는 사람 입력까지 완료
			
			partys = new ArrayList[m];
			
			// 파티에 오는 사람들
			for (int i = 0; i < m; i++) {
				stk = new StringTokenizer(br.readLine());
				int count = Integer.parseInt(stk.nextToken());
				
				partys[i] = new ArrayList<>();
				
				boolean isPossible = true;
				
				for (int j = 0; j < count; j++) {
					int num = Integer.parseInt(stk.nextToken());
					
					partys[i].add(num);
					
					// 진실을 알고 있는 인덱스가 1개라도 있으면 과장된 이야기를 할 수 없는 파티
					if(know[num])
						isPossible = false;
					// 진실을 알고 있는 인덱스가 1개도 없으면 과장된 파티이기 때문에 저장해 둔다.
					else
						liePartys[num].add(i);
				}
				
				// 과장된 이야기를 할 수 없는 파티이면 DFS를 이용해 현재 파티의 인덱스들과 연관되어있고 포함되어있는 파티들을 null로 바꿔줍니다.
				if(!isPossible)
					DFS(i);
			}
			
			int ans = 0;
			
			// 파티가 null이 아니라면 과장된 파티 (파티에 아무도 없는 경우에는 DFS에서 null로 만들 수 없기 때문에 counting 됩니다.)
			for (int i = 0; i < m; i++)
				if(partys[i] != null)
					++ans;
			
			System.out.println(ans);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



### Union Find(합집합 찾기)

파티에서 서로 만났던 사람들 중 한 명이라도 진실을 알게되면 모두 진실을 알게 되기 때문에 **파티에서 만나는 사람들을 Union Find를 이용해 이어주고, 진실을 알고 있는 사람과 같은 부모를 가지는 사람이 파티에 속해 있으면 그 파티는 과장된 파티가 아니게 됩니다.**

`부모를 저장할 배열`과 `진실을 아는 사람들의 인덱스를 저장할 List`, `파티 번호를 인덱스로 하고 사람들의 번호를 저장하는 List`가 필요합니다.



#### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Set;
import java.util.StringTokenizer;

public class Main {
	static int n;
	// 부모를 저장할 배열
	static int parent[];
	// 진실을 아는 사람의 인덱스를 저장
	static List<Integer> knows;
		
	// 부모를 찾아서 부모가 다르면 서로 연결해 줍니다.
	public static void union(int a, int b) {
		int num1 = find(a);
		int num2 = find(b);
		if(num1 != num2)
			parent[num2] = num1;
	}
	
	// 부모를 찾습니다. 최상위 부모는 자기 자신을 가르킵니다.
	public static int find(int a) {
		if(parent[a] == a)
			return parent[a];
		else
			return parent[a] = find(parent[a]);
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			// 입력 시작
			StringTokenizer stk = new StringTokenizer(br.readLine());
			
			n = Integer.parseInt(stk.nextToken());
			int m = Integer.parseInt(stk.nextToken());
			
			parent = new int[51];
			// 각 파티에 속한 인덱스들을 저장
			List<Integer> list[] = new ArrayList[m];
			
			for (int i = 0; i <= 50; i++)
				parent[i] = i;
			
			stk = new StringTokenizer(br.readLine());
			int k = Integer.parseInt(stk.nextToken());
			
			knows = new ArrayList<>();
			
			for (int i = 0; i < k; i++) {
				int num = Integer.parseInt(stk.nextToken());
				knows.add(num);
			}
			// 진실을 아는 사람들 까지 입력 끝
			
			// 파티에 오는 사람들
			for (int i = 0; i < m; i++) {
				stk = new StringTokenizer(br.readLine());
				int count = Integer.parseInt(stk.nextToken());
				
				// 이전 인덱스
				int prev = 0;
				
				list[i] = new ArrayList<>();
				
				for (int j = 0; j < count; j++) {
					int num = Integer.parseInt(stk.nextToken());
					
					// 이전 인덱스가 0이 아니라면, 서로 연결해줍니다.
					// 이전 인덱스가 0이라는 것은 파티에서 맨 처음에 입력한 인덱스이기 때문에 합칠 게 없습니다.
					if(prev != 0) 
						union(prev, num);
					
					// 현재 인덱스를 이전 인덱스로 설정
					prev = num;
					list[i].add(num);
				}
			}
			
			int ans = 0;
			
			for (int i = 0; i < m; i++) {
				boolean isPossible = true;
				
				for(Integer know : knows) {
					// 진실을 알고있는 사람들의 최상위 부모
					int knowNum = find(know);
					
					// 각 파티마다 순회 (파티에 아무도 없는 경우는 List가 비어있기 때문에 counting 됩니다.)
					for(Integer num : list[i])
						
						// 진실을 알고있는 사람의 최상위 부모와 파티에 속한 인덱스의 최상위 부모가 같으면 과장된 파티가 아닙니다. 
						if(knowNum == find(num)) {
							isPossible = false;
							break;
						}
				}
				
				if(isPossible)
					++ans;
			}
			
			System.out.println(ans);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

