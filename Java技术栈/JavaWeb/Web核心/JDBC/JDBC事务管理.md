- JDBC管理事务的介绍

之前我们说过，在JDBC里，管理事务的功能类是Connection，该功能类可以执行开启事务，提交事务和回滚事务的动作

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfe34a68bebbf65eb6879240acedb86d3.png)

- JDBC管理事务的演示

那么接着我们就来进行JDBC管理事务的演示，首先我们在UserSerice里提供了批量添加的接口，其定义如下

```
    /**
     * 批量添加
     * @param users
     */
    void batchAdd(List<User> users);
}
```

然后我们在对应的Serice层的实现类下写入如下代码

```
/*
    事务要控制在此处
 */
@Override
public void batchAdd(List<User> users) {
    //获取数据库连接对象
    Connection con = JDBCUtils.getConnection();
    try {
        //开启事务
        con.setAutoCommit(false);

        for (User user : users) {
            //1.创建ID,并把UUID中的-替换
            String uid = UUID.randomUUID().toString().replace("-", "").toUpperCase();
            //2.给user的uid赋值
            user.setUid(uid);
            //3.生成员工编号
            user.setUcode(uid);

            //出现异常
            //int n = 1 / 0;

            //4.保存
            userDao.save(con,user);
        }

        //提交事务
        con.commit();

    }catch (Exception e){
        //回滚事务
        try {
            con.rollback();
        } catch (SQLException e1) {
            e1.printStackTrace();
        }
        e.printStackTrace();
    } finally {
        //释放资源
        JDBCUtils.close(con,null);
    }
}
```

这样就可以实现对事务的回滚操作了，我们这里首先在方法中获取数据库的连接对象，然后我们开启事务，接着我们通过循环遍历我们集合中的对象，先给uid和员工编号进行一个值的自动赋，然后我们最后将对应的集合传入到userDao层的添加方法中， 我们在循环结束之后调用提交事务方法，在异常执行中调用回滚事务的方法，这样一旦出现一个异常就会让我们的事务回滚，不出现那就这样呗。最后不要忘了释放我们的资源，没有的对象传入null就完了

这里为什么我们不往Dao层里进行事务的回滚动作的写入呢？这是因为有些操作压根就不需要回滚事务，而Dao层是底层代码，所有的上级代码都需要调用它，如果我们往Dao层上写，那就会变成不管是哪个需求都会出现事务的回滚动作，这显然不是我们所需要的，因此我们这里往service层上写，这样就可以达成我们所需求的部分特定需求使用特定功能的想法了。