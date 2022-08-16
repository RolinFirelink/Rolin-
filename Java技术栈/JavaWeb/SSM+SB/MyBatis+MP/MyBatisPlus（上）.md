学着SB，但是有知识需要补充，因此我们这里来重新补充MyBatisPlus的知识，同样我们还是先做一个入门案例

- MP入门案例

先来看看我们的MyBatisPlus（简称MP）的其作用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb92a4147a3f26f6d72b7a840868829a4.png)

我们首先创建一个SB案例，同时我们往里面只添加MySQL的驱动坐标，因为MP坐标并没有被整合到SB框架中，因此我们不勾选，而是自己添加。那么我们就要添加我们的依赖如下

```
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>
```

我们这里添加的依赖有两个，一个是我们的MP的依赖，另外一个是我们的连接池的依赖

然后我们创建对应的实体类然后提供给其对应的构造方法和setandget方法，接着在配置文件上我们配置对应的连接信息，然后我们创建对应的Dao层接口及其实现类，如果是之前，我们肯定是要使用Mapper注解然后一个个构造我们的查询代码的，但是我们有了MP之后，我们就不需要搞这些有的没的了

我们这里同样要使用Mapper注解，但是我们不需要构造这么多的具体方法，我们只需要令其继承BaseMapper类，类中的多态我们选择使用我们自定义的数据封装类型就够了，我们不往里面写入任何其他的代码

```
package com.itheima.dao;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.itheima.domain.User;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserDao extends BaseMapper<User> {

}

```

然后我们在测试类中获得该对象，我们会发现里面已经自动生成了许多的方法，我们可以对其进行对应的调用，并且我们真的去调用之后会发现的确可以获得对应的数据，而我们这时，是没有写入任何的SQL代码的，从中就可以一窥MP的强大便利性

最后我们再来看看MP的优点

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe47824ab53bca09de2474d423556686e.png)

- 标准CRUD制作

接着我们来正式制作的我们的MP的连接数据库的增删改查的案例，我们先来看看MP接口中和自定义接口中方法的不同

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd6f245df579953ae55d4be7bb2bb0c2a.png)

其实实际上也不需要我们做什么，因为基本大部分我们所需要的方法其都帮我们做好了，我们直接使用就可以了，虽然我们还不知道里面有什么方法，但是只要输入CRUD的对应关键词其就会自动提示了，我们只要找到我们有印象的然后选择该方法就可以了

这里值得一提的是，我们进行值的更新时，如果我们有一些属性值没有传入任何变量，那么其就不会对其做对应的修改，而不会跟以前一样如果不传入值直接给赋值为空，这就方便不少了

最后我们引入一个新的jar包，那就是lombok，我们引入其依赖如下

```
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

然后我们使用这个注解可以动态生成我们需要的对应方法，那么我们的实体类的代码就可以修改如下

```
package com.itheima.domain;

import lombok.*;

//lombok
@Setter
@Getter
@ToString
@AllArgsConstructor
@NoArgsConstructor
@EqualsAndHashCode
//@Data 一个包含上面的全部，除了构造方法的注解
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
}
```

像我们上面，只要使用对应的注解就可以令其动态生成对应的方法，很是方便，同时最多使用的是Data注解，使用其一个基本可以获得我们所需要的方法，最常使用的就是这个注解

- 标准分页功能制作

接着我们来学习分页功能的使用，说实话吧，制作个几把，基本需要使用什么都实现帮我们做好了，我属实不知道我还要做什么。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8407d7bb1e40037935bd250f49c45b12.png)

然后我们就来实现我们的分页查询，分页查询其也提供给了我们对应的方法，这个方法就是selectPage，里面需要传入一个page对象，而Page对象需要我们自己去New出来，其下有current和size属性，前者表示我们要查询的页数，后者表示一页有几条数字。然后我在其下也可以调用其对应的方法来查看我们当前分页的具体的数据分布情况

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE67a63ac6fc0af8df4d0aea91610b0d4b.png)

但是如果我们只配置这个，那么我们的分页功能是不能正确执行的，这是因为在MP里，其执行分页的方法是先执行查询所有，之后获得所有的数据之后在进行对应的分页操作，这里就需要将数据拦截下来并对其功能进行一个增强，这个思想非常类似于AOP，但不是，这里我们称拦截并增强的行为是拦截器的工作，所以我们就需要配置一个拦截器。那么我们可以写入其拦截器的代码如下

```
package com.itheima.config;

