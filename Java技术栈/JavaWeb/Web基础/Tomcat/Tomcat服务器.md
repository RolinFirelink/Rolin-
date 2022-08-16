紧接着我们来学期Tomcat服务器，也就是汤姆猫服务器，但是在正式学习之前，我们要先学习服务器的相关知识

服务器也是计算机的一种，不过其比普通计算机要牛逼得多，大多数时候服务器的作用是给普通计算机的用户用于处理数据，服务器还可以连接更多的其他服务器用于提升其数据处理效率

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc291abe62e9d2b144f14087ceb29e2b4.png)

我们常用的应用服务器有weblogic、websphereAS、JBOSSAS以及Tomcat，其中前三者都是重量级服务器（重量级的意思是其实现了所有的JavaEE规范），而Tomcat则是轻量级，其只实现了部分的javaEE规范，但对于我们目前的阶段来说已经很足够了，而且由于其只实现了部分JavaEE规范，因此其本身也比较容易配置，还是开源免费的，非常棒

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc278a31def160b4146460d2f961e1c59.png)

- Tomcat的介绍

关于Tomcat的历史就不多介绍了，重点我们要知道Tomcat的官网地址是https://tomcat.apache.org/，虽然这个猫未免画的未免有些抽象，但无所谓，我们并不关注这个

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2596e478b44a259fdc3d9ba493c4ee78.png)

然后我们来看看Tomcat各个版本所需要的支持，了解这个主要是让我们搞清楚不同的Tomcat版本所需要的对应的JDK版本以及我们后续要学习的其他技术的版本

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcde007024765972d53113e548e73bd68.png)

- Tomcat的基本使用

当然，我们要使用Tomcat的话当然得先下载安装是吧，不过我们这里已经有了对应的Tomcat安装包了，因此我们可以直接拿来用了，不需要下载了。安装的过程就是直接解压

解压之后我们能够看到其安装文件里有许多文件夹，我们一一来讲解其作用，首先bin目录下保存的都是可执行的二进制文件，里面包括Tomcat的启动和停止。其次是conf，该文件夹里存放的都是Tomcat的配置文件，非常重要。然后是lib目录，该目录存放Tomcat运行时所需要的jar包。logs目录只是存放Tomcat的日志文件，temp则是Tomcat的临时文件目录。webapps是Tomcat的应用发布目录，该目录下有个文件夹就代表就一个javaweb应用。最后work则是Tomcat的工作目录

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7d95a7dfc726ae6972c8d56111a7ab43.png)

我们重点要记住bin、conf和webapps这三个目录。接下来我们就来启动Tomcat，我们进入bin目录，打开startup.bat文件就可以成功运行了

如果我们想要确定我们的程序是否真的已经运行了，我们可以进入http://localhost:8080/，如果我们可以打开这个网址，就说明我们的Tomcat已经运行了

如果我们想要关闭我们的Tomcat的话，有两种方式，一种是运行bin目录下的shutdown.bat，另一种是我们直接将Tomcat程序用鼠标关了就完了

接着我们来学习部署我们自己的项目，我们首先进入webapps文件夹，然后创建一个hello目录，然后创建一个html文件，随便写入一个标题（注意即使我们没有所谓的头部尾部，只写入一个一级标题，我们的浏览器仍然能够解析），然后我们再次打开Tomcat，然后还是输入原来的网址，可以看到我们还是进入原来的网址，说明我们已经成功启动了Tomcat，然后我们将网址改为http://localhost:8080/hello/hello.html，此时就可以打开我们之前所创建的网页了（其实这里网址之后输入的就是我们所创建的页面的路径）

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE519fa45842fe1b11411f489adad59cfa.png)

- Tomcat控制台乱码的解决

可以看到虽然我们的Tomcat成功启动了，但是美中不足的是我们的控制台显示的文字居然是乱码的，造成这个问题的原因是字符集不匹配，我们要解决这个乱码的方式也很简单，只要改变Tomcat的字符集就行了。我们首先进入conf文件夹，打开logging.properties，然后定位到第51行，将UTF-8改为gbk就可以了，这样我们的乱码问题就解决了

