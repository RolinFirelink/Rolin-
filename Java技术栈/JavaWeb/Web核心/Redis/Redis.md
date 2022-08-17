# Redis入门

本来是在学Spring的，发现Spring用到了Maven和Redis技术，所以还是回来先把这两门课程学完吧，后面再去学那些技术也不迟

## NoSQL概念

在学习Redis之前，我们要先对Redis进行介绍，为了介绍我们首先要学习NoSQL概念。传统的关系型数据库SQL存在两大问题，分别是磁盘IO性能低下和数据关系复杂，扩展性差，不便于大规模集群。由于这些问题的存在导致出现海量用户同时访问时，就会出现服务器崩溃的情况。这定然不是我们想要的，那为了解决这个问题，我们的简单想法就是降低磁盘的IO次数，为此我们可以使用内存存储，我们同时还希望去除数据之间的关系，为此我们可以不存储数据间的关系，只存储数据。这两个想法一结合，就产生了我们的Nosql概念

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc3a6116e41633cbc06b693471badb949.png)

NoSQL全称是Not-Only SQL，其泛指非关系型数据库，作为关系型数据库的补充。可以应对基于海量用户和海量数据前提下的数据处理问题，当然，这里的前提是有海量用户和海量数据，换言之就是没有这个前提下那么NoSQL就并不适合。NoSQL的优点就自己去图里看吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcdd7125372f541d1aa87b831a1b7e779.png)

常见的NoSQL数据库就是我们的今天的主角，也就是Redis。下面我们通过一个具体的例子来讲解Redis的理论使用，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEde880244f0b747430b0765840a00a22c.png)

比如说在一个电商场景下，就比如说淘宝吧，我们的商品必然是有基本信息的，那些基本信息我们就存放到MySQL中，商品本身也有附加信息，比如描述、详情、评论，我们将这些存放到MongoDB中，然后有图片信息，我们存放到分布式文件系统中，最后是一些搜索关键字的技术，我们存放到ES等一类检索技术上，最后一个特殊的点是热点信息，热点信息具有短时高频和波动性，由于所有的信息都有可能成为热点信息，因此我们需要存放一些热点信息，我们希望这些热点信息的存储可以解决短时高并发的情况，那么我们就将这些信息存放到Redis中，存放到此我们就能解决某些信息突然被大量访问的情况。最后我们可以看右边的图，可以完整看到我们数据库的一个大概存储结构，我们的基础信息总是先存储到MySQL数据库中，然后向上将MySQL的数据加载各种不同的集群中，主要是为了方便用户获取，接着这些集群和数据库共同对外提供数据服务。

最后我们可以做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa325b4fd5008031a33718b8f42dcd6e3.png)

## Redis简介

Redis是Remote DIctionary Server的缩写，其是用C语言开发的一个开源高性能键值对数据库，其特征是数据没有必然的关联关系，而且内部采用单线程机制进行工作，虽然其采用单线程进行工作，但是由于其读写的速度非常快，因此单线程的对效率的影响几乎可以不计。其支持多种数据类型，最后其还提供一个数据灾难恢复的功能，防止出现数据丢失的情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe8c135f28b630270ef2ebc4d9e95f2e3.png)

Redis应用场景有很多，比如可以为热点数据加速查询，或者即时信息的查询，又或者是时效性信息的控制，如验证码，投票控制，最后是分布式的数据共享，其实还有一个消息队列，但是那个不怎么用了，知道有就行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4dc7e86814b9e58e47b1e6068a722383.png)

## Redis的下载与安装

在正式讲解下载和安装之前，我们要先做一个课程约定，任何Linux的命令，我们都用紫色窗口表示。任何配置文件的命令，我们都用淡色窗口表示。任何Redis的指令或者是Java代码，我们都用蓝色窗口表示。任何示例，我们都用红色窗口展示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE44cc7effc3ec5fa9615a01ed5c8e5015.png)

然后是安装，请看安装步骤，注意我们这里的Linux的版本需要CenterOS7

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbd4b09c832bb927f7cdae946c6aba66f.png)

redis安装目录下有多个命令值得我们先去了解的，这里先列出来了，我们可以先稍微记忆记忆

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf48ddcee44a98a11eafb420c81cd4abc.png)

## 服务器与客户端启动

那么现在我们已经安装好Redis了，下一步自然就是启动Redis，启动的方式有参数启动和配置文件启动，一般主流方式都是配置文件启动。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc19df4863c96a17f000e92a905143ff7.png)

如果我们想要启动Redis，我们可以使用reids-server，如果后续什么都不输入，那么我们就是启动默认端口，也就是6380。如果我们想要指定端口，那么我们就要在后面接上--port 自己指定的端口号

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3820dd7081625204cc75f149e4d170c3.png)

至于客户端连接则是使用redis-cli指令，同样如果后续什么都不写那么就是连接默认端口，如果想要指定连接某个端口就要输入-p 端口号，当然，能连接成功的前提是这个端口号是确实存在的

