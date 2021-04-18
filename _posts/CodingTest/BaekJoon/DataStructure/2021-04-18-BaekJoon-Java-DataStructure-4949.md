---
title: "BaekJoon : 4949번(균형잡힌 세상)"
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



## 4949

세계는 균형이 잘 잡혀있어야 한다. 양과 음, 빛과 어둠 그리고 왼쪽 괄호와 오른쪽 괄호처럼 말이다.

정민이의 임무는 어떤 문자열이 주어졌을 때, 괄호들의 균형이 잘 맞춰져 있는지 판단하는 프로그램을 짜는 것이다.

문자열에 포함되는 **괄호는 소괄호("()") 와 대괄호("[]")로 2종류**이고, 문자열이 균형을 이루는 조건은 아래와 같다.

- 모든 왼쪽 소괄호("(")는 오른쪽 소괄호(")")와만 짝을 이뤄야 한다.
- 모든 왼쪽 대괄호("[")는 오른쪽 대괄호("]")와만 짝을 이뤄야 한다.
- 모든 오른쪽 괄호들은 자신과 짝을 이룰 수 있는 왼쪽 괄호가 존재한다.
- 모든 괄호들의 짝은 1:1 매칭만 가능하다. 즉, 괄호 하나가 둘 이상의 괄호와 짝지어지지 않는다.
- 짝을 이루는 두 괄호가 있을 때, 그 사이에 있는 문자열도 균형이 잡혀야 한다.

정민이를 도와 문자열이 주어졌을 때 균형잡힌 문자열인지 아닌지를 판단해보자.



### 입력

하나 또는 여러줄에 걸쳐서 문자열이 주어진다. 각 **문자열은 영문 알파벳, 공백, 소괄호("( )") 대괄호("[ ]")등으로 이루어져 있으며, 길이는 100글자보다 작거나 같다.**

**입력의 종료조건으로 맨 마지막에 점 하나(".")**가 들어온다.

```java
So when I die (the [first] I will see in (heaven) is a score list).
[ first in ] ( first out ).
Half Moon tonight (At least it is better than no Moon at all].	// 이렇게 소괄호, 대괄호 짝이 맞지 않으면 안됩니다.
A rope may form )( a trail in a maze.
Help( I[m being held prisoner in a fortune cookie factory)].	// 이렇게 소괄호, 대괄호 짝이 맞지 않으면 안됩니다.
([ (([( [ ] ) ( ) (( ))] )) ]).
 .	// 괄호가 하나도 없는 경우도 균형잡힌 문자열로 간주합니다.
.	// '.'만 입력되었을 때 종료합니다. (출력에 포함 X)
```



### 출력

각 줄마다 해당 문자열이 **균형을 이루고 있으면 "yes"를, 아니면 "no"를 출력**한다.

```java
yes
yes
no
no
no
yes
yes
```



### Stack

푸는 방법을 모르면 매우 복잡한 문제일 수 있지만, 방법만 알면 생각보다 쉬운 문제입니다.

5번 째 입력 `Help( I[m being held prisoner in a fortune cookie factory)]`를 보면, 문자열을 체크할 때, **오른쪽 소괄호or대괄호(이하 오른쪽)를 만나면 마지막으로 온 왼쪽 소괄호or대괄호(이하 왼쪽)를 체크**하게 되기 때문에 `Stack`에 **왼쪽을 저장하고 오른쪽을 만나면 pop해주는 방식**을 사용하여 문제를 해결할 수 있습니다.

이 때, `Stack`을 이용할 때 짝이 맞지 않는 3가지의 경우가 있습니다.

1. 오른쪽을 만났을 때, `Stack`의 맨 위에 짝이 맞지 않는 괄호가 있는 경우
2. 오른쪽을 만났을 때, `Stack`에 데이터가 없는 경우(짝을 이룰 왼쪽이 없다는 말)
3. 문자열을 전부 체크 했을 때, `Stack`에 데이터가 남아 있는 경우(남은 데이터의 짝이 없다는 말)

이 3가지의 조건을 이용하면 문제를 해결할 수 있습니다.



### 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Stack;

public class Main {
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		StringBuilder sb = new StringBuilder();
		
		try {
			while(true) {
				String str = br.readLine();
				
				// 문자열에 '.' 하나만 있으면 반복문을 종료
				if(str.equals("."))
					break;
				
				Stack<Character> stack = new Stack<>();
				
				// 현재 문자열이 균형을 이룰 수 있는지 없는지 체크할 변수
				boolean isPossible = true;
				
				// 문자열 길이 만큼 하나씩 문자를 체크
				for (int i = 0; i < str.length()-1; i++) {
					char ch = str.charAt(i);
					
					// 왼쪽이면 stack에 저장
					if(ch == '(' || ch == '[')
						stack.add(ch);
					// 오른쪽이 ')'일 때
					else if(ch == ')') {
						// stack이 비어 있으면, 즉 짝을 이룰 왼쪽이 없으면 체크 변수를 false로 만들고 문자열 체크 종료(2번)
						if(stack.empty()) {
							isPossible = false;
							break;
						}
						// stack에 데이터가 있으면 짝이 맞는지 체크, 짝이 맞지 않으면 체크 변수를 false로 만들고 문자열 체크 종료(1번)
						if(stack.peek() == '(')
							stack.pop();
						else {
							isPossible = false;
							break;
						}
					}
					// 오른쪽이 ']'일 때, 나머지는 오른쪽이 ')'일 때와 동일
					else if(ch == ']') {
						if(stack.empty()) {
							isPossible = false;
							break;
						}
						
						if(stack.peek() == '[')
							stack.pop();
						else {
							isPossible = false;
							break;
						}
					}
						
				}
				
				// 문자열을 체크가 끝났는데, stack에 데이터가 남아 있는 경우 (3번)
				if(!stack.empty())
					isPossible = false;
				
				if(isPossible)
					sb.append("yes\n");
				else
					sb.append("no\n");
			}
			
			System.out.println(sb.toString());
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}


```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

