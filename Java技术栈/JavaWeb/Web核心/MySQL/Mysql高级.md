# 存储过程与函数

到了这里就是MySQL高级的内容了，其实只有增删改查是初级也是最重要的内容。后面学习的都属于暂时的了解性质的，以后再掌握，现在并不着急。其实主要是因为面试官会问，那我们现在先学习下也无妨。本章我们学习存储过程和存储函数

## 存储过程和函数的介绍

其实，简单来说，存储过程和函数，其实就类似于java中的方法，其是事先经过编译并存储在数据库中的一段SQL语句的集合。存储过程和函数存在的好处有三个，一是能提高代码的复用性、二是能减少数据库和应用服务器之间的传输，提高效率、三是减少代码层面的业务处理。因为简单的代码在数据库层面就已经被处理了。我们的存储过程和函数不但能在里面定义方法，甚至可以在里面定义变量、循环等。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfb076801af2e0a12aaa5ba2d1570c2b3.png)

最后我们要来说下存储过程和函数的区别，其实就是存储函数必须有返回值，而存储过程可以没有。

在我们学习之前，我们要做一些数据准备，请看用于数据准备的代码

```
-- 创建db6数据库
        CREATE DATABASE db6;

        -- 使用db6数据库
        USE db6;

        -- 创建学生表
        CREATE TABLE student(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 学生id
        NAME VARCHAR(20),        -- 学生姓名
        age INT,            -- 学生年龄
        gender VARCHAR(5),       -- 学生性别
        score INT                               -- 学生成绩
        );
        -- 添加数据
        INSERT INTO student VALUES (NULL,'张三',23,'男',95),(NULL,'李四',24,'男',98),
        (NULL,'王五',25,'女',100),(NULL,'赵六',26,'女',90);




        -- 按照性别进行分组，查询每组学生的总成绩。按照总成绩的升序排序
        SELECT gender,SUM(score) getSum FROM student GROUP BY gender ORDER BY getSum ASC;
```

## 创建和调用存储过程

创建存储过程要使用到DELIMITER关键字，这个关键字的作用是定义一个结束符号，这个结束符号可以用于让我们的程序知道我们的语句到底执行到哪里结束。由于是创建，自然要用到CREATE，存储过程的关键字是PROCEDURE，与java类似，我们可以自己定义调用这个方法是否需要传入参数

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd3fe95cd3c07fcd0f47f610caa7a4d94.png)

调用函数的方式很简单，CALL 存储过程名称(实际参数)

请看示例代码

```
/*
   创建存储过程

   -- 修改分隔符为$
   DELIMITER $

   -- 标准语法
   CREATE PROCEDURE 存储过程名称(参数列表)
   BEGIN
      SQL 语句列表;
   END$

   -- 修改分隔符为分号
   DELIMITER ;
*/
-- 创建stu_group()存储过程，封装 分组查询总成绩，并按照总成绩升序排序的功能
DELIMITER $

CREATE PROCEDURE stu_group()
BEGIN
        SELECT gender,SUM(score) getSum FROM student GROUP BY gender ORDER BY getSum ASC;
END$

DELIMITER ;

/*
   调用存储过程
   CALL 存储过程名称(实际参数);
*/
        -- 调用stu_group()存储过程
        CALL stu_group();
```

我们这里生成存储过程的过程是将前面的按照性别将学生分组并计算总和的成绩拿出来生成一个存储过程，之后我们调用这个存储过程。生成的存储过程可以再SQLyog左侧中看到

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb786a49eaa5305e2eab292fc9f4d31e4.png)

## 查看和删除存储过程

学会了创建存储过程之后，自然地，我们接下来就要学习如何查看和删除存储过程。查看数据库的语法是SELECT * FROM mysql.proc WHERE db='数据库名称'，其中proc是PROCEDURE的缩写，代表存储过程。删除过程就不提了，自己看吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE04ef2083a7f0ff0e4fa2c7e9505ecea5.png)

请看示例代码

```
/*
   查询数据库中所有的存储过程
   SELECT * FROM mysql.proc WHERE db='数据库名称';
*/
-- 查看db6数据库中所有的存储过程
        SELECT * FROM mysql.proc WHERE db='db6';



/*
   删除存储过程
   DROP PROCEDURE [IF EXISTS] 存储过程名称;
*/
        DROP PROCEDURE IF EXISTS stu_group;
```

我们运行查看的代码，可以看到其下会显示以下内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE861ee916e6e5f371bacc2b80811e9f79.png)

这和我们查看数据库的情况是十分相同的，其实，我们查看存储过程，也就很相当于是查看我们的数据表或者是数据库。

## 变量的使用

关于变量的使用无非也就是变量的定义和赋值，在存储过程中定义变量要使用DECLARE关键字，后面接变量名和数据类型，赋予的默认值可以自己指定，当然，也可以不指定。赋值方式最简单的赋值方式是使用SET

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE73422930b28656468451d057781fa790.png)

第二种赋值方式最值得说道，第二种赋值方式是SELECT 列名 INTO 变量名 FROM 表名 [WHERE条件]，这里INTO执行的是赋值动作，其会将SELECT后面得到的结果赋值到我们的变量上。用这种方式虽然长了点，但是好处在于其可以动态获取我们某些表的数据并进行赋值

示例代码如下

```
/*
   定义变量
   DECLARE 变量名 数据类型 [DEFAULT 默认值];
*/
-- 定义一个int类型变量，并赋默认值为10
        DELIMITER $

        CREATE PROCEDURE pro_test1()
        BEGIN
        -- 定义变量
        DECLARE num INT DEFAULT 10;
        -- 使用变量
        SELECT num;
        END$

        DELIMITER ;

        -- 调用pro_test1存储过程
        CALL pro_test1();



/*
   变量赋值-方式一
   SET 变量名 = 变量值;
*/
        -- 定义一个varchar类型变量并赋值
        DELIMITER $

        CREATE PROCEDURE pro_test2()
        BEGIN
        -- 定义变量
        DECLARE NAME VARCHAR(10);
        -- 为变量赋值
        SET NAME = '存储过程';
        -- 使用变量
        SELECT NAME;
        END$

        DELIMITER ;

        -- 调用pro_test2存储过程
        CALL pro_test2();




/*
   变量赋值-方式二
   SELECT 列名 INTO 变量名 FROM 表名 [WHERE 条件];
*/
        -- 定义两个int变量，用于存储男女同学的总分数
        DELIMITER $

        CREATE PROCEDURE pro_test3()
        BEGIN
        -- 定义两个变量
        DECLARE men,women INT;
        -- 查询男同学的总分数，为men赋值
        SELECT SUM(score) INTO men FROM student WHERE gender='男';
        -- 查询女同学的总分数，为women赋值
        SELECT SUM(score) INTO women FROM student WHERE gender='女';
        -- 使用变量
        SELECT men,women;
        END$

        DELIMITER ;

        -- 调用pro_test3存储过程
        CALL pro_test3();
```

