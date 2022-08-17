数据库基本概念

经过了图书管理系统的练习之后，现在我们来正式学习数据库，学习数据库之前，我们要先了解数据库的基本概念

首先数据库，顾名思义，就是存储数据的库，我们学习数据库是为了让我们更加方便地对开发中的数据进行管理，我们先来看看我们没学习数据库之前修改文件的方式的麻烦程度

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc47a408eae47bbad6c6f380833486521.png)

这里我们利用IO流进行插入，足足有6个步骤，是真的很麻烦，但如果我们使用了数据库，那么我们只需要一个语句就能完成

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEea9a1bead831f1fddb2a22d03b72f140.png)

上图的语句就是我们的使用的修改的语句而更加上面的内容则是数据库里的数据的保存方式，上面的语句内容也很好理解，前面是我们要更新的内容，UPDATE代表更新，USER是我们需要更新的类，SET后面则要跟上我们要修改的属性名和具体到的属性，后面的则是我们用于定位到准确数据的条件

而像我们的一条语句，我们的精确术语就叫做SQL语句

## 数据库介绍

现在让我们来正式介绍下数据库的作用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9b789189d054d554a46d540de1cc5822.png)

市面上的数据库很多，我们这里要学习的数据库就是MySQL数据库,之所以学习它，是因为MySQL是最流行的关系型数据库管理系统之一。所谓关系型数据库，是指将数据存放在不同的数据表中，表与表之间可以有关联关系，这样就提高了访问速度及其灵活性。而且MySQL所使用的SQL语句是用于访问数据库的最常用的标准化语言。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE58450ea2cbf11bb8c2a875761362cbd2.png)

# DDL

- **数据库、数据表、数据之间的关系**

简单来说，我们的电脑管理数据库管理系统，该管理系统下有多个数据库，每个数据库有多个数据表，每个数据表下又有多个数据。客户端通过数据库管理系统来操作MySQL数据库

## SQL介绍

所谓SQL其实就是结构化查询语言，其是定义了操作所有关系型数据库的一种规则。其通用的语法规则请看图。其下有四个重要分类，分别是**DDL（数据定义语言）、DML（数据操作语言）、DQL（数据查询语言）和DCL（数据控制语言）**。其中DQL最为重要，DCL最不重要。**本节我们学习DDL，也就是数据定义语言。主要用于操作数据库，表，列等**

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817134422.png)

## 数据库的创建与查询

现在我们就来学习一些DDL的基本语法，首先是查询和创建

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817134635.png)

我们打开sqlyog，可以看到左侧有下图所示的情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817134709.png)

这四个都是数据库DB，其下有数据表，数据表下又有数据。这四个数据库是其Mysql安装的时候帮我们自动创建的，我们不要去操作这些东西，也不要去删除他们。

关于各种指令的代码示例，请看代码

```
    /*
   查询所有数据库
   标准语法：
      SHOW DATABASES;
*/
-- 查询所有数据库
        SHOW DATABASES;


/*
   查询某个数据库的创建语句
   标准语法：
      SHOW CREATE DATABASE 数据库名称;
*/
        -- 查询mysql数据库的创建语句
        SHOW CREATE DATABASE mysql;



/*
   创建数据库
   标准语法：
      CREATE DATABASE 数据库名称;
*/
        -- 创建db1数据库
        CREATE DATABASE db1;


/*
   创建数据库，判断、如果不存在则创建
   标准语法：
      CREATE DATABASE IF NOT EXISTS 数据库名称;
*/
        -- 创建数据库db2(判断，如果不存在则创建)
        CREATE DATABASE IF NOT EXISTS db2;


/*
   创建数据库、并指定字符集
   标准语法：
      CREATE DATABASE 数据库名称 CHARACTER SET 字符集名称;
*/
        -- 创建数据库db3、并指定字符集utf8
        CREATE DATABASE db3 CHARACTER SET utf8;

        -- 查看db3数据库的字符集
        SHOW CREATE DATABASE db3;


        -- 练习：创建db4数据库、如果不存在则创建，指定字符集为gbk
        CREATE DATABASE IF NOT EXISTS db4 CHARACTER SET gbk;


        -- 查看db4数据库的字符集
        SHOW CREATE DATABASE db4;
```

这里我们值得一提的是，创建了对应的数据库之后要在左侧按f5刷新之后才显示，然后是所有的指定都要用鼠标选中之后手动按左上角的“播放”按钮之后才会执行，执行结果会放到下方

## 数据库的修改删除和使用

请看下图的语法介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817134918.png)

请看代码

```
/*
查询所有数据库
标准语法：
  SHOW DATABASES;
*/
-- 查询所有数据库
        SHOW DATABASES;


/*
   查询某个数据库的创建语句
   标准语法：
      SHOW CREATE DATABASE 数据库名称;
*/
        -- 查询mysql数据库的创建语句
        SHOW CREATE DATABASE mysql;



/*
   创建数据库
   标准语法：
      CREATE DATABASE 数据库名称;
*/
        -- 创建db1数据库
        CREATE DATABASE db1;


/*
   创建数据库，判断、如果不存在则创建
   标准语法：
      CREATE DATABASE IF NOT EXISTS 数据库名称;
*/
        -- 创建数据库db2(判断，如果不存在则创建)
        CREATE DATABASE IF NOT EXISTS db2;


/*
   创建数据库、并指定字符集
   标准语法：
      CREATE DATABASE 数据库名称 CHARACTER SET 字符集名称;
*/
        -- 创建数据库db3、并指定字符集utf8
        CREATE DATABASE db3 CHARACTER SET utf8;

        -- 查看db3数据库的字符集
        SHOW CREATE DATABASE db3;


        -- 练习：创建db4数据库、如果不存在则创建，指定字符集为gbk
        CREATE DATABASE IF NOT EXISTS db4 CHARACTER SET gbk;


        -- 查看db4数据库的字符集
        SHOW CREATE DATABASE db4;
```

其实经过简单的学习我们也能够认识到其实这些都是属于非常简单的语法使用的内容，纯粹是在介绍了属于是，我们也照旧记住就可以了，由于是适记性内容，所以我们这里就不过多赘述了。

## **数据表的查询**

我们先来看看有关于数据表查询的语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817135042.png)



然后我们在正式学习之前，可以用上面的语法为例来具体讲讲到底什么是数据表，其展示的方式又是怎么样的。首先我们来看看SHOW TABLES;这个语句。我们这里已经先进入了mysql数据库了，然后调用这个命令令其展示所有的数据表

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817135117.png)

我们可以看到在我们下方显示的各种数据表，而这些数据表在我们打开数据库之后也能够确实地看到，这说明我们的查询的结果就是我们当前使用的数据库其下所有的数据表，其查询的结果也是正确的。同时我们也可以知道，数据表就是数据表，不是文件夹，所以下面的视图一类的文件夹的内容并不会被展示，因为他们不是数据表（当然，也有可能是因为这四个文件夹里啥都没有的原因）

然后我们再来看看DESC user;的结果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817135154.png)

这里Field表示的是一个字段，所谓字段其实也就是一个列，里面存放的也是相关的数据，更加具体的内容我们留到后面来讲。然后Type表示的是当前这个字段所表示的数据类型。null则是代表是否可以为空，key则代表键的设置，后面以及这些内容我们后面会有更加具体的学习

我们同样也可以打开user这个数据表来看看情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817135216.png)

我们可以看到这个内容，我们非常的熟悉，这里存放的内容你说不是用户相关的内容我是不信的，那么多localhost还有一个百分号，这也正好对应我们的user数据表的名字，用户相关嘛。后面的一些内容则是跟权限相关的内容，我们暂时不需要去理会。

我们还可以查询某一个数据表的各种状态信息，也就是SHOW TABLE STATUS FROM 数据库名 LIKE '数据表名';这个语句

那么最后我们的演示代码如下

```
-- 使用mysql数据库
        USE mysql;

/*
   查询所有数据表
   标准语法：
      SHOW TABLES;
*/
        -- 查询库中所有的表
        SHOW TABLES;

/*
   查询表结构
   标准语法：
      DESC 表名;
*/
        -- 查询user表结构
        DESC USER;

/*
   查询数据表的字符集
   标准语法：
      SHOW TABLE STATUS FROM 数据库名称 LIKE '表名';
*/
        -- 查看mysql数据库中user表字符集
        SHOW TABLE STATUS FROM mysql LIKE 'user';
```

## 数据表的创建

关于创建数据库，重要的是列名，数据类型和约束，其中列名我们稍后介绍，而约束是可以不添加的。至于在mysql里常用的数据类型，我们也在下图中给出了。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817135350.png)

如果我们想了解更多的数据类型的话可以看下文

各数据类型及字节长度一览表：