import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor mpInterceptor (){
        //1.定义Mp拦截器
        MybatisPlusInterceptor mpInterceptor = new MybatisPlusInterceptor();
        //2.添加具体的拦截器
        mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mpInterceptor;
    }
}
```

我们这里自己创建了一个拦截器类，然后令其返回拦截器对象，我们在里面创建了一个存放所有拦截器的MybatisPlusInterceptor对象，然后往其下添加了一个具体的分页拦截器，接着返回这个拦截器，然后我们需要将其加入到我们的Spring容器中，我们可以往我们的最初的核心注解配置类，也就是我们的引导类，这里的名字是Mybatisplus01QuickstartApplication，往该类中加入注解Import引入我们的拦截器类，但是这个方法太low，我们直接往拦截器类上添加Configuration注解，令我们的拦截器类成为一个配置类，然后我们的引导类里的扫描配置的注解会将该注解扫描到并自动添加到Spring容器中，这样我们的拦截器类就可以正常工作了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb20babec0ebe6620888ab10a61e48085.png)

此时我们再回去运行我们的程序，就会发现其可以工作了，但是我们还想要知道其执行的sql语句，所以我们要开启日志，希望在控制台中打印所有其执行的语句，那么我们就可以在我们的application.yml的配置文件中写入开启日志的代码如下

```
# 开启mp的日志（输出到控制台）
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

然后我们在控制台中就可以正确获取到我们的日志记录了

- 条件查询的三种格式

接着我们来学习MP中的条件查询，条件查询的方式要使用的对象是QueryWrapper，我们进行条件查询就传入该对象到对应的方法中，而条件查询的具体条件则是通过这个对象中的方法并传入对应的参数设置出来的，比如我们希望我们查询的数据中某个数据小于某个值，那么就调用其下的lt()方法，反之则是gt（）方法（值得一提的是无论是lt还是gt都应事先调用lambda()方法），而往方法中传入参数的格式有三种

第一种格式是直接往其下传入对应的属性名和要设置的属性值，这种方式存在的问题是可能名字输错了，到时候就寄。第二种方式是通过User::getAge的方式来获得其属性名然后传入其属性值，这种方式虽然麻烦了些，但是就不用担心名字设置错了

第三种方式是通过LambdaQueryWrapper对象实现条件查询，这里和方式一二唯一的不同是不用调用lambda()方法了，直接调用对应的lt或者gt方法就行了

```
    @Test
    void testGetAll() {
//        //方式一：按条件查询
//        QueryWrapper qw = new QueryWrapper();
//        qw.lt("age",18);
//        List<User> userList = userDao.selectList(qw);
//        System.out.println(userList);

//        //方式二:lambda格式按条件查询
//        QueryWrapper<User> qw = new QueryWrapper<User>();
//        qw.lambda().lt(User::getAge,10);
//        List<User> userList = userDao.selectList(qw);
//        System.out.println(userList);

//        //方式三:lambda格式按条件查询
//        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
//        lqw.lt(User::getAge,10);
//        List<User> userList = userDao.selectList(lqw);
//        System.out.println(userList);

        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
//        lqw.lt(User::getAge,30);
//        lqw.gt(User::getAge,10);
//        lqw.lt(User::getAge,30).gt(User::getAge,10);
        lqw.lt(User::getAge,30).or().gt(User::getAge,10);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

最后我们提一下如果我们想要复合条件查询我们应该怎么做，如果我们的条件是且，那么直接继续设置就行了，可以分行设置两个条件，也可以通过链式法则直接在前一个条件下继续设置

如果是或，那么则需要在链式条件设置里额外调用or()方法，如果是分行的话则是独立一行调用or()方法(这一点是我个人猜测的，不保证对)

最后，一般而言，我们最推荐格式四来实现条件查询

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6a41d6930ab4c87528336aca1ce8e319.png)

- 条件查询null判定

接着我们还有一个问题要解决，就是我们的条件可能存在空值，比如说如果用户想要20元以上的商品，那么此时其不会设置最大值，那么此时我们就不应该设置我们的最大值上限，此时我们也不能够拿空值去判断，因此我们要在我们的过滤中做我们的数据是否为空的判断

为此我们首先要判断我们的那些值是有最大值和最小值的，因此我们可以新创建一个类，该类继承原来的类并且有一个同样类型的变量，但其是用于承载最大值的，比如我们假设年龄有最大值，那么我们可以创建一个类令其继承原来的类并写入其代码如下

```
package com.itheima.domain.query;

