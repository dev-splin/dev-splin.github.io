---
title: "Codeup : 기초 100제(1041 ~ 1049)"
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

# Java : codeup 기초 100제 (1041~1049)

codeup 기초 100제 저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 1041

영문자 1개를 입력받아 그 다음 문자를 출력해보자.

영문자 'A'의 다음 문자는 'B'이고, 영문자 '0'의 다음 문자는 '1'이다.

```java
a	// 입력하면
b	// 출력합니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			char character = br.readLine().charAt(0);
			 ++character;
			 // 다음 문자를 출력하기위해 +1을 더 해줍니다.
			 
			 bw.write(character);
			 bw.flush();
			 bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1042

정수 2개(a, b) 를 입력받아 a를 b로 나눈 몫을 출력해보자.
단, -2147483648 <= a <= b <= +2147483647, b는 0이 아니다.

정수 2개(a, b)가 공백을 두고 입력된다.

```java
1 3	// 입력하면
0	// a(1)을 b(3)로 나눈 몫을 출력합니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String strNum = br.readLine();
			
			StringTokenizer stk = new StringTokenizer(strNum);
			
			List<Integer> nums = new ArrayList<Integer>();
			// 정수를 받기 위한 리스트
			
			while (stk.hasMoreTokens()) {
				nums.add(Integer.parseInt(stk.nextToken()));				
			}
			
			int result = nums.get(0) / nums.get(1);
			
			bw.write(Integer.toString(result));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1043

정수 2개(a, b) 를 입력받아 a를 b로 나눈 나머지를 출력해보자.
단, 0 <= a, b <= +2147483647, b는 0이 아니다.

정수 2개(a, b)가 공백을 두고 입력된다.

```java
10 3	// 두 숫자가 입력되면
1		// a(10)을 b(3)로 나눈 나머지가 나옵니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String strNums = br.readLine();
			
			StringTokenizer stk = new StringTokenizer(strNums);
			
			List<Integer> nums = new ArrayList<Integer>();
			// 정수를 받기 위한 리스트
			
			while (stk.hasMoreTokens()) {
				int num = Integer.parseInt(stk.nextToken());
				
				if(num < 0)
					return;
				// 숫자가 0보다 작으면 프로그램을 종료해줍니다.
				
				nums.add(num);				
			}
			
			int result = nums.get(0) % nums.get(1);
			
			bw.write(Integer.toString(result));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1044

정수를 1개 입력받아 1만큼 더해 출력해보자.
단, -2147483648 ~ +2147483647 의 범위로 입력된다.

주의
계산되고 난 후의 값의 범위(데이터형)에 주의한다.

```java
2147483647	// 정수가 입력되면
2147483648	// 출력됩니다. (정수 범위를 넘어서도 제대로 출력해줍니다.)
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String Strnum = br.readLine();
			long num = Long.parseLong(Strnum);
			++num;
			
			bw.write(Long.toString(num));
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1045

정수 2개(a, b)를 입력받아 합, 차, 곱, 몫, 나머지, 나눈 값을 자동으로 계산해보자.
단 0 <= a, b <= 2147483647, b는 0이 아니다.

```java
10 3	// 정수 두개가 입력되면
13		// 합
7		// 차
30		// 곱
3		// 몫
1		// 나머지
3.33	// 나눈 값을 실수로 소수점이하 셋째 자리에서 반올림해 출력합니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String Strnums = br.readLine();
			StringTokenizer stk = new StringTokenizer(Strnums);
			
			List<Integer> nums = new ArrayList<Integer>();
			
			while(stk.hasMoreTokens())
			{
				int num = Integer.parseInt(stk.nextToken());
				
				if (num < 0)
					return;
				
				nums.add(num);
			}
			
			int a = nums.get(0);
			int b = nums.get(1);
			double dividedNum = (double)a / (double)b;
			// 가독성을 위해 변수에 저장해놓습니다.
			
			bw.write(Integer.toString(a + b));
			bw.write("\r\n");
			bw.write(Integer.toString(a - b));
			bw.write("\r\n");
			bw.write(Integer.toString(a * b));
			bw.write("\r\n");
			bw.write(Integer.toString(a / b));
			bw.write("\r\n");
			bw.write(Integer.toString(a % b));
			bw.write("\r\n");
			bw.write(String.format("%.2f", dividedNum));
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1046

정수 3개를 입력받아 합과 평균을 출력해보자.
단, -2147483648 ~ +2147483647

```java
1 2 3	// 정수 3개가 입력되면
6		// 합과
2.0		// 평균이 소수점 이하 둘째 자리에서 반올림해서 출력됩니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String strNums = br.readLine();
			StringTokenizer stk = new StringTokenizer(strNums);
			List<Integer> nums = new ArrayList<Integer>();
			
			while (stk.hasMoreTokens()) {
				int num = Integer.parseInt(stk.nextToken());
				
				nums.add(num);
			}
			
			int sum = 0;
			
			for (int num : nums)
				sum += num;
			// list의 모든 값을 더해줍니다.
			
			double averageNum = (double)sum / (double)nums.size();
			
			bw.write(Integer.toString(sum));
			bw.write("\r\n");
			bw.write(String.format("%.1f", averageNum));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1047

정수 1개를 입력받아 2배 곱해 출력해보자.

```java
1024	// 정수를 입력하면
2048	// 두배로 출력됩니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String strNum = br.readLine();
			int num = Integer.parseInt(strNum);
			num <<= 1;
            // 비트연산자를 이용해 2배 해줍니다!
			
			bw.write(Integer.toString(num));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1048

정수 2개(a, b)를 입력받아 a를 2<sup>b</sup>배 곱한 값으로 출력해보자.
0 <= a <= 10, 0 <= b <= 10

정수 2개가 공백을 두고 입력된다.

```java
1 3	// 정수를 2개 입력하면
8	// 1에 2의 3승 곱한 값이 출력됩니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String strNums = br.readLine();
			StringTokenizer stk = new StringTokenizer(strNums);
			List<Integer> nums = new ArrayList<Integer>();
			
			while(stk.hasMoreTokens())
			{
				int num = Integer.parseInt(stk.nextToken());
				
				if(num < 0 || num > 10)
					return;
				// 숫자가 0~10 사이가 아니면 종료됩니다.
				nums.add(num);
			}
			int result = nums.get(0) * (int)Math.pow(2, nums.get(1));
			// a * 2^b
            
			bw.write(Integer.toString(result));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1049

두 정수(a, b)를 입력받아

a가 b보다 크면 1을, a가 b보다 작거나 같으면 0을 출력하는 프로그램을 작성해보자.

두 정수 a, b가 공백을 두고 입력된다.
-2147483648 <= a, b <= +2147483647

```java
9 1		// 정수 2개가 입력되면
1		// a(9) 가 b(1)보다 크면 1, 작거나 같으면 0을 출력합니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String strNums = br.readLine();
			StringTokenizer stk = new StringTokenizer(strNums);
			List<Integer> nums = new ArrayList<Integer>();
			
			while(stk.hasMoreTokens())
			{
				int num = Integer.parseInt(stk.nextToken());
				
				nums.add(num);
			}
			
			int result;
			
			if (nums.get(0) > nums.get(1))
				result = 1;
			else
				result =0;
			
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