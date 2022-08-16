之前我们用传统方式实现了Dao层，但是传统方式既要写接口，又要写实现类，很麻烦。MyBatis框架可以帮助我们省略编写Dao层接口实现类的步骤，我们只需要直接编写接口，接着由MyBatis框架根据接口的定义来创建接口的动态代理对象，但是使用这种方法是有实现规则的，具体规则如下图所示

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE71f3a074856d42444f9d3cec32ccec16.png)

- 代码的实现

那么接着我们就来用接口代理的方式来实现我们的Dao层，请看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4da4035d1098ed56bdf7dc182f2eb990.png)

由于我们的接口一开始编写的时候就特意和我们的Dao层的接口的各项参数保持一直了，因此我们这里只需要改动名称空间就可以了，将名称空间改为Dao层接口类所在的名字（其实有些方法对应的属性压根就没写，但是也不会出问题，我也不知道其个中原理，老师也没提，因此先按下不表）

```
<mapper namespace="com.itheima.mapper.StudentMapper">
```

然后我们可以将在service层的实现类中来实现我们的功能，我们写入其代码如下

```
package com.itheima.service.impl;

import com.itheima.bean.Student;
import com.itheima.mapper.StudentMapper;
import com.itheima.service.StudentService;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/*
    业务层实现类
 */
public class StudentServiceImpl implements StudentService {

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

            //4.获取StudentMapper接口的实现类对象
            StudentMapper mapper = sqlSession.getMapper(StudentMapper.class); // StudentMapper mapper = new StudentMapperImpl();

            //5.通过实现类对象调用方法，接受结果
            list = mapper.selectAll();

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
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

        //7.返回结果
        return list;
    }

    /*
        根据id查找
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

            //4.获取StudentMapper接口的实现类对象
            StudentMapper mapper = sqlSession.getMapper(StudentMapper.class); // StudentMapper mapper = new StudentMapperImpl();

            //5.通过实现类对象调用方法，接受结果
            stu = mapper.selectById(id);

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
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

        //7.返回结果
        return stu;
    }

    /*
        插入数据
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

            //4.获取StudentMapper接口的实现类对象
            StudentMapper mapper = sqlSession.getMapper(StudentMapper.class); // StudentMapper mapper = new StudentMapperImpl();

            //5.通过实现类对象调用方法，接受结果
            result = mapper.insert(stu);

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
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

        //7.返回结果
        return result;
    }

    /*
        修改数据
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

            //4.获取StudentMapper接口的实现类对象
            StudentMapper mapper = sqlSession.getMapper(StudentMapper.class); // StudentMapper mapper = new StudentMapperImpl();

            //5.通过实现类对象调用方法，接受结果
            result = mapper.update(stu);

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
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

        //7.返回结果
        return result;
    }

    /*
        删除数据
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

            //4.获取StudentMapper接口的实现类对象
            StudentMapper mapper = sqlSession.getMapper(StudentMapper.class); // StudentMapper mapper = new StudentMapperImpl();

            //5.通过实现类对象调用方法，接受结果
            result = mapper.delete(id);

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //6.释放资源
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

        //7.返回结果
        return result;
    }
}
```

我们这里可以看到其实内容和我们之前Dao层写得大差不差，唯一有区别的是第四步，其调用了getMapper方法传入了我们的Dao层接口的对象，然后Dao层对象。其实这里就相当于是我们的new了一个Dao层接口的实现类，只不过这里的接口是由我们的程序动态生成的，而不是由我们手动编写的。所以我们就不要理解为这是我们new了一个接口啊，不是这个意思啊

然后我们调用mapper里的对应方法并传入对应的参数，其会利用这个参数去通过mapper的名称空间寻找到我们对应的映射配置文件，然后通过方法名来寻找具有相同id的映射配置文件里的方法，接着执行对应的SQL语句，也将参数对应传入，最后返回我们所需要的数据。

通过接口代理方式实现Dao层的方式是比较符合开发规范的方式，以后我们也会使用这种方式来进行开发。

- 源码分析

