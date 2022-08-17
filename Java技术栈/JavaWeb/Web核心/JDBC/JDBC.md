# JDBC快速入门

学习完了MySQL数据库之后，本章内容我们来学习JDBC，首先我们先对JDBC做一个简单的介绍

## JDBC的概念

首先JDBC其全称是Java DataBase Connectivity java数据库连接，其是一种用于执行SQL语句的javaAPI，可以为多种关系型数据库提供统一访问且其是由一组Java语言编写的类和接口组成的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132043.png)

简单来说，就是我们可以通过JDBC来去操作各种不同的数据库，这样就省去了我们换不同的数据库时还要学习的数据库的操作的成本，这就是JDBC的好处

## JDBC入门程序

接下来我们就用我们的idea来使用我们的JDBC做一个快速入门的小程序，注意，我们这里只是一个快速入门，因此对于各种方法，我们先不做解释，只是使用。后续正式学习的时候我们再来讲解

先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132044.png)

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

# JDBC功能类详解

## DriverManager

那么我们接着就来介绍我们的各个功能类，首先我们来介绍我们的注册驱动的DriverManager方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132045.png)

首先我们注册驱动时应该要使用static void register Driver(Driver driver)方法，其会返回一个DriverManager对象，但是我们容易注意到，我们实际的代码里，注册驱动是调用得Class.forName()方法并写入com.mysql.jdbc.Driver，这是怎么回事呢？这是因为在com.mysql.jdbc.Driver下存在静态代码块，这个代码块就是我们注册驱动所需要的代码，我们只要让这个类执行，其静态代码块就会自动执行，因此我们可以通过Class.forName()的方式来注册驱动

最后是在mysql5之后可以省略这一步骤，因为在jar包中存在对应的配置文件，但我们这里处于初学阶段，因此还是采用手动方式来进行注册

然后我们来看看DriverManager驱动管理对象本身，其本身要先调用数据库连接对象，需要输入指定连接的url，其是采用固定前缀接远程ip地址接端口号再接数据库名称的方式来指定，然后是要输入用户名和密码，用/分隔开来。该方法会返回一个Connection数据库连接对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132046.png)

## Connection

Connection数据库执行对象，其可以调用对应的方法获取普通执行者对象和预编译执行者对象，普通执行者对象就是用于执行我们的sql语句的，而预编译执行者对象我们后面会介绍。其次是我们可以管理事务，开启事务只要调用setAutoCommit()方法，传入一个true的参数就能开启事务了，然后可以调用commit()方法提交事务，还可以调用rollback()回滚事务，当然，我们还有close的释放资源的方法，千万不能忘记的是要释放我们的对应资源，防止资源的占用和浪费

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132047.png)

## Statement

Connection数据库连接对象可以获取普通执行者对象Statement，我们接下来就来重点讲下这个普通执行者对象。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132048.png)

Statement对象可以执行两大类的语句，分别是增删改的DML语句，此时需要调用int executeUpdate(String sql)，其返回值是int，该int类型返回的是影响的行数，接着我们可以在传入的String类型的参数里传入对应的执行语句。第二个是可以执行用于查找的DQL语句，其调用ResultSet executeQuery()方法，其返回值是ResultSet，这个对象里面含有我们查询到的信息，但是对其进行了对应的封装，同样也是传入对应的sql字符串来执行语句。最后也是需要释放资源

## ResultSet

然后我们来讲讲ResultSet结果集对象，其下有next()方法，该方法的作用是判断还有没有数据，有就返回true并将索引向下移动一行，反之则返回false。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132049.png)

我们要获取结果集中的数据是要通过对应的getXxx方法的，Xxx代表我们获取的量的数据类型，其会返回对应的数据类型，我们要输入对应的列名就可以获取到其对应的数据，最后这个也是要释放资源

其获取资源的方式非常像我们集合里的迭代器的方式

# JDBC案例

学完了相应的知识之后，接下来我们来学习对应的JDBC的案例，通过这个案例来加深我们对JDBC的使用理解

## 需求介绍和数据准备

先来看看我们的案例需求

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132050.png)

来完成这些之前，我们需要一些简单的数据准备，其准备数据的代码如下

```
-- 创建db14数据库
        CREATE DATABASE db14;

        -- 使用db14数据库
        USE db14;


        -- 创建student表
        CREATE TABLE student(
        sid INT PRIMARY KEY AUTO_INCREMENT,    -- 学生id
        NAME VARCHAR(20),        -- 学生姓名
        age INT,            -- 学生年龄
        birthday DATE          -- 学生生日
        );

        -- 添加数据
        INSERT INTO student VALUES (NULL,'张三',23,'1999-09-23'),(NULL,'李四',24,'1998-08-10'),
        (NULL,'王五',25,'1996-06-06'),(NULL,'赵六',26,'1994-10-20');
```

做好了对应的数据之后我们要在java中创建一个对应的学生类

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132051.png)

注意我们这里学生类的变量要跟我们数据中的学生类对象的属性保持一致，而且如果其会基本属性，我们就要使用包装类来代替，因为我们读取出的值有可能是null，此时为了避免抛出异常需要使用包装类来承接这个数据。然后我们这里要模拟真实的项目构建，所以我们要创造三层来调用我们的代码

具体的创建过程就不演示了，这里我们权当已经创建好了

## 查询所有学生信息

- 需求一查询所有学生信息

先来看看我们的所有需求

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132052.png)

我们本节就来实现我们的需求一，我们首先要在对应的service，dao，controller层创建对应的接口类并写入一些方法，在dao层下我们先给我们的方法进行指定，写入其代码如下

```
package com.itheima02.dao;

import com.itheima02.domain.Student;

import java.util.ArrayList;

/*
    Dao层接口
 */
public interface StudentDao {
    //查询所有学生信息
    public abstract ArrayList<Student> findAll();

    //条件查询，根据id获取学生信息
    public abstract Student findById(Integer id);

    //新增学生信息
    public abstract int insert(Student stu);

    //修改学生信息
    public abstract int update(Student stu);

    //删除学生信息
    public abstract int delete(Integer id);
}

```

这里我们采用的是抽象类，然后我们在service层也写入对应的代码如下

```
package com.itheima02.service;

import com.itheima02.domain.Student;

import java.util.ArrayList;

/*
    Service层接口
 */
public interface StudentService {
    //查询所有学生信息
    public abstract ArrayList<Student> findAll();

    //条件查询，根据id获取学生信息
    public abstract Student findById(Integer id);

    //新增学生信息
    public abstract int insert(Student stu);

    //修改学生信息
    public abstract int update(Student stu);

    //删除学生信息
    public abstract int delete(Integer id);
}
```

其实差不多，不过service层我们后续还用得上就是，注意上面两个抽象类，而不是实现类

然后我们创建一个Dao层的实现类并实现其对应的方法，我们可以写入第一个具体实现的方法的代码如下

```
/*
    查询所有学生信息
*/
@Override
public ArrayList<Student> findAll() {
    ArrayList<Student> list = new ArrayList<>();
    Connection con = null;
    Statement stat = null;
    ResultSet rs = null;

    try {
        //1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取数据库连接
        con = DriverManager.getConnection("jdbc:mysql://175.178.114.158:3306/db14","root","itheima");
        //3.获取执行者对象
        stat = con.createStatement();
        //4.执行sql语句，并且接受返回的结果集
        String sql = "SELECT * FROM student";
        rs = stat.executeQuery(sql);
        //5.处理结果集
        while (rs.next()){
            int sid = rs.getInt("sid");
            String name = rs.getString("name");
            int age = rs.getInt("age");
            Date birthday = rs.getDate("birthday");

            //封装Student对象
            Student stu = new Student(sid, name, age, birthday);

            //将student对象保存到集合中
            list.add(stu);
        }
    }catch (Exception e){
        e.printStackTrace();
    }finally {
        //6.释放资源
        if(con!=null) {
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(stat!=null){
            try {
                stat.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
    return list;
}
```

然后我们同样在service层上也创建这么一个实现类，直接调用Dao层的方法然后返回该集合，然后到controller层我们具体对这个集合进行打印处理

```
import org.junit.Test;

import java.util.ArrayList;

public class StudentController{
    private StudentService service = new StudentServiceImpl();
    /*
        查询所有学生信息
     */
    @Test
    public void findAll() {
        ArrayList<Student> list = service.findAll();
        for (Student stu:list) {
            System.out.println(stu);
        }
    }
```

注意这里我们需要自己引入Test的jar包类，不然我们无法进行对应的测试

那么到此为止我们的查询所有学生的功能就实现完毕了

## 根据id查询学生信息

当我们实现了第一个功能之后，之后的功能就很好实现了，因为很多代码都是十分相似的，我们只需要复制然后稍微改改就完了，请看我们第二个方法的dao层代码

```
/*
    条件查询，根据id查询学生信息
 */
@Override
public Student findById(Integer id) {
    Student stu = new Student();
    Connection con = null;
    Statement stat = null;
    ResultSet rs = null;

    try {
        //1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取数据库连接
        con = DriverManager.getConnection("jdbc:mysql://175.178.114.158:3306/db14","root","itheima");
        //3.获取执行者对象
        stat = con.createStatement();
        //4.执行sql语句，并且接受返回的结果集
        String sql = "SELECT * FROM student WHERE sid='"+id+"'";
        rs = stat.executeQuery(sql);
        //5.处理结果集
        while (rs.next()){
            int sid = rs.getInt("sid");
            String name = rs.getString("name");
            int age = rs.getInt("age");
            Date birthday = rs.getDate("birthday");
            stu.setSid(sid);
            stu.setName(name);
            stu.setAge(age);
            stu.setBirthday(birthday);
        }
        return stu;
    }catch (Exception e){
        e.printStackTrace();
    }finally {
        //6.释放资源
        if(con!=null) {
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(stat!=null){
            try {
                stat.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
    return stu;
}
```

然后我们在service层调用，最后在Controller层进行对应的测试。我们这里因为是查询一个学生对象，所以我们除去了集合，还有更改了一下查询语句，没了

## 添加学生信息

接下来我们来实现添加学生信息的功能，我们可以在Dao层写入代码如下