- IDEA集成Tomcat

我们首先点击idea上方的Run，然后点击Configurations，然后在弹出的界面中点击Defaults，点击Tomcat Server，然后点击Local，然后我们再点击弹出界面的configurations，在Tomcat Home里选择Tomcat对应的路径就可以了（具体过程看视频吧，没啥含金量不提了）

当我们成功集成之后我们会发现我们的idea似乎没有任何变化，不着急哈，这时候我们还需要创建一个对应的企业工程才能看到效果，我们先创建一个新的模块，选择javaEE，选择对应的1.8的JDK版本，然后赋予名字，接着就能创建了一个JAVAEE工程了，这时候我们可以在下方看到一个Tomcat的一个选项卡，后期我们可以通过这个选项卡来找到Tomcat。至于在idea当中JAVAEE的项目和Tomcat到底怎么使用，我们留到后面来讲

- Linux安装Tomcat

根据下图所说的做就完了，没啥特别值得说的（不过这里我的启动程序出了一点问题导致启动不能，这个问题一下子解决不了，先放着吧），如果要发布项目的话，参考Windows里的操作就可以了，在Linux系统中也是一样的，无非是操作方式的变化而已

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE21418dc677bb02212cfd9e458bd5a421.png)

- JavaWEB项目的创建

注意虽然我们之前已经创建过一个JavaEE的项目了，但我们创建那个项目主要是为了展示Tomcat展示卡，现在我们要创建的项目和之前创建的还是有所区别的，我们首先同样创建JavaEE项目，按照同样的过程完成创建，但是完成创建之后我们右键点击该项目，然后点击添加框架，将Web Application框架添加进去就可以了（这里出现的框架不显示的问题真的没把我折磨吐了），这样我们可以看到如下的模块文件内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1d621569538eef088a019829798a9e1a.png)

我们一个个来解释这些文件的作用，首先是src，其作用很简单，就是用来保存我们的java源代码的。web目录核心的作用是用来帮助我们来保存一些相关的资源（比如说HTML，CSS，JS等），我们还可以看到在web目录下有一个index.jsp文件，这个文件是系统自动生成的首页，这个默认首页我们暂时不需要关心。接着我们来看WEB-INF目录，这个目录主要是存放各种配置文件，还有我们后期所需要的jar包，都是放在这个目录下。我们还可以看到在该目录下还有一个web.xml文件，这个配置文件就是我们将来学习Servlet的时候需要编写的文件，目前我们先不用去管它

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcd4de432a960bfeb7466379c3c0a88a3.png)

- IDEA发布项目

你根本不知道我他妈经历了什么才终于搞好这个逼玩意，是真几把痛苦啊。题外话就不多说了，来看看本节内容吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa96636d9c53b0752d014f8433cb64ccb.png)

这里一定要确认自己的Deployment选项中有无war包，无则说明出错了，重来吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE67c7cc0b025e7af845fa4590de4b9122.png)

然后选择tomcat，接着按照下图中对应的选项选就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf3329b90aa5a84dcc4a3c96817d1c20f.png)

最后我们可以运行tomcat来看看效果，如果发布成功的话，则运行tomcat会自动打开我们的浏览器并展示发布项目

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6bb0848a6604664a995a0d3ce4d76aff.png)

这个过程中如果发生了问题，可以参考下面两个网址的教程进行解决

https://www.cnblogs.com/de-ming/p/14545047.html

https://www.icode9.com/content-4-797312.html

上面的方式是通过idea进行项目的发布，我们接着来学习第二种方式，就是通过war包来发布，这种发布方式并不是非常重要，我们只是作为了解就可以了。首先我们要进入项目的web路径下打 一个war包

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc766dca80f552e6394d3bf113d5e67e4.png)

打完之后我们将其该war包放到webapps路径下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcc9431cb1a1a01299d0fbd2070d7a7d2.png)

