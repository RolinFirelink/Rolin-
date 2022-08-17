# MyBatis快速入门

那么本节我们就来学习MyBatis框架，这是我们在JDBC之后学习的更加进一步的内容。首先我们要做对框架的介绍，首先我们要知道到底什么是框架

**JDBC是Java提供的一个操作数据库的API; MyBatis是一个持久层ORM框架,底层是对JDBC的封装**

## 框架的介绍

框架其实就是一款半成品软件，我们可以程序员可以基于这个软件继续开发来完成个性化的定制需求。如果以盖房子为例，那么框架就相当于是房子本身，我们程序员要做的事情就是给房子涂上我们所需要的颜色

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4d7230c81240de057d5bf861fdf2507f.png)

## ORM介绍

在正式学习MyBatis之前，我们还需要学习ORM。所谓ORM就是Object Relational Mapping的简写，其意为对象关系映射。指的是持久化数据和实体对象的映射模式，其是为了解决面向对象与关系型数据库存在的互不匹配的现象的技术

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe4b52d3e3ef4b982865fef9de7474321.png)

简单来说就是我们的数据库里的数据表的内容在我们java程序里是要将数据封装为一个对象来表示的，而java里的一个对象里的内容也是要将其数据取出来放到我们对应的数据库里的数据表中的，这个过程靠的就是ORM。这里我们是存在映射规则的，首先我们的一个数据表就代表一个类，比如说我们的Student数据表，在我们的java程序中就是一个Student学生类。而表中的字段就是类的属性，比如我们Studnet数据表中的age字段就代表了学生类中的age属性，而表中的数据则是我们所需要映射到我们的对象中对应的属性的数据

## MyBatis介绍

在正式介绍MyBatis之前，我们要先了解下我们之前的JDBC框架的不足和改进方法，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe0ef7f946e8a5158f0adc1e1b78b0e0a.png)

我们可以看到我们的JDBC存在的问题主要有四点，一是频繁的创建销毁数据库连接造成资源浪费从而影响效率，二是Sql语句是硬编码，不易于维护。三是查询时需要手动封装数据到对象中，四是增删改查涉及到参数是要手动设置数据到？占位符中。这些都是有解释方法的，比如说对于第一点，我们可以使用数据库连接池，第二点可以将sql语句抽取到配置文件中，第三点可以使用反射等技术完成自动封装。但这些实现起来都比较麻烦，有没有一种框架已经帮我们把这些实现好了呢？当然有，这就不得不提我们的MyBatis了

首先我们的MyBatis是一个基于java的持久层框架，内部封装了JDBC，开发者只需要关注SQL语句本身，不需要去处理其他复杂操作。其次是MyBatis通过xml或注解的方式将要执行的Statement配置起来，并通过Java对象和Statment中的SQL动态参数进行映射生成最终要执行的SQL语句。最后是MyBatis框架执行完SQL并将结果映射为Java对象并返回，这里采用了ORM思想，内部对JDBC进行了封装，使我们不必与JDBC打交道就可以完成对数据库的持久化操作

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7dec24525161f8f662f579668ac13da4.png)

## mybatis入门程序

我们现在做一个入门程序来作为我们对mybatis的学习，这里我们用到的方法先不解释，我们先用着

先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdbc79b80f0f743374d6a170ce53fd967.png)

首先我们要做相关的数据库的数据的准备，我们可以在数据库中创建db1，并创建student数据表，写入数据如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb5135c9faf6f05d5608f0a0fb980d2a3.png)

请看代码

```
DROP DATABASE db1;
        CREATE DATABASE db1;
        CREATE TABLE student(
        id INT,
        NAME VARCHAR(10),
        age INT
        );
        INSERT INTO student VALUES (1,'张三',23),(2,'李四',24),(3,'王五',25);
        ALTER TABLE student MODIFY id INT PRIMARY KEY AUTO_INCREMENT;
        DESC student;
```

然后我们要创建对应的类，导入对应的jar包，这里我们都假定我们已经做好了。然后我们要在src下创建映射配置文件，这个映射配置文件的名字其实是可以随意指定的，但是我们一般采用类名+Mapper的方式来指定比较好，这样比较有规范。那么我们可以创建并写入代码如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="StudentMapper">
    <select id="selectAll" resultType="com.itheima.bean.Student">
        SELECT * FROM student
    </select>
</mapper>
```

最上面的内容是固定的内容，规定了字符集和其他的内容，在我们的资料中有提供，我们直接复制黏贴就完了，后面我们要指定mapper标签，然后我们要写入namespace属性，然后写入对应的select标签，加入id属性，以及赋予将数据映射给对象的对象地址（放在resultType属性中，指定地址就可以了），然后标签内部写入我们对应的sql查询语句，我们这里写入查询全部的sql语句

接着我们还需要在src下创建核心配置文件并写入代码如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://175.178.114.158:3306/db1"/>
                <property name="username" value="root"/>
                <property name="password" value="itheima"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper resource="StudentMapper.xml"/>
    </mappers>
</configuration>
```

这里最上面的也是固定内容，同样提供好了，我们复制黏贴。然后我们要写入configuration标签，然后写入environment子标签，子标签下有default属性，这个属性指的是我们默认采用的配置，比如我们这里填入mysql，那么其默认就会寻找environment标签里id属性为mysql的配置来执行，这也侧面说明了我们可以在这个配置文件下编写多个配置内容。然后我们写入transactionManager标签，type属性我们填写JDBC，然后我们写入dataSource标签，type属性写入POOLED，代表连接池，然后再写入property子标签，标签下的属性name和value，就分别写入我们之前填写过的driver，url，用户名和密码那些就行了

最后我们可以写入测试类并填入代码如下

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

最后我们可以对我们的快速入门做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf0aeb928993c57e852a8a39ddce92bfb.png)

# Mybatis相关API

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

## Resources的介绍

Resource其实就是用于加载资源的工具类，其会返回一个指定资源的字节输入流。我们上面写入的代码Resources.getResourceAsStream("MyBatisConfig.xml")，其实就相当于是StudentTest01.class.getClassLoader().getResourceAsStream("MyBatisConfig.xml")，这里我们的Resuouces就相当于是代替了StudentTest01.class.getClassLoader()

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1c3a616b4026996a6c7736e32f12cca2.png)

## SqlSessionFactoryBuilder的介绍

该类的主要作用就是用于获取SqlSessionFactory工厂对象，其内部的build方法就是用于获取工厂对象的方法，要求传入一个字节输入流来获取工厂对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe36f31ad739b9a5a0538007ea2f01aff.png)

## SqlSessionFactory的介绍

工厂对象SqlSessionFactory的主要作用就是获取SqlSession构建者对象，其下的方法openSession()就是用于获取的，有一个重载方法，如果传入true则开启自动提交事务，如果不传入，那么默认是手动提交事务。我们上面的例程里没有开启自动提交事务，是要手动提交事务的，但因为我们是查询的sql语句而非添加，并不涉及修改，因此提不提交无所谓

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE61709229517d577681dcfb8c2aaa180d.png)

## SqlSession的介绍

SqlSession有三个重要的功能，分别是执行SQL、管理事务以及接口代理。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe70f7a8870a3f7fe72c414ca1bd2ba6b.png)

上图中前五个方法用于执行对应的sql语句，倒数第三第四的方法用于提交和回滚事务，而倒数第二个方法则用于获取指定接口，close方法则用于释放资源

最后我们可以对本章做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE20e12b846295c8ab6fe0cac816ed5cd3.png)

# 映射配置文件

学习完了相关的API之后，接着我们来学习我们的映射配置文件，同样我们先把我们映射配置文件的代码放出来

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="StudentMapper">
    <select id="selectAll" resultType="com.itheima.bean.Student">
        SELECT * FROM student
    </select>
</mapper>
```

## 映射配置文件的介绍

关于映射配置文件中各个语句的作用，我们请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE32d059e56adf8b6ba2eb7fea9a721fa5.png)

最开头的一行代码指定的配置文件的版本和字符集

后一行代码指定了配置文件的约束，比如说这里我们的mapper标签就是其事先指定好的，包括mapper里的子标签，都是依赖最开头的约束才实现的

后面的mapper里的namespace属性则是用于给核心配置文件指定的一个属性，我们可以理解为是类似于id的唯一标识，其命名规则一般是采用类名+Mapper

我们的resultType指定的映射对象类型，简单来说就是指定数据要封装的对象。而parameterType属性指定参数映射对象类型，说白了就是用于给里面的？占位符赋值的数据的属性

那么最终我们可以将我们的代码增加注释如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--
    mapper:核心根标签
    namespace属性：名称空间
-->
<mapper namespace="StudentMapper">
    <!--
        select:查询功能的标签
        id属性：唯一标识
        resultType属性：指定结果映射对象类型
        parameterType属性：指定参数映射对象类型
    -->
    <select id="selectAll" resultType="com.itheima.bean.Student">
        SELECT * FROM student
    </select>
</mapper>
```

## 查询功能的使用

那么学习了简单的映射配置文件的对应语句的说明之后，接着我们在配置文件中实现我们的增删改查四个功能。先来实现根据id查询的功能，先从配置文件开始构造。那么我们可以在映射配置文件中的mapper标签中写入代码如下