这里我们要注意一点的是，服务器启动指定端口使用的是--port，而客户端指定端口使用的是-p，不但英文字母的数量不同，而且-的数量也不同

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd736fa4fd10de8dfb622e4ab1face113.png)

最后我们做一些准备工作，比如说创建配置文件的存储目录conf和文件存储目录data等

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6a8486b03a4f3430540fcea7a7a66374.png)

## 配置文件启动与常用配置

那么接着我们就来学习用配置文件启动redis，不过在哪之前我们要先对配置文件做一定的处理，我们要先将我们的配置文件进行一定的过滤并更改其中的一部分内容，我们可以输入命令cat redis.conf | grep -v "#" | grep -v "^$" > redis-6379.conf，此意为我们除去配置文件中的所有空格和#号，然后生成一个新的配置文件，其名字就是redis-6379.conf。然后我们进入这个配置文件中看看，配置文件中的内容很多，我们这里只挑一部分讲

首先是bind 127.0.0.1，bind表示我们默认绑定的服务器地址，一旦绑定，那么就不能通过别的方式去访问了，由于我们现在都是使用我们自己的服务器自己连接自己，所以这个其实改不改都无所谓，暂时不用理。

然后是port 6379，其代表指定启动方式时我们默认启动的端口号

接着是timeout 0，其代表超时时间，也即是说我们的客户端连接我们的服务器，多长时间不操作之后，其就会自动断开，这样来节省我们的空间，不过由于我们还是学习阶段，所以其实这一个可以不指定

然后是比较重要的daemonize no，这个比较重要，其代表我们的服务器是否以守护线程的方式启动，如果我们选择是，那么就相当于我们的服务器会默认以后台方式，也就是守护线程的方式启动，此时我们的日志信息就会默认写到我们的日志文件中，而不会直接显示在控制台中。如果我们选否，那么我们的服务器就会以前台方式启动，此时我们一旦启动我们的服务器，我们在控制台上就不能做任何的操作了，只会显示日志文件，必须去另外的控制台上。我们这里推荐以前台方式启动，因为查日志又要我们专门去看，就会很慢，不好使

接着是loglevel notice，这个是日志的等级，这个知识一会再说。logfile ""则是可以指定生成的日志文件的名字

最后是dir ./，这个指定的是我们生成的日志文件的地址，我们这个地址当然要指定到我们的data中

然后我们对我们的配置文件做相应的修改，我们在其中指定我们的日志文件的生成目录，然后日志文件的生成名，我们这里取了有6379关键字的名字，这是因为我们现在使用的就是6379的默认端口，如果以后用别的端口，我们再把日志的名称给改成对应的端口关键字，这样我们好区分，不然啥玩意都放一个日志文件里，这没法整

最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe4f7b864f0f0070a249f24cb98113151.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5317abac132edc1ab2e351806b6e5c9a.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf1f1a97835e173bd9a599238551a3cac.png)

最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd0873f2c14eeab8c38df9e1bc75217d2.png)

## 命令行常用操作

由于我们的Redis是命令行模式的，所以我们首先要学习以下四种命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE36fb721539fc31ad3eaaca77388f241f.png)

首先我们学习最简单的set和get命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd6c71bde574f395e9e14057659c1724f.png)

如果我们想要获得某个命令的帮助，可以输入help 命令，如果想要获得命令所在的群组的帮助，则要输入help @群组名称

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbb4047502248f42c3168e4480c7b6b34.png)

最后是下边的退出命令行模式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe1fa20082574708b52212797c0b4895e.png)

# 数据类型

本节我们来学习Redis的数据类型

## 数据类型简介

我们首先要了解下哪些数据我们要置于我们的Redis中，具体请看下图，下图中的信息就是要放置到我们的Redis中的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7962a7d0b9406d2ac48c41e1cb93a762.png)

最后我们的Redis的数据类型总结出来五种，分别是string、hash、list、set以及zset，最后一种zset的使用场景很少，所以我们这里就不重点学习了，提一提就差不多得了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0710ba918210bbfad0757f8c5e657c4d.png)

## String类型数据基本操作

在正式学习String之前，我们要先学习redis的数据存储格式，我们都知道redis自身是一个Map，其下所有的数据都是采用key：value的形式来存储的，那么其数据类型就是指value的部分，key部分永远都只是字符串

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1221e98212b83234412ff9a202c82658.png)

接着来我们具体说说string类型，其一般使用字符串来保存数据，但是如果字符串是以整数的形式展示，那么也可以作为数字操作使用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe6081d15e92b5a2593ddd060cc8bbde6.png)

接着我们正式来学习string类型的数据操作，set和get以及del就不多说了，这个很好理解。这里值得重点一提的是setnx，其也是用于设置的命令，但是其会返回一个布尔类型的结果，如果一开始我们的key和value都没有被设置，那么其会正确添加并返回1，代表成功。但是如果我们尝试覆盖，在一个已经有值的key上添加值，那么就会返回0，代表不成功

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf560df7402fab0c4ff6281bb6ebb3416.png)