```
/*
    添加学生信息
 */
@Override
public int insert(Student stu) {
    Connection con = null;
    Statement stat = null;
    int result = 0;
    try {
        //1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取数据库连接
        con = DriverManager.getConnection("jdbc:mysql://175.178.114.158:3306/db14","root","itheima");
        //3.获取执行者对象
        stat = con.createStatement();

        //4.执行sql语句，并且接受返回的结果集
        Date d = stu.getBirthday();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        String birthday = sdf.format(d);

        String sql = "INSERT INTO student VALUES ('"+stu.getSid()+"','"+stu.getName()+"','"+stu.getAge()+"','"+birthday+"')";
        result = stat.executeUpdate(sql);
    }catch (Exception e){
        e.printStackTrace();
    }finally {
        //6.释放资源
        if(con!=null) {
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(stat!=null){
            try {
                stat.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
    return result;
}
```

这里我们是要添加学生对象，我们会一个int数字，该数字代表本次添加影响的行数，注意我们这里将我们的日期格式进行了改造以令其符合我们的SQL中的语法格式，另外我们分割的方式都是先构造一个大括号，内部先填入SQL中会填入的两个''，然后我们在这两个''中间输入"++"，在++的中间调用对应的命令获得我们想要的数据，通过这种方式传入我们的数据

由于是添加资源，因此我们需要调用的是executeUpdate方法

## 修改学生信息

修改学生信息的代码和添加学生信息的几乎一模一样，我们可以现在Dao层写入代码如下

```
/*
    修改学生
 */
@Override
public int update(Student stu) {
    Connection con = null;
    Statement stat = null;
    int result = 0;
    try {
        //1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取数据库连接
        con = DriverManager.getConnection("jdbc:mysql://175.178.114.158:3306/db14","root","itheima");
        //3.获取执行者对象
        stat = con.createStatement();

        //4.执行sql语句，并且接受返回的结果集
        Date d = stu.getBirthday();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        String birthday = sdf.format(d);

        String sql = "UPDATE student SET sid='"+stu.getSid()+"',name='"+stu.getName()+"',age='"+stu.getAge()+"',birthday='"+birthday+"' WHERE sid='"+stu.getSid()+"'";
        result = stat.executeUpdate(sql);
    }catch (Exception e){
        e.printStackTrace();
    }finally {
        //6.释放资源
        if(con!=null) {
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(stat!=null){
            try {
                stat.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
    return result;
}
```

然后在对应的service层上调用，接着在controller层上测试发现也没有问题。我们这里修改的就只是我们的sql语句而已

## 删除学生信息

最后我们来实现删除学生信息的需求，这个需求其实是最简单的，就是拼接一个id就完了，请看Dao层代码

```
/*
    删除学生信息
 */
@Override
public int delete(Integer id) {
    Connection con = null;
    Statement stat = null;
    int result = 0;
    try {
        //1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取数据库连接
        con = DriverManager.getConnection("jdbc:mysql://175.178.114.158:3306/db14","root","itheima");
        //3.获取执行者对象
        stat = con.createStatement();

        //4.执行sql语句，并且接受返回的结果集

        String sql = "DELETE FROM student WHERE sid='"+id+"'";
        result = stat.executeUpdate(sql);
    }catch (Exception e){
        e.printStackTrace();
    }finally {
        //6.释放资源
        if(con!=null) {
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(stat!=null){
            try {
                stat.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
    return result;
}
```

那么最后我们放下service层实现类的代码

```
package com.itheima02.service;

import com.itheima02.dao.StudentDao;
import com.itheima02.dao.StudentDaoImpl;
import com.itheima02.domain.Student;

import java.util.ArrayList;

public class StudentServiceImpl implements StudentService{
    private StudentDao dao = new StudentDaoImpl();
    /*
        查询所有学生信息
     */
    @Override
    public ArrayList<Student> findAll() {
        return dao.findAll();
    }

    @Override
    public Student findById(Integer id) {
        return dao.findById(id);
    }

    @Override
    public int insert(Student stu) {
        return dao.insert(stu);
    }

    @Override
    public int update(Student stu) {
        return dao.update(stu);
    }

    @Override
    public int delete(Integer id) {
        return dao.delete(id);
    }
}

```

然后是controller层的代码

```
package com.itheima02.controller;

import com.itheima02.domain.Student;
import com.itheima02.service.StudentService;
import com.itheima02.service.StudentServiceImpl;
import org.junit.Test;

import java.util.ArrayList;
import java.util.Date;

public class StudentController{
    private StudentService service = new StudentServiceImpl();
    /*
        查询所有学生信息
     */
    @Test
    public void findAll() {
        ArrayList<Student> list = service.findAll();
        for (Student stu:list) {
            System.out.println(stu);
        }
    }

    /*
        条件查询，根据id查询学生信息
     */
    @Test
    public void findById(){
        Student stu = service.findById(3);
        System.out.println(stu);
    }

    /*
        添加学生信息
     */
    @Test
    public void insert(){
        Student stu = new Student(5, "周七", 27, new Date());
        int result = service.insert(stu);
        if(result!=0){
            System.out.println("添加成功");
        }else {
            System.out.println("添加失败");
        }
    }

    /*
        修改学生信息
     */
    @Test
    public void update() {
        Student stu = service.findById(5);
        stu.setName("周七七");

        int result = service.update(stu);
        if(result!=0){
            System.out.println("修改成功");
        }else {
            System.out.println("修改失败");
        }
    }

    /*
        删除学生信息
     */
    @Test
    public void delete() {
        int result = service.delete(5);

        if(result!=0){
            System.out.println("删除成功");
        }else {
            System.out.println("删除失败");
        }
    }
}

```

最后经过测试我们会发现我们的功能都实现成功了，那么我们的这个案例就算是实现了

# JDBC工具类

本章节我们来学习编写JDBC工具类，因为我们Dao层的代码很多都重复使用了，代码复用的效率不高，因此我们要将相同的代码尽可能地将其编写成工具类，以实现代码复用

## 配置文件的编写

编写配置文件，首先我们要创建对应的配置文件并写入如图所示的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132053.png)

那么待会我们的工具类，就是来读取这个配置文件来操作的

## 工具类的编写

我们在对应的包下创建工具类，然后我们写入代码如下

```
package com.itheima02.utils;

import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

/*
    JDBC工具类
 */
public class JDBCUtils {
    //1.私有构造方法
    private JDBCUtils(){}

    //2.声明所需要的配置变量
    private static String driverClass;
    private static String url;
    private static String username;
    private static String password;
    private static Connection con;

    //3.提供静态代码块，读取配置文件的信息为变量赋值，注册驱动
    static{
        try {
            //读取配置文件的信息为变量赋值
            InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("config.properties");
            Properties prop = new Properties();
            prop.load(is);

            driverClass=prop.getProperty("driverClass");
            url = prop.getProperty("url");
            username = prop.getProperty("username");
            password = prop.getProperty("password");

            //注册驱动
            Class.forName(driverClass)
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    //4.提供获取数据库连接方法
    public static Connection getConnection(){
        try {
            con = DriverManager.getConnection(url,username,password);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return con;
    }

    //5.提供释放资源的方法
    public static void close(Connection con, Statement stat, ResultSet rs){
        if(con!=null){
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

        if(stat!=null){
            try {
                stat.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }

    //5.提供释放资源的方法
    public static void close(Connection con, Statement stat{
        if(con!=null){
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

        if(stat!=null){
            try {
                stat.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}

```

我们首先提供了一个构造方法，这个构造方法可以让我们的类初始化，然后先声明我们所需要使用到的对象为我们的这个类的成员变量，接着我们这里使用静态代码块，注意静态代码块内的代码在类被初始化时就会自动执行，在静态代码块中结合我们的配置文件将我们的成员变量都赋予对应的值，然后自动完成了注册驱动的动作，也就是说，我们的这个类一初始化，我们的注册动作就完成了

然后我们提供数据库的链接方法，在其中链接数据库并返回一个连接对象

最后我们提供一个释放资源的方法，在其中会进行资源的释放，由于rs对象并不是每一次都有的，因此我们还提供一个重载方法，这个重载方法里rs是不存在的。

## 优化学生案例

那么有了对应的工具类之后，我们可以将我们的案例优化如下

```
package com.itheima02.dao;

import com.itheima02.domain.Student;
import com.itheima02.utils.JDBCUtils;

import java.util.Date;
import java.sql.*;
import java.text.SimpleDateFormat;
import java.util.ArrayList;

public class StudentDaoImpl implements StudentDao{
    /*
        查询所有学生信息
    */
    @Override
    public ArrayList<Student> findAll() {
        ArrayList<Student> list = new ArrayList<>();
        Connection con = null;
        Statement stat = null;
        ResultSet rs = null;

        try {
            con = JDBCUtils.getConnection();

            con = DriverManager.getConnection("jdbc:mysql://175.178.114.158:3306/db14","root","itheima");
            //3.获取执行者对象
            stat = con.createStatement();
            //4.执行sql语句，并且接受返回的结果集
            String sql = "SELECT * FROM student";
            rs = stat.executeQuery(sql);
            //5.处理结果集
            while (rs.next()){
                int sid = rs.getInt("sid");
                String name = rs.getString("name");
                int age = rs.getInt("age");
                Date birthday = rs.getDate("birthday");

                //封装Student对象
                Student stu = new Student(sid, name, age, birthday);

                //将student对象保存到集合中
                list.add(stu);
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
            JDBCUtils.close(con,stat,rs);
        }
        return list;
    }

    /*
        条件查询，根据id查询学生信息
     */
    @Override
    public Student findById(Integer id) {
        Student stu = new Student();
        Connection con = null;
        Statement stat = null;
        ResultSet rs = null;

        try {

            con = JDBCUtils.getConnection();

            //3.获取执行者对象
            stat = con.createStatement();
            //4.执行sql语句，并且接受返回的结果集
            String sql = "SELECT * FROM student WHERE sid='"+id+"'";
            rs = stat.executeQuery(sql);
            //5.处理结果集
            while (rs.next()){
                int sid = rs.getInt("sid");
                String name = rs.getString("name");
                int age = rs.getInt("age");
                Date birthday = rs.getDate("birthday");
                stu.setSid(sid);
                stu.setName(name);
                stu.setAge(age);
                stu.setBirthday(birthday);
            }
            return stu;
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
            JDBCUtils.close(con,stat,rs);
        }
        return stu;
    }

    /*
        添加学生信息
     */
    @Override
    public int insert(Student stu) {
        Connection con = null;
        Statement stat = null;
        int result = 0;
        try {
            //1.注册驱动
            con=JDBCUtils.getConnection();

            //3.获取执行者对象
            stat = con.createStatement();

            //4.执行sql语句，并且接受返回的结果集
            Date d = stu.getBirthday();
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            String birthday = sdf.format(d);

            String sql = "INSERT INTO student VALUES ('"+stu.getSid()+"','"+stu.getName()+"','"+stu.getAge()+"','"+birthday+"')";
            result = stat.executeUpdate(sql);
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
            JDBCUtils.close(con,stat);
        }
        return result;
    }

    /*
        修改学生
     */
    @Override
    public int update(Student stu) {
        Connection con = null;
        Statement stat = null;
        int result = 0;
        try {
            con=JDBCUtils.getConnection();
            //3.获取执行者对象
            stat = con.createStatement();

            //4.执行sql语句，并且接受返回的结果集
            Date d = stu.getBirthday();
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            String birthday = sdf.format(d);

            String sql = "UPDATE student SET sid='"+stu.getSid()+"',name='"+stu.getName()+"',age='"+stu.getAge()+"',birthday='"+birthday+"' WHERE sid='"+stu.getSid()+"'";
            result = stat.executeUpdate(sql);
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
            JDBCUtils.close(con,stat);
        }
        return result;
    }

    /*
        删除学生信息
     */
    @Override
    public int delete(Integer id) {
        Connection con = null;
        Statement stat = null;
        int result = 0;
        try {
            con=JDBCUtils.getConnection();
            //3.获取执行者对象
            stat = con.createStatement();

            //4.执行sql语句，并且接受返回的结果集

            String sql = "DELETE FROM student WHERE sid='"+id+"'";
            result = stat.executeUpdate(sql);
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
            JDBCUtils.close(con,stat);
        }
        return result;
    }
}
```

