本来是在学Spring的，发现Spring用到了Maven和Redis技术，所以还是回来先把这两门课程学完吧，后面再去学那些技术也不迟

- NoSQL概念

在学习Redis之前，我们要先对Redis进行介绍，为了介绍我们首先要学习NoSQL概念。传统的关系型数据库SQL存在两大问题，分别是磁盘IO性能低下和数据关系复杂，扩展性差，不便于大规模集群。由于这些问题的存在导致出现海量用户同时访问时，就会出现服务器崩溃的情况。这定然不是我们想要的，那为了解决这个问题，我们的简单想法就是降低磁盘的IO次数，为此我们可以使用内存存储，我们同时还希望去除数据之间的关系，为此我们可以不存储数据间的关系，只存储数据。这两个想法一结合，就产生了我们的Nosql概念

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc3a6116e41633cbc06b693471badb949.png)

NoSQL全称是Not-Only SQL，其泛指非关系型数据库，作为关系型数据库的补充。可以应对基于海量用户和海量数据前提下的数据处理问题，当然，这里的前提是有海量用户和海量数据，换言之就是没有这个前提下那么NoSQL就并不适合。NoSQL的优点就自己去图里看吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa325b4fd5008031a33718b8f42dcd6e3.png)

常见的NoSQL数据库就是我们的今天的主角，也就是Redis。下面我们通过一个具体的例子来讲解Redis的理论使用，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEde880244f0b747430b0765840a00a22c.png)

比如说在一个电商场景下，就比如说淘宝吧，我们的商品必然是有基本信息的，那些基本信息我们就存放到MySQL中，商品本身也有附加信息，比如描述、详情、评论，我们将这些存放到MongoDB中，然后有图片信息，我们存放到分布式文件系统中，最后是一些搜索关键字的技术，我们存放到ES等一类检索技术上，最后一个特殊的点是热点信息，热点信息具有短时高频和波动性，由于所有的信息都有可能成为热点信息，因此我们需要存放一些热点信息，我们希望这些热点信息的存储可以解决短时高并发的情况，那么我们就将这些信息存放到Redis中，存放到此我们就能解决某些信息突然被大量访问的情况。最后我们可以看右边的图，可以完整看到我们数据库的一个大概存储结构，我们的基础信息总是先存储到MySQL数据库中，然后向上将MySQL的数据加载各种不同的集群中，主要是为了方便用户获取，接着这些集群和数据库共同对外提供数据服务。

最后我们可以做一个小结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcdd7125372f541d1aa87b831a1b7e779.png)

- Redis简介

Redis是Remote DIctionary Server的缩写，其是用C语言开发的一个开源高性能键值对数据库，其特征是数据没有必然的关联关系，而且内部采用单线程机制进行工作，虽然其采用单线程进行工作，但是由于其读写的速度非常快，因此单线程的对效率的影响几乎可以不计。其支持多种数据类型，最后其还提供一个数据灾难恢复的功能，防止出现数据丢失的情况

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE44cc7effc3ec5fa9615a01ed5c8e5015.png)

Redis应用场景有很多，比如可以为热点数据加速查询，或者即时信息的查询，又或者是时效性信息的控制，如验证码，投票控制，最后是分布式的数据共享，其实还有一个消息队列，但是那个不怎么用了，知道有就行了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe8c135f28b630270ef2ebc4d9e95f2e3.png)

- Redis的下载与安装

在正式讲解下载和安装之前，我们要先做一个课程约定，任何Linux的命令，我们都用紫色窗口表示。任何配置文件的命令，我们都用淡色窗口表示。任何Redis的指令或者是Java代码，我们都用蓝色窗口表示。任何示例，我们都用红色窗口展示

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4dc7e86814b9e58e47b1e6068a722383.png)

然后是安装，请看安装步骤，注意我们这里的Linux的版本需要CenterOS7

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbd4b09c832bb927f7cdae946c6aba66f.png)

redis安装目录下有多个命令值得我们先去了解的，这里先列出来了，我们可以先稍微记忆记忆

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc19df4863c96a17f000e92a905143ff7.png)

- 服务器与客户端启动

