---
title: "Codeup : 기초 100제(1053 ~ 1083)"
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

# Java : codeup 기초 100제 (1053~1083)

codeup 기초 100제 저의 문제풀이 입니다. 기존의 문제를 조금만 수정하면 되는 문제들은 생략하였습니다.

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 1053

1(true, 참) 또는 0(false, 거짓) 이 입력되었을 때
반대로 출력하는 프로그램을 작성해보자.

정수 1개가 입력된다.(단, 0 또는 1 만 입력된다.)

```java
1	// 1(true)을 입력하면
0	// 0(false)가 출력됩니다.
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
			String strNums = br.readLine();
			
			int num = Integer.parseInt(strNums);
			
			if (num != 1 && num != 0)
				return;
			// 입력된 숫자가 1이나 0이 아니면 종료되게 합니다.
			num = Math.abs(num - 1);
			// java에서는 %d를 이용해 정수형에 '!'를 사용할 수 없습니다.
			// 그렇기 때문에 boolean으로 형변환 후 '!' 사용한 다음 다시 Integer로 변환 후 출력하는 방법이 있는데
			// 이 방법은 작업이 많아 질 거 같아 num에서 1을 빼고 절댓값을 이용해 '!'의 역할을 해주었습니다.
			// ex) abs(1 - 1) = 0 / abs(0 - 1) = 1
			
			bw.write(Integer.toString(num));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1054

두 개의 참(1) 또는 거짓(0)이 입력될 때,
모두 참일 때에만 참을 출력하는 프로그램을 작성해보자.

1 또는 0의 값만 가지는 2개의 정수가 공백을 두고 입력된다.

```java
1 1	// 1(true), 1(true)가 입력되면
1	// 1(true)가 출력됩니다.
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
			List<Boolean> nums = new ArrayList<Boolean>();
			
			while(stk.hasMoreTokens())
			{
				int intNum = Integer.parseInt(stk.nextToken());
				
				if (intNum != 1 && intNum!= 0)
					return;
				
				boolean num = intNum != 0;
				// java는 boolean에 true를 입력하지 않으면 전부 false로 받아 들이는 것 같습니다.
				// 그렇기 때문에 문자열에서 boolean으로 바로 변환하지 않고 정수형으로 변환 후 (1 != 0)의 결과 true false를 이용해 boolean에 값을 넣습니다.
				nums.add(num);
			}
				
			int result;
			if (nums.get(0) && nums.get(1))
				result = 1;
			else
				result = 0;
			
			bw.write(Integer.toString(result));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1064

입력된 세 정수 a, b, c 중 가장 작은 값을 출력하는 프로그램을 작성해보자.
단, 조건문을 사용하지 않고 3항 연산자 ? 를 사용한다.

```java
3 -1 5	// 3개의 값이 입력되면
-1		// 제일 작은 값이 출력됩니다.
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
				
			int firstSmallNum = nums.get(0) > nums.get(1) ? nums.get(1) : nums.get(0);
			// 0번 값과 1번 값을 비교해서 더 작은 값을 가져옵니다.
			int result = firstSmallNum > nums.get(2) ? nums.get(2) : firstSmallNum;
			// 위에 비교한 더 작은값과 2번 숫자를 비교해서 제일 작은 값을 가져옵니다.
			
			bw.write(Integer.toString(result));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1065

세 정수 a, b, c가 입력되었을 때, 짝수만 출력해보자.

```java
1 2 4	// 3개의 정수가 입력되면
2
4		// 짝수만 출력됩니다.
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
				if (num % 2 == 0)
					nums.add(num);
				// 짝수 값만 리스트에 넣어줍니다.
			}
				
			for (int num : nums)
			{
				bw.write(Integer.toString(num));
				bw.write("\r\n");
			}
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1069

평가를 문자(A, B, C, D, ...)로 입력받아 내용을 다르게 출력해보자.

평가 내용
평가 : 내용
A : best!!!
B : good!!
C : run!
D : slowly~
나머지 문자들 : what?