| 数据类型           | 字节长度 | 范围或用法                                                   |
| ------------------ | -------- | ------------------------------------------------------------ |
| Bit                | 1        | 无符号[0,255]，有符号[-128,127]，天缘博客备注：BIT和BOOL布尔型都占用1字节 |
| TinyInt            | 1        | 整数[0,255]                                                  |
| SmallInt           | 2        | 无符号[0,65535]，有符号[-32768,32767]                        |
| MediumInt          | 3        | 无符号[0,2^24-1]，有符号[-2^23,2^23-1]]                      |
| Int                | 4        | 无符号[0,2^32-1]，有符号[-2^31,2^31-1]                       |
| BigInt             | 8        | 无符号[0,2^64-1]，有符号[-2^63 ,2^63 -1]                     |
| Float(M,D)         | 4        | 单精度浮点数。天缘博客提醒这里的D是精度，如果D<=24则为默认的FLOAT，如果D>24则会自动被转换为DOUBLE型。 |
| Double(M,D)        | 8        | 双精度浮点。                                                 |
| Decimal(M,D)       | M+1或M+2 | 未打包的浮点数，用法类似于FLOAT和DOUBLE，天缘博客提醒您如果在ASP中使用到Decimal数据类型，直接从数据库读出来的Decimal可能需要先转换成Float或Double类型后再进行运算。 |
| Date               | 3        | 以YYYY-MM-DD的格式显示，比如：2009-07-19                     |
| Date Time          | 8        | 以YYYY-MM-DD HH:MM:SS的格式显示，比如：2009-07-19 11：22：30 |
| TimeStamp          | 4        | 以YYYY-MM-DD的格式显示，比如：2009-07-19                     |
| Time               | 3        | 以HH:MM:SS的格式显示。比如：11：22：30                       |
| Year               | 1        | 以YYYY的格式显示。比如：2009                                 |
| Char(M)            | M        | 定长字符串。                                                 |
| VarChar(M)         | M        | 变长字符串，要求M<=255                                       |
| Binary(M)          | M        | 类似Char的二进制存储，特点是插入定长不足补0                  |
| VarBinary(M)       | M        | 类似VarChar的变长二进制存储，特点是定长不补0                 |
| Tiny Text          | Max:255  | 大小写不敏感                                                 |
| Text               | Max:64K  | 大小写不敏感                                                 |
| Medium Text        | Max:16M  | 大小写不敏感                                                 |
| Long Text          | Max:4G   | 大小写不敏感                                                 |
| TinyBlob           | Max:255  | 大小写敏感                                                   |
| Blob               | Max:64K  | 大小写敏感                                                   |
| MediumBlob         | Max:16M  | 大小写敏感                                                   |
| LongBlob           | Max:4G   | 大小写敏感                                                   |
| Enum               | 1或2     | 最大可达65535个不同的枚举值                                  |
| Set                | 可达8    | 最大可达64个不同的值                                         |
| Geometry           |          |                                                              |
| Point              |          |                                                              |
| LineString         |          |                                                              |
| Polygon            |          |                                                              |
| MultiPoint         |          |                                                              |
| MultiLineString    |          |                                                              |
| MultiPolygon       |          |                                                              |
| GeometryCollection |          |                                                              |

我们可以创建数据表的语句如下

```mysql
/*
   创建数据表
   标准语法：
      CREATE TABLE 表名(
         列名 数据类型 约束,
         列名 数据类型 约束,
         ...
         列名 数据类型 约束
      );
*/
-- 创建一个product商品表(商品编号、商品名称、商品价格、商品库存、上架时间)
        CREATE TABLE product(
        id INT,
        NAME VARCHAR(20),
        price DOUBLE,
        stock INT,
        insert_time DATE
        );
```

这里我们可以看到，我们创建数据表是需要输入CREATE TABLE，然后指定我们的商品名字的，然后我们在小括号里写入我们所创建的表内所需要的变量名字以及他们的数据类型，先写名字后面写类型，如果是字符串类型的那么就写VARCHAR，而且后面还需要指定容量，即最长能容纳多少个字符，注意每一个数据之间都要用,隔开，但是最后一个数据不需要。我们创建之后我们可以通过decs product已经来查看我们创建的数据表的内容，其结果如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817135914.png)

我们可以看到，所谓的字段，就是我们当初定义的变量名字，所以其实关于字段我们可以暂时将其简单的理解为变量的名字，后面则是经典的数据类型，是否可以为空等等

我们也可以右键打开表来看看内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817135935.png)

我们可以看到我们打开表的内容里面的列表显示的就很简单，就是我们的变量名和里面存储了什么内容，由于我们只是声明了这个数据表而没有赋任何值，因此其下的值全为null

## 数据表的修改

接下来我们来学习数据表的相关内容的修改以及添加的数据表中的数据，请看下图的语法说明

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817140000.png)

这里我们要先讲下关于单独添加一列的add方法，ALTER TABLE 表明 ADD 列名 数据类型是往我们的表上再添加一个一列，其实也就是多添加一个变量，就相当于我们当初创建的时候多指定一个列一样的，同样的我们也要赋予其列名和对应的数据类型，而modify和change，前者是修改某列的数据类型，而后者是可以同时修改列名和数据类型。

示例代码如下

```
/*
   修改表名
   标准语法：
      ALTER TABLE 旧表名 RENAME TO 新表名;
*/
-- 修改product表名为product2
        ALTER TABLE product RENAME TO product2;



/*
   修改表的字符集
   标准语法：
      ALTER TABLE 表名 CHARACTER SET 字符集名称;
*/
        -- 查看db3数据库中product2数据表字符集
        SHOW TABLE STATUS FROM db3 LIKE 'product2';

        -- 修改product2数据表字符集为gbk
        ALTER TABLE product2 CHARACTER SET gbk;


/*
   给表添加列
   标准语法：
      ALTER TABLE 表名 ADD 列名 数据类型;
*/
        -- 给product2表添加一列color
        ALTER TABLE product2 ADD color VARCHAR(10);


/*
   修改表中列的数据类型
   标准语法：
      ALTER TABLE 表名 MODIFY 列名 数据类型;
*/
        -- 将color数据类型修改为int
        ALTER TABLE product2 MODIFY color INT;

        -- 查看product2表详细信息
        DESC product2;

/*
   修改表中列的名称和数据类型
   标准语法：
      ALTER TABLE 表名 CHANGE 旧列名 新列名 数据类型;
*/
        -- 将color修改为address
        ALTER TABLE product2 CHANGE color address VARCHAR(200);

        -- 查看product2表详细信息


/*
   删除表中的列
   标准语法：
      ALTER TABLE 表名 DROP 列名;
*/
        -- 删除address列
        ALTER TABLE product2 DROP address;
```

其实学习了这么多，我们也可以大概整出个规律了，首先我们的创造新的结构总是与CREATE联系的，而修改则是与ALTER联系，如果要修改表，则应该要加上TABLE，如果要删除，则是DROP，要设置字符集会跟上CHARACTER SET。

## **数据表的删除**

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817140121.png)

和删除DB一样，其同样也存在IF EXISTS的语法，演示代码如下

```
/*
   删除表
   标准语法：
      DROP TABLE 表名;
*/
-- 删除product2表
        DROP TABLE product2;
        DROP TABLE product2;

/*
   删除表，判断、如果存在则删除
   标准语法：
      DROP TABLE IF EXISTS 表名;
*/
        -- 删除product2表，如果存在则删除
        DROP TABLE IF EXISTS product2;
```

# DML

学习完了上一节DDL的内容之后，这一节我们来学习DML的内容。DML主要的作用是对于数据库中的表数据进行增删改。具体请看代码

## 新增表数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa9d9a0ea066fadfe048df84fda18146a.png)

我们这里学习的内容要注意的是，添加与INSERT INTO和VALUES绑定，前者跟着我们的列名，而后者则跟着我们要添加的值。这里值得一提的是我们VALUES后跟的值的数量和类型要和列名中的一样，其会对应将值赋予给对应的列名。如果我们的表明后没有跟随列名，那么VALUES后跟随的值就不但要有和我们的最初的列相当的数据，而且其顺序必须和我们当初创造该表时按顺序放置的变量顺序一样才能正确完成赋值（记不住顺序了可以调用DESC命令来看看对应表的变量的顺序），其下的值也会按照顺序对应赋予。

如果我们要批量添加的话，只要在VALUES后的小括号上多加几个小括号然后按照同样的规则写就可以了

其演示代码如下（我们这里默认我们已经实现创建好了一个product的数据表了）

```
/*
   给指定列添加数据
   标准语法：
      INSERT INTO 表名(列名1,列名2,...) VALUES (值1,值2,...);
*/
-- 向product表添加一条数据
    INSERT INTO product (id,NAME,price,stock,insert_time) VALUES (1,'手机',1999.99,25,'2020-02-02');

-- 向product表添加指定列数据
    INSERT INTO product (id,NAME,price) VALUES (2,'电脑',3999.99);

/*
   给全部列添加数据
   标准语法：
      INSERT INTO 表名 VALUES (值1,值2,值3,...);
*/
-- 默认给全部列添加数据
    INSERT INTO product VALUES (3,'冰箱',1500,35,'2030-03-03');

/*
   批量添加所有列数据
   标准语法：
      INSERT INTO 表名 VALUES (值1,值2,值3,...),(值1,值2,值3,...),(值1,值2,值3,...);
*/
-- 批量添加数据
    INSERT INTO product VALUES (4,'洗衣机',800,15,'2030-05-05'),(5,'微波炉',300,45,'2030-06-06');
```

这里如果我们只对某一些变量赋予了值，那么其他没有被赋予值的变量会被赋予null

## 修改和删除表数据

这里我们要搞清楚一点，那就是我们修改表中的数据是要加上一个WHERE的判断条件的，否则，我们的所有的数据都会被修改，这是非常恐怖的事情。同理删除也是

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE260d28a68d4d6394b61a88db778ca43d.png)

这里我们的修改语句是和UPDATE和SET绑定的，前者写要修改的表明，后者则写对应的列名和要修改的值，而WHERE则是用于写筛选判断条件的。删除语句则和DELETE FROM绑定在一起

其示例代码如下