那么现在我们已经安装好Redis了，下一步自然就是启动Redis，启动的方式有参数启动和配置文件启动，一般主流方式都是配置文件启动。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf48ddcee44a98a11eafb420c81cd4abc.png)

如果我们想要启动Redis，我们可以使用reids-server，如果后续什么都不输入，那么我们就是启动默认端口，也就是6380。如果我们想要指定端口，那么我们就要在后面接上--port 自己指定的端口号

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3820dd7081625204cc75f149e4d170c3.png)

至于客户端连接则是使用redis-cli指令，同样如果后续什么都不写那么就是连接默认端口，如果想要指定连接某个端口就要输入-p 端口号，当然，能连接成功的前提是这个端口号是确实存在的

这里我们要注意一点的是，服务器启动指定端口使用的是--port，而客户端指定端口使用的是-p，不但英文字母的数量不同，而且-的数量也不同

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd736fa4fd10de8dfb622e4ab1face113.png)

最后我们做一些准备工作，比如说创建配置文件的存储目录conf和文件存储目录data等

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf1f1a97835e173bd9a599238551a3cac.png)

- 配置文件启动与常用配置

那么接着我们就来学习用配置文件启动redis，不过在哪之前我们要先对配置文件做一定的处理，我们要先将我们的配置文件进行一定的过滤并更改其中的一部分内容，我们可以输入命令cat redis.conf | grep -v "#" | grep -v "^$" > redis-6379.conf，此意为我们除去配置文件中的所有空格和#号，然后生成一个新的配置文件，其名字就是redis-6379.conf。然后我们进入这个配置文件中看看，配置文件中的内容很多，我们这里只挑一部分讲

首先是bind 127.0.0.1，bind表示我们默认绑定的服务器地址，一旦绑定，那么就不能通过别的方式去访问了，由于我们现在都是使用我们自己的服务器自己连接自己，所以这个其实改不改都无所谓，暂时不用理。

然后是port 6379，其代表指定启动方式时我们默认启动的端口号

接着是timeout 0，其代表超时时间，也即是说我们的客户端连接我们的服务器，多长时间不操作之后，其就会自动断开，这样来节省我们的空间，不过由于我们还是学习阶段，所以其实这一个可以不指定

然后是比较重要的daemonize no，这个比较重要，其代表我们的服务器是否以守护线程的方式启动，如果我们选择是，那么就相当于我们的服务器会默认以后台方式，也就是守护线程的方式启动，此时我们的日志信息就会默认写到我们的日志文件中，而不会直接显示在控制台中。如果我们选否，那么我们的服务器就会以前台方式启动，此时我们一旦启动我们的服务器，我们在控制台上就不能做任何的操作了，只会显示日志文件，必须去另外的控制台上。我们这里推荐以前台方式启动，因为查日志又要我们专门去看，就会很慢，不好使

接着是loglevel notice，这个是日志的等级，这个知识一会再说。logfile ""则是可以指定生成的日志文件的名字

最后是dir ./，这个指定的是我们生成的日志文件的地址，我们这个地址当然要指定到我们的data中

然后我们对我们的配置文件做相应的修改，我们在其中指定我们的日志文件的生成目录，然后日志文件的生成名，我们这里取了有6379关键字的名字，这是因为我们现在使用的就是6379的默认端口，如果以后用别的端口，我们再把日志的名称给改成对应的端口关键字，这样我们好区分，不然啥玩意都放一个日志文件里，这没法整

最后我们可以做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6a8486b03a4f3430540fcea7a7a66374.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5317abac132edc1ab2e351806b6e5c9a.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe4f7b864f0f0070a249f24cb98113151.png)

最后我们可以做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd6c71bde574f395e9e14057659c1724f.png)

- 基本操作

由于我们的Redis是命令行模式的，所以我们首先要学习以下四种命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE36fb721539fc31ad3eaaca77388f241f.png)

首先我们学习最简单的set和get命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd0873f2c14eeab8c38df9e1bc75217d2.png)

如果我们想要获得某个命令的帮助，可以输入help 命令，如果想要获得命令所在的群组的帮助，则要输入help @群组名称

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbb4047502248f42c3168e4480c7b6b34.png)

最后是下边的退出命令行模式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe1fa20082574708b52212797c0b4895e.png)

