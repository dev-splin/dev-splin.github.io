---
title: "Programmers : 2021 Kakao Blind 1번(신규 아이디 추천)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - Programmers
tags:
  - "Coding Test"
  - Programmers
  - "Programmers : Regular Expression"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : Programmers Regular Expression

Programmers **Regular Expression(정규표현식)** 저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 신규 아이디 추천

카카오에 입사한 신입 개발자 `네오`는 "카카오계정개발팀"에 배치되어, 카카오 서비스에 가입하는 유저들의 아이디를 생성하는 업무를 담당하게 되었습니다. "네오"에게 주어진 첫 업무는 새로 가입하는 유저들이 카카오 아이디 규칙에 맞지 않는 아이디를 입력했을 때, 입력된 아이디와 유사하면서 규칙에 맞는 아이디를 추천해주는 프로그램을 개발하는 것입니다.
다음은 카카오 아이디의 규칙입니다.

- **아이디의 길이는 3자 이상 15자 이하여야 합니다.**
- **아이디는 알파벳 소문자, 숫자, 빼기(`-`), 밑줄(`_`), 마침표(`.`) 문자만 사용할 수 있습니다.**
- **단, 마침표(`.`)는 처음과 끝에 사용할 수 없으며 또한 연속으로 사용할 수 없습니다.**

"네오"는 다음과 같이 **7단계의 순차적인 처리 과정을 통해 신규 유저가 입력한 아이디가 카카오 아이디 규칙에 맞는 지 검사하고 규칙에 맞지 않은 경우 규칙에 맞는 새로운 아이디를 추천**해 주려고 합니다.
신규 유저가 입력한 아이디가 `new_id` 라고 한다면,

```
1단계 new_id의 모든 대문자를 대응되는 소문자로 치환합니다.
2단계 new_id에서 알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.)를 제외한 모든 문자를 제거합니다.
3단계 new_id에서 마침표(.)가 2번 이상 연속된 부분을 하나의 마침표(.)로 치환합니다.
4단계 new_id에서 마침표(.)가 처음이나 끝에 위치한다면 제거합니다.
5단계 new_id가 빈 문자열이라면, new_id에 "a"를 대입합니다.
6단계 new_id의 길이가 16자 이상이면, new_id의 첫 15개의 문자를 제외한 나머지 문자들을 모두 제거합니다.
     만약 제거 후 마침표(.)가 new_id의 끝에 위치한다면 끝에 위치한 마침표(.) 문자를 제거합니다.
7단계 new_id의 길이가 2자 이하라면, new_id의 마지막 문자를 new_id의 길이가 3이 될 때까지 반복해서 끝에 붙입니다.
```

---

예를 들어, new_id 값이 `...!@BaT#*..y.abcdefghijklm` 라면, 위 7단계를 거치고 나면 new_id는 아래와 같이 변경됩니다.

1단계 대문자 'B'와 'T'가 소문자 'b'와 't'로 바뀌었습니다.
`"...!@BaT#*..y.abcdefghijklm"` → `"...!@bat#*..y.abcdefghijklm"`

2단계 '!', '@', '#', '*' 문자가 제거되었습니다.
`"...!@bat#*..y.abcdefghijklm"` → `"...bat..y.abcdefghijklm"`

3단계 '...'와 '..' 가 '.'로 바뀌었습니다.
`"...bat..y.abcdefghijklm"` → `".bat.y.abcdefghijklm"`

4단계 아이디의 처음에 위치한 '.'가 제거되었습니다.
`".bat.y.abcdefghijklm"` → `"bat.y.abcdefghijklm"`

5단계 아이디가 빈 문자열이 아니므로 변화가 없습니다.
`"bat.y.abcdefghijklm"` → `"bat.y.abcdefghijklm"`

6단계 아이디의 길이가 16자 이상이므로, 처음 15자를 제외한 나머지 문자들이 제거되었습니다.
`"bat.y.abcdefghijklm"` → `"bat.y.abcdefghi"`

7단계 아이디의 길이가 2자 이하가 아니므로 변화가 없습니다.
`"bat.y.abcdefghi"` → `"bat.y.abcdefghi"`

따라서 신규 유저가 입력한 new_id가 `...!@BaT#*..y.abcdefghijklm`일 때, 네오의 프로그램이 추천하는 새로운 아이디는 `bat.y.abcdefghi` 입니다.



### 문제

신규 유저가 입력한 아이디를 나타내는 new_id가 매개변수로 주어질 때, "네오"가 설계한 **7단계의 처리 과정을 거친 후의 추천 아이디를 return 하도록 solution 함수를 완성**해 주세요.



### 제한사항

- new_id는 길이 1 이상 1,000 이하인 문자열입니다.
- new_id는 알파벳 대문자, 알파벳 소문자, 숫자, 특수문자로 구성되어 있습니다.
- new_id에 나타날 수 있는 특수문자는 `-_.~!@#$%^&*()=+[{]}:?,<>/` 로 한정됩니다.



