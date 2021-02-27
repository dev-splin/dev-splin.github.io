---
title: "Java : Scanner와 BufferedReader"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Scanner"
  - "Java : BufferedReader"
toc: true
toc_sticky: true
toc_label: 목차

---

# Scanner와 BufferedReader/Writer



## Scanner

- 공란과 줄바꿈을 모두 입력값의 경계로 인식하기 때문에 쉽게 데이터를 입력받을 수 있도록 해줍니다.
- 데이터 타입이 입력받는 시점에서 결정되므로 별도의 Casting이 필요하지 않습니다.



```java
import java.util.Scanner;

public class parc {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
       	int a = sc.nextInt();
        float b = sc.nextFloat();
        String str = sc.next();
        String strWithSpace = sc.nextLine();        //공백을 포함
 		// 다양한 데이터 타입으로 입력이 가능합니다.
        
        sc.close();
 
        System.out.println(a);
        System.out.println(b);
        System.out.println(str);
        System.out.println(strWithSpace);
    }
}
```



## BufferedReader/Writer

- InputStreamReader는 문자열을 Character단위(한 글자 단위)로 읽어 들이는데, 긴 문자열을 읽을 때 불편하고 비효율적입니다. 이 점을 보완한 것이 BufferedReader 입니다.
- 사용자가 요청할 때 마다 데이터를 읽어 오는 것이 아니라 일정한 크기의 데이터를 한번에 읽어와 버퍼에 보관 후, 사용자의 요청이 있을 때 버퍼에서 데이터를 읽어오는 방식으로 동작합니다.
  - 속도가 향상되고 시간 부하가 적습니다.
- 라인단위로 입력 받기 때문에 공백의 경우에도 String으로 인식하여 받아들입니다.
- 입력,출력받은 데이터 타입이 String 타입이므로 다른 타입의 데이터라면 형변환이 필요합니다.
- try / catch(예외처리)를 해 주어야 합니다.



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
 
            bw.write(str);                  //출력
            bw.flush();						// 버퍼를 비워줍니다.
            bw.close();
 
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```





## Scanner vs BufferedReader

|                 |  BufferedReader  |     Scanner      |
| :-------------: | :--------------: | :--------------: |
| **Buffer Size** |       8192       |       1024       |
| **Syncronized** |        O         |        X         |
|   **Parsing**   | 단순히 읽어 들임 | 문자열 파싱 가능 |
|  **Exception**  | IOException 던짐 | IOException 숨김 |

- Scanner는 문자열 파싱 기능이 제공되는 반면 BufferedReader는 단순히 읽어 들이기 때문에 상대적으로 BufferedReader가 더 빠릅니다.
- Scanner는 정규표현식(Regex)을 많이 사용하여 알아서 Tokenizing과 Parsing을 해주기 때문에 패턴을 분석하는데 있어서 시간을 많이 잡아먹습니다.
  - 정규표현식을 이용하면 패턴을 이용해 문자열을 좀 더 세밀히 분석할 수 있습니다.



---

출처 : https://carpediem0212.tistory.com/11

https://ohmycode9328.tistory.com/10