那么这一节我们就来分析下我们的接口代理方式的源码，看看其内部到底是怎么运作的，先来看看我们动态获得实现类对象的方法

```
//4.获取StudentMapper接口的实现类对象
StudentMapper mapper = sqlSession.getMapper(StudentMapper.class); // StudentMapper mapper = new StudentMapperImpl();
```

我们可以看到其实通过getMapper方法获得的，传入了一个我们的接口对象，我们点进去getMapper里看看

```
<T> T getMapper(Class<T> var1);
```

会发现它继承一个接口，我们找到其默认的实现类，可以看到这个方法

```
public <T> T getMapper(Class<T> type) {
    return this.configuration.getMapper(type, this);
}
```

这里的type就是我们传入的接口对象，this代表的是其当前的SqlSession对象，可以看到其将这两个对象一起传入给了其下的getMapper方法，我们再点进去看看

```
public <T> T getMapper(Class<T> type, SqlSession sqlSession) {
    return this.mapperRegistry.getMapper(type, sqlSession);
}
```

可以看到他又调用了一个新的getMapper方法，同样是传入这两个参数，那我们继续点击去看看

```
public <T> T getMapper(Class<T> type, SqlSession sqlSession) {
    MapperProxyFactory<T> mapperProxyFactory = (MapperProxyFactory)this.knownMappers.get(type);
    if (mapperProxyFactory == null) {
        throw new BindingException("Type " + type + " is not known to the MapperRegistry.");
    } else {
        try {
            return mapperProxyFactory.newInstance(sqlSession);
        } catch (Exception var5) {
            throw new BindingException("Error getting mapper instance. Cause: " + var5, var5);
        }
    }
}
```

可以看到其获得了传入的参数，然后调用了一个映射代理工厂对象，也就是MapperProxyFactory，后面其利用这个工厂对象调用其newInstance方法，并传入sqlSession对象，点进去看看

```
public T newInstance(SqlSession sqlSession) {
    MapperProxy<T> mapperProxy = new MapperProxy(sqlSession, this.mapperInterface, this.methodCache);
    return this.newInstance(mapperProxy);
}
```

可以我们看到这里其就创建了一个MapperProxy映射对象，然后其又调用了一个newInstance方法并传入这个映射对象，同样点进去

```
protected T newInstance(MapperProxy<T> mapperProxy) {
    return Proxy.newProxyInstance(this.mapperInterface.getClassLoader(), new Class[]{this.mapperInterface}, mapperProxy);
}
```

我可以看到其调用Proxy.newProxyInstance方法，这就是JDK底层给我们提供的动态代理方法，这里提供了类加载器，然后提供类的对象的数组，然后提供代理规则，最后就能动态生成我们的所需要的实现类了。所以说，框架的本质还是靠sun公司提供的老东西

那么了解了动态代理的执行原理之后，我们接着来看看insert方法是怎么执行的，由于其实动态代理的，因此其类我们要利用debug的方式来查看其执行原理

我们先在其调用方法的代码上设置断点

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcb9fb40fed9b8e3be047e260fc3bda88.png)

然后进入其类中查看其代码，该类为MapperProxy，其利用反射机制获得其所需要的各种类和属性，然后我们进入到里面能看到很多方法，但这些我们需要关心，我们重点关心下面这两行

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc78b2306eb29e5a6c2b4f09ed0d76556.png)

可以看到其获得了我们的映射方法类，然后调用了里面的execute方法，传入了sqlSession对象和参数，我们点进去execute中看看

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbd15e0776bb9c6372f45679de4298fe3.png)

可以看到其先是获得方法执行的类型，然后进行了判断，调用其对应的方法，我们不难看到，其本质还是调用sqlSession里的对应方法，所以说其实到头来本质还是没变，只是省略了过程而已

那么最后我们可以对本节总结如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0c69d80ac3dcab256c26b164657b0032.png)

- 小结

最后我们对本章学习的内容做一个总结，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9c41eed2b0bd0ab51f4ff472fa8fd0df.png)

