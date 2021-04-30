---
title: "Java : 정규 표현식(Regular Expression(Pattern, Matcher))"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Regular Expression"
  - "Java : Pattern / Matcher"
toc: true
toc_sticky: true
toc_label: 목차
---

# Regular Expression

`정규표현식(Regular Expression)`이란 컴퓨터 과학의 정규언어로부터 유래한 것으로 **특정한 규칙을 가진 문자열의 집합을 표현하기 위해 쓰이는 형식언어** 입니다. 개발을 하다보면 **전화번호, 주민등록번호, 이메일등과 같이 정해져있는 형식이 있고 사용자가 그 형식대로 제대로 입력했는지 검증을 해야하는 경우**가 종종 있습니다. 이런 **입력값을 정해진 형식에 맞는지 검증해야 할 때에는 정규표현식을 사용하면 쉽게 구현**할 수 있습니다.



## 정규표현식 작성 방법

정규 표현식을 사용하기 위해서는 자바 API인 `java.util.regex` 패키지를 사용해야 합니다. 자바에서 정규표현식을 사용할때에는 `java.util.regex` 패키지 안에 있는 `Pattern`클래스와 `Matcher`클래스를 주로 사용합니다. 또 `String` 클래스의 `replaceAll()` 함수는 정규표현식을 이용해 정규표현식에 해당하는 문자를 변경할 수 있습니다.



### 정규표현식 문법

