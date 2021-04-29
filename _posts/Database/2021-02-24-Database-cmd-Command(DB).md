---
title: "Database : cmd 명령어"
excerpt_separator: <!--more-->
categories:
  - Database
tags:
  - Database
  - "Database : cmd Command"
toc: true
toc_sticky: true
toc_label: 목차
---

# cmd 명령어 (DB)

- mysql이 붙은 명령어는 mysql로 접속하기 전에 사용할 수 있습니다.
- 키워드는 대소문자를 구별하지 않습니다.
- 여러 문장을 한 줄에 연속으로 붙여서 실행가능합니다.
  - 한 줄이 끝나는 것을 semicolon(;) 으로 구별하기 때문에
- 하나의 SQL을 여러 줄로 입력가능합니다.
  - 마찬가지로 한 줄이 끝나는 것을 semicolon(;)으로 구별하기 때문에 가능합니다.
- SQL입력하는 도중에 `\c`를 입력하면 취소할 수 있습니다.



## 명령어

##### mysql 

- –uroot  -p
  - mySQL 관리자 계정인 root로 데이터베이스 관리 시스템에 접속

- -h**호스트명** -u**DB계정명** -p **데이터베이스명** < **실행시킬파일명**
  - `호스트명`에 해당하는 호스트에서 `DB계정명`에 해당하는 계정을 원하는 DB에 접속할 수 있게 한다.
  - `<` 뒤에 파일명을 적으면 파일을 실행시켜준다(실행시킬 파일이 있는 폴더로 이동해야함)

---

##### grant **all privileges** on **db이름.*** to **계정이름@'%'** identified by ＇암호’;

- **all privileges**
  - 모든 권한을 부여합니다.
- **db이름.***
  - `db이름` 으로된 데이터베이스의 모든테이블에 권한을 부여합니다.
- **계정이름@'%' ** or **계정이름@'localhost'**
  - '%'가 붙으면`계정이름` 으로된 계정을 어떤 클라이언트에서든 접근 가능
  - 'localhost'가 붙으면 해당 컴퓨터에서만 접근 가능

---

##### flush privileges;

- DMBS에 적용하라는 뜻입니다. 위에 grant all 후에 해주어야 적용됩니다.

---

##### **quit;** or **exit**

- 연결끊기

---

##### show databases;

- 현재 서버에 어떤데이터베이스가 존재하는지 알아봅니다.

---

##### use **데이터베이스명**

- 데이터베이스를 전환합니다. (사용할 데이터베이스를 선택)

---

##### show tables;

- Database를 선택 후, Database의 전체 테이블 목록을 출력합니다.

---

##### desc **테이블명**

- `테이블명`에 해당하는 테이블의 구조를 볼 수있습니다.

---

##### create database **DB이름**;

- `DB이름` 으로 데이터베이스를 생성합니다.

---

##### tree /f

- 현재 폴더 안에 있는 폴더의 경로를 트리 형태로 볼 수 있습니다.

출처 : https://www.cockroachlabs.com/docs/stable/grant.html#supported-privileges

https://www.boostcourse.org/web326/lecture/258482/

https://www.boostcourse.org/web326/lecture/258481/