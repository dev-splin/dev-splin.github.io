---
title: "Spring : STS를 이용하지 않는 웹 프로젝트"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
toc: true
toc_sticky: true
toc_label: 목차
---

# STS를 이용하지 않는 웹 프로젝트
- STS를 사용해서 만들었던 구조와 동일하게 직접 폴더와 파일들을 생성해서 웹 프로젝트를 만든다.

- 편한 STS를 이용하지 않는 이유는 실무에서 작업을 할 때 STS를 쓰지 않는 경우도 있으므로 다양한 환경에 적응하기 위함이다.

  

## 스프링 MVC 웹 애플리케이션 제작을 위한 폴더 생성

![1](https://user-images.githubusercontent.com/58713853/102049153-b5cfbd00-3e23-11eb-939d-c5c5847b4311.PNG)



## pom.xml 및 이클립스 import

- pom, web, root-context, servlet-context는 Summary7에서 만든 것을 복붙 한다. (여러 네임스페이스들을 전부 외워 입력할 수 없기 때문에)
- pom.xml을 복붙 했을 때 groupId와 artifactId로 이루어진 이름으로 패키지를 만든다 ( 여기서는 com.bs.lec16 )

![2](https://user-images.githubusercontent.com/58713853/102049156-b6685380-3e23-11eb-8ef1-b74a8c364832.PNG)

![3](https://user-images.githubusercontent.com/58713853/102049158-b700ea00-3e23-11eb-9f03-1cb36e38b115.PNG)



## web.xml 작성

![4](https://user-images.githubusercontent.com/58713853/102049160-b700ea00-3e23-11eb-8cae-e5b824e989d3.PNG)



## 스프링 설정 파일(servlet-context.xml) 작성

![5](https://user-images.githubusercontent.com/58713853/102049165-b7998080-3e23-11eb-86fa-76b43c9d764f.PNG)



## root-context.xml 작성

![6](https://user-images.githubusercontent.com/58713853/102049167-b7998080-3e23-11eb-8b9a-ca32a79b3d12.PNG)



## 컨트롤러와 뷰 작성

![7](https://user-images.githubusercontent.com/58713853/102049171-b8321700-3e23-11eb-9238-a3f77f57ea93.PNG)



## 실행

![8](https://user-images.githubusercontent.com/58713853/102049173-b8321700-3e23-11eb-9fe5-d0c9b75383d9.PNG)
