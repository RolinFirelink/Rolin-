- DriverManager

那么我们接着就来介绍我们的各个功能类，首先我们来介绍我们的注册驱动的DriverManager方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEadb901968f4c241aa8a8f2d5f73a9d6a.png)

首先我们注册驱动时应该要使用static void register Driver(Driver driver)方法，其会返回一个DriverManager对象，但是我们容易注意到，我们实际的代码里，注册驱动是调用得Class.forName()方法并写入com.mysql.jdbc.Driver，这是怎么回事呢？这是因为在com.mysql.jdbc.Driver下存在静态代码块，这个代码块就是我们注册驱动所需要的代码，我们只要让这个类执行，其静态代码块就会自动执行，因此我们可以通过Class.forName()的方式来注册驱动

最后是在mysql5之后可以省略这一步骤，因为在jar包中存在对应的配置文件，但我们这里处于初学阶段，因此还是采用手动方式来进行注册

然后我们来看看DriverManager驱动管理对象本身，其本身要先调用数据库连接对象，需要输入指定连接的url，其是采用固定前缀接远程ip地址接端口号再接数据库名称的方式来指定，然后是要输入用户名和密码，用/分隔开来。该方法会返回一个Connection数据库连接对象

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE98cfbc4f446d45f616ba43e85cfeb5ae.png)

- Connection

Connection数据库执行对象，其可以调用对应的方法获取普通执行者对象和预编译执行者对象，普通执行者对象就是用于执行我们的sql语句的，而预编译执行者对象我们后面会介绍。其次是我们可以管理事务，开启事务只要调用setAutoCommit()方法，传入一个true的参数就能开启事务了，然后可以调用commit()方法提交事务，还可以调用rollback()回滚事务，当然，我们还有close的释放资源的方法，千万不能忘记的是要释放我们的对应资源，防止资源的占用和浪费

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE10cef2d22c34b234d83075c517325a3c.png)

- Statement

Connection数据库连接对象可以获取普通执行者对象Statement，我们接下来就来重点讲下这个普通执行者对象。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1bbffb37191a7e4c89c0dae5ea01b870.png)

Statement对象可以执行两大类的语句，分别是增删改的DML语句，此时需要调用int executeUpdate(String sql)，其返回值是int，该int类型返回的是影响的行数，接着我们可以在传入的String类型的参数里传入对应的执行语句。第二个是可以执行用于查找的DQL语句，其调用ResultSet executeQuery()方法，其返回值是ResultSet，这个对象里面含有我们查询到的信息，但是对其进行了对应的封装，同样也是传入对应的sql字符串来执行语句。最后也是需要释放资源

- ResultSet

然后我们来讲讲ResultSet结果集对象，其下有next()方法，该方法的作用是判断还有没有数据，有就返回true并将索引向下移动一行，反之则返回false。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE23712876ccb8bcca3e93fedd74e0b4a6.png)

我们要获取结果集中的数据是要通过对应的getXxx方法的，Xxx代表我们获取的量的数据类型，其会返回对应的数据类型，我们要输入对应的列名就可以获取到其对应的数据，最后这个也是要释放资源

其获取资源的方式非常像我们集合里的迭代器的方式