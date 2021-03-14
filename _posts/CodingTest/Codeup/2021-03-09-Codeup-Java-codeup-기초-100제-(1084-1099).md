---
title: "Codeup : 기초 100제(1084 ~ 1099)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - Codeup 
tags:
  - "Coding Test"
  - "Codeup : 기초 100제"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : codeup 기초 100제 (1084~1099)

codeup 기초 100제 저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 1084

빨강(red), 초록(green), 파랑(blue) 빛을 섞어
여러 가지 빛의 색을 만들어 내려고 한다.

빨강(r), 초록(g), 파랑(b) 각각의 빛의 개수가 주어질 때,
(빛의 강약에 따라 0 ~ n-1 까지 n가지의 빛 색깔을 만들 수 있다.)

주어진 rgb 빛들을 다르게 섞어 만들 수 있는 모든 경우의 조합(r g b)과
총 가짓 수를 계산해보자.



빨녹파(r, g, b) 각 빛의 강약에 따른 가짓수(0 ~ 128))가 공백을 사이에 두고 입력된다.
예를 들어, 3 3 3 은 각 색깔 빛에 대해서 그 강약에 따라 0~2까지 3가지의 색이 있음을 의미한다.

만들 수 있는 rgb 색의 정보를 오름차순(계단을 올라가는 순, 12345... abcde..., 가나다라마...)으로
줄을 바꿔 모두 출력하고, 마지막에 그 개수를 출력한다.