```
<select id="selectById" resultType="com.itheima.bean.Student" parameterType="java.lang.Integer">
    SELECT * FROM student WHERE id = #{id}
</select>
```

这里我们要执行根据id查询的语句，需要参数进行赋值，我们这里就需要在parameterType中指定我们的参数的数据类型，当然，如果是基本类型就要改成是包装类。然后我们这里不能占位符？来指代需要传入参数的变量，我们需要用#{}，内部写入的名字可以随意指定，我们这里为了便于阅读就写入id，因为我们是根据id进行查找

然后我们可以在java类中写入代码如下

```
/*
    根据id查询
 */
@Test
public void selectById() throws Exception{
    //1.加载核心配置文件
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    //2.获取SqlSession工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    //3.通过工厂对象获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();

    //4.执行映射配置文件中的sql语句，并接收结果
    Student stu = sqlSession.selectOne("StudentMapper.selectById", 3);

    //5.处理结果
    System.out.println(stu);

    //6.释放资源
    sqlSession.close();
    is.close();
}
```

我们这里做出的改动就是在第四步和第五步，我们这里调用了selectOne，之所以调用selectOne，因为我们这里得到结果必然是一个的，这个方法是sqlSession中已经提供好了给我们使用的，并且传入了用于查询的id，内部调用了我们刚刚自己构造的查询的sql语句，最后打印该对象

最后我们可以对本小节做个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf132116319aad13fb01616f8dd7771f0.png)

## 新增功能的使用

接着我们来实现我们的新增功能，首先我们要在配置文件里写入代码如下

```
<insert id="insert" parameterType="com.itheima.bean.Student">
    INSERT INTO student VALUES (#{id},#{name},#{age})
</insert>
```

我们这里实现的是插入功能，由于所有的增删改功能返回的结果都是影响的行数而不是对应数据表内的数据内容，因此我们这里可以省略掉resultType标签。我们这里提供的参数就是我们要插入的整个对象，对象里有三个属性，因此我们在values内部也是用三个语句承载这三个数据

最后我们在java程序中写入代码如下

```
/*
    新增功能
*/
@Test
public void insert() throws Exception{
    //1.加载核心配置文件
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    //2.获取SqlSession工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    //3.通过工厂对象获取SqlSession对象
    //SqlSession sqlSession = sqlSessionFactory.openSession();
    SqlSession sqlSession = sqlSessionFactory.openSession(true);

    //4.执行映射配置文件中的sql语句，并接收结果
    Student stu = new Student(5,"周七",27);
    int result = sqlSession.insert("StudentMapper.insert", stu);

    //5.提交事务
    //sqlSession.commit();

    //6.处理结果
    System.out.println(result);

    //7.释放资源
    sqlSession.close();
    is.close();
}
```

我们这里其实演示了两种方式，一种手动提交，另一种是自动提交，这两种方式都是可以的。我们这里执行插入语句时，调用了sqlSession中的insert插入方法，然后内部又调用我们自己写入的sql插入配置语句

最后我们可以总结一下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaa3a327e8680f417f5dbb4f198e9754d.png)

## 修改功能的使用

那么接着我们来实现修改功能，首先我们将我们的配置文件的代码写入如下

```
<update id="update" parameterType="com.itheima.bean.Student">
    UPDATE student SET name = #{name},age = #{age} WHERE id = #{id}
</update>
```

可以看到我们这里压传入的参数，但是我们这里指定了一个学生类，而且指定了学生类里的id，age，name，这些属性，我们的配置文件运行时会取出我们传入的对应属性并用于执行对应的功能

那么我们可以写入我们的更新代码如下

```
/*
    修改功能
*/
@Test
public void update() throws Exception{
    //1.加载核心配置文件
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    //2.获取SqlSession工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    //3.通过工厂对象获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    //SqlSession sqlSession = sqlSessionFactory.openSession(true);

    //4.执行映射配置文件中的sql语句，并接收结果
    Student stu = new Student(5,"周七",37);
    int result = sqlSession.update("StudentMapper.update",stu);

    //5.提交事务
    sqlSession.commit();

    //6.处理结果
    System.out.println(result);

    //7.释放资源
    sqlSession.close();
    is.close();
}
```

学到这里我们或许也能稍微了解到MyBatis的框架的好处了，就是使用这个框架我们不用关心JDBC的底层的东西，我们只需要关注SQL语句就可以了（其实我完全不这么觉得，说实话还是很麻烦，没有啥改善）

最后我们同样可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8ad497f2e3dca2a93dfba3a3e439e22f.png)

## 删除功能的使用

接着我们来实现删除功能，写入配置文件的代码如下

```
<delete id="delete" parameterType="java.lang.Integer">
    DELETE FROM student WHERE id = #{id}
</delete>
```

然后我们写入java代码如下

```
/*
    删除功能
*/
@Test
public void delete() throws Exception{
    //1.加载核心配置文件
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    //2.获取SqlSession工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    //3.通过工厂对象获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    //SqlSession sqlSession = sqlSessionFactory.openSession(true);

    //4.执行映射配置文件中的sql语句，并接收结果
    int result = sqlSession.delete("StudentMapper.delete",5);

    //5.提交事务
    sqlSession.commit();

    //6.处理结果
    System.out.println(result);

    //7.释放资源
    sqlSession.close();
    is.close();
}
```

最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf7ea9be3591ac4632fd54046518f6104.png)

- 映射配置文件的小结

最后我们对本章节做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa1003ee63c0b7b43ab4916b3ef3004d3.png)

# 核心配置文件

接着我们来学习最后一个讲解内容，核心配置文件。

## 核心配置文件的介绍

首先我们来介绍下核心配置文件的各个语句的作用，具体的说明都写在了注释中了

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--configuration 核心根标签-->
<configuration>
    <!--environments配置数据库环境，环境可以有多个。default属性指定使用的是哪个-->
    <environments default="mysql">
        <!--environment配置数据库环境  id属性唯一标识-->
        <environment id="mysql">
            <!-- transactionManager事务管理     type属性，采用JDBC默认的事务-->
            <transactionManager type="JDBC"></transactionManager>
            <!-- dataSource数据源信息    type属性 连接池-->
            <dataSource type="POOLED">
                <!-- property获取数据库连接的配置信息-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://175.178.114.158:3306/db1"/>
                <property name="username" value="root"/>
                <property name="password" value="itheima"/>
            </dataSource>
        </environment>
    </environments>

    <!--mappers引入映射配置文件-->
    <mappers>
        <!-- mapper 引入指定的映射配置文件     resource属性指定映射配置文件的名称-->
        <mapper resource="StudentMapper.xml"/>
    </mappers>
</configuration>
```

这里我们的environment标签是可以写入多个的，改变defalut的值可以令其执行不同的environment标签内的配置内容

引入映射配置文件的代码其实也可以写多个，不过这里只写一个就可以了

总结请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6c4af317a121279e82318e3940b37f8d.png)

## properties标签的使用

根据我们的开发规范，我们的获取数据库连接的相关配置是不可以写死到我们的配置文件里的，我们是要将其额外抽取出来到我们的properties文件中的，那么我们可以构造properties文件并写入代码如下

```
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://175.178.114.158:3306/db1
username=root
password=itheima
```

然后将我们的核心配置文件的配置信息改造如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--configuration 核心根标签-->
<configuration>

    <!--引入数据库连接的配置文件-->
    <properties resource="jdbc.properties"/>

    <!--environments配置数据库环境，环境可以有多个。default属性指定使用的是哪个-->
    <environments default="mysql">
        <!--environment配置数据库环境  id属性唯一标识-->
        <environment id="mysql">
            <!-- transactionManager事务管理     type属性，采用JDBC默认的事务-->
            <transactionManager type="JDBC"></transactionManager>
            <!-- dataSource数据源信息    type属性 连接池-->
            <dataSource type="POOLED">
                <!-- property获取数据库连接的配置信息-->
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--mappers引入映射配置文件-->
    <mappers>
        <!-- mapper 引入指定的映射配置文件     resource属性指定映射配置文件的名称-->
        <mapper resource="StudentMapper.xml"/>
    </mappers>
</configuration>
```

## 起别名的使用

我们观察我们的映射配置文件，会看到我们映射配置文件里总是要引入很长的Student类的地址，这样很不方便，我们可以给这个类起一个别名，这样我们只需要写入这个别名就可以了，能让我们的代码变得简洁

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5885a271777b38da56aa2d45535c554d.png)

起别名需要两个标签，分别是父标签typeAliases和子标签typeAlias，子标签是真正用于添加别名的标签，其下有两个属性，分别是type，用于指定要取别名的类的地址，alias，用于给该类取别名。如果我们想要给整个包取别名，也可以调用typeAliases下的package标签

那么我们可以在核心配置文件中写入代码如下

```
<!--起别名-->
<typeAliases>
    <typeAlias type="com.itheima.bean.Student" alias="student"/>
</typeAliases>
```

然后我们就可以在映射配置文件中将我们的全路径全部改为我们的别名了，我们可以改造我们的映射配置代码如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--
    mapper:核心根标签
    namespace属性：名称空间