```java
A	// 영문자 1개가 입력되면
best!!	// 알맞은 평가 내용이 출력됩니다.
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
			char alphabet = br.readLine().toUpperCase().charAt(0);
			// 소문자가 들어오면 대문자로 바꿔서 인식할 수 있게 했습니다.

			switch (alphabet) {
			// 스위치에는 byte, short, char(ASCII로 변경), int 처럼 정수형만 올 수 있습니다. 
			case 'A':
				bw.write("best!!!");
				break;
			case 'B':
				bw.write("good!!");
				break;
			case 'C':
				bw.write("run!");
				break;
			case 'D':
				bw.write("slowly~");
				break;
			default:
				bw.write("what?");
				break;
			}
			
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1071

정수가 순서대로 입력된다.
-2147483648 ~ +2147483647, 단 개수는 알 수 없다.

0이 아니면 입력된 정수를 출력하고, 0이 입력되면 출력을 중단해보자.
while( ), for( ), do~while( ) 등의 반복문을 사용할 수 없다.

```java
7 4 2 3 0 1 5 6 9 10 8	// 여러개의 정수가 입력되면
7
4
2
3	// 0이 나오기 전까지 줄을 바꿔 출력합니다.
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
	
	static InputStreamReader isr = new InputStreamReader(System.in);
	static BufferedReader br = new BufferedReader(isr);
	
	static OutputStreamWriter osw = new OutputStreamWriter(System.out);
	static BufferedWriter bw = new BufferedWriter(osw);
	// printNum() 함수에서 출력을 하기위해 class의 속성으로 주었습니다.
	// static main에서 사용할 변수나 함수에는 static을 붙여주어야 합니다.
	
	// StringTokenizer 객체를 받아 값이 있는지 확인 후, 정수로 바꾸고 0이 아닐 시 출력해주고 다시 printNum()함수를 부르는 재귀함수입니다. (0이 나올 때 까지 출력)
	public static void printNum(StringTokenizer stk) throws Exception {
		// BufferedReader나 Writer를 사용할 땐 반드시 예외처리를 해주어야 하는데 throws를 이용해 함수를 호출하는 곳에서 예외처리를 하게 해주었습니다.
		int num = 0;
		if(stk.hasMoreTokens())
			num = Integer.parseInt(stk.nextToken());

		if (num != 0)
		{
			bw.write(Integer.toString(num));
			bw.write("\r\n");
			printNum(stk);
		}			
	}
	
	public static void main(String[] args) {
		
		try {
			String strnums = br.readLine();
			StringTokenizer stk = new StringTokenizer(strnums);
			// 받은 정수들을 공백(default)을 기준으로 나누어줍니다.
			
			printNum(stk);				
									
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1072

n개의 정수가 순서대로 입력된다.
-2147483648 ~ +2147483647, 단 n의 최대 개수는 알 수 없다.

첫 줄에 정수의 개수 n이 입력되고,
두 번째 줄에 n개의 정수가 공백을 두고 입력된다.

```java
5	// 첫 줄에 정수의 개수를 정해주고
1 2 3 4 5	// 두 번째 줄에 정해준 개수 만큼 입력하면
1
2
3
4
5	// 출력됩니다.
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
		
	static OutputStreamWriter osw = new OutputStreamWriter(System.out);
	static BufferedWriter bw = new BufferedWriter(osw);
	static StringTokenizer stk;
	// 어디서나 사용할 수 있게 static으로 주었습니다.
	
	// 입력할 정수 개수와,입력받은 문자를 인자로 받고 입력받은 문자를 쪼갠 갯수와 입력한 정수개수를 체크해 주고 출력해주는 함수 
	public static void printNum(int count, String nums) throws Exception {
		stk = new StringTokenizer(nums);
		
		if (stk.countTokens() > count)	//입력한 숫자가 count보다 많으면 함수를 종료합니다. 
			return;
		
		while(stk.hasMoreTokens())	// 출력하는 부분
		{
			int num = Integer.parseInt(stk.nextToken());
			bw.write(Integer.toString(num));
			bw.newLine();
		}
	}
	
	public static void main(String[] args) {
		
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			int count = Integer.parseInt(br.readLine().trim());
			// 혹시 모를 공백을 제거 해주면서 정수를 입력 받습니다.
			String nums = br.readLine();
			
			printNum(count, nums);				
							
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1081

1부터 n까지, 1부터 m까지 숫자가 적힌
서로 다른 주사위 2개를 던졌을 때 나올 수 있는 모든 경우를 출력해보자.

주사위 2개의 면의 개수 n, m이 공백을 두고 입력된다.
단, n, m은 10이하의 자연수

```java
2 3		// 두 개의 정수가 입력되면
1 1
1 2
1 3
2 1
2 2
2 3		// 그 두개의 수로 조합할 수 있는 모든 경우의 수가 나옵니다.
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
			StringTokenizer stk = new StringTokenizer(nums);

			if(stk.countTokens() != 2)
				return;
			// 두 개의 숫자만 입력받아야 하기 때문에 나누어진 문자가 2개가 아니면 프로그램을 종료합니다.
			
			int firstNum = Integer.parseInt(stk.nextToken());
            
            if (firstNum > 10)
				return;
            
			int secondNum = Integer.parseInt(stk.nextToken());
            
            if (secondNum > 10)
				return;
			// 나누어진 숫자가 2개인 경우에만 실행되게 했으므로 hasMoreTokens로 체크하지 않아도 됩니다.
			
			for (int i = 1; i <= firstNum; ++i) {
				for (int j = 1; j <= secondNum ; ++j) {
					bw.write(Integer.toString(i) + " " + Integer.toString(j));
					bw.newLine();
				}
			}
			// 두 숫자가 조합되는 경우의 수를 출력해줍니다.
					
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



## 1082

16진수(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F)를 배운
영일(01)이는 16진수끼리 곱하는 16진수 구구단?에 대해서 궁금해졌다.

A, B, C, D, E, F 중 하나가 입력될 때,
1부터 F까지 곱한 16진수 구구단의 내용을 출력해보자.
(단, A ~ F 까지만 입력된다.)

```java
B	// A ~ F까지의 16진수가 입력되면
B*1=B
B*2=16
B*3=21
B*4=2C
B*5=37
B*6=42
B*7=4D
B*8=58
B*9=63
B*A=6E
B*B=79
B*C=84
B*D=8F
B*E=9A
B*F=A5	// 결과가 16진수로 출력되는 16진수 구구단(?)이 출력됩니다.
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
			int hexNum = Integer.valueOf(br.readLine().trim(),16);
			// 16진수를 10진수로 바꿔서 저장해줍니다.
            
			if(hexNum < 10)
				return;
			// A ~ F까지 입력이므로 10(A)보다 작으면 프로그램을 종료합니다.
			
			for (int i = 1; i < 16 ; ++i) {
				int result = hexNum * i;
				bw.write(Integer.toHexString(hexNum).toUpperCase() + "*");
				bw.write(Integer.toHexString(i).toUpperCase() + "=");
				bw.write(Integer.toHexString(result).toUpperCase());
				bw.newLine();
			}
            // 10진수로 계산을 한 후 16진수로 출력해줍니다. 
			
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



## 1083

3 6 9 게임을 하던 영일이는 3 6 9 게임에서 잦은 실수로 계속해서 벌칙을 받게 되었다.
3 6 9 게임의 왕이 되기 위한 마스터 프로그램을 작성해 보자.

10 보다 작은 정수 1개가 입력된다.
(1 ~ 9)

```java
9	// 정수를 입력하면
1 2 X 4 5 X 7 8 X	// 3,6,9가 포함된 숫자는 X로 나오면서 출력됩니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	
	// 숫자를 받아서 그 숫자에 3,6,9(박수를 칠 숫자)가 몇번 들어 갔는지 찾아주는 함수입니다. 
	public static int clapNum(int num) {
		// 몇 번의 박수가 나오는 지 저장할 변수입니다.
		int clapNum = 0;
	
		// num이 0일 때 까지 반복합니다.
		while(num != 0)
		{
			// 10의 나머지 연산을 이용해 1의 자리수가 3,6,9면 박수개수를 늘려줍니다. 
			if(num % 10 == 3 ||
					num % 10 == 6 ||
					 num % 10 == 9) {
					++clapNum;				
			}
			
			// 원래의 숫자를 10씩 나눠주어서 한 자리씩 줄입니다.
			// ex) 3456/10=345, 345/10=34, 34/10= 3
			num /= 10;
		}
				
		return clapNum;
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int num = Integer.parseInt(br.readLine().trim());
			// 숫자가 1~9 범위에서 벗어나면 프로그램을 종료합니다.
			if(num < 1 || num > 9)
				return;
			
			for (int i = 1; i <= num; ++i) {
				// 박수의 개수를 구하는 함수를 이용해 박수의 개수를 구합니다.
				int clapNum = clapNum(i);
				
				// 박수의 개수가 0이면 그냥 숫자를 출력해주고 0이 아니면 박수의 개수만큼 X를 출력해줍니다.
				if (clapNum == 0)
					bw.write(Integer.toString(i));
				else {
					for (int j = 1; j <= clapNum; ++j) {
						bw.write('X');
					}
				}
				bw.write(" ");
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