接着是批量添加和批量获取，分别是mset和mget，我们也可以通过strlen命令，输入对应的key获取我们的对应的value的字符串长度，然后是append key value，这个命令是将信息追加到我指定的信息的后面，如果原始信息压根就没有那么就直接创建

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE73d01fd22e5687d5313879b695b14bdc.png)

最后我们要解决一个问题，那就是我们到底要用单个添加还是多个添加比较好，实际的测试结果是，如果我们要添加的数据比较少，那么使用单个添加还是批量添加都是可以的，但是如果我们要添加的数据比较多，那么我们就应该选择批量添加

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE638b2d01a120983da9bcaab136d70015.png)

- string扩展操作

接下来我们学习string的扩展操作，首先是incr，这个简单可以理解为i++，会将我们指定的值+1，当然，这个前提是这个值的确是int类型或是可以被转换为int类型的，而incrby则是可以指定每次增长的值，incrbyfloat则是可以对小数操作。decr则反之，这个不多提了。setex是可以设置我们的数据一定时间的生命周期，这里生命周期的时间是以秒来设计的。这里的操作常常用在我们的验证码中，比如说我们设置一个数据，设置五分钟内有效，这就是验证码经常的一种使用情况了，这也对应了我们之前说的验证码常放在我们的redis中的知识。而psetex的作用和我们的一样的，只不过这里的秒数变成了毫秒而已

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf0d7e98d2453f432261944238385d225.png)

最后关于string类型的数据操作我们还有一些注意事项要看，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf9a11785753cbdd34995248a6902c0c4.png)

- string应用场景与key命名约定

比如说我们微博中的大V，那么其粉丝数量，博客数量那些要优先加载而且总是更新的数据，我们都是放在redis中的，我们往里面添加用户信息的时候，我们可以用用户主键和属性值作为key，然后后来设定定时刷新策略。当然，我们也可以使用json格式保存数据，这里就不再演示了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe777d0cadb4932464d61fa8beda10be9.png)

最后我们说下我们key的命名约定，一般我们是以表明：主键名：主键值来命名一个key的，如果说其下还有字段的不同，那么我们还可以在后续再设置上一个字段名上去

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3f32c5210645363621ef574cdf397414.png)

## hash的基本操作

下面我们来学习第二个数据类型，hash。在学习hash之前，我们先来介绍下什么是hash。我们之前介绍过json的存储的方式，其可以将数据封装为类似于是一个对象，这样很多数据都可以放一块，便于管理，但是相应的，这种存储方式却让我们的数据更新变得繁琐，比如说我们要更新一个数据的话，我们需要将所有数据更一起更新一遍，哪怕其他数据其实没有发生变化。

这时我们的一个解决方式就是将我们的各种数据分开存储，此时我们会发现这些数据的key部分都是一样的，只是最后一段有所不同而已，而且我们仍然期待有一种方式可以将这些数据封装到一起，而更新数据时又不必一起更新，此时就轮到hash登场了。hash可以将这些数据封装到一起，称为一个hash，不同的字段列我们成为field，也就是属性，其有对应的value值。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8d65b3a098ce3aa85a0704c167cc7a7d.png)

下面是对于hash类型的图示介绍，其底层是使用哈希表结构实现数据存储。这里有一点值得一提，那就是如果field的数量较少，存储结构将优化为类数组结构，而如果field数量较多，存储结构将使用HashMap结构

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5936120ada91652fc14cd046d74aa0e5.png)

接下来我们来学习hash数据类型的基本操作，其实这里很多是非常类似我们之前学习过的操作的，只不过的命令名称有所变化罢了，还有在Hash里是要添加上field字段的后跟值的。添加以及修改数据是hset，而获取数据是hget，其还提供了hgetall，获得hash里所有数据的命令，当然还有删除数据的hdel，最后是hsetnx，该命令可以设置field的值，如果field已经存在值了则不做任何变动

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8492c1c50909f9b022b19d1769d300d9.png)

批量添加的命令是hmset，批量获得是hmget，hlen可以获得哈希表中字段的数量，也就是field的数量，hexists可以判断哈希表中是否有指定的字段

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa4b62ef63208972c594d0597d3838839.png)

这里特别要提一下hgetall方法，该方法可以获取到hash表中的所有数据，其表示方式是先展示字段，后展示字段的value值，如下图所示，也可以理解为奇数展示字段，偶数展示值

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE693e8e74f8ddd2a41bfb95c013e556e3.png)

### hash扩展操作

hash的扩展操作有hkeys和hvals，前者可以获取哈希表中所有的字段名，后者可以获取哈希表中所有的字段值

