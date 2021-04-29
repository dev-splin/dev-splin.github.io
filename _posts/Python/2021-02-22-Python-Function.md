---
title: "Python : 함수"
excerpt_separator: <!--more-->
categories:
  - Python
tags:
  - Python	
toc: true
toc_sticky: true
toc_label: 목차
---

## 함수
- 반복되는 코드를 제거할 수 있다.
  
  - 비슷한 코드를 함수로 만들어 여러번 호출하여 사용
- 유지보수
  
  - 핵심 코드가 한 번만 작성이 되어 있어 한 곳만 수정하면 됨
- 재사용성
  
  - 함수를 잘 만들어 놓으면 다른 프로젝트에도 또 사용 할 수 있음
- 실수의 가능성을 줄임
  - ex) range의 함수는 끝 범위가 제외 되지만 함수에서 end + 1 을 처리 하여 실수 가능성이 낮음
  
  ```python
  def 함수명(인수 목록) :
    본체
  ```
  



### pass 명령

- pass 명령은 아무 동작도 하지 않는다.
- 나중에 내용을 작성 해야할 때 사용한다.
  - 함수의 내용을 나중에 작성 해야할 때
  - if, else 내용을 나중에 작성 해야할 때
  
  ```python
  def calctotal():
    pass
    
  if sum >= 90:
    pass
  else:
    pass
  ```
  
  
### 가변 인수
- 일반 함수를 호출할 때는 함수 정의문의 형식에 정의된 인수 개수만큼 전달 해야한다.
- 가변 인수 함수는 임의 개수의 인수를 받을 수 있다.
- 인수 이름 앞에 * 기호를 붙이면 된다.
- 가변 인수는 인수 목록의 마지막에 와야 한다.

```python
def intsum(s, *ints):
def intsum(*ints, s):  #TypeError
def intsum(*ints, *nums):  #SyntaxError
```



#### 가변 인수를 사용해서 인수를 모두 더하는 예제

```python
def intsum(*ints):
 sum = 0
 for num in ints:
   sum += num
 return sum
  
print(intsum(1, 2, 3))
print(intsum(5, 7, 9, 11, 13))
```



### 인수의 기본값

- 인수가 많으면 작업을 섬세하게 전달할 수 있다.
- 잘 변하는 값이 아니면 호출 할 때 항상 전달해야 함으로 불편한다.
- 잘 변하는 값이 아니면 기본값을 지정하면 호출 할때 생략이 가능하다.



#### 인수의 기본값 사용한 예제

```python
def calcstep(begin, end, step = 1):
 sum = 0
 for num in range(begin, end + 1, step):
   sum += num
 return sum
  
print(calcstep(1, 10, 2))
print(calcstep(1, 100))
```



### 키워드 인수

- 많은 인수를 함수에 정의된 순서로 인수를 넘기면 헷갈린다.
- 인수 위치를 잘 못 넣으면 정상적인 동작을 할 수 없다.
- 인수의 이름을 지정하여 함수에 전달하는 방식을 키워드 인수라 한다.



#### 키워드 인수를 사용한 예제)

 ```python
 def calcstep(begin, end, step):
  sum = 0
  for num in range(begin, end + 1, step):
    sum += num
  return sum
  
print("3 ~ 5 =", calcstep(3, 5, 1))
print("3 ~ 5 =", calcstep(begin=3, end=5, step=1)) 
print("3 ~ 5 =", calcstep(step=1, end=5, begin=3)) 
print("3 ~ 5 =", calcstep(3, 5, step=1))    # 키워드를 한개만 붙여주면 나머지 인수는 원래 순서대로 인식한다. (begin = 3, end = 5)
print("3 ~ 5 =", calcstep(3, step=5, end=1))  # begin = 3

# 다만 일정부분만 키워드를 사용한다면 인수 맨 오른쪽부터 사용가능하다
# 3, 5, begin=3  같은 형식으로 사용 불가, 프로그램이 3, 5중 어느 것이 end 고 step인지 인지할 수 없음
# 내 생각엔 함수는 인수 오른쪽부터 인식하는 이유 때문인 것 같다. (C++ 수업 때 들었던 내용)
 ```



### 키워드 가변 인수

- 가변 인수와 키워드 인수를 합친 개념이다.
- 가변 인수의 특징처럼 임의의 인수를 받을 수 있지만, 함수 내부에서 특정 이름의 인수를 사용한다.
- 사용시엔 키워드 인수처럼 인수이름을 붙여주어야 한다.
- 내 생각엔 임의의 인수를 받을 수 있지만, 함수 내부에서 특정 이름의 인수를 사용함으로써 사용시 인수를 잘 못 입력하는 실수를 방지할 수 있는 것 같다.



#### 키워드 가변 인수 사용 예제

```python
def calcstep(**args):
  begin = args['begin']
  end = args['end']
  step = args['step']
   
  sum = 0 
  for num in range(begin, end + 1, step):
    sum += num   return sum
    
print("3 ~ 5 = ", calcstep(begin=3, end=5, step=1))
print("3 ~ 5 = ", calcstep(step=1, end=5, begin=3))
```



### 지역 변수

- 함수 내부에 선언하는 변수를 지역 변수라고 한다.
- 지역변수는 함수 안에서만 사용될 뿐 함수 밖에서는 사용할 수 없다.



### 전역 변수

- 함수 밖에서 선언된 변수를 전역 변수라고 한다.
- 전역 변수는 프로그램 어디서나 사용 가능하다.
- 함수 내부에서 global 키워드를 사용하여 변수를 생성하면 전역 변수가 된다.
  - 다만, 함수를 호출한 후에야 global을 사용한 전역 변수가 만들어 진다.



### docstring

- 함수의 사용법이나 인수의 의미, 주의 사항 등을 남길 수 있다.
- 함수 선언문과 본체 사이에 작성한다.
- help 함수를 사용해 docstring을 출력 할 수 있다.

```python
a = 0 # 전역 변수

def sale():
  """ 전역 변수와 지역 변수 차이 """ # docstring
  saleprice = 0 # 지역 변수
  global price  # 함수 내부에서 전역 변수 선언
  price = 500
    
sale() 
print(price)
```



### 내장 함수

- 파이썬 내부에 미리 정의해 놓은 함수를 내장 함수라고 한다.
- 별도의 import문을 사용하지 않아도 언제든지 사용할 수 있다.
  - 입력과 출력 : print, input
  - 타입 변환 : int, str, float
  - 진법 변환 : hex, oct, bin
  