可以看到代码量少了非常多，很不错，我很喜欢

- 学生表操作整合界面

最后我们实现了JDBC的操作页面之后，我们只要将这些系统和前端的一些网站结合起来接受前端输入的用户数据就可以了，这里就不演示了，我们到此为止其实已经见得多了

# SQL注入攻击

所谓注入攻击，其意思是利用SQL的语句的漏洞，通过SQL语句来破坏我们系统的原有逻辑，我们本章就做一个简单的对注入攻击的了解

## SQL注入攻击的演示

比方说，假设我们有下面所示的这么一份代码，这份代码的主要作用就是令用户登录

```java
@Override
public User findByLoginNameAndPassword(String loginName, String password) {
    //定义必要信息
    Connection conn = null;
    Statement st = null;
    ResultSet rs = null;
    User user = null;
    try {
        //1.获取连接
        conn = JDBCUtils.getConnection();
        //2.定义SQL语句
        String sql = "SELECT * FROM user WHERE loginname='"+loginName+"' AND password='"+password+"'";
        System.out.println(sql);
        //3.获取操作对象，执行sql语句，获取结果集
        st = conn.createStatement();
        rs = st.executeQuery(sql);
        //4.获取结果集
        if (rs.next()) {
            //5.封装
            user = new User();
            user.setUid(rs.getString("uid"));
            user.setUcode(rs.getString("ucode"));
            user.setUsername(rs.getString("username"));
            user.setPassword(rs.getString("password"));
            user.setGender(rs.getString("gender"));
            user.setDutydate(rs.getDate("dutydate"));
            user.setBirthday(rs.getDate("birthday"));
            user.setLoginname(rs.getString("loginname"));
        }
        //6.返回
        return user;
    }catch (Exception e){
        throw new RuntimeException(e);
    }finally {
        JDBCUtils.close(conn,st,rs);
    }
}
```

那么我们这份代码的主要问题出现哪里呢？主要问题出现在Statement对象中，在正常情况中，我们用户输入的账号和密码都应该是账号和密码，然而，如果用户输入在密码窗口里输入的是一条sql语句，那么就可能出现令输入的sql语句执行的情况，比如其输入bbb' or '1'='1'，这样的语句，其那么其在Mysql中执行的语句是这样的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132054.png)

这样的语句，虽然前面的部分无法查找出任何数据，然而or后面的数据却可以执行，这样就导致了即使用户输入了错误的密码，其仍然可以登录，这就是注入攻击。其出现问题的本质原因是Statement对象在执行sql语句时，将密码的一部分内容当查询条件来执行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132055.png)

## SQL注入攻击的解决

那么现在我们了解了问题的所在了，接下来我们当然是要学习如何解决问题，这时解决问题，就要依赖我们的预编译执行者对象PreparedStatement了，具体解释请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132056.png)

那么我们可以将我们的代码改造如下

```
public class UserDaoImpl implements UserDao {

    /*
        使用PreparedStatement登陆方法，解决注入攻击
     */
    @Override
    public User findByLoginNameAndPassword(String loginName, String password) {
        //定义必要信息
        Connection conn = null;
        PreparedStatement st = null;
        ResultSet rs = null;
        User user = null;
        try {
            //1.获取连接
            conn = JDBCUtils.getConnection();
            //2.定义SQL语句
            String sql = "SELECT * FROM user WHERE loginname=? AND password=?";
            //3.获取操作对象，执行sql语句，获取结果集
            st = conn.prepareStatement(sql);
            st.setString(1,loginName);
            st.setString(2,password);

            rs = st.executeQuery();

            //4.获取结果集
            if (rs.next()) {
                //5.封装
                user = new User();
                user.setUid(rs.getString("uid"));
                user.setUcode(rs.getString("ucode"));
                user.setUsername(rs.getString("username"));
                user.setPassword(rs.getString("password"));
                user.setGender(rs.getString("gender"));
                user.setDutydate(rs.getDate("dutydate"));
                user.setBirthday(rs.getDate("birthday"));
                user.setLoginname(rs.getString("loginname"));
            }
            //6.返回
            return user;
        }catch (Exception e){
            throw new RuntimeException(e);
        }finally {
            JDBCUtils.close(conn,st,rs);
        }
    }
```

先看第17行，可以看到我们这里采用预编译对象，我们将所有的数据都先用占位符?来代替，然后我们看第19行，我们这里先调用预编译对象的prepareStatement方法先确定了我们的sql语句的格式，然后我们调用两个set方法对占位符赋予固定格式的内容，最后我们直接调用查询方法就可以正确查询了。我们以后的实现任何编译都是采用预编译对象的方式的

那么最后我们可以将我们之前的代码全部替换为预编译对象的形式来去实现，其代码如下

```
package com.itheima.dao.impl;

import com.itheima.dao.StudentDao;
import com.itheima.domain.Student;
import com.itheima.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;

/**
 * @author 黑马程序员
 * @Company http://www.itheima.com
 */
public class StudentDaoImpl implements StudentDao {

    @Override
    public ArrayList<Student> findAll() {
        //定义必要信息
        Connection conn = null;
        PreparedStatement pstm = null;
        ResultSet rs = null;
        ArrayList<Student> students = null;
        try {
            //1.获取连接
            conn = JDBCUtils.getConnection();
            //2.获取操作对象
            pstm = conn.prepareStatement("select * from student");
            //3.执行sql语句，获取结果集
            rs = pstm.executeQuery();
            //4.遍历结果集
            students = new ArrayList<Student>();
            while (rs.next()) {
                //5.封装
                Student student = new Student();
                student.setSid(rs.getInt("sid"));
                student.setName(rs.getString("name"));
                student.setAge(rs.getInt("age"));
                student.setBirthday(rs.getDate("birthday"));
                //加入到集合中
                students.add(student);
            }
            //6.返回
            return students;
        }catch (Exception e){
            throw new RuntimeException(e);
        }finally {
            JDBCUtils.close(conn,pstm,rs);
        }
    }

    @Override
    public Student findById(Integer sid) {
        //定义必要信息
        Connection conn = null;
        PreparedStatement pstm = null;
        ResultSet rs = null;
        Student student = null;
        try {
            //1.获取连接
            conn = JDBCUtils.getConnection();
            //2.获取操作对象
            pstm = conn.prepareStatement("select * from student where sid = ? ");
            pstm.setInt(1,sid);
            //3.执行sql语句，获取结果集
            rs = pstm.executeQuery();
            //4.遍历结果集
            if (rs.next()) {
                //5.封装
                student = new Student();
                student.setSid(rs.getInt("sid"));
                student.setName(rs.getString("name"));
                student.setAge(rs.getInt("age"));
                student.setBirthday(rs.getDate("birthday"));
            }
            //6.返回
            return student;
        }catch (Exception e){
            throw new RuntimeException(e);
        }finally {
            JDBCUtils.close(conn,pstm,rs);
        }
    }

    @Override
    public int insert(Student student) {
        //定义必要信息
        Connection conn = null;
        PreparedStatement pstm = null;
        int result = 0;
        try {
            //1.获取连接
            conn = JDBCUtils.getConnection();
            //2.获取操作对象
            pstm = conn.prepareStatement("insert into student(sid,name,age,birthday)values(null,?,?,?)");
            //3.设置参数
            //pstm.setInt(1,null);
            pstm.setString(1,student.getName());
            pstm.setInt(2,student.getAge());
            pstm.setDate(3,new Date(student.getBirthday().getTime()));
            //4.执行sql语句
            result = pstm.executeUpdate();
        }catch (Exception e){
            throw new RuntimeException(e);
        }finally {
            JDBCUtils.close(conn,pstm);
        }
        return result;
    }

    @Override
    public int update(Student student) {
        //定义必要信息
        Connection conn = null;
        PreparedStatement pstm = null;
        int result = 0;
        try {
            //1.获取连接
            conn = JDBCUtils.getConnection();
            //2.获取操作对象
            pstm = conn.prepareStatement("update student set name=?,age=?,birthday=? where sid=? ");
            //3.设置参数
            pstm.setString(1,student.getName());
            pstm.setInt(2,student.getAge());
            pstm.setDate(3,new Date(student.getBirthday().getTime()));
            pstm.setInt(4,student.getSid());
            //4.执行sql语句
            result = pstm.executeUpdate();
        }catch (Exception e){
            throw new RuntimeException(e);
        }finally {
            JDBCUtils.close(conn,pstm);
        }
        return result;
    }

    @Override
    public int delete(Integer sid) {
        //定义必要信息
        Connection conn = null;
        PreparedStatement pstm = null;
        int result = 0;
        try {
            //1.获取连接
            conn = JDBCUtils.getConnection();
            //2.获取操作对象
            pstm = conn.prepareStatement("delete from student where sid=? ");
            //3.设置参数
            pstm.setInt(1,sid);
            //4.执行sql语句
            result = pstm.executeUpdate();
        }catch (Exception e){
            throw new RuntimeException(e);
        }finally {
            JDBCUtils.close(conn,pstm);
        }
        return result;
    }
}

```