而hincrby可以设置指定字段，令其增加具体范围的值，如果希望其减少的话，那么只要指定其增加一个负数就行了，而hincrbyfloat则是可以指定小数的值

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6f6de4547684d2f8108796ccc0922f6a.png)

最后我们来看hash类型数据操作时的注意事项，首先hash类型中的value只能存储字符串类型，不能存储其他属性，同时不允许hash嵌套hash，别整天想着嵌套的骚操作啊。然后是hash类型虽然十分贴切对象的数据存储形式，很好用，但是其设计初中并不是为了存储大量对象而设计的，因此我们不可滥用，别有个数据就直接往hash里放，总体来说hash用的还是比较少的，更多时候我们都是使用string，只要用得好就行。最后hgetall操作可以获得全部属性，但是如果内部field太多，那么整体的效率就会很低，所以我们平时要尽量小心使用这个命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa9e0e864364fd6c64dca7c502bd4c8de.png)

### hash的应用场景

hash的一个简单的应用场景就是手机移动卡的充值，比如说我们电信推出了30、50、100的抢购活动，每种商品上钩的上限是1000张，那么我们就可以构建一个hash表，其中我们的30、50、100是我们的field字段，而数量则是我们value的值，卖多少就调用相关的hincrby命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd96995ee64f800221d77e80b4715f05f.png)

我们来看看实现的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE56124df73c9abcf5a9f84cdd7c3cb090.png)

这里有一点需要注意的，那就是实际的项目里还有超卖的问题存在，比如说我们明明只剩下3张了，给我卖了五张，那肯定不行啊，但是我们这里就不考虑这个问题了，这个问题等到我们以后做项目的时候再考虑

## list基本操作

list类型保存多个数据，底层使用双向链表存储结构实现，其与hash有一点显著不同的是其保存的数据是具有顺序的，并且可以做对应的顺序操作。下图中还带大家复习了顺序表链表以及双向链表的定义，虽然说对我来说其实没有必要就是了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE435d2d651a90cba25acf24763625375a.png)

list类型可以存储多个数据并且可以对存储空间的顺序进行区分，其中key和列表都可以执行对应的操作，其添加元素可以往左添加也可以往右添加，还可以通过下标找到对应元素，弹出元素也可以往左弹和往右弹，但是要注意，我们弹出去就代表出去了，那么这个元素就不在我们的list中了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf2b00bc8aa688882264dbfa44e80dc22.png)

接下来我们来学习下list的基本操作，lpush和rpush分别是往左和往右添加数据，而lrange则是可以获取对应区间中的数据，这里要指定开始与结束的下标，如果指定0 -1则可以直接查看到所有的内容，如果不这么指定又想要获得全部内容的话则需要指定0 长度-1，lindex可以获得指定下标的数据内容，如果我们的下标越界了，其会返回nil，代表不存在数据，而不会抛出数组下标越界异常，llen则可以返回list中key的数目。lpop和rpop则可以从左和从右弹出数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE852eef085d033e3ea38c443dcfaba5a0.png)

### list扩展操作

学习完了基本操作之后，我们来学习list扩展操作，list的扩展操作有一个是可以移除lrem指定数据，这里移除可以指定移除的数量，如果指定的数量小于实际的数量，则其会从左往右逐个移除指定的数量，大于或者等于则会移除所有。然后blpop是可以从在规定时间内从左端获取指定的元素，我们这里可以指定多个list，如果第一个有就直接从第一个里拿，没有就从第二个里拿，直到全部都没得拿了，没得拿其就会在我们规定的时间里等待，如果等待过程中有人往我们指定的集合里添加数据，那么其就能取出，若没有，那么到了规定时间之后其就会显示取不到

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1ab691ddc1a67595c3ca05a05f5d58c2.png)

最后来看下我们的list集合里一些需要注意的事

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8f7ae47a57946add708afb6aab5b60b2.png)

### list应用场景

list集合的一个简单的应用场景就是用于将多态服务器的操作日志统一顺序输出，我们可以将我们多台服务器的日志信息全部存储到redis中，然后当我们想要访问其数据文件的时候我们就连接redis然后用list集合返回这些信息就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeb06edc437e7cb78d80124b9ee96078a.png)

如果我们想要得到最新的消息，那么就使用栈模型，让最后的信息最先出来，如果只是想令其汇总到一起，那么只需要使用队列模型就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEec5c6ad02114f1f89d8f4270f048e323.png)

## set基本操作

我们同样先来介绍下什么是set，其实和我们java里的set差不多，这里我们理解set数据结构可以用map数据结构来理解，我们这里set代表一整块空间，而其中原来存放key的值就变为的value，而原来存放value的值，则全部存放nil，也就是不存储。而key则变成了代表set本身的一个字符串类型的值。set集合能够保存大量数据，便于查询，其下的数据是不允许重复的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE661fc4a9801cd7ffb50eca1e20781033.png)

其对应的操作有添加和获取全部数据以及删除数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6272161e2a4f723685dfa6f0c0c27cf2.png)

