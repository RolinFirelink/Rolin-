接着我们来进行我们的MyBatis的最后一个内容，就是用传统方式实现Dao层

- 环境介绍

我们先来介绍下什么是传统方式，所谓传统方式就是分层思想，我们将我们的层数分为控制层，业务层和持久层，其调用关系是从上往下的，具体请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE370cef1073d41a1eac289c1032085085.png)

然后我们就要在java中创建对应的包并完成其实现类，不过这里值得一提的是在MyBatis里，我们的最后一层不是Dao，而是mapper。其实就相当于是把Dao替换成了mapper，没了。

现在我们假设我们做好了一些预备工作了，比如说创建对应的各层的接口和实现类，那么我们就可以来正式去实现我们的代码了

- 传统方式实现Dao层

那么接着我们Dao层的代码，也就是我们的Mapper类中的代码可以实现如下

```
package com.itheima.mapper.impl;

import com.itheima.bean.Student;
import com.itheima.mapper.StudentMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class StudentMapperImpl implements StudentMapper {

    /*
        查询全部
     */
    @Override
    public List<Student> selectAll() {
        List<Student> list = null;
        SqlSession sqlSession = null;
        InputStream is = null;

        try {
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.执行映射配置文件中的sql语句，并接受结果
            list = sqlSession.selectList("StudentMapper.selectAll");

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //5.释放资源
            if(sqlSession!=null){
                sqlSession.close();
            }
            if(is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        //6.返回结果
        return list;
    }

    /*
        根据id查询
     */
    @Override
    public Student selectById(Integer id) {
        Student stu = null;
        SqlSession sqlSession = null;
        InputStream is = null;

        try {
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.执行映射配置文件中的sql语句，并接受结果
            stu = sqlSession.selectOne("StudentMapper.selectById",id);

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //5.释放资源
            if(sqlSession!=null){
                sqlSession.close();
            }
            if(is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        //6.返回结果
        return stu;
    }

    /*
        新增功能
     */
    @Override
    public Integer insert(Student stu) {
        Integer result = null;
        SqlSession sqlSession = null;
        InputStream is = null;

        try {
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.执行映射配置文件中的sql语句，并接受结果
            result = sqlSession.insert("StudentMapper.insert",stu);

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //5.释放资源
            if(sqlSession!=null){
                sqlSession.close();
            }
            if(is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        //6.返回结果
        return result;
    }


    /*
        修改功能
     */
    @Override
    public Integer update(Student stu) {
        Integer result = null;
        SqlSession sqlSession = null;
        InputStream is = null;

        try {
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.执行映射配置文件中的sql语句，并接受结果
            result = sqlSession.update("StudentMapper.update",stu);

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //5.释放资源
            if(sqlSession!=null){
                sqlSession.close();
            }
            if(is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        //6.返回结果
        return result;
    }

    /*
        删除功能
     */
    @Override
    public Integer delete(Integer id) {
        Integer result = null;
        SqlSession sqlSession = null;
        InputStream is = null;

        try {
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.执行映射配置文件中的sql语句，并接受结果
            result = sqlSession.delete("StudentMapper.delete",id);

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //5.释放资源
            if(sqlSession!=null){
                sqlSession.close();
            }
            if(is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        //6.返回结果
        return result;
    }
}

```

后续我们还有很多上级调用的代码，这里就不一一展示了，经过测试我们会发现这些代码都是确实可行的。

- LOG4J的使用

在日常的开发过程中，我们有时排查问题时还需要真正执行的SQL语句、参数、结果等信息，此时我们可以借助LOG4J的功能来实现执行信息的输出

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbdfc9f381762ad2ad9e77e78fd35db61.png)

来看看其使用步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa2a0306553c22de40c3829eb657387b7.png)

我们首先要导入对应的jar包，然后我们要编写其对应的配置信息到我们的核心配置文件上，其配置代码如下

```
<!--配置LOG4J-->
<settings>
    <setting name="logImpl" value="log4j"/>
</settings>
```

然后我们要写入log4j自己的配置代码如下

```
# Global logging configuration
# ERROR WARN INFO DEBUG（有四个级别，分别对应要打印的内容）,stout表示将其输出到控制台上
log4j.rootLogger=DEBUG, stdout
# Console output...
# 下面表示的是输出的格式
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

最后我们执行对应的语句时就会在控制台上多打印一堆我们执行功能时的信息，我们只需要关注下面三行的信息就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb549739b3a680bf56277e9db82b7960c.png)

可以看到这三行分别是实际执行的SQL语句、传入的参数以及总影响行数。