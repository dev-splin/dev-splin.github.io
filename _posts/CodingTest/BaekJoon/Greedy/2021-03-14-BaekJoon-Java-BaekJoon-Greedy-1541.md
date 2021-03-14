---
title: "BaekJoon : 1541번(잃어버린 괄호)"
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



## 1541

세준이는 양수와 +, -, 그리고 괄호를 가지고 식을 만들었다. 그리고 나서 세준이는 괄호를 모두 지웠다.

그리고 나서 세준이는 괄호를 적절히 쳐서 이 식의 값을 최소로 만들려고 한다.

괄호를 적절히 쳐서 이 식의 값을 최소로 만드는 프로그램을 작성하시오.



첫째 줄에 식이 주어진다. 식은 ‘0’~‘9’, ‘+’, 그리고 ‘-’만으로 이루어져 있고, 가장 처음과 마지막 문자는 숫자이다. 그리고 연속해서 두 개 이상의 연산자가 나타나지 않고, 5자리보다 많이 연속되는 숫자는 없다. 수는 0으로 시작할 수 있다. 입력으로 주어지는 식의 길이는 50보다 작거나 같다.

첫째 줄에 정답을 출력한다.

```java
55-50+40	// 수식을 입력하면
-35		// 괄호를 이용해 만들 수 있는 최소값이 나옵니다.
```



### 방법1

최소 값을 구하기 위해 처음 `-`를 만나는 이후의 값을 전부 더해서 `-`연산을 해주었습니다.

예를 들어 `55-50+40+30+10-35` 가 있다고 했을 때, `55`는 첫번 째 값이니 그냥 더해주고 처음 만나는 `-`인 `-50` 이후의 값들을 전부 더합니다 `50+40+30+10+35=165`  이 값을 `55`에서 빼주면 `-110`으로 최소값이 나오게 됩니다.

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
			String equation = br.readLine().trim();
			StringTokenizer stk = new StringTokenizer(equation,"+-",true);
			
			List<Integer> nums = new ArrayList<>();
			List<Character> signs = new ArrayList<>();
			
			// '+'와 '-'로 나눈 토큰을 순회하여 토큰을 Object객체로 받고
			// try catch를 이용해 int형으로 형변환이 가능하면 int로 바꾸고 아니면 char형으로 바꿉니다. 
			while(stk.hasMoreElements()) {
				Object numOrSign = stk.nextElement();
				try {
					int num = Integer.parseInt((String)numOrSign);
					nums.add(num);
				} catch (NumberFormatException e) {
					char sign = numOrSign.toString().charAt(0);
					signs.add(sign);
				}
			}
			
			int tempsum = 0;
			int sum = nums.get(0);
			boolean meetMinus = false;
			
			// 첫번째 인덱스는 무조건 양수이므로, 숫자리스트의 1부터 순회하며 '-'를 만나기 전까지의 값은 총 합계에 더해주고
			// -를 만난뒤의 값은 임시합계에 다 더해준다음 "총합계 - 임시합계"를 합니다.
			for (int i = 1; i < nums.size(); i++) {
				if(signs.get(i-1) == '-')
					meetMinus = true;
				
				if(meetMinus)
					tempsum += nums.get(i);
				else 
					sum += nums.get(i);
			}
			sum -= tempsum;
			
			bw.write(Integer.toString(sum));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 방법2

먼저 식을 `-`를 기준으로 토큰으로 나눕니다. 그럼 `+`연산인 토큰들만 남는데, 이 토큰들을 `+`를 기준으로 나눈 후, 전부 더해서 총 합계에서 빼줍니다(이 때, 첫번 째 토큰은 무조건 양수이므로 주의해야합니다.)

예를 들어 `55-50+40+30+10-35` 에서 `-`기준으로 나누면 `55`, `50+40+30+10`, `35` 가 되는데, 이 나누어진 토큰들을 더해주고(`55`, `130`, `35`) 첫 번째 토큰을 제외한 나머지 토큰들을 빼줍니다.(`55-130-35 = 110`)

```java
package Greedy;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class g1541_2 {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			String equation = br.readLine().trim();
			StringTokenizer minusStk = new StringTokenizer(equation,"-");
			
			int sum = 0;
			boolean firstToken = true;
			
			// '-'를 기준으로 나눈 토큰을 순회면서 나눈 토큰을 '+' 로 또 나누고 '+'로 나눈 토큰들을 임시합계에 더해줍니다.
			// '-'로 나누었을 때, 첫번 째 토큰이면 "총 합계 += 임시합계", 다른 토큰이면 "총 합계 -= 임시합계". 즉, "첫 번째 토큰 - 나머지 토큰"이 됩니다.
			while(minusStk.hasMoreTokens()) {
				int tempsum = 0;
				StringTokenizer plusStk1 = new StringTokenizer(minusStk.nextToken(),"+");
				
				while(plusStk1.hasMoreTokens()) {
					int num = Integer.parseInt(plusStk1.nextToken());
					
					tempsum += num;
				}
				
				if(firstToken) {
					sum += tempsum;
					firstToken = false;
				}
				else
					sum -= tempsum;
			}
			
			bw.write(Integer.toString(sum));
			bw.flush();
			bw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

참고 : [https://st-lab.tistory.com/148](https://st-lab.tistory.com/148)



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.