```java
2 2 2	// r g b 값이 입력되면
0 0 0
0 0 1
0 1 0
0 1 1
1 0 0
1 0 1
1 1 0
1 1 1
8		// 색 조합의 오름차순으로 출력되고 마지막에 개수가 출력됩니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String nums = br.readLine().trim();
			
			//rgb값을 받을 int 배열
			int rgb[] = new int[3];
			
			StringTokenizer stk = new StringTokenizer(nums);
			
			// r,g,b 3개의 값을 받아야 하므로 나눈 문자열이 3개가 안되면 프로그램을 종료합니다.
			if (stk.countTokens() != 3)
				return;
			
			// 값이 3개만 들어오게 했으므로 hasNextTokens()로 체크하지 않고 바로 넣어줍니다.
			rgb[0] = Integer.parseInt(stk.nextToken());
			rgb[1] = Integer.parseInt(stk.nextToken());
			rgb[2] = Integer.parseInt(stk.nextToken());
			
			for (int i = 0; i < rgb[0]; ++i) {
				for (int j = 0; j < rgb[1]; ++j) {
					for (int k = 0; k < rgb[2]; ++k) {
						bw.write(Integer.toString(i) + " ");
						bw.write(Integer.toString(j) + " ");
						bw.write(Integer.toString(k));
						bw.newLine();
					}
				}
			}
			// rgb 경우의 수 개수
			int count = rgb[0] * rgb[1]  * rgb[2];
			
			bw.write(Integer.toString(count));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1085

소리가 컴퓨터에 저장될 때에는 디지털 데이터화 되어 저장된다.

마이크를 통해 1초에 적게는 수십 번, 많게는 수만 번 소리의 강약을 체크해
그 값을 정수값으로 바꾸고, 그 값을 저장해 소리를 파일로 저장할 수 있다.

값을 저장할 때에는 비트를 사용하는 정도에 따라 세세한 녹음 정도를 결정할 수 있고,
좌우(스테레오) 채널로 저장하면 2배… 5.1채널이면 6배의 저장공간이 필요하고,
녹음 시간이 길면 그 만큼 더 많은 저장공간이 필요하다.

1초 동안 마이크로 소리강약을 체크하는 수를 h
(헤르쯔, Hz 는 1초에 몇 번? 체크하는가를 의미한다.)

한 번 체크한 결과를 저장하는 비트 b
(2비트를 사용하면 0 또는 1 두 가지, 16비트를 사용하면 65536가지..)

좌우 등 소리를 저장할 트랙 개수인 채널 c
(모노는 1개, 스테레오는 2개의 트랙으로 저장함을 의미한다.)

녹음할 시간 s가 주어질 때,

필요한 저장 용량을 계산하는 프로그램을 작성해보자.



h, b, c, s 가 공백을 두고 입력된다.
h는 48,000이하, b는 32이하(단, 8의배수), c는 5이하, s는 6,000이하의 자연수이다.

필요한 저장 공간을 MB 단위로 바꾸어 출력한다.
단, 소수점 둘째 자리에서 반올림해 첫째 자리까지 출력하고 MB를 공백을 두고 출력한다.

```java
44100 16 2 10	// h, b, c, s를 입력하면
1.7 MB	// MB단위로 바꾸어서 출력합니다.
```

`비트에서 바이트로, 바이트에서 킬로바이트로, 킬로 바이트에서...`  를 보면 1024가 되면 다음 단위로 바뀌므로 1024를 이용해 바뀐 단위를 구할 수 있습니다. (단, 처음 입력받은 단위는 bit이기 때문에 `8bit == 1byte`라는 것을 주의해주어야합니다.)



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String nums = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(nums);
			
			// 4개의 값이 다 입력되지 않았으면 프로그램을 종료합니다.
			if(stk.countTokens() != 4)
				return;
			
			// 값을 담을 변수, 각 수의 최대 수가 들어오면 연산 값이 커지기 때문에 long으로 주었습니다.
			long h,b,c,s = 0;
			
			// 정해진 4개의 값만 들어오게 했기 때문에 hasNextTokens()없이 진행합니다.
			// 각 수에 맞는 범위를 체크합니다.
			h = Long.parseLong(stk.nextToken());
			if(h > 48000)
				return;
			
			b = Long.parseLong(stk.nextToken());
			if(b > 32)
				return;
			
			c = Long.parseLong(stk.nextToken());
			if(c > 5)
				return;
	
			s = Long.parseLong(stk.nextToken());
			if(s > 6000)
				return;
			
			// 처음 받은 단위가 bit이기 때문에 byte로 바꾸어 줍니다.
			long bytes = (h * b * c * s) / 8;
			// 1024Byte == 1KB, 1024KB == 1MB 이기 때문에 1024*1024로 나누어 줍니다.
			// 이 때, 실수로 나와야 하기 때문에 형변환을 해줍니다.
			double result = (double)bytes / (1024 * 1024);
			
			bw.write(String.format("%.1f MB", result));	
			
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1093

정보 선생님은 수업을 시작하기 전에 이상한 출석을 부른다.

선생님은 출석부를 보고 번호를 부르는데,
학생들의 얼굴과 이름을 빨리 익히기 위해 번호를 무작위(랜덤)으로 부른다.

그리고 얼굴과 이름이 잘 기억되지 않는 학생들은 번호를 여러 번 불러
이름과 얼굴을 빨리 익히려고 하는 것이다.

출석 번호를 n번 무작위로 불렀을 때, 각 번호(1 ~ 23)가 불린 횟수를 각각 출력해보자.



첫 번째 줄에 출석 번호를 부른 횟수인 정수 n이 입력된다. (1 ~ 10000)
두 번째 줄에는 무작위로 부른 n개의 번호(1 ~ 23)가 공백을 두고 순서대로 입력된다.

```java
10	// 출석 번호를 부른 횟수를 입력하고
1 3 2 2 5 6 7 4 5 9	// 횟수에 맞게 무작위로 번호를 입력하면
1 2 1 1 2 1 1 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0	// 23번까지의 출석번호의 불린 횟수를 출력해줍니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			// 출석 번호를 부른 횟수
			int n = Integer.parseInt(br.readLine().trim());
			
			String nums = br.readLine().trim();
			
			StringTokenizer stk = new StringTokenizer(nums);
			
			// 무작위로 부른 번호의 개수와 처음 입력한 부른 횟수가 같지 않으면 프로그램 종료
			if(stk.countTokens() != n)
				return;
			
			// 23개의 출석번호 배열을 만듭니다.
			int attendanceNum[] = new int[23];
		
			while(stk.hasMoreTokens())
			{
				int num = Integer.parseInt(stk.nextToken());
                // 입력된 숫자가 출석번호가 아니면 넘어가줍니다.
                if(num < 1 || num > 23)
					continue;
                // 입력될 때 첫번 째는 1 이므로 -1을 빼줍니다.
				++attendanceNum[num-1];
			}
			
			for (int i = 0; i < attendanceNum.length; ++i) {
				bw.write(Integer.toString(attendanceNum[i]) + " ");
			}
		
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1096

기숙사 생활을 하는 학교에서 어떤 금요일(전원 귀가일)에는 모두 집으로 귀가를 한다.