## if语句的使用

在存储过程中我们同样也可以定义if语句，这个用法其实和Linux的非常像，都是有THEN的，具体的使用规则请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf7e37de21322304bbe35928827b3b8e3.png)

我们可以写入其示例代码如下

```
/*
   if语句
   IF 判断条件1 THEN 执行的sql语句1;
   [ELSEIF 判断条件2 THEN 执行的sql语句2;]
   ...
   [ELSE 执行的sql语句n;]
   END IF;
*/

/*
   定义一个int变量，用于存储班级总成绩
   定义一个varchar变量，用于存储分数描述
   根据总成绩判断：
      380分及以上   学习优秀
      320 ~ 380     学习不错
      320以下       学习一般
*/
DELIMITER $

        CREATE PROCEDURE pro_test4()
        BEGIN
        -- 定义变量
        DECLARE total INT;
        DECLARE info VARCHAR(10);
        -- 查询总成绩，为total赋值
        SELECT SUM(score) INTO total FROM student;
        -- 对总成绩判断
        IF total > 380 THEN
        SET info = '学习优秀';
        ELSEIF total >= 320 AND total <= 380 THEN
        SET info = '学习不错';
        ELSE
        SET info = '学习一般';
        END IF;
        -- 查询总成绩和描述信息
        SELECT total,info;
        END$

        DELIMITER ;




        -- 调用pro_test4存储过程
        CALL pro_test4();
```

这里我们是先获得总成绩，然后利用if语句对总成绩进行一个判断，并执行判断中对应的动作。

## 参数传递的使用

在MySQL中，参数传递有三种，一种是IN，其代表输入参数，需要有调用者传入实际的数据，同时也这是默认的参数传递类型，也就是说，如果前面没有指定任何的参数传递类型，那么默认其就是输入参数，需要调用者传入。而OUT代表输入参数，该参数可以作为返回值。而INOUT既可以代表输入参数，也可以作为输出参数

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa5d87d454aff9bed7891fcd332d8f5af.png)

我们这里主要演示IN和OUT，请看示例代码

```
/*
   参数传递
   CREATE PROCEDURE 存储过程名称([IN|OUT|INOUT] 参数名 数据类型)
   BEGIN
      SQL 语句列表;
   END$
*/
/*
   输入总成绩变量，代表学生总成绩
   输出分数描述变量，代表学生总成绩的描述信息
   根据总成绩判断：
      380分及以上  学习优秀
      320 ~ 380    学习不错
      320以下      学习一般
*/
DELIMITER $

        CREATE PROCEDURE pro_test5(IN total INT,OUT info VARCHAR(10))
        BEGIN
        -- 对总成绩判断
        IF total > 380 THEN
        SET info = '学习优秀';
        ELSEIF total >= 320 AND total <= 380 THEN
        SET info = '学习不错';
        ELSE
        SET info = '学习一般';
        END IF;
        END$

        DELIMITER ;

        -- 调用pro_test5存储过程
        CALL pro_test5(350,@info);

        CALL pro_test5((SELECT SUM(score) FROM student),@info);

        SELECT @info;
```

可以看到我们输入参数的传入可以直接传入350（在第35行），而对于输出参数的传递，我们这里是写入了@info，写入这个语句就意味着我们在当前会话中定义了一个info的变量，这个变量会在将info的值获取到，我们可以再后续的代码中通过到这个变量的值，这个名字是无所谓的，我们这里填入info只是为了便于人类读者的理解而已，并不是说一定要填入和我们内部定义的变量名一样的名字，获得该变量的值是在我们18行里的命令完成的，与我们在会话中定义的变量名没有关系

还有一点就是如果我们想要动态获取最大值的话，我们可以在传入参数里传入一个查询结果，查询到其最大值再传入，具体我们可以看第35行代码

## while循环的使用

直接看图吧，没啥好说的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE78c6acae0493979ddd98e3c1a0d111af.png)

示例代码如下，这里的目的是要求1-100的偶数和

```
/*
   while循环
   初始化语句;
   WHILE 条件判断语句 DO
      循环体语句;
      条件控制语句;
   END WHILE;
*/
-- 计算1~100之间的偶数和
        DELIMITER $

        CREATE PROCEDURE pro_test6()
        BEGIN
        -- 定义求和变量
        DECLARE result INT DEFAULT 0;
        -- 定义初始化变量
        DECLARE num INT DEFAULT 1;
        -- while循环
        WHILE num <= 100 DO
        IF num % 2 = 0 THEN
        SET result = result + num;
        END IF;

        SET num = num + 1;
        END WHILE;

        -- 查询求和结果
        SELECT result;
        END$

        DELIMITER ;


        -- 调用pro_test6存储过程
        CALL pro_test6();
```

## 存储函数的使用

那么我们终于到了我们这一章的最后一个内容了，就是关于存储函数的使用。先来看看相关的知识点

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE03a7c708589c78cf186487d46ec34cbf.png)

存储函数跟存储过程也差不多，关键知识也是创建、调用和删除，其中创建的时候，其存储函数的关键字是FUNCTION，且存储函数必须有返回值，且在创建开头就要先指定返回值的类型，在BEGIN和END中间要有RETURN来返回对应的结果。

其调用存储函数的方式直接就是SELECT 函数名称(实际参数)，我认为其实这个所谓的调用SELECT其实就是获取其返回值然后将其展示出来，以前我们使用的是Call，展示是需要在函数内部自己定义的，而我们这里直接就可以将展示定义在外面了

另外必须有返回值的意思是其会返回一个值，不是说我们一定要传入什么值，这是两回事。

请看示例代码

