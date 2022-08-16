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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE14e88070ff57b71bf39871bbc070323f.png)

SELECT关键字用于选择查询的字段列表，所谓的字段列表简单理解就是我们的变量名。然后FROM选择的是我们的表明列表，这个懂得都懂。WHERE则是用于写入我们用于筛选的条件的。而GROUP BY则是用于分组查询的，而在分组之后，我们如果想要过滤动作，那么就不能用WHERE而是用HAVING，ORDER BY则是排序的方法，后面跟随排序的字段和排序的规则。最后LIMIT用于分页，后面跟随一个分页的参数。

注意啊，我们上面的查询的场景都是说查询表的数据啊，别想到查询表本身去了。

- 查询全部

本节我们就来学习DQL的表数据查询的语法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb8b6316b1a41001b8c015e78b0dbc936.png)

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

- 条件查询

接着我们来学习条件查询，这是最符合我们生活场景的一种查询，同时也是重点。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE42b502ba5dc00ac4994272ae0ed69270.png)

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

- 聚合函数查询

所谓聚合函数，简单来说就是可以将一列数据进行纵向计算。其可以统计数量，求和求平均值求最大最小值。其语法和前面的条件查询的差不多，其中where的筛选条件可加可不加

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe3e8769889a7a1f61c418e21010b8955.png)

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

- 排序查询

然后我们来学习排序查询，排序查询的语法是SELECT 列名列表 FROM 表明 [WHERE 条件] ORDER BY 列名 排序方式，列名，排序方式...；

这里ASC表示升序，DESC表示降序，注意如果有多个排序条件，那么只有当第一个条件一样的时候才会去判断第二个条件，同时升序也是默认的排序条件

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6083dead1242a0e6d096d2e3ee549e03.png)

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

- 分组查询

分组查询的语法是SELECT 列名列表 FROM 表明 GROUP BY 分组列名，这里我们可以先添加WHERE条件进行过滤，只对某一些数据进行分组，分组之后如果还想再过滤的话就要使用HAVING，如果想要排序的话那就继续使用ORDER BY语法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1bc2a79fb4236c4fb18d1fdb91a1d1e0.png)

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

- 分页查询

我们来学习本章节的最后一个内容，分页查询

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe1ffb12c7422d59ada10d30620e05be2.png)

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

