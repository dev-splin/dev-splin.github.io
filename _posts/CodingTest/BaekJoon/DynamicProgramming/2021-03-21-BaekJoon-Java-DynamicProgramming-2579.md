---
title: "BaekJoon : 2579번(계단 오르기)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Dynamic Programming"
toc: true
toc_sticky: true
toc_label: 목차

---

# Java : BaekJoon Dynamic Programming

BaekJoon Dynamic Programming(동적 프로그래밍)  저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 2579

계단 오르기 게임은 계단 아래 시작점부터 계단 꼭대기에 위치한 도착점까지 가는 게임이다. <그림 1>과 같이 각각의 계단에는 일정한 점수가 쓰여 있는데 계단을 밟으면 그 계단에 쓰여 있는 점수를 얻게 된다.

![img](https://www.acmicpc.net/upload/images/k64or2GOK1vmpEig7Ud.png)

예를 들어 <그림 2>와 같이 시작점에서부터 첫 번째, 두 번째, 네 번째, 여섯 번째 계단을 밟아 도착점에 도달하면 총 점수는 10 + 20 + 25 + 20 = 75점이 된다.

![img](https://www.acmicpc.net/upload/images/f62omMF2kQYD5rDct.png)

계단 오르는 데는 다음과 같은 규칙이 있다.

1. 계단은 한 번에 한 계단씩 또는 두 계단씩 오를 수 있다. 즉, 한 계단을 밟으면서 이어서 다음 계단이나, 다음 다음 계단으로 오를 수 있다.
2. 연속된 세 개의 계단을 모두 밟아서는 안 된다. 단, 시작점은 계단에 포함되지 않는다.
3. **마지막 도착 계단은 반드시 밟아야 한다.**

따라서 **첫 번째 계단을 밟고 이어 두 번째 계단이나, 세 번째 계단으로 오를 수 있다. 하지만, 첫 번째 계단을 밟고 이어 네 번째 계단으로 올라가거나, 첫 번째, 두 번째, 세 번째 계단을 연속해서 모두 밟을 수는 없다.**

각 계단에 쓰여 있는 점수가 주어질 때 이 게임에서 얻을 수 있는 총 점수의 최댓값을 구하는 프로그램을 작성하시오.



### 입력

입력의 첫째 줄에 계단의 개수가 주어진다.

둘째 줄부터 한 줄에 하나씩 제일 아래에 놓인 계단부터 순서대로 각 계단에 쓰여 있는 점수가 주어진다. 계단의 개수는 300이하의 자연수이고, 계단에 쓰여 있는 점수는 10,000이하의 자연수이다.

```java
6		// 계단의 개수를 입력
10
20
15
25
10
20		// 계단의 계수만큼 점수를 입력합니다.
```



### 출력

첫째 줄에 계단 오르기 게임에서 얻을 수 있는 총 점수의 최댓값을 출력한다.

```java
75		// 계단을 올라서 얻을 수 있는 점수의 최댓값을 출력합니다.
```



### 내코드

조건이 까다로운 문제입니다. 아직 DP에 익숙치 않은지 매우 헤매면서 풀었습니다.. ㅠ

주의 할 조건은 `2.연속된 세 개의 계단을 모두 밟아서는 안 된다.`라는 조건입니다. <br>문제를 풀 때 이용할 수 있는 조건은 `마지막 도착 계단은 반드시 밟아야 한다.`라는 조건입니다.<br>또, 마지막 계단이 0이나 1, 2인 경우에는 쉽게 유추할 수 있으므로 이런 조건들은 미리 값을 넣어두고 푸는 것이 좋은 것 같습니다.(이 때 계단의 수를 잘 체크해야 합니다. ex)마지막 계단이 1인데 2를 입력하는 경우 )

조건이 이렇게 복잡할 경우 머리 싸매고 있는 것 보다 공책같은 곳에 경우의 수가 작은 것 부터 나열해보면서 찾는 것이 좋아보입니다.<br>이 때, 경우의 수를 구하는 조건을 잘 체크해야합니다. 1번 조건을 보면 최대 2칸 오를 수 있으므로 `(5,2,..)`같은 조합은 이루어질 수 없습니다.<br>한번 `마지막 계단 -> (만들 수 있는 경우),(경우2),...` 형식으로 표현하겠습니다. 

`1 -> 1` <br>`2 -> (2,1) (2)`<br>`3 -> (3,2) (3,1)`<br>`4 -> (4,3,1) (4,2,1) (4,2)`<br>`5 -> (5,4,2,1) (5,4,2) (5,3,2) (5,3,1)`

이렇게 보면.. 음.. 뭔가 비슷한 것들이 보입니다. **4의 경우**에서 `(4,2,1) (4,2)`는 `(2,1) (2)` 즉, **2의경우에서 각각 + 4**인 것을 볼 수 있습니다. 하지만 `(4,3,1)`은 좀 애매합니다. <br>그렇기 때문에 5로 넘어가서 **5의 경우**도 잘 보면 `(5,4,2,1) (5,4,2)`에서 `2의 경우 (2,1) (2)`가 보이고 어라라? `3의경우 (3,2) (3,1)`도 보입니다.. 뭔가 패턴이 그려지는 것 같은 느낌이 확 듭니다.

이걸 그대로 n으로 표현해보면 **n이 4**일 때, `(4,2,1) (4,2)`는 `n-2인 경우 + n`인 것을 볼 수 있습니다. `(4,3,1)`은 아직도 좀 애매한 것 같으니 5부터 확인해봅니다.

**n이 5**일 때, `(5,3,2) (5,3,1)`은 위와 같이 `n-2인 경우 + n`으로 볼 수 있습니다. 그럼 `(5,4,2,1) (5,4,2)`은 어떨까요? 4의 경우가 포함됐다고 보기엔 (4,3,1)이 없습니다. 하지만 2의 경우는 보이네요? 그럼 이걸 n으로 표현해 보면, `n-3인 경우 + (5,4)`인데.. 4도 공통으로 들어가는 거 보니 n으로 표현할 수 있다는 생각이 드네요? 그럼.. `n-3인경우 + n-1 + n`이 되겠습니다. 

하지만.. **n이 4일 때**와 **n이 5일 때** `n-2인 경우 + n`는 찾았는데.. 아직 애매해서 패스한 (4,3,1)이 남아있습니다.. 근데 또 잘 보니??.. `n-3인경우 + n-1 + n`이 적용이 되네요???!! 뭔가 패턴을 찾은 것 같습니다!

문제는 **n의 계단까지 도달할 수 있는 경우의 수 중 최대 점수를 구하는 것** 이기 때문에 `n-2인 경우 + n` , `n-3인경우 + n-1 + n` 중에서 최대 값을 찾으면 됩니다.

그럼 코드를 한번 보겠습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	// 계단의 점수를 저장할 배열 변수
	static int stairs[];
	// n계단까지 나올 수 있는 경우의 수들 중 최대 값을 저장할 배열 변수
	static Integer memo[];
	
	// maxScore(n)은 n계단까지 나올 수 있는 경우의 수들 중 최대 값을 반환해주는 메서드 입니다.
	public static int maxScore(int num) {
		// 찾은 패턴을 식으로 표현 합니다. (n-2의 경우 + n) , (n-3의 경우 + n-1 + n)에서 n은 공통되므로 밖으로 빼주고
		// (n-2의 경우) , (n-3의 경우 + n-1) 즉, maxScore(n-2), maxScore(n-3) + stairs[n-1]로 표현할 수 있습니다.
		if(memo[num] == null) 
			memo[num] = Math.max(maxScore(num-3) + stairs[num-1], maxScore(num-2)) + stairs[num];
		
		return memo[num];
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			
			stairs = new int[n+1];
			memo = new Integer[n+1];
			
			for (int i = 1; i <= n; i++) {
				stairs[i] = Integer.parseInt(br.readLine().trim());
			}
			
			// 쉽게 값을 유추할 수 있는 초기의 값들은 넣어주고 시작 합니다.
			memo[0] = stairs[0];
			memo[1] = stairs[1];
			// n이 1이 들어올 수도 있기 때문에 범위를 보고 2의 값을 넣어줍니다.
			if(n >= 2)
				memo[2] = stairs[1] + stairs[2];
			
			int result = maxScore(n);
			
			bw.write(Integer.toString(result));
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```

항상 그렇듯 DP문제의 핵심은 패턴을 찾는 것인데(이게 아직은 정말 어렵습니다...), 이 문제를 풀면서 머리 싸매는 것 보다 **조그만 경우의 수 부터 그려가며 패턴을 찾는 것**과 **쉽게 값을 유추할 수 있는 값들은 미리 값을 넣어두는 것**이 좋다는 것을 깨달았습니다.

참고 : [https://st-lab.tistory.com/132](https://st-lab.tistory.com/132)

[https://sundries-in-myidea.tistory.com/22](https://sundries-in-myidea.tistory.com/22)

---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