接着我们只要启动tomcat服务，其会自动解压war包

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5296ea783e98df518745ca5d3436d233.png)

最后我们可以输入对应的网址来验证我们的结果，看看项目是否成功发布了。

其实我们这里的war包就是一个压缩文件，我们可以通过解压缩文件查看里面的内容，会发现里面就有我们项目的内容。我们将war包放到webapps文件夹中，然后运行tomcat，tomcat会自动解压这个war包，然后我们可以输入对应的网址看看项目是否成功发布。

war包的发布方法是手动发布，比较麻烦，我们平时不推荐用

- Tomcat配置文件

在tomcat里有很多的配置文件，这里我们只讲最重要的一个，就是server.xml配置文件，我们打开这个配置文件可以找到如下内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe7d5239a4661613c0efad6edf95f9169.png)

上面的内容里port代表的就是端口号，我们的tomcat默认的是8080端口号，我们要访问其地址必须要手动写8080，这也能解释为什么我们要输入的网址是localhost:8080，后面的8080代表的就是我们的端口号，而前面的localhost则代表我们本机的地址，换成我们的IP地址也是一样的效果。那为什么我们平时输入的网址就不需要手动输入这些端口号呢？这是因为我们平时输入的网址是http协议，其采用的是80端口，该端口是不要输入端口号的，我们后续如果要发布项目的话也需要将我们的端口号改为80

如果我们将端口号改为80的话，那么我们输入网址时只需要输入80就可以了，而且输入之后其网址也不会再显示80的端口号了。由于我们目前还处理学习阶段，因此我们就暂时不改动这个端口号了

- Tomcat配置虚拟目录

现在我们要发布项目都必须放在webapps目录下才能发布，那么有没有办法能够让我们在别的目录下也能够发布文件呢？这就需要用到虚拟目录了，假设我们将原来的项目复制到d盘根目录下并命名为my，那么我们就要找到server.xml文件，添加上下图所示的内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf6ed0f4d7c945fea65a4462d8fb0db25.png)

path是我们在网址里填写的路径，而docBase则是我们的文件的真实存放的路径。当我们设置完成之后，我们只需要输入http://localhost:8080/my/就可以成功访问到我们的项目了。虚拟目录的好处自然是我们可以用于发布任意目录下的项目，不过我们一般还是选择将项目放到一个位置下的，因此这个方法只作为了解就好了

- Tomcat配置虚拟主机

如果我们希望我们可以通过某一个指定的网址格式来访问我们的项目而不是输入难看的localhost的话，那么我们利用tomcat配置虚拟主机来达成这个目的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5aafc7039ade819c3cdcb55a2544cae7.png)

首先我们要进入server.xml配置文件，找到<Engine>标签，然后写入上图所示的内容。这里name是我们要指定的网址的名词，而appBase则是我们的项目实际存放的路径，unpackWARs则是是否自动解压war包，true则为是，而autoDeploy则是是否自动发布项目，true则为是

完成了之后我们还需要修改hosts文件，将我们的网址与虚拟主机绑定，这里的127.0.0.1是一个通用的ip地址，在任何电脑上其都表示当前这台电脑

当我们进入server.xml文件中，我们能在<Engine>标签中找到下面的内容

```
  <Host name="localhost"  appBase="webapps"
        unpackWARs="true" autoDeploy="true">
```

看到这个内容，我想大家伙们应该就明白为什么我们当初要写localhost来访问了，这是因为其在内部指定的名称就是localhost

然后我们进行相应的修改，接着进入C盘，windows，drivers，etc，然后选中hosts，接着写入上图的内容，然后我们只要输入www.webdemo.com:8080就可以访问到我们的项目了，能访问到就说明我们的虚拟主机已经配置成功了（这里之所以还要输入8080端口号是因为我们的端口号此时还没有设置为80，如果设置为80，那么我们就不必写端口号了，此时就和普通的网址没有分别了）