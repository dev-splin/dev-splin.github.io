---
title: "BaekJoon : 1918번 (후위 표기식)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : DataStructure"
  - "BaekJoon : Stack"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon DataStructure

BaekJoon `DataStructure`(자료구조)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 1918

수식은 일반적으로 3가지 표기법으로 표현할 수 있다. 연산자가 피연산자 가운데 위치하는 중위 표기법(일반적으로 우리가 쓰는 방법이다), 연산자가 피연산자 앞에 위치하는 전위 표기법(prefix notation), 연산자가 피연산자 뒤에 위치하는 후위 표기법(postfix notation)이 그것이다. 예를 들어 중위 표기법으로 표현된 a+b는 전위 표기법으로는 +ab이고, 후위 표기법으로는 ab+가 된다.

이 문제에서 우리가 **다룰 표기법은 후위 표기법**이다. 후위 표기법은 위에서 말한 법과 같이 연산자가 피연산자 뒤에 위치하는 방법이다. 이 방법의 장점은 다음과 같다. 우리가 흔히 쓰는 **중위 표기식 같은 경우에는 덧셈과 곱셈의 우선순위에 차이가 있어 왼쪽부터 차례로 계산할 수 없지만 후위 표기식을 사용하면 순서를 적절히 조절하여 순서를 정해줄 수 있다.** 또한 같은 방법으로 괄호 등도 필요 없게 된다. 예를 들어 a+b*c를 후위 표기식으로 바꾸면 abc*+가 된다.

중위 표기식을 후위 표기식으로 바꾸는 방법을 간단히 설명하면 이렇다. 우선 주어진 중위 표기식을 연산자의 우선순위에 따라 괄호로 묶어준다. 그런 다음에 괄호 안의 연산자를 괄호의 오른쪽으로 옮겨주면 된다.

예를 들어 **a+b*c는 (a+(b*c))의 식과 같게 된다. 그 다음에 안에 있는 괄호의 연산자 *를 괄호 밖으로 꺼내게 되면 (a+bc*)가 된다. 마지막으로 또 +를 괄호의 오른쪽으로 고치면 abc*+가 되게 된다.**

다른 예를 들어 그림으로 표현하면 A+B*C-D/E를 완전하게 괄호로 묶고 연산자를 이동시킬 장소를 표시하면 다음과 같이 된다.

