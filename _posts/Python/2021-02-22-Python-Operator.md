---
title: "Python : 연산자"
excerpt_separator: <!--more-->
categories:
  - Python
tags:
  - Python
toc: true
toc_sticky: true
toc_label: 목차
---



# 연산자



## 산술 연산자

- 더하기(+), 빼기(-), 곱하기(*), 나누기(/) 연산을 수행한다.



## 고급 연산자

- 거듭제곱(**), 정수 나누기(//), 나머지(%) 



## 복합 대입 연산자

- 대입 연산자는 우변의 값이나 수식을 계산하여 좌변에 대입한다.
- 우변의 수식에 좌변의 변수가 올 수 있다.

```python
a = 5
a = a + 1   ## a += 1로 표현가능
print(a)
```



## 문자열 연산자

- 문자열에도 연산자를 사용할 수 있으며 수학적인 연산과는 다르게 동작
- 문자열을 비교 할 때에는 a보다 b가 더 큰 문자이고 사전의 더 뒤쪽에 나오는 문자열을 더 큰것으로 판단한다.

```python
s1 = "대한민국"
s2 = "만세" 

print(s1 + s2) 
print("대한민국" + "만세")

print("만세" * 5) ## 만세가 5번 나옴
print("-" * 40)   ## '-' 이 40번 나옴
```



## 정수와 문자열 연산

- 정수와 문자열은 섞어서 사용할 수 없다.
- 같은 타입으로 바꾸어서 사용해야한다.
- 정수는 str함수를 사용하여 문자열로 변경 할 수 있다.
- 문자열은 int함수로 정수로 변경할 수 있다.

```python
print("대한민국" + 2002)        ## TypeError
print("대한민국" + str(2002))

print(22 + "10")                ## TypeError
print(22 + int("10"))
print(22 + int("10점"))           ## 숫자와 문자가 섞여있으므로 불가능
```



## 실수의 변환

- 문자열을 실수로 변환할 때 float 함수를 사용한다.
- 실수를 정수로 변환할 때 int 함수를 사용한다.



## 반올림

- 반올림을 하기 위해서는 round 함수를 사용한다.

```python
print(round(2.54,1)) ## 소수점 첫번째 자리에서 반올림
print(round(2.54,-3)) ## 백의 자리에서 반올림
```



## 연산자의 종류

![연산자 종류1](https://user-images.githubusercontent.com/58713853/71084622-0ee91e80-21d9-11ea-9b0f-654c746fc95d.PNG)

![연산자 종류2](https://user-images.githubusercontent.com/58713853/71084627-101a4b80-21d9-11ea-83e9-4b740d93efe1.PNG)

