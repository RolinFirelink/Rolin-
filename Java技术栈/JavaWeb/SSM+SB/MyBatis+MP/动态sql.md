之前我们的学习了使用sql语句来进行相应的查询、添加、增加、删除等操作，但是那些操作都是比较简单的，而且他们都写死在了我们的映射配置文件中，如果我们以后有新的需求的话，那么我们又需要再增加一个查询语句，这太麻烦了。我们期待有这么一种方式，可以让我们的查询变成动态查询，我们希望我们的查询语句可以只用一条就实现我们的多种查询，而这也是我们本章节要学习的内容。

- 动态sql的介绍

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE422794f861f8350979054dd61e14cb45.png)

- if标签的使用

那么接着我们就来使用对应的标签来实现动态sql，首先是if条件判断标签，但是在哪之前，我们要先学习where标签

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf69159d95013fa97f8a35a8bba8e8621.png)

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

- foreach标签的使用

foreach标签主要用于遍历标签，适合用于多个参数直接的或者关系。简单来说就是假如我们想要用多个id查询用户，符合里面任何一个的都显示出来，此时就需要用到这个标签

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3ca57adc3eba990e1de6c2802b6e4f1f.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3d07df3021c795e2ea32a26dd7adf9cd.png)

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

- sql片段的抽取

我们容易观察到，我们的sql语句里，有很多重复代码，比如说SELECT * FROM student，这几乎每一个命令都需要重复写入一次，属实是太麻烦了。那么这时我们就可以用sql片段的抽取，将重复代码抽取到一起，然后用一个标签来代替他们使用，这样便于我们日后的维护。

sql片段抽取就可以满足我们的需求，抽取需要使用sql标签，其中有id属性，属性中要引入片段唯一标识，然后又include标签，include标签是可以使用抽取的sql的片段，其下有refid属性，属性下填入对应的片段唯一标识可以达到引入抽取的sql语句的效果

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7d012bb3ab1fa0237a642f2fdc343255.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0b0e90973d9991e7493fee42275f3fe2.png)

