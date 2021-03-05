---
title: "Java : JDBC(Java Database Connectivity)"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : JDBC(Java Database Connectivity)"
toc: true
toc_sticky: true
toc_label: 목차
---

# JDBC (Java Database Connectivity)

- 자바를 이용한 데이터베이스 접속과 SQL 문장의 실행, 그리고 실행 결과로 얻어진 데이터의 핸들링을 제공하는 방법과 절차에 관한 규약
-  자바 프로그램 내에서 SQL문을 실행하기 위한 자바 API
- SQL과 프로그래밍 언어의 통합 접근 중 한 형태

- JAVA는 표준 인터페이스인 JDBC API를 제공
- 데이터베이스 벤더, 또는 기타 써드파티에서는 JDBC 인터페이스를 구현한 드라이버(driver)를 제공합니다.



## JDBC 환경 구성

- JDK 설치
- JDBC 드라이버 설치
  -  Maven에 다음과 같은 의존성을 추가한다. MySQL사이트에서 다운로드 한다.



```xml
<dependency>   
  <groupId>mysql</groupId>   
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.45</version>
 </dependency>
```



- [Java API Reference 참고 바로가기](https://docs.oracle.com/javase/8/docs/api/)
- [JDBC Tutorial 참고 바로가기](https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html)



### JDBC를 이용한 프로그래밍 방법

1. import java.sql.*;
2. 드라이버를 로드 한다.
   - 벤더(Mysql, Oracle...)가 제공해주는 라이브러리를 사용할 수 있게 로딩해줍니다.
3. Connection 객체를 생성한다.
   - Connection 객체는 인터페이스이며, DB 접속을 도와주는 객체입니다.
4. Statement 객체를 생성 및 질의 수행
   - Statement 객체는 쿼리문을 생성하고 실행하는 것을 도와주는 객체입니다.
5. SQL문에 결과물이 있다면 ResultSet 객체를 생성한다.
   - select 은 결과가 나오지만 나머지 insert, update, delete는 수행하고 나면 결과가 나오지 않습니다. (잘 입력했다는 안내 메세지만 나옵니다.)
6. 모든 객체를 닫는다.



### JDBC 클래스의 생성 단계

[![img](https://cphinf.pstatic.net/mooc/20180201_49/1517475141729UGWfv_PNG/2_11_1_JDBC_.PNG?type=w760)](https://www.boostcourse.org/web326/lecture/58939/#)



#### JDBC 사용 - 단계별 설명

1. IMPORT

```java
import java.sql.*;
```

2. 드라이버 로드

```java
Class.forName( "com.mysql.jdbc.Driver" );
```

3. Connection 얻기

```java
String dburl  = "jdbc:mysql://localhost/dbName";

Connection con =  DriverManager.getConnection ( dburl, ID, PWD );
```

​	**소스코드 예제**

```java
public static Connection getConnection() throws Exception{
	String url = "jdbc:oracle:thin:@117.16.46.111:1521:xe";
	String user = "smu";
	String password = "smu";
	Connection conn = null;
	Class.forName("oracle.jdbc.driver.OracleDriver");
	conn = DriverManager.getConnection(url, user, password);
	return conn;
}
```

4. Statement 생성

```java
Statement stmt = con.createStatement();
```

5. 질의 수행

```java
ResultSet rs = stmt.executeQuery("select no from user" );

참고
stmt.execute(“query”);             //any SQL
stmt.executeQuery(“query”);     //SELECT
stmt.executeUpdate(“query”);   //INSERT, UPDATE, DELETE
```

6. ResultSet으로 결과 받기

```java
ResultSet rs =  stmt.executeQuery( "select no from user" );
while ( rs.next() )
      System.out.println( rs.getInt( "no") );
```

7. Close

```java
rs.close();

stmt.close();

con.close();
// 역순으로 닫아 줍니다.
```

 

#### 예제

```java
public List<GuestBookVO> getGuestBookList(){
		List<GuestBookVO> list = new ArrayList<>(); 
		GuestBookVO vo = null;
		Connection conn = null;		// Connection객체를 받을 변수
		PreparedStatement ps = null;	// Statement객체를 받을 변수 
		ResultSet rs = null;	// ResultSet객체를 받을 변수
		try{
			conn = DBUtil.getConnection(); 
            // getConnection()함수를 통해 DriverManager에서 Connection객체를 가져 옵니다.
			String sql = "select * from guestbook";
			ps = conn.prepareStatement(sql);	
            // Connection에서 Statement를 가져옵니다.
			rs = ps.executeQuery();	// Statement에서 sql을 통해 ResultSet을 가져옵니다.
			while(rs.next()){	// 반복문을 통해 ResultSet에 값이 있는 지 체크
				vo = new GuestBookVO();
				vo.setNo(rs.getInt(1));
				vo.setId(rs.getString(2));
				vo.setTitle(rs.getString(3));
				vo.setConetnt(rs.getString(4));
				vo.setRegDate(rs.getString(5));
				list.add(vo);
			}
		}catch(Exception e){
			e.printStackTrace();
		}finally {
			DBUtil.close(conn, ps, rs);	// close 함수를 통해 닫아줍니다.
		}		
		return list;		
	}
```

---

```java
public int addGuestBook(GuestBookVO vo){
		int result = 0;
		Connection conn = null;
		PreparedStatement ps = null;
		try{
			conn = DBUtil.getConnection();
			String sql = "insert into guestbook values("
					+ "guestbook_seq.nextval,?,?,?,sysdate)";
			ps = conn.prepareStatement(sql);
			ps.setString(1, vo.getId());	
            // 파라미터로 받아온 GuestBookVo의 값들을 sql의 values로 설정해 줍니다.
			ps.setString(2, vo.getTitle());
			ps.setString(3, vo.getConetnt());
			result = ps.executeUpdate();	// sql문을 실행시키고 결과를 받아옵니다. (insert/delete/update 는 반영된 레코드 수, create/drop은 -1)
		}catch(Exception e){
			e.printStackTrace();
		}finally {
			DBUtil.close(conn, ps);
		}
		
		return result;
	}
```

---

```java
public static void close(Connection conn, PreparedStatement ps){
    // Connetion과 Statement를 닫아주는 함수
		if (ps != null) {
			try {
				ps.close();
			} catch (SQLException e) {e.printStackTrace(); }
		}
		if (conn != null) {
			try {
				conn.close();
			} catch (SQLException e) {e.printStackTrace();}
		}
	}
```

---

참고 : https://www.boostcourse.org/web326/lecture/58939/

 