---
title: "Java : try-with-resources를 통한 자원해제"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : try-with-resources"
toc: true
toc_sticky: true
toc_label: 목차
---

# try-with-resources를 통한 자원해제

- 기존의 try catch 방법에서는 자원해제를 위해 finally를 사용했지만, try-wit-resources는 try에 자원을 넣으면 try 구문이 끝날 때 자동으로 자원해제 해주기 때문에 실수를 방지할 수 있습니다.

- try-with-resources를 사용하려면 `java.lang.AutoCloseable` , `java.io.Closeable` 인터페이스를 포함한 객체여야만 합니다.

- try-with-resources를 사용할 때 catch와 finally는 생성된 자원이 닫히고난 후에 실행됩니다.



#### 예제

Role이라는 데이터베이스에서 모든 데이터를 가져오는 예제입니다.'



**기존의 방법**

```java
public List<Role> getRoles() {
		List<Role> list = new ArrayList<>();

		try {
			Class.forName("com.mysql.jdbc.Driver");
            Connection conn = DriverManager.getConnection(dburl, dbUser, dbpasswd);
            
            String sql = "SELECT description, role_id FROM role order by role_id desc";
            PreparedStatement ps = conn.prepareStatement(sql)
            ResultSet rs = ps.executeQuery()
            
            while (rs.next()) {
					String description = rs.getString(1);
					int id = rs.getInt("role_id");
					Role role = new Role(id, description);
					list.add(role);
				}
            
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
            if (rs != null) {
			try {
				rs.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			}
		
            if (ps != null) {
                try {
                    ps.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
		
            if (con != null) {
                try {
                    con.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
		return list;
}
```



**try-with-resource를 통한 방법**

```java
public List<Role> getRoles() {
		List<Role> list = new ArrayList<>();

		try {
			Class.forName("com.mysql.jdbc.Driver");
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}

		String sql = "SELECT description, role_id FROM role order by role_id desc";
		try (Connection conn = DriverManager.getConnection(dburl, dbUser, dbpasswd);
				PreparedStatement ps = conn.prepareStatement(sql)) {
            // 여러개의 자원을 넣을 수 있습니다.
			try (ResultSet rs = ps.executeQuery()) {
                // 해제해야할 객체들을 try에 넣어줍니다.

				while (rs.next()) {
					String description = rs.getString(1);
					int id = rs.getInt("role_id");
					Role role = new Role(id, description);
					list.add(role);
				}
			} catch (Exception e) {
				e.printStackTrace();
			}
		} catch (Exception ex) {
			ex.printStackTrace();
		}
    	// 따로 close하지 않아도 자동으로 해제해 줍니다.
		return list;
```



## 주의할 점

만약 try 블록에서 예외가 발생하고 try-with-resources에서도 예외가 발생하면 try-with-resources의 예외는 억제되고 try 블록에서 던져진 예외는 해당 메소드에 의해 던져지는 예외입니다.

억제된 try-with-resources의 예외는 try블록에서 `Throwable.getSuppressed` 메소드를 호출하여 찾아볼 수 있다고 합니다.

---

참고 : https://www.boostcourse.org/web326/lecture/258490/

https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html#suppressed-exceptions