还有一些特殊的方法，可以获得集合中的数据总量或者是判断其有没有数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbebba51726d9c52c22e32fa58263f9e5.png)

### set扩展操作

set集合内的扩展操作有sinter求两个集合的交集，sunion求两个集合的并集，以及sdiff求两个集合的差集，注意差集的结果会因为两个集合的顺序不同而发生变化。而后面的命令则是可以将结果所求的交并差结果存储到指定集合中

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe564d8212b32c7a4d29161ec8c8c4c56.png)

最后的smove指令可以将原始集合移动到目标集合中。最后我们看一些set集合的注意事项

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8961f3ad1448aee1642f587ab16e31e3.png)

### set应用场景

最后我们来看一下set集合的应用场景，set集合的一个经典的应用场景就是用于维护黑名单，具体请看下图介绍的情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5e6870bbe0a109f78a36f471e2e4fae7.png)

黑名单的过滤法有很多种，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE824d5673fd743beea4051ff0f053a6f2.png)

## 实践案例

那么讲解完了所有的数据类型之后，接着我们就来做一个实践案例，我们首先来看看我们的业务场景，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbae325a957f292fc71b5c9c3c0aefdb1.png)

接着我们进行一个业务分析，我们可以创建三个数据类型用于完成我们的业务场景，分别是两个list集合和一个set集合，我们的set集合用于存储用户设置为置顶的用户，list集合则有两个，一个设置为存放普通消息，一个设置为存放置顶消息，每次有人发送信息，我们都从先将该的信息与set里的信息进行比对，若其存在于set置顶中，则我们将其放置于置顶集合中，若不是则放置于普通集合中。同时我们每次放集合里放置消息时，都要将集合遍历一遍，然后删除掉原先的内容对应用户的数据，接着将新用户的数据压入，这样就能保证最后发送数据的用户总是排在最前面，这里的list集合是要采用栈模型的，最后我们取出里面的内容时，我们总是先将置顶集合中的内容取出来，后面将普通集合中的内容取出来

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE783379d9a03b6c44a5941b771d51aa57.png)

然后我们来看看具体步骤，这里值得一提的是，我们可以再另外创造一个hash来记录用户发送消息的数量

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdf40a8b676c9d7f042c74c321013fee6.png)

最后我们只需要实现这个逻辑就可以了，这里就不带着大家做一遍了。

# 常用指令

## key常用指令

我们之前学习的指令都是一些对于具体的数据类型的指令，而我们现在要学习的指令是对于所有的key的指令，我们先来看看我们要学习key相关的哪些类型的指令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE941ae890adb505b79db03b0a1c52c12f.png)

首先是最基本的删除、获取、以及查看key的类型的指令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4945cdd3effec4270d13acdfe7a71136.png)

然后是排序和改名的命令，排序的命令并不会真正对我们的内部进行一个排序，而是将其展示的结果进行排序，同时如果我们想要倒序排序或者是对字母进行排序，那么我们就需要调用其相关的子命令，这里就不赘述了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf1fb76fdc97c44d151317c2f5ffe5746.png)

接着这个命令就是比较重要了的，其可以为指定的key设置有效期，其中expire是设置通过秒来设置有效期，而pexpire则是以毫秒展示，expireat是以时间戳的形式来设置，而pexpireat则是以毫秒时间戳的方式来设置。当然我们也还有获取key的有效时间的方法，分别是ttl和pttl，前者返回的是以秒为单位，而后者则是以毫秒为单位。如果我们的key是永久的，那么使用ttl的结果会是-1，如果是失效的，那么会是-2，如果还在倒计时中，那么其就会正确显示对应的倒计时。最后是persist方法，其会将key从时效性转换为永久性的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa1cdae6e5d02344410a5bf03bbaf09cb.png)

最后我们还有查询key的操作，其有各种不同的查询模式，具体请看下图，这里就不赘述了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE810752049a4b75df8eec59d6de01bc73.png)

## DB常用指令

接着我们来学习关于redis数据库的常用指令，在学习之前，我们要先知道，为了解决数据库中出现大量的数据以及重复的key时，我们的redis数据库提供了编号从0-15的16个数据库，每个数据库之间相互独立。如果是在0数据库，则不会显示编号，若是在其他的，则会显示对应的编号。实际上，数据库的数量我们也是可以修改的，但是我们一般不这么做。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa23f8b0da4de699a9bf361d9bdd12f41.png)

数据库的基本操作有select命令，该命令可以在不同数据库之间切换，还有ping命令，该命令可以让测试我们的服务器是否连接，若是连接，我们输入ping其会打印pong，若没连接其会直接提示没连接

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd9d80e26d279b5efb577fbfba86e1146.png)

最后我们说一下db的扩展命令，首先是move命令，其可以指定某个key移动到另一个数据库中，dbsize则可以获取当前数据库的key的数量。flushdb会清除当前数据库的所有内容，而flushall会清除所有数据库的所有内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeb06a57459bbb15f83c73b27594ab6d1.png)

