---
title: "BaekJoon : 14391(종이 조각)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Brute Force"
  - "BaekJoon : Bit Mask"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Brute Force

BaekJoon Brute Force(브루트포스)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 14391

영선이는 숫자가 쓰여 있는 직사각형 종이를 가지고 있다. 종이는 1×1 크기의 정사각형 칸으로 나누어져 있고, 숫자는 각 칸에 하나씩 쓰여 있다. 행은 위에서부터 아래까지 번호가 매겨져 있고, 열은 왼쪽부터 오른쪽까지 번호가 매겨져 있다.

영선이는 직사각형을 겹치지 않는 조각으로 자르려고 한다. 각 조각은 크기가 세로나 가로 크기가 1인 직사각형 모양이다. **길이가 N인 조각은 N자리 수**로 나타낼 수 있다. **가로 조각은 왼쪽부터 오른쪽까지 수**를 이어 붙인 것이고, **세로 조각은 위에서부터 아래까지 수**를 이어붙인 것이다.

아래 그림은 4×4 크기의 종이를 자른 한 가지 방법이다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/14391/1.png" alt="img" style="zoom: 33%;" />

각 조각의 합은 493 + 7160 + 23 + 58 + 9 + 45 + 91 = 7879 이다.

**종이를 적절히 잘라서 조각의 합을 최대로 하는 프로그램을 작성**하시오.



### 입력

**첫째 줄에 종이 조각의 세로 크기 N과 가로 크기 M**이 주어진다. **(1 ≤ N, M ≤ 4)**

**둘째 줄부터 종이 조각**이 주어진다. 각 **칸에 쓰여 있는 숫자는 0부터 9까지** 중 하나이다.

```java
4 3		// 세로 크기(N)와 가로 크기(M)를 입력합니다.
001		// 여기서부터
010
111
100		// 여기까지 종이 조각을 입력합니다.
```



### 출력

영선이가 얻을 수 있는 **점수의 최댓값**을 출력한다.

```java
1131	// 적절히 잘라 점수의 최대값을 출력합니다.
```



### 비트마스크

종이 조각이 나누어지는 **겹치지 않는 2가지 경우(가로,세로)를 완전탐색**해야 하므로 2가지의 경우를 비트(1과0)으로 표현할 수 있는 `비트마스크`를 사용하여 해결했습니다.

[비트마스크란?](https://blog.naver.com/ndb796/221312837477)

`비트 마스크`에 대한 자세한 설명은 링크를 통해 확인하시면 되겠습니다만, 간단히 설명하자면 **비트(1과0)와 비트연산을 이용해 배열을 사용한 것과 같은 효과**를 낼 수 있습니다. 다만 **배열 대신 정수형 숫자를 비트로 나누어 사용하기 때문에 메모리를 적게 사용**할 수 있습니다.

**4x4 배열**이 만들어 진다면 **배열을 1자로 쭉 늘어놓은 것 같은 4x4크기의 비트를 만들어 2차원 배열을 표현**할 수 있습니다.

표를 보면서 설명하겠습니다. **표의 값은 비트의 자리수** 입니다.(1 << 0 == 1)

|   4X4   | 0열  | 1열  | 2열  | 3열  |
| :-----: | :--: | :--: | :--: | :--: |
| **0행** |  0   |  1   |  2   |  3   |
| **1행** |  4   |  5   |  6   |  7   |
| **2행** |  8   |  9   |  10  |  11  |
| **3행** |  12  |  13  |  15  |  15  |

이렇게 4X4 2차원 배열을 16개(0~15)의 비트로 표현할 수 있습니다.<br>이 때, 표를 잘 보면 **행은 1증가할 때마다 비트 자리수는 행의 개수 만큼 증가(`[0][1]->1, [1][1]->5`)**하는 것을 볼 수 있고<br>**열은 1증가할 때마다 행이 증가하면서 증가된 비트 자리수에서  1씩 증가(`[1][0]->4(0행에서 1행이 증가하면서 증가된 비트 자리수 4), [1][1]->5`**하는 것을 볼 수 있습니다. **이 방식을 이용해 모든 경우를 순회하며 최대값을 찾을 수 있습니다.**



### 코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {
	static int paper[][];
	static int bitMask[];
	static int n;
	static int m;
	
	public static int findScore() {
		
		int result = 0; // 최대값이 저장 될 변수
		
		// 비트 마스크 사용 (겹치지 않는 모든 경우를 체크할 수 있습니다.)
		// 0을 가로, 1을 세로로 판단 
		// n X m 배열을 n * m길이의 비트로 표현 
		for (int i = 0; i < 1<<(n * m); i++) {
			
			int totalScore = 0; // 종이를 나누고 나온 숫자들을 전부 더할 변수
			
			int sum = 0; // 연속된 가로 숫자를 합해서 표현해 줄 변수 
			int row = 0; // 행을 표현해 줄 변수
			
			// 가로(행 고정, 열 증가)
			for (int j = 0; j < n; j++) {
				
				sum = 0;	
				row = j * m; // 행이 증가할 때마다 열의 개수(m)만큼 비트 자리수가 증가
				

				for (int k = 0; k < m; k++) { // 행이 증가한 만큼의 비트 자리수 + 열 == [행][열]
					if((i & 1<<(row + k)) == 0) { 
						sum *= 10; // 2차원 배열에는 한 자리의 수만 들어가 있기 때문에 연속될 때마다 먼저 연산한 숫자에 10을 곱해줍니다.
						sum += paper[j][k];
					}
					else { // 연속이 끊기면 totalScore에 저장해주고 sum을 누적하고 sum을 0으로 만듭니다.(연속이 끊겼기 때문)
						totalScore += sum;
						sum = 0;
					}
				}
				totalScore += sum;
			}
			
			// 세로(열 고정, 행 증가)
			for (int j = 0; j < m; j++) {
				sum = 0; 
				
				for (int k = 0; k < n; k++) {
					row = k * m;
					if((i & 1<<(row+j)) > 0) {
						sum *= 10;
						sum += paper[k][j];
					}
					else {
						totalScore += sum;
						sum = 0;
					}
				}
				totalScore += sum;
			}
			
			if(totalScore > result)
				result = totalScore;
		}
		
		return result;
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String nums = br.readLine();
			StringTokenizer stk = new StringTokenizer(nums);
			
			n = Integer.parseInt(stk.nextToken());
			m = Integer.parseInt(stk.nextToken());
			
			paper = new int[n][m];
			bitMask = new int[n];
			
			for (int i = 0; i < n; i++) {
				nums = br.readLine();
				for (int j = 0; j < m; j++)
					paper[i][j] = nums.charAt(j) - 48;
			}
			
			System.out.println(findScore());
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}


```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

