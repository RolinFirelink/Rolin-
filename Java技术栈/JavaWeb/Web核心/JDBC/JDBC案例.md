学完了相应的知识之后，接下来我们来学习对应的JDBC的案例，通过这个案例来加深我们对JDBC的使用理解

- 需求介绍和数据准备

先来看看我们的案例需求

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE51b4d617e91cb2b4036a842855bbd54b.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcf9d154847f33baeb7b32ff5585c82eb.png)

注意我们这里学生类的变量要跟我们数据中的学生类对象的属性保持一致，而且如果其会基本属性，我们就要使用包装类来代替，因为我们读取出的值有可能是null，此时为了避免抛出异常需要使用包装类来承接这个数据。然后我们这里要模拟真实的项目构建，所以我们要创造三层来调用我们的代码

具体的创建过程就不演示了，这里我们权当已经创建好了

- 需求一查询所有学生信息

先来看看我们的所有需求

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE41c779bd5c9d461f3144efbfd83e9d05.png)

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

- 需求二根据id查询学生信息

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

- 需求三添加学生信息

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

- 需求四修改学生信息

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

- 需求五删除学生信息

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