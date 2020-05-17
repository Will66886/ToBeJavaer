# JDBC基础

## jdbc查询

```java
package com.shary.test;

import java.sql.*;

public class TestJDBC{
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //防止出现乱码
        String url = "jdbc:mysql://localhost:3306/mybatisdb?useUnicode=true&characterEncoding=gbk";
        String username = "root";
        String password = "123456";
//        1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");
//        2.连接数据库
        Connection connection = DriverManager.getConnection(url,username,password);
//        3.向数据库发送SQL的对象statement ：CRUD
        Statement statement = connection.createStatement();
//        4.编写SQL
        String sql = "select * from user";
//        connection.prepareStatement(sql);安全，预编译
//        5.执行查询SQL，返回一个ResultSet :结果集
        ResultSet resultSet = statement.executeQuery(sql);
        while (resultSet.next()){
            System.out.println("id="+resultSet.getObject("id"));
            System.out.println("name="+resultSet.getObject("name"));
            System.out.println("pwd="+resultSet.getObject("pwd"));
        }
//        6.关闭连接，释放资源
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

## jdbc删除

```java
package com.shary.test;

import java.sql.*;

public class TestJDBC{
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        String url = "jdbc:mysql://localhost:3306/mybatisdb?useUnicode=true&characterEncoding=gbk";
        String username = "root";
        String password = "123456";
//        1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");
//        2.连接数据库
        Connection connection = DriverManager.getConnection(url,username,password);
//        3.向数据库发送SQL的对象statement ：CRUD
        Statement statement = connection.createStatement();
//        4.编写SQL
//        String sql = "select * from user";
        String sql = "delete from user where id = '3'";
//        connection.prepareStatement(sql);安全，预编译
//        5.执行查询SQL，返回一个ResultSet :结果集
        int i = statement.executeUpdate(sql);
        System.out.println("受影响的行数"+i);
        statement.close();
        connection.close();
    }
}
```

## jdbc预编译

```java
package com.shary.test;

import java.sql.*;

public class TestJDBC{
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        String url = "jdbc:mysql://localhost:3306/mybatisdb?useUnicode=true&characterEncoding=gbk";
        String username = "root";
        String password = "123456";
//        1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");
//        2.连接数据库
        Connection connection = DriverManager.getConnection(url,username,password);
//        3.编写SQL
        String sql = "insert into user (id,name,pwd) values (?,?,?);";
//        4.预编译
        PreparedStatement statement = connection.prepareStatement(sql);
        statement.setInt(1,4);
        statement.setString(2,"一一");
        statement.setString(3,"123456");

        int i = statement.executeUpdate();
        System.out.println("受影响行数"+i);
        statement.close();
        connection.close();
    }
}
```

## jdbc事务管理

**事务**

要么都成功，要么都失败！

ACID原则：保证数据的安全。

```java
开启事务
事务提交  commit()
事务回滚  rollback()
关闭事务

转账：
A:1000
B:1000
    
A(900)   --100-->   B(1100) 
```



```java
package com.shary.test;

import java.sql.*;

public class TestJDBC{
    public static void main(String[] args) throws ClassNotFoundException {
        String url = "jdbc:mysql://localhost:3306/mybatisdb?useUnicode=true&characterEncoding=gbk";
        String username = "root";
        String password = "123456";
//        1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");
//        2.连接数据库
        Connection connection = null;
        try {
            connection = DriverManager.getConnection(url,username,password);
//        3.开启事务
            connection.setAutoCommit(false);
            String sql = "update employeetest set money = money-100 where name = 'A';";
            connection.prepareStatement(sql).executeUpdate();

//        4.制造错误
            int i = 1/0;

            String sql2 = "update employeetest set money = money-100 where name = 'B';";
            connection.prepareStatement(sql2).executeUpdate();
            connection.commit();
            System.out.println("success");

        }catch (SQLException e){
            try {
                connection.rollback();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
            e.printStackTrace();
        }finally {
            try {
                connection.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

    }
}
```

