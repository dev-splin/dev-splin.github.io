---
title: "Codeup : 기초 100제(1031 ~ 1040)"
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

# Java : codeup 기초 100제 (1031~1040)

codeup 기초 100제 저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 1031

10진수를 입력받아 8진수(octal)로 출력해보자.

10진수 1개가 입력된다.
단, 입력되는 정수는 int 범위이다.

```java
10	// 입력하면
12	// 8진수로 출력됩니다.
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
			String num = br.readLine();
			int changedNum = Integer.parseInt(num);
			// num 문자열을 int로 변환하고
			
			bw.write(Integer.toOctalString(changedNum));
			// changedNum을 8진수 형태의 문자열로 출력합니다.
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1032

10진수를 입력받아 16진수(hexadecimal)로 출력해보자.

```java
255	// 입력하면
ff	// 16진수로 출력됩니다.
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
			String num = br.readLine();
			int changedNum = Integer.parseInt(num);
			
			bw.write(Integer.toHexString(changedNum));
            // int형을 16진수로 바꿔서 문자열로 출력합니다.
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1033

10진수를 입력받아 16진수(hexadecimal)로 출력해보자. (대문자로)

```java
255	// 입력하면
FF	// 10이 넘어가는 값은 대문자로 출력합니다.
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
			String num = br.readLine();
			int changedNum = Integer.parseInt(num);
			
			bw.write(Integer.toHexString(changedNum).toUpperCase());
			// int형을 16진수로 바꿔서 문자열로 출력합니다.(영어는 대문자로)
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



## 1034

8진수로 입력된 정수 1개를 10진수로 바꾸어 출력해보자.

```java
13	// 8진수를 입력하면
11	// 10진수로 출력됩니다.
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
			String num = br.readLine();
			
			bw.write(Integer.valueOf(num, 8).toString());
			// valueOf(num,8)은 8진수로 입력된 문자열을 10진수로 바꾸어 줍니다.
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1035

16진수로 입력된 정수 1개를 8진수로 바꾸어 출력해보자.

```java
f	// 16진수를 입력하면
17	// 8진수로 출력됩니다.
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
			String num = br.readLine().toUpperCase();
			// Integer.valueOf()에 16진수를 넣어줄 때 소문자면 오류가 나기 때문에 입력되는 모든 문자를 대문자로 바꾸어줍니다. 
			int changedNum = Integer.valueOf(num, 16);
			// 16진수형태의 문자열을 10진수로 바꾸어 받아옵니다.
			
			bw.write(Integer.toOctalString(changedNum));
			// 10진수를 8진수 형태의 문자열로 출력합니다.
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1036

영문자 1개를 입력받아 아스키 코드표의 10진수 값으로 출력해보자.

```java
A	// 영문이 입력되면
65	// 그에 해당하는 아스키코드가 출력됩니다.
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
			char alphabet = br.readLine().charAt(0);
			// 아스키코드로 변환하기 위해선 문자열이아니라 문자로 받아야합니다.
			int ascii = (int)alphabet;
			// 형변환을 하면 문자는 자동으로 아스키코드로 받아옵니다.
			
			bw.write(Integer.toString(ascii));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1037

10진 정수 1개를 입력받아 아스키 문자로 출력해보자.
단, 0 ~ 255 범위의 정수만 입력된다.

```java
65	// 정수가 입력되면
A	// 해당하는 아스키문자가 나옵니다
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
			String num = br.readLine();
			int intNum = Integer.parseInt(num);
			byte byteNum = (byte)intNum;
			char ascii = (char)(byteNum & 0xff);
			// java의 byte는 unsigned를 지원해주지 않기 때문에 비트연산을 통해 0~255로 만들어줍니다. (256이 입력되면 0으로 인식합니다.)
			
			bw.write(String.valueOf(ascii));
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

[byte양수 표현 및 0xff 사용법 에 대한 링크](https://emflant.tistory.com/133)



## 1038

정수 2개를 입력받아 합을 출력하는 프로그램을 작성해보자.
(단, 입력되는 정수는 -1073741824 ~ 1073741824 이다.)

```java
123 -123	// 정수 두 개가 입력되면
0			// 결과가 출력됩니다.
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
			String num = br.readLine();
			String[] tokens = num.split(" ");
			// 공백을 기준으로 문자열을 나누어 줍니다.
			long sum = 0;
			// 합을 저장할 변수. 만약 1073741824 + 1073741824 하게 되면 정수범위를 넘어가게 되므로 long타입으로 주었습니다.
			
			for (int i = 0; i < tokens.length; i++) {
				sum += Long.parseLong(tokens[i]);
			}
			
			bw.write(Long.toString(sum));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1040

입력된 정수의 부호를 바꿔 출력해보자.
단, -2147483647 ~ +2147483647 범위의 정수가 입력된다.

```java
-1	// 입력하면
1	// 부호가 바뀌어서 출력됩니다.
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
			String num = br.readLine();
			int resultNum = -Integer.parseInt(num);
			
			bw.write(Integer.toString(resultNum));
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