```
/*
   创建存储函数
   CREATE FUNCTION 函数名称([参数 数据类型])
   RETURNS 返回值类型
   BEGIN
      执行的sql语句;
      RETURN 结果;
   END$
*/
-- 定义存储函数，获取学生表中成绩大于95分的学生数量
        DELIMITER $

        CREATE FUNCTION fun_test1()
        RETURNS INT
        BEGIN
        -- 定义变量
        DECLARE s_count INT;
        -- 查询成绩大于95分的数量，为s_count赋值
        SELECT COUNT(*) INTO s_count FROM student WHERE score > 95;
        -- 返回统计结果
        RETURN s_count;
        END$

        DELIMITER ;


/*
   调用函数
   SELECT 函数名称(实际参数);
*/
        -- 调用函数
        SELECT fun_test1();


/*
   删除函数
   DROP FUNCTION 函数名称;
*/
        -- 删除函数
        DROP FUNCTION fun_test1;
```

存储函数保存在函数文件夹中，如图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE98e41284fbb8d0db9eb7c243a12ae92a.png)

# 触发器

这个知识当初居然忘了学，现在我又没啥重新学的心情了，这里先记录一下，后续有时间再回来补充这个笔记

# 事务

现在这一节我们来学习事务，事务是在了解内容里比较重要的一节，我们就当他是正常要学习的内容来学习就可以了

## 事务的介绍

所谓事务就是由一条或多条SQL语句组成的一个执行单元，其特点是这个单元的内容要么同时成功，要么同时失败。其执行时一旦有一条语句执行失败，那么整个单元就会撤回到事务最初的状态，如果所有的语句都执行成功了，事务才会顺利执行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd7b3f4f1c2e3ce5e1f9ef7291687059a.png)

在我们正式学习之前，我们还要做一些基本的数据准备，其代码如下

```
-- 创建db8数据库
        CREATE DATABASE db8;

        -- 使用db8数据库
        USE db8;

        -- 创建账户表
        CREATE TABLE account(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 账户id
        NAME VARCHAR(20),        -- 账户名称
        money DOUBLE            -- 账户余额
        );
        -- 添加数据
        INSERT INTO account VALUES (NULL,'张三',1000),(NULL,'李四',1000);
```

## 事务的基本使用

事务的基本操作分三种，一种是开启事务，其关键字是START TRANSACTION，另一种是回滚事务，也就是当我们的事务执行中出现问题时我们需要执行的回滚操作，关键字是ROLLBACK，最后是提交事务，也就是当我们的事务成功执行之后，我们让我们的数据变成执行之后的状态，其关键字时COMMIT

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3adae5e16872d3fbdd068bfba1a3ff57.png)

我们可以写入示例代码如下

```
-- 张三给李四转账500元

        -- 开启事务
        START TRANSACTION;

        -- 1.张三账户-500
        UPDATE account SET money=money-500 WHERE NAME='张三';

        出错了...

        -- 2.李四账户+500
        UPDATE account SET money=money+500 WHERE NAME='李四';

        -- 回滚事务
        ROLLBACK;

        -- 提交事务
        COMMIT;
```

我们要使用事务，我们就要先开启事务（第4行），然后我们执行相应的语句，如果中间出错了，我们可以执行第15行的回滚事务，让我们的数据回滚到最初我们执行开始事务时的状态，如果我们确定我们的语句执行成功了，那么我们最后执行提交事务，也就是第18行，执行完毕就可以让我们的数据真正发生改变了

## 事务的提交方式

事务的提交方式有两种，一种是自动提交，一种是手动提交。其中前者是我们的MySQL默认的提交方式。我们可以通过语句SELECT @@AUTOCOMMIT;来查看我们事务的提交方式，这里@@就相当于是在查看全局变量。查看到的结果会是数字，1代表自动提交，0代表手动提交。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9c0b67d7d327744b0559206521097e8a.png)

我们同样可以修改我们事务的提交方式，如果我们将事务的提交方式修改为手动提交，那么我们对任何数据的改动都会变成临时的，如果想要令其持久化到我们的数据中，要调用COMMIT;命令才可以实现。我们来看看其示例代码

```
/*
   查询事务提交方式：SELECT @@AUTOCOMMIT;  -- 1代表自动提交    0代表手动提交
   修改事务提交方式：SET @@AUTOCOMMIT=数字;
*/
-- 查询事务的提交方式
        SELECT @@autocommit;

UPDATE account SET money=2000 WHERE id=1;

        COMMIT;


        -- 修改事务的提交方式
        SET @@autocommit = 1;

```

## 事务的四大特征

这是面试中的经典问题，就是事务有什么特征，我们要知道，事务有四大特征。分别是原子性、一致性、隔离性、持久性。这四个性质可以简称为ACID，其中隔离性最为难，其中还可以设置不同的隔离级别，每个不同的隔离级别可能又会引发不同的问题，反正就特别麻烦。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE52faabcc739a2027d0f4724eba8d790f.png)

原子性（Atomicity）指的是事务包含的操作要么全部成功，要么就全部失败，回滚到执行事务之前的状态。其所带来的的结果是，事务的操作如果成功就必须完全应用到数据库，如果失败则不能对数据库本身产生任何影响。

一致性指的是事务必须使数据库从一个一致性状态变换到另外地一个一致性状态，简而言之就是事务执行之前和之后我们的数据库都必须处于一致性状态。那什么是一致性状态呢，其实我也不是很整得明白，反正先记住吧

隔离性指的是当多个用户并发访问数据表，比如说操作同一张表的时候，数据库为每一个用户开启的事务，不能被其他事务的操作干扰，多个并发事务要互相隔离，有点像线程安全的意思

持久性指的是一个事务一旦被提交，那么其对数据库中的数据改变是永久性的，即使数据库遇到了故障，也不会停止或者是丢失提交事务的操作。

## 事务的隔离级别

我们上一节说过，当多个客户端操作时，各个客户端的事务应该是隔离的，相互独立的，不受影响的。而如果多个事务操作同一批数据，就会产生各种不同的问题，我们需要设置不同的隔离级别来解决这些问题。隔离级别一共有四类，分别是read uncommitted（读未提交）、read committed（读已提交）、repeatable read（可重复读）、serializable（串行化）。具体的对应内容都写到下图中了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb0ee5c4ab4e537015399a90047eca79d.png)

我们接着来了解下不同隔离级别引发的不同问题的内容，所谓脏读指的是在一个事务处理时读到了另一个未提交事务中的数据。而不可重复读指的是一个事务处理过程中读到了另一个事物中修改并以提交的数据。而幻读分为两种，一种是指查询某数据不存在，而执行插入时却发现该记录已经存在，无法插入。另一种是查询数据时不存在，然后执行删除操作，却出现了删除成功的结果。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbdfc5af363899ef9bd6cb7807d9ab3ca.png)

