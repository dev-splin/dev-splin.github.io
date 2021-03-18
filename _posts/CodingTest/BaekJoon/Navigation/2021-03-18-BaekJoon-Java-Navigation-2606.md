---
title: "BaekJoon : 2606(바이러스)"
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

BaekJoon Navigation(탐색)  저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 2606

신종 바이러스인 웜 바이러스는 네트워크를 통해 전파된다. 한 컴퓨터가 웜 바이러스에 걸리면 그 컴퓨터와 네트워크 상에서 연결되어 있는 모든 컴퓨터는 웜 바이러스에 걸리게 된다.

예를 들어 7대의 컴퓨터가 <그림 1>과 같이 네트워크 상에서 연결되어 있다고 하자. **1번 컴퓨터가 웜 바이러스에 걸리면** 웜 바이러스는 2번과 5번 컴퓨터를 거쳐 3번과 6번 컴퓨터까지 전파되어 **2, 3, 5, 6 네 대의 컴퓨터는 웜 바이러스에 걸리게 된다.** 하지만 **4번과 7번 컴퓨터는 1번 컴퓨터와 네트워크상에서 연결되어 있지 않기 때문에 영향을 받지 않는다.**

![img](https://www.acmicpc.net/upload/images/zmMEZZ8ioN6rhCdHmcIT4a7.png)

어느 날 1번 컴퓨터가 웜 바이러스에 걸렸다. 컴퓨터의 수와 네트워크 상에서 서로 연결되어 있는 정보가 주어질 때, **1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 출력하는 프로그램을 작성**하시오.



**첫째 줄에는 컴퓨터의 수**가 주어진다. 컴퓨터의 수는 100 이하이고 **각 컴퓨터에는 1번 부터 차례대로 번호가 매겨진다.** **둘째 줄에는 네트워크 상에서 직접 연결되어 있는 컴퓨터 쌍의 수**가 주어진다. **이어서 그 수만큼 한 줄에 한 쌍씩 네트워크 상에서 직접 연결되어 있는 컴퓨터의 번호 쌍**이 주어진다.

1번 컴퓨터가 웜 바이러스에 걸렸을 때, **1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 첫째 줄에 출력**한다.

```java
7		// 총 컴퓨터의 수(노드)를 입력합니다.
6		// 네트워크 상에서 연결되어 있는 컴퓨터 수(간선)를 입력합니다.
1 2
2 3
1 5
5 2
5 6
4 7		// 연결되어 있는 컴퓨터 쌍을 입력합니다.
4		// 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 출력합니다.
```



### 내코드

미로같은 간선의 수가 많은 경우가 아니기 때문에 인접리스트를 이용하여 `BFS`로 연결된 노드(컴퓨터)를 찾아 개수를 셉니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
	static InputStreamReader isr = new InputStreamReader(System.in);
	static BufferedReader br = new BufferedReader(isr);
	// 컴퓨터 수
	static int computerCount;
	// 컴퓨터 수만큼 컴퓨터를 만들어서 넣기 위한 배열
	static Computer[] computers;
	
	// 컴퓨터 번호와 인접리스트를 가지고 있는 컴퓨터 클래스
	public static class Computer {
		int num;
		List<Computer> adjacentComputer;
		
		public Computer(int num) {
			this.num = num;
			adjacentComputer = new ArrayList<>();
		}
		
		public void add(Computer computer) {
			adjacentComputer.add(computer);
		}
	}
	
	// 컴퓨터 개수만큼 컴퓨터를 만들고 네트워크를 입력하고 연결해줍니다.(서로의 인접리스트에 서로 추가해줍니다.)
	public static void makeComputer() throws Exception{
		computers = new Computer[computerCount];
		
		for (int i = 0; i < computers.length; i++)
			computers[i] = new Computer(i+1);
		
		int networkCount = Integer.parseInt(br.readLine().trim());
		
		for (int i = 0; i < networkCount; i++) {
			String nums = br.readLine().trim();
			
			StringTokenizer stk = new StringTokenizer(nums);
			int computerIndex1 = Integer.parseInt(stk.nextToken()) - 1;
			int computerIndex2 = Integer.parseInt(stk.nextToken()) - 1;
			
			computers[computerIndex1].add(computers[computerIndex2]);
			computers[computerIndex2].add(computers[computerIndex1]);
		}
	}

	// BFS를 이용하여 1번 컴퓨터와 이어진 컴퓨터들을 찾아 개수를 체크합니다.
	public static int BFS() {
		Queue<Computer> queue = new LinkedList<>();
		queue.add(computers[0]);
		
		// 컴퓨터가 체크됐는 지 확인하기 위한 boolean 배열
		boolean check[] = new boolean[computerCount];
		check[0] = true;
		
		// 감염된 숫자를 셀 때 1은 빼므로 0부터 시작합니다.
		int count = 0;
		
		while(!queue.isEmpty()) {
			Computer checkComputer = queue.poll();
			
			// 해당 컴퓨터의 인접리스트에서 체크되지 않은 컴퓨터를 큐에 넣어주고 체크상태로 만들면서 count를 늘려줍니다.
			for(Computer computer : checkComputer.adjacentComputer) {
				if(!check[computer.num - 1]) {
					queue.add(computer);
					++count;
					check[computer.num - 1] = true;
				}
			}
		}
		return count;
	}
	
	public static void main(String[] args) {
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			computerCount = Integer.parseInt(br.readLine().trim());
			
			makeComputer();
			
			int result = BFS();
			
			bw.write(Integer.toString(result));
			
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