```
/*
   修改表数据
   标准语法：
      UPDATE 表名 SET 列名1 = 值1,列名2 = 值2,... [where 条件];
*/
-- 修改手机的价格为3500
    UPDATE product SET price=3500 WHERE NAME='手机';

-- 修改电脑的价格为1800、库存为36
    UPDATE product SET price=1800,stock=36 WHERE NAME='电脑';


/*
   删除表数据
   标准语法：
      DELETE FROM 表名 [WHERE 条件];
*/
-- 删除product表中的微波炉信息
    DELETE FROM product WHERE NAME='微波炉';

-- 删除product表中库存为10的商品信息
    DELETE FROM product WHERE stock=10;
```

# DQL

那么现在我们来学习我们MySQL里面最为重要的一章，也就是DQL，其主要的作用是用于查询数据表中的数据。它是那么的重要，以至于我们要专门分出一章来讲解它

那么在讲解之前，我们要先创造一个用于查询的数据表，其创建的代码如下

```
-- 创建db1数据库
    CREATE DATABASE db1;

-- 使用db1数据库
    USE db1;

-- 创建数据表
    CREATE TABLE product(
            id INT,          -- 商品编号
            NAME VARCHAR(20),  -- 商品名称
    price DOUBLE,     -- 商品价格
    brand VARCHAR(10), -- 商品品牌
    stock INT,    -- 商品库存
    insert_time DATE        -- 添加时间
);

-- 添加数据
    INSERT INTO product VALUES
            (1,'华为手机',3999,'华为',23,'2088-03-10'),
(2,'小米手机',2999,'小米',30,'2088-05-15'),
        (3,'苹果手机',5999,'苹果',18,'2088-08-20'),
        (4,'华为电脑',6999,'华为',14,'2088-06-16'),
        (5,'小米电脑',4999,'小米',26,'2088-07-08'),
        (6,'苹果电脑',8999,'苹果',15,'2088-10-25'),
        (7,'联想电脑',7999,'联想',NULL,'2088-11-11');
```

全部选中然后点击运行就完了其实，其会按照顺序给我们自动运行的

接着我们来看看我们的DQL的一些语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb8b6316b1a41001b8c015e78b0dbc936.png)

SELECT关键字用于选择查询的字段列表，所谓的字段列表简单理解就是我们的变量名。然后FROM选择的是我们的表明列表，这个懂得都懂。WHERE则是用于写入我们用于筛选的条件的。而GROUP BY则是用于分组查询的，而在分组之后，我们如果想要过滤动作，那么就不能用WHERE而是用HAVING，ORDER BY则是排序的方法，后面跟随排序的字段和排序的规则。最后LIMIT用于分页，后面跟随一个分页的参数。

注意啊，我们上面的查询的场景都是说查询表的数据啊，别想到查询表本身去了。

## 查询全部

本节我们就来学习DQL的表数据查询的语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE14e88070ff57b71bf39871bbc070323f.png)

首先是查询全部的语法，SELECT * FROM 表名;就可以了，这里的*其实也就代表全部。如果我们只是想要查询某些指定的列，那么可以在SELECT中加上指定的列名。我们还可以去除重复查询，比如说加入我们指定我们只找品牌，那么我们就没必要获得每一个数据的所有品牌，对应的品牌有一个就行了，此时就需要用到DISTINCT来帮助去重。然后是计算列的值，我们可以指定某一些列在其显示的时候给我们加上一些我们指定的数量，如果说该数据为null的话，我们也可以结合ifnull函数来帮助我们进行判断，先赋予其默认值就可以了。最后我们在结果上这一列的数据内容会显示一个很长的名字，该名字就是我们的当初设置的命令，不便于阅读，我们可以结合AS命令，来给其添加别名。事实上，这个AS命令是可以省略的，只要在别名前面加个空格就可以了

其演示代码如下

```
/*
   查询全部数据
   标准语法：
      SELECT * FROM 表名;
*/
-- 查询product表所有数据
    SELECT * FROM product;


/*
   查询指定列
   标准语法：
      SELECT 列名1,列名2,... FROM 表名;
*/
-- 查询名称、价格、品牌
    SELECT NAME,price,brand FROM product;


/*
   去除重复查询
   标准语法：
      SELECT DISTINCT 列名1,列名2,... FROM 表名;
*/
-- 查询品牌
    SELECT brand FROM product;
-- 查询品牌，去除重复
    SELECT DISTINCT brand FROM product;



/*
   计算列的值
   标准语法：
      SELECT 列名1 运算符(+ - * /) 列名2 FROM 表名;
      
   如果某一列为null，可以进行替换
   ifnull(表达式1,表达式2)
   表达式1：想替换的列
   表达式2：想替换的值
*/
-- 查询商品名称和库存，库存数量在原有基础上加10
    SELECT NAME,stock+10 FROM product;

-- 查询商品名称和库存，库存数量在原有基础上加10。进行null值判断
    SELECT NAME,IFNULL(stock,0)+10 FROM product;



/*
   起别名
   标准语法：
      SELECT 列名1,列名2,... AS 别名 FROM 表名;
*/
-- 查询商品名称和库存，库存数量在原有基础上加10。进行null值判断。起别名为getSum
    SELECT NAME,IFNULL(stock,0)+10 AS getSum FROM product;
    SELECT NAME,IFNULL(stock,0)+10 getSum FROM product;
```

## 条件查询

接着我们来学习条件查询，这是最符合我们生活场景的一种查询，同时也是重点。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE42b502ba5dc00ac4994272ae0ed69270.png)

我们这里要着重提的是条件查询的语法，SELECT 列名列表 FROM 表明 WHERE 条件，具体的条件查询的分类就自己去看图了。模糊查询时，%表示多个任意字符，_表示单个任意字符。还有一点要注意的是在MySql里，=是表示相等，而不是赋值。最后多个条件查询时如果是同一个变量的条件查询，可以用IN关键字来进行囊括

其演示代码如下

```
/*
   条件查询
   标准语法：
      SELECT 列名列表 FROM 表名 WHERE 条件;
*/
-- 查询库存大于20的商品信息
        SELECT * FROM product WHERE stock > 20;


        -- 查询品牌为华为的商品信息
        SELECT * FROM product WHERE brand='华为';

        -- 查询金额在4000 ~ 6000之间的商品信息
        SELECT * FROM product WHERE price >= 4000 AND price <= 6000;
        SELECT * FROM product WHERE price BETWEEN 4000 AND 6000;


        -- 查询库存为14、30、23的商品信息
        SELECT * FROM product WHERE stock=14 OR stock=30 OR stock=23;
        SELECT * FROM product WHERE stock IN(14,30,23);


        -- 查询库存为null的商品信息
        SELECT * FROM product WHERE stock IS NULL;


        -- 查询库存不为null的商品信息
        SELECT * FROM product WHERE stock IS NOT NULL;


        -- 查询名称以小米为开头的商品信息
        SELECT * FROM product WHERE NAME LIKE '小米%';

        -- 查询名称第二个字是为的商品信息
        SELECT * FROM product WHERE NAME LIKE '_为%';

        -- 查询名称为四个字符的商品信息
        SELECT * FROM product WHERE NAME LIKE '____';

        -- 查询名称中包含电脑的商品信息
        SELECT * FROM product WHERE NAME LIKE '%电脑%';
```

## 聚合函数查询

所谓聚合函数，简单来说就是可以将一列数据进行纵向计算。其可以统计数量，求和求平均值求最大最小值。其语法和前面的条件查询的差不多，其中where的筛选条件可加可不加

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe3e8769889a7a1f61c418e21010b8955.png)

我们可以构造其示例代码如下：

```
/*
   聚合函数
   标准语法：
      SELECT 函数名(列名) FROM 表名 [WHERE 条件];
*/
-- 计算product表中总记录条数
        SELECT COUNT(*) FROM product;

        -- 获取最高价格
        SELECT MAX(price) FROM product;


        -- 获取最低库存
        SELECT MIN(stock) FROM product;

        -- 获取总库存数量
        SELECT SUM(stock) FROM product;


        -- 获取品牌为苹果的总库存数量
        SELECT SUM(stock) FROM product WHERE brand='苹果';

        -- 获取品牌为小米的平均商品价格
        SELECT AVG(price) FROM product WHERE brand='小米';

```

## 排序查询

然后我们来学习排序查询，排序查询的语法是SELECT 列名列表 FROM 表明 [WHERE 条件] ORDER BY 列名 排序方式，列名，排序方式...；

这里ASC表示升序，DESC表示降序，注意如果有多个排序条件，那么只有当第一个条件一样的时候才会去判断第二个条件，同时升序也是默认的排序条件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6083dead1242a0e6d096d2e3ee549e03.png)

我们可以写入示例代码如下

```
/*
   排序查询
   标准语法：
      SELECT 列名 FROM 表名 [WHERE 条件] ORDER BY 列名1 排序方式1,列名2 排序方式2;
*/
-- 按照库存升序排序
        SELECT * FROM product ORDER BY stock ASC;

        -- 查询名称中包含手机的商品信息。按照金额降序排序
        SELECT * FROM product WHERE NAME LIKE '%手机%' ORDER BY price DESC;

        -- 按照金额升序排序，如果金额相同，按照库存降序排列
        SELECT * FROM product ORDER BY price ASC,stock DESC;
```

## 分组查询

分组查询的语法是SELECT 列名列表 FROM 表明 GROUP BY 分组列名，这里我们可以先添加WHERE条件进行过滤，只对某一些数据进行分组，分组之后如果还想再过滤的话就要使用HAVING，如果想要排序的话那就继续使用ORDER BY语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe1ffb12c7422d59ada10d30620e05be2.png)