接着我们来学习如何修改事务的隔离级别，这里要注意的一点是，一旦我们修改事务的隔离级别，我们要重启我们的SQLyog之后才会生效

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0a651f743e20e72aa496cb62c764d9c2.png)

示例代码如下：

```
/*
   查询隔离级别：SELECT @@TX_ISOLATION;
   修改隔离级别：SET GLOBAL TRANSACTION ISOLATION LEVEL 级别字符串;
*/
-- 查询事务隔离级别
        SELECT @@tx_isolation;

-- 修改事务隔离级别(修改后需要重新连接)
        SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

### 脏读问题的演示和解决

接下来我们来演示下脏读的问题，我们先来回顾下脏读的定义，脏读指的是一个事务中读取到了其他事务未提交的数据

这里为了模拟两个客户端的操作，我们需要打开两个SQLyog窗口，并分别写入代码。

第一个窗口的代码如下

```
/*
   脏读的问题演示和解决
   脏读：一个事务中读取到了其他事务未提交的数据
*/
-- 设置事务隔离级别为read uncommitted
        SET GLOBAL TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
        SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;

        -- 开启事务
        START TRANSACTION;

        -- 转账
        UPDATE account SET money = money-500 WHERE NAME='张三';
        UPDATE account SET money = money+500 WHERE NAME='李四';

        -- 查询account表
        SELECT * FROM account;

        -- 回滚
        ROLLBACK;

        -- 提交事务
        COMMIT;
```

第二个窗口的代码如下

```
-- 查询事务隔离级别
        SELECT @@tx_isolation;

-- 开启事务
        START TRANSACTION;

        -- 查询account表
        SELECT * FROM account;

        -- 提交事务
        COMMIT;
```

这里我们就来说下脏读的问题，比方说，我们窗口一里创建了事务并执行了转账操作，但是我们的数据还没有提交，此时第二个窗口同样查询表，会发现我们的数据就是修改之后的数据，此时就发生了脏读现象。因为虽然得到的结果是一致的，但是因为窗口1还没有提交，如果窗口一因为各种情况发生了回滚，那么此时窗口2就会获得最初的数据，此时会脏读现象会提供给用户错误的信息。

解决脏读的方法也很简单，我们只要将我们的隔离方式下降一级就完了，此时只要窗口一的事务没有提交，窗口二查询的内容就还是没提交前的内容。

### 不可重复读的问题的演示和解决

我们同样需要两个窗口来演示我们的不可重复读的问题，我们同样来复习下不可重复读的定义，不可重复读指一个事务中读取到了其他事务已提交的数据

第一个窗口的代码如下

```
/*
   不可重复读的问题演示和解决
   不可重复读：一个事务中读取到了其他事务已提交的数据
*/
-- 设置事务隔离级别为read committed
        SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;
        SET GLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;

        -- 开启事务
        START TRANSACTION;

        -- 转账
        UPDATE account SET money = money-500 WHERE NAME='张三';
        UPDATE account SET money = money+500 WHERE NAME='李四';

        -- 查询account表
        SELECT * FROM account;

        -- 提交事务
        COMMIT;
```

第二个窗口的代码如下

```
-- 查询隔离级别
        SELECT @@tx_isolation;

-- 开启事务
        START TRANSACTION;

        -- 查询account表
        SELECT * FROM account;

        -- 提交事务
        COMMIT;
```

其实，像我们上面，第一个窗口发生了转账并提交结束了事务之后，再第二个窗口里查询到的第一个窗口里已经修改的数据的时候，这时我们的不可重复读的问题就已经发生了。虽然说这个情况似乎是十分合乎逻辑的，但实际情况里其是有问题的。可能会出现上午A查询和下午B查询的结果不同的情况发生（因为上午和下午发生了数据的修改），这种情况有时候是不为我们所允许的。因此此时我们要解决这问题，解决这个问题的方式也很简单，就是将其隔离级别再下放一级。下放一级之后，我们即使在另一个事务提交之后，我们也查询不到修改之后的数据（但是查询动作可以执行），只有当两个事务都结束之后，我们才能够查询到对应的数据。

### 幻读问题的演示和解决

我们照例先来复习一下幻读的定义，幻读存在两种情况，如下所示

   查询某记录是否存在，不存在

   准备插入此记录，但执行插入时发现此记录已存在，无法插入

   或某记录不存在执行删除，却发现删除成功

同样我们这里需要两个窗口，先来看看窗口1的代码

```
/*
   幻读的问题演示和解决
   查询某记录是否存在，不存在
   准备插入此记录，但执行插入时发现此记录已存在，无法插入
   或某记录不存在执行删除，却发现删除成功
*/
-- 设置隔离级别为repeatable read
        SET GLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;
        SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;

        -- 开启事务
        START TRANSACTION;

        -- 添加记录
        INSERT INTO account VALUES (3,'王五',2000);
        INSERT INTO account VALUES (4,'赵六',3000);

        -- 查询account表
        SELECT * FROM account;

        -- 提交事务
        COMMIT;
```

接着来看看窗口2的代码

```
-- 查询隔离级别
        SELECT @@tx_isolation;

-- 开启事务
        START TRANSACTION;

        -- 查询account表
        SELECT * FROM account;

        -- 添加
        INSERT INTO account VALUES (3,'王五',2000);

        -- 提交事务
        COMMIT;
