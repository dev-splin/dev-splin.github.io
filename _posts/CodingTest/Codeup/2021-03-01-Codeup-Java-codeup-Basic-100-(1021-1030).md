---
title: "Codeup : 기초 100제(1021 ~ 1030)"
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

# Java : codeup 기초 100제 (1021~1030)

codeup 기초 100제 저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 1021

1개의 단어를 입력받아 그대로 출력해보자.

한 단어가 입력된다.(단, 단어의 길이는 50자 이하이다.)

```java
Informatics	// 입력되면
Informatics	// 출력됩니다.
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
			char[] arr; 
			String data = br.readLine();
			
			if (data.length() < 50)	
				arr = data.toCharArray();
			// 단어 수가 50이 넘지 않으면 문자 배열에 그냥 저장하고
			else
				arr = data.substring(0, 50).toCharArray();
			// 50이 넘으면 50자만큼 잘라서 저장합니다.
			
			bw.write(arr);
			
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1022

공백 문자가 포함되어 있는 문장을 입력받고 그대로 출력하는 연습을 해보자.

공백이 포함되어 있는 한 문장이 입력된다.
단, 입력되는 문장은 여러 개의 단어로 구성되고, 엔터로 끝나며,
최대 길이는 2000 문자를 넘지 않는다.

```java
Programming is very fun!!	// 입력되면
Programming is very fun!!	// 출력됩니다.
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
			String data = br.readLine();
			
			if (data.length() > 2000)
				data.substring(0, 2000);
			
			bw.write(data);
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1023

실수 1개를 입력받아 정수 부분과 실수 부분으로 나누어 출력한다.

실수 1개가 입력된다.
(단, 입력값은 절댓값이 10000을 넘지 않으며, 소수점 이하 자릿수는 최대 6자리까지이고
0이 아닌 숫자로 시작한다.)

