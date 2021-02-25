---
title: "Java : StringTokenizer"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : StringTokenizer"
toc: true
toc_sticky: true
toc_label: 목차

---

# StringTokenizer

- 문자열을 우리가 지정한 구분자로 문자열을 쪼개주는 클래스입니다. 그렇게 쪼개어진 문자열을 토큰(token)이라고 부릅니다.
- 사용하기 위해서는 java.util.StringTokenizer를 import 해야합니다.



**생성자(Constructor)**

|                            생성자                            |                             설명                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                 StringTokenizer(String str);                 | 전달된 매개변수 str을 기본(default) delim으로 분리합니다. 기본 delimiter는 공백 문자들인 " \t\n\r\t"입니다. |
|          StringTokenizer(String str,String delim);           |             특정 delim으로 문자열을 분리합니다.              |
| StringTokenizer(String str,String delim,boolean returnDelims); | str을 특정 delim으로 분리시키는데 그 delim까지 token으로 포함할지를 결정합니다. 그 매개변수가 returnDelims로 true일시 포함, false일땐 포함하지 않습니다. |



**메소드(Method)**

|                       메소드                       |                             설명                             |
| :------------------------------------------------: | :----------------------------------------------------------: |
|                 int countTokens()                  | 남아있는 token의 개수를 반환합니다. 전체 token의 갯수가 아닌 현재 남아있는 token 개수입니다. |
| boolean hasMoreElements(), boolean hasMoreTokens() | 다음의 token을 반환합니다. 토큰이 더 있으면 true 없으면 false를 반환합니다. StringTokenizer는 내부적으로 어떤 위치의 토큰을 사용하였는지 기억하고 있고 그 위치를 다음으로 옮깁니다. |
|      Object nextElement(), String nextToken()      | 이 두가지 메소드는 다음의 토큰을 반환합니다. 두가지 메소드는 같은 객체를 반환하는데 반환형은 다릅니다. nextElement는 Object를, nextToken은 String을 반환하고 있습니다. |



#### 예제

- String 클래스에 있는 split 메소드를 이용합니다.

```java
public static void main(String[] ar){
	String str="this string includes default delims";
	System.out.println(str);
	System.out.println();
		
	System.out.println("==========using split method============");
	String []tokens=str.split(" ");
		
	for(int i=0;i<tokens.length;i++){
		System.out.println(tokens[i]);
	}
}
```

```java
// 결과
this string includes default delims

==========using split method============
this
string
includes
default
delims
```

​	String 클래스의 메소드 split 메소드를 사용해 StringTokenizer를 흉내낼 수 있습니다. split이 반환하는 값은 String 배열입니다.



- Default Delim을 이용합니다.

```java
public static void main(String[] ar){
	String str="this string\tincludes\ndefault delims";
	StringTokenizer stk=new StringTokenizer(str);
	System.out.println(str);
	System.out.println();
		
	System.out.println("total tokens:"+stk.countTokens());
	System.out.println("================tokens==================");
	while(stk.hasMoreTokens()){	// 현재위치의 다음에 토큰이 있는지 확인합니다.
		System.out.println(stk.nextToken());	// 다음 토큰을 가져옵니다.
	}
	System.out.println("total tokens:"+stk.countTokens());
}
```

```java
// 결과
this string includes
default delims

total tokens:5
================tokens==================
this
string
includes
default
delims
total tokens:0
```



- 특정 Delim을 이용합니다.

```java
public static void main(String[] ar){
	String str="this-=string-includes=delims";
	StringTokenizer stk=new StringTokenizer(str,"-=");	
    // -와 = 문자로 문자열을 분리합니다.
	System.out.println(str);
	System.out.println();
		
	System.out.println("total tokens:"+stk.countTokens());
	System.out.println("================tokens==================");
	while(stk.hasMoreTokens()){
		System.out.println(stk.nextToken());
	}
	System.out.println("total tokens:"+stk.countTokens());
}
```

```java
// 결과
this-=string-includes=delims

total tokens:4
================tokens==================
this
string
includes
delims
total tokens:0
```

​	`-`와 `=` 로 문자를 쪼갭니다. `-=` 도가능합니다.



- String의 split과 비교

```java
public static void main(String[] ar){
	String str="this-=string-includes=delims";
	System.out.println(str);
	System.out.println();
		
	String[] tokens=str.split("-=");
	System.out.println("total tokens:"+tokens.length);
	System.out.println("================tokens==================");
		
	for(int i=0;i<tokens.length;i++){
		System.out.println(tokens[i]);
	}
		
}
```

```java
// 결과
this-=string-includes=delims

total tokens:2
================tokens==================
this
string-includes=delims
```

​	정확히 `-=` 로 문자를 쪼갭니다.



- Delim까지 토큰으로 포함합니다.

```java
public static void main(String[] ar){
	String str="this-string-includes=delims";
	StringTokenizer stk=new StringTokenizer(str,"-=",true);
    // returnDelims를 true로 함으로써, Delim을 토큰으로 포함시킵니다.
	System.out.println(str);
	System.out.println();
		
	System.out.println("total tokens:"+stk.countTokens());
	System.out.println("================tokens==================");
	while(stk.hasMoreTokens()){
		System.out.println(stk.nextToken());
	}
	System.out.println("total tokens:"+stk.countTokens());
}
```

```java
// 결과
this-string-includes=delims

total tokens:7
================tokens==================
this
-
string
-
includes
=
delims
total tokens:0
```

 `-` 와 `=` 까지 토큰으로 포함하기 때문에 총 토큰 수가 7개로 나오고 토큰에도 포함된 것을 볼 수 있습니다.

---

출처 : https://reakwon.tistory.com/90