오랜만에 집에 간 영일이는 아버지와 함께 두던 매우 큰 오목에 대해서 생각해 보다가
"바둑판에 돌을 올린 것을 프로그래밍 할 수 있을까?"하고 생각하였다.

바둑판(19 * 19)에 n개의 흰 돌을 놓는다고 할 때,
n개의 흰 돌이 놓인 위치를 출력하는 프로그램을 작성해보자.



바둑판에 올려 놓을 흰 돌의 개수(n)가 첫 줄에 입력된다.
둘째 줄 부터 n+1 번째 줄까지 힌 돌을 놓을 좌표(x, y)가 n줄 입력된다.
n은 10이하의 자연수이고 x, y 좌표는 1 ~ 19 까지이며, 같은 좌표는 입력되지 않는다.

흰 돌이 올려진 바둑판의 상황을 출력한다.
흰 돌이 있는 위치는 1, 없는 곳은 0으로 출력한다.

```java
5		// 흰돌의 개수를 입력하교
1 1
2 2
3 3
4 4
5 5		// 흰돌의 위치를 입력하면
1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0	
0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
// 19 X 19의 바둑판에 흰돌의 위치가 표시 됩니다.
```



### 내코드

```java
package Codeup.Basic100by1099;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.StringTokenizer;


public class b1096 {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			
			int goBoard[][] = new int[19][19]; 
			
			// n만큼 흰 돌의 위치를 입력 받고 해당 위치에 흰돌 유무를 표시해 줍니다.
			for (int i = 1; i <= n; ++i) {
				String nums = br.readLine().trim();
				StringTokenizer stk = new StringTokenizer(nums);
				
				// 두 개가 입력된게 아니면 프로그램을 종료합니다.
				if(stk.countTokens() != 2)
					return;
				
				// 토큰에서 숫자를 가져와 x,y에 넣습니다.
				int x = Integer.parseInt(stk.nextToken());
				int y = Integer.parseInt(stk.nextToken());
				
				// 제일 처음의 공간은 1 1 이라고 입력하기 때문에 배열에 저장할 때 -1해 줍니다.
				goBoard[x-1][y-1] = 1;
			}
			
			// 바둑판의 크기가 정해져 있기 때문에 19를 넣어줍니다.
			for (int i = 0; i < 19; i++) {
				for (int j = 0; j < 19; j++) {
					bw.write(Integer.toString(goBoard[i][j]) + " ");
				}
				bw.newLine();
			}
			
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1097

부모님을 기다리던 영일이는 검정/흰 색 바둑알을 바둑판에 꽉 채워 깔아 놓고 놀다가...

"십(+)자 뒤집기를 해볼까?"하고 생각했다.

바둑판(19 * 19)에 흰 돌(1) 또는 검정 돌(0)이 모두 꽉 채워져 놓여있을 때,
n개의 좌표를 입력받아 십(+)자 뒤집기한 결과를 출력하는 프로그램을 작성해보자.



바둑알이 깔려 있는 상황이 19 * 19 크기의 정수값으로 입력된다.
십자 뒤집기 횟수(n)가 입력된다.
십자 뒤집기 좌표가 횟수(n) 만큼 입력된다. 단, n은 10이하의 자연수이다.

```java
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0	// 19 * 19 크기의 바둑판을 배치합니다
2		// 십자 뒤집기 횟수를 입력합니다.
10 10
12 12	// 십자뒤집기를 할 x,y위치의 바둑알을 입력합니다. (x.y위치는 뒤집지 않습니다.)
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0	// 십자뒤집기 한 결과를 출력합니다.
```



### 내코드

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
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			// 19X19의 바둑판을 만듭니다.
			int goBoard[][] = new int[19][19];
			
			// 바둑판의 행의 개수만큼 반복해 열을 입력받고 저장합니다.
			for (int i = 0; i < goBoard.length; i++) {
				String nums = br.readLine().trim();
				StringTokenizer stk = new StringTokenizer(nums);
				
				// 입력된 열의 개수가 바둑판의 열의 개수와 다르면 프로그램을 종료합니다.
				if(stk.countTokens() != goBoard[0].length)
					return;
				
				//몇번 째 열인지 저장하는 변수
				int j = 0;
				while(stk.hasMoreTokens())
				{
					int num = Integer.parseInt(stk.nextToken());
					goBoard[i][j] = num;
					++j;
				}
			}
			
			// 뒤집기 횟수
			int flipNum = Integer.parseInt(br.readLine().trim());
            
            if(flipNum > 10)
				return;
			
			// 뒤집기 횟수만큼 입력받고 십자뒤집기를 해줍니다.
			for (int i = 0; i < flipNum; i++) {
				String nums = br.readLine().trim();
				StringTokenizer stk = new StringTokenizer(nums);
				
				// 입력된 x,y 가 2개가 아니면 프로그램을 종료합니다.
				if(stk.countTokens() != 2)
					return;
				
				// 만약 10,10을 입력받았다고 하면 배열에서 실제 인덱스는 9,9 이기 때문에 -1 해줍니다.
				int x = Integer.parseInt(stk.nextToken()) - 1;
				int y = Integer.parseInt(stk.nextToken()) - 1;
				
				// 십자 뒤집기를 위해 바둑판의 행의 갯수만큼 반복해 입력한 y번째 열을 바꿔줍니다.
				for (int j = 0; j < goBoard.length; j++) {
					if(goBoard[j][y] == 0)
						goBoard[j][y] = 1;
					else
						goBoard[j][y] = 0;
				}
				
				// 십자 뒤집기를 위해 바둑판의 열의 갯수만큼 반복해 입력한 x번째 행을 바꿔줍니다.
				for (int j = 0; j < goBoard[0].length; j++) {
					if(goBoard[x][j] == 0)
						goBoard[x][j] = 1;
					else
						goBoard[x][j] = 0;
				}
			}
			
			// 바둑판을 출력해줍니다.
			for (int i = 0; i < goBoard.length; i++) {
				for (int j = 0; j < goBoard[0].length; j++) {
					bw.write(Integer.toString(goBoard[i][j]) + " ");
				}
				bw.newLine();
			}
			
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1098

부모님과 함께 유원지에 놀러간 영일이는
설탕과자(설탕을 녹여 물고기 등의 모양을 만든 것) 뽑기를 보게 되었다.

길이가 다른 몇 개의 막대를 바둑판과 같은 격자판에 놓는데,

막대에 있는 설탕과자 이름 아래에 있는 번호를 뽑으면 설탕과자를 가져가는 게임이었다.
(잉어, 붕어, 용 등 여러 가지가 적혀있다.)



첫 줄에 격자판의 세로(h), 가로(w) 가 공백을 두고 입력되고,
두 번째 줄에 놓을 수 있는 막대의 개수(n)
세 번째 줄부터 각 막대의 길이(l), 방향(d), 좌표(x, y)가 입력된다.

입력값의 정의역은 다음과 같다.

1 <= w, h <= 100
1 <= n <= 10
d = 0 or 1
1 <= x <= 100-h
1 <= y <= 100-w

> 문제에 조금 혼란이 있습니다.  문제에선 좌표(x,y)라고 했는데, 보통 좌표(2,3)은 원점이 왼쪽 위라고 했을 때, 오른쪽으로 2칸 아래쪽으로 3칸으로 생각합니다. 하지만 문제 입출력을 잘 보면 `3 1 2 3` 부분에서 좌표(2,3)은 2행 3열을 나타내고 있는 것을 볼 수 있기 때문에 이 점에 유의하여 문제를 풀어야할 것 같습니다.

```java
5 5			// 격자판의 세로,가로를 입력하고
3			// 격자판에 놓을 막대의 개수
2 0 1 1		// 막대의길이, 방향(0(오른쪽),1(아래쪽)) x,y좌표를 입력합니다.
3 1 2 3
4 1 2 5
1 1 0 0 0	// 격자판에 막대가 있는 공간을 1로 표현해서 출력합니다.
0 0 1 0 1
0 0 1 0 1
0 0 1 0 1
0 0 0 0 1
```



### 내코드

```java
package Codeup.Basic100by1099;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;