这里前面两个方法我都可以自己实现，后面两个方法我自己没整明白到底是个啥情况，老是出错，直接不搞了，有兴趣的自己去看看上面的答案吧，看看它是怎么修改的

# JDBC事务管理

## JDBC管理事务的介绍

之前我们说过，在JDBC里，管理事务的功能类是Connection，该功能类可以执行开启事务，提交事务和回滚事务的动作

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132057.png)

## JDBC管理事务的演示

那么接着我们就来进行JDBC管理事务的演示，首先我们在UserSerice里提供了批量添加的接口，其定义如下

```
    /**
     * 批量添加
     * @param users
     */
    void batchAdd(List<User> users);
}
```

然后我们在对应的Serice层的实现类下写入如下代码

```
/*
    事务要控制在此处
 */
@Override
public void batchAdd(List<User> users) {
    //获取数据库连接对象
    Connection con = JDBCUtils.getConnection();
    try {
        //开启事务
        con.setAutoCommit(false);

        for (User user : users) {
            //1.创建ID,并把UUID中的-替换
            String uid = UUID.randomUUID().toString().replace("-", "").toUpperCase();
            //2.给user的uid赋值
            user.setUid(uid);
            //3.生成员工编号
            user.setUcode(uid);

            //出现异常
            //int n = 1 / 0;

            //4.保存
            userDao.save(con,user);
        }

        //提交事务
        con.commit();

    }catch (Exception e){
        //回滚事务
        try {
            con.rollback();
        } catch (SQLException e1) {
            e1.printStackTrace();
        }
        e.printStackTrace();
    } finally {
        //释放资源
        JDBCUtils.close(con,null);
    }
}
```

这样就可以实现对事务的回滚操作了，我们这里首先在方法中获取数据库的连接对象，然后我们开启事务，接着我们通过循环遍历我们集合中的对象，先给uid和员工编号进行一个值的自动赋，然后我们最后将对应的集合传入到userDao层的添加方法中， 我们在循环结束之后调用提交事务方法，在异常执行中调用回滚事务的方法，这样一旦出现一个异常就会让我们的事务回滚，不出现那就这样呗。最后不要忘了释放我们的资源，没有的对象传入null就完了

这里为什么我们不往Dao层里进行事务的回滚动作的写入呢？这是因为有些操作压根就不需要回滚事务，而Dao层是底层代码，所有的上级代码都需要调用它，如果我们往Dao层上写，那就会变成不管是哪个需求都会出现事务的回滚动作，这显然不是我们所需要的，因此我们这里往service层上写，这样就可以达成我们所需求的部分特定需求使用特定功能的想法了。

# 连接池

之前我们学习的都是JDBC的基础内容，而到了这一章节，我们来正式学习JDBC的高级内容，本章节我们来学习，连接池

## 数据库连接池的概念

首先我们来介绍下什么是数据库连接池，为什么又这个概念。首先数据库连接池简单来说就相当于是线程池，用户向数据库访问时需要从池中获取钥匙，然后访问数据库，访问完毕之后重新将钥匙放回池中。如果没有数据库连接池这个概念，那么用户每次向数据库的访问，java程序都需要新开辟一个空间用于访问，然后再释放这个空间，这样效率太慢了，对数据库的链接的管理是能显著影响到整个应用程序的性能指标的，因此我们提出了数据库连接池的概念，用于提高我们的效率。

**之前我们的案例每次连接数据库都是直接请求数据库创建一个新连接的，效率很低**

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132058.png)

数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接而不是重建建立一个，这样能明显提高数据库操作的性能

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132059.png)

比方说，我们在一开始就往连接池中创建六个钥匙，用户需要访问时就从池中取出一个钥匙出来用于访问，有更多的用户访问就继续取，取完了还要就让他等，等到别人放回去为止或者直接抛出异常，这样能有效提高我们的效率

## DataSource

那么学习了数据库连接池之后，我们接着就来学习如何自定义一个数据库连接池，在学习这个内容之前，我们要先来学习DataSource接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132100.png)

DataSource接口是java官方提供的数据库连接池规范，简单来说就是如果我们要自定义一个数据库连接池，那么我们就必须要令其实现DataSource接口，而这个接口的核心功能是getConnection()，该方法可以返回一个Connection对象。而我们连接池中自然要有保存连接的容器，这个容器我们可以用一个集合来保存。那么我们来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132101.png)

### 自定义数据库连接池

接着我们就来自定义我们的数据库连接池，我们这里先创建一个模块来定义进行我们本章的学习，所以不要忘了要将之前写好的工具类还有配置文件和相关的jar包先导入进来。然后我们自己创建一个类，令其实现DataSource接口，虽然其下的方法很多，但是我们只需要重写Connection方法就可以了，请看代码

```
/*
    自定义数据库连接池
 */
public class MyDataSource implements DataSource {
    //1.准备容器,用于保存多个连接对象,由于需要线程安全的集合对象，因此采用Collections的内置方法获取所需的集合对象
    private static List<Connection> pool = Collections.synchronizedList(new ArrayList<>());

    //2.定义静态代码块,通过工具类获取10个连接对象并放置于实现构造的集合容器中
    static {
        for (int i = 1; i <= 10; i++) {
            Connection con = JDBCUtils.getConnection();
            pool.add(con);
        }
    }

    //3.重写getConnection(),用于获取一个连接对象,每次我们都获取第一个对象
    @Override
    public Connection getConnection() throws SQLException {
        if(pool.size()>0){
            Connection con = pool.remove(0);
            return con;
        }else {
            throw new RuntimeException("连接数量已用尽");
        }
    }

    //4.定义getSize(),获取连接池容器的大小
    public int getSize() {
        return pool.size();
    }
```

下面还有很多的代码，都是重写的方法，这里我们为了版面就不放出来了。我们这里定义了我们的容器，然后我们提供了静态代码块，代码块内会令我们定义的容器获得十个连接对象，然后重写了getConnection方法，该方法可以用于获取连接对象，最后是获得连接池还有几个连接的方法

### 自定义数据库连接池测试

那么为了测试我们自定义的数据库连接池是真的有用，我们可以创建一个类并写入其测试代码入如下

```
package com.itheima01;

import com.itheima.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class MyDataSourceTest {
    public static void main(String[] args) throws Exception{
        //1.创建连接池对象
        MyDataSource dataSource = new MyDataSource();
        //先打印使用之前的连接池的连接数量
        System.out.println("使用之前的数量：" + dataSource.getSize());

        //2.通过连接池对象获取连接对象，并打印该连接对象的字节码文件
        Connection con = dataSource.getConnection();
        System.out.println(con.getClass());

        //3.查询学生表的全部信息，使用预编译执行对象
        String sql = "SELECT * FROM student";
        PreparedStatement pst = con.prepareStatement(sql);

        //4.执行sql语句，接收结果集
        ResultSet rs = pst.executeQuery();

        //5.处理结果集
        while (rs.next()){
            System.out.println(rs.getInt("sid") + "\t" + rs.getString("name") + "\t" + rs.getInt("age") + "\t" + rs.getDate("birthday"));
        }

        //6.释放资源
        rs.close();
        pst.close();
        con.close();

        System.out.println("使用之后的数量：" + dataSource.getSize());
    }
}
```

解释都已经写在里面了，这里就不多赘述了，实际上我们能够轻易知道我们这份代码所构造的连接池是切实可行的，真实有效的。

## 归还连接方式

### 继承方式

但是我们上面的代码有一个问题我们没有解决，就是我们的上面实现的连接池并没有实现归还连接的功能，我们在测试代码里可以看到我们的连接就是用一份少一份的，而且最后也是直接关闭的，因此接下来我们要实现归还连接的功能、

归还连接有四种方式，分别是继承方式、装饰设计模式、适配器设计模式以及动态代理方式。我们这里先来学习继承方式归还数据库连接

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132102.png)

继承方式归还数据库的原理很简单，我们打印连接对象，会发现这个连接实现类是JDBC4Connection，那么我们就可以自定义一个类，令其继承上面的类，然后重写其close()方法实现连接对象的归还，而其他方法不做改动

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132103.png)

那么我们可以创建这么一个类并写入代码如下

```
package com.itheima02;

import com.mysql.jdbc.JDBC4Connection;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;
import java.util.Properties;

/*
    自定义的连接对象
    1.定义一个类，继承JDBC4Connection
    2.定义Connection连接对象和容器对象的成员变量
    3.通过有参构造方法为成员变量赋值
    4.重写close方法，完成归还连接
 */
//1.定义一个类，继承JDBC4Connection
public class MyConnection extends JDBC4Connection {
    //2.定义Connection连接对象和容器对象的成员变量
    private Connection con;
    private List<Connection> pool;

    //3.通过有参构造方法为成员变量赋值
    public MyConnection(String hostToConnectTo, int portToConnectTo, Properties info, String databaseToConnectTo, String url,Connection con,List<Connection> pool) throws SQLException {
        super(hostToConnectTo, portToConnectTo, info, databaseToConnectTo, url);
        this.con = con;
        this.pool = pool;
    }

    //4.重写close方法，完成归还连接
    @Override
    public void close() throws SQLException{
        pool.add(con);
    }
}
```

对于这其中的MyConnection的构造方法，我们加入了con和pool两个对象进去，这个方法的原理我个人的理解是外面的构造方法我们可以随便定义，然而内部的super构造方法是用于构造父类的代码，而父类的构造方法需要传入对应的四个对象，这四个对象是必不可少的，而我们自己定义的对象则可以由我们自己决定赋予给谁或者是不赋予给谁，我们继承的时候其要求我们去实现这个方法，这是因为子类在实例化前必须让父类先实例化，而这里的父类里又只有有参构造器，所以一定要重写构造方法并利用super关键字调用其父类的有参构造器

