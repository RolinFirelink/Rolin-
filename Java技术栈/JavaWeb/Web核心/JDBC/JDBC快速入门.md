学习完了MySQL数据库之后，本章内容我们来学习JDBC，首先我们先对JDBC做一个简单的介绍

- JDBC的概念

首先JDBC其全称是Java DataBase Connectivity java数据库连接，其是一种用于执行SQL语句的javaAPI，可以为多种关系型数据库提供统一访问且其是由一组Java语言编写的类和接口组成的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe4b868139285b1e83ccda3d184d2b104.png)

简单来说，就是我们可以通过JDBC来去操作各种不同的数据库，这样就省去了我们换不同的数据库时还要学习的数据库的操作的成本，这就是JDBC的好处

- JDBC快速入门

接下来我们就用我们的idea来使用我们的JDBC做一个快速入门的小程序，注意，我们这里只是一个快速入门，因此对于各种方法，我们先不做解释，只是使用。后续正式学习的时候我们再来讲解

先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3e5812643ad39451ba042947645f52ef.png)

我们这里首先要导入jar包，因为我们的JDBC虽然的确有这么一套规范，但是具体的实现是由各个数据库的厂家自己实现的，因此我们要导入对应的jar包

那么我们可以写入代码如下

```
package com.itheima01;

import java.sql.*;

public class JDBCDemo01 {
    public static void main(String[] args) throws Exception {
        //1.导入jar包

        //2.注册驱动
        Class.forName("com.mysql.jdbc.Driver");

        //3.获取连接,输入远程连接，用户名以及密码获得连接对象
        Connection con = DriverManager.getConnection("jdbc:mysql://175.178.114.158:3306/db2","root","itheima");

        //4.通过连接对象获取执行者对象
        Statement stat = con.createStatement();

        //5.执行sql语句，并且调用executeQuery()方法接受结果
        String sql = "SELECT * FROM user";
        ResultSet rs = stat.executeQuery(sql);

        //6.利用rs对象的next()方法循环处理结果
        while (rs.next()){
            System.out.println(rs.getInt("id")+"\t"+rs.getString("name"));
        }

        //7.释放资源,万物皆准的close方法
        con.close();
        stat.close();
        con.close();
    }
}

```

具体的解释都写在注释上了，自己去看吧