정규표현식 문법입니다. [정규표현식 테스트](https://regexr.com/5mhou)에서 손 쉽게 테스트해 볼 수 있습니다.

| **정규 표현식** | **설명**                                                     | 예시(간단한 건 생략)                                         |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ^               | 문자열 시작                                                  | `^[e]` : e로 시작하는 문자                                   |
| $               | 문자열 종료                                                  | `[e]$` : e로 끝나는 문자                                     |
| .               | 임의의 한 문자(단 \은 넣을 수 없음)                          | `.[0-9]` : 임의의 한 문자 + 숫자(총 2글자)로 이루어진 문자   |
| *               | 앞 문자가 없을 수도 무한정 많을 수도 있음                    | `a*[0-9]` : 앞에 a가 붙거나 붙지 않는 숫자로 이루어진 문자 (a1, 3, aaa4, ...) |
| +               | 앞 문자가 하나 이상                                          | `a+[0-9]` : 숫자 앞에 a가 하나 이상으로 이루어진 문자 (a1, aa2, aaa4, ...) |
| ?               | 앞 문자가 없거나 하나 있음                                   | `a?[0-9]` : 숫자 앞에 a가 하나 있거나, 없는 문자 (a1, 2, 3, a9, ...) |
| [ ]             | []사이에 있는 문자의 집합이나 범위를 나타내며 두 문자 사이는 - 기호로 범위를 나타냅니다. | `[a-zA-Z0-9]` : 대문자 or 소문자 or 숫자로 이루어진 문자 1글자 (a, 1, A, ...) |
| [^]             | []사이에 있는 임의의 한 문자 또는 문자집합을 제외            | `[^a-z0-9]` : 소문자와 숫자를 제외한 문자 1글자(A, #, @, ...) |
| { }             | 횟수 또는 범위를 나타냅니다.                                 | `[a]{n}` : a가 n번 반복되는 문자<br>`[a]{min,}` : a가 min 이상 반복되는 문자<br>`[a]{min, max}` : a가 min이상 max이하 반복되는 문자 |
| ( )             | 소괄호 안의 문자를 하나의 문자로 인식(그룹). 괄호로 묶어진 문자 순서대로 그룹 번호가 매겨짐 | `(abc).` : abc와 임의의 한 문자로 이루어진 문자 (abcd, abc3, acb# ...) |
| (?:)            | 소괄호 안의 문자를 하나의 문자로 인식하지만 그룹으로 관리되지 않음. 그룹 번호 X | -                                                            |
| \|              | 패턴 안에서 or 연산을 수행할 때 사용                         | `^[a]|[a]$` : a로 시작하거나 a로 끝나는 문자                 |
| \               | 정규 표현식 역슬래시(\)는 확장문자 (역슬래시 다음에 일반 문자가 오면 특수문자로 취급하고 역슬래시 다음에 특수문자가 오면 그 문자 자체를 의미) | `\b` 같은 경우는 단어의 경계, `\.`(온점) 같은 경우는 `.(온점)` 자체를 의미합니다. (자바의 경우에는`\\b`, `\\.`) |
| \b              | 단어의 경계(문자의 시작, 끝이나 공백)                        | `a\b` : 단어의 경계 전에 있는 a (asd aqa df 에서 aqa 중 뒤에 있는 a)<br>`\ba` : 단어의 경계 후에 있는 a (asd의 a, aqa의 앞에 있는 a) |
| \B              | 단어의 경계 아닌 것                                          | `a\B` : 단어의 경계가 아닌 것 전에 있는 a (asd aqa df 에서 asd의 a, aqa의 앞에 있는 a)<br/>`\Ba` : 단어의 경계가 아닌 것 후에 있는 a (aqa의 뒤에 있는 a) |
| \A              | 입력의 시작 부분                                             | -                                                            |
| \G              | 이전 매치의 끝                                               | -                                                            |
| \Z              | 입력의 끝이지만 종결자가 있는 경우                           | -                                                            |
| \z              | 입력의 끝                                                    | -                                                            |
| \s              | 공백 문자                                                    | -                                                            |
| \S              | 공백 문자가 아닌 나머지 문자                                 | -                                                            |
| \w              | 알파벳이나 숫자                                              | -                                                            |
| \W              | 알파벳이나 숫자를 제외한 문자(공백 or 특수문자)              | -                                                            |
| \d              | 숫자 [0-9]와 동일                                            | -                                                            |
| \D              | 숫자를 제외한 모든 문자 (공백 or 특수문자 포함)              | -                                                            |
| (?i)            | 앞 부분에 (?!)라는 옵션을 넣어주게 되면 대소문자는 구분하지 않습니다. | -                                                            |



### 자주 사용하는 정규 표현식

자주 사용하는 정규 표현식을 살펴보면서 분석 해보겠습니다. (자바 기준)

| **정규 표현식**                              | **설명**     |
| -------------------------------------------- | ------------ |
| `^[0-9]*$`                                   | 숫자         |
| `^[a-zA-Z]*$`                                | 영문자       |
| `^[가-힣]*$`                                 | 한글         |
| `\\w+@\\w+\\.\\w+(\\.\\w+)?`                 | E-Mail       |
| `^\\d{2,3}-\\d{3,4}-\\d{4}$`                 | 전화번호     |
| `^01(?:0|1|[6-9])-(?:\\d{3}|\\d{4})-\\d{4}$` | 휴대전화번호 |
| `\\d{6}-[1-4]\\d{6}`                         | 주민등록번호 |
| `^\\d{3}-\\d{2}$`                            | 우편번호     |

- `^[0-9]*$` : 문자열의 시작(`^`)과 끝(`$`) 사이에 숫자(`[0-9]`)가 없거나 많은(`*`) 문자 즉, **숫자로만 이루어진 문자를 의미**합니다.

- `^[a-zA-Z]*$` : 문자열의 시작(`^`)과 끝(`$`) 사이에 대소문자 알파벳(`[a-zA-z]`)이 없거나 많은(`*`) 문자 즉, **대소문자 알파벳으로만 이루어진 문자를 의미**합니다.

- `^[가-힣]*$` : 문자열의 시작(`^`)과 끝(`$`) 사이에 한글(`[가-힣]`)이 없거나 많은(`*`) 문자 즉, **한글로만 이루어진 문자를 의미**합니다.

- `\\w+@\\w+\\.\\w+(\\.\\w+)?` : 조금 복잡하기 때문에 나누어서 설명하겠습니다. 문자열 `splin@gmail.com`으로 예를 들면,
  - `\\w+@` : 알파벳이나 숫자(`\\w`)가 하나 이상으로(`+`) 이루어진 문자가 `@`앞에 오고 (`splin@`)
  - `\\w+\\.` : 알파벳이나 숫자(`\\w`)가 하나 이상으로(`+`) 이루어진 문자가 온점(`\\.`) 앞에 옵니다. (`gmail.`)
  - `\\w+` : 마지막으로 알파벳이나 숫자(`\\w`)가 하나 이상으로(`+`) 이루어진 문자가 오는데(`com`),
  - `(\\.\\w+)?` : 온점(`\\.`) 뒤에 알파벳이나 숫자(`\\w`)가 하나 이상으로(`+`) 이루어진 문자(`\\.`과 `\\w`를 한 문자`()`로 인식)가 있을 수도, 없을 수도(`?`) 있습니다. (이메일 주소가 co.kr 로 이루어진 형식인 경우)
  - 즉, **이메일 주소 형식을 의미**합니다.

- `^01(?:0|1|[6-9])-(?:\\d{3}|\\d{4})-\\d{4}$` : 이것도 복잡하기 때문에 나누어서 설명하겠습니다.
  - 문자열의 시작(`^`)과 끝(`$`) 사이에
  - `01(?:0|1|[6-9])-` : `010, 011, 016, 017, 018, 019 중 1개(0|1|[6-9)`가 오고난 후 `-`을 붙입니다. (이 때, 그룹으로 관리하지 않습니다(`?:`))
  - `(?:\\d{3}|\\d{4})-` : 숫자가 연속으로 3개나(`\\d{3}|`) 4개(`\\d{4}`)가 오고난 후 `-`을 붙입니다. (그룹으로 관리하지 않습니다.(`?:`))
  - `\\d{4}` : 숫자가 연속으로 4개가 옵니다.
  - 즉, **휴대전화번호 형식을 의미**합니다.

주민등록번호나 우편번호는 비슷하기 때문에 생략하겠습니다.



### Pattern 클래스

정규 표현식에 대상 문자열을 검증하는 기능은 `java.util.rege.Pattern` 클래스의 `matches()`메소드를 활용하여 검증할 수 있습니다. **matches() 메서드의 첫번째 매개값은 정규표현식이고 두번째 매개값은 검증 대상 문자열**입니다. 검증 후 **대상문자열이 정규표현식과 일치하면 true, 그렇지 않다면 false값을 리턴**합니다.  **String 에서도 matches를 사용할 수 있지만, Pattern 클래스를 이용하는 것이 재사용성이 좋습니다.**

`Pattern` 객체들은 `비상태 유지 객체`들이기 때문에 여러 개의 `Matcher` 객체들이 공유할 수 있습니다.

```java
import java.util.regex.Pattern;

public class RegexExample {
	public static void main(String[] args)  {
    
            String pattern = "^[0-9]*$"; //숫자만
            String val = "123456789"; //대상문자열
        
            boolean regex = Pattern.matches(pattern, val);
            System.out.println(regex);
    }
}
```

위 예제는 `Pattern`클래스의 `matches()` 메서드를 활용하여 대상문자열이 숫자인지 아닌지 검증하는 예제입니다. **대상문자열이 숫자가 맞다면 ture 그렇지 않다면 false가 출력**됩니다.



#### Pattern 클래스 주요 메서드

- `compile(String regex)` : 주어진 정규표현식으로부터 패턴을 만듭니다. (Static 함수)
- `matcher(CharSequence input)` :  `CharSequence`에서 패턴을 찾는 `Matcher` 객체를 만듭니다. (Static 함수)
- `matches(String regex, CharSequence input)` : 주어진 정규식으로 컴파일하고, 주어진 문자열이 규칙에 부합되는지 여부를 확인합니다.
- `asPredicate()` : 문자열을 일치시키는 데 사용할 수있는 술어를 작성합니다.
- `pattern()` : 컴파일된 정규표현식을 String 형태로 반환합니다.
- `split(CharSequence input)` : 문자열을 주어진 인자값 `CharSequence` 패턴에 따라 분리합니다.



#### Pattern 플래그 값 사용(상수)

- `Pattern.CANON_EQ` : None표준화된 매칭 모드를 활성화합니다.
- `Pattern.CASE_INSENSITIVE` : 대소문자를 구분하지 않습니다. 
- `Pattern.COMMENTS` : 공백과 #으로 시작하는 주석이 무시됩니다. (라인의 끝까지).
- `Pattern.MULTILINE` : 수식 ‘^’ 는 라인의 시작과,  '$'는 라인의 끝과 match 됩니다.
- `Pattern.DOTALL` : 수식 ‘.’과 모든 문자와 match 되고 ‘\n’ 도 match 에 포함됩니다.
- `Pattern.UNICODE_CASE` : 유니코드를 기준으로 대소문자 구분 없이 match 시킵니다.
- `Pattert.UNIX_LINES` : 수식 ‘.’ 과 ‘^’ 및 ‘$’의 match시에 한 라인의 끝을 의미하는 ‘\n’만 인식됩니다.



### Matcher 클래스

**Matcher 클래스는 대상 문자열의 패턴을 해석하고 주어진 패턴과 일치하는지 판별할 때 주로 사용**됩니다.  **Matcher객체는 Pattern객체의 matcher() 메소드를 호출하여 받아올 수 있습니다.** 

`Matcher` 클래스의 입력값으로는 `CharSequence`라는 새로운 인터페이스가 사용되는데 이를 통해 다양한 형태의 입력 데이터로부터 문자 단위의 매칭 기능을 지원 받을 수 있습니다. 기본적으로 제공되는 `CharSequence` 객체들은 `CharBuffer, String, StringBuffer` 클래스가 있습니다.

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexExample {
	public static void main(String[] args)  {
            Pattern pattern = Pattern.compile("^[a-zA-Z]*$"); //영문자만
            String val = "abcdef"; //대상문자열
	
            Matcher matcher = pattern.matcher(val);
            System.out.println(matcher.find());
	}
}
```

위 예제는 **Matcher 클래스의 find() 메서드를 활용하여 대상 문자열이 영문자인지 검증하는 예제**입니다. **대상 문자열의 특정 부분이 영문자가 맞다면 ture를 return 하고 그 위치로 이동하고, 그렇지 않다면 false가 출력**됩니다. 



#### Matcher 클래스 주요 메서드

- `matches()` : 대상 문자열 전체와 패턴이 일치할 경우 true 반환합니다.
- `lookingAt()` : 주어진 문자열이 특정 패턴으로 시작하는가를 판단합니다.
- `find()` : 대상 문자열에서 패턴이 일치하는 경우 true를 반환하고, 그 위치로 이동합니다.
- `find(int start)` : start위치 이후부터 매칭검색을 수행합니다.
- `replaceAll(String replacement)`  : 패턴과 일치되는 부분을 replacement로 대체합니다.
- `replaceFirst(String replacement)`  :  패턴과 일치되는 첫 번째 문자열을 replacement로 대체합니다.
- `start()` : 매칭되는 문자열 시작위치 반환합니다.
- `start(int group)` : 지정된 그룹이 매칭되는 시작위치 반환합니다.
- `end()` : 매칭되는 문자열 끝 다음 문자위치 반환합니다.
- `end(int group)` : 지정된 그룹이 매칭되는 끝 다음 문자위치 반환합니다.
- `group()` : 매칭된 부분을 반환합니다.
- `group(int group)` : 매칭된 부분중 group번 그룹핑 매칭부분 반환합니다. 
- `groupCount()` : 패턴내 그룹핑한(괄호지정) 전체 갯수를 반환합니다.
- `appendReplacement(StringBuffer sb, String replacement) ` : 일치되는 패턴이 나타날 때까지 모든 문자들을 버퍼(StringBuffer)로 옮기고, 패턴에 일치되는 문자열은 두 번째 파라미터인 replacement 문자열로 대체합니다.



### 유효성 검사

정규 표현식은 유효성 검사 코드 작성 시 가장 효율적인 방법입니다.

```java
import java.util.regex.Pattern;

public class RegexExample {
	public static void main(String[] args)  {
          String name = "홍길동";
          String tel = "010-1234-5678";
          String email = "test@naver.com";
         
          //유효성 검사
          boolean name_check = Pattern.matches("^[가-힣]*$", name);
          boolean tel_check = Pattern.matches("^01(?:0|1|[6-9])-(?:\\d{3}|\\d{4})-\\d{4}$", tel);
          boolean email_check = Pattern.matches("\\w+@\\w+\\.\\w+(\\.\\w+)?", email);

          //출력
          System.out.println("이름 : " + name_check);
          System.out.println("전화번호 : " + tel_check);
          System.out.println("이메일 : " + email_check);
    }
}
```



### String의 replaceAll()

**String의 replaceAll() 함수는 첫 번째 인자로 정규표현식을 받습니다.** 그렇기 때문에 **문자열의 특정 문자들을 입맛에 맞게 바꿀 수 있습니다.**

```java
import java.util.regex.Pattern;

public class Example {
	public static void main(String[] args)  {
          String str = "$#%asdqf.#$32v4sad15..";
         
          // 특수문자 삭제
          String result1 = str.replaceAll("\\W","");
          // 온점 삭제
          String result2 = str.replaceAll("[.]","");
          // 숫자 삭제    
          String result3 = str.replaceAll("\\d","");

          //출력
          System.out.println("특수문자 삭제 : " + result1);
          System.out.println("온점 삭제 : " + result2);
          System.out.println("숫자 삭제 : " + result3);
    }
}
```

위 예제에서 정규표현식에 따로 개수를 정해주지 않은 이유는 **replaceAll() 함수는 문자 하나 하나 마다 정규표현식을 적용**해 주기 때문입니다.



---

참고 : [https://www.youtube.com/watch?v=t3M6toIflyQ&t=183s](https://www.youtube.com/watch?v=t3M6toIflyQ&t=183s)

[https://coding-factory.tistory.com/529](https://coding-factory.tistory.com/529)

[https://jamesdreaming.tistory.com/179](https://jamesdreaming.tistory.com/179)

[https://ktko.tistory.com/entry/JAVA-%EC%9E%90%EB%B0%94%EC%9D%98-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D-Pattern-Matcher](https://ktko.tistory.com/entry/JAVA-%EC%9E%90%EB%B0%94%EC%9D%98-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D-Pattern-Matcher)