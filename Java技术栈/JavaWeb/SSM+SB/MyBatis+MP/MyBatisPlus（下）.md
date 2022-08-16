- 查询条件设置

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9c264c80f8aec6376629afb031fdd7eb.png)

实际上还有许许多多的查询方法我们这里是没有演示到的，不过我们什么时候做业务有需求了我们再去查询就行了，去MP的官网上查看就行

- 映射匹配兼容性

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

- id生成策略

接着我们来讲我们的id生成策略，我们的id生成策略是有很多种的，最简单的一种就是自增，还有一些具有特殊规则的则是订单，而类似于快递单其生成的单号，不同的数字是表示着不同的地区位置的，那么我们的id的生成策略就需要控制，那么我们在java程序中如何完成这件事呢？

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE742690de3e332557cf973e243ff8406f.png)

这就要靠我们的TableId注解了，该注解可以设置当前类中的主键属性的生成策略，我们来看看其下有几种设置策略

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd02884360ce976f2f74e484eec75aa4f.png)

其下的策略有五种，分别是AUTO（自增）、NONE（不设置）、INPUT（自己输入）、ASSIGN_ID（雪花算法生成ID）、ASSIGN_UUID（以UUID算法生成id）。什么是雪花算法？简单来说雪花算法就是一个六十四位的二进制数，利用其不同的二进制数的表示法令其能够正确产生不可能重复的id号

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf5db4b3503924a754f71e6f633be4d23.png)

接着还有一个问题是，如果我们给我们的每一个需要添加时间戳的实体类都加上这个注解，似乎有一些太麻烦了，因此我们有了全局配置的方式，可以让我们的需要查找的对应数据的实体类都解决这个问题，全局配置需要到我们的配置文件application.yml中去设置，其全局配置是mybatis-plus下的global-config下的db-config下的id-type，我们将其设置我们想要的对应的id生成策略，同样的，我们上一节学习过的解决数据表名和类名冲突的注解TableName也是可以进行全局设置的，其与id-type平级，名为table_prefix，注意我们这里设置的是前缀，我们之前的注解是直接设置全名的，我们这里是设置前缀，其最终的要寻找的表明和前缀+类名组成的，也就是说，如果我们的前缀结合而成的方式没有办法在数据表中有对应的表名，那么照样报错

另外这种全局的方式我觉得属实不太实用，如果可以指定一部分的包的内容执行这个的话应该就很不错

接着我们来将下在MP中，我们如何一次删除多条记录，这就需要用到我们的经典方法，deleteBatchIds()了，使用这个方法，我们往里面传入一个List集合，集合内部写入对应的ID，就可以实现多个删除了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc407a339276f57a6f6c105dc42068115.png)

同样我们也可以实现多个查询，其方法是selectBatchIds()，同样是传入集合对象就可以查询了，返回的也是存放对应实体类的集合

- 逻辑删除

我们实际的业务开发中经常对我们的数据进行的是逻辑删除而不是物理删除，如果要实现逻辑删除，我们需要做两件事，一是添加规定逻辑删除的字段，二是指定逻辑删除的逻辑。假设我们规定0为未删除，1为删除，那么首先我们要在数据表中加入对应的字段，然后全设置为0，意味着我们的数据一开始都没有删除，然后我们在对应的实体类中创建对应的逻辑删除的属性，然后在其上加入TableLogic字段，接着指定其逻辑判断的规则，其中value代表为删除时的判断字符串，而delval则代表删除的判断字符串，我们这里给前者赋值为0，后者赋值为1（注意，需要是字符串的形式）

```
//逻辑删除字段，标记当前记录是否被删除
@TableLogic(value = "0",delval = "1")
private Integer deleted;
```

一旦我们配置了这个属性，那么当我们执行对应的删除时，其就不会执行删除，而是更新，其会将对应的数据的指定字段的值改变为delval的值，同时，当我们执行查询操作时，其也只会对那些逻辑字段为value的值的数据进行查询，这非常符合我们的平时的需求，如果数据应该被删除了，那么我们就没必要对其进行查询，其为了完成这个功能，会在对应的sql语句上加一个固定的where语句用于判断

如果我们遇上了特殊情况，比如说我们这次的查询需要无视逻辑删除的字段，那么我们就自己构造一个sql语句吧，没法了属于是

最后我们还有对对应的逻辑字段进行全局设置的方式，同样需要到我们的配置文件中去设置，其设置的代码如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE53fc2c25990ff3ff51dfed287335bffe.png)

- 乐观锁

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5a51fb1db59adf911548bd9db819cf5b.png)

- 代码生成器

接着我们来学习我们的MP的最后一章，我总有一种这一节的内容使用得并不多的感觉，因此这最后一节主要以了解为主，以后真用上了我们再回来查找就行了，直接看图吧

比方说对于我们的构造我们的对应的查询类的代码来说，我们是可以将统一的代码抽取出来作为模板的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa01565f7793d6b51dcb55754564f70a6.png)

在我们的实体类中也是如此，有一些内容徐亚开发者配置的那就生成的时候不指定内容就行了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7cddcfda48c0e564d616dede5e1d891e.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE234977bf692e7b40a56f3f4ad3e1a3fd.png)

然后我们要设置全局配置的代码，我们要设置全局配置就需要new一个GlobalConfig()对象出来，调用其下的方法进行设置，其中第一个方法setOutputDir是设置其生成的类的存放位置，我们这里这样设置是令其生成在我们的idea读取文件的路径中，默认路径是D盘

其他的内容则有对应的注释说明了，这里不再赘述，最后我们要将这个全局设置传入到我们的自动生成的类中，这里需要调用其setGlobalConfig()方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEeeace5c835490d6a9e5d060178115a93.png)

然后是设置包名相关配置的代码，这样其生成的类所在的包的包名就是我们所指定的包名了，这里需要new出PackageConfig对象来

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE288a7aa892f6908eb7c63ca10758fcaf.png)

最后一块是策略配置，需要new一个StrategyConfig对象，对应的方法都有注解，就不再提了。但是重点要提一下的是setTablePrefix方法，其可以设置数据库的前缀名称，那么我们的模块名就是数据库表明-前缀名，使用这个方法可以让我们生成的实体类的名称变得更加直观，比如我们的数据库的名字是456_sdf，那么我们只要在其中输入456_，最后生成的实体类就是sdf而不是456_sdf了，这样就很不错

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd719415a5a1d4673f5e1829765b7650f.png)

不过当我们往右侧看时，我们会发现其还有一个service文件夹，其下有对应的接口和其实现类，该实现类继承了Iservice类且泛型填写我们的实体类

其实这个继承动作就跟我们之前在Dao层所继承的BaseMapper的作用是一样的，其作用就是可以自动生成许多业务层的方法，这些方法可以调用数据层的对应方法而已。我们一般很少用这个，因为我们的业务层一般来说不只是单纯的调用数据层的东西，我们有可能还有其他的功能需要做，此时我们是不可以用其单纯提供给我们的方法的。但是其毕竟都帮我们自动生成了是吧，所以爱用不用吧，不用也行，删掉就完了