然后我们在下面重写我们的close方法，完成归还连接，这样我们的继承类就实现完毕了。但是虽然说这个代码实现完毕了，但是这份代码是无法使用的。这是为什么呢？这是因为我们获取的connection连接对象是通过工具类获得的，其内部代码是

```
//4.提供获取数据库连接方法
public static Connection getConnection(){
    try {
        con = DriverManager.getConnection(url,username,password);
    } catch (SQLException throwables) {
        throwables.printStackTrace();
    }
    return con;
}
```

而这里获得的连接对象实际上还是我们的JDBC4Connection连接对象，并不是我们自己定义的类，而我们也无法进行强转，因为在java中只有父类引用指向子类对象，没有子类引用指向父类对象的，所以我们的这份代码只能够用来图一乐

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132104.png)

### 装饰设计模式

归还连接的设计模式的思路是我们自定义一个类，令其实现Connection接口，这样其就具备了和JDBC4Connection相同的功能，然后我们重写其close方法，完成连接的归还，其他的方法就原封不动地调用JDBC4Connection内的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132105.png)

那么我们新创建一个Connection2类令其实现对应的接口并写入其代码如下

```
package com.itheima02;

import java.sql.*;
import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.concurrent.Executor;

/*
    1.定义一个类，实现Connection接口
    2.定义连接对象和连接池容器对象的成员变量
    3.通过有参构造方法为成员变量赋值
    4.重写close方法，完成连接的归还
    5.剩余方法还是调用原有的连接对象中的功能即可
 */
//1.定义一个类，实现Connection接口
public class MyConnection2 implements Connection {

    //2.定义连接对象和连接池容器对象的成员变量
    private Connection con;
    private List<Connection> pool;

    //3.通过有参构造方法为成员变量赋值
    public MyConnection2(Connection con,List<Connection> pool) {
        this.con=con;
        this.pool=pool;
    }

    //4.重写close方法，完成归还连接
    @Override
    public void close() throws SQLException {
        pool.add(con);
    }

    //5.剩余方法还是调用原有的连接对象中的功能即可
    @Override
    public Statement createStatement() throws SQLException {
        return con.createStatement();
    }
```

后面还有很多重写的方法，都是直接调用原来的方法就可以了，这里为了版面就不放上来了。然后我们将我们的从连接池中获取连接的代码修改如下

```
//3.重写getConnection(),用于获取一个连接对象,每次我们都获取第一个对象
@Override
public Connection getConnection() throws SQLException {
    if(pool.size()>0){
        Connection con = pool.remove(0);
        //通过自定义的连接对象 对原有的连接对象进行包装
        MyConnection2 myCon = new MyConnection2(con,pool);
        return myCon;
    }else {
        throw new RuntimeException("连接数量已用尽");
    }
}
```

可以看到我们这里其实返回的是一个Connection的子类连接，该子类连接内含有连接池和连接，如果执行close就会将连接放回连接池中，执行其他方法就直接执行connection的方法。这个方法是可行的，其本质是利用了多态达成的。而我们第一个继承的方式之所以不可以，是因为继承的方式直接改变的是我们的Connection的对象，如果我们要使用的话，那么我们一开始获得的Connection对象就必须是我们所创造的对象，而我们无法获得这个对象，因此我们采用这种有些迂回的方式，虽然绕，但是能用。

但这种方式也不是没有缺点的，那就是这种方式要写入的代码实在是太多了，需要重写很多的父类方法，非常难看，因此我们要对这个代码进行改造

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132106.png)

### 适配器设计模式

适配器设计模式的思想是提供一个适配器类充当中间类，该中间类要实现Connection接口，然后将除了close方法外的所有方法都进行实现，接着我们的自定义的连接类只需要继承这个适配器类，再重写close方法就可以了，当然，这个中间的close()类应该要是抽象类而非接口类，因为在抽象类中才可以同时存在抽象方法和方法，并且可以被继承

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132107.png)

那么我们可以创建一个中间适配器类并写入代码如下

```
package com.itheima02;

import java.sql.*;
import java.util.Map;
import java.util.Properties;
import java.util.concurrent.Executor;

/*
    1.定义一个适配器类，实现Connection接口
    2.定义连接对象的成员变量
    3.通过有参构造为变量赋值
    4.重写所有的抽象方法(除了close)
 */
public abstract class MyAdapter implements Connection {

    //2.定义连接对象的成员变量
    private Connection con;

    //3.通过有参构造为变量赋值
    public MyAdapter(Connection con){
        this.con=con;
    }

    //4.重写所有的抽象方法(除了close)
    @Override
    public Statement createStatement() throws SQLException {
        return con.createStatement();
    }
```

后面还有很多重写的抽象方法，这里为了版面就不放上来了。

然后我们再定义一个类令其连接中间类，写入代码如下

```
package com.itheima02;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

/*
    1.定义一个类，继承适配器类
    2.定义连接对象和连接池容器对象的成员变量
    3.通过有参构造为变量赋值
    4.重写close方法，完成归还连接
 */
//1.定义一个类，继承适配器类
public class MyConnection3 extends MyAdapter{

    //2.定义连接对象和连接池容器对象的成员变量
    private Connection con;
    private List<Connection> pool;

    //3.通过有参构造为变量赋值
    public MyConnection3(Connection con,List<Connection> pool){
        super(con);
        this.con=con;
        this.pool=pool;
    }

    //重写close方法，完成归还连接
    @Override
    public void close() throws SQLException {
        pool.add(con);
    }
}
```

最后我们在连接池类里令其返回MyConnection3就可以了，其实这个本质和我们的第二个方法是差不多的，无非就是多了个中间适配器类让我们的代码一开始点击进去看起来更加舒服些罢了。该有的缺点还是有的，因此还是需要改进

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132108.png)

### 动态代理

那么接着我们就来学习我们的最终的一种归还连接池的方式，这种方式就非常得好，能够让我们不用写这么多重复的代码而又能实现我们所需要的功能，这个方法就是动态代理！再学习之前，我们要先了解下什么是动态代理，我们先来看看它的作用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132109.png)

那么我们要怎么理解这个动态代理呢？我们通过一个案例来理解，首先我们创建一个学生类，并写入代码如下

```
package com.proxy;

public class Student {
    public void eat(String name) {
        System.out.println("学生吃"+name);
    }

    public void study() {
        System.out.println("在家自学");
    }
}
```

接着我们要实现的需求就是在不改动Student类中任何的代码的前提下，通过study方法输出一句话：来黑马学习，那么我们要如何去实现这个需求呢？这时候就需要用到我们的动态代理

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132110.png)

动态代理有两个组成，一个是被代理对象，一个是代理对象，其中前者是真实的对象，而后者是内存中的一个对象。动态代理实现的前提是代理对象和被代理对象都实现相同的接口。而动态代理的使用则依赖于Proxy.newProxyInstance()方法

那么我们首先创造一个接口，内部写上student类的所有方法

```
package com.proxy;

public interface StudentInterface {
    void eat(String name);
    void study();
}
```

接着令Student类继承这个接口，然后我们在我们的测试类里写入如下代码

```
package com.proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class Test {
    public static void main(String[] args) {
        Student stu = new Student();
        /*stu.eat("米饭");
        stu.study();*/

        /*
            要求：在不改动Student类中任何的代码的前提下，通过study方法输出一句话：来黑马学习
            类加载器：和被代理对象使用相同的类加载器
            接口类型Class数组：和被代理对象使用相同接口
            代理规则：完成代理增强的功能
         */
          StudentInterface proxyStu = (StudentInterface) Proxy.newProxyInstance(stu.getClass().getClassLoader(), new Class[]{StudentInterface.class}, new InvocationHandler() {
            /*
                执行Student类中所有的方法都会经过invoke方法
                对method方法进行判断
                    如果是study，则对其增强
                    如果不是，还调用学生对象原有的功能即可
             */
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                if(method.getName().equals("study")){
                    System.out.println("来黑马学习");
                    return null;
                }else {
                    return method.invoke(stu,args);
                }
            }
        });

          proxyStu.eat("米饭");
          proxyStu.study();
    }
}
```

这里我们先嗲用Proxy.newProxyInstance方法，该方法需要传入三个参数，这三个参数分别要实现代理的对象的类加载器，以及和被代理对象使用相同接口的接口的Class数组，最后要传入我们的代理规则，我们这里的代理规则采用匿名内部类的方式来实现，可以看到我们这里重写了invoke方法，我们只需要记住一件事情，那就是任何被我们代理的方法一旦执行，都要经过我们的invoke方法，因此我们在invoke方法里进行方法的判断，方法本身被封装到method对象里，我们通过方法名来判断其方法是否是我们要增强的方法，若是就对其进行增强，若不是就调用method的invoke方法并传入其对象和数据，这样就会令该方法按照原本的的方式执行。

最后我们通过我们创建的对象调用方法，就可以得到我们所需要的结果了。

- 归还连接之动态代理方式

那么接下来我们来正式学习动态代理方式来归还数据库，我们首先通过Proxy完成对Connect实现类对象的代理，然后我们在这个过程中判断是否是close方法，是的话就将其归还到池中，不是就照常执行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132111.png)

那么我们可以将我们的连接池的代码改造如下

```
/*
    自定义数据库连接池
 */
public class MyDataSource implements DataSource {
    //1.准备容器,用于保存多个连接对象,由于需要线程安全的集合对象，因此采用Collections的内置方法获取所需的集合对象
    private static List<Connection> pool = Collections.synchronizedList(new ArrayList<>());

    //2.定义静态代码块,通过工具类获取10个连接对象并放置于实现构造的集合容器中
    static {
        for (int i = 1; i <= 10; i++) {
            Connection con = JDBCUtils.getConnection();
            pool.add(con);
        }
    }

    @Override
    public Connection getConnection() throws SQLException {
        if(pool.size()>0){
            Connection con = pool.remove(0);

            Connection proxyCon = (Connection) Proxy.newProxyInstance(con.getClass().getClassLoader(), new Class[]{Connection.class}, new InvocationHandler() {
                /*
                    执行Connection实现类连接对象所有的方法都会经过invoke
                    如果是close，归还连接
                    反之则直接执行连接对象原有的功能即可
                 */
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    if(method.getName().equals("close")){
                        //归还连接
                        pool.add(con);
                        return null;
                    }else {
                        return method.invoke(con,args);
                    }
                }
            });
            return proxyCon;
        }else {
            throw new RuntimeException("连接数量已用尽");
        }
    }
    
    //4.定义getSize(),获取连接池容器的大小
    public int getSize() {
        return pool.size();
    }

```

