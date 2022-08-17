# MyBatisPlus

学着SB，但是有知识需要补充，因此我们这里来重新补充MyBatisPlus的知识，同样我们还是先做一个入门案例

- MP入门案例

先来看看我们的MyBatisPlus（简称MP）的其作用

**注意，MP是基于MB框架上则增强工具，其主要作用就是简化开发，提高效率，可以让我们轻松实现CURD操作**

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb92a4147a3f26f6d72b7a840868829a4.png)

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

## MP优点

最后我们再来看看MP的优点

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe47824ab53bca09de2474d423556686e.png)

## 标准CRUD制作

接着我们来正式制作的我们的MP的连接数据库的增删改查的案例，我们先来看看MP接口中和自定义接口中方法的不同

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8407d7bb1e40037935bd250f49c45b12.png)

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

## 标准分页功能制作

接着我们来学习分页功能的使用，说实话吧，制作个几把，基本需要使用什么都实现帮我们做好了，我属实不知道我还要做什么。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd6f245df579953ae55d4be7bb2bb0c2a.png)

然后我们就来实现我们的分页查询，分页查询其也提供给了我们对应的方法，这个方法就是selectPage，里面需要传入一个page对象，而Page对象需要我们自己去New出来，其下有current和size属性，前者表示我们要查询的页数，后者表示一页有几条数字。然后我在其下也可以调用其对应的方法来查看我们当前分页的具体的数据分布情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb20babec0ebe6620888ab10a61e48085.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6a41d6930ab4c87528336aca1ce8e319.png)

此时我们再回去运行我们的程序，就会发现其可以工作了，但是我们还想要知道其执行的sql语句，所以我们要开启日志，希望在控制台中打印所有其执行的语句，那么我们就可以在我们的application.yml的配置文件中写入开启日志的代码如下

```
# 开启mp的日志（输出到控制台）
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

然后我们在控制台中就可以正确获取到我们的日志记录了

## 条件查询的三种格式

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE67a63ac6fc0af8df4d0aea91610b0d4b.png)

## 条件查询null判定

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf4013ae2f252dd14f4946d174293a8a3.png)

最后我们提一嘴我们的链式编程，一般来说，我们是不会让我们的代码溢出屏幕的，因为溢出的话不方便我们看，一般我们使用链式编程，都是在下一个链前用空格分开的，这样我们看起来比较顺眼

## 查询投影

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEadc045876f8dca8f7a40d00670b0481a.png)

可以看到我们这里是List集合下存放着Map集合，我们的count是Map集合的String，其右边存放的是数据总量，而我们的tel则是我们的第二个展示的值，其右边存放的是tel的数据值本身

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0c028466b14db9dc584448b3ac8cbabe.png)

最后一点我们要记住，我们的简单的需求可以通过我们的MP提供的方法解决，但如果是比较复杂的需求，那还是得返璞归真，正经用Select注解构建一个sql语句然后查询吧

## 查询条件设置

接着我们来学习我们的设置我们的查询条件，这里能设置的条件是非常多的，我们这里就不一一演示了，我们只演示一部分

首先我最常用的查询就是等于查询，等于查询的方法是eq()，这个我们注解看示例就可以了，这里就不再提了

然后我们来看看我们的范围查询，我们的范围查询使用的lt le gt ge eq，这些条件查询的结合使用我们之前已经讲过了。我们这里重点要提的between，我们的between是且的查询，且前面是最小值后面是最大值，这点要搞清楚，搞错了就寄了，到时候肯定给你查个空出来

```
        //条件查询
//        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
        //等同于=
//        lqw.eq(User::getName,"Jerry").eq(User::getPassword,"jerry");
//        User loginUser = userDao.selectOne(lqw);
//        System.out.println(loginUser);

//        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
        //范围查询 lt le gt ge eq between