-->
<mapper namespace="StudentMapper">
    <!--
        select:查询功能的标签
        id属性：唯一标识
        resultType属性：指定结果映射对象类型
        parameterType属性：指定参数映射对象类型
    -->
    <select id="selectAll" resultType="student">
        SELECT * FROM student
    </select>

    <select id="selectById" resultType="student" parameterType="int">
        SELECT * FROM student WHERE id = #{id}
    </select>

    <insert id="insert" parameterType="student">
        INSERT INTO student VALUES (#{id},#{name},#{age})
    </insert>

    <update id="update" parameterType="student">
        UPDATE student SET name = #{name},age = #{age} WHERE id = #{id}
    </update>

    <delete id="delete" parameterType="java.lang.Integer">
        DELETE FROM student WHERE id = #{id}
    </delete>
</mapper>
```

我们这里将Interger类型的路径直接改为int是因为在MyBatis里已经将常用类型改别名了，其对应的别名如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEef860c4efa7cf86f058f433a3207a41e.png)

核心配置文件小结

最后我们可以做一个关于核心配置文件的小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE370cef1073d41a1eac289c1032085085.png)

# 传统方式实现Dao层

接着我们来进行我们的MyBatis的最后一个内容，就是用传统方式实现Dao层

## 环境介绍

我们先来介绍下什么是传统方式，所谓传统方式就是分层思想，我们将我们的层数分为控制层，业务层和持久层，其调用关系是从上往下的，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb549739b3a680bf56277e9db82b7960c.png)

然后我们就要在java中创建对应的包并完成其实现类，不过这里值得一提的是在MyBatis里，我们的最后一层不是Dao，而是mapper。其实就相当于是把Dao替换成了mapper，没了。

现在我们假设我们做好了一些预备工作了，比如说创建对应的各层的接口和实现类，那么我们就可以来正式去实现我们的代码了

## 实现Dao层

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

## LOG4J的使用

在日常的开发过程中，我们有时排查问题时还需要真正执行的SQL语句、参数、结果等信息，此时我们可以借助LOG4J的功能来实现执行信息的输出

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbdfc9f381762ad2ad9e77e78fd35db61.png)

来看看其使用步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE71f3a074856d42444f9d3cec32ccec16.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa2a0306553c22de40c3829eb657387b7.png)

可以看到这三行分别是实际执行的SQL语句、传入的参数以及总影响行数。

# 接口代理方式实现Dao层

之前我们用传统方式实现了Dao层，但是传统方式既要写接口，又要写实现类，很麻烦。MyBatis框架可以帮助我们省略编写Dao层接口实现类的步骤，我们只需要直接编写接口，接着由MyBatis框架根据接口的定义来创建接口的动态代理对象，但是使用这种方法是有实现规则的，具体规则如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4da4035d1098ed56bdf7dc182f2eb990.png)

## 代码实现

那么接着我们就来用接口代理的方式来实现我们的Dao层，请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcb9fb40fed9b8e3be047e260fc3bda88.png)

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

## 源码分析

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

**也就是说，最本质的东西其实还是反射，反射大法好**

那么了解了动态代理的执行原理之后，我们接着来看看insert方法是怎么执行的，由于其实动态代理的，因此其类我们要利用debug的方式来查看其执行原理

我们先在其调用方法的代码上设置断点

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc78b2306eb29e5a6c2b4f09ed0d76556.png)

然后进入其类中查看其代码，该类为MapperProxy，其利用反射机制获得其所需要的各种类和属性，然后我们进入到里面能看到很多方法，但这些我们需要关心，我们重点关心下面这两行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE422794f861f8350979054dd61e14cb45.png)

可以看到其获得了我们的映射方法类，然后调用了里面的execute方法，传入了sqlSession对象和参数，我们点进去execute中看看

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9c41eed2b0bd0ab51f4ff472fa8fd0df.png)

可以看到其先是获得方法执行的类型，然后进行了判断，调用其对应的方法，我们不难看到，其本质还是调用sqlSession里的对应方法，所以说其实到头来本质还是没变，只是省略了过程而已

那么最后我们可以对本节总结如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf69159d95013fa97f8a35a8bba8e8621.png)

- 小结

最后我们对本章学习的内容做一个总结，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0c69d80ac3dcab256c26b164657b0032.png)

# 动态sql

之前我们的学习了使用sql语句来进行相应的查询、添加、增加、删除等操作，但是那些操作都是比较简单的，而且他们都写死在了我们的映射配置文件中，如果我们以后有新的需求的话，那么我们又需要再增加一个查询语句，这太麻烦了。我们期待有这么一种方式，可以让我们的查询变成动态查询，我们希望我们的查询语句可以只用一条就实现我们的多种查询，而这也是我们本章节要学习的内容。

## 动态sql的介绍

我们可以做一个例子来对不使用动态sql造成的不便进行一个演示，假设我们现在要进行一个多条件查询，那么此时我们先在映射配置文件中写入对应的配置代码如下

```
<select id="selectCondition" resultType="student" parameterType="student">
    SELECT * FROM student WHERE id = #{id} AND name = #{name} AND age = #{age}
</select>
```

然后我们在对应的接口处增加多条件查询的方法

```
//多条件查询
public abstract List<Student> selectCondition(Student stu);
```

然后我们这里为了快捷就不整什么几把三层了，直接创建一个测试类并且调用我们的方法，我们可以写入代码如下

```
public class Test01 {
    @Test
    public void selectCondition() throws Exception{
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        SqlSession sqlSession = sqlSessionFactory.openSession(true);

        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

        Student stu = new Student();
        stu.setId(2);
        stu.setName("李四");
        stu.setAge(24);

        List<Student> list = mapper.selectCondition(stu);

        for (Student student:list) {
            System.out.println(student);
        }
    }
}

```

实际上这个的确可以查询到内容，但如果我们下次只是想要查询id为2和姓名为李四的成员，而不查询年龄的话，那么就没法了，因为此时我们会将年龄默认赋为null，这样我们什么都查询不到，而如果我们只想要执行查询前两个的话，就不得不重新构造代码，那可实在是太麻烦了，因此我们需要学习动态sql，来帮助解决这个问题

我们要学习的动态sql的标签主要有两个，一个是if，另一个是foreach

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3ca57adc3eba990e1de6c2802b6e4f1f.png)

## if标签的使用

那么接着我们就来使用对应的标签来实现动态sql，首先是if条件判断标签，但是在哪之前，我们要先学习where标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7d012bb3ab1fa0237a642f2fdc343255.png)

where标签的用法很简单，如果我们要使用动态条件，那么我们就应该要用where标签代替where关键字，并且将动态标签的内容写到where标签下。那么我们可以将我们的配置文件的代码改造如下

```
<select id="selectCondition" resultType="student" parameterType="student">
    SELECT * FROM student
    <where>
        <if test="id != null">
            id = #{id}
        </if>
        <if test="name != null">
            AND name = #{name}
        </if>
        <if test="age != null">
            AND age = #{age}
        </if>
    </where>
</select>
```

可以看到我们这里写入了SELECT * FROM student这些语句然后我们通过条件判断决定要不要给对应的值赋值，其中第一个之后的属性则需要再加上AND，这是因为本来两个条件连接就需要加AND关键词，不过按说第一个没加，第二个加了，第一个要是不赋值，就会变成没有第一个值直接一个AND套在第二个变量上才是，但实际测试发现没有，还是一个正常的表示，可能是内部有它自己的处理机制吧，这里我们记住就可以了

采用这种方式来设置配置文件之后，我们就可以进行一个动态sql了，此时我们如果只给我们的对象设置了姓名和年龄，那么id属性就不会被加入，具体是不是这样我们可以看在控制台上打印的信息来确认

```
DEBUG [main] - ==>  Preparing: SELECT * FROM student WHERE name = ? AND age = ?
        DEBUG [main] - ==> Parameters: 李四(String), 24(Integer)
        DEBUG [main] - <==      Total: 1
```

我们可以再控制台上看到上面的信息，说明的确是按照我们想要的方式进行了判断，因为这里执行的sql语句的确只使用了name和age

## foreach标签的使用

foreach标签主要用于遍历标签，适合用于多个参数直接的或者关系。简单来说就是假如我们想要用多个id查询用户，符合里面任何一个的都显示出来，此时就需要用到这个标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3d07df3021c795e2ea32a26dd7adf9cd.png)

那么我们先在对应的接口上创建对应的方法

```
//根据多个id查询
public abstract List<Student> selectByIds(List<Integer> ids);
```

然后我们再实现Dao层的方法，其代码如下

```
@Test
public void selectbyIds() throws Exception{
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    SqlSession sqlSession = sqlSessionFactory.openSession(true);

    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

    List<Integer> ids = new ArrayList<>();
    ids.add(1);
    ids.add(2);

    List<Student> list = mapper.selectByIds(ids);

    for (Student student:list) {
        System.out.println(student);
    }
}
```

接着我们来正式学一学foreach的使用，其下有四个属性，分别是collection、open、close以及separator。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7220b4bb049d6128d19a22d0326668a6.png)

其中collection属性是用于指定我们的传入的集合类型，是list还是数组，而open则是开始的SQL语句，一些sql语句的不会变动的前缀就放置于此，而close则是反过来的，不会变动的后缀放置于此，item则是参数的变量名，就是我们的数据到底要赋给谁，比如我们想要赋给id，并用于查找，那么此处就填入id（也可以理解为是要查询数据中的哪一列的相同数据），而separator则是集合内部元素的分隔符。那么我们可以构造我们的代码如下

```
<select id="selectByIds" resultType="student" parameterType="list">
    SELECT * FROM student
    <where>
        <foreach collection="list" open="id IN (" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```

我们可以再控制台上看到我们实际执行的sql语句，的确是我们所需要的，而且的确有两个

```
DEBUG [main] - ==>  Preparing: SELECT * FROM student WHERE id IN ( ? , ? )
DEBUG [main] - ==> Parameters: 1(Integer), 2(Integer)
DEBUG [main] - <==      Total: 2
```

## sql片段的抽取

我们容易观察到，我们的sql语句里，有很多重复代码，比如说SELECT * FROM student，这几乎每一个命令都需要重复写入一次，属实是太麻烦了。那么这时我们就可以用sql片段的抽取，将重复代码抽取到一起，然后用一个标签来代替他们使用，这样便于我们日后的维护。

sql片段抽取就可以满足我们的需求，抽取需要使用sql标签，其中有id属性，属性中要引入片段唯一标识，然后又include标签，include标签是可以使用抽取的sql的片段，其下有refid属性，属性下填入对应的片段唯一标识可以达到引入抽取的sql语句的效果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0b0e90973d9991e7493fee42275f3fe2.png)

那么我们可以将我们的代码简化如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--
    mapper:核心根标签
    namespace属性：名称空间
-->
<mapper namespace="com.itheima.mapper.StudentMapper">

    <sql id="select">SELECT  * FROM student</sql>

    <!--
        select:查询功能的标签
        id属性：唯一标识
        resultType属性：指定结果映射对象类型
        parameterType属性：指定参数映射对象类型
    -->
    <select id="selectAll" resultType="student">
        <include refid="select"/>
    </select>

    <select id="selectById" resultType="student" parameterType="int">
        <include refid="select"/> WHERE id = #{id}
    </select>

    <insert id="insert" parameterType="student">
        <include refid="select"/> VALUES (#{id},#{name},#{age})
    </insert>

    <update id="update" parameterType="student">
        UPDATE student SET name = #{name},age = #{age} WHERE id = #{id}
    </update>

    <delete id="delete" parameterType="java.lang.Integer">
        DELETE FROM student WHERE id = #{id}
    </delete>

    <select id="selectCondition" resultType="student" parameterType="student">
        <include refid="select"/>
        <where>
            <if test="id != null">
                id = #{id}
            </if>
            <if test="name != null">
                AND name = #{name}
            </if>
            <if test="age != null">
                AND age = #{age}
            </if>
        </where>
    </select>

    <select id="selectByIds" resultType="student" parameterType="list">
        <include refid="select"/>
        <where>
            <foreach collection="list" open="id IN (" close=")" item="id" separator=",">
                #{id}
            </foreach>
        </where>
    </select>
</mapper>


```

虽然说，采用标签的方式并没有让我们的代码量减少多少，但这样做主要是易于我们的维护

- 动态sql小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa6b3cf29122f2497b02ae30d81e418c3.png)

