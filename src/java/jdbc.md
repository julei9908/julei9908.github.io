# jdbc

#### 前言

jdbc（java database connection）定义了一套java操作数据库的接口规范。相关的类在java.sql、javax.sql包下

#### JDBC配置文件：jdbc.properties

```
driverClass = com.mysql.jdbc.Driver
url = jdbc:mysql://localhost:3306/dnname
user = root
password = 123456
```

#### JDBC工具类: JDBCUtils

```java
import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class JDBCUtils {

    static String driverClass = null;

    static String url = null;

    static String user = null;

    static String password = null;
	
    static {
        // 通过properties对象读取外部配置的内容
        Properties prop = new Properties();
        try {
            InputStream in = JDBCUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
            // 加载外部的配置文件
            prop.load(in);
        } catch (IOException e) {
            e.printStackTrace();
        }
        // 读取外部配置文件的内容
        driverClass = prop.getProperty("driverClass");
        url = prop.getProperty("url");
        user = prop.getProperty("user");
        password = prop.getProperty("password");
        // 注册驱动
        try {
            Class.forName(driverClass);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    // 获得连接
    public static Connection getConnection() throws Exception {
        // 获得连接
        Connection connection = DriverManager.getConnection(url, user, password);
        return connection;
    }
	
    // 释放资源
    public static void release(Connection conn, Statement stmt, ResultSet rs) {
        try {
            if (rs != null) {
                rs.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        rs = null;
        release(conn, stmt);
    }

    // 释放资源
    public static void release(Connection conn, Statement stmt) {
        try {
            if (stmt != null) {
                stmt.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        stmt = null;
        try {
            if (conn != null) {
                conn.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        conn = null;
    }
   
}
```

#### 测试

```java
@Test
public void testJDBCUtils() {

    Connection conn = null;

    Statement stmt = null;

    ResultSet rs = null;

    try {
        // 获得连接
        conn = JDBCUtils.getConnection();
        // 获得发送sql的对象
        stmt = conn.createStatement();
        // 拼接sql
        String uname = "zhangsan";
        String upwd = "abc";
        String sql = "select * from user where username='" + uname + "' and password='" + upwd + "'";
        // 执行sql 获得结果
        rs = stmt.executeQuery(sql);
        // 处理结果
        while (rs.next()) {
            int id = rs.getInt("id");
            String username = rs.getString("username");
            System.out.println(id + ">>>>" + username);
        }
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        // 释放资源
        JDBCUtils.release(conn, stmt, rs);
    }

}
```