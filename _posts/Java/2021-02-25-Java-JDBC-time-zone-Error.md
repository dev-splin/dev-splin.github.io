---
title: "Java : JDBC time zone Error"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Error"
toc: true
toc_sticky: true
toc_label: 목차
---

# JDBC `time zone` 관련 에러

JDBC 실습 진행 중 에러가 발생했습니다..ㅠ

```java
 The server time zone value '���ѹα� ǥ�ؽ�' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.
```

원인은 MySQL 버전 5.1.23보다 높은 버전을 사용하면 MySQL 타임존의 시간표현 포맷이 달라져서 connector 에서 인식을 하지 못한다 것이라고 합니다. 

해결 방법은 MySQL 버전을 5.1.23 이하로 낮추거나 밑의 3가지 방법있습니다.



## 1. JDBC URL에 옵션추가

`?serverTimezone=UTC` 를 `<property name="url" value="jdbc:mysql://127.0.0.1:3306/blog_backupdb` 뒤에 넣는 방법입니다.

**UTC(Coordinated Universal Time:세계 협정시)**는 예전의 GMT(Greenwich Mean Time)가 표준화된 것 이고 우리나라의 표준시는 **KST(UTC + 9:00)** 라고 합니다.

jdbc url의 옵션으로 KST를 사용하면 에러가 발생합니다. 대신 `?serverTimezone=Asia/Seoul` 을 사용할 수 있습니다.

이처럼 URL 에 옵션으로 타임존 값을 주면 에러가 발생하지는 않지만 시간은 서버의 시스템 시간 값으로 값이 나온다고 합니다.

```xml
<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
  <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
  <property name="url" value="jdbc:mysql://127.0.0.1:3306/blog_backupdb?serverTimezone=UTC" /> 
  <property name="username" value="backupdbadm"/>
  <property name="password" value="backupdbadm"/>
</bean>
```



## 2. dburl에 timezone 설정해주기

이 방법은 간단합니다. dburl 뒤에 `?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC` 만 추가해주시면 됩니다. 만약 url 뒤에 다른 내용이 있다면 `?` 대신 `&` 를 사용해 주세요.

```java
private static String dburl = "jdbc:mysql://localhost:3306/connectdb?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC";
```



## 3. 데이터베이스 서버에 default-time-zone 설정하기

`my.ini` 파일에 `[mysqld]` 섹션에 `default-time-zone`을 추가합니다. 아래는 KST 로 지정한 것입니다.

```ini
[mysqld]
default-time-zone='+9:00'
```

default-time-zone='UTC' 형식으로 설정하면 에러가 발생한다고 합니다. MariaDB, MySQL 동일하다고 합니다. 

UTC로 지정하기 위해서는 '+0:00' 또는 '-0:00' 으로 설정합니다. 그냥 '0:00' 으로 설정하면 안됩니다.

my.ini 파일에서 설정하여 시간을 조회하면 정확히 설정된 타임존의 시간이 나옵니다.



### my.ini 경로

`my.ini`는 보통 `C:\ProgramData\MySQL\MySQL Server 5.7`에 있습니다.

ProgramData 파일은 숨겨진 파일인데, 숨겨진 파일은 `폴더의 옵션 중 보기 -> 옵션 -> 폴더 및 검색 옵션 변경 ->  보기 탭 ` 에 가셔서 스크롤 바 내리다보면 `숨김 파일, 폴더 및 드라이브 표시` 에 체크하시면 됩니다.



---

참고 : https://irerin07.tistory.com/14

https://offbyone.tistory.com/318