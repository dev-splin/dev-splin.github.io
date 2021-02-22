---
title: "Python : 파일 입출력"
excerpt_separator: <!--more-->
categories:
  - Python
tags:
  - Python
toc: true
toc_sticky: true
toc_label: 목차
---

# 파일 입출력

- 프로그램에서 생성한 정보를 영구적으로 저장할 때는 파일에 기록한다.
- 메모리는 전원이 끊기면 내용을 잊어버리기 때문에 하드디스크에 저장해야한다.
- open 함수를 사용해 파일을 연다.
  ![파일입출력1](https://user-images.githubusercontent.com/58713853/73336215-8d46df00-42b4-11ea-9d7c-423f93abea6e.PNG)

- open 함수는 파일의 입출력을 준비한다.
- read, write 메소드를 호출해 파일을 사용한다.
- 파일을 다 사용하고 나면 close 메소드를 호출 한다.
  ![파일입출력2](https://user-images.githubusercontent.com/58713853/73339950-1feb7c00-42bd-11ea-9867-62d8180a87c8.PNG)



## 파일 쓰기



#### live.txt 파일을 생성하고 내용을 저장해 보는 예제

```python
file = open("live.txt", "w", encoding='UTF-8') // 한글을 사용하려면 인코딩을 해주어야 한다.
file.write(""" 삶이 그대를 속일지라도 슬퍼하거나 노하지 말라! 우울한 날들을 견디면 믿으라, 기쁨의 날이 오리니 """) 
file.close()
```



## 파일 읽기



#### live.txt 파일을 읽어서 출력해 보는 예제

```python
try:
  file = open("live.txt","r")
  text = file.read()
  print(text)
except FileNotFoundError:
  print("파일이 없습니다.")
finally:
  file.close()
```



#### 한 줄 씩 읽어서 출력해 보는 예제

```python
file = open("live.txt", "r")
line = 1
while True:
  row = file.readline()
  if not row:
    break
  print(str(line) + " :" + row, end='')
  line += 1
file.close()
```



## 파일 내용 추가



#### live.txt 파일에 내용을 추가 해보는 예제

```python
file = open("live.txt","a")
file.write("\n\n푸쉬킨 형님의 말씀이었습니다.")
file.close()
```



## 파일 관리 함수

- 파일 입출력이 아닌 파일 복사, 삭제, 폴더 복사, 삭제 등 파일을 관리한다.
  ![파일입출력3](https://user-images.githubusercontent.com/58713853/73340513-347c4400-42be-11ea-9ee4-2d5c1ec73551.PNG)



## 폴더 관리 함수

- 폴더를 관리 하는 함수이다.
  ![파일입출력4](https://user-images.githubusercontent.com/58713853/73340514-347c4400-42be-11ea-95b6-0196755e025f.PNG)



## 경로(PATH) 관리 함수

- 폴더 경로를 조사하고 파일 및 폴더를 구분하는 핫무를 제공한다.
  ![파일입출력5](https://user-images.githubusercontent.com/58713853/73340512-33e3ad80-42be-11ea-934e-c339f4830c60.PNG)



#### 지정한 경로에 파일 목록을 출력해보는 예제

```python
def dumpdir(path):
  files = os.listdir(path)
  for f in files:
    fullpath = path + "/" + f
    if os.path.isdir(fullpath):
      print("[" + fullpath + "]")
      dumpdir(fullpath)
    else:
      print("\t" + fullpath)
      
dumpdir("/Users/Yun/Documents") // 해당하는 경로를 쓰면 된다.
```