//        lqw.between(User::getAge,10,30);
//        List<User> userList = userDao.selectList(lqw);
//        System.out.println(userList);

        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
        //模糊匹配 like
        lqw.like(User::getName,"J");
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
```

然后是我们的模糊查询，我们的模糊查询使用的是like，我们直接使用like就是查找对应属性中含有我们指定的值的数据，还有对应的likeLeft和likeRight方法，其实他们的所代表的意思也就是左边加%还是右边加%的分别，也就是查找我们指定的值为结尾的数据还是查找我们指定的值为开头的数据罢了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf5db4b3503924a754f71e6f633be4d23.png)

实际上还有许许多多的查询方法我们这里是没有演示到的，不过我们什么时候做业务有需求了我们再去查询就行了，去MP的官网上查看就行

## 映射匹配兼容性

接着我们来要讲解注解兼容性的问题，因为我们的数据库和后端的数据连接的时候实际上是会产生各种各样的问题的，这些时候都会出现兼容性问题，而我们需要去解决这些问题，这些问题都有

1. 表字段与编码属性不同步，比如我们的数据库中的字段名为pwd，而在我们的java项目中对应属性的名字是password，而这种问题我们可以指定TableFile注解中的value属性来解决

1. 编码中添加了数据库中未定义的属性，比如我们的编码中可能存在用于表示用户状态的属性，而数据库中压根没有这玩意，此时查询会报错。解决该问题的方法是指定TableField的exist属性，指定其为false设置该属性在数据库的表字段中不存在。注意，此属性无法和value属性合并使用

1. 采用默认查询开放了更多的字段查看权限，比如我们的查询每次都是查询全部，但是这样的话密码也会查询出来，查询出来还没有设置不允许查看，那就不安全。我们可以使用TableField注解，指定其中的select属性为false，意为该属性不参与查询，这样的密码就不会暴露了。

1. 表名与编码开发设计不同步，我们查询的时候，其是自动将我们构造的类的表明当做要查询的表的表名的，那么此会出现找不到对应表的情况（如果表明与类名不一致的话），我们可以在对应类中使用TableName注解，括号内写入我们的数据表中的表名就可以了

那么最后我们可以将我们的实体类的代码改造如下

```
package com.itheima.domain;

import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.*;

//lombok

@Data
@TableName("tbl_user")
public class User {
    private Long id;
    private String name;
    @TableField(value = "pwd",select = false)
    private String password;
    private Integer age;
    private String tel;

    @TableField(exist = false)
    private Integer online;
}