```

假设我们在窗口一中尝试插入一个新用户，但是我们不提交，然后我们在窗口2中查询，会查询到我们插入之前的数据情况，然后我们在窗口2中也执行插入，此时窗口2会一直卡顿，因为窗口1没提交，窗口2是无法执行的，然后窗口1提交，此时数据中就突然有了第三个数据了，此时窗口2会显示插入失败。这里发生的问题就是幻读，窗口2查询到的结果是没有该用户，插入应该是成功的，但实际上却是失败的。而对于另一个情况，我们也能很简单的猜测到，窗口2中执行删除，自然会一直等待，然后窗口1中提交，数据中就有了相应的数据，此时插入成功。

解决的方式同样是将隔离下放一级，当然，我们这里说的下放一级其实指的是图片上的往下看的意思，其实，最下面的那个权限是最高的，所以它发生的问题也最少，这个对前面的相同说法也是适用的。当我们将隔离权限提升至最高时，此时当窗口1没有提交时，窗口2执行查询都会直接卡住，除非窗口1提交事务完毕，否则就一直卡，这就相当于是直接把整个事务给锁住了，没有执行完就不许其他事务执行任何动作。

其安全性最高，同时效率也最差。

### 事务隔离级别的小结

最后我们的MySQL数据库里的默认隔离级别是repeatable read，不同的数据库或许有不同的隔离默认隔离级别

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3338f4238ecc18849f1fe70253482265.png)

注意，隔离级别从小到大安全性越来越高，效率越来越低，我们是不建议修改数据库默认的隔离级别的。至于脏读幻读和不可重复读的问题，我们后面会学习其他解决的方法，不要以为解决这些问题就只是嗯换隔离级别

# 存储引擎

这一章我们来学习存储引擎，我们先来进行一个存储引擎的介绍

## 存储引擎的介绍

但是在介绍存储引擎之前，我们先要去学习MySQL的体系结构，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb2846eef2e69cf4be969b100d398c26d.png)

在我们的MySQL最前面的是我们的客户端连接的层面，客户端连接里我们可以看到哪些是我们支持的客户端语言，非常多啊，JDBC其实就是我们的java语言，然后其他语言也支持。过了这个客户端连接之后，我们就到了我们的网络连接层

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEae2cea0d173f38bb77d3ec69bd6d3b32.png)

网络连接层是一个连接池，我们可以将其看成我们以前学习过的线程池。当我们的客户端发起一个连接，就会从网络连接层中获取一个连接来使用，并且在使用完毕之后将这个连接归还到连接池中。获取连接之后就到了我们的核心服务区

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc568fc2da53248329c2393320d0c8918.png)

最左侧的Enterprise Management代表的是我们的系统管理和操作管理的相关内容，内含管理服务和工具，比如备份和恢复。然后SQL Interface代表的是SQL接口，这里可以接受对应的sql语句并返回查询的结果。然后是Parser查询解析器，在此处会进行sql语法结构的正确性验证以及解析sql语句。然后是Optimizer查询优化器，该优化器会帮我们优化我们的SQL语句（在查询操作之前），最后是Caches & Buffers，其代表一个缓存，如果缓存内有数据，就直接从缓存获取，如果没有，那再去数据库中查询。

过了第二层之后，我们就到了第三层，存储引擎层

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEee119bfd99bd9a5e1971aa948d8be5f9.png)

在MySQL中的存储引擎是插件式的，也就是说一个数据库可以安装多个引擎，这样就可以根据不同的场景来使用不同的存储引擎。接着就到了最后一层，系统文件层

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE42eafdda8b79ebfc5bc1126260ec8ac0.png)

在系统文件层中，会保存一些数据文件，配置文件，以及日志文件。上面这些就是MySQL的基础结构，

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEade2650df805e7e22ddfd36a55bd9d75.png)

了解了这些基础结构之后，接下来我们就来重点学习下我们的引擎，也就是第三层的部分，首先什么是引擎呢？

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE85345999c6869b6b892dc9d0355aa9f4.png)

引擎在日常生活中就是整个机器运行的核心，不同的引擎具备不同的功能，同时也应用在 不同的场景之中。而在MySQL中，也是一样的。mysql中不同的引擎具备不同的功能，同时应用于不同的场景之中

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3f670b803dd429708984053abded7113.png)

MySQL中最常用的引擎就是InnoDB，其也是5.5版本后MySQL默认的存储引擎。后面的两个做了解就可以了，这三者的详细对比如图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd75c7b60d2bc3a9b71001739c357ccd7.png)

## 存储引擎的操作

关于存储引擎的操作，无非也就是查询和设置，当然没有删除，因为我们不能把我们的引擎给删了。具体语法内容在下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcbe6cf3887ba5315b01d06569d1e4ba5.png)

示例代码如下

```
/*
   查询数据库支持的存储引擎
   SHOW ENGINES;
*/
-- 查询数据库支持的存储引擎
        SHOW ENGINES;



/*
   查询某个数据库中所有数据表的存储引擎
   SHOW TABLE STATUS FROM 数据库名称;
*/
        -- 查询db4数据库所有表的存储引擎
        SHOW TABLE STATUS FROM db4;



/*
   查询某个数据库中某个表的存储引擎
   SHOW TABLE STATUS FROM 数据库名称 WHERE NAME = '数据表名称';
*/
        -- 查看db4数据库中category表的存储引擎
        SHOW TABLE STATUS FROM db4 WHERE NAME = 'category';



/*
   创建数据表指定存储引擎
   CREATE TABLE 表名(
         列名,数据类型,
         ...
   )ENGINE = 引擎名称;
*/
        CREATE TABLE engine_test(
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(10)
        )ENGINE = MYISAM;

        SHOW TABLE STATUS FROM db4;

/*
   修改数据表的存储引擎
   ALTER TABLE 表名 ENGINE = 引擎名称;
*/
        -- 修改engine_test表的存储引擎为InnoDB
        ALTER TABLE engine_test ENGINE = INNODB;

```

## 存储引擎的选择

这个是适记性的知识，我们直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE886e61f75d7e052403496efa4137455b.png)

# 索引

## 索引的介绍

首先介绍下什么是索引，索引本身是存储于数组或者是集合中的用于快速寻找某个数据的数据，在MySQL中，其实帮助MySQL高效获取数据的一种数据结构。

在表数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式指向数据，这样就可以在这些数据结构之上实现某些高级查看的算法，这种数据结构就是索引

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5aca3cc9f9434f9d9877d76bcd7c8fe5.png)

我们在上图中可以看到当没有索引时，我们找到5的话，就需要持续对比五次，而有了索引的话，用类似于二叉树的结构，我们只需要对比三次，效率上就有所提升，当然实际上MySQL里不一定是通过上图的方式来保存索引的，这里只是用于理解

## 索引分类

索引按照功能分类可以分为普通索引、唯一索引、主键索引、联合索引、外键索引和全文索引。按照结构分类可分为BTree索引（其底层基于B+Tree数据结构）和Hash索引。各种不同的所有的特点都在图上注明了，自己去看吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf0a03f59861fe1733b69824a4f2c5940.png)

在学习之前，我们要先进行一些数据上的准备，请看代码

```
-- 创建db9数据库
        CREATE DATABASE db9;

        -- 使用db9数据库
        USE db9;

        -- 创建student表
        CREATE TABLE student(
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(10),
        age INT,
        score INT
        );
        -- 添加数据
        INSERT INTO student VALUES (NULL,'张三',23,98),(NULL,'李四',24,95),
        (NULL,'王五',25,96),(NULL,'赵六',26,94),(NULL,'周七',27,99);
