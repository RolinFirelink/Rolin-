本章节我们来学习编写JDBC工具类，因为我们Dao层的代码很多都重复使用了，代码复用的效率不高，因此我们要将相同的代码尽可能地将其编写成工具类，以实现代码复用

- 配置文件的编写

编写配置文件，首先我们要创建对应的配置文件并写入如图所示的内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcc42a789e371ba11bed26003093f187d.png)

那么待会我们的工具类，就是来读取这个配置文件来操作的

- 工具类的编写

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

- 优化学生案例

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