```java
1.414213	// 입력하면
1
414213		// 정수, 실수 나누어서 출력하지만 실수는 0.xxxx식이아니고 실수의 숫자부분만 나옵니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.math.BigDecimal;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String strnum = br.readLine();
			BigDecimal num = new BigDecimal(strnum);
			// 수를 입력받고 BigDecimal타입의 num에 넣습니다.
						
			if (Math.abs(num.doubleValue()) < 10000)	// 절대값 체크
			{
				int integerPart = num.intValue();
				bw.write(Integer.toString(integerPart));
				bw.write("\r\n");
				// BigDecimal의 정수값을 가져와 출력합니다.
								
				BigDecimal IntNum = new BigDecimal(Integer.toString(integerPart));
				// 실수 값을 구하려면 '원본숫자 - 정수' 를 해야합니다.
				// 정확한 실수 값 계산을 하려면 BigDecimal안의 연산 메소드를 사용해야 합니다.
				// BigDecimal의 연산 메소드는 BigDecimal타입만 받기때문에 정수값을 BigDecimal 타입을 만들어 넣어줍니다.

				BigDecimal deciamlNum = num.subtract(IntNum);
				// BigDecimal의 연산 메소드를 이용해 실수부분을 구합니다.

				BigDecimal absDecimalNum = deciamlNum.abs();
				// 정수 부분은 '-'가 나와도 상관없지만 실수부분이 '-'붙어서 나오면 안되기 때문에 절대값으로 만들어 줍니다.
				BigDecimal multiplyNum = new BigDecimal("10");
				// 실수부분은 '0.xxx' 형식이 아니고 정수처럼 'xxxx'형식으로 나와야 되기 때문에 곱할 수를 BigDecimal로 만들어 줍니다.
				// BigDecimal 연산으로 하는 이유는 '실수 * 10' 으로 하게되면 부동소수점의 특징 때문에 오차가 생기게 됩니다.
				
				int i = 1;
				// 여섯 번째 자리인지 체크하기 위한 변수
				
				while (true) {
					BigDecimal multiplyResult = absDecimalNum.multiply(multiplyNum);
					// 임시로 BigDecimal를 만들어 10을 곱해보고 
					
					if (multiplyResult.doubleValue() % 10 == 0)
						break;
					// 딱 나누어 떨어지면 반복문을 나갑니다.
					// ex) 0.4356는 4356으로 나와야 하기 때문에 43560이 됐을 때 원본 값을 업데이트 하지 않고 끝내는 것입니다.
				
					absDecimalNum = absDecimalNum.multiply(multiplyNum);
					// 원본 값을 변경해 줍니다.
					++i;
					
					if (i > 6)	// 7번째 자리를 연산하려고 한다면 끝냅니다.
						break;
				}
				bw.write(Integer.toString(absDecimalNum.intValue()));
				bw.write("\r\n");
				
				bw.flush();
				bw.close();
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

부동소수점 때문에 고생한 문제입니다..!! 부동소수점과 BigDecimal을 더 자세히 알고싶다면  Reference를 봐주세요!

### Reference

[부동소수점이란?](https://dev-splin.github.io/java/Java-Floating-Point/)

[BigDecimal이란?](https://dev-splin.github.io/java/Java-BigDecimal/)



## 1024

단어를 1개 입력받는다.<br>입력받은 단어(영어)의 각 문자를 한줄에 한 문자씩 분리해 출력한다.

단어(영어) 하나를 입력받는다.
(단, 단어의 길이는 20자 이하이다.)

```java
Boy	// 입력하면
'B'	
'o'
'y' // 다 따로 출력됩니다.
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
			String word = br.readLine();
			
			for (int i = 0; i < word.length(); ++i) {
				bw.write("\'" + word.charAt(i) + "\'");
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



## 1025

다섯 자리의 정수 1개를 입력받아 각 자리별로 나누어 출력한다.

다섯 자리로 이루어진 1개의 정수를 입력받는다.
(단, 10,000 <= 입력받는 수 <= 99,999 )

```java
75254	// 입력하면
[70000]
[5000]
[200]
[50]
[4]		// 각 자리의 숫자를 분리해 한 줄에 하나씩 [ ]속에 넣어 출력합니다.
```



### 내코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Iterator;
import java.util.List;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			List<Integer> nums = new ArrayList<Integer>();
			
			String num = br.readLine();
			
			int parsenum = Integer.parseInt(num);
			
			if(parsenum < 10000 || parsenum >= 100000)
				return;
			
			int i = 0;
			// 75254라면 7은 10000을 곱해주어야 하기 때문에 10의 n승의 n을 위한 변수입니다.
			
			while (parsenum > 0) // 계속 나눠 숫자가 0이 될 때 까지 반복합니다.
			{
				int singleDigit = parsenum % 10;
				// 맨 마지막 한자리 수를 구합니다. ex) 75254 % 10 = 4 
				
				singleDigit *= Math.pow(10, i);
				// 75254에서 2는 100을 곱해주어야 하기 때문에 10의 n승을 이용합니다.
				nums.add(singleDigit);
				
				++i;
				parsenum /= 10;
				// 원본값은 10으로 나누어 자리 수를 줄여줍니다. ex) 75254 / 10 = 7525
			}
			
			Collections.reverse(nums);
			// 현재 리스트에는 75254가 '4, 50, 200, ...' 식으로 들어가 있기 때문에 반전시켜줍니다.
			
			for(int digitNum : nums)
			{
				bw.write("[" + Integer.toString(digitNum) + "]");
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



## 1026

입력되는 시:분:초 에서 분만 출력해보자.

시 분 초가
시:분:초 형식으로 입력된다.

```java
17:23:57	// 입력되면
23			// 출력됩니다.
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
			String time = br.readLine();
			StringTokenizer separatedChar = new StringTokenizer(time,":");
			
			List<Integer> separatedTime = new ArrayList<Integer>();
			// 시, 분, 초를 하나씩 받아주기 위한 리스트
			
			while(separatedChar.hasMoreElements()) {
				separatedTime.add(Integer.parseInt(separatedChar.nextToken()));
			}
			
			bw.write(separatedTime.get(1).toString());
			bw.flush();
			bw.close();
				
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



## 1027

년월일을 출력하는 방법은 나라마다, 형식마다 조금씩 다르다.<br>년월일(yyyy.mm.dd)를 입력받아, 일월년(dd-mm-yyyy)로 출력해보자.

(단, 한 자리 일/월은 0을 붙여 두자리로, 년도도 0을 붙여 네자리로 출력한다.)

```java
2014.07.15	// 입력하면
15-07-2014	// 출력합니다.
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
			String date = br.readLine();
			StringTokenizer separatedCharacter = new StringTokenizer(date,".");
			
			List<String> separatedDates = new ArrayList<String>();
			// 분할된 문자를 날짜를 담는 리스트
			
			while(separatedCharacter.hasMoreTokens()) {
				separatedDates.add(separatedCharacter.nextToken());
			}
			
			Collections.reverse(separatedDates);
			// 리스트를 반전 시켜준다 'dd-mm-yyyy'가 되어야 하기 때문
			
			for (int i = 0; i < separatedDates.size(); i++) {
				if (i != 0)
					bw.write("-");
				// 첫 번째 인덱스 앞엔 '-' 가 나오면 안됩니다.
				
				bw.write(separatedDates.get(i));
			}
			
			bw.flush();			
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



## 1028

정수 1개를 입력받아 그대로 출력해보자.
(단, 입력되는 정수의 범위는 0 ~ 4,294,967,295 이다.)

```java
2147483648	// 입력되면
2147483648 	// 출력됩니다.
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
			int unsignedNum = Integer.parseUnsignedInt(num);
			// String을 '0 ~ 4,294,967,295'범위를 가지는 UnsignedInt로 바꾸어 줍니다.
			// Java는 언어의 간결성을 위해 부호없는 타입을 제외했기 때문에 
			// 기본 UnsignedInt 형을 지원하지 않아 메소드를 이용해 사용해야 합니다.
			// Java는 부호없는 타입 사용을 권장하지 않는다고 합니다.
							
			bw.write(Integer.toUnsignedString(unsignedNum));
			// Integer.toUnsignedString을 사용하지 않으면 기본 정수형 최대 범위인 2147483647을 넘어가는 2147483648 부터
			// UnsignedInt 의 최대 범위인 4294967295과의 차(음수)가 나오기 때문에 
			// UnsignedInt를 출력할 때는 반드시 Integer.toUnsignedString을 사용하여야 합니다.
			
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1029

실수 1개를 입력받아 그대로 출력해보자.
(단, 입력되는 실수의 범위는 +- 1.7*10-308 ~ +- 1.7*10308 이다.)

소수점 아래 숫자가 11개 이하인 실수 1개가 입력된다.

```java
3.14159265359	// 입력되면
3.14159265359	// 출력됩니다.
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
			double realNum = Double.parseDouble(num);
			
			bw.write(String.format("%.11f", realNum)); 
			// String.format()을 사용할 때 "%.11f"로 소수점 자리 수를 지정해주면 그 아래의 소수점은 자동으로 반올림됩니다.
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



## 1030

정수 1개를 입력받아 그대로 출력해보자.
단, 입력되는 정수의 범위는
-9,223,372,036,854,775,808 ~ +9,223,372,036,854,775,807 이다.

```java
-2147483649	// 입력되면
-2147483649	// 출력됩니다.
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
			long longNum = Long.parseLong(num);
			// Java는 longlong이 없고 long이 longlong 범위만큼을 가집니다.
			
			bw.write(Long.toString(longNum));
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

