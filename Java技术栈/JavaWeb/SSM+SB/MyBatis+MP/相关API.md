接着我们来正式进行我们相关API的学习，学习之前我们先把之前我们案例的代码再放出来过一遍

```
package com.tiheima.dao;

import com.itheima.bean.Student;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class StudentTest01 {
    /*
        查询全部
     */
    @Test
    public void selectAll() throws Exception {
        //1.加载核心配置文件
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

        //2.获取SqlSession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        //3.通过SqlSession工程对象获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //4.执行映射配置文件中的sql语句，并接受结果
        List<Student> list = sqlSession.selectList("StudentMapper.selectAll");

        //5.处理结果
        for (Student stu:list) {
            System.out.println(stu);
        }

        //6.释放资源
        sqlSession.close();
        is.close();
    }
}

```

- Resources的介绍

Resource其实就是用于加载资源的工具类，其会返回一个指定资源的字节输入流。我们上面写入的代码Resources.getResourceAsStream("MyBatisConfig.xml")，其实就相当于是StudentTest01.class.getClassLoader().getResourceAsStream("MyBatisConfig.xml")，这里我们的Resuouces就相当于是代替了StudentTest01.class.getClassLoader()

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1c3a616b4026996a6c7736e32f12cca2.png)

- SqlSessionFactoryBuilder的介绍

该类的主要作用就是用于获取SqlSessionFactory工厂对象，其内部的build方法就是用于获取工厂对象的方法，要求传入一个字节输入流来获取工厂对象

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe36f31ad739b9a5a0538007ea2f01aff.png)

- SqlSessionFactory的介绍

工厂对象SqlSessionFactory的主要作用就是获取SqlSession构建者对象，其下的方法openSession()就是用于获取的，有一个重载方法，如果传入true则开启自动提交事务，如果不传入，那么默认是手动提交事务。我们上面的例程里没有开启自动提交事务，是要手动提交事务的，但因为我们是查询的sql语句而非添加，并不涉及修改，因此提不提交无所谓

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE61709229517d577681dcfb8c2aaa180d.png)

- SqlSession的介绍

SqlSession有三个重要的功能，分别是执行SQL、管理事务以及接口代理。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE20e12b846295c8ab6fe0cac816ed5cd3.png)

上图中前五个方法用于执行对应的sql语句，倒数第三第四的方法用于提交和回滚事务，而倒数第二个方法则用于获取指定接口，close方法则用于释放资源

最后我们可以对本章做一个小结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe70f7a8870a3f7fe72c414ca1bd2ba6b.png)