可以看到我们这里主要进行的改造就是在连接池内部的改造，这里使用的动态代理的方式，我们传入了连接对象的类加载器，然后传入了对应的接口的Class文件，然后添加了增强规则，规则里我们就进行方法的判断，这个大家都懂了。然后这份代码同时也省略了后面许多的重写的方法

这份代码的唯一缺点就是由于这是我们自己编写的，所以其判断方式过于简单，存在缺陷

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132112.png)

## C3P0连接池

由于我们自己的连接池做的太辣鸡了，所以我们这里要学习使用别人的连接池，也就是开源数据库连接池，这里我们先来学习C3P0连接池，请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132113.png)

那么我们首先导入对应的jar包，然后我们将对应的配置文件复制进去，接着我们来看看配置文件的内容

```
<c3p0-config>
  <!-- 使用默认的配置读取连接池对象 -->
  <default-config>
   <!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://175.178.114.158:3306/db14</property>
    <property name="user">root</property>
    <property name="password">itheima</property>
    
    <!-- 连接池参数 -->
    <!--初始化的链接数量-->
    <property name="initialPoolSize">5</property>
    <!--最大连接数量-->
    <property name="maxPoolSize">10</property>
    <!--超时时间-->
    <property name="checkoutTimeout">3000</property>
  </default-config>

  <named-config name="otherc3p0"> 
    <!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/db15</property>
    <property name="user">root</property>
    <property name="password">itheima</property>
    
    <!-- 连接池参数 -->
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">8</property>
    <property name="checkoutTimeout">1000</property>
  </named-config>
</c3p0-config>
```

配置文件里首先有的是名字的指定，也就是说，我们可以通过指定特殊的名字来决定让我们的那些配置代码执行，比如说我们这里19行到后面是另外一份，而前面到19行又是一份，我们连接池参数里有初始的连接储量，还有最大的连接数量，当池中没有连接而又需要时，会创建新的连接，如果连接的数目过多就不创建了，此时指定一个等待时间，这里设置为3s，如果等待时间内还没有获得连接，那么就会抛出异常

那么接着我们创建一个测试代码，并写入代码如下

```
package com.itheima03;

import com.mchange.v2.c3p0.ComboPooledDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class C3P0Test1 {
    public static void main(String[] args) throws Exception{
        //1.创建c3p0的数据库连接池对象
        DataSource dataSource = new ComboPooledDataSource();

        //2.通过连接池对象获取数据库连接
        Connection con = dataSource.getConnection();

        //3.执行操作
        String sql = "SELECT * FROM student";
        PreparedStatement pst = con.prepareStatement(sql);

        //4.执行sql语句，接收结果集
        ResultSet rs = pst.executeQuery();

        //5.处理结果集
        while (rs.next()){
            System.out.println(rs.getInt("sid") + "\t" + rs.getString("name") + "\t" + rs.getInt("age") + "\t" + rs.getDate("birthday"));
        }

        //6.释放资源
        rs.close();
        pst.close();
        con.close();
    }
}
```

这里我们先创建c3p0的数据库连接池，然后获得连接池的内的链接，然后执行查询操作，最后的测试结果也没有问题

- C3P0连接池的配置

那么接着我们再来创建一个类来测试下我们的连接池里的连接的close方法到底是直接关闭了我们的连接还是把连接放回到了连接池中，我们创建一个类并写入代码如下

```
package com.itheima03;

import com.mchange.v2.c3p0.ComboPooledDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

public class C3P0Test2 {
    public static void main(String[] args) throws SQLException {
        //1.创建c3p0的数据库连接池对象
        DataSource dataSource = new ComboPooledDataSource();

        //2.测试
        for (int i = 1; i <= 11; i++) {
            Connection con = dataSource.getConnection();
            System.out.println(i+" : "+con);
            if(i==5){
                con.close();
            }
        }
    }
}
```

我们在第五次执行时调用了一次close方法，我们期待能够获得11个连接池对象并打印，且第五个和第六个是一样的连接对象，因为这个连接对象放进去又拿出来，所以是同一个连接对象，实际测试也的确是这样的，这就说明我们的close方法的确把连接放到了连接池中了

接着我们看我们的配置文件，可以通过指定不同的名字来指定执行不同的配置代码，我们这里采用的无参构造，没有任何指定，因此执行默认的代码，如果我们往里面指定了实际存在的字符串且在配置文件中真实存在，那么就会执行对应的配置，同样的，我们在配置文件里按照这个格式可以自己去指定各种不同的配置代码并用于执行

## Druid连接池

那么学习完了C3P0连接池的使用之后，接着我们来学习Druid连接池的使用，这是阿里巴巴搞的连接池，也是国内目前最好用的连接池技术之一。先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132114.png)

同样我们要导入jar包和配置文件，然后我们来看看配置文件里的内容

```
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://175.178.114.158:3306/db14
username=root
password=itheima
# 初始化连接数量
initialSize=5
# 最大连接数量
maxActive=10
# 超时时间
maxWait=3000
```

可以看到我们这个配置文件的内容还就那个跟上一个差不多，不过简洁了不少，这里就不再赘述了

然后我们就创建对应的测试类并写入代码如下

```
package com.itheima04;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Properties;

/*
    1.通过Properties集合，加载配置文件
    2.通过Druid连接池工程类获取数据库连接池对象
    3.通过连接池对象获取数据库连接进行使用
 */
public class DruidTest1 {
    public static void main(String[] args) throws Exception {
        //获取配置文件的流对象
        InputStream is = DruidTest1.class.getClassLoader().getResourceAsStream("druid.properties");

        //1.通过Properties集合，加载配置文件
        Properties prop = new Properties();
        prop.load(is);

        //2.通过Druid连接池工厂类获取数据库连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        //3.通过连接池对象获取数据库连接进行使用
        Connection con = dataSource.getConnection();

        String sql = "SELECT * FROM student";
        PreparedStatement pst = con.prepareStatement(sql);

        //4.执行sql语句，接收结果集
        ResultSet rs = pst.executeQuery();

        //5.处理结果集
        while (rs.next()){
            System.out.println(rs.getInt("sid") + "\t" + rs.getString("name") + "\t" + rs.getInt("age") + "\t" + rs.getDate("birthday"));
        }

        //6.释放资源
        rs.close();
        pst.close();
        con.close();
    }
}
```

我们这里先获取我们配置文件的流对象，然后我们通过Properties集合加载我们的配置文件，这里具体的操作就是将流对象传入到我们的prop对象中，这里面什么原理说实话也不知道，也不用知道。接下来我们通过Druid连接池的工程类，传入该prop对象获得连接池对象，然后通过连接池对象获得连接，接着执行查询所有的语句，实际上，也的确可以查询，此时就说明我们的连接池是的确可以使用的

## 连接池工具类

我们通过观察容易知道，其实我们的很多代码都是重复的，可以对其进行改进，比如说，我们的关闭方法，还有我们的创建连接池的代码，这些都是可以集合到一个工具类中的，所以本章我们就来集合这些代码到一个工具类中

那么我们可以创建一个工具类并写入如下代码

```
package com.itheima.utils;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

/*
    数据库连接池的工具类
 */
public class DataSourceUtils {
    //1.私有构造方法
    private DataSourceUtils(){};

    //2.声明数据源变量
    private static DataSource dataSource;

    //3.提供静态代码块，完成配置文件的加载和获取数据库连接池对象
    static {
        try {
            //完成配置文件的加载
            InputStream is = DataSourceUtils.class.getClassLoader().getResourceAsStream("druid.properties");

            Properties prop = new Properties();
            prop.load(is);

            //获取数据库连接池对象并赋给dataSource
            dataSource = DruidDataSourceFactory.createDataSource(prop);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    //4.提供一个获取数据库连接的方法
    public static Connection getConnection(){
        Connection con = null;
        try {
            con = dataSource.getConnection();
        }catch (Exception e){
            e.printStackTrace();
        }
        return con;
    }

    //5.提供一个获取数据库连接池对象的方法(学习框架时会用到)
    public static DataSource getDataSource() {
        return dataSource;
    }

    //6.释放资源
    public static void close(Connection con, Statement stat, ResultSet rs){
        if(con!=null){
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

        if(stat!=null){
            try {
                stat.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}

```

在这个代码里，我们将我们的方法都声明为静态方法，然后我们里面集合了各种我们用得到的方法，那么接着我们就可以用这份工具类来改造我们之前的代码

```
package com.itheima04;

import com.itheima.utils.DataSourceUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class DruidTest2 {
    public static void main(String[] args) throws SQLException {
        //1.通过连接池工具类获取一个数据库连接
        Connection con = DataSourceUtils.getConnection();

        String sql = "SELECT * FROM student";
        PreparedStatement pst = con.prepareStatement(sql);

        //2.执行sql语句，接收结果集
        ResultSet rs = pst.executeQuery();

        //3.处理结果集
        while (rs.next()){
            System.out.println(rs.getInt("sid") + "\t" + rs.getString("name") + "\t" + rs.getInt("age") + "\t" + rs.getDate("birthday"));
        }

        //4.释放资源
        DataSourceUtils.close(con,pst,rs);
    }
}

```

可以看到上面的这份代码就要比我们之前的代码简洁太多了，这个就很棒！

# JDBC框架

接下来我们来学习自己做一个JDBC的框架，这一节的内容并不要求我们能够独立写出来，我们主要是去感受这个思路，感受这个过程，理解到位就可以了。

## 框架背景介绍

首先我们分析我们之前实现过的JDBC案例，也就是Dao层里实现增删改查的代码，其实就是StudentDaoImpl类，可以在里面发现很多很多的重复代码，比如说获取数据库的连接，又或者是释放资源，这些都是相同的代码，而我们的核心功能其实只是执行一条sql语句而已，所以我们可以抽取出JDBC的模板类，将一些方法封装到里面，令其专门帮我们执行增删改查的操作。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132115.png)

构造这样的模板类的核心思想就是将之前那些重复操作都抽取到模板类的方法里，以此来简化我们的代码，这其实感觉就像是在搞工具类，但是又有所不同，具体不同请看后文的具体实现

## 数据库的源信息