```

## 创建和查询索引

创建和查询索引的具体语法如下所示，其中UNIQUE代表唯一，写入其代表创建唯一索引，写入FULLTEXT代表创建全文索引，什么都不写代表创建普通索引

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE90565bc39f17d47294fc50c86a4f96bc.png)

最后我们还有两点需要注意，一是主键列自带主键索引，二是外键列自带外键索引，什么是外键索引？其实就是指两个表被外键约束连接起来的那两个列，一个列是主键列，一个列是外键列，外键约束在谁哪里创建，谁的指定的列就是外键列，被指向的列就是主键列，简单记忆可以理解为主键列后面写，外键列前面写。请看代码

```
/*
   创建索引
   CREATE [UNIQUE|FULLTEXT] INDEX 索引名称
   [USING 索引类型]  -- 默认是BTREE
   ON 表名(列名...);
*/
-- 为student表中的name列创建一个普通索引
        CREATE INDEX idx_name ON student(NAME);

        -- 为student表中的age列创建一个唯一索引
        CREATE UNIQUE INDEX idx_age ON student(age);


/*
   查询索引
   SHOW INDEX FROM 表名;
*/
        -- 查询student表中的索引  (主键列自带主键索引)
        SHOW INDEX FROM student;

        -- 查询db4数据库中的product表 (外键列自带外键索引)
        SHOW INDEX FROM product;
```

最后我们查看对应的列，可以看到如下的结果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE78ecabe70c3b108a24f9f166c7bad204.png)

其中Table代表表名，Non_unique代表是否唯一，0代表必须唯一，1则反之。Key_name代表的是索引名称。Comlumn_name代表我们给表的哪一列添加的索引。Index_type代表索引类型。Null代表索引是否可以为空，YES代表可以，不写代表不可以。

## 添加和删除索引

除了用CREATE关键字来创建索引之外，我们还可以用修改表结构的方式来添加索引，其语法如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0d362ad5d5ae3ab2000f0933fb480fe8.png)

上图已经按照分类写出了创建不同索引时所用的不同的语法，最后我们来看看演示代码

```
/*
   ALTER添加索引
   -- 普通索引
   ALTER TABLE 表名 ADD INDEX 索引名称(列名);

   -- 组合索引
   ALTER TABLE 表名 ADD INDEX 索引名称(列名1,列名2,...);

   -- 主键索引
   ALTER TABLE 表名 ADD PRIMARY KEY(主键列名); 

   -- 外键索引(添加外键约束，就是外键索引)
   ALTER TABLE 表名 ADD CONSTRAINT 外键名 FOREIGN KEY (本表外键列名) REFERENCES 主表名(主键列名);

   -- 唯一索引
   ALTER TABLE 表名 ADD UNIQUE 索引名称(列名);

   -- 全文索引
   ALTER TABLE 表名 ADD FULLTEXT 索引名称(列名);
*/
-- 为student表中score列添加唯一索引
        ALTER TABLE student ADD UNIQUE idx_score(score);


        -- 查询student表的索引
        SHOW INDEX FROM student;



/*
   删除索引
   DROP INDEX 索引名称 ON 表名;
*/
        -- 删除idx_score索引
        DROP INDEX idx_score ON student;