import com.itheima.domain.User;
import lombok.Data;

@Data
public class UserQuery extends User {
    private Integer age2;
}

```

接着我们创建这个新的实体类然后用这个实体类进行判断，每次我们要判断其对应的值是否为空，我们的一个简单的想法就是加if判断条件，但这样的话整多了我们的代码就会变得难看非常，我们有一个别的方式，就是原来的lt方法中，是有重载方法的，我们可以直接先传入一个判断条件，只有当条件为真时才执行这个语句，这样我们的代码就会变得通畅多了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf4013ae2f252dd14f4946d174293a8a3.png)

最后我们提一嘴我们的链式编程，一般来说，我们是不会让我们的代码溢出屏幕的，因为溢出的话不方便我们看，一般我们使用链式编程，都是在下一个链前用空格分开的，这样我们看起来比较顺眼

- 查询投影

本章的最后一节我们来学习查询投影的部分，所谓的查询投影其实也就是我们去设置我们的数据如何展示的部分

如果我们想要让我们的数据展示一部分内容，那么用不同的对象有不同的设置方法，如果我们是使用LambdaQueryWrapper对象，那么设置展示的对象是就是要调用select()方法，且往里面传入我们的数据的属性名时要求使用User::getId的方法来获取

而如果我们用QueryWrapper对象，则也是调用select()方法，但是这里传入的属性名则是一定要求使用字符串格式才可以，否则就进行一个错误的报出

```
//        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
//        lqw.select(User::getId,User::getName,User::getAge);

//        QueryWrapper<User> lqw = new QueryWrapper<>();
//        lqw.select("id","name","age","tel");
//        List<User> userList = userDao.selectList(lqw);
//        System.out.println(userList);

        QueryWrapper<User> lqw = new QueryWrapper<>();
        lqw.select("count(*) as count, tel");
        lqw.groupBy("tel");
        List<Map<String, Object>> userList = userDao.selectMaps(lqw);
        System.out.println(userList);
```

最后我们还可以进行一些更加复杂查询，这需要用到我们的动态生成的对象里的selectMaps()方法，里面需要传入我们的QueryWrapper对象，我们调用需要调用其下的select方法，往里面传入我们要执行查询的sql语句，比如我们传入count(*)，其就意为着我们要查询我们的数据总数，然而这个数据需要Map集合来进行封装，我们的Map集合里往往是左边是String变量，代表我们要查询出来的结果的名字，我们题目里将其改名为count，所以其String中显示的名字就是count，然后右边则存放我们的结果，比如我们上面的右边的结果存放的是数字，是我们的数据总量的值

同时我们这里还可以进行分组，我们这里对我们的组通过电话号码进行了一个组别的分，传入电话号码的数据的值，然后其就会按照电话号码的不同给我们的值进行分组，然后再执行对应的统计，最后我们得到的结果如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0c028466b14db9dc584448b3ac8cbabe.png)

可以看到我们这里是List集合下存放着Map集合，我们的count是Map集合的String，其右边存放的是数据总量，而我们的tel则是我们的第二个展示的值，其右边存放的是tel的数据值本身

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEadc045876f8dca8f7a40d00670b0481a.png)

最后一点我们要记住，我们的简单的需求可以通过我们的MP提供的方法解决，但如果是比较复杂的需求，那还是得返璞归真，正经用Select注解构建一个sql语句然后查询吧