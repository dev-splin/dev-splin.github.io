---
title: "BaekJoon : 1339번(단어 수학)"
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





## 1339

민식이는 수학학원에서 단어 수학 문제를 푸는 숙제를 받았다.

단어 수학 문제는 N개의 단어로 이루어져 있으며, **각 단어는 알파벳 대문자**로만 이루어져 있다. 이때, 각 **알파벳 대문자를 0부터 9까지의 숫자 중 하나로 바꿔서 N개의 수를 합하는 문제**이다. **같은 알파벳은 같은 숫자**로 바꿔야 하며, **두 개 이상의 알파벳이 같은 숫자로 바뀌어지면 안 된다.**

예를 들어, GCF + ACDEB를 계산한다고 할 때, A = 9, B = 4, C = 8, D = 6, E = 5, F = 3, G = 7로 결정한다면, 두 수의 합은 99437이 되어서 최대가 될 것이다.

N개의 단어가 주어졌을 때, 그 수의 합을 최대로 만드는 프로그램을 작성하시오.



첫째 줄에 단어의 개수 N(1 ≤ N ≤ 10)이 주어진다. 둘째 줄부터 N개의 줄에 단어가 한 줄에 하나씩 주어진다. 단어는 알파벳 대문자로만 이루어져있다. 모든 단어에 포함되어 있는 **알파벳은 최대 10개**이고, 수의 최대 길이는 8이다. 서로 다른 문자는 서로 다른 숫자를 나타낸다.

```java
2		// 단어의 개수(N)를 입력
GCF	
ACDEB	// 개수에 맞는 단어를 입력하면
99437	// 최대의 합이 출력됩니다.
```



### 내코드

Map을 이용해 각 자리수 에 해당하는 알파벳을 한 곳으로 모으고 높은 자리수에 있는 알파벳들 부터 9~0순으로 숫자를 부여해서 구하려고 했지만, 이 방식으로는 무조건 높은 자리수에 높은 숫자를 부여하기 때문에 `ABB BB BB BB BB BB BB BB BB BB`같은 단어가 들어오게 되면 `틀렸습니다`가 나옵니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			
			// 각 자리수의 알파벳을 받기위해 맵 안에 정수, 캐릭터형 리스트를 넣었습니다.
			// 입력한 순서대로 출력할 수 있게 LinkedHashMap을 사용했습니다.
			// ex) abc, defg가 있으면 1번 에는 c,g 2번에는 b,f가 들어가는 식으로
			Map<Integer, List<Character>> digitWords = new LinkedHashMap<>();
			
			// 맵에 알파벳단어의 자리수(integer)를 역순으로 넣어주고 그 자리수에 들어갈 알파벳들을 받을 리스트를 초기화합니다.
			for (int i = 7; i >= 0 ; --i) {
				digitWords.put(i, new ArrayList<>());
			}
			
			// 단어를 입력받고 char형 배열로 바꾼 후, map의 해당자리수(key)의 리스트에(value) 알파벳을 넣어줍니다.
			for (int i = 0; i < n; i++) {
				String word = br.readLine().toUpperCase().trim();
				char alphas[] = word.toCharArray();
				
				for (int j = 0; j < alphas.length; j++) {
					digitWords.get(alphas.length - j - 1).add(alphas[j]);
				}
			}
				
			// 어떤 알파벳이 어떤숫자를 의미하는 지 담기위한 맵입니다.
			Map<Character, Integer> alphaToInt = new HashMap<>();
			
			// 큰 자리수의 알파벳부터 검사하기 때문에 높은숫자부터 들어가야합니다. 때문에 9로 시작.
			int num = 9;
			int totalSum = 0;
			
			// 총 8자리의 단어(최대 abcdefgh 이런식으로) 중 해당 자리 수가 비어있다면(리스트에 없다면) 넘어가주고
			// 리스트에 값이 있으면 리스트를 순회하면서 alphaToInt에 해당 알파벳의 값이 정해져 있는 지 보고
			// 정해져 있다면 값을 넣고 num을 -- 해줍니다. 이렇게 되면 제일 큰 자리수에서 첫번 째 만나는 알파벳이 9가 되고 하나씩 내려갑니다.
			// 마지막으로 해당 알파벳의 숫자와 해당 자리수를 곱해서 총 합계에 더합니다. 
			for (int i = 7; i >= 0 ; --i) {
				if(digitWords.get(i).isEmpty())
					continue;
				
				for(char alpha : digitWords.get(i)) {
					if(!alphaToInt.containsKey(alpha)) {
						alphaToInt.put(alpha, num);
						--num;
					}
					totalSum += alphaToInt.get(alpha) * (int)Math.pow(10, i);
				}
			}
						
			bw.write(Integer.toString(totalSum));
			bw.flush();
			bw.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



### 더 좋은 방법

알파벳 개수만큼 정수형 배열로 만들어서`A(0)~Z(25)`, 알파벳이 사용된 모든 자리수를 더해준 다음, 많이 사용된 알파벳 부터 9~0까지 숫자를 부여한 후 결과를 저장합니다.

예를 들어, `c`가 `cdsq`, `dfsc`, `accd`에 사용되고 있으면 `c`에 해당하는 인덱스의 값은 `1000 + 1 + 110 = 1111`이 됩니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int n = Integer.parseInt(br.readLine().trim());
			
			// 아스키 코드를 이용해서 알파벳 A~Z를 인덱스로할 배열을 만듭니다.
			int alphabets[] = new int[26];

			// 단어 char배열로 입력받고 단어의 길이만큼 자리수(digit)를 만들어 주고(abcd면 10의 3승)
			// 해당 알파벳에-65(아스키코드)를 해서 해당 인덱스에 자리수를 더합니다.
			// 만약 ABC와 CDE가 있다고 하면 C(67 - 65)는 2번인덱스에 1 + 100이 들어가 있습니다.
			for (int i = 0; i < n; i++) {
				char word[] = br.readLine().toUpperCase().trim().toCharArray();
				
				int digit = (int)Math.pow(10, word.length - 1);
				for (int j = 0; j < word.length; j++) {
					alphabets[word[j] - 65] += digit;
					
					digit /= 10;
				}
			}
			
			// 알파벳들을 오름차순으로 정렬
			Arrays.sort(alphabets);
			
			// 해당 알파벳을 num으로 인식하게 하기위한 변수
			int num = 9;
			int sum = 0;
			
			// 알파벳들을 오름차순으로 정렬했기 때문에 거꾸로 순회하면서 제일 큰 숫자를 가지고 있는 알파벳 부터
			// 9~0순으로 부여해주고 결과를 구해줍니다. 이 때, 0이면 순회를 종료해줍니다.(알파벳을 사욯아지 않았으면 0이기 때문에)
			// c가 cdsq, dfsc, accd에 사용되고 있으면 c에 해당하는 인덱스의 값은 1000 + 1 + 110 = 1111 이됩니다.
			for (int i = alphabets.length -1 ; i >= 0; --i) {
				if(alphabets[i] == 0)
					break;
				
				sum += num * alphabets[i];
				--num;
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

참고 : [https://1-7171771.tistory.com/112](https://1-7171771.tistory.com/112)



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.