# 分页插件

本章节我们来学习在MyBatis里的分页插件，我们不妨先来看看关于分页插件的介绍

## 分页插件的介绍

分页插件可以将很多条结果进行分页显示，我们从中要得到的信息是当前在第几页，一共有几页，一页有多少条结果，这就是分页插件所要完成的事情。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc12e2f23074a010eb078583d4edbf7de.png)

我们不妨现在MySQL上来实现下我们的分页功能，当然，首先我们需要一些数据，假设我们的数据表中有以下数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa3ae3d5821a6a656978486f4008aa6d0.png)

然后我们在mysql中执行分页操作，可以写入其代码如下

```
-- 第一页：显示3条数据  当前页（当前页-1*每页显示条数）,每页显示条数
SELECT * FROM student LIMIT 0,3

-- 第二页：显示3条数据  当前页（当前页-1*每页显示条数）,每页显示条数
SELECT * FROM student LIMIT 3,3

-- 第三页：显示3条数据  当前页（当前页-1*每页显示条数）,每页显示条数
SELECT * FROM student LIMIT 6,3
```

我们上面的代码虽然可以执行分页操作，但是其不方便，可以看到我们要显示不同的页还要自己计算，突出一个傻逼，而且这里我们使用分页操作利用到了LIMIT关键词，而这个关键词又只有mysql支持，要是换了个数据库就没法用了，那这就很没用。此时我们的分页插件就应运而生了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1bc4904803a0c4093d454eff389dfd18.png)

我们要利用到的第三方的分页助手是PageHelper

## 分页插件的使用

接着我们来正式学习下分页插件PageHelper的使用，我们先来看看其实现步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE55cca9163302c4382b67fbbb89807a15.png)

然后我们首先导入jar包，然后我们就首先要在核心配置文件中集成我们的分页助手插件，写入如下代码就行了（没错，集成就是这么简单）

```
<!--集成分页助手插件-->
<plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

那么我们可以创建测试类并写入代码如下

```
package com.itheima.paging;

import com.github.pagehelper.PageHelper;
import com.itheima.bean.Student;
import com.itheima.mapper.StudentMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;

public class Test01 {
    @Test
    public void selectPaging() throws Exception{
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        SqlSession sqlSession = sqlSessionFactory.openSession(true);

        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

        //通过分页助手来实现分页功能
        //第一页：显示三条数据
        //PageHelper.startPage(1,3);
        //第二页：显示三条数据
        //PageHelper.startPage(2,3);
        //第三页：显示三条数据
        PageHelper.startPage(2,3);

        List<Integer> ids = new ArrayList<>();
        ids.add(1);
        ids.add(1);

        List<Student> list = mapper.selectAll();

        for (Student student:list) {
            System.out.println(student);
        }
    }
}

```

这里我们在获取结果，甚至于是传入参数之前就调用了分页助手，令其来帮助我们展示指定的页数，最后也能打印出我想要的数据

## 分页参数的获取

在我们的分页插件里，不但有分页功能，还有很多其他的方法可以帮助我们来获取很多其他的分页时所存在的信息。具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1ad07dcaaafe8d6d8f041f44ad943686.png)

我们可以在我们的测试类里写入对应的代码，来测试测试这些方法，那么我们可以写入代码如下

```
@Test
public void selectPaging() throws Exception{
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    SqlSession sqlSession = sqlSessionFactory.openSession(true);

    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

    //通过分页助手来实现分页功能
    //第一页：显示三条数据
    //PageHelper.startPage(1,3);
    //第二页：显示三条数据
    //PageHelper.startPage(2,3);
    //第三页：显示三条数据
    PageHelper.startPage(3,3);

    List<Integer> ids = new ArrayList<>();
    ids.add(1);
    ids.add(1);

    List<Student> list = mapper.selectAll();

    for (Student student:list) {
        System.out.println(student);
    }

    //获取分页相关参数
    PageInfo<Student> info = new PageInfo<>(list);
    System.out.println("总条数"+info.getTotal());
    System.out.println("总页数"+info.getPages());
    System.out.println("当前页："+info.getPageNum());
    System.out.println("每页显示条数"+info.getPageSize());
    System.out.println("上一页"+info.getPageSize());
    System.out.println("下一页"+info.getNextPage());
    System.out.println("是否是最后一页"+info.isIsLastPage());
    System.out.println("是否是第一页"+info.isIsFirstPage());
}
```

在上面的代码里我们调用了其各种方法来获得其各种参数的内容，实际得到的内容如下

```
    总条数8
            总页数3
当前页：3
        每页显示条数3
        上一页3
        下一页0
        是否是最后一页true
        是否是第一页false

        Process finished with exit code 0

```

这些内容都是符合我们所要的内容的，没有什么问题

- 分页插件的小结

最后我们可以对本章做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6ecc1633ea36b4698d9768bf0512ce41.png)

# 多表操作

本章节我们来学习MyBatis的多表操作，我们之前学习的都是十分简单的数据查询，随着业务的深入，我们必然是要使用到较为深入的多表操作的，因此我们学习多表操作。

## 多表模型的介绍

我们不妨先来复习下多表模型，免得大家忘了。在多表模型中，有一对一、一对多和多对多三种。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE18790018e45cbc69fa5f20aad4431911.png)

## 一对一的实现

首先我们来看看一对一的情况，其模型当然是经典的人和身份证

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdcf33bbb1a59d8dc3f7150c2321feb10.png)

然后我们来看看其数据准备，我们先做mysql的，其代码如下

```
DROP DATABASE db2;

    CREATE DATABASE db2;

    USE db2;

    CREATE TABLE person(
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(20),
    age INT
    );
    INSERT INTO person VALUES (NULL,'张三',23);
    INSERT INTO person VALUES (NULL,'李四',24);
    INSERT INTO person VALUES (NULL,'王五',25);

    CREATE TABLE card(
    id INT PRIMARY KEY AUTO_INCREMENT,
    number VARCHAR(30),
    pid INT,
    CONSTRAINT cp_fk FOREIGN KEY (pid) REFERENCES person(id)
    );
    INSERT INTO card VALUES (NULL,'12345',1);
    INSERT INTO card VALUES (NULL,'23456',2);
    INSERT INTO card VALUES (NULL,'34567',3);
