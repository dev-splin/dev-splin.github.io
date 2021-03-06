---
title: "Codeup : 기초 100제(1001 ~ 1010)"
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

# Java : codeup 기초 100제 (1001 ~ 1010)

codeup 기초 100제 저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 1001

printf()를 이용해 다음 단어를 출력하시오.

```java
Hello
```



### 내코드

```java
class Main {  
  public static void main(String args[]) { 
    System.out.println("Hello"); 
  } 
}
```



## 1002

이번에는 공백()을 포함한 문장을 출력한다.
다음 문장을 출력해보자.

```java
Hello World
// 대소문자에 주의한다.
```



### 내코드

```java
class Main {  
  public static void main(String args[]) { 
    System.out.println("Hello World"); 
  } 
}
```



## 1003

이번에는 줄을 바꿔 출력하는 출력문을 연습해보자.
다음과 같이 줄을 바꿔 출력해야 한다.

```java
Hello
World
// (두 줄에 걸쳐 줄을 바꿔 출력)
```



### 내코드

```java
class Main {  
  public static void main(String args[]) { 
    System.out.println("Hello\nWorld"); 
  } 
}
```



## 1004

이번에는 작은 따옴표(single quotation mark)가 들어있는
특수한 형태의 출력문에 대한 연습을 해보자.

다음 문장을 출력하시오.

```java
'Hello'
```



### 내코드

```java
class Main {  
  public static void main(String args[]) { 
    System.out.println("\'Hello\'"); 
    // java에서 \ 나 " 같은 특수문자를 출력문에 포함시키고 싶을땐 \(역슬래시)를 앞에 넣어주면 됩니다!
  } 
}
```



## 1005

이번에는 큰따옴표(double quotation mark)가 포함된 출력문을 연습해보자.

다음 문장을 출력하시오.

```java
"Hello World"
// (단, 큰따옴표도 함께 출력한다.)
```



### 내코드

```java
class Main {  
  public static void main(String args[]) { 
    System.out.println("\"Hello World\""); 
  } 
}
```



## 1006

이번에는 특수문자 출력에 도전하자!!

다음 문장을 출력하시오.

```java
"!@#$%^&*()"
// (단, 큰따옴표도 함께 출력한다.)
```



### 내코드

```java
class Main {  
  public static void main(String args[]) { 
    System.out.println("\"!@#$%^&*()\""); 
  } 
}
```



## 1007

윈도우 운영체제의 파일 경로를 출력하는 연습을 해보자.

파일 경로에는 특수문자들이 포함된다.

다음 경로를 출력하시오.

```java
"C:\Download\hello.cpp"
// (단, 큰따옴표도 함께 출력한다.)
```



### 내코드

```java
class Main {  
  public static void main(String args[]) { 
    System.out.println("\"C:\\Download\\hello.cpp\""); 
  } 
}
```



## 1008

이번에는 특수문자를 출력하는 연습을 해보자.

키보드로 입력할 수 없는 다음 모양을 출력해보자.
(** 참고 : 운영체제의 문자 시스템에 따라 아래와 같은 모양이 출력되지 않을 수 있다.)

```java
┌┬┐
├┼┤
└┴┘
```



### 내코드

```java
class Main {  
  public static void main(String args[]) { 
    System.out.println("\u250C\u252c\u2510"); 
    System.out.println("\u251C\u253C\u2524");
    System.out.println("\u2514\u2534\u2518");
  } 
}
```

유니코드로 특수문자를 표한하는 방법입니다. 관련된 유니코드 표는 [codeup](https://www.codeup.kr/problem.php?id=1008)에 있습니다.



## 1010

정수형(int)으로 변수를 선언하고, 변수에 정수값을 저장한 후
변수에 저장되어 있는 값을 그대로 출력해보자.

```java
15 // 15라고 입력하면
15 // 15가 출력됩니다.
```



### 내코드

#### Scanner를 이용한 방법

```java
import java.util.Scanner;

class Main {  
  public static void main(String args[]) { 
   
    Scanner sc = new Scanner(System.in);
    
     int n = sc.nextInt();
     
     System.out.println(n);
    
  } 
}
```



#### BufferedReader를 이용한 방법

```java
import java.io.*;
 
public class prac {
    public static void main(String[] args){
        InputStreamReader isr = new InputStreamReader(System.in);
        BufferedReader br = new BufferedReader(isr);
 
        OutputStreamWriter osw = new OutputStreamWriter(System.out);
        BufferedWriter bw = new BufferedWriter(osw);
 
        try {
            String str = br.readLine();     //입력
            
            int num = Integer.parseInt(str);
 
            bw.write(num);                  //출력
            bw.flush();						// 버퍼를 비워줍니다.
            bw.close();
 
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

[Scanner와 BufferedReader에 대한 자세한 설명](https://dev-splin.github.io/java/Java-Scanner,BufferedReader/)