我们可以构造示例代码如下

```
/*
   分组查询
   标准语法：
      SELECT 列名 FROM 表名 [WHERE 条件] GROUP BY 分组列名 [HAVING 分组后条件过滤] [ORDER BY 排序列名 排序方式];
*/
-- 按照品牌分组，获取每组商品的总金额
        SELECT brand,SUM(price) FROM product GROUP BY brand;

        -- 对金额大于4000元的商品，按照品牌分组,获取每组商品的总金额
        SELECT brand,SUM(price) FROM product WHERE price > 4000 GROUP BY brand;

        -- 对金额大于4000元的商品，按照品牌分组，获取每组商品的总金额，只显示总金额大于7000元的
        SELECT brand,SUM(price) getSum FROM product WHERE price > 4000 GROUP BY brand HAVING getSum > 7000;

        -- 对金额大于4000元的商品，按照品牌分组，获取每组商品的总金额，只显示总金额大于7000元的、并按照总金额的降序排列
        SELECT brand,SUM(price) getSum FROM product WHERE price > 4000 GROUP BY brand HAVING getSum > 7000 ORDER BY getSum DESC;
```

## 分页查询

我们来学习本章节的最后一个内容，分页查询

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE56d52e6d4d4a3a09c02cb7242c9d7f51.png)

这里的语法其实和之前的差不多，只不过多了一个LIMIT，这个LIMIT后跟随两个参数，分别是当前页数和每页显示的条数，每页显示的条数是一个我们人为规定的值，而当前页数却不是传统的12345，而要用一个公式来计算，这个公式是(当前页数-1)*每页显示的条数。

那么我们可以构造其示例代码如下

```
/*
   分页查询
   标准语法：
      SELECT 列名 FROM 表名 
      [WHERE 条件] 
      [GROUP BY 分组列名]
      [HAVING 分组后条件过滤] 
      [ORDER BY 排序列名 排序方式] 
      LIMIT 当前页数,每页显示的条数;
   
   LIMIT 当前页数,每页显示的条数;
   公式：当前页数 = (当前页数-1) * 每页显示的条数
*/
-- 每页显示3条数据

        -- 第1页  当前页数=(1-1) * 3
        SELECT * FROM product LIMIT 0,3;

        -- 第2页  当前页数=(2-1) * 3
        SELECT * FROM product LIMIT 3,3;

        -- 第3页  当前页数=(3-1) * 3
        SELECT * FROM product LIMIT 6,3;
```

# 约束

## 外键约束

本章节我们先来学习外键约束，首先我们要先介绍下为什么又外键约束。当表与表的数据有关联性的时候，如果没有外键约束，那么就无法保证数据的准确性。比如假设我们有两个数据表，一个是订单表orderlist，另一个是用户表user，我们用户表的id和uid关联，id和uid一致就表示这个订单是谁的，比如在下图中，张三的就拥有12两个订单，有了这个外键约束，我们就可以确保我们的用户表中的用户在有关联的时候，不会被随意的删除，订单中的订单也不会随意添加一个没有对应id的uid订单

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1bc2a79fb4236c4fb18d1fdb91a1d1e0.png)

简而言之，外键约束的作用就是让表与表之间产生关联关系，从而保证数据的准确性。接下来请看下图关于外键约束的语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9a0dca8476e953b1121012488bba5452.png)

创建外键约束的方式有两种，一种是在创建数据表的时候创建约束，另一种是建表之后单独添加外键约束，两种方法用的本质语法都是大差不差的，都是也要用CONSTRAINT 外键名 FOREIGN KEY(本表外键列名) REFERENCES 主表名(主键列名)。我们要区分谁是主表的话，可以简单记住一点，谁调用外键约束创建命令，谁就不是主表。这样会好记很多

而由于我们这里都是对标进行的操作，因此最开头自然也是要加上ALTER TABLE的这个字段，有删除的话还要加上DROP关键字

当我们成功构建了一个约束关系之后，我们再来查看表

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8f9c639ae9b818d96de8d5f2a30f0f10.png)

我们会发现这个表对应的有约束的部分的上方多了个钥匙扣住了两个表的感觉，这就用图形告诉了我们，这里有约束

我们可以构造示例代码如下

```
-- 创建db2数据库
        CREATE DATABASE db2;

        -- 使用db2数据库
        USE db2;

/*
   外键约束
   标准语法：
      CONSTRAINT 外键名 FOREIGN KEY (本表外键列名) REFERENCES 主表名(主表主键列名)
*/
        -- 建表时添加外键约束
        -- 创建user用户表
        CREATE TABLE USER(
        id INT PRIMARY KEY AUTO_INCREMENT,    -- id
        NAME VARCHAR(20) NOT NULL             -- 姓名
        );
        -- 添加用户数据
        INSERT INTO USER VALUES (NULL,'张三'),(NULL,'李四');

        -- 创建orderlist订单表
        CREATE TABLE orderlist(
        id INT PRIMARY KEY AUTO_INCREMENT,    -- id
        number VARCHAR(20) NOT NULL,          -- 订单编号
        uid INT,               -- 外键列
        CONSTRAINT ou_fk1 FOREIGN KEY (uid) REFERENCES USER(id)
        );
        -- 添加订单数据
        INSERT INTO orderlist VALUES (NULL,'hm001',1),(NULL,'hm002',1),
        (NULL,'hm003',2),(NULL,'hm004',2);


        -- 添加一个订单，但是没有真实用户。添加失败
        INSERT INTO orderlist VALUES (NULL,'hm005',3);

        -- 删除李四用户。删除失败
        DELETE FROM USER WHERE NAME='李四';




/*
   删除外键约束
   标准语法：
      ALTER TABLE 表名 DROP FOREIGN KEY 外键名;
*/
        -- 删除外键约束
        ALTER TABLE orderlist DROP FOREIGN KEY ou_fk1;


/*
   建表后单独添加外键约束
   标准语法：
      ALTER TABLE 表名 ADD CONSTRAINT 外键名 FOREIGN KEY (本表外键列名) REFERENCES 主表名(主键列名);
*/
        -- 添加外键约束
        ALTER TABLE orderlist ADD CONSTRAINT ou_fk1 FOREIGN KEY (uid) REFERENCES USER(id);
```

## 外键级联操作

什么是外键级联操作呢？其实就是两个，一是级联更新，二是级联删除。所谓级联更新，就是当主表中的数据进行修改的时候，从表的数据也会随之修改。而级联删除也是如此。虽然很方便，但是实际开发中不怎么用，因为其总是牵一发而动全身，不好管理我们的数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc30458455564300eb2d9cfd82b27945f.png)

本节内容只做了解，我们来看看语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE90cf70ad44f2321c3a8b1eff5a769b96.png)

最后我们可以构造演示代码如下

```
/*
   添加外键约束，同时添加级联更新  标准语法：
   ALTER TABLE 表名 ADD CONSTRAINT 外键名 FOREIGN KEY (本表外键列名) REFERENCES 主表名(主键列名) 
   ON UPDATE CASCADE;
   
   添加外键约束，同时添加级联删除  标准语法：
   ALTER TABLE 表名 ADD CONSTRAINT 外键名 FOREIGN KEY (本表外键列名) REFERENCES 主表名(主键列名) 
   ON DELETE CASCADE;
   
   添加外键约束，同时添加级联更新和级联删除  标准语法：
   ALTER TABLE 表名 ADD CONSTRAINT 外键名 FOREIGN KEY (本表外键列名) REFERENCES 主表名(主键列名) 
   ON UPDATE CASCADE ON DELETE CASCADE;
*/
-- 删除外键约束
        ALTER TABLE orderlist DROP FOREIGN KEY ou_fk1;

        -- 添加外键约束，同时添加级联更新和级联删除
        ALTER TABLE orderlist ADD CONSTRAINT ou_fk1 FOREIGN KEY (uid) REFERENCES USER(id)
        ON UPDATE CASCADE ON DELETE CASCADE;


        -- 将李四这个用户的id修改为3,订单表中的uid也自动修改
        UPDATE USER SET id=3 WHERE id=2;

        -- 将李四这个用户删除,订单表中的该用户所属的订单也自动删除
        DELETE FROM USER WHERE id=3;
```

## 约束介绍

我们先来学习下什么是约束，约束就是对表中的数据进行限定，保证数据的正确、有效和完整。比如我们可以约束一列的数据是不可重复的等等内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE25425d7a787d88d1b087748fa3675c26.png)

约束有四个重要关键字，分别是PRIMARY KEY(主键约束)、PRIMARY KEY AUTO_INCREMENT(主键自增)、UNIQUE(唯一约束)、NOT NULL(非空约束)。我们接下来一一学习这四个关键字

## 主键约束

主键约束有以下三个特点，1、主键约束默认该主键必须是非空和唯一的。2、一张表只能有一个主键。3、主键一般用于表中数据的唯一标识

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa8f2dc11197075ff0bd099fa592347b3.png)

我们添加主键约束有两种方式，一种是建表时添加，语法是在对应的数据类型上添加上PRIMARY KEY关键词。另外一种是建表后单独添加主键约束，其语法是ALTER TABLE 表明 MODIFY 列名 数据类型 PRIMARY KEY，当然我们也可以删除主键约束，其语法就是和DROP关键词绑定罢了。

我们可以构造示例代码如下

