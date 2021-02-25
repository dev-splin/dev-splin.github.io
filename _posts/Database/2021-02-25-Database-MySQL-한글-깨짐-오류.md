---
title: "Database : MySQL 한글 깨짐 오류"
excerpt_separator: <!--more-->
categories:
  - Database
tags:
  - Database
  - "Database : Error"
toc: true
toc_sticky: true
toc_label: 목차
---

# MySQL 한글 깨짐 오류

JDBC 실습 중에 DB에 INSERT 했더니 한글이 깨진 현상이 발생해 시간을 꽤 잡아먹고 해결했는데.. 다른 분들은 금방 해결하길 바라며.. 포스팅합니다.. ㅠ

<u>이 포스트는 MySQL에서 한글이 깨지거나 한글 INSERT가 안될 때 해결할 수 있는 방법입니다.</u>



## MySQL 기본 character set 설정하기

character set을 명시적으로 설정하지 않으면 MySQL 5.7 이하는 *latin1*, MySQL 8은 *utf8mb4* 가 된다. 현재 character set 확인은 다음과 같이 mysql 클라이언트로 연결한 후에 `status` 나 `show variables like 'char%';`명령어로 알수 있습니다.



**status**

![status](https://user-images.githubusercontent.com/79291114/109167752-6f9ebe80-77c1-11eb-836c-75fd96667321.png)



**show variables like 'char%'**

![variables](https://user-images.githubusercontent.com/79291114/109167755-70cfeb80-77c1-11eb-8b10-b5473cf35396.png)



character set은 my.ini에서 바꿀 수 있습니다.

MySQL 5.1 과 5.5 이상은 my.ini 에 character set 설정하는 방식이 약간 다르다.



### my.ini(cnf) 경로

`my.ini`는 보통 `C:\ProgramData\MySQL\MySQL Server 5.7`에 있습니다.

ProgramData 파일은 숨겨진 파일인데, 숨겨진 파일은 `폴더의 옵션 중 보기 -> 옵션 -> 폴더 및 검색 옵션 변경 ->  보기 탭 ` 에 가셔서 스크롤 바 내리다보면 `숨김 파일, 폴더 및 드라이브 표시` 에 체크하시면 됩니다.





### My SQL 5.5이상

```ini
[mysqld]
collation-server = utf8_unicode_ci
character-set-server = utf8
skip-character-set-client-handshake
```



### MY SQL 5.1

```ini
[mysqld]
character-set-server = utf8
 
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8
```

ini 설정 후 MySQL을 재시작하고 `status` 나 `show variables like 'char%';` 확인해 보면 utf8로 바뀐것을 볼 수 있습니다.



#### 기존 DB

이미 기존에 만들어진 database의 DB character set이 안바뀌는 경우가 있는데, 이 때는 `alter database` 명령어로 character set과 collate 를 변경할 수 있습니다. (전 이부분을 몰라서 오류인 줄 알고.. sql을 몇 번 다시 설치했습니다.. ㅠㅠ)

```sql
ALTER DATABASE homestead CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;
```



#### 콘솔 창에서만 한글이 깨지는 경우

character set을 설정을 해도 콘솔 창에서는 한글이 깨지는 경우가 있습니다. 그 이유는 MySQL 콘솔의 문자 셋 설정이 아래 그림과 같이 MS949로 설정되어 있기 때문입니다.

![cmd character set](https://user-images.githubusercontent.com/79291114/109170034-c1484880-77c3-11eb-8fce-cf4e2db762d7.jpg)

**해결 방법**

해결 방법은 간단합니다. cmd 창에서 `chcp 65001`을 입력하면 character set이 utf-8로 바뀝니다.



---

참고 : https://www.lesstif.com/dbms/mysql-rhel-centos-ubuntu-20775198.html

https://prioriq.tistory.com/5