# Jedis

本章节我们来学习Jedis，什么是Jedis呢？其实Jedis就是连接我们的Redis的一种Java程序的称呼

## Jedis介绍

Jedis用于Java语言连接redis服务，并提供对应的操作API

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6a5b91a3786bc486f062ddbbdf43b9a6.png)

除了Jedis以外，还有很多的Java语言可以连接redis服务，不过我们这里就不提了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6b43b1736ec449835a0cccec13e8851b.png)

接下来我们要连接到我们的redis中，我们首先要导入对应的jar包，然后我们调用其对应的方法获取其连接对象，那么我们容易构造代码如下

```
public class JedisTest {
    public static void main(String[] args) {
        //1.获取连接对象
        Jedis jedis = new Jedis("127.0.0.1",6379);
        //2.执行操作
        jedis.set("name","itheima");
        //3.关闭连接
        jedis.close();
    }
}

```

但是这一份代码却会报错，这是为什么呢？这是因为我的Redis是装载在虚拟机上，其有一个对外的虚拟ip，我们要用这个虚拟ip连接才可以，进入Xshell，输入ip addr，可以查找到我们的虚拟ip是10.0.8.11，那么我们可以将我们的代码改造如下

```
public class JedisTest {
    public static void main(String[] args) {
        //1.获取连接对象
        Jedis jedis = new Jedis("10.0.8.11",6379);
        //2.执行操作
        jedis.set("name","itheima");
        //3.关闭连接
        jedis.close();
    }
}
```

不过由于我们是云服务器而非虚拟机，因此我们的代码其实是应该修改如下

```
package com.itheima;

import redis.clients.jedis.Jedis;

public class JedisTest {
    public static void main(String[] args) {
        //1.获取连接对象
        Jedis jedis = new Jedis("175.178.114.158",6379);
        //2.执行操作
        jedis.set("name","itheima");
        //3.关闭连接
        jedis.close();
    }
}
```

但是即使如此，我们的这份代码还是不能连接，会报另外一个错误，其会说我们的Redis在运行时被保护了，那我们应该怎么办呢？这其实是因为最开始我们的Redis服务的配置文件搞了保护所致，我们只要将对应的redis服务的配置文件的第一个bind端口号设置成其虚拟端口号就行了。

最后我们经过测试会发现这个的确是可以使用的，没有问题的，此时就说明我们的Redis已经成功被我们的Jedis连接了

最后我们的Jedis有一个基于maven的配置文件，这个先看看就行，因为以后我们要学maven的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1697bbb76276603a51acfd4216ef1076.png)

最后我们来看看我们的步骤总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE09b6a3ba843a4305727754ee8ede2c89.png)

## 工具类开发

我们之前学习了连接redis，但是我们之前的连接都是将我们的代码写死的，这样当然不便于我们的开发，我们有一个想法就是将其写在一个工具类中，不但如此，而且还要提供连接池的内容，那么最后可以构建代码如下

```
package com.itheima.util;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.util.ResourceBundle;

public class JedisUtils {

    private static int maxTotal;
    private static int maxIdel;
    private static String host;
    private static int port;

    private static JedisPoolConfig jpc;

    private static JedisPool jp;

    static {
        ResourceBundle bundle = ResourceBundle.getBundle("redis");
        maxTotal = Integer.parseInt(bundle.getString("redis.maxTotal"));
        maxIdel = Integer.parseInt(bundle.getString("redis.maxIdel"));
        host = bundle.getString("redis.host");
        port = Integer.parseInt(bundle.getString("redis.port"));

        //Jedis连接池配置
        jpc = new JedisPoolConfig();

        jpc.setMaxTotal(maxTotal);//设置连接池的最大的数量
        jpc.setMaxIdle(maxIdel);//设置连接池里的活动连接数量

        //连接池对象
        jp = new JedisPool(jpc,host,port);
    }

    public static Jedis getJedis() {

        //主机IP
        //String host = "175.178.114.158";
        //端口
        //int port = 6379;


        return jp.getResource();
    }
}

```

这里我们用创建一个配置文件的方式引入我们的连接，配置文件的内容如下

```
redis.maxTotal=50
redis.maxIdel=10
redis.host=175.178.114.158
redis.port=6379
```

这里将对应的给连接池设置的代码，设置连接池里数量的代码，创建连接池的代码都放在了静态代码块里，而只留下返回连接池中连接的对象的代码到方法中，这样才是符合我们开发时要设置的代码方式

最后我们来看看总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE113e66cbbd8549a592bf1212a9957679.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd9605f6b744431333b79b1905f94402c.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE79a857f56130d6792b8dae4f4611613c.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe502d92c0157f233a2565aca42531454.png)

## 可视化工具