```
-- 创建学生表(编号、姓名、年龄)  编号设为主键
        CREATE TABLE student(
        id INT PRIMARY KEY,
        NAME VARCHAR(30),
        age INT
        );

        -- 查询学生表的详细信息
        DESC student;

        -- 添加数据
        INSERT INTO student VALUES (1,'张三',23);
        INSERT INTO student VALUES (2,'李四',24);

        -- 删除主键
        ALTER TABLE student DROP PRIMARY KEY;

        -- 建表后单独添加主键约束
        ALTER TABLE student MODIFY id INT PRIMARY KEY;
```

## 主键自增约束

所谓主键自增约束，其实就是说会自动增长的意思。其语法是AUTO_INCREMENT，其添加删除语法其实和上一节学习的主键约束差不多

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE284a4ab1e888009ddb6ea71efaccbcc4.png)

这里有一点必须提一下，就是MySQL里的自增约束，必须配合键的约束一起使用，这个键的约束不一定要是主键约束，可以是其他约束，但是自增约束是不能独立使用的。还有就是其删除主键自增约束调用的命令是MODIFY，其本质是将自增约束修改为无，而不删除主键。从其中没有调用DROP我们也能够看出端倪

那么我们可以构造其示例代码如下

```
-- 创建学生表(编号、姓名、年龄)  编号设为主键自增
        CREATE TABLE student(
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(30),
        age INT
        );

        -- 查询学生表的详细信息
        DESC student;

        -- 添加数据
        INSERT INTO suudent VALUES (NULL,'张三',23),(NULL,'李四',24);

        -- 删除自增约束
        ALTER TABLE student MODIFY id INT;
        INSERT INTO student VALUES (NULL,'张三',23);

        -- 建表后单独添加自增约束
        ALTER TABLE student MODIFY id INT AUTO_INCREMENT;
```

最后我们要提一点的是，自增约束是中，其逐渐增加的值是根据其自身所用过的值的，而不是列表中目前存在的值的。简单来说，如果我们创建了一个学生，其自增的值到了3，之后我们删除掉这个3，再创建一个用户，其自增值会到达4，而不是3。同时当我们的自增约束和主键约束一起用的时候，我们可以往里面插入null值

## 唯一约束

唯一约束其实就是让我们的值变得唯一，不能允许存在相同的值。其创建的方式跟此前的差不多，关键词是UNIQUE

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE40d2dff00eba0fe153c0a3788e72ba50.png)

这里值得特别说一下删除，删除不是简单的和DROP绑定的，而是和DROP INDEX绑定的，这个要记住，感觉也只有这个唯一约束的删除方式比较特殊

那么我们可以构造示例代码如下：

```
-- 创建学生表(编号、姓名、年龄)  编号设为主键自增，年龄设为唯一
        CREATE TABLE student(
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(30),
        age INT UNIQUE
        );


        -- 查询学生表的详细信息
        DESC student;

        -- 添加数据
        INSERT INTO student VALUES (NULL,'张三',23);
        INSERT INTO student VALUES (NULL,'李四',23);

        -- 删除唯一约束
        ALTER TABLE student DROP INDEX age;

        -- 建表后单独添加唯一约束
        ALTER TABLE student MODIFY age INT UNIQUE;

```

这里我们还要提一下，当我添加唯一约束的时候，要确保本身我们的数据满足唯一的约束的条件，才能添加成功。简单来说，你不能往已经有两个重复元素的变量里增加唯一约束，这肯定报错的。

## 非空约束

非空约束其实就是规定我们的变量不能为null值，关键字是NOT NULL，其删除采用的是MODIFY，同时如果我们给一个null值的列赋予非空约束，其可以成功，但会有警告，同时将为null值的变量改为空值，没有任何数据，但不是null

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb6f535fff62da3c5ecbbaf4f1de4669c.png)

其示例代码如下

```
-- 创建学生表(编号、姓名、年龄)  编号设为主键自增，姓名设为非空，年龄设为唯一
        CREATE TABLE student(
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(30) NOT NULL,
        age INT UNIQUE
        );

        -- 查询学生表的详细信息
        DESC student;

        -- 添加数据
        INSERT INTO student VALUES (NULL,'张三',23);

        -- 删除非空约束
        ALTER TABLE student MODIFY NAME VARCHAR(30);
        INSERT INTO student VALUES (NULL,NULL,25);

        -- 建表后单独添加非空约束
        ALTER TABLE student MODIFY NAME VARCHAR(30) NOT NULL;
```

# 多表操作

接下来我们要学习的内容就偏向于面试而非实际了，因此我们可以抱着了解的心态来，但并非是说不重要，该学还是要学的，我们照旧要打起十分精神。

首先我们来介绍下什么是多表，顾名思义，多表其实就是有多张数据表，而表与表之间由一定的关联关系，这种关联关系可以通过外键约束实现

多表的分类有三种，分别是一对一、一对多、多对多。这三种，我们先来介绍第一种

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2f7880d3f3dee0315cbbafc8059bb08a.png)

## 一对一

一对一的情况，其最佳的例子就是人和身份证，一个人只能有一张身份证，同时一张身份证也只能对应一个人。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE05b897766e904dbc836ddefc7117d5f3.png)

其建表原则是在任意一个表中建立外键，去关联另外一个表的主键。

我们可以构造其示例代码如下

```
-- 创建db3数据库
        CREATE DATABASE db3;

        -- 使用db3数据库
        USE db3;

        -- 创建person表
        CREATE TABLE person(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        NAME VARCHAR(20)                        -- 姓名
        );
        -- 添加数据
        INSERT INTO person VALUES (NULL,'张三'),(NULL,'李四');

        -- 创建card表
        CREATE TABLE card(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        number VARCHAR(20) UNIQUE NOT NULL,    -- 身份证号
        pid INT UNIQUE,                         -- 外键列
        CONSTRAINT cp_fk1 FOREIGN KEY (pid) REFERENCES person(id)
        );
        -- 添加数据
        INSERT INTO card VALUES (NULL,'12345',1),(NULL,'56789',2);
```

在我们的sqyog中，我们可以下图中的红圈选项，打开架构设计器，然后将对应的表拖入设计器中，可以看到表与表之间的关系

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe5b52753219df997a7f4c907cdab2b4d.png)

我们这里是一对一的关系，并且person是主键，因此card表内的pid指向person表中的主键id。而且我们也可以看到，这里两边都写着1

## 一对多

一对多的关系，在现实生活中有很多，最简单的场景就是，购物车，也就是用户和订单，一个用户可以有多个订单，多个订单可以是一个人的。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd0a89d66bb5a8b7f4f2c52fa68ee4202.png)

我们最重要的是要记住一对多的建表原则，记住，一对多的建表原则是在多的一方建立外键约束，来关联一的一方主键。所以对于一对多的场景而言，多的那一方，肯定不是主键。

我们可以构造其示例代码如下

```
-- 创建user表
        CREATE TABLE USER(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        NAME VARCHAR(20)                        -- 姓名
        );
        -- 添加数据
        INSERT INTO USER VALUES (NULL,'张三'),(NULL,'李四');

        -- 创建orderlist表
        CREATE TABLE orderlist(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        number VARCHAR(20),                     -- 订单编号
        uid INT,            -- 外键列
        CONSTRAINT ou_fk1 FOREIGN KEY (uid) REFERENCES USER(id)
        );
        -- 添加数据
        INSERT INTO orderlist VALUES (NULL,'hm001',1),(NULL,'hm002',1),(NULL,'hm003',2),(NULL,'hm004',2);



/*
   商品分类和商品
*/
        -- 创建category表
        CREATE TABLE category(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        NAME VARCHAR(10)                        -- 分类名称
        );
        -- 添加数据
        INSERT INTO category VALUES (NULL,'手机数码'),(NULL,'电脑办公');

        -- 创建product表
        CREATE TABLE product(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        NAME VARCHAR(30),        -- 商品名称
        cid INT,            -- 外键列
        CONSTRAINT pc_fk1 FOREIGN KEY (cid) REFERENCES category(id)
        );
        -- 添加数据
        INSERT INTO product VALUES (NULL,'华为P30',1),(NULL,'小米note3',1),
        (NULL,'联想电脑',2),(NULL,'苹果电脑',2);
```

我们同样可以在架构设计器上查看这两者的关系

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe2b5206dc525b54d33b39ad0c692cb1b.png)

## 多对多

多对多的关系最好的就是例子就是学生选课，一个学生可以选多门课程，而一门课程也可以被多个学生选择

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8d8e7cedcd66c84def9e4daf0e4ba0cf.png)

我们多对多的建表原则是要借助第三张中间表，中间表至少要包含两个列。这两个列分别作为中间表的外键，关联另外两张表的主键。比如在上图中，我们中间的表就可以表示张三选择了两门课程，同时李四也选择了两门课程，这里就是靠sid和cid来令两张表关联出关系的

最后我们可以构造示例代码如下

```
-- 创建student表
        CREATE TABLE student(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        NAME VARCHAR(20)         -- 学生姓名
        );
        -- 添加数据
        INSERT INTO student VALUES (NULL,'张三'),(NULL,'李四');

        -- 创建course表
        CREATE TABLE course(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        NAME VARCHAR(10)         -- 课程名称
        );
        -- 添加数据
        INSERT INTO course VALUES (NULL,'语文'),(NULL,'数学');


        -- 创建中间表
        CREATE TABLE stu_course(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        sid INT,  -- 用于和student表中的id进行外键关联
        cid INT,  -- 用于和course表中的id进行外键关联
        CONSTRAINT sc_fk1 FOREIGN KEY (sid) REFERENCES student(id), -- 添加外键约束
        CONSTRAINT sc_fk2 FOREIGN KEY (cid) REFERENCES course(id)   -- 添加外键约束
        );
        -- 添加数据
        INSERT INTO stu_course VALUES (NULL,1,1),(NULL,1,2),(NULL,2,1),(NULL,2,2);
```