```

然后我们更改一下我们的对应的映射配置文件和核心配置文件就完了，还有创建对应的person类和card类，card类中包含有person类当成员变量，我们这里假设我们就已经搞定了

- 一对一的功能实现

我们首先在对应的文件夹下创建对应的xml文件，然后引入对应的映射配置文件的固定代码，接着我们在核心配置文件上引入映射配置文件的路径。

```
<!--mappers引入映射配置文件-->
<mappers>
    <mapper resource="com/itheima/one_to_one/OneToOneMapper.xml"/>
</mappers>
```

然后我们在table01文件夹创建对应的接口并写入对应的方法

```
public interface OneToOneMapper {
    //查询全部
    public abstract List<Card> selectAll();
}
```

那么我们现在的目标是希望查找处所有的身份证，然后找出这些身份证所对应的人，如果是在sql中，我们容易编写出这样的语句

```
SELECT c.id cid,number,pid,NAME,age FROM card c,person p WHERE c.`pid`=p.`id`;
```

我们这里之所以不选择展示全部的原因是如果我们选择展示全部那么会有多的一列id会被寻找到，而这份id并没有什么作用，还妨碍我们将数据封装到对象中，因此我们的选择只展示我们所需要的一部分。这里我们展示的前两列的内容是我们身份证的数据，而后两列的内容则是对应的人的数据。

那么我们将这行语句放到我们的映射配置文件里的查询语句中，由于此时我们的要封装的对象涉及到了两个，一个是身份证，一个是人，此时再用resultType就不适合了，因为这玩意只适合封装一个对象，如果我们要进行多表操作时对象的封装，此时我们要改用resultMap属性，其是在select标签下的属性，该属性可以用于给对应的多表对象封装数据。然后我们额外创建新的resultMap标签，其下有id和type属性，前者是唯一标识，我们在select和resultMap上的id属性都应该要填写一致的，这样select才能正确使用到resultMap。后者则是我们的数据要封装到的数据类型，我们这里封装的是身份证，所以我们填入card，代表card类型。

然后在resultMap中我们如何封装我们的数据呢，我们要写入id的子标签，这个子标签有两个属性，column属性和property属性，后者用于指定对象中要封装数据的成员变量，前者表示要封装的数据在数据库中的列名，用于取出对应的列名并封装，这个可以写入多个，以给多个成员变量赋值。但是要注意的是，只有主键id的列我们采用id标签，其他应该采用result标签，其下的方法是一样的，用法也一样，无非是标签不同罢了

我们知道在Card中包含有Person引用变量，如果我们想要给对象中的另一个对象赋值的话，那么就不该还是用上面的方法，应该使用association标签，其可以配置被包含对象的映射关系，其下有两个属性，property和javaType，前者要填入被包含对象的变量名，后者要填入其对应的数据类型。我们也是通过id标签和result标签进行的赋值的

那么最终我们可以写入我们的配置文件的代码如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.table01.OneToOneMapper">
    <!--配置字段和实体对象属性的映射关系-->
    <resultMap id="oneToOne" type="card">
        <id column="cid" property="id"/>
        <result column="number" property="number"/>

        <!--
            association：配置被包含对象的映射关系
            property：被包含对象的变量名
            javaType：被包含对象的数据类型
        -->
        <association property="p" javaType="person">
            <id column="pid" property="id"/>
            <result column="name" property="name"/>
            <result column="age" property="age"/>
        </association>
    </resultMap>

    <select id="selectAll" resultMap="oneToOne">
        SELECT c.id cid,number,pid,NAME,age FROM card c,person p WHERE c.pid=p.id;
    </select>
</mapper>
```

然后我们写入测试文件的代码如下

```
@Test
public void selectAll() throws Exception{
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    SqlSession sqlSession = sqlSessionFactory.openSession(true);

    //获取OneToOneMapper接口的实现类对象
    OneToOneMapper mapper = sqlSession.getMapper(OneToOneMapper.class);

    List<Card> list = mapper.selectAll();

    for (Card c:list) {
        System.out.println(c);
    }

    sqlSession.close();
    is.close();
}
```

实际测试也没有问题，此时就说明我们的功能已经完成了。

最后我们可以再做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9dfd0ddeb178c3cbb6fd421f670b7815.png)

## 一对多的实现

那么接着我们来学习一对多的多表操作，其最经典的模型莫过于是学生和班级，一个班级可以有多个学生，多个学生可以是一个班级的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE27f76f05f11c571c9b0fcd222cc0628c.png)

我们首先要做一些对应的环境准备，先来看看创造数据sql语句

```
CREATE TABLE classes(
id INT PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(20)
);
INSERT INTO classes VALUES (NULL,'黑马一班');
INSERT INTO classes VALUES (NULL,'黑马二班');


CREATE TABLE student(
id INT PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(30),
age INT,
cid INT,
CONSTRAINT cs_fk FOREIGN KEY (cid) REFERENCES classes(id)
);
INSERT INTO student VALUES (NULL,'张三',23,1);
INSERT INTO student VALUES (NULL,'李四',24,1);
INSERT INTO student VALUES (NULL,'王五',25,2);
INSERT INTO student VALUES (NULL,'赵六',26,2);
```

然后是一些在idea上的对应准备，这里就省略了，反正无非也就是创建对应的班级类和学生类，班级类中有多个学生类，用集合保存

- 一对多的功能实现

接着我们来实现我们的一对多的功能，首先我们还是先构造出我们的查询的sql语句，因为我们的查询的语句比较复杂，所以我们先构造出来，这样不容易犯错，那么我们可以构造其语句如下

```
SELECT c.id cid,c.name cname,s.id sid,s.name sname,s.age sage FROM classes c,student s WHERE c.`id`=s.`cid`;
```

构造的基本思路和上题差不多，这里不再赘述。然后我们创建一个配置文件并在核心配置文件中将其映射，接着我们同样进入对应的封装语句的构造，不过这里要注意的是，我们这里由于是一对多的操作，因此给被包含对象赋值时不可以使用association标签，这标签是专为一对一准备的。此时我们应该要使用collection标签，其可以用于一对多的使用，其下有两个属性property和ofType，前者存放被包含对象的变量名，后者存放集合内部存放的对象的数据类型，其实和前面的差不多，那么我们可以构造其代码如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.table02.OneToManyMapper">
    <!--配置字段和实体对象属性的映射关系-->
    <resultMap id="oneToMany" type="classes">
        <id column="cid" property="id"/>
        <result column="cname" property="name"/>

        <!--
            collection:配置被包含的集合对象的映射关系
            property:被包含的对象的变量名
            ofType:被包含对象的实际参数类型
        -->
        <collection property="students" ofType="student">
            <id column="sid" property="id"/>
            <result column="sname" property="name"/>
            <result column="sage" property="age"/>
        </collection>
    </resultMap>

    <select id="selectAll" resultMap="oneToMany">
        SELECT c.id cid,c.name cname,s.id sid,s.name sname,s.age sage FROM classes c,student s WHERE c.id=s.cid;
    </select>
</mapper>


```

当然我们不要忘了在核心配置文件上配置我们对应的映射配置文件的路径，不然我们的程序是无法运行的。同时要创造对应的接口以用于动态生成实现类

```
package com.itheima.table02;

import com.itheima.bean.Classes;

import java.util.List;

public interface OneToManyMapper {
    //查询全部
    public abstract List<Classes> selectAll();
}
```

最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdc01f7247fd24034cce642f981c5cf65.png)

## 多对多的实现

多对多的最经典模型莫过于是学生和课程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEacf5c38d0ac63a10babeeed42f6a693c.png)

我们首先要做多对多的数据准备，先来看看我们的sql语句

```
CREATE TABLE course(
id INT PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(20)
);
INSERT INTO course VALUES (NULL,'语文');
INSERT INTO course VALUES (NULL,'数学');


CREATE TABLE stu_cr(
id INT PRIMARY KEY AUTO_INCREMENT,
sid INT,
cid INT,
CONSTRAINT sc_fk1 FOREIGN KEY (sid) REFERENCES student(id),
CONSTRAINT sc_fk2 FOREIGN KEY (cid) REFERENCES course(id)
);
INSERT INTO stu_cr VALUES (NULL,1,1);
INSERT INTO stu_cr VALUES (NULL,1,2);
INSERT INTO stu_cr VALUES (NULL,2,1);
INSERT INTO stu_cr VALUES (NULL,2,2);
```

然后是经典创建对应的包和类了，这里就不赘述了。不过我们这里值得一提的是我们并没有创建中间表的对象，这是为什么呢？这是因为我们只在这里演示查询操作，因此我们没有必要去保存中间表对象，所以我们不创造，主要还是贪方便。像之前的外键约束的名称那些的实体类我们也是没有提供的，这个了解下就行了

- 多对多的功能实现

在实现其功能之前，我们需要将我们的学生实体类里添加这么一行代码

```
private List<Course> courses;  //学生所选择的课程集合
```

就是往里面添加了一个课程集合，因为学生是包含有课程集合的，我们往里面添加这行代码自然也无可厚非

首先我们创造一个对应的映射配置文件，然后我们在核心配置文件中添加该映射配置文件的地址。

然后我们创建对应的接口类并写入对应的代码

```
package com.itheima.table03;