public class b1098 {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			// 격자판의 가로,세로를 입력받습니다.
			String plaidSize = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(plaidSize);
			
			if(stk.countTokens() != 2)
				return;
			
			// 토큰으로 나눈 격자판의 세로길이(h)를 저장합니다.
			int h = Integer.parseInt(stk.nextToken());
			if(h < 1 || h > 100)
				return;
			
			// 토큰으로 나눈 격자판의 가로길이(w)를 저장합니다.
			int w = Integer.parseInt(stk.nextToken());
			if(w < 1 || w > 100)
				return;
			
			// w,h로 격자판을 만듭니다. 행,렬 이기 때문에 h,w 순으로 넣었습니다.
			int plaid[][] = new int[h][w]; 
			
			// 막대의 개수(n)을 입력받고 저장합니다.
			int n = Integer.parseInt(br.readLine().trim());
			if(n < 1 || n > 10)
				return;
			
			// 막대의 정보를 입력받고 격자판에 막대의 위치에 1을 채워줍니다.
			for (int i = 0; i < n; i++) {
				// 막대의 길이, 방향, 좌표를 입력받습니다.
				String stick = br.readLine().trim();
				stk = new StringTokenizer(stick);
				
				// 입력받아서 나눈 토큰의 수가 4개(길이,방향,좌표x,y)가 아니면 프로그램을 종료합니다.
				if(stk.countTokens() != 4)
					return;
				
				// 토큰으로 나눈 막대의 길이를 저장합니다.
				int l = Integer.parseInt(stk.nextToken());
				
				// 토큰으로 나눈 막대의 방향을 저장합니다. (0은 오른쪽, 1은 아래쪽)
				int d = Integer.parseInt(stk.nextToken());
				if(d != 0 && d != 1)
					return;
				
				// 토큰으로 나눈 막대의 좌표를 저장합니다. 
				int x = Integer.parseInt(stk.nextToken()) - 1;
				// x,y에 -1을 해주었기 때문에 0보다 작거나 w보다 크거나 같을 때 즉 격자판을 벗어나는 좌표면 종료해줍니다.
				if(x < 0 || x >= w)
					return;
				int y = Integer.parseInt(stk.nextToken()) - 1;
				if(y < 0 || y >= h)
					return;
				
				// 방향에 따라 x,y좌표에 연산을 할 변수
				int plusrow, pluscol = 0;
			
				// 방향이 0 즉 오른쪽이면 열(col)만 증가하고 행(row)은 증가하지 않습니다.
				if(d == 0) {
					plusrow = 0;
					pluscol = 1;
				}
				else {	// 아래쪽이면 행(row)만 증가하고 열(col)은 증가하지 않습니다.
					plusrow = 1;
					pluscol = 0;
				}
				
				// 막대의 길이만큼 반복하는데, 1부터 시작하지 않고 0부터 시작하게 하였습니다.
				// 그 이유는 연산할 변수 plusX,plusY에 j를 곱해주면 길이만큼 좌표에 더하게 됩니다.
				// 이 때, 0부터 시작하면 처음 시작 좌표에 더하지 않기 때문에 처음 시작좌표부터 표시할 수 있게 됩니다.
				for (int j = 0; j < l; j++) {
					int finalrow = x + plusrow * j;
					
					// row나 col이 격자판의 범위를 넘어서면 종료하게 됩니다.
					if(finalrow >= h)
						break;
					
					int finalcol = y + pluscol * j;
					if(finalcol >= w)
						break;
					
					plaid[finalrow][finalcol] = 1;
				}
			}
			