其在架构设计器中的表示如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa40d69881b962a485723e12788912352.png)

当然，我们也可以很容易的知道，多对多的关系里，我们的中间表是没有一个主键的，两个表分别有主键。其实，这个可以理解为两个一对多，用两个一对多构建一个多对多

## 多表查询

接下来我们来学习多表查询，多表查询的分类有内连接查询、外连接查询、子查询、自关联查询。我们接下来就来一个个学习

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb2d126164e9c3992041545870f2d7fa0.png)

但是在学习之前，我们要先做查询所需要的数据准备，请看代码

```
-- 创建db4数据库
        CREATE DATABASE db4;
        -- 使用db4数据库
        USE db4;

        -- 创建user表
        CREATE TABLE USER(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 用户id
        NAME VARCHAR(20),        -- 用户姓名
        age INT                                 -- 用户年龄
        );
        -- 添加数据
        INSERT INTO USER VALUES (1,'张三',23);
        INSERT INTO USER VALUES (2,'李四',24);
        INSERT INTO USER VALUES (3,'王五',25);
        INSERT INTO USER VALUES (4,'赵六',26);


        -- 订单表
        CREATE TABLE orderlist(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 订单id
        number VARCHAR(30),          -- 订单编号
        uid INT,    -- 外键字段
        CONSTRAINT ou_fk1 FOREIGN KEY (uid) REFERENCES USER(id)
        );
        -- 添加数据
        INSERT INTO orderlist VALUES (1,'hm001',1);
        INSERT INTO orderlist VALUES (2,'hm002',1);
        INSERT INTO orderlist VALUES (3,'hm003',2);
        INSERT INTO orderlist VALUES (4,'hm004',2);
        INSERT INTO orderlist VALUES (5,'hm005',3);
        INSERT INTO orderlist VALUES (6,'hm006',3);
        INSERT INTO orderlist VALUES (7,'hm007',NULL);


        -- 商品分类表
        CREATE TABLE category(
        id INT PRIMARY KEY AUTO_INCREMENT,  -- 商品分类id
        NAME VARCHAR(10)                    -- 商品分类名称
        );
        -- 添加数据
        INSERT INTO category VALUES (1,'手机数码');
        INSERT INTO category VALUES (2,'电脑办公');
        INSERT INTO category VALUES (3,'烟酒茶糖');
        INSERT INTO category VALUES (4,'鞋靴箱包');


        -- 商品表
        CREATE TABLE product(
        id INT PRIMARY KEY AUTO_INCREMENT,   -- 商品id
        NAME VARCHAR(30),                    -- 商品名称
        cid INT, -- 外键字段
        CONSTRAINT cp_fk1 FOREIGN KEY (cid) REFERENCES category(id)
        );
        -- 添加数据
        INSERT INTO product VALUES (1,'华为手机',1);
        INSERT INTO product VALUES (2,'小米手机',1);
        INSERT INTO product VALUES (3,'联想电脑',2);
        INSERT INTO product VALUES (4,'苹果电脑',2);
        INSERT INTO product VALUES (5,'中华香烟',3);
        INSERT INTO product VALUES (6,'玉溪香烟',3);
        INSERT INTO product VALUES (7,'计生用品',NULL);


        -- 中间表
        CREATE TABLE us_pro(
        upid INT PRIMARY KEY AUTO_INCREMENT,  -- 中间表id
        uid INT, -- 外键字段。需要和用户表的主键产生关联
        pid INT, -- 外键字段。需要和商品表的主键产生关联
        CONSTRAINT up_fk1 FOREIGN KEY (uid) REFERENCES USER(id),
        CONSTRAINT up_fk2 FOREIGN KEY (pid) REFERENCES product(id)
        );
        -- 添加数据
        INSERT INTO us_pro VALUES (NULL,1,1);
        INSERT INTO us_pro VALUES (NULL,1,2);
        INSERT INTO us_pro VALUES (NULL,1,3);
        INSERT INTO us_pro VALUES (NULL,1,4);
        INSERT INTO us_pro VALUES (NULL,1,5);
        INSERT INTO us_pro VALUES (NULL,1,6);
        INSERT INTO us_pro VALUES (NULL,1,7);
        INSERT INTO us_pro VALUES (NULL,2,1);
        INSERT INTO us_pro VALUES (NULL,2,2);
        INSERT INTO us_pro VALUES (NULL,2,3);
        INSERT INTO us_pro VALUES (NULL,2,4);
        INSERT INTO us_pro VALUES (NULL,2,5);
        INSERT INTO us_pro VALUES (NULL,2,6);
        INSERT INTO us_pro VALUES (NULL,2,7);
        INSERT INTO us_pro VALUES (NULL,3,1);
        INSERT INTO us_pro VALUES (NULL,3,2);
        INSERT INTO us_pro VALUES (NULL,3,3);
        INSERT INTO us_pro VALUES (NULL,3,4);
        INSERT INTO us_pro VALUES (NULL,3,5);
        INSERT INTO us_pro VALUES (NULL,3,6);
        INSERT INTO us_pro VALUES (NULL,3,7);
        INSERT INTO us_pro VALUES (NULL,4,1);
        INSERT INTO us_pro VALUES (NULL,4,2);
        INSERT INTO us_pro VALUES (NULL,4,3);
        INSERT INTO us_pro VALUES (NULL,4,4);
        INSERT INTO us_pro VALUES (NULL,4,5);
        INSERT INTO us_pro VALUES (NULL,4,6);
        INSERT INTO us_pro VALUES (NULL,4,7);
```

这里我们所创建的各个数据表的关系如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeccc25046a7ac842c5609add5d34499f.png)

其中user表和orderlist表时一对多的关系，而category和product也是一对多的关系。然后user和product具有多对多的关系，其中us_pro是中间表。

在这里就是将用户绑定用户表，然后商品绑定商品表，然后中间表就将具体的用户和商品连接在一起，通过用户我们可以查看到多个中间表的数据，通过这个数据可以知道其购买的商品，又可以通过该商品知道其具体购买的商品种类，这也就是多对多的好处，虽然复杂了些，但的确适合更加复杂的业务

### 内连接查询

我们先来学习内连接查询，所谓内连接查询就是查询两张表有交集的部分数据，也就是主键和外键有关联的部分数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE02dea42ab77948061c050b72063be357.png)

查询的语法有两种，分别是SELECT 列名 FROM 表名1 [INNER] JOIN 表明2 ON 条件的显示内连接查询和SELECT 列名 FROM 表名1,表名2 WHERE 条件

的隐式内连接查询

那么我们可以写入其示例代码如下

```
/*
   显示内连接
   标准语法：
      SELECT 列名 FROM 表名1 [INNER] JOIN 表名2 ON 关联条件;
*/
-- 查询用户信息和对应的订单信息
        SELECT * FROM USER INNER JOIN orderlist ON orderlist.uid = user.id;

        -- 查询用户信息和对应的订单信息，起别名
        SELECT * FROM USER u INNER JOIN orderlist o ON o.uid=u.id;

        -- 查询用户姓名，年龄。和订单编号
SELECT
        u.name,       -- 用户姓名
        u.age,    -- 用户年龄
        o.number   -- 订单编号
FROM
        USER u          -- 用户表
INNER JOIN
        orderlist o     -- 订单表
ON
        o.uid=u.id;


/*
   隐式内连接
   标准语法：
      SELECT 列名 FROM 表名1,表名2 WHERE 关联条件;
*/
        -- 查询用户姓名，年龄。和订单编号
SELECT
        u.name,       -- 用户姓名
        u.age,    -- 用户年龄
        o.number   -- 订单编号
FROM
        USER u,       -- 用户表
        orderlist o     -- 订单表
WHERE
        o.uid=u.id;
```

这里我们首先要讲下显示内查询和隐式内查询的分别，简单来看，这两者的分别无非也就是前者是的关键字是INNER JOIN ... ON ，而后者的关键字是WHERE。不过底层上这两者到底有什么分别，说实话我也搞不懂，那就先不管吧。不过无论是哪种方式，我们都是要在最后加入判断条件的，比如在上面我们就加入了两张表的对应id要相等的判断条件

我们先看我们的第七行代码，可以看到我们写入判断条件时我们写入了全部的数据表的名称，这样未免有些不方便。因此在第10行里我们采用了将我们的数据表再命名的方式来简化我们的代码，以后我们写sql语句也可以常常使用这种方式来优化我们的代码、

其次是有时候我们可能会觉得我们的代码全挤在一行不好，那么我们就可以采用13~22行的代码，我们总是将关键词写在最左边，将我们的具体的命令内容写在一个换行符的后面，这样我们就可以让我们的代码变得美观，而且，还可以加注释

还有一点需要提及的是，在SQL语句中，即使我们是在后面进行的重命名动作，我们在前面仍然可以使用这些重命名之后的内容来进行调用，这样做是不会报错的