import com.itheima.bean.Student;

import java.util.List;

public interface ManyToManyMapper {
    //查询全部
    public abstract List<Student> selectAll();
}
```

然后我们就开始写入我们的对应的配置文件的代码了，首先我们要创造我们对应的sql语句

```
SELECT sc.sid,s.name sname,s.age sage,sc.cid,c.name cname FROM student s,course c,stu_cr sc WHERE sc.sid=s.id AND sc.cid=c.id;
```

我们这里特别提一下我们是怎么创建我们的对应的sql语句的，首先我们要确定的当然是查询的总体代码，我们先查询出来，至于显示什么，怎么显示我们先按下不表，此时我们容易构造出FROM及其后面的语句，然后我们发现展示的数据又多又杂乱，不适合我们封装数据，此时我们再选择性展示数据，我们从中选择我们要封装的数据，先从第一个对象也就是学生开始，拿下两列，再拿下第一个对象里的包含对象，也就是课程对象，在表里对标的是选择展示对应的三列，用于封装课程对象，给过长的名字进行对应的命名，最后我们就会得到一个我们想要的便于封装的数据展示

那么我们可以写入我们的配置文件的代码如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.table03.ManyToManyMapper">
    <resultMap id="manyToMany" type="student">
        <id column="sid" property="id"/>
        <result column="sname" property="name"/>
        <result column="sage" property="age"/>
        <collection property="courses" ofType="course">
            <id column="cid" property="id"/>
            <result column="cname" property="name"/>
        </collection>
    </resultMap>

    <select id="selectAll" resultMap="manyToMany">
        SELECT sc.sid,s.name sname,s.age sage,sc.cid,c.name cname FROM student s,course c,stu_cr sc WHERE sc.sid=s.id AND sc.cid=c.id;
    </select>
</mapper>
```

其实我们可以看到多对多的方式和一对一的方式差不多，只不过是查询语句变了些而已。

最后我们做个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1b45644fa5fbfe496f80488bc8bb269b.png)

- 多表操作的小结

最后我们可以做一个多表操作的小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE19772fda01e30b808d7fc5946e6e989c.png)

# 注解开发

我们之前的开发都是利用配置文件的形式开发的，非常不方便，还要写一大堆，我们现在来学习使用注解形式开发。值得一提的是并不是说我们学习了注解形式开发之后配置文件的形式就不需要了，需不需要主要取决于公司的需求

## 注解开发的介绍

注解形式的开发需要用到常用注解，具体我们要学习的常用注解请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcc5afbc21f5715f720b1bdca66d8f410.png)

## 注解实现查询的操作

我们先来看看我们实现的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE25158d498a9c6b55e1043cbb8dced90c.png)

我们创建接口并写入方法然后使用对应的查询注解并在注解里写入对应的实现sql语句

```
package com.itheima.mapper;

import com.itheima.bean.Student;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface StudentMapper {
    //查询全部
    @Select("SELECT * FROM student")
    public abstract List<Student> selectAll();
}
```

接着我们在核心配置文件里面配置映射

```
<!--配置应关系-->
<mappers>
    <package name="com.itheima.mapper"/>
</mappers>
```

这里我们要使用子标签package，内部只要输入我们的接口所在的包名就可以了

最后我们写入测试类的代码如下

```
package com.itheima.test;

import com.itheima.bean.Student;
import com.itheima.mapper.StudentMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class Test01 {
    @Test
    public void selectAll() throws Exception {
        //1.加载核心配置文件
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

        //2.获取SqlSession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        //3.通过工厂对象获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession(true);

        //4.获取StudentMapper接口的实现类对象
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

        //5.调用实现类对象中的方法，接收结果
        List<Student> list = mapper.selectAll();

        //6.处理结果
        for (Student student:list) {
            System.out.println(student);
        }

        //7.释放资源
        sqlSession.close();
        is.close();
    }
}

```

经过测试我们发现这是可以运行的，没有任何问题。那我只能说我草了，这他妈的比几把什么配置文件方便太多了啊 ，鬼才会用配置文件啊，反正是我我肯定用注解

最后我们这里的代码运行过程是先动态获取实现类，调用实现类的方法，然后通过注解的信息执行对应的方法最后返回相应的结果

## 注解实现新增和修改

本节我们来学习用注解来实现新增操作，请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE699652e14e722b89e26e933836ffef65.png)

首先我们在接口类中写入对应方法的代码和注解

```
//新增操作
@Insert("INSERT INTO student VALUES (#{id},#{id},#{age})")
public abstract Integer insert(Student stu);
```

然后我们将我们的测试代码稍作修改，其实就是将之前的获取集合改为传入一个自己的对象然后打印返回的影响条数就行了

```
@Test
public void insert() throws Exception {
    //1.加载核心配置文件
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    //2.获取SqlSession工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    //3.通过工厂对象获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession(true);

    //4.获取StudentMapper接口的实现类对象
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

    //5.调用实现类对象中的方法，接收结果
    Student stu = new Student(5,"周七",26);
    Integer result = mapper.insert(stu);

    //6.处理结果
    System.out.println(result);

    //7.释放资源
    sqlSession.close();
    is.close();
}
```

- 注解实现修改的操作

首先在接口中写入代码如下

```
//修改操作
@Update("UPDATE student SET name=#{name},age=#{age} WHERE id=#{id}")
public abstract Integer update(Student stu);
```

然后在测试类中稍微改改代码就完了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE604be88475e040e54a2a03858eec9823.png)

- 注解实现删除的操作

首先在接口中写入代码如下

```
//删除操作
@Delete("DELETE FROM student WHERE id=#{id}")
public abstract Integer delete(Integer id);
```

然后修改下测试类的代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd0c6d05235a4bf2bb06f61a35b1b71f6.png)

- 注解开发的小结

在本章中，我们可以很明显地感受到注解开发的好处，他令我们不用去花时间关注什么几把的传入参数，传回参数，我们只需要嗯用注解就可以了，真的方便太多了。最后我们可以看看本章的总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE292da629ea4bcc4c21d6fc1fef18a1f7.png)

# 注解多表操作

接下来我们学习用注解来实现多表操作，没错，又又又回来了，就突出一个烦，但是没办法，学吧。

## 一对一

我们的一对一的环境准备里有很多是跟之前一样的，有所不同的是我们这里在创建的onetoone包里我们要写入两个接口并写入对应的代码，第一个是CardMapper，其代码是

```
package com.itheima.one_to_one;

import com.itheima.bean.Card;

import java.util.List;

public interface CardMapper {
    //查询全部
    public abstract List<Card> selectAll();
}
```

第二个是PersonMapper

```
package com.itheima.one_to_one;

import com.itheima.bean.Person;
import org.apache.ibatis.annotations.Select;

public interface PersonMapper {
    //根据id查询
    @Select("SELECT * FROM person WHERE id=#{id}")
    public abstract Person selectById(Integer id);
}

```

那我们可以看到PersonMapper里实现了根据id查找，但是在CardMapper里还没做什么，别急，怎么在里面做文章令其实现一对一的多表操作是下一节的事情。

- 一对一的实现

接下来我们正式来实现一对一多表操作的功能，首先我们在CardMapper接口类里写入对应的查询全部card对象的查询语句，当然，是使用注释实现的。那么我们可以写入@Select("SELECT * FROM card")

然后我们就要用到Results注释了，我们首先要输入一个Results注释，然后加入小括号，小括号内部加大括号，然后回车在大括号内继续写代码

我们在大括号内使用Result注释，注释内有两个属性，分别是column和property，前者表示表中的数据列的名称，后者表示要给我们创建的实体类中赋值的属性名称，我们这里先给我们的Card对象赋值，因此我们我们这里先输入两个Result注释进行赋值。那么通过上面的叙述我们也知道，Reuslt注释的作用之一就是用于封装对象

接着我们我们要解决的一个问题是，我们的card对象里还有p这个引用数据，我们要如何给它赋值呢？我们的一个想法就是我们可以拿第一次查询cid来去查询我们的person表，查询到cid和id相同的数据并封装进去，那么我们要如何实现这个思路呢？我们同样要使用Result注释，写入小括号然后回车，内有property和javaType属性，前者存放被包含对象的变量名，这里就是其引用变量的名字p，后者存档被包含对象的实际数据类型的class文件。然后还有colum属性，该属性可以拿到我们数据列中对应的数据，我们这里输入pid，意为拿到pid的数据，然后我们要继续写入one = @one()，这个格式是一对一表中所使用的固定格式，可以让我们执行指定接口类的查询方法，内有select属性，该属性通过填入查询方法的所在类的对应方法的所在地址来执行这个方法，同时，执行这个查询方法时需要的参数就由我们上面的pid传入。

