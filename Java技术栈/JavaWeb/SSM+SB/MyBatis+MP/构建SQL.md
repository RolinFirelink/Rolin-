我们之前构建我们的SQL语句的方式都是采用自己直接手动构建的，这种方式不但不方便而且容易出错，因此在MyBatis中提供了一个工具类可以帮助我们构建对应的查询语句，这个功能类的名字就叫做SQL，本章节我们就来学习如果利用SQL功能类构建我们所需要的SQL语句

- SQL功能类的介绍

SQL功能类下有很多方法，其需要传入对应的一些参数来进行调用，传入其所需要的字符，其会自动将其拼接一个SQL语句，可以直接为我们所用，具体什么请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1b8579637137a0128a52639c14592332.png)

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

- 查询功能的实现

接着我们来实现我们的查询功能，构建查询全部的SQL我们上一节已经学习过了，所以这不是难点。这里难点在于我们怎么将我们生成的sql语句加入到我们的注解中。这时，就得靠我们的SelectProvider注解了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE11a5507121cde5476f1f943d93b5f0cf.png)

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

- 新增功能的实现

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4c42c05d9309fdaf3a35d93abd32d2a5.png)

- 修改功能的实现

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

- 删除功能的实现

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE85789167262d48edd4bc7623e1052b23.png)

