---
title: "BaekJoon : 1931번(회의실 배정)"
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



## 1931

한 개의 회의실이 있는데 이를 사용하고자 하는 N개의 회의에 대하여 회의실 사용표를 만들려고 한다. 각 회의 I에 대해 시작시간과 끝나는 시간이 주어져 있고, 각 회의가 겹치지 않게 하면서 회의실을 사용할 수 있는 회의의 최대 개수를 찾아보자. 단, 회의는 한번 시작하면 중간에 중단될 수 없으며 한 회의가 끝나는 것과 동시에 다음 회의가 시작될 수 있다. 회의의 시작시간과 끝나는 시간이 같을 수도 있다. 이 경우에는 시작하자마자 끝나는 것으로 생각하면 된다.



첫째 줄에 회의의 수 N(1 ≤ N ≤ 100,000)이 주어진다. 둘째 줄부터 N+1 줄까지 각 회의의 정보가 주어지는데 이것은 공백을 사이에 두고 회의의 시작시간과 끝나는 시간이 주어진다. 시작 시간과 끝나는 시간은 23<sup>1</sup>-1보다 작거나 같은 자연수 또는 0이다.



### 수정하기 전 코드

저의 코드 입니다..  딱 봐도 복잡해 보입니다.. 엄청나게 많은 반복문이 들어갔죠..ㅠㅠ 결과는.. 역시나 시간 초과 입니다..

코딩 중에 `ConcurrentModificationException`이 발생해 [Enhanced에서 객체를 삭제하는 방법](https://m.blog.naver.com/tmondev/220393974518)을 알 수 있었고 메모리와 시간 초과를 해결하기 위해 [메모리 누수에 관한 글](https://lelecoder.com/20)과 메모리 관리에 대해 알 수 있었던 시간이었지만.. 코드는 참... 그렇습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;
import java.util.StringTokenizer;



public class Main {
	// Meeting(회의) 클래스, 시작시간과, 종료시간을 갖습니다.
	public static class Meeting {
		int startTime;
		int endTime;
		
		public Meeting(int startTime, int endTime) {
			this.startTime = startTime;
			this.endTime = endTime;
		}
	}
	
	// list와 endTime(종료시간)을 인자로 받아 리스트 중에 종료 시간 보다 앞의 시간의 시작시간을 갖는 회의(Meeting)를 삭제합니다.
	// 예를 들어, 1~4 회의 시간이 결정 됐으면 1~3의 시작시간을 가지고 있는 회의들은 배치할 수 없기 때문입니다. 
	public static void deleteEarlyStartTime(List<Meeting> meetings, int endTime) {
		
		Iterator<Meeting> iter = meetings.iterator();
		
		while (iter.hasNext()) {
			if(iter.next().startTime < endTime) {
				iter.remove();
			}
		}
	}
	
	// list의 객체들 중에 제일 앞에 배치할 수 있는 회의를 찾아 반환해줍니다.
	// 종료 시간을 비교해 찾아주는데, 종료 시간이 같다면 시작 시간이 더 빠른 객체를 찾습니다.
	// 시작시간이 더 빠른 객체를 찾는 이유는 만약 5~6과 6~6이 있을 때 시작시간이 더 긴 6~6을 먼저 찾게 되면 5~6은 건너뛰기 때문입니다.
	public static Meeting findEarliestMeeting(List<Meeting> meetings) {
		Meeting tempMeeting = meetings.get(0);
		
		for(Meeting meeting : meetings) {
			
			if(meeting.endTime < tempMeeting.endTime)
				tempMeeting = meeting;
			else if(meeting.endTime == tempMeeting.endTime) {			
				if(meeting.startTime < tempMeeting.startTime)
					tempMeeting = meeting;
			}
		}
		
		return tempMeeting;
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			
			if(n < 1 || n > 100000)
				return;
			
			List<Meeting> meetings = new LinkedList<>();
			
			for (int i = 0; i < n; i++) {
				String times = br.readLine().trim();
				StringTokenizer stk = new StringTokenizer(times);
				
				if(stk.countTokens() != 2)
					return;
				
				int startTime = Integer.parseInt(stk.nextToken());
				int endTime = Integer.parseInt(stk.nextToken());
				
				Meeting meeting = new Meeting(startTime, endTime);
				meetings.add(meeting);
			}
			int meetingCount = 0;
			
            // list가 비어있을 때 까지 반복합니다.
			while(!meetings.isEmpty())
			{
				Meeting tempMeeting = findEarliestMeeting(meetings);		
				
				++meetingCount;
				deleteEarlyStartTime(meetings,tempMeeting.endTime);		
			}
			
			bw.write(Integer.toString(meetingCount));
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 수정 후 코드

[다른 사람이 한 것](https://st-lab.tistory.com/145)을 참고하여 다시 구현해본 것입니다. [comparator](https://dev-splin.github.io/java/Java-Comparable,Comparator/)와 [람다식](https://dev-splin.github.io/java/Java-functional-programming(Lambda)/)을사용하는 게 되게 간편하다는 걸 알았습니다!

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.StringTokenizer;



public class Main {
	public static class Meeting {
		int startTime;
		int endTime;
		
		public Meeting(int startTime, int endTime) {
			this.startTime = startTime;
			this.endTime = endTime;
		}
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			
			if(n < 1 || n > 100000)
				return;
			
			List<Meeting> meetings = new ArrayList<>();
			
			for (int i = 0; i < n; i++) {
				String times = br.readLine().trim();
				StringTokenizer stk = new StringTokenizer(times);
				
				if(stk.countTokens() != 2)
					return;
				
				int startTime = Integer.parseInt(stk.nextToken());
				int endTime = Integer.parseInt(stk.nextToken());
				
				Meeting meeting = new Meeting(startTime, endTime);
				meetings.add(meeting);
			}
			
			// 람다식과 comprator를 이용하여 endTime 기준으로 오름차순 정렬 합니다.
			// 이 때, endTime이 같으면 startTime이 더 빠른(작은) 회의를 앞에 둡니다.
			Collections.sort(meetings,(meeting1,meeting2b)->{
				if(meeting1.endTime == meeting2b.endTime)
					return meeting1.startTime - meeting2b.startTime;
				return meeting1.endTime - meeting2b.endTime;
			});
			
			int meetingCount = 0;
			int prevEndTime = 0;
			
			// 리스트를 순회하면서 이전에 배치된 endTime 이후에 시작되는 회의를 구하고
			// 배치될 수 있으면 이전 endTime을 배치 될 회의의 endTime으로 바꿉니다.
			for(Meeting meeting : meetings) {
				if(meeting.startTime >= prevEndTime) {
					prevEndTime = meeting.endTime;
					++meetingCount;
				}
			}
			
			
			bw.write(Integer.toString(meetingCount));
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

