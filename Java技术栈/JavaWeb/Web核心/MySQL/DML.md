学习完了上一节DDL的内容之后，这一节我们来学习DML的内容。DML主要的作用是对于数据库中的表数据进行增删改。具体请看代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE260d28a68d4d6394b61a88db778ca43d.png)

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

- 修改和删除表数据

这里我们要搞清楚一点，那就是我们修改表中的数据是要加上一个WHERE的判断条件的，否则，我们的所有的数据都会被修改，这是非常恐怖的事情。同理删除也是

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa9d9a0ea066fadfe048df84fda18146a.png)

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