```

最后我们经过测试会发现我们的程序没有问题，此时就说明我们的项目成功了

## id生成策略

接着我们来讲我们的id生成策略，我们的id生成策略是有很多种的，最简单的一种就是自增，还有一些具有特殊规则的则是订单，而类似于快递单其生成的单号，不同的数字是表示着不同的地区位置的，那么我们的id的生成策略就需要控制，那么我们在java程序中如何完成这件事呢？

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd02884360ce976f2f74e484eec75aa4f.png)

这就要靠我们的TableId注解了，该注解可以设置当前类中的主键属性的生成策略，我们来看看其下有几种设置策略

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9c264c80f8aec6376629afb031fdd7eb.png)

其下的策略有五种，分别是AUTO（自增）、NONE（不设置）、INPUT（自己输入）、ASSIGN_ID（雪花算法生成ID）、ASSIGN_UUID（以UUID算法生成id）。什么是雪花算法？简单来说雪花算法就是一个六十四位的二进制数，利用其不同的二进制数的表示法令其能够正确产生不可能重复的id号

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc407a339276f57a6f6c105dc42068115.png)

接着还有一个问题是，如果我们给我们的每一个需要添加时间戳的实体类都加上这个注解，似乎有一些太麻烦了，因此我们有了全局配置的方式，可以让我们的需要查找的对应数据的实体类都解决这个问题，全局配置需要到我们的配置文件application.yml中去设置，其全局配置是mybatis-plus下的global-config下的db-config下的id-type，我们将其设置我们想要的对应的id生成策略，同样的，我们上一节学习过的解决数据表名和类名冲突的注解TableName也是可以进行全局设置的，其与id-type平级，名为table_prefix，注意我们这里设置的是前缀，我们之前的注解是直接设置全名的，我们这里是设置前缀，其最终的要寻找的表明和前缀+类名组成的，也就是说，如果我们的前缀结合而成的方式没有办法在数据表中有对应的表名，那么照样报错

另外这种全局的方式我觉得属实不太实用，如果可以指定一部分的包的内容执行这个的话应该就很不错

接着我们来将下在MP中，我们如何一次删除多条记录，这就需要用到我们的经典方法，deleteBatchIds()了，使用这个方法，我们往里面传入一个List集合，集合内部写入对应的ID，就可以实现多个删除了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5a51fb1db59adf911548bd9db819cf5b.png)

同样我们也可以实现多个查询，其方法是selectBatchIds()，同样是传入集合对象就可以查询了，返回的也是存放对应实体类的集合

## 逻辑删除

我们实际的业务开发中经常对我们的数据进行的是逻辑删除而不是物理删除，如果要实现逻辑删除，我们需要做两件事，一是添加规定逻辑删除的字段，二是指定逻辑删除的逻辑。假设我们规定0为未删除，1为删除，那么首先我们要在数据表中加入对应的字段，然后全设置为0，意味着我们的数据一开始都没有删除，然后我们在对应的实体类中创建对应的逻辑删除的属性，然后在其上加入TableLogic字段，接着指定其逻辑判断的规则，其中value代表为删除时的判断字符串，而delval则代表删除的判断字符串，我们这里给前者赋值为0，后者赋值为1（注意，需要是字符串的形式）

```
//逻辑删除字段，标记当前记录是否被删除
@TableLogic(value = "0",delval = "1")
private Integer deleted;
```

一旦我们配置了这个属性，那么当我们执行对应的删除时，其就不会执行删除，而是更新，其会将对应的数据的指定字段的值改变为delval的值，同时，当我们执行查询操作时，其也只会对那些逻辑字段为value的值的数据进行查询，这非常符合我们的平时的需求，如果数据应该被删除了，那么我们就没必要对其进行查询，其为了完成这个功能，会在对应的sql语句上加一个固定的where语句用于判断

如果我们遇上了特殊情况，比如说我们这次的查询需要无视逻辑删除的字段，那么我们就自己构造一个sql语句吧，没法了属于是

最后我们还有对对应的逻辑字段进行全局设置的方式，同样需要到我们的配置文件中去设置，其设置的代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa01565f7793d6b51dcb55754564f70a6.png)

## 乐观锁

接着我们要来讲解乐观锁的内容，在我们的实际业务中，常常会遇到秒杀的情况，此时我们的物品很可能会出现超卖情况，也就是没这么多东西，但是卖多了，这就很尬。为了解决这个问题，我们需要使用乐观锁机制，所谓乐观锁就是我们每次查询时都认为此时不会被其他人进来修改数据，所以不对表的更新做一个判断。（不过说实话我不明白这跟我们的超卖问题有啥关系...）

我们解决超卖问题的方法是给我们的商品添加一个version字段，每次有用户要修改该字段的值时，我们先查询要修改的数据的字段的值，然后修改时将拿到的值与当前的值进行比较，如果相同则修改并且令该值自增，如果不相同则不允许修改。这个逻辑非常简单，不过这玩意只能对我们的访问量比较少的时候能用，如果我们的访问量越过了两千，那么我们就要学习其他的方法了，我们这里先介绍这个方法

首先我们在对应的数据库里添加这个字段，然后我们在对应的实体类中添加这个属性，然后我们加上Version注解，这样其自己就知道我们这个是用于解决超卖问题的属性了

```
@Version
private Integer version;
```

然后我们使用这个方法来达成我们的目的，必然要对我们的sql语句进行一个增强，那这里也是要使用到我们的经典的拦截器了，我们这里配置拦截器的代码是

```
package com.itheima.config;

import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor;
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
        //3.添加乐观锁拦截器
        mpInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return mpInterceptor;
    }

}