当然，这个可以执行的前提是，你先创造好了对应的接口类并提供了对应的执行查询方法的注解，要不然都没有这个玩意你怎么查啊是吧。那么我们可以构造代码如下

```
package com.itheima.one_to_one;

import com.itheima.bean.Card;
import com.itheima.bean.Person;
import org.apache.ibatis.annotations.One;
import org.apache.ibatis.annotations.Result;
import org.apache.ibatis.annotations.Results;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface CardMapper {
    //查询全部
    @Select("SELECT * FROM card")
    @Results({
            @Result(column = "id",property = "id"),
            @Result(column = "number",property = "number"),
            @Result(
                    property = "p",          //被包含对象的变量名
                    javaType = Person.class, //被包含对象的实际数据类型
                    column = "pid",          //根据查询处的card表中的pid字段来查询person表
                    /*
                        one、@One 一对一固定写法
                        select属性：指定调用哪个接口中的哪个方法
                     */
                    one = @One(select = "com.itheima.one_to_one.PersonMapper.selectById")
            )
    })
    public abstract List<Card> selectAll();
}
```

然后我们构造测试类代码如下

```
@Test
public void selectAll() throws Exception {
    //1.加载核心配置文件
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    //2.获取SqlSession工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    //3.通过工厂对象获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession(true);

    //4.获取StudentMapper接口的实现类对象
    CardMapper mapper = sqlSession.getMapper(CardMapper.class);

    //5.调用实现类对象中的方法，接收结果
    List<Card> list = mapper.selectAll();

    //6.处理结果
    for (Card card:list) {
        System.out.println(card);
    }

    //7.释放资源
    sqlSession.close();
    is.close();
}
```

然后我们会发现我们的代码整得没有毛病，可以运行并查出结果，那这样的话就可以了

当然，不要忘了在对应的核心配置文件里写入对应的配置代码

```
<!--配置映射关系-->
<mappers>
    <package name="com.itheima"/>
</mappers>
```

注意到我们的核心配置文件里配置的具有映射关系的是直接写在com.itheima这个大包下的，这样我们的测试代码在寻找对应的注解的时候就会从整个包下寻找，保证能寻找到。换言之，如果我们有多个子包，子包下都有不同接口注释类，那么我们可以将映射关系直接定义在包含子包的大包下

最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe36836808cba8a7ba542e4ab890bd6ce.png)

## 一对多

- 一对多的环境介绍

一对多的模型是经典的学生对班级

我们首先要先创造出一对多的对应的环境，我们数据表中的数据就挺好的，能用，而且没啥问题。然后我们要创建对应的实体类，这个也没有问题，因为实体类我们之前就创造过了。最后我们要创建两个接口类并写入对应的代码，首先是StudentMapper，写入代码如下

```
package com.itheima.ome_to_many;

import com.itheima.bean.Student;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface StudentMapper {
    //根据cid查询student表
    @Select("SELECT * FROM student WHERE cid=#{cid}")
    public abstract List<Student> selectByCid(Integer cid);
}

```

然后是ClassesMapper，写入代码如下

```
package com.itheima.ome_to_many;

import com.itheima.bean.Classes;

import java.util.List;

public interface ClassesMapper {
    //查询全部
    public abstract List<Classes> selectAll();
}

```

其实为什么有StudentMapper这个其实大家懂得都懂了，这肯定是因为后面我们要用cid来查找哪个对应的学生，由于一个班级里能有多个学生，所以返回的是一个学生集合

- 一对多的实现

我们一对多的代码实现其实还是跟以前得差不多，不同的是我们的one变成了many而已，具体请看代码

```
package com.itheima.ome_to_many;

import com.itheima.bean.Classes;
import org.apache.ibatis.annotations.Many;
import org.apache.ibatis.annotations.Result;
import org.apache.ibatis.annotations.Results;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface ClassesMapper {
    //查询全部
    @Select("SELECT * FROM classes")
    @Results({
            @Result(column = "id",property = "id"),
            @Result(column = "name",property = "name"),
            @Result(
                    property = "students",        //被包含对象的变量名
                    javaType = List.class,        //被包含对象的实际数据类型的class文件
                    column = "id",                //根据查询处的classes表的id字段来查询student表
                    /*
                        many、@Many 一对多查询的固定写法
                        select属性：指定调用哪个接口中的哪个查询方法
                     */
                    many = @Many(select = "com.itheima.ome_to_many.StudentMapper.selectByCid")
            )
    })
    public abstract List<Classes> selectAll();
}

```

然后我们写入测试类如下

```
package com.itheima.ome_to_many;

import com.itheima.bean.Card;
import com.itheima.bean.Classes;
import com.itheima.bean.Student;
import com.itheima.one_to_one.CardMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.List;

public class Test01 {
    @Test
    public void selectAll() throws Exception {
        //1.加载核心配置文件
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

        //2.获取SqlSession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        //3.通过工厂对象获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession(true);

        //4.获取StudentMapper接口的实现类对象
        ClassesMapper mapper = sqlSession.getMapper(ClassesMapper.class);

        //5.调用实现类对象中的方法，接收结果
        List<Classes> list = mapper.selectAll();

        //6.处理结果
        for (Classes cls:list) {
            System.out.println(cls.getId()+","+cls.getName());
            List<Student> students = cls.getStudents();
            for (Student student:students) {
                System.out.println("\t"+student);
            }
        }

        //7.释放资源
        sqlSession.close();
        is.close();
    }
}

```

那么我们的一对多就算是实现完毕了

最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE11a5507121cde5476f1f943d93b5f0cf.png)

## 多对多

- 多对多的环境介绍

经典学生对课程，没啥好说的

同样写入两个接口的代码

```
package com.itheima.many_to_many;

import com.itheima.bean.Course;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface CourseMapper {
    //根据学生id查询所选课程
    @Select("SELECT c.id,c.name FROM stu_cr sc,course c WHERE sc.cid=c.id AND sc.sid=#{id}")
    public abstract List<Course> selectBySid(Integer id);
}

```

我们这里解释下为什么我们这里的查询代码是SELECT c.id,c.name FROM stu_cr sc,course c WHERE sc.cid=c.id AND sc.sid=#{id}，这是因为我们最终肯定是要通过学生选择的课程号得到对应的课程的，那么我们查询时就通过学生的课程号查询到中间表中对应的号码，也就是sid，然后我们要通过sid查询到对应的含有相同id的课程，然后将其打印，所以我们将查询语句写成这样。说白了其实就是多对多里我们要通过中间表定位到我们所需要的数据，所以我们要构建较为复杂的sql代码

```
package com.itheima.many_to_many;

import com.itheima.bean.Student;

import java.util.List;

public interface StudentMapper {
    //查询全部
    public abstract List<Student> selectAll();
}

```

- 多对多的实现

为什么我们可以Results下写多个Result注解呢，这是因为该注解类下有一个Result注解类数组........

```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})
public @interface Results {
    String id() default "";

    Result[] value() default {};
}
```

言归正传，我们最终可以构造代码如下

```
package com.itheima.many_to_many;

import com.itheima.bean.Student;
import org.apache.ibatis.annotations.Many;
import org.apache.ibatis.annotations.Result;
import org.apache.ibatis.annotations.Results;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface StudentMapper {
    //查询全部
    //@Select("SELECT * FROM student")
    @Select("SELECT DISTINCT s.id,s.name,s.age FROM student s,stu_cr sc WHERE sc.sid=s.id")
    @Results({
            @Result(column = "id",property = "id"),
            @Result(column = "name",property = "name"),
            @Result(column = "age",property = "age"),
            @Result(
                    property = "courses",  // 被包含对象的变量名
                    javaType = List.class, // 被包含对象的实际数据类型
                    column = "id",         // 根据查询处student表的id来作为关联条件，去查询中间表和课程表
                    /*
                        many、@Many 一对多查询的固定写法
                        select属性：指定调用哪个接口中的哪个查询方法
                     */
                    many = @Many(select = "com.itheima.many_to_many.CourseMapper.selectBySid")
            )
    })
    public abstract List<Student> selectAll();
}

```

这里如果我们采用第一个查询语句，也就是13行的代码，那么会将剩下两个没有选择课程的学生也查出来，而我们所希望查出来的内容是选择了课程的学生，因此我们这里的查询语句是查询所有和中间表有相同标识id的学生，同时加入DISTINCT关键字用于去重，由于无法对*去重，因此这里指定了展示的内容

其他的和我们之前的大差不差了，这里就不再赘述了。最后我们来看看本节总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1b8579637137a0128a52639c14592332.png)

- 注解多表操作的小结

最后我们来看看本章总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8cf8ae8a5ea74c69c6853e54953824c0.png)

# 自动构建SQL

我们之前构建我们的SQL语句的方式都是采用自己直接手动构建的，这种方式不但不方便而且容易出错，因此在MyBatis中提供了一个工具类可以帮助我们构建对应的查询语句，这个功能类的名字就叫做SQL，本章节我们就来学习如果利用SQL功能类构建我们所需要的SQL语句