```

## 索引在磁盘存储的特点

索引主要是在存储引擎中实现的，不同的存储引擎支持的索引也不同，我们这里主要介绍InnoDB的Btree索引。BTree索引是基于B+Tree数据结构的，而B+Tree数据结构是BTree数据结构的变种。通常使用在数据库和操作系统中的文件系统，能够保持数据的稳定有序

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7502ee83ef726a00857a2450d3d2081b.png)

我们需要理解的是磁盘存储和BTree以及B+Tree的数据结构，为此我们需要先理解磁盘存储，

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE248665b2961fc27fc6e74e8d91249903.png)

系统从磁盘读取数据到内存中时是以磁盘块（block）为基本单位的。位于同一块磁盘的数据会被一次性读取出来，不是需要什么取什么。在InnoDB存储引擎中，页（Page）是其磁盘管理的最小单位，每个页的大小为16KB。该引擎将若干个磁盘块地址连接成一个页，查询数据时，如果一个页中的每条数据都能定位数据记录的位置，这将减少磁盘I/O次数，提高查询效率。

- 索引原理之BTree数据结构

我们先来讲解BTree的数据结构，一个BTree结点由id，指针，数据三个共同组成，我们看下图可以看到该树的分步方式，当数据在17与35之间时，由c3指向一下层，当比17小时由c2指向下一层，大于则由c4指向下一层，我们是利用id来比较大小的，确定到对应的id之后就可以拿到该id下对应的数据，通过这与方式来提高我们的查询效率。

然而BTree有个问题， 就是只要其查询了任一结点，其就会将该结点的所有所有数据给读取到，比如我们要查询15的数据，其首先查找磁盘块1，然后进入磁盘块2，最后定位到磁盘块7，这三个动作里，其会将三个磁盘块的内容都读取进去，准确来说，当其用第一个磁盘块的id来比较时，就已经将整个磁盘块的内容给读取了。然而实际上，我们只需要磁盘块7的数据，BTree的这种特性就会导致其效率的降低，因此B+Tree的数据结构应运而生

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe09b71b01a65cfae990aadf81de95192.png)

## B+Tree数据结构

B+Tree数据结构同样也是有id指针数据三部分组成，但是其只在叶子结点上保存数据和id，非叶子结点上只保存id和指针，这样我们的数据每次查询时就不用进行io操作了，只会在叶子结点上进行io操作，因为只有叶子结点上有数据。同时B+Tree还有一个特点，那就是其将叶子结点上的结点都连接在了一起，这样当用户需要一块数据时不需要多次查找，可以直接通过id范围返回一部分或者是自己所需要的指定数据，能有效提高效率

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcc5fc9c6d019bf5f1537168a8279cbc8.png)

最后我们可以对BTree和B+Tree数据结构做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3fd78c1a9c85166e2fd69c0383f71916.png)

## 索引的设计原则

索引的设计也是需要遵守一定的原则的，不可以随便创建。首先我们应该要对查询频次较高，且数据量比较大的表建立索引，因为不怎么查询或者数据量很小的表没有创建索引的比较。其次是我们建议使用唯一索引，因为索引的区分度越高，其使用效率就越高，而且唯一索引的区分度显然是最高的。最后是索引字段的选择，其最佳候选列应该从where的子句的条件中去提取，因为索引本来就是用于快速查询的，从where子句中去提取，能够确保我们的索引总是建立在需要被查询的列上。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE36b8f3f9073687b782438e98c0fe9176.png)

当然，索引虽然很好，但并不是越多越好，因为维护索引也是需要占用我们的效率的，所以索引的创建要适当，而不是越多越好。

最后我们要学习最左匹配原则，其只适用于组合索引。加入我们给user表中的三个属性添加组合索引，那么实际上其实是建立了三个索引的不同组合的索引，而我们使用哪个组合的查询语句都可以命中语句，而且我们的索引出现的字段是可以是任意的，也就是说，即使我们创建索引时是name属性最先创建，我们仍然可以将其他属性放在前面，因为MySQL优化器会帮我们自动调整其条件顺序。然后最后的一点是，如果我们的创建的不同组合索引中最左边的列不在我们的查询条件中，那么就不会命中索引。比如在下图中明明我们的name在最左边，而最后的查询语句根本就没有name，那肯定是查不到的，因为其组合索引中最左边的列不再查询条件中。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE67628e94b52ef17bd26e714110b5db31.png)

# 锁

本章节我们来学习锁的内容，关于这一章节的内容，我们可以结合我们以前在JAVASE里学习过的多线程同步的知识来学习。首先我们来进行锁的介绍

## 锁的介绍

首先我们要知道什么锁，锁是数据库为了保证数据的一致性而设计的一种规则，其类似于多线程的中的线程同步，可以保证数据的一致性和安全性。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9b6dc82e2e8761c963eba0cc0034a98d.png)

锁按操作分类可以分类共享锁和排他锁，共享锁也叫读锁，而排他锁也叫写锁，前者可以多个锁共存和多个线程查询数据但不可修改数据，后者不可共存且只允许当前线程查询和修改数据。

按力度分类可以分为表级锁和行级锁，前者会锁定整个表，具有加锁快，开销小，力度大的特点，但是其容易发生锁冲突，而且并发度底，不过其不会出现死锁情况。而后者会锁定当前行，其具有开销大，加锁慢，力度小的缺点，但是其发生锁冲突的概率低，并发度还高，不过其最大的问题是会出现死锁的情况

我们还可以按使用方式进行分类，将锁分为悲观锁和乐观锁，前者是认定每次查询数据时都认为别人会修改，所以每次查询都会加锁，后者是认为每次查询时别人都不会修改，只会在更新时判断此期间别有没有去更新该数据

不同引擎所支持的锁也是不同的，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf1af3494c9d2a1702448818978f89f9c.png)

## InnoDB共享锁

那么我们先来介绍共享锁，先来看看共享锁的语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9572c84e610c8801bc5e899ec80618f5.png)

在学习内容之前，我们要先做一些对应的数据准备，其代码如下

```
-- 创建db10数据库
        CREATE DATABASE db10;

        -- 使用db10数据库
        USE db10;

        -- 创建student表
        CREATE TABLE student(
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(10),
        age INT,
        score INT
        );

        -- 添加数据
        INSERT INTO student VALUES (NULL,'张三',23,99),(NULL,'李四',24,95),
        (NULL,'王五',25,98),(NULL,'赵六',26,97);
```

我们准备的数据是一个db10的数据库，并在其下创建了一个student表，其中id是主键列，其他属性则没有添加任何约束，然后我们添加了对应的数据。接着我们就可以来正式开始学习了，我们要创建两个窗口并写入其对应的代码，窗口1的代码如下

```
/*
   共享锁：数据可以被多个事务查询，但是不能修改
   创建锁的格式
      SELECT语句 LOCK IN SHARE MODE;
*/
-- 开启事务
        START TRANSACTION;

        -- 查询id为1数据，并加入共享锁
        SELECT * FROM student WHERE id=1 LOCK IN SHARE MODE;

        -- 查询分数为99的数据，并加入共享锁
        SELECT * FROM student WHERE score=99 LOCK IN SHARE MODE;

        -- 提交事务
        COMMIT;
```

窗口2的代码如下

```
-- 开启事务
        START TRANSACTION;

        -- 查询id为1数据,(普通查询没问题)
        SELECT * FROM student WHERE id=1;

        -- 查询id为1数据,也加入共享锁(共享锁和共享锁是兼容)
        SELECT * FROM student WHERE id=1 LOCK IN SHARE MODE;

        -- 修改id为1数据，姓名改成张三三(修改失败。会出现锁的情况。只有窗口1提交事务后才能修改成功)
        UPDATE student SET NAME='张三三' WHERE id=1;

        -- 修改id为2数据，姓名改成李四四(修改成功，InnoDB引擎默认加的是行锁)
        UPDATE student SET NAME='李四四' WHERE id=2;

        -- 修改id为3数据，姓名改成王五五(修改失败，InnoDB引擎如果不采用带索引的列加锁，加的就是表锁)
        UPDATE student SET NAME='王五五' WHERE id=3;


        -- 提交事务
        COMMIT;
```

我们先在第一个表里开启事务，然后我们查询id为1的数据并且加入共享锁，此时我们在窗口而中也可以查询id为1的数据，同时也可以将其加入共享锁，这是没有问题的，因为共享锁之间可以共存，但是如果我们要在窗口2中修改id为1的数据，那就不行了，一定要等到窗口1提交结束之后才可以，然后我们窗口2中修改之后再次点击提交就可以完成对数据的真正修改了

但如果我们是将id为1的数据加入到共享锁中，然后修改id为2的数据，此时却是可以的，这是因为在InnoDB引擎中，如果采用带索引的列加锁，那么默认加的是表所，id是主键列，其下有主键索引。如果我们是查询分数为99的数据并加入到共享锁的话，此时默认加的就是表锁了，因为在InnoDB引擎中，如果不采用带索引的列加锁，那么加的就是表锁

## InnoDB排他锁

同样的我们需要两个窗口，先来看看窗口1的代码

```
/*
   排他锁：加锁的数据，不能被其他事务加锁查询或修改
   排他锁创建格式
      SELECT语句 FOR UPDATE;
*/
-- 开启事务
        START TRANSACTION;

        -- 查询id为1数据，并加入排他锁
        SELECT * FROM student WHERE id=1 FOR UPDATE;

        -- 提交事务
        COMMIT;
