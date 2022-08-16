接下来我们来学习自己做一个JDBC的框架，这一节的内容并不要求我们能够独立写出来，我们主要是去感受这个思路，感受这个过程，理解到位就可以了。

- 框架背景介绍

首先我们分析我们之前实现过的JDBC案例，也就是Dao层里实现增删改查的代码，其实就是StudentDaoImpl类，可以在里面发现很多很多的重复代码，比如说获取数据库的连接，又或者是释放资源，这些都是相同的代码，而我们的核心功能其实只是执行一条sql语句而已，所以我们可以抽取出两个JDBC的模板类，将一些方法封装到里面，令其专门帮我们执行增删改查的操作。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa2425c0922b7aa08effb8a4a1b2ab0d6.png)

构造这样的模板类的核心思想就是将之前那些重复操作都抽取到模板类的方法里，以此来简化我们的代码，这其实感觉就像是在搞工具类，但是又有所不同，具体不同请看后文的具体实现

- 数据库的源信息

在我们正式学习JDBC框架的构造之前，我们要先学习一些必要的知识，其实就是各种源信息，我们以后用得上，所以要先学习。

首先是DataBaseMetaData，该对象封装了整个数据库的源信息，我们可以调用这个对象的一些方法来获取数据库的名称和版本号，不过这个知识只做了解，因为我们用不太上

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa16e5ab2dd94eafb1e5720b4215ba926.png)

接着是ParameterMetaData对象，这个对象可以通过预编译执行者对象PreparedStatement获得，该对象保存了参数的源信息，其封装了预编译执行者里对个参数的类型和属性，我们可以通过其中的getParameterCount()方法，获得sql语句中参数的个数。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb095342e7a45c7768c27831ceaf1e5b0.png)

最后是ResultSetMetaData对象，该对象封装了结果集对象中列的类型和属性，其是ResultSet对象调用getMetaData方法获得的，我们可以通过其下的getColumnCount()获取列的总数，还可以通过getColumnName(int i)，获得指定列的列名

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE668ae94992eddfe243460ca60d6519c7.png)

- UPDATE方法的实现

接下来我们正式来编写我们的JDBC框架，请看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb823685bed4c7db929df8076f4954888.png)

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

- UPDATE方法的测试

那么接着我们就来进行update方法的测试，请看我们的测试步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb268669cb7f4504885442ecd0205c72e.png)

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

- 查询功能的前期准备

接着我们来实现我们的查询功能，由于查询功能是比较复杂的，我们可以先将查询分为多种情况，然后分情况去实现这个功能，我们这里就将查询情况分为三种。分别是查询一条记录并封装对象，查询多条记录并封装集合以及查询聚合函数并返回数据的这三种情况并提供对应的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6ba982fc2c77a1a1da2efbb2aa00e060.png)

那么既然我们要返回一个封装对象，那么我们当然要先把这个对象给创建出来，所以我们先进行这个实体类的编写

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE22e3491239f7716209d5fb2a05b8716d.png)

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

接着我们要定义一个处理结果集的泛型接口ResultSetHandler<T>并提供泛型方法，我们定义这个接口只是为了给不同结果集的处理提供一个规范，具体的实现类以及内部的实现逻辑，这还需要我们自己去编写

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfbfdf6b7eeb0be79d53fac31b1660625.png)

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

- BeanHandeler实现类

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

- queryForObject的实现和测试

那么接着我们正式实现queryForObject方法，请看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3d69628515b3efe195b0a01127cfb509.png)

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

- BeanListHandler实现类

接着我们来实现第二种情况的代码，当然，我们也是先实现底层类的代码，先来看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf7c558fda3a3c5e5f9b864f5d6e78d5c.png)

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

- queryForList的实现和测试

那么编写完底层代码之后，现在我们正式来写其实现和测试，请看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEab9b7f36e97cb901f753df5e94678357.png)

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

- ScalarHandler实现类

那么现在我们来实现我们的最后一个需求，就是查询聚合函数，先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4fffc08ce622e4b0f8e81097400c65b5.png)

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

- queryForScalar的实现和测试

那么我们来正式去实现第三个功能，同样先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE391c32aec4fe1f10470c45856ec85c24.png)

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