---
title: "Java : Scanner.useDelimeter"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Scanner.useDelimeter"
toc: true
toc_sticky: true
toc_label: 목차
---

# Scanner.useDelimeter

Scanner의 method로 Scanner의 구분 패턴을 지정된 문자열로 설정합니다. 문자를 입력 받을 때 특정한 문자열로 구분지어 분리할 때 유용하게 사용할 수 있습니다.

`useDelimeter`를 더 이상 사용하고 싶지 않을 경우 `reset` method로 scanner구분 패턴을 디폴트(공백)로 초기화 시킬 수 있습니다.



## 예제

[codeup](https://codeup.kr/) 의 기초 100제 1018번 문제입니다.

 `3:16` 방식으로 시간을 입력하면 정수형으로 `3:16` 똑같이 출력하는 문제입니다.

```java
import java.util.Scanner;

class Main {  
  public static void main(String args[]) { 
      Scanner sc = new Scanner(System.in);
      // 값을 입력하고
      
      sc = new Scanner(sc.next()).useDelimiter(":");
      // '':'를 구분 패턴으로 설정해 입력했던 값을 가져와서 나누어 줍니다.
      int a = sc.nextInt();
      int b = sc.nextInt();
      // 나눈 값들을 Int형으로 가져옵니다.
      
      System.out.printf("%d:%d",a,b);
      sc.close();
  } 
}
```

useDelimiter를 이용해 구분 패턴을 `:`로 설정해 `next()`를 통해 입력받은 값을 가져와  `:`를 기준으로 토큰을 나눌 수 있습니다. 그 다음, `nextInt()`를 이용해 나누어진 토큰을 가져올 수 있습니다.  

---

참고 : https://docs.oracle.com/javase/7/docs/api/java/util/Scanner.html#reset()