```

再来看看窗口2的代码

```
-- 开启事务
        START TRANSACTION;

        -- 查询id为1数据(普通查询没问题)
        SELECT * FROM student WHERE id=1;

        -- 查询id为1数据,并加入共享锁(排他锁和共享锁是不兼容的)
        SELECT * FROM student WHERE id=1 LOCK IN SHARE MODE;

        -- 查询id为1数据，并加入排他锁(排他锁和排他锁是不兼容的)
        SELECT * FROM student WHERE id=1 FOR UPDATE;

        -- 修改id为1数据，将姓名改成张三(修改失败，会出现锁的情况。只有窗口1提交事务后才能修改成功)
        UPDATE student SET NAME='张三' WHERE id=1;

        -- 提交事务
        COMMIT;
```

将对应的数据加入到排他锁时，是无法将其加入到其他锁的，同时也不能修改数据，但是可以读

## MyISAM锁

接着我们来学习MyISAM引擎的读锁，先来看看语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7b584fb78889ed7fddca0c26f6a2190f.png)

然后我们要做相应的数据准备，其代码如下

```
-- 创建product表
        CREATE TABLE product(
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(20),
        price INT
        )ENGINE = MYISAM;  -- 指定存储引擎为MyISAM

        -- 添加数据
        INSERT INTO product VALUES (NULL,'华为手机',4999),(NULL,'小米手机',2999),
        (NULL,'苹果',8999),(NULL,'中兴',1999);
```

注意，MyISAM引擎不能加行锁，只能加表锁。同样我们需要两个窗口，窗口1的代码

```
/*
   读锁：所有连接只能读取数据，不能修改
   加锁
      LOCK TABLE 表名 READ;

   解锁
      UNLOCK TABLES;
*/
-- 为product表添加读锁
        LOCK TABLE product READ;

        -- 查询id为1数据
        SELECT * FROM product WHERE id=1;

        -- 修改id为1数据，将金额修改4999
        UPDATE product SET price = 4999 WHERE id=1;

        -- 解锁
        UNLOCK TABLES;
```

窗口2的代码

```
-- 查询id为1数据
        SELECT * FROM product WHERE id=1;

        -- 修改id为1数据，将金额改成5999(修改失败，只有窗口1解锁后才能修改成功)
        UPDATE product SET price=5999 WHERE id=1;
```

读锁非常简单，对一个表加入读锁就完了，加入读锁之后，其他窗口和本窗口都只能做读取操作，不能坐任何的修改，其他窗口修改数据会进入无限等待，直到解锁

## MyISAM写锁

接着我们来学习写锁，同样我们先来学习写锁的语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb596ce9fdf825092465d376bae31a1d8.png)

我们同样也需要两个窗口并写入对应的代码，窗口1的代码如下

```
/*
   写锁：其他连接不能查询和修改数据
   加锁
      LOCK TABLE 表名 WRITE;

   解锁
      UNLOCK TABLES;
*/
-- 为product表添加写锁
        LOCK TABLE product WRITE;

        -- 查询
        SELECT * FROM product;

        -- 修改
        UPDATE product SET price=1999 WHERE id=2;

        -- 解锁
        UNLOCK TABLES;
```

窗口而的代码如下

```
-- 查询(查询失败，只有窗口1解锁后才能查询成功)
        SELECT * FROM product;

        -- 修改(修改失败，只有窗口1解锁后才能修改成功)
        UPDATE product SET price=2999 WHERE id=2;
```

窗口1中添加写锁之后只有窗口1可以做到修改数据和查询数据，窗口2一旦这样做就要一直等待窗口1的解锁。最后我们这里指的修改数据并不是说单纯的修改数据，增删改都是修改数据，不是说只有改属于修改数据，只是我们这里方便因此这么说而已

## 悲观锁和乐观锁

接着我们来学习最后一个内容，悲观锁和乐观锁，先来看看乐观锁和悲观锁的介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE14494f7355e8dfd95e2dc76c4ba1c91a.png)

我们之前学习的锁就是悲观锁，至于乐观锁则需要用户去手动实现，那么我们要如何实现乐观锁呢？大体有以下两种方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2d8cb925745afd200e3faa7c6238ebc3.png)

这两种方法的本质都是加标记的思想，其实这有点类似于我们JAVASE里学习的序列版本号其实，我们来演示下方式一创建乐观锁，请看代码

```
-- 创建city表
        CREATE TABLE city(
        id INT PRIMARY KEY AUTO_INCREMENT,  -- 城市id
        NAME VARCHAR(20),                   -- 城市名称
        VERSION INT                         -- 版本号
        );

        -- 添加数据
        INSERT INTO city VALUES (NULL,'北京',1),(NULL,'上海',1),(NULL,'广州',1),(NULL,'深圳',1);


        -- 将北京修改为北京市
        -- 1.将北京的版本号读取出来
        SELECT VERSION FROM city WHERE NAME='北京';   -- 1
        -- 2.修改北京为北京市，版本号+1.并对比版本号是否相同
        UPDATE city SET NAME='北京市',VERSION=VERSION+1 WHERE NAME='北京' AND VERSION=1;
```

这里我们创建了一个city表，然后添加了对应的数据，我们先读出其版本号，然后将该版本号进行对比，我们这里最后一行对比是手动获得了一，如果希望是动态的，那么可以将1置换为14行的代码。乐观锁的实现方式有许多种，但是大体都是加标记的思想

最后值得一提的是，乐观锁比较少用，我们只做了解就可以了，这是因为现实生活里本来乐观锁的应用就少，而且乐观锁的实现也更加繁琐，因此我们只做一些简单的了解就可以了

# Mycat

关于MyCat，我们这里只是了解，因为我们只有做项目的时候会用得到，而且用得到的时候，也会再进行一次系统性的学习的，因此我们这里只做了解。

## MyCat介绍

所谓MyCat，简单来说就是用于管理多台服务器之间的服务器的工具

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8c6533876bfe3e06786efaee60cd1fde.png)

## MyCat安装

我还就那个直接跳过，有兴趣自己去看吧，懒得，主要还是因为跟我说还要换一个虚拟机，我才不换呢。反正只是了解，这里我们权当我们已经安装好了

## 克隆虚拟机准备集群环境

看了下，后面的内容一堆安装的，太几把恶心了，不搞了，反正不重要，不学了