这个工具的名字就叫做Redis Desktop Manager，这个可视化工具的使用很简单，我相信以后也有更加简单的可视化工具给我们使用，这里我们只是提一提有这种工具就行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEda389f4cea4059b25ecedc9bdeebf33c.png)

都什么年代了，还在用宝塔，现在都用Another Redis Desktop Manager了

# 持久化

本章节我们来学习持久化，这也是Redis最后一章的内容。我们这一章实际操作的少，理论讲解的部分多，所以要注意听

## 持久化介绍

首先我们来讲解下什么是持久化，持久化简单来说就是利用永久性存储介质将数据进行保存，并且在特定的世界里将保存的数据进行恢复的工作机制。其主要目的是为了防止数据的意外丢失，确保数据安全性

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEba9ab1aea4e321151a37a278028bee1a.png)

持久化有两种方式，一种是直接存储数据的结果，格式简单，关注点在数据，这种方式成为RDB。另一种方式是保存数据的操作过程，其是利用日志形式，关注点在数据的操作过程，成为AOF

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5dc451ee9aa21528e9ebd43a126fd9d8.png)

我们接下来就对这两种数据的保存方式进行逐一的讲解

## RDB

### save指令完成RDB

RDB有是那种方式可以完成，我们先来介绍第一种方式，也就是使用save指令来完成的方式。这个方式非常简单，直接输入save就行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd0ab4ab59b4f5f61f9c9f880237d8169.png)

不过输入这个save命令的时候，有一些参数我们需要提前设置，也就是下图中的四个参数，下图中的下个参数是在配置文件里设置的，其中后两个默认开启我们不用管，第二个我们已经设置了，所以我们只需要设置第一个就行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEad37d32cb84788382a0657956481c8a2.png)

那么我们只需要进入我们对应的conf文件，然后增加对应的配置信息就可以了。接着我们可以开启几个窗口自己去测试，最后测试我们容易发现每次save我们的文件都会保存到data文件夹中，其内容会是乱码的，只有英文名称能稍微看见。每次我们的服务器重启时就会先加载这里的文件进入到内存中接着再运行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf74df7b6e105e249436ffb7c57fba85b.png)

那么这种方式有什么弊端呢？其最大的弊端就是我们的Redis服务器是单线程的，如果我们的数据量较大的话，save指令就可能会耗时过长，最后阻塞Redis服务器，直到当前的RDB过程完成位置，线上环境不建议使用这个方法，当然你非要用也没法是吧

### bgsave指令完成RDB

那有没有什么办法可以解决这个阻塞问题？当然有，那就是使用bgsave指令，该指令可以启动后台保存操作，但不是立刻执行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc854d1bf7834d505bb63bfb8f62890c3.png)

来看看其相关配置，由于我们之前都已经设置过了，所以我们这里就不重复设置了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8a2152ce66e082fe38188864fbdbf529.png)

最后我们来讲解下bgsave的内部执行过程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa461d3b79ccedb9dcbadb628d940d607.png)

bgsave命令向Redis发送后，Redis会调用fork函数生成子进程来创建rdb文件，创建成功之后再返回成功的消息，这个保存的方式是主流的方式，我们以后所有的save都用bgsave替代

### save配置完成RDB

我们可以设置一个自动持久化的条件，让我们的Redis在满足限定时间范围内的key的变化数量时就进行持久化

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2bea02a89258d6822c9356208b26cee9.png)

设置这个需要到配置文件中设置，其中second是监控的时间范围，而changes则是key的变化量

然后我们来看看相关配置

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE69abc2d315e0ca83dc134fc824b4a2db.png)

save配置的工作原理是用户没进行一次操作就往Redis中发送一次指令，Redis会返回这个指令的结果并记录，一旦在规定时间内达到了指定数量就进行保存。不过这里返回的结果里只有对数据产生影响的结果才会被计入，比如说我们调用get，这是不会计数的，或者我们调用set但是没有set成功，这样不计入，同时，即使我们修改的数量的一样的，也就是说，我们将1设置为1，这样设置虽然表面看起来没啥变化，但是实际上也是会被计数的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE94c96c429a5df8216033d919d5c9e4af.png)

最后一点我们要注意的save配置要根据实际业务情况进行设置，频度过高会有性能问题，过低会有数据不安全的情况，最后save配置启动后执行的是bgsave

### RDB方案比对与优缺点分析

最后我们来分析下RDB的三种方式的优缺点，先看对比图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa2d1eb42d4542603655ba35f397701c1.png)

其实根本不用看，因为我们肯定都是用bgsave的，然后我们来讲解RDB的特殊启动形式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6d001ca1f25fd96eec20b373b45f1d51.png)

debug reload可以在我们的服务器运行过程中启动，也就是热启动，简单来说就是服务器重启时我们的save指令也会执行

然后是shutdown save，这个指令关闭服务器时是可以指定保存数据的。

最后是全量赋值，这个留到我们明天的课程中讲