最后我们得到的结果如下，这个结果其实是将我们的user的数据和orderlist里的数据进行了对应的填充，这样的结果用户看到了就会一目了然。比如我们在这里就能轻易知道张三有两个订单，李四也有两个。中间的id列是orderlist的id列，没有什么意义，这里存在只是因为一开始我们没过滤掉这个无用列而已

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE42eb88c153a27266f59a6f17f80042e3.png)

### 外连接查询

现在我们来学习外连接查询，外连接查询分左外连接查询和右外连接查询。左外连接的查询的意思是查询左表的全部数据以及和左右两表有交集的数据。其查询语法是SELECT 列名 FROM 表名1 LEFT [OUTER] JOIN 表名2 ON 条件。这具体是什么意思呢？比方说在我们的用户表里，有一个用户是没有数据的，在我们之前的内连接查询里，我们只能查询到交集部分，那如果我们希望不但要查询到交集部分的内容，还希望将没有交集部分的内容也一起显示，这时候就需要用到我们的左外连接查询了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE397b38609d3ec1e3aa61d43db39ef84d.png)

同理还有右外连接查询，这个就依葫芦画瓢了，因此不再赘述。

那么我们可以写入其示例代码如下

```
/*
   左外连接
   标准语法：
      SELECT 列名 FROM 表名1 LEFT [OUTER] JOIN 表名2 ON 条件;
*/
-- 查询所有用户信息，以及用户对应的订单信息
SELECT
        u.*,
        o.number
FROM
        USER u
LEFT OUTER JOIN
        orderlist o
ON
        o.uid=u.id;
   


/*
   右外连接
   标准语法：
      SELECT 列名 FROM 表名1 RIGHT [OUTER] JOIN 表名2 ON 条件;
*/
        -- 查询所有订单信息，以及订单所属的用户信息
SELECT
        o.*,
        u.name
FROM
        USER u
RIGHT OUTER JOIN
        orderlist o
ON
        o.uid=u.id;
```

其实，左外连接和右外连接本质上是差不多的，因为实际上我们可以调整我们的调用命令的顺序来达到同样的效果，所以实际上我们只要记住一个就够了

### 子查询

子查询的概念是查询已经中嵌套了查询语句，我们将这个嵌套查询成为子查询。子查询也分情况，各种情况，首先是单行单列的子查询

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbaca3841c40fba7b2516ab89ff9bd95a.png)

多行单列的子查询和多行多列的子查询

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE958d02cdc208067de5306ea2484f5656.png)

其实，嵌套查询就类似于我们java中的直接调用我们的函数，然后该结果不赋予任何变量，直接将这个变量拿来用的感觉。请看代码

```
/*
   结果是单行单列的
   标准语法：
      SELECT 列名 FROM 表名 WHERE 列名=(SELECT 列名 FROM 表名 [WHERE 条件]);
*/
-- 查询年龄最高的用户姓名
        SELECT MAX(age) FROM USER;/*获得年龄最高的年龄*/
        SELECT NAME,age FROM USER WHERE age=(SELECT MAX(age) FROM USER);/*将该年龄用于查询，获得年龄最高的用户姓名*/


/*
   结果是多行单列的
   标准语法：
      SELECT 列名 FROM 表名 WHERE 列名 [NOT] IN (SELECT 列名 FROM 表名 [WHERE 条件]); 
*/
        -- 查询张三和李四的订单信息
        SELECT * FROM orderlist WHERE uid IN (1,2);/*查询uid为1和2的订单信息*/
        SELECT id FROM USER WHERE NAME IN ('张三','李四');/*查询张三和李四的uid*/
        /*将该查询的结构的uid直接用在第一个查询上，可以查询到对应的张三和李四的订单信息*/
        SELECT * FROM orderlist WHERE uid IN (SELECT id FROM USER WHERE NAME IN ('张三','李四'));


/*
   结果是多行多列的
   标准语法：
      SELECT 列名 FROM 表名 [别名],(SELECT 列名 FROM 表名 [WHERE 条件]) [别名] [WHERE 条件];
*/
        -- 查询订单表中id大于4的订单信息和所属用户信息
        SELECT * FROM orderlist WHERE id > 4;/*先获得id大于4的表*/
SELECT
        u.name,
        o.number
FROM
        USER u,
        (SELECT * FROM orderlist WHERE id > 4) o/*获取该表并命名为o，在该表中进行查询*/
WHERE
        o.uid=u.id;
```

如果是单行单列的结果，我们可以直接将该结果结合WHERE来用。如果是多行单列的结果，我们可以结合IN进行使用。如果得到的结果是一个多行多列的另外一个表，那么我们可以将这个表与另外一个表结合使用，进行一个内查询或者是外查询。其实我觉得理论上也可以对这个表进行再次查询，获得一个单行单列的结果然后再查询的，不过很麻烦就没有去试了。反正具体怎么用到时候看我们的业务逻辑吧

### 自关联查询

什么是自关联查询？也就是说，当我们的同一张表中的数据有关联性的时候，我们可以把这张表当成多个表来查询

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0f8690f60a1030e3e962bc56bd602d98.png)

举个例子，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe83a42f4dcffdc3d6cd8d95400bcdd44.png)

比方说在上图中，我们可以看到孙悟空的上一级是1005，而1005是唐僧。这就是一种在一张表中存在数据关联性的典型例子。如果我们现在想要实现一种需求，就是将所有的人的上一级都找出来，没有的也要找，那我们应该怎么办呢？这时候就要用到自关联查询

请看示例代码

```
-- 创建员工表
        CREATE TABLE employee(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 员工编号
        NAME VARCHAR(20),        -- 员工姓名
        mgr INT,            -- 上级编号
        salary DOUBLE           -- 员工工资
        );
        -- 添加数据
        INSERT INTO employee VALUES (1001,'孙悟空',1005,9000.00),
        (1002,'猪八戒',1005,8000.00),
        (1003,'沙和尚',1005,8500.00),
        (1004,'小白龙',1005,7900.00),
        (1005,'唐僧',NULL,15000.00),
        (1006,'武松',1009,7600.00),
        (1007,'李逵',1009,7400.00),
        (1008,'林冲',1009,8100.00),
        (1009,'宋江',NULL,16000.00);


        -- 查询所有员工的姓名及其直接上级的姓名，没有上级的员工也需要查询
/*
分析
   员工信息 employee表
   条件：employee.mgr = employee.id
   查询左表的全部数据，和左右两张表有交集部分数据，左外连接
*/
SELECT
        e1.id,
        e1.name,
        e1.mgr,
        e2.id,
        e2.name
FROM
        employee e1
LEFT OUTER JOIN
        employee e2
ON
        e1.mgr = e2.id;
```

我们这里是将一张表当调用两次，令其自己和自己比较，最后得到我们所需要的结果，这就是自关联查询。最后我们得到的结果如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1077ed8a6590ab934a9089a8703fa300.png)

这里我们的唐僧和宋江由于根本就没有对应的数据可供展示，因此我们可以看到其显示的除了前两列之外，都是null。因为前两列是调用原来的表的数据，而后两列的数据是要求比较到了结果之后才可以选择的，而由于根本比较不出来什么，所以结果是null

### 多表查询练习

由于本章节我们的内容很多，所以我们最好还是做一些对应的练习来加强我们对本章节的知识的印象。请看题目

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE74fe8a95d52f36dde77f3f9244890cd7.png)

但是捏，因为我们现在还是了解阶段，所以，我们这里的练习也不具体去练习，听个思路就行了。

我们这里对于任何的题目，在解题之前，我们都应该要进行对应的分析，要知道我们解题具体要使用什么语句，然后我们的解题又具体要用到怎样的判断条件，又要用到哪些表进行对比，之后我们再进行解题。请看题解代码