```

配置本节所需要的拦截器的代码在第19行，然后我们写入对应的测试代码如下

```
//1.先通过要修改的数据id将当前数据查询出来
User user = userDao.selectById(3L);  //version=3
User user1 = userDao.selectById(3L); //version=3

user.setName("sdf");
userDao.updateById(user);  //version=4


user1.setName("sdf");
userDao.updateById(user1);
```

我们这里模拟的情况就是两个用户同时想要购买一个商品的情况，此时我们的修改只有第一个用户能成功，因为两个用户拿到的version都是3，但是当第一个用户完成了修改之后，version就成为4了，此时是修改语句中的要求version相等的条件是不成立的，此时该修改语句就不会通过

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE53fc2c25990ff3ff51dfed287335bffe.png)

## 代码生成器

接着我们来学习我们的MP的最后一章，我总有一种这一节的内容使用得并不多的感觉，因此这最后一节主要以了解为主，以后真用上了我们再回来查找就行了，直接看图吧

比方说对于我们的构造我们的对应的查询类的代码来说，我们是可以将统一的代码抽取出来作为模板的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7cddcfda48c0e564d616dede5e1d891e.png)

在我们的实体类中也是如此，有一些内容徐亚开发者配置的那就生成的时候不指定内容就行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE234977bf692e7b40a56f3f4ad3e1a3fd.png)

按照这个思想，MP给我们提供了代码生成器，其就是利用模板来生成我们所需要的类的，其模板是其事先提供给我们的热门模板，我们用他的模板来生成类一般就足够满足需求了，当然也有不满足的时候，不满足的话也可以自己构造模板，不过我们一般不会这么做，这太麻烦了

```
<!--代码生成器-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.1</version>
</dependency>

<!--velocity模板引擎-->
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.3</version>
</dependency>
```

我们要使用我们的模板new一个AutoGenerator()对象然后调用其中的execute方法，但是这中间还需要很多东西进行设置的，这里我们进行一个逐一的讲解

首先是我们提前写好的创建对象并设置数据库相关配置的代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeeace5c835490d6a9e5d060178115a93.png)

然后我们要设置全局配置的代码，我们要设置全局配置就需要new一个GlobalConfig()对象出来，调用其下的方法进行设置，其中第一个方法setOutputDir是设置其生成的类的存放位置，我们这里这样设置是令其生成在我们的idea读取文件的路径中，默认路径是D盘

其他的内容则有对应的注释说明了，这里不再赘述，最后我们要将这个全局设置传入到我们的自动生成的类中，这里需要调用其setGlobalConfig()方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE288a7aa892f6908eb7c63ca10758fcaf.png)

然后是设置包名相关配置的代码，这样其生成的类所在的包的包名就是我们所指定的包名了，这里需要new出PackageConfig对象来

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd719415a5a1d4673f5e1829765b7650f.png)

最后一块是策略配置，需要new一个StrategyConfig对象，对应的方法都有注解，就不再提了。但是重点要提一下的是setTablePrefix方法，其可以设置数据库的前缀名称，那么我们的模块名就是数据库表明-前缀名，使用这个方法可以让我们生成的实体类的名称变得更加直观，比如我们的数据库的名字是456_sdf，那么我们只要在其中输入456_，最后生成的实体类就是sdf而不是456_sdf了，这样就很不错

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE742690de3e332557cf973e243ff8406f.png)

不过当我们往右侧看时，我们会发现其还有一个service文件夹，其下有对应的接口和其实现类，该实现类继承了Iservice类且泛型填写我们的实体类

其实这个继承动作就跟我们之前在Dao层所继承的BaseMapper的作用是一样的，其作用就是可以自动生成许多业务层的方法，这些方法可以调用数据层的对应方法而已。我们一般很少用这个，因为我们的业务层一般来说不只是单纯的调用数据层的东西，我们有可能还有其他的功能需要做，此时我们是不可以用其单纯提供给我们的方法的。但是其毕竟都帮我们自动生成了是吧，所以爱用不用吧，不用也行，删掉就完了