最后我们来看看RDB的优缺点分析

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9ee99a33782072e4d8b68f991ae4c552.png)

RDB我们已经讲完了，接下来我们来讲解AOF

## AOF

首先我们来讲RDB存储的弊端，具体请看下图

### RDB弊端

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7659e7d9d67b47bde44228d5cc877551.png)

为了解决这个问题，我们就是使用到AOF来进行数据持久化，其主要作用是解决数据持久化的实时性，目前已经是Redis持久化的主流方式

### AOF介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4621dd49228bcc1393be7ebfe606f714.png)

然后我们来看看AOF写数据的过程，每当用户写入一个数据，AOF就将写入的命令刷新至缓存区，当缓存区的命令达到一定数量之后，就将其同步到AOF文件中，以此来提高效率，而不是每做一个命令就同步一次

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE30e5477d6da4486fbb30f2f412d30ad7.png)

### AOF配置

接着我们来启动AOF的相关配置，需要在配置文件里写入以下四个参数

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE11d57c73dae19a904f1eec76767e06fe.png)

第一个是开启AOF持久化功能，我们写个yes就完了这里。第二个是取名，第三个是指定保存地址

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE11cf3ad502c51a6bfc77f4ca37f1b9d8.png)

最后一个配置值得我们特别提一提，我们可以设置三个参数，一个是always，其代表每次写入操作均同步到AOF文件中，其数据能达到零误差，但是效率比较低。然后是everysec，其代表每秒将缓冲区的指令同步到AOF文件中，会丢失一秒的数据，其数据准确性较高，性能比较好，我们推荐使用这个，同时这也是默认配置。最后一种由操作系统控制，整体过程不可能，几乎不会使用，不多提了

最后我们写入其配置如下，注意，这里的bind后面连接的地址并不是我们真正要填入的地址，我们要填入的地址是10.0.8.11，这点不要搞错了，然后我们使用Jedis连接时要使用我们的远程ip地址。

另外如果我们设置了密码，那么在对应的java连接中，要调用其auth方法并传入密码才能正确连接，同时在Linux程序中连接了Redis之后还需要调用auth 密码，这样的命令才能正确连接进去。

Redis设置密码的方式就是进入下面的配置文件，并写入requirepass 你想指定的密码，然后保存就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE069c30c3c41fdc1050f5048b094b27a9.png)

然后我们去查看内部保存的日志的内容的时候，我们可以看到如下的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEceba49e2b97033ec524fd65b576a5234.png)

这里连个星号之间的内容表示的是我们执行的语句，星号后面的数字表示的执行的语句数量，而$符后面的数字表示我们执行的语句的长度，就这样了

### 手动AOF重写机制与工作原理

我们先来讲讲什么是AOF重写，AOF重写简单来说，就是如果我们的命令最终的效果是一个，前面的动作都可以被省略，那么其就会将其省略，直接写到最后一个结果，比如说我们对一个数执行三次+1操作，那其实还原的时候没必要这样啊，直接+3不就完了，那么此时就可以使用AOF重写机制

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE618ce1de7a35ea18e535cca819ffbb14.png)

该机制可以提高我们的持久化效率，并且降低磁盘占用量

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbb376f966ea54ff1510e32b7ccc71dbf.png)

最后我们来看看其重写规则

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8f8d8069e12c104cb15ca6baa6f515b0.png)

AOF的重写方式有两种，一种是手动重写，一种是自动重写

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2e3fefd3c94b800b5065c1118363e022.png)

这里值得一提的是，自动重写调用的都是手动重写的方式。还有一点是，重写后的内容是乱码的形式，也就是最开始我们的直接保存数据的形式，而后续的内容则会以命令的形式继续往下保存，这点了解一下

最后我们来看看手动重写的原理

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdd07dc3a75241c98826af77b0d75e067.png)

### 自动AOF重写机制与工作原理

AOF自动重写的除法条件设置有两种，一种是设置大小，当我们的aof文件超过一定大小时就执行重写，另外一种是设置百分比，当aof文件占到我们内存中的百分之几时就执行重写。这两个条件的执行依赖于其下面的两个函数

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb91fc8a173ec94a6ff27c8867ee157dd.png)

接着我们来看看AOF的重写流程，首先我们来看不进行重写的两个不同AOF方式的执行过程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd9b8e7b48945b2475515563de87e2e01.png)

接着是会重写的过程，或重写的过程里会将aof放置于aof重写缓存区，其会提供数据用于重写，然后会有子进程来执行重写，当重写完毕后会产生新的aof文件，该aof文件会替代原先的文件，同时替换后的文件就没有存在的必要了，此时原先的文件消失

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9d594b4abc78a40f604fcfefe7ee9f4d.png)

## RDB与AOF优缺点分析

先来看看对比

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7c817f7823ab4147908be0d6b6aa4486.png)

然后我们来看看我们应该选择哪个好

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1cdad923817ad45f405b822ebf33842e.png)



