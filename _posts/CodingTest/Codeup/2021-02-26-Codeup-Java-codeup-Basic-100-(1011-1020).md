---
title: "Codeup : 기초 100제(1011 ~ 1020)"
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

# Java : codeup 기초 100제 (1011~1020)

codeup 기초 100제 저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 1011

문자형(char)으로 변수를 하나 선언하고, 변수에 문자를 저장한 후
변수에 저장되어 있는 문자를 그대로 출력해보자.

```java
c // c를 입력하면
c // c가 출력됩니다.
```



### 내코드

```java
import java.util.Scanner;

class Main {  
  public static void main(String args[]) { 
    Scanner sc = new Scanner(System.in);
    
     char c = sc.next().charAt(0);
     
     System.out.println(c);
  } 
}
```



## 1012

실수형(float)로 변수를 선언하고 그 변수에 실수값을 저장한 후
저장되어 있는 실수값을 출력해보자.

```java
1.414213 // 실수를 입력하면
1.414213 // 실수가 출력됩니다.
```



### 내코드

```java
import java.util.Scanner;

class Main {  
  public static void main(String args[]) { 
      Scanner sc = new Scanner(System.in);
      
      float x = sc.nextFloat();
      System.out.printf("%f",x);
  } 
}
```



## 1013

정수(int) 2개를 입력받아 그대로 출력해보자.

```java
1 2	// 1과 2를 입력하면
1 2 // 그대로 출력됩니다.
```



### 내코드

```java
import java.util.Scanner;

class Main {  
  public static void main(String args[]) { 
      Scanner sc = new Scanner(System.in);
      
      int a,b;
      
      a = sc.nextInt();
      b = sc.nextInt();
      
      System.out.printf("%d %d",a,b);
  } 
}
```



## 1014

2개의 문자(ASCII CODE)를 입력받아서 순서를 바꿔 출력해보자.

```java
a b // a, b를 입력하면
b a // 순서가 바뀌어서 출력됩니다.
```



### 내코드

```java
import java.util.Scanner;

class Main {  
  public static void main(String args[]) { 
      Scanner sc = new Scanner(System.in);
      
      char a,b;
      a = sc.next().charAt(0);
      b = sc.next().charAt(0);
      
      System.out.printf("%c %c",b,a);
      sc.close();
  } 
}
```



## 1015

실수(float) 1개를 입력받아 저장한 후,
저장되어 있는 값을 소수점 셋 째 자리에서 반올림하여
소수점 이하 둘 째 자리까지 출력하시오.

```java
1.59254	// 실수를 입력하면
1.59	// 반올림해서 소수점 둘 째 자리까지 출력합니다.
```



### 내코드

```java
import java.util.Scanner;

class Main {  
  public static void main(String args[]) { 
      Scanner sc = new Scanner(System.in);
      
      float a = sc.nextFloat();
      
      System.out.printf("%.2f",a);
      sc.close();
  } 
}
```



## 1017

int형 정수 1개를 입력받아 공백을 사이에 두고 3번 출력해보자.

```java
125	// 125를 입력하면
125 125 125 // 125가 3개 출력됩니다.
```



### 내코드

```java
import java.util.Scanner;

class Main {  
  public static void main(String args[]) { 
      Scanner sc = new Scanner(System.in);
      
      int a = sc.nextInt();
      
      System.out.printf("%d %d %d",a,a,a);
      sc.close();
  } 
}

```



## 1018

어떤 형식에 맞추어 시간이 입력될 때, 그대로 출력하는 연습을 해보자.

입력받은 시간을 "시:분" 형식으로 출력한다.

```java
3:16	// 을 입력하면
3:16	// 이 출력된다.
```



### 내코드

#### Scanner.useDelimiter를 이용한 방법

```java
import java.util.Scanner;

class Main {  
  public static void main(String args[]) { 
      Scanner sc = new Scanner(System.in);
      
      sc = new Scanner(sc.next()).useDelimiter(":");
      // 입력한값을 가져와 ':'를 구분 패턴으로 설정해 나누어줍니다.
      int a = sc.nextInt();
      int b = sc.nextInt();
      
      System.out.printf("%d:%d",a,b);
      sc.close();
  } 
}
```

[useDelimiter에 대한 설명](https://dev-splin.github.io/java/Java-Scanner.useDelimeter/)



#### StringTokenizer를 이용한 방법

```java
package Codeup;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {

	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			String time = br.readLine();
			
			StringTokenizer stk = new StringTokenizer(time,":");
			
			List<Integer> timeSeparations = new ArrayList<>();
			// 만약 추후에 시:분:초로 출력할 수도 있다는 가정하에 list로 받았습니다. 
			
			while (stk.hasMoreTokens()) {
				int separatedTime = Integer.parseInt(stk.nextToken());
				
				timeSeparations.add(separatedTime);
			}
			
			System.out.printf("%d:%d",timeSeparations.get(0),timeSeparations.get(1));
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

}


```

[StringTokenizer에 대한 설명](https://dev-splin.github.io/java/Java-StringTokenizer/)



## 1019

년, 월, 일을 입력받아 지정된 형식으로 출력하는 연습을 해보자.

입력받은 연, 월, 일을 yyyy.mm.dd 형식으로 출력한다.

```java
2021.2.26	// 2021.2.26을 입력하면
2021.02.26	// 2021.02.26이 출력됩니다.
// 21.2.26 을 입력하면 0021.02.26 으로 나와야 정답처리가 됩니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			String date = br.readLine();
			
			StringTokenizer stk = new StringTokenizer(date,".");
			
			int i = 0;
			
			while (stk.hasMoreTokens()) {
				int datetoken = Integer.parseInt(stk.nextToken());
				if (i == 0)
					System.out.printf("%04d",datetoken);
				else
					System.out.printf(".%02d",datetoken);
				++i;
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



## 1020

주민번호는 다음과 같이 구성된다.

XXXXXX-XXXXXXX

앞의 6자리는 생년월일(yymmdd)이고 뒤 7자리는 성별, 지역, 오류검출코드이다.
주민번호를 입력받아 형태를 바꿔 출력해보자.

'-'를 제외한 주민번호 13자리를 모두 붙여 출력한다.

```java
000907-1121112 // 을 입력하면
0009071121112	// 가 출력됩니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		try {
			String Resident_Registration_number = br.readLine();
			
			StringTokenizer stk = new StringTokenizer(Resident_Registration_number,"-");
			
			while (stk.hasMoreTokens()) {
				System.out.print(stk.nextToken());
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