![img](https://www.acmicpc.net/JudgeOnline/upload/201007/4.png)

이러한 사실을 알고 **중위 표기식이 주어졌을 때 후위 표기식으로 고치는 프로그램을 작성**하시오



### 입력

**첫째 줄에 중위 표기식**이 주어진다. 단 이 수식의 **피연산자는 A~Z의 문자로 이루어지며 수식에서 한 번씩만 등장**한다. 그리고 **-A+B와 같이 -가 가장 앞에 오거나 AB와 같이 *가 생략되는 등의 수식은 주어지지 않는다. 표기식은 알파벳 대문자와 +, -, *, /, (, )로만 이루어져 있으며, 길이는 100을 넘지 않는다.** 

```java
A+B*C*((D-E)*G)		// 중위 표기식
```



### 출력

첫째 줄에 후위 표기식으로 바뀐 식을 출력하시오

```java
ABC*DE-G**+		// 후위 표기식
```



### Stack

`Stack`을 이용하여 해결할 수 있을 것 같은 문제이지만, `Stack`에 어떤 조건으로 넣어야 하는지 난해할 수 있습니다. 일반적으로 생각할 수 있는 조건은 아래의 조건입니다.



#### 생각할 수 있는 조건

1. `*`를 `+`보다 먼저 처리합니다. (`*`의 우선순위가 더 높다는 의미입니다.)
2. 괄호 안에 표기식이 있으면 괄호 안 부터 처리합니다. (닫힌 괄호가 나오면 열린 괄호가 나올 때까지 검사)

위의 입력 예제 `A+B*C*((D-E)*G)`를 보면서 조건을 이용해 출력 결과 `ABC*DE-G**+`를 도출해보겠습니다. 이 때, 처리 완료된 표기식은 `[]`로 감쌉니다. 즉, 처리 완료된 것은 하나로 생각하는 것입니다.

1. `A+B*C*((D-E)*G) -> A+[BC*]*((D-E)*G)` (1번 조건 처리)
2. `A+[BC*]*((D-E)*G) -> A+[BC*]*([DE-]*G)` (2번 조건 처리, `A+[BC*]`보다 `[BC*]*((D-E)*G)`의 우선순위가 더 높기 때문)
3. `A+[BC*]*([DE-]*G) -> A+[BC*]*[DE-G*]` (2번 조건 처리)
4. `A+[BC*]*[DE-G*] -> A+[BC*DE-G**]` (1번 조건 처리)
5. `A+[BC*DE-G**] -> ABC*DE-G**+` (1번 조건 처리)

이런 방식으로 처리할 수 있다는 것은 알겠는데, 이 조건들을 `Stack`으로 어떻게 처리할 수 있을까요? 

자세히 살펴보면 **피연산자 즉 알파벳들은 연산자와 다르게 드라마틱하게 순서가 바뀌지 않는다는 것**을 알 수 있기 때문에 **알파벳들을 출력해주는 중간 중간에 알맞는 연산자를 출력해주면 되는 것**입니다.

또, `A+`가 처음으로 나왔지만 뒤의 `*` 즉, 우선순위가 높은 연산자가 다 처리되고 난 마지막에 처리되는 것을 볼 수 있습니다. 때문에 **연산자에 우선순위를 설정해 준 다음 Stack에 넣어놓은 후, 다음 연산자를 만났을 때 Stack에 위에 있는 연산자의 우선순위가 다음 연산자보다 크거나 같을 때(`*`가 `*`를 만났을 때) 출력해주면 됩니다.** 

마지막으로 괄호 처리입니다. 괄호는 닫는 괄호`)`가 나왔을 때 이제 까지 검사한 표기식에서 가장 가까운 열린 괄호`(`까지가 괄호로 묶인 것`()`이라고 볼 수 있습니다. **즉 Stack에 열린 괄호`(`를 넣어 놓고 닫힌 괄호`)`를 만나면 열린 괄호가 나올 때까지 Stack에서 빼면서 출력**해줍니다.



#### 최종 조건

아래와 같이 최종 조건을 정리해보겠습니다.

1. 연산자에 우선순위를 설정
2. Stack에 연산자와 괄호만 들어감 (이 때, 괄호도 Stack에 들어가기 때문에 우선순위 설정을 해주어야 함)
3. 알파벳들을 출력해주는 중간 중간에 알맞는 연산자를 출력
4. 표기식을 검사하다가 연산자를 만났을 때 Stack의 위에 있는 연산자의 우선순위가 만난 연산자보다 크거나 같을 때 Stack에서 뺀 후 출력
5. 닫는 괄호가 나왔을 때 이제 까지 검사한 표기식에서 가장 가까운 열린 괄호까지 위의 조건들을 이용해 출력

그렇다면 이 최종 조건들을 이용해 예제의 입력 `A+B*C*((D-E)*G)`에서 출력 결과 `ABC*DE-G**+`를 다시 도출해보겠습니다.

1. `A+B*C*((D-E)*G)` : 시작 (`우선순위 : () = 0, +- = 1, */ = 2 `)
   - `검사 :`
   - `Stack :`
   - `출력 :`
2. `B*C*((D-E)*G)`
   - `검사 : A+` 
   - `Stack : +`
   - `출력 : A`
3. `C*((D-E)*G)` : Stack의 위에 있는 연산자(`+`)의 우선순위가 만난 연산자(`*`) 보다 낮기 때문에 출력하지 않음
   - `검사 : B*` 
   - `Stack : +, *`
   - `출력 : AB`
4. `((D-E)*G)`  : Stack에 위에 있는 연산자(`*`)의 우선순위가 만난 연산자(`*`)와 같기 때문에 Stack에서 뺀 후 출력
   - `검사 : C*` 
   - `Stack : +, *`
   - `출력 : ABC*`
5. `D-E)*G)` : 괄호를 Stack에 넣음
   - `검사 : ((` 
   - `Stack : +, *, (, (`
   - `출력 : ABC*`
6. `E)*G)` : Stack에 위에 있는 연산자(`(`)의 우선순위가 만난 연산자(`-`) 보다 낮기 때문에 출력하지 않음
   - `검사 : D-` 
   - `Stack : +, *, (, (, -`
   - `출력 : ABC*D`
7. `*G) -> {E)}  [+,*,(] 출력 : ABC*DE-`  : 닫힌 괄호(`)`)를 만났기 때문에 열린 괄호(`(`)까지  Stack에서 뺀 후 출력 (괄호는 출력하지 않음)
   - `검사 : E)` 
   - `Stack : +, *, (`
   - `출력 : ABC*DE-`
8. `)`
   - `검사 : *G` 
   - `Stack : +, *, (, *`
   - `출력 : ABC*DE-G`
9. 닫힌 괄호(`)`)를 만났기 때문에 열린 괄호(`(`)까지  Stack에서 뺀 후 출력 (괄호는 출력하지 않음)
   - `검사 : )` 
   - `Stack : +, *`
   - `출력 : ABC*DE-G*`
10. Stack에 남아있는 연사자 모두 출력
    - `검사 :` 
    - `Stack :`
    - `출력 : ABC*DE-G**+`

위의 조건들을 사용하면 결과 `ABC*DE-G**+`가 제대로 나오는 것을 알 수 있습니다.



### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.HashMap;
import java.util.Map;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		StringBuilder sb = new StringBuilder();
		
		try {
			String str = br.readLine();
			
			ArrayDeque<Character> stack = new ArrayDeque<>();
			Map<Character, Integer> priority = new HashMap<>();
			
			// 우선순위 설정 (숫자가 높은 게 우선순위가 높은 것)
			priority.put('(', 0);
			priority.put('+', 1);
			priority.put('-', 1);
			priority.put('*', 2);
			priority.put('/', 2);
			
			for (int i = 0; i < str.length(); i++) {
				char ch = str.charAt(i);
				
				// 알파벳은 출력
				if(ch >= 'A' && ch <= 'Z')
					sb.append(ch);
				// 여는 괄호는 스택에 추가
				else if(ch == '(')
					stack.push(ch);
				// 닫는 괄호는 여는 괄호가 나올 때까지 출력
				else if(ch == ')') {
					while(!stack.isEmpty()) {
						if(stack.peekFirst() == '(') {
							stack.pop();
							break;
						}
						
						sb.append(stack.pop());
					}
				} else { // +, -, *, / 
					// (스택에서 제일 위에 있는 값의 우선순위) >= (현재 문자의 우선순위) 이어야만 출력
					// +,- 보다 *,/가 먼저 오기 때문에 
					while(!stack.isEmpty() && priority.get(stack.peekFirst()) >= priority.get(ch))
						sb.append(stack.pop());
					stack.push(ch);
				}
			}
			
			// 연산자가 남아 있으면 모두 출력
			while(!stack.isEmpty())
				sb.append(stack.pop());
		
			System.out.println(sb.toString());
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