```
-- 1.查询用户的编号、姓名、年龄。订单编号
/*
分析
   用户的编号、姓名、年龄  user表      订单编号 orderlist表
   条件：user.id=orderlist.uid
*/
SELECT
        u.id,
        u.name,
        u.age,
        o.number
        FROM
        USER u,
        orderlist o
        WHERE
        u.id=o.uid;



        -- 2.查询所有的用户。用户的编号、姓名、年龄。订单编号
/*
分析
   用户的编号、姓名、年龄  user表    订单编号 orderlist表
   条件：user.id=orderlist.uid
   查询所有的用户，左外连接
*/
        SELECT
        u.id,
        u.name,
        u.age,
        o.number
        FROM
        USER u
        LEFT OUTER JOIN
        orderlist o
        ON
        u.id=o.uid;



        -- 3.查询所有的订单。用户的编号、姓名、年龄。订单编号
/*
分析
   用户的编号、姓名、年龄 user表    订单编号 orderlist表
   条件：user.id=orderlist.uid
   查询所有的订单，右外连接
*/
        SELECT
        u.id,
        u.name,
        u.age,
        o.number
        FROM
        USER u
        RIGHT OUTER JOIN
        orderlist o
        ON
        u.id=o.uid;



        -- 4.查询用户年龄大于23岁的信息。显示用户的编号、姓名、年龄。订单编号
/*
分析
   用户的编号、姓名、年龄 user表    订单编号 orderlist表
   条件：user.id=orderlist.uid AND user.age > 23
*/
        SELECT
        u.id,
        u.name,
        u.age,
        o.number
        FROM
        USER u,
        orderlist o
        WHERE
        u.id=o.uid
        AND
        u.age > 23;


        -- 5.查询张三和李四用户的信息。显示用户的编号、姓名、年龄。订单编号
/*
分析
   用户的编号、姓名、年龄 user表   订单编号 orderlist表
   条件：user.id=orderlist.uid AND user.name IN ('张三','李四')
*/
        SELECT
        u.id,
        u.name,
        u.age,
        o.number
        FROM
        USER u,
        orderlist o
        WHERE
        u.id=o.uid
        AND
        u.name IN ('张三','李四');


        -- 6.查询商品分类的编号、分类名称。分类下的商品名称
/*
分析
   商品分类的编号、分类名称 category表    商品名称 product表
   条件：category.id=product.cid
*/
        SELECT
        c.id,
        c.name,
        p.name
        FROM
        category c,
        product p
        WHERE
        c.id=p.cid;


        -- 7.查询所有的商品分类。商品分类的编号、分类名称。分类下的商品名称
/*
分析
   商品分类的编号、分类名称 category表    商品名称 product表
   条件：category.id=product.cid
   查询所有的商品分类，左外连接
*/
        SELECT
        c.id,
        c.name,
        p.name
        FROM
        category c
        LEFT OUTER JOIN
        product p
        ON
        c.id=p.cid;


        -- 8.查询所有的商品信息。商品分类的编号、分类名称。分类下的商品名称
/*
分析
   商品分类的编号、分类名称  category表   商品名称 product表
   条件：category.id=product.cid
   查询所有的商品信息，右外连接
*/
        SELECT
        c.id,
        c.name,
        p.name
        FROM
        category c
        RIGHT OUTER JOIN
        product p
        ON
        c.id=p.cid;



        -- 9.查询所有的用户和该用户能查看的所有的商品。显示用户的编号、姓名、年龄。商品名称
/*
分析
   用户的编号、姓名、年龄 user表   商品名称 product表    中间表 us_pro
   条件：us_pro.uid=user.id AND us_pro.pid=product.id
*/
        SELECT
        u.id,
        u.name,
        u.age,
        p.name
        FROM
        USER u,
        product p,
        us_pro up
        WHERE
        up.uid=u.id
        AND
        up.pid=p.id;



        -- 10.查询张三和李四这两个用户可以看到的商品。显示用户的编号、姓名、年龄。商品名称
/*
分析
   用户的编号、姓名、年龄 user表   商品名称 product表   中间表 us_pro
   条件：us_pro.uid=user.id AND us_pro.pid=product.id AND user.name IN ('张三','李四') 
*/
        SELECT
        u.id,
        u.name,
        u.age,
        p.name
        FROM
        USER u,
        product p,
        us_pro up
        WHERE
        up.uid=u.id
        AND
        up.pid=p.id
        AND
        u.name IN ('张三','李四');
```

主要问题在于第九第十题，有些难理解，其他都还好

# 视图

学习完了多表操作之后，现在我们来学习新的内容，视图！

## 视图的介绍

那么视图是什么呢？其实视图就是一种虚拟存在的数据表，这个数据表并不会在实际的数据库中存在。

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCEbec6b1c6605edf76db942ead5e2da493.png)

那么他又有什么作用呢？我们可以举个例子，假设我们要查询下图中对应城市的对应国家的话，那么我们一定要写一串很长的SQL语法代码，如果我们要再次查询的话，那么又要再次输入，这可太麻烦了。但是有了视图之后，我们就可以将我们第一次查询的表自动封装为一个虚拟存在的数据表，我们可以称之为city_country，那么以后我们还需要查找这种数据的话，我们直接查找这个表就完了，就不用这么麻烦了。

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCEe82931a6fd43fb38cd4b64f83402b6cf.png)

简单来说，视图的作用就是提高我们的查询效率。同时，为了接下来的学习，我们要写入一些数据准备，其代码如下

```
-- 创建db5数据库
        CREATE DATABASE db5;

        -- 使用db5数据库
        USE db5;

        -- 创建country表
        CREATE TABLE country(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 国家id
        NAME VARCHAR(30)                        -- 国家名称
        );
        -- 添加数据
        INSERT INTO country VALUES (NULL,'中国'),(NULL,'美国'),(NULL,'俄罗斯');

        -- 创建city表
        CREATE TABLE city(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 城市id
        NAME VARCHAR(30),        -- 城市名称
        cid INT, -- 外键列。关联country表的主键列id
        CONSTRAINT cc_fk1 FOREIGN KEY (cid) REFERENCES country(id)  -- 添加外键约束
        );
        -- 添加数据
        INSERT INTO city VALUES (NULL,'北京',1),(NULL,'上海',1),(NULL,'纽约',2),(NULL,'莫斯科',3);
```

## 视图的创建和查询

先来看看视图的创建语法

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCE5d99acbcc9cab365046e2fd37fb13af2.png)

我们创建视图的语法是CREATE VIEW 视图名称[(列名列表)] AS 查询语句，创建的视图的列名我们是可以自由指定的，如果不指定那么就会保持原来的命名。来看看我们的示例代码

```
/*
   创建视图
   标准语法
      CREATE VIEW 视图名称 [(列名列表)] AS 查询语句;
*/
-- 创建city_country视图，保存城市和国家的信息(使用指定列名)
        CREATE VIEW city_country (city_id,city_name,country_name) AS

SELECT
        c1.id,
        c1.name,
        c2.name
FROM
        city c1,
        country c2
WHERE
        c1.cid=c2.id;


   
/*
   查询视图
   标准语法
      SELECT * FROM 视图名称;
*/
        -- 查询视图
        SELECT * FROM city_country;
```

我们上面的代码是先写入了创建视图的命令，然后后面输入了对应的查询命令，我们这里采用了利用回车的方式写入的代码。

我们创建完毕之后往sqlyog的右边看，打开对应数据表的视图文件夹，可以在哪里看到我们创建的视图，如下图所示

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCE92a4bbdb2882c0af871ac1e1755a976b.png)

可以看到这个视图的图标有点危险啊，这个触犯到了某一位来自另类时空的智者，我反正先润了。

## 视图的修改和删除

其实，视图的修改和删除跟我们数据表的修改和删除，是大差不差的，这里我们先来看看语法

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCE0044c0438eeefb1fb79a4da65d599290.png)

这里我们重点要说下修改视图结构的语法，我们这里修改视图结构，后面要紧跟随一个原来的查询语句，而不是直接跟着原来生成的视图。我觉得可以理解为新生成了一个视图并取代了原来的视图。请看示例代码

```
/*
   修改视图数据
   标准语法
      UPDATE 视图名称 SET 列名=值 WHERE 条件;
   
   修改视图结构
   标准语法
      ALTER VIEW 视图名称 (列名列表) AS 查询语句; 
*/
-- 修改视图数据,将北京修改为深圳。(注意：修改视图数据后，源表中的数据也会随之修改)
        SELECT * FROM city_country;
        UPDATE city_country SET city_name='深圳' WHERE city_name='北京';


        -- 将视图中的country_name修改为name
        ALTER VIEW city_country (city_id,city_name,NAME) AS
SELECT
        c1.id,
        c1.name,
        c2.name
FROM
        city c1,
        country c2
WHERE
        c1.cid=c2.id;


/*
   删除视图
   标准语法
      DROP VIEW [IF EXISTS] 视图名称;
*/
        -- 删除city_country视图
        DROP VIEW IF EXISTS city_country;
```

# 备份与恢复

数据的备份和恢复也是十分重要的，因为我们的数据库一旦丢失，那就玩完了，是非常重大的问题，因此我们要学习数据库的备份和恢复。我们先来学习备份和恢复数据库的第一种方式，命令行方式

## 命令行方式

命令行方式的备份与恢复的方式都在下图中了，现在我们来逐一测试下这两种方式

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCEf562447365af61d720674d00cf60917a.png)

备份很简单，我们只要登录我们的Linux服务器，然后输入对应的命令就可以了，假设我们想要备份db5，并且保存到root根目录下，那么我们就直接在Linux服务器中输入mysqldump -u root -p db5 > /root/db5.sql，就可以备份了，之后我们可在根目录下看到db5.sql的文件。我们通过cat命令查看这个文件，我们会发现文件里保存的内容其实就是对应的命令行，可想而知，恢复的时候，其实它就是把所有备份好的命令行执行一遍就当是恢复了。

如果我们要恢复的话，那就有些麻烦了，但是我们还是可以去实现恢复。我们要恢复我们的数据库，第一步是要登录我们的MySQL，然后我们要删除已经备份的数据库，接着我们创建一个和之前删除的名称相同的数据库，然后我们使用该数据库，然后直接调用source /root/db5.sql命令再回车就可以恢复了。注意，前面的命令，我们都要加分号，但是最后一个恢复不用。恢复的地址是动态的，如果我们备份到了其他的地址，那么我们恢复的时候输入的路径就是其他的。

## 图形化工具方式

我们进入sqlyog，假设我们要备份db5，我们首先选中db5，然后我们右键选择备份，选择备份数据库。

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCE27221e9cadaf81f27a0e949e008ecee2.png)

然后自己指定我们的保存的数据的文件名和路劲，选择导出，就会生成对应的文件了

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCE954141c64840b942600ec34944bd6c49.png)

文件内容也是一大堆命令，跟我们在Linux中的备份是一样的。最后我们删除db5，创建db5，然后我们选择导入，执行SQL脚本

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCE9939202f8c68f3112a19674c41d71e76.png)

然后我们自己指定用于恢复的文件的路径，点击执行，确定执行就可以了

![](D:\Rolin的学习笔记\youdaonote-pull\youdaonote\youdaonote-images\WEBRESOURCE872a131a0005c8f57b0a69c981526c15.png)

都什么年代了，还用sqlyog，大伙们现在都用navicat了