在我们正式学习JDBC框架的构造之前，我们要先学习一些必要的知识，其实就是各种源信息，我们以后用得上，所以要先学习。

首先是DataBaseMetaData，该对象封装了整个数据库的源信息，我们可以调用这个对象的一些方法来获取数据库的名称和版本号，不过这个知识只做了解，因为我们用不太上

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132116.png)

接着是ParameterMetaData对象，这个对象可以通过预编译执行者对象PreparedStatement获得，该对象保存了参数的源信息，其封装了预编译执行者里对个参数的类型和属性，我们可以通过其中的getParameterCount()方法，获得sql语句中参数的个数。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132117.png)

最后是ResultSetMetaData对象，该对象封装了结果集对象中列的类型和属性，其是ResultSet对象调用getMetaData方法获得的，我们可以通过其下的getColumnCount()获取列的总数，还可以通过getColumnName(int i)，获得指定列的列名

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132118.png)

## UPDATE方法的实现

接下来我们正式来编写我们的JDBC框架，请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132119.png)

那么我们可以创建一个新的框架类并写入其代码如下，注意，下面放入的是我们用于执行更新操作，也就是增删改的代码

```
package com.itheima05;

import com.itheima.utils.DataSourceUtils;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ParameterMetaData;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

/*
    JDBC框架类
 */
public class JDBCTemplate {

    //1.定义参数变量(数据源、连接对象、执行者对象、结果集对象)，后面用得上，因此先定义
    private DataSource dataSource;
    private Connection con;
    private PreparedStatement pst;
    private ResultSet rs;

    //2.通过有参构造为数据源赋值，可以简单理解为其是一个连接池
    public JDBCTemplate(DataSource dataSource){
        this.dataSource=dataSource;
    }

    //3.定义update方法。参数：sql语句、sql语句中的参数
    public int update(String sql,Object... objs) {
        //4.定义int类型变量，用于接受增删改影响的行数
        int result = 0;
        try {
            //5.通过数据源获取一个数据库连接
            con = dataSource.getConnection();

            //6.通过数据库连接对象获取执行者对象，并对sql语句进行预编译
            pst = con.prepareStatement(sql);

            //7.通过执行者对象获取参数的源信息对象
            ParameterMetaData parameterMetaData = pst.getParameterMetaData();

            //8.通过参数源信息对象获取参数的个数
            int count = parameterMetaData.getParameterCount();

            //9.判断参数数量是否一致，确定用户传入的参数与实际要执行的匹配
            if(count!=objs.length){
                throw new RuntimeException("参数个数不匹配");
            }

            //10.为sql语句占位符赋值，采用setObject方法，这样就不会区分具体的数据类型
            //但这样做要保证我们传入的数据的类型和我们查询所需要的数据类型是一致的
            for (int i = 0; i < objs.length; i++) {
                pst.setObject(i+1,objs[i]);
            }

            //11.执行sql语句并接受结果
            result = pst.executeUpdate();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //12.释放资源
            DataSourceUtils.close(con,pst,null);
        }

        //13返回结果
        return result;
    }
}
```

具体的解释已经写在注释里了，这里就不再赘述了，那么到这里我们就成功构建了一个实现增删改的框架类了

那么接着我们就来进行update方法的测试，请看我们的测试步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132120.png)

那么我们可以创建一个测试并写入代码如下，注意，我们这里的测试类同时也是在模拟我们的之前所创建的Dao层代码

```
package com.itheima05;

import com.itheima.utils.DataSourceUtils;
import org.junit.Test;

/*
    模拟dao层
 */
public class JDBCTemplateTest1 {

    //通过工具类获得连接池对象，然后创建JDBCTemplate对象
    private JDBCTemplate template = new JDBCTemplate(DataSourceUtils.getDataSource());

    @Test
    public void delete() {
        //删除数据的测试
        String sql = "DELETE FROM student WHERE name=?";
        int result = template.update(sql,"周七");
        System.out.println(result);
    }

    @Test
    public void update() {
        //修改数据的测试
        String sql = "UPDATE student SET age=? WHERE name=?";
        Object[] params = {37,"周七"};
        int result = template.update(sql,params);
        System.out.println(result);
    }

    @Test
    public void insert() {
        //新增数据的测试
        String sql = "INSERT INTO student VALUES (?,?,?,?)";
        Object[] params = {5,"周七",27,"1997-07-07"};
        //调用自定义连接池内的更新方法
        int result = template.update(sql,params);
        if(result!=0){
            System.out.println("添加成功");
        }else {
            System.out.println("添加失败");
        }
    }
}

```

可以看到，我们现在增删改的代码就要比之前简洁不少，我们只需要关心sql语句就可以了，而不用去搞其他的，这就是我们框架类的好处，这个效果可以说是立竿见影的

## 查询功能的前期准备

接着我们来实现我们的查询功能，由于查询功能是比较复杂的，我们可以先将查询分为多种情况，然后分情况去实现这个功能，我们这里就将查询情况分为三种。分别是查询一条记录并封装对象，查询多条记录并封装集合以及查询聚合函数并返回数据的这三种情况并提供对应的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132121.png)

那么既然我们要返回一个封装对象，那么我们当然要先把这个对象给创建出来，所以我们先进行这个实体类的编写

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132122.png)

我们编写一个Student实体类，其代码如下

```
package com.itheima05.domain;

import java.util.Date;

/*
    学生的实体类
 */
public class Student {
    private Integer sid;
    private String name;
    private Integer age;
    private Date birthday;

    @Override
    public String toString() {
        return "Student{" +
                "sid=" + sid +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", birthday=" + birthday +
                '}';
    }

    public Integer getSid() {
        return sid;
    }

    public void setSid(Integer sid) {
        this.sid = sid;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public Student(Integer sid, String name, Integer age, Date birthday) {
        this.sid = sid;
        this.name = name;
        this.age = age;
        this.birthday = birthday;
    }

    public Student() {
    }
}

```

我们这里要特别注意的是我们的实体类中定义的成员变量的顺序一定要和我们实际数据中的保持一致，否则在后续的数据插入中会抛出异常

### ResultSetHandler

接着我们要定义一个处理结果集的泛型接口ResultSetHandler<T>并提供泛型方法，我们定义这个接口只是为了给不同结果集的处理提供一个规范，具体的实现类以及内部的实现逻辑，这还需要我们自己去编写

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132123.png)

那么我们可以编写这个接口并写入代码如下

```
package com.itheima05.handler;

import java.sql.ResultSet;

/*
    用于处理结果集方式的接口
 */
public interface ResultSetHandler<T> {
    <T> T handler(ResultSet rs);
}
```

### BeanHandeler实现类

那么我们现在就来进行对我们的接口进行一个实现，我们这里先来实现第一种情况，也就是查询一条记录并封装对象的底层代码，将来我们的查询一条记录并封装对象就需要调用我们下面所写入的代码

我们可以创建这个类并令其实现接口，我们写入其代码如下

```
package com.itheima05.handler;

import java.beans.PropertyDescriptor;
import java.lang.reflect.Method;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;

/*
    实现类1：将用于查询到的一条记录，封装为Student对象并返回
 */

//1.定义一个类，实现ResultSetHandler接口
public class BeanHandler<T> implements ResultSetHandler<T> {
    //2.定义Class对象类型成员变量
    private Class<T> beanClass;

    //3.通过有参构造为变量赋值，此处要传入指定对象的class对象，将其赋给成员变量
    public BeanHandler(Class<T> beanClass){
        this.beanClass = beanClass;
    }

    //4.重写handler方法。用于将一条记录封装到自定义对象中
    @Override
    public T handler(ResultSet rs){
        //5.声明自定义对象
        T bean = null;
        try {
            //6.调用newInstance()方法，利用反射机制创建传递参数的对象，为自定义对象赋值
            bean = beanClass.newInstance();

            //7.判断结果集中是否有数据
            if(rs.next()){
                //8.如果有数据就通过结果集对象获取结果集源信息的对象
                ResultSetMetaData metaData = rs.getMetaData();

                //9.通过结果集源信息对象获取列数
                int count = metaData.getColumnCount();

                //10.通过循环遍历列数
                for (int i = 1; i <= count; i++) {
                    //11.通过结果集源信息对象获取列名
                    String columnName = metaData.getColumnName(i);

                    //12.通过列名获取该列的数据
                    Object value = rs.getObject(columnName);

                    //13.创建属性描述器对象，将获取到的值通过该对象的set方法进行赋值
                    PropertyDescriptor pd = new PropertyDescriptor(columnName.toLowerCase(), beanClass);

                    //获取set方法
                    Method writeMethod = pd.getWriteMethod();

                    //执行set方法，给成员变量赋值
                    writeMethod.invoke(bean,value);
                }
            }
        }catch (Exception e){
            e.printStackTrace();
        }
        //14.返回封装好的对象
        return bean;
    }
}
```

注意29行之后的内容我们是利用反射机制实现的，忘了反射的内容的话可以回去复习，总之我们是通过反射机制，在方法中完成了对象的创建以及对对象的对应属性的赋值，最后我们返回这个对象。我们要记住一件事，那就是反射就是我们的框架的灵魂所在，所以关于反射机制我们是一定要认真了解的

## queryForObject的实现和测试

那么接着我们正式实现queryForObject方法，请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132124.png)

虽然说步骤看起来很多，但实际很多代码跟我们的update方法是写入的代码是一样的，我们复制黏贴再修改下就完了，那么我们可以写入其用于查询的代码如下

```
/*
    查询方法：用于将一条记录封装成自定义对象并返回
 */
public <T> T queryForObject(String sql, ResultSetHandler<T> rsh,Object... objs){
    T obj = null;
    try {
        //5.通过数据源获取一个数据库连接
        con = dataSource.getConnection();

        //6.通过数据库连接对象获取执行者对象，并对sql语句进行预编译
        pst = con.prepareStatement(sql);

        //7.通过执行者对象获取参数的源信息对象
        ParameterMetaData parameterMetaData = pst.getParameterMetaData();

        //8.通过参数源信息对象获取参数的个数
        int count = parameterMetaData.getParameterCount();

        //9.判断参数数量是否一致，确定用户传入的参数与实际要执行的匹配
        if(count!=objs.length){
            throw new RuntimeException("参数个数不匹配");
        }

        //10.为sql语句占位符赋值，采用setObject方法，这样就不会区分具体的数据类型
        //但这样做要保证我们传入的数据的类型和我们查询所需要的数据类型是一致的
        for (int i = 0; i < objs.length; i++) {
            pst.setObject(i+1,objs[i]);
        }

        //11.执行sql语句并接受结果
        rs = pst.executeQuery();

        //通过BeanHandler方式对结果进行处理
        obj = rsh.handler(rs);
    }catch (Exception e){
        e.printStackTrace();
    }finally {
        //12.释放资源
        DataSourceUtils.close(con,pst,null);
    }

    //13返回结果
    return obj;
}
```