			for (int i = 0; i < plaid.length; i++) {
				for (int j = 0; j < plaid[0].length; j++) {
					bw.write(Integer.toString(plaid[i][j]) + " ");
				}
				bw.newLine();
			}			
			
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1099

영일이는 생명과학에 관심이 생겨 왕개미를 연구하고 있었다.

왕개미를 유심히 살펴보던 중 특별히 성실해 보이는 개미가 있었는데,
그 개미는 개미굴에서 나와 먹이까지 가장 빠른 길로 이동하는 것이었다.

개미는 오른쪽으로 움직이다가 벽을 만나면 아래쪽으로 움직여 가장 빠른 길로 움직였다.
(오른쪽에 길이 나타나면 다시 오른쪽으로 움직인다.)

이에 호기심이 생긴 영일이는 그 개미를 미로 상자에 넣고 살펴보기 시작하였다.

미로 상자에 넣은 개미는 먹이를 찾았거나, 더 이상 움직일 수 없을 때까지
오른쪽 또는 아래쪽으로만 움직였다.

미로 상자의 구조가 0(갈 수 있는 곳), 1(벽 또는 장애물)로 주어지고,
먹이가 2로 주어질 때, 성실한 개미의 이동 경로를 예상해보자.

단, 맨 아래의 가장 오른쪽에 도착한 경우, 더 이상 움직일 수 없는 경우, 먹이를 찾은 경우에는
더이상 이동하지 않고 그 곳에 머무른다고 가정한다.

미로 상자의 테두리는 모두 벽으로 되어 있으며,
개미집은 반드시 (2, 2)에 존재하기 때문에 개미는 (2, 2)에서 출발한다.

```java
1 1 1 1 1 1 1 1 1 1
1 0 0 1 0 0 0 0 0 1
1 0 0 1 1 1 0 0 0 1
1 0 0 0 0 0 0 1 0 1
1 0 0 0 0 0 0 1 0 1
1 0 0 0 0 1 0 1 0 1
1 0 0 0 0 1 2 1 0 1
1 0 0 0 0 1 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 1 1 1 1 1 1 1 1 1	// 10*10 크기의 미로 상자의 구조와 먹이의 위치가 입력됩니다.
1 1 1 1 1 1 1 1 1 1	// 개미가 이동한 경로를 9로 표시해 출력합니다.
1 9 9 1 0 0 0 0 0 1
1 0 9 1 1 1 0 0 0 1
1 0 9 9 9 9 9 1 0 1
1 0 0 0 0 0 9 1 0 1
1 0 0 0 0 1 9 1 0 1
1 0 0 0 0 1 9 1 0 1
1 0 0 0 0 1 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 1 1 1 1 1 1 1 1 1
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;


public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			// 10X10의 비어있는 미로를 만듭니다.
			int maze[][] = new int[10][10];
			
			// 미로의 행(세로의 길이) 만큼 반복해서 해당 행의 열을 입력해 미로를 만듭니다.
			for (int i = 0; i < maze.length; i++) {
				String colNums = br.readLine().trim();
				StringTokenizer stk = new StringTokenizer(colNums);
				
				// 입력된 열의 개수가 미로의 열과 다르면 프로그램을 종료합니다.
				if(stk.countTokens() != maze[0].length)
					return;
				
				// 열의 번호를 담는 변수
				int j = 0;
				
				// 나누어진 토큰을 i행 j열에 넣습니다.
				while(stk.hasMoreTokens()) {
					maze[i][j] = Integer.parseInt(stk.nextToken());
					++j;
				}
			}
			
			// 개미가 2,2(배열에선 1,1)에서 시작하기 때문에 개미의 행열을 설정해줍니다.
			int antRow = 1;
			int antCol = 1;
			
			// 개미의 오른쪽 행과 아래쪽 행을 설정해 줍니다.
			// 이 때, 오른쪽은 행은 같고 열만 다르기 때문에 열만 구하면 되고
			// 아래쪽은 열은 같고 행만 다르기 때문에 행만 구해줍니다.
			int rightCol = antCol + 1;
			
			int bottomRow = antRow + 1;
			
			// 개미가 먹이(2)를 찾거나 가장 오른쪽 아래에 도착할 때 까지 반복합니다.
			while(true) {
				// 개미의 위치가 2에 있으면 위치를 표시해주고 반복을 종료합니다.
				if(maze[antRow][antCol] == 2) {
					maze[antRow][antCol] = 9;
					break;
				}
				// 현재 개미의 위치를 표시해줍니다.
				maze[antRow][antCol] = 9;
				
				
				// 오른쪽과 아래쪽에 갈 수 있는 쪽이 없으면 오른쪽 맨 아래라는 뜻 이므로 반복을 종료합니다.
				if(maze[antRow][rightCol] == 1 && maze[bottomRow][antCol] == 1) {
					break;
				}				
				// 오른쪽으로 갈 수 있으면(0) 개미의 열을 오른쪽 열로 바꿔주고 오른쪽 열에 1을 더해줍니다.
				// "!= 1"이라고 준 이유는 먹이가 있을 때도 일단 이동해주기 위함입니다.
				else if(maze[antRow][rightCol] != 1) {
					antCol = rightCol;
					++rightCol;
				} 
				// 아래쪽으로 갈 수 있으면(0) 개미의 행을 아래쪽 행으로 바꿔주고 아래쪽 행에 1을 더해줍니다.
				else if(maze[bottomRow][antCol] != 1) {
					antRow = bottomRow;
					++bottomRow;
				}	
			}
			
			// 미로를 출력합니다.
			for (int i = 0; i < maze.length; i++) {
				for (int j = 0; j < maze[0].length; j++) {
					bw.write(Integer.toString(maze[i][j]) + " ");			
				}
				bw.newLine();
			}
			
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들과 사이의 포스팅하지 않은 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.