## SQL功能类的介绍

SQL功能类下有很多方法，其需要传入对应的一些参数来进行调用，传入其所需要的字符，其会自动将其拼接一个SQL语句，可以直接为我们所用，具体什么请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE85789167262d48edd4bc7623e1052b23.png)

接下来我们来做一个案例来演示，请看代码

```
package com.itheima.sql;

import org.apache.ibatis.jdbc.SQL;

public class sqlTest {
    public static void main(String[] args) {
        String sql = getSql();
        System.out.println(sql);
    }

/*    //定义方法获取查询student表的sql语句
    public static String getSql() {
        String sql = "SELECT * FROM student";
        return sql;
    }*/

    //定义方法获取查询student表的sql语句
    public static String getSql() {
        return new SQL(){
            {
                SELECT("*");
                FROM("student");
            }
        }.toString();
    }
}

```

我们这里创建SQL对象采用了匿名内部类的方式，在内部调用对应的方法然后利用toString方法令其返回我们所需要的语句，我们内部调用了SELECT和FROM方法。实际在我们的SQL功能类中还有很多很多的对应构建sql语句的方法，这里就不一一展示了

## 查询功能的实现

接着我们来实现我们的查询功能，构建查询全部的SQL我们上一节已经学习过了，所以这不是难点。这里难点在于我们怎么将我们生成的sql语句加入到我们的注解中。这时，就得靠我们的SelectProvider注解了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4c42c05d9309fdaf3a35d93abd32d2a5.png)

SelectProvider可以生成查询用的SQL语句，其下有两个属性，type和method，前者要传入生成SQL语句的功能类的class文件，后者则要直接写入我们想要调用的方法名，不需要加任何的括号。那么我们可以先构造我们的功能类并写入代码如下

```
package com.itheima.sql;

import org.apache.ibatis.jdbc.SQL;

public class ReturnSql {
    //定义方法,返回查询的sql语句
    public static String getSelectAll() {
        return new SQL(){
            {
                SELECT("*");
                FROM("student");
            }
        }.toString();
    }
}

```

然后我们将对应的注解类代码修改如下

```
//查询全部
//@Select("SELECT * FROM student")
@SelectProvider(type = ReturnSql.class,method = "getSelectAll")
public abstract List<Student> selectAll();
```

那么这样我们就算是利用SQL并获得了其构建的sql语句了，实际测试之后发现没有问题，那么我们查询操作的语句就实现完毕了

## 新增功能的实现

同样我们可以写入其功能类代码如下

```
//定义方法，返回新增的sql语句
public String getInsert(Student stu){
    return new SQL(){
        {
            INSERT_INTO("student");
            INTO_VALUES("#{id},#{name},#{age}");
        }
    }.toString();
}
```

这里我们完成的插入操作，因此我们这里内部写入的代码是INSERT_INTO以及INTO_VALUES，而且我们新增sql语句需要传入一个学生对象，因此这里的接受参数需要一个学生对象。然后我们将我们的注释代码修改如下

```
//新增操作
//@Insert("INSERT INTO student VALUES (#{id},#{name},#{age},#{cid})")
@InsertProvider(type = ReturnSql.class,method = "getInsert")
public abstract Integer insert(Student stu);
```

这里我们执行的是插入操作，所以我们对应的注释也是InsertProvider，两个属性是没有变化的，还是照之前的填写就行了。

当我们执行测试类的时候，调用方法时就传入一个要新增的学生对象就行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9b8760365559cccd36374a6960b30e51.png)

## 修改功能的实现

没啥值得说的，看功能类代码吧

```
//定义方法，返回修改的sql语句
public String getUpdate(Student stu) {
    return new SQL() {
        {
            UPDATE("student");
            SET("name=#{name}","age=#{age}");
            WHERE("id=#{id}");
        }
    }.toString();
}
```

然后是注解的代码

```
//修改操作
//@Update("UPDATE student SET name=#{name},age=#{age} WHERE id=#{id}")
@UpdateProvider(type = ReturnSql.class,method = "getUpdate")
public abstract Integer update(Student stu);
```

## 删除功能的实现

最后我们来实现我们的删除功能，先来看功能类代码

```
//定义方法，返回删除的sql语句
public String getDelete(Integer id) {
    return new SQL() {
        {
            DELETE_FROM("student");
            WHERE("id=#{id}");
        }
    }.toString();
}
```

然后是注解的代码

```
//删除操作
//@Delete("DELETE FROM student WHERE id=#{id}")
@UpdateProvider(type = ReturnSql.class,method = "getDelete")
public abstract Integer delete(Integer id);
```

- 构建SQL的小结

最后我们可以对本章做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbd15e0776bb9c6372f45679de4298fe3.png)

# 综合案例

之前我们做过一个学生管理系统里的添加案例，当时我们是使用JDBC进行实现的，采用了传统的三层调用进行实现，现在我们既然学习了注解方式，我们大可以用注解来进行开发。所以本节我们就要将我们之前实现好的学生管理系统改用注解的形式来实现

## 学生管理系统的介绍和环境搭建

之前我们用三层进行实现，Dao层里具有其实现类，既然现在我们要用注解形式开发，那么就不需要实现类，所以我们直接删除掉这个实现类，然后我们对应的接口中写入其代码如下

```
package com.itheima.dao;

import com.itheima.domain.Student;
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;

import java.util.ArrayList;

/*
    Dao层接口
 */
public interface StudentDao {
    //查询所有学生信息
    @Select("SELECT * FROM student")
    public abstract ArrayList<Student> findAll();

    //条件查询，根据id获取学生信息
    @Select("SELECT * FROM student WHERE sid=#{sid}")
    public abstract Student findById(Integer sid);

    //新增学生信息
    @Insert("INSERT INTO student VALUES (#{sid},#{name},#{age},#{birthday})")
    public abstract int insert(Student stu);

    //修改学生信息
    @Update("UPDATE student SET name=#{name},age=#{age},birthday=#{birthday} WHERE sid=#{sid}")
    public abstract int update(Student stu);

    //删除学生信息
    @Delete("DELETE FROM student WHERE sid=#{sid}")
    public abstract int delete(Integer sid);
}
```

我们这里图方便就不搞什么构建SQL了，直接写入我们所要执行的查询语句

## 学生管理系统的实现

然后我们进入到service层，由于Dao层已经被我们删了，所以service层肯定是无法调用Dao层的，我们将其整体的代码都推翻重写，我们主要在service的实现类里做调用其接口的方法的事情，其实现类会动态帮我们生成。那么我们可以写入代码如下

```
package com.itheima.service.impl;

import com.itheima.dao.StudentDao;
import com.itheima.domain.Student;
import com.itheima.service.StudentService;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;

/**
 * 学生的业务层实现类
 * @author 黑马程序员
 * @Company http://www.itheima.com
 */
public class StudentServiceImpl implements StudentService {

    @Override
    public List<Student> findAll() {
        ArrayList<Student> list = null;
        SqlSession sqlSession = null;
        InputStream is = null;
        try{
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.获取StudentDao接口的实现类对象
            StudentDao mapper = sqlSession.getMapper(StudentDao.class);

            //5.调用实现类对象的方法，接收结果
            list = mapper.findAll();

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if(sqlSession != null) {
                sqlSession.close();
            }
            if(is != null) {
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

    @Override
    public Student findById(Integer sid) {
        Student stu = null;
        SqlSession sqlSession = null;
        InputStream is = null;
        try{
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.获取StudentDao接口的实现类对象
            StudentDao mapper = sqlSession.getMapper(StudentDao.class);

            //5.调用实现类对象的方法，接收结果
            stu = mapper.findById(sid);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if(sqlSession != null) {
                sqlSession.close();
            }
            if(is != null) {
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

    @Override
    public void save(Student student) {
        SqlSession sqlSession = null;
        InputStream is = null;
        try{
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.获取StudentDao接口的实现类对象
            StudentDao mapper = sqlSession.getMapper(StudentDao.class);

            //5.调用实现类对象的方法，接收结果
            mapper.insert(student);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if(sqlSession != null) {
                sqlSession.close();
            }
            if(is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    @Override
    public void update(Student student) {
        SqlSession sqlSession = null;
        InputStream is = null;
        try{
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.获取StudentDao接口的实现类对象
            StudentDao mapper = sqlSession.getMapper(StudentDao.class);

            //5.调用实现类对象的方法，接收结果
            mapper.update(student);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if(sqlSession != null) {
                sqlSession.close();
            }
            if(is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    @Override
    public void delete(Integer sid) {
        SqlSession sqlSession = null;
        InputStream is = null;
        try{
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.获取StudentDao接口的实现类对象
            StudentDao mapper = sqlSession.getMapper(StudentDao.class);

            //5.调用实现类对象的方法，接收结果
            mapper.delete(sid);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if(sqlSession != null) {
                sqlSession.close();
            }
            if(is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

最后经过测试发现其都没有问题，那么我们的综合案例就搞定了，其比我们之前的用JDBC实现的方式要简单不少