然后我们需要测试下这个代码是否可行，所以我们可以在模拟Dao层代码的测试类中写入其测试的代码如下

```
@Test
public void queryForObject() {
    String sql = "SELECT * FROM student WHERE sid=?";
    Student stu = template.queryForObject(sql,new BeanHandler<>(Student.class),1);
    System.out.println(stu);
}
```

那么最后我们就要来讲讲我们这个总体代码的运行过程了，首先我们在测试类中定义我们的sql语句，然后调用连接池中的queryForObject方法，该方法是我们定义的，里面要求传入sql语句，传入一个ResultSetHandler接口对象（这个对象也是我们自己定义的），而这个对象的构造方法则是必须要传入一个要返回对象的字节码对象，因此我传入Student的字节码对象，最后我们传入用于查找的数据，我们这里传入1

接着这行语句开始执行，当然我们的代码会先进入到我们的BeanHandler类中，先构造出一个BeanHandler对象，然后我们的代码进入到queryForObject中，最开始的代码都差不多，无非是获取连接，获取参数个数然后对参数个数进行判断确定其是否匹配罢了，确认无误之后，我们执行查询语句，并最后获得一个名称为rs的结果集对象，我们调用rsh，也就是我们的构造出来的BeanHandler对象里的handler方法并传入这个结果集对象，handler方法会执行对应的代码，将结果集对象内部的数据赋予到其创建的对象中并将这个对象返回，我们就接受这个对象，最终返回这个结果，也就是obj。这就是我们这个代码的全过程了

## BeanListHandler实现类

接着我们来实现第二种情况的代码，当然，我们也是先实现底层类的代码，先来看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132125.png)

同样的，虽然步骤很长，不多很多其实和我们之前的学习过的非常类似了，我们这里复制过来再改改就完了，请看代码

```
package com.itheima05.handler;

import java.beans.PropertyDescriptor;
import java.lang.reflect.Method;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.util.ArrayList;
import java.util.List;

/*
    实现类2：将用于查询到的多条记录，封装为Student对象并添加到集合中返回
 */

//1.定义一个类，实现ResultSetHandler接口
public class BeanListHandler<T> implements ResultSetHandler<T> {
    //2.定义Class对象类型成员变量
    private Class<T> beanClass;

    //3.通过有参构造为变量赋值，此处要传入指定对象的class对象，将其赋给成员变量
    public BeanListHandler(Class<T> beanClass){
        this.beanClass = beanClass;
    }

    //4.重写handler方法。用于将多条记录封装到自定义对象中并添加到集合返回
    @Override
    public List<T> handler(ResultSet rs){
        //5.声明集合对象类型
        List<T> list = new ArrayList<>();
        try {
            //7.判断结果集中是否有数据
            while (rs.next()){
                //7.创建传递参数的对象，为自定义对象赋值
                T bean = beanClass.newInstance();

                //8.如果有数据就通过结果集对象获取结果集源信息的对象
                ResultSetMetaData metaData = rs.getMetaData();

                //9.通过结果集源信息对象获取列数
                int count = metaData.getColumnCount();

                //10.通过循环遍历列数
                for (int i = 1; i <= count; i++) {
                    //11.通过结果集源信息对象获取列名
                    String columnName = metaData.getColumnName(i);

                    //12.通过列名获取该列的数据
                    Object value = rs.getObject(columnName);

                    //13.创建属性描述器对象，将获取到的值通过该对象的set方法进行赋值
                    PropertyDescriptor pd = new PropertyDescriptor(columnName.toLowerCase(), beanClass);

                    //获取set方法
                    Method writeMethod = pd.getWriteMethod();

                    //执行set方法，给成员变量赋值
                    writeMethod.invoke(bean,value);
                }
                //将对象保存到集合中
                list.add(bean);
            }
        }catch (Exception e){
            e.printStackTrace();
        }
        //14.返回封装好的对象
        return list;
    }
}
```

我们这里进行的改动很简单，首先是我们让返回对象变成了集合泛型对象，然后我们的判断是否还有数据改成了while，因为我们这里并不是只判断一次，是只要还有数据我们就执行，其实我觉得按这个思路来，其实最开始那个查询一个对象的我们也是可以用while的说实话，然后每次循环就将生成的对象添加到集合中。

## queryForList的实现和测试

那么编写完底层代码之后，现在我们正式来写其实现和测试，请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132126.png)

那么我们可以写入其实现代码如下

```
/*
    查询方法：用于将多条记录封装成自定义对象并添加到集合中返回
 */
public <T> List<T> queryForList(String sql, ResultSetHandler<T> rsh, Object... objs){
    List<T> list = new ArrayList<>();
    try {
        //5.通过数据源获取一个数据库连接
        con = dataSource.getConnection();

        //6.通过数据库连接对象获取执行者对象，并对sql语句进行预编译
        pst = con.prepareStatement(sql);

        //7.通过执行者对象获取参数的源信息对象
        ParameterMetaData parameterMetaData = pst.getParameterMetaData();

        //8.通过参数源信息对象获取参数的个数
        int count = parameterMetaData.getParameterCount();

        //9.判断参数数量是否一致，确定用户传入的参数与实际要执行的匹配
        if(count!=objs.length){
            throw new RuntimeException("参数个数不匹配");
        }

        //10.为sql语句占位符赋值，采用setObject方法，这样就不会区分具体的数据类型
        //但这样做要保证我们传入的数据的类型和我们查询所需要的数据类型是一致的
        for (int i = 0; i < objs.length; i++) {
            pst.setObject(i+1,objs[i]);
        }

        //11.执行sql语句并接受结果
        rs = pst.executeQuery();

        //通过BeanHandler方式对结果进行处理
        list = rsh.handler(rs);
    }catch (Exception e){
        e.printStackTrace();
    }finally {
        //12.释放资源
        DataSourceUtils.close(con,pst,rs);
    }

    //13返回结果
    return list;
}
```

我们这里的改动非常简单，就是改成了集合对象，然后调用对应的方法获得集合对象并将这个对象返回而已

接着我们编写测试类的代码

```
@Test
public void queryForList() {
    //查询所有学生信息的测试
    String sql = "SELECT * FROM student";
    List<Student> list = template.queryForList(sql,new BeanListHandler<>(Student.class));
    for (Student student :list) {
        System.out.println(student);
    }
}
```

可以看到我们这里也还是同样的套路，调用对应方法然后传入参数，获得我们需要的集合，然后打印集合就完了，最后我们也的确能够得到我们想要的结果

## ScalarHandler实现类

那么现在我们来实现我们的最后一个需求，就是查询聚合函数，先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132127.png)

那么我们可以新创建一个类并写入代码如下

```
package com.itheima05.handler;

import java.sql.ResultSet;
import java.sql.ResultSetMetaData;

/*
    实现聚合函数的查询
 */
//1.定义一个类，实现ResultSetHandler接口
public class ScalarHandler<T> implements ResultSetHandler<T> {

    //2.重写handler方法
    @Override
    public Long handler(ResultSet rs){
        //3.定义一个Long类型
        Long value = null;
        try {
            //4.判断结果集对象中是否还有数据
            if(rs.next()){
                //5.获取结果集源信息的对象
                ResultSetMetaData metaData = rs.getMetaData();
                //6.获取第一列的列名
                String columnName = metaData.getColumnName(1);
                //7.根据列名获取该列的值
                value = rs.getLong(columnName);
            }
        }catch (Exception e){
            e.printStackTrace();
        }
        //8.返回结果
        return value;
    }
}

```

我们这里只是需要数字而已，所以我们这里返回的是long的包装类Long，聚合函数的结果总是在第一个的，因此我们这里获取第一列的列名，然后根据列名从结果集中获取到这个数据并返回

## queryForScalar的实现和测试

那么我们来正式去实现第三个功能，同样先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817132128.png)

那么我们可以写入其代码如下

```
/*
    查询方法：用于将聚合函数的查询结果进行返回
 */
public Long queryForScalar(String sql, ResultSetHandler<Long> rsh, Object... objs){

    Long value = null;

    try {
        //5.通过数据源获取一个数据库连接
        con = dataSource.getConnection();

        //6.通过数据库连接对象获取执行者对象，并对sql语句进行预编译
        pst = con.prepareStatement(sql);

        //7.通过执行者对象获取参数的源信息对象
        ParameterMetaData parameterMetaData = pst.getParameterMetaData();

        //8.通过参数源信息对象获取参数的个数
        int count = parameterMetaData.getParameterCount();

        //9.判断参数数量是否一致，确定用户传入的参数与实际要执行的匹配
        if(count!=objs.length){
            throw new RuntimeException("参数个数不匹配");
        }

        //10.为sql语句占位符赋值，采用setObject方法，这样就不会区分具体的数据类型
        //但这样做要保证我们传入的数据的类型和我们查询所需要的数据类型是一致的
        for (int i = 0; i < objs.length; i++) {
            pst.setObject(i+1,objs[i]);
        }

        //11.执行sql语句并接受结果
        rs = pst.executeQuery();

        //通过ScalarHandler方式对结果进行处理
        value = rsh.handler(rs);
    }catch (Exception e){
        e.printStackTrace();
    }finally {
        //12.释放资源
        DataSourceUtils.close(con,pst,rs);
    }

    //13返回结果
    return value;
}
```

其实就是做了简单的返回值的修改，将返回值改成了Long而已

最后我们写入测试代码

```
@Test
public void queryForScalar() {
    //查询聚合函数的测试
    String sql = "SELECT COUNT(*) FROM student";
    Long value = template.queryForScalar(sql,new ScalarHandler<Long>());
    System.out.println(value);
}
```

实际执行我们也会发现这个测试代码没有什么问题，那么到此为止，我们的JDBC的框架类就实现完毕了！