### 입출력 예

| no   | new_id                          | result              |
| ---- | ------------------------------- | ------------------- |
| 예1  | `"...!@BaT#*..y.abcdefghijklm"` | `"bat.y.abcdefghi"` |
| 예2  | `"z-+.^."`                      | `"z--"`             |
| 예3  | `"=.="`                         | `"aaa"`             |
| 예4  | `"123_.def"`                    | `"123_.def"`        |
| 예5  | `"abcdefghijklmn.p"`            | `"abcdefghijklmn"`  |



### 조건문으로 해결한 방법

String을 하나 하나 검사하면서 조건문으로 해결한 방법입니다.

```java
class Solution {
	    public String solution(String new_id) {
	    	// id의 대문자를 소문자로 변환
	    	String answer = new_id.toLowerCase();
	    	answer = convertId(answer);
	        return answer;
	    }
	    
	    // id를 변환할 함수
	    public String convertId(String Id) {
	    	String result = "";
	    	int count = 0;
	    	
	    	for (int i = 0; i < Id.length(); i++) {
	    		char ch = Id.charAt(i);
	    		
	    		// 허용된 문자들만 String에 더해줍니다.
	    		// 이 때, '.'은 따로 체크를 해주어서 '.'이 연속될 시 count를 누적 시키고
	    		// '.'체크가 끝나고 다음 문자열이 추가될 때, '.'하나만 추가해 줍니다. 
	    		// 마지막 까지 '.'이 있을 시 추가하지 않기 때문에 자동으로 마지막의 '.'이 제거 됩니다.
	    		if((ch >= 97 && ch <= 122) || (ch >= 48 && ch <= 57) ||
	    				ch == '-' || ch == '_') {
	    			if(count > 0) {
	    				result += '.';
	    				count = 0;
	    			}
	    			result += ch;
	    
	    		}
	    		else if(ch == '.') {
	    			++count;
	    		}
			}
	    	result = checkId(result);
	  
	    	return result;
	    }
	    
	    // Id의 길이와 '.'을 체크합니다.
	    public String checkId(String id) {
	    	String result = removePeriod(id);
	    	int length = result.length();
	  
	    	// 문자열 길이가 0 이라면 a를 추가
	    	if(length == 0) {
	    		result = "a";
	    		length = 1;
	    	}
	    	
	    	// 16자 이상이면 잘라주고 '.'제거
	    	if(length >= 16)
	    		result = removePeriod(result.substring(0,15));
	    	// 2자 이하면 마지막 문자를 3이 될 때까지 추가
	    	else if(length <= 2) {
	    		char lastCh = result.charAt(length-1);
	    		for (int i = length; i < 3; i++)
	    			result += lastCh;
	    	}
	    		
	    	return result;
	    }
	    
	    // 처음과 끝의 '.'을 제거합니다.
	    public String removePeriod(String id) {
	    	String result = "";
	    	int startIndex = 0;
	    	int endIndex = id.length();
	    	
	    	if(id.startsWith("."))
	    		++startIndex;
	    	if(id.endsWith("."))
	    		--endIndex;
	    	
	    	if(startIndex <= endIndex)
	    		result = id.substring(startIndex, endIndex);
	    	
	    	return result;
	    }
	}
```



### 정규표현식으로 해결한 방법

정규표현식을 사용하면 간단하게 해결할 수 있습니다.

[정규표현식이란?](https://dev-splin.github.io/java/Java-RegularExpression(Pattern,-Matcher)/)

```java
static class Solution {
	    public String solution(String new_id) {
	        String answer = "";
	        String temp = new_id.toLowerCase();
	        
	        // 허용된 문자를 제외한 나머지 문자들은 삭제
	        temp = temp.replaceAll("[^-_.a-z0-9]", "");
	        // '.'이 2개이상 연속되면 '.' 한 개로 치환
	        temp = temp.replaceAll("[.]{2,}", ".");
	        // 처음과 끝에 '.'이 있으면 제거
	        temp = temp.replaceAll("^[.]|[.]$", "");
	        // 길이가 0 이면 a를 추가
	        if(temp.length() == 0)
	        	temp = "a";
	        
	        // 길이가 16자 이상이면 자르고, 마지막 '.'이 있을 시 삭제
	        if(temp.length() >= 16) {
	        	temp = temp.substring(0,15);
	        	temp = temp.replaceAll("[.]$", "");
        	// 길이가 2 이하면 길이가 3이될 때까지 마지막 문자를 추가
	        } else if(temp.length() <= 2)
	        	while(temp.length() < 3)
	        		temp += temp.charAt(temp.length()-1);
	        
	        answer = temp;
	        
	        return answer;
	    }
	}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.

