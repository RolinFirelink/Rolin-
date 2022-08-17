# 企业开发简介

- 企业开发简介

那么现在我们正式到企业版开发的学习（终于要开张了兄弟们，再也不是捣鼓玩具了），在正式学习之前，我们要先了解关于企业开发的相关知识

所谓Java企业版，其实就是JavaEE，其实Java Enterprise Edition的简称，早期称为J2EE，后面叫做JavaEE，JavaEE规范是很多Java开发技术的总称，一共包括13个技术规范，这里面大多都是我们后面所需要学习的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6820c8dcbe5352f3bcf6894cfa6db2dd.png)

## web概述和资源分类

WEB就是网络，网络相关的技术是为了让我们在网络世界中获取资源，而资源的存放地我们就将其称之为网站，简而言之，网站其实就是一个存放资源的容器

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEac201da8e4623988973c00088e9a5d6e.png)

接下来我们来讲讲资源的分类，网站中的资源总分类静态资源和动态资源，对于给予给任何人任何时间看到的内容都是一样的资源那么就是静态资源，比如我们编写的HTML、CSS

、JavaScript都属于静态资源。而当网站中展示的资源是会因为各种因素而变化时，其就会动态资源，比如我们编写的JSP、Servlet就属于动态资源

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5d318368f92feccb8e13ba9c8221fbd1.png)

## 系统结构的介绍

接下来我们来学习系统结构，我们之前开发的都是Java工程，这些工程在企业中被称为项目或者产品，其都是具有系统结构的。系统架构有三种三分方式，可以根据基础结构划分，也可以根据技术选型划分，还可以根据部署方式划分，根据基础结构划分可以将架构分为CS结构和BS结构，我们目前重点要学习的就是这两种结构

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2bc00c542e707bf4971a614673f30e40.png)

我们先来讲讲CS结构，CS结构是采用客户端+服务器的方式，服务器用于接受客户端的数据并进行对应的处理然后返回给客户端，客户端则是用户操作的并得到反馈的平台，在这个过程里Internet充当了中间对象，将服务器和客户端联系起来。这种方式的好处在于客户端安装好之后其网络占用或者是资源消耗相对较少，但是客户端的更新与维护往往还需要耗费企业大量的心力

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe413b280594dfe13bf49ff02963e0f18.png)

接下来我们来讲讲BS结构，BS结构是通过浏览器+服务器的方式来访问对应的服务器的，Internet也充当了中间人的身份，与CS不同的是，其只需要一个浏览器就可以访问到各种不同的服务器，而且也不需要对网站做过多的更新与维护，当然其本身也不是没有缺点的，其缺点我们先按下不表

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc291abe62e9d2b144f14087ceb29e2b4.png)

CS和BS结构都是我们现实生活中比较常接触到的结构，这两种结构各有利弊，采用怎样的结构往往取决于我们有怎么样的需求，但是今天我们主要是要来学习BS结构

# Tomcat服务器

紧接着我们来学期Tomcat服务器，也就是汤姆猫服务器，但是在正式学习之前，我们要先学习服务器的相关知识

服务器也是计算机的一种，不过其比普通计算机要牛逼得多，大多数时候服务器的作用是给普通计算机的用户用于处理数据，服务器还可以连接更多的其他服务器用于提升其数据处理效率

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcd4ec857f3c83df60d69baafb5bc3261.png)

我们常用的应用服务器有weblogic、websphereAS、JBOSSAS以及Tomcat，其中前三者都是重量级服务器（重量级的意思是其实现了所有的JavaEE规范），而Tomcat则是轻量级，其只实现了部分的javaEE规范，但对于我们目前的阶段来说已经很足够了，而且由于其只实现了部分JavaEE规范，因此其本身也比较容易配置，还是开源免费的，非常棒

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc278a31def160b4146460d2f961e1c59.png)

## Tomcat的介绍

关于Tomcat的历史就不多介绍了，重点我们要知道Tomcat的官网地址是https://tomcat.apache.org/，虽然这个猫未免画的未免有些抽象，但无所谓，我们并不关注这个

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2596e478b44a259fdc3d9ba493c4ee78.png)

然后我们来看看Tomcat各个版本所需要的支持，了解这个主要是让我们搞清楚不同的Tomcat版本所需要的对应的JDK版本以及我们后续要学习的其他技术的版本

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcde007024765972d53113e548e73bd68.png)

## Tomcat的基本使用

当然，我们要使用Tomcat的话当然得先下载安装是吧，不过我们这里已经有了对应的Tomcat安装包了，因此我们可以直接拿来用了，不需要下载了。安装的过程就是直接解压

解压之后我们能够看到其安装文件里有许多文件夹，我们一一来讲解其作用，首先bin目录下保存的都是可执行的二进制文件，里面包括Tomcat的启动和停止。其次是conf，该文件夹里存放的都是Tomcat的配置文件，非常重要。然后是lib目录，该目录存放Tomcat运行时所需要的jar包。logs目录只是存放Tomcat的日志文件，temp则是Tomcat的临时文件目录。webapps是Tomcat的应用发布目录，该目录下有个文件夹就代表就一个javaweb应用。最后work则是Tomcat的工作目录

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7d95a7dfc726ae6972c8d56111a7ab43.png)

我们重点要记住bin、conf和webapps这三个目录。接下来我们就来启动Tomcat，我们进入bin目录，打开startup.bat文件就可以成功运行了

如果我们想要确定我们的程序是否真的已经运行了，我们可以进入http://localhost:8080/，如果我们可以打开这个网址，就说明我们的Tomcat已经运行了

如果我们想要关闭我们的Tomcat的话，有两种方式，一种是运行bin目录下的shutdown.bat，另一种是我们直接将Tomcat程序用鼠标关了就完了

接着我们来学习部署我们自己的项目，我们首先进入webapps文件夹，然后创建一个hello目录，然后创建一个html文件，随便写入一个标题（注意即使我们没有所谓的头部尾部，只写入一个一级标题，我们的浏览器仍然能够解析），然后我们再次打开Tomcat，然后还是输入原来的网址，可以看到我们还是进入原来的网址，说明我们已经成功启动了Tomcat，然后我们将网址改为http://localhost:8080/hello/hello.html，此时就可以打开我们之前所创建的网页了（其实这里网址之后输入的就是我们所创建的页面的路径）

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE519fa45842fe1b11411f489adad59cfa.png)

## Tomcat控制台乱码的解决

可以看到虽然我们的Tomcat成功启动了，但是美中不足的是我们的控制台显示的文字居然是乱码的，造成这个问题的原因是字符集不匹配，我们要解决这个乱码的方式也很简单，只要改变Tomcat的字符集就行了。我们首先进入conf文件夹，打开logging.properties，然后定位到第51行，将UTF-8改为gbk就可以了，这样我们的乱码问题就解决了

## IDEA集成Tomcat

我们首先点击idea上方的Run，然后点击Configurations，然后在弹出的界面中点击Defaults，点击Tomcat Server，然后点击Local，然后我们再点击弹出界面的configurations，在Tomcat Home里选择Tomcat对应的路径就可以了（具体过程看视频吧，没啥含金量不提了）

当我们成功集成之后我们会发现我们的idea似乎没有任何变化，不着急哈，这时候我们还需要创建一个对应的企业工程才能看到效果，我们先创建一个新的模块，选择javaEE，选择对应的1.8的JDK版本，然后赋予名字，接着就能创建了一个JAVAEE工程了，这时候我们可以在下方看到一个Tomcat的一个选项卡，后期我们可以通过这个选项卡来找到Tomcat。至于在idea当中JAVAEE的项目和Tomcat到底怎么使用，我们留到后面来讲

## Linux安装Tomcat

根据下图所说的做就完了，没啥特别值得说的（不过这里我的启动程序出了一点问题导致启动不能，这个问题一下子解决不了，先放着吧），如果要发布项目的话，参考Windows里的操作就可以了，在Linux系统中也是一样的，无非是操作方式的变化而已

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE21418dc677bb02212cfd9e458bd5a421.png)

## JavaWEB项目的创建

注意虽然我们之前已经创建过一个JavaEE的项目了，但我们创建那个项目主要是为了展示Tomcat展示卡，现在我们要创建的项目和之前创建的还是有所区别的，我们首先同样创建JavaEE项目，按照同样的过程完成创建，但是完成创建之后我们右键点击该项目，然后点击添加框架，将Web Application框架添加进去就可以了（这里出现的框架不显示的问题真的没把我折磨吐了），这样我们可以看到如下的模块文件内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1d621569538eef088a019829798a9e1a.png)

我们一个个来解释这些文件的作用，首先是src，其作用很简单，就是用来保存我们的java源代码的。web目录核心的作用是用来帮助我们来保存一些相关的资源（比如说HTML，CSS，JS等），我们还可以看到在web目录下有一个index.jsp文件，这个文件是系统自动生成的首页，这个默认首页我们暂时不需要关心。接着我们来看WEB-INF目录，这个目录主要是存放各种配置文件，还有我们后期所需要的jar包，都是放在这个目录下。我们还可以看到在该目录下还有一个web.xml文件，这个配置文件就是我们将来学习Servlet的时候需要编写的文件，目前我们先不用去管它

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE67c7cc0b025e7af845fa4590de4b9122.png)

## IDEA发布项目

你根本不知道我他妈经历了什么才终于搞好这个逼玩意，是真几把痛苦啊。题外话就不多说了，来看看本节内容吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcd4de432a960bfeb7466379c3c0a88a3.png)

这里一定要确认自己的Deployment选项中有无war包，无则说明出错了，重来吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa96636d9c53b0752d014f8433cb64ccb.png)

然后选择tomcat，接着按照下图中对应的选项选就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf3329b90aa5a84dcc4a3c96817d1c20f.png)

最后我们可以运行tomcat来看看效果，如果发布成功的话，则运行tomcat会自动打开我们的浏览器并展示发布项目

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcc9431cb1a1a01299d0fbd2070d7a7d2.png)

这个过程中如果发生了问题，可以参考下面两个网址的教程进行解决

https://www.cnblogs.com/de-ming/p/14545047.html

https://www.icode9.com/content-4-797312.html

上面的方式是通过idea进行项目的发布，我们接着来学习第二种方式，就是通过war包来发布，这种发布方式并不是非常重要，我们只是作为了解就可以了。首先我们要进入项目的web路径下打 一个war包

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6bb0848a6604664a995a0d3ce4d76aff.png)

打完之后我们将其该war包放到webapps路径下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc766dca80f552e6394d3bf113d5e67e4.png)

接着我们只要启动tomcat服务，其会自动解压war包

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf6ed0f4d7c945fea65a4462d8fb0db25.png)

最后我们可以输入对应的网址来验证我们的结果，看看项目是否成功发布了。

其实我们这里的war包就是一个压缩文件，我们可以通过解压缩文件查看里面的内容，会发现里面就有我们项目的内容。我们将war包放到webapps文件夹中，然后运行tomcat，tomcat会自动解压这个war包，然后我们可以输入对应的网址看看项目是否成功发布。

war包的发布方法是手动发布，比较麻烦，我们平时不推荐用

## Tomcat配置文件

在tomcat里有很多的配置文件，这里我们只讲最重要的一个，就是server.xml配置文件，我们打开这个配置文件可以找到如下内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe7d5239a4661613c0efad6edf95f9169.png)

上面的内容里port代表的就是端口号，我们的tomcat默认的是8080端口号，我们要访问其地址必须要手动写8080，这也能解释为什么我们要输入的网址是localhost:8080，后面的8080代表的就是我们的端口号，而前面的localhost则代表我们本机的地址，换成我们的IP地址也是一样的效果。那为什么我们平时输入的网址就不需要手动输入这些端口号呢？这是因为我们平时输入的网址是http协议，其采用的是80端口，该端口是不要输入端口号的，我们后续如果要发布项目的话也需要将我们的端口号改为80

如果我们将端口号改为80的话，那么我们输入网址时只需要输入80就可以了，而且输入之后其网址也不会再显示80的端口号了。由于我们目前还处理学习阶段，因此我们就暂时不改动这个端口号了

## Tomcat配置虚拟目录

现在我们要发布项目都必须放在webapps目录下才能发布，那么有没有办法能够让我们在别的目录下也能够发布文件呢？这就需要用到虚拟目录了，假设我们将原来的项目复制到d盘根目录下并命名为my，那么我们就要找到server.xml文件，添加上下图所示的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5aafc7039ade819c3cdcb55a2544cae7.png)

path是我们在网址里填写的路径，而docBase则是我们的文件的真实存放的路径。当我们设置完成之后，我们只需要输入http://localhost:8080/my/就可以成功访问到我们的项目了。虚拟目录的好处自然是我们可以用于发布任意目录下的项目，不过我们一般还是选择将项目放到一个位置下的，因此这个方法只作为了解就好了

## Tomcat配置虚拟主机

如果我们希望我们可以通过某一个指定的网址格式来访问我们的项目而不是输入难看的localhost的话，那么我们利用tomcat配置虚拟主机来达成这个目的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5296ea783e98df518745ca5d3436d233.png)

首先我们要进入server.xml配置文件，找到<Engine>标签，然后写入上图所示的内容。这里name是我们要指定的网址的名词，而appBase则是我们的项目实际存放的路径，unpackWARs则是是否自动解压war包，true则为是，而autoDeploy则是是否自动发布项目，true则为是

完成了之后我们还需要修改hosts文件，将我们的网址与虚拟主机绑定，这里的127.0.0.1是一个通用的ip地址，在任何电脑上其都表示当前这台电脑

当我们进入server.xml文件中，我们能在<Engine>标签中找到下面的内容

```
  <Host name="localhost"  appBase="webapps"
        unpackWARs="true" autoDeploy="true">
```

看到这个内容，我想大家伙们应该就明白为什么我们当初要写localhost来访问了，这是因为其在内部指定的名称就是localhost

然后我们进行相应的修改，接着进入C盘，windows，drivers，etc，然后选中hosts，接着写入上图的内容，然后我们只要输入www.webdemo.com:8080就可以访问到我们的项目了，能访问到就说明我们的虚拟主机已经配置成功了（这里之所以还要输入8080端口号是因为我们的端口号此时还没有设置为80，如果设置为80，那么我们就不必写端口号了，此时就和普通的网址没有分别了）

# HTTP协议

学习完了铸币的Tomcat之后，我们现在来进入HTTP协议的学习

## HTTP协议的介绍

首先我们来说说什么是HTTP协议，HTTP是Hyper Text Transfer Protocol的缩写，其意为超文本传输协议，其实基于TCP/IP协议的协议。

超文本即是比普通文本更加强大的文本，而传输协议指的是客户端与服务器端的通信规则，也被称为握手规则

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd574805ce850e7c574452b88fb4d2351.png)

HTTP协议最重要的两个部分就是请求和响应

注意客户端与服务器之间，是由客户端先向服务器发起请求，而服务器响应其请求的，而不会是服务器自顾自响应，客户端接收。假设我们输入bilibili的网址，就相当于我们请求bilibili的服务器，服务器会响应我们的请求。而我们向服务器手动发起网址请求是我们做的，而其他一类图片音像资源则是会自动发起请求的

## HTTP协议的请求

我们之前说过HTTP协议中最重要的两个部分就是请求和响应，我们现在讲讲请求这一部分

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcf94f83ac2c91d3ce55fe4e16335a035.png)

请求一般分为四个部分，分别是请求行，请求头，请求空行和请求体。根据请求的方式不同还分为get请求方式和post请求方式，这两个方式是我们之前学习过的，get请求方式会将用户输入的内容直接显示在地址上，不但不安全而且有长度的限制，而post请求方式则不会，所以一般都使用post请求方式进行请求。这里值得注意的是，只有POST请求方式才有请求体

### GET请求

先来看看get请求方式的例子

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE79305f2a8c5c7a24c6589a11ebca5cca.png)

首先是请求行，请求行中的GET代表请求方式为GET方式，后面login.html代表我们处于的请求网址，而更后面的内容则代表我们的用户输入的内容，HTTP/1.1则分别代表我们的协议名称和目前使用的协议的版本号

接着是请求头的部分，Host代表我们请求的主机。User-Agent则代表我们浏览器的信息， 比如在这里我们能知道用户使用的64位系统，然后使用的是火狐浏览器。Accept则代表支持的资源类型，比如这里可以看到其支持text、html、xml等类型。Accept-Language则代表其支持的语言语言，比如这里能接受的语言有中文中国，中国繁体，英语等。Accept-Encoding则代表其能支持的压缩方式，一般默认是gzip压缩方式。Connection则代表连接状态，这里的keep-alive代表一直处于连接状态。Referer则代表请求来源，其实就相当于是我们在地址栏中输入的完整路径。后面的内容则都是与会话管理与缓存相关的，这些都非常重要，但是这些部分就由我们后期学习到那个内容的时候再来讲

然后是请求空行，其实请求空行就是个回车，主要作用就是将请求头和请求体分开的

最后是请求体的部分，由于我们这里使用的GET方式，因此我们没有请求体

### POST请求

再来看看POST请求方式的例子

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE97e4ad4a00b94c4e7fa716c77fbd3b22.png)

可以看到POST方式也同样有请求行请求头请求空行以及请求体，与GET请求方式不同的是，POST的请求方式将用户输入的内容放置于请求体中

在火狐的开发者工具里可以按F12调用开发者工具，然后可以看到点击网络并点击第一行可以看到我们的内部GET请求方式的具体信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE95058c5b93f9ebd11a24fb2ebc4e1400.png)

同理对于Post方式也是一样的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2e84381e4f47a382167673bd08eed452.png)

这里我们选择了编辑，就可以看到请求主体了

最后我们来做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE59cc45744a41292780eef31b0aba8923.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEef5d7da985910e44b27d621693144897.png)

## HTTP协议的响应

与HTTP的请求类似，HTTP的响应也分为四部分，分别是响应行，响应头，响应空行以及响应体

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa4008b4792354ccfb43d17925df2b8b7.png)

首先是响应行，POST代表的是响应的方式，请求的方式是怎么样的那么响应的方式就是怎么样的，接着HTTP/1.1分别代表的是协议的名词的协议版本，200则是代表成功的意思，OK则代表一切正常

接着是响应体，Accept-Ranges代表的是支持存储的类型，这里支持的单位是bytes字节。ETag则表示当前我们的响应在整个服务器中的一个标识，就相当于是序列号。Last-Modified代表的是当前的资源在服务器中最后的修改时间。Content-Type代表的是我们响应的类型。Content-Length代表的是我们的响应的长度。Date代表日期。Keep-Alive代表我们连接的状态，timeout代表的是一个超时时间。Connection代表我们的连接状态，这里我们的链接状态时要保持连接的。

响应空行的作用与请求空行一样，都是代表一个换行

响应体里则是有相应返回给客户端的资源文件

同样在火狐浏览器里调用开发者工具就可以看到我们的响应体，在响应体窗口上面选择响应栏目，可以看到响应之后获得的原始的代码内容，这里我们就不演示了

最后我们可以做一个总结，首先是关于状态码的一些了解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE16fb15b53d991880a44268c78023c17f.png)

然后是关于响应头的知识总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE99309288a1774f92a7e963d84173e508.png)

最后是响应空行和响应体

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4b060afe61766467643dfd53cda9bbff.png)

# 动态资源案例

接着我们要来学习发布一个静态资源，发布的过程如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7909f2e919a71caf625d0daa2a074dd6.png)

总之我搞了一堆终于是搞定了，中间的辛酸就不多说了，大概我们创建一个新的WEB项目再稍作关联就可以了，不多谈。

## 案例效果

在下图的案例效果中，crm是我们的一个虚拟的目录名称，而studentServlet则是我们自己编写的一个功能类，其并不是一个网址，当我们在网址栏输入这些时，会在控制台上打印出，这是我的第一个servlet入门案例。这就是我们的servlet效果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe08c0d9e3b83b4d2ef9f645a0f8a02fc.png)

要实现上面的案例效果就要靠servlet，那么什么是servlet呢？其实Servlet就是运行在Java服务器端的程序，用于接收和响应客户端基于HTTP协议的请求。其次，如果我们想要实现Servlet的功能，可以通过实现javax.servlet.Servlet接口或者继承其实现类。Servlet的核心方法是service()，任何客户端的请求都会经过该方法，我们在控制台输入的内容其实就可以写在该方法内

## Servlet介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3abe4ea7d329828b1eb996c64e9f8d53.png)

## 动态资源的发布

那么学习完Servlet相关知识之后，我们现在正式去发布动态资源。这里我们创建一个对应的JAVAEE工程，然后将对应的案例文件导入到对应的web包下，然后我们创建一个lib包，然后恩导入所有需要的jar包，接着添加add as library，然后我们要先关联我们的首页，进入web.xml文件，写入如下代码

```
<!--修改默认主页-->
<welcome-file-list>
    <welcome-file>/html/frame.html</welcome-file>
</welcome-file-list>
```

上面的代码能够讲述我们的tomcat打开的首页定位到我们的自己创建的简易系统中

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbcc42530a9797b83a5f323475edf05bb.png)

可以看到我们的这个系统除了那个女人脸太几把诡异了之外一切都好哦，那么到此为止我们的动态资源就算是简单发布完毕了，但是其中还有很多功能没有实现，我们后续来实现其功能

现在我们来实现我们案例里最开始要的效果，我们先在src包下创建一个com.itheima.servlet包，然后创建一个StudentServlet类，令其实现Servlet类，然后在最重要的Servlet()类中写入输出语句，根据思路我们可以构造代码如下

```
package com.itheima.servlet;

import javax.servlet.*;
import java.io.IOException;

/*
    Servlet入门程序
 */
public class StudentServlet implements Servlet {

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    /*
        所有的客户端请求都会通过这个方法
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("这是我的第一个servlet入门案例...");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```

其他的方法我们都还不用管，接着我们在web.xml类中写入如下内容

```
<!--servlet声明-->
<servlet>
    <servlet-name>studentServlet</servlet-name>
    <servlet-class>com.itheima.servlet.StudentServlet</servlet-class>
</servlet>

<!--servlet映射-->
<servlet-mapping>
    <servlet-name>studentServlet</servlet-name>
    <url-pattern>/studentServlet</url-pattern>
</servlet-mapping>
```

这里我们先写一个servlet声明，声明中我们要写入一个名字，其实可以随意写，但是这里我们写入和类名一样的studentServlet，然后在下一行内容中写上类真正的地址，接着我们继续写servlet映射，我们同样写入名字，但是这里要注意我们写入的第二个名字必须和我们第一个写入的名字保持一致，接着我们下一个也写入一个内容，这个内容写什么主要取决于我们在浏览器请求时想要在网址中写入什么，这里我们还是写原来的内容

那么上面的内容搞定之后，我们启动tomcat，然后输入localhost:8080/crm/studentServlet就可以在控制台中输入我们写入的语句了，之所以会输出，是因为我们的网址向服务器做了一次请求，然后这个请求会通过service方法，会执行里面的语句。接着让我们来看看这个结果的执行过程

## 内部执行过程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaa72b21eb89c0122b54c50ac2fce01a4.png)

当我们在网址中输入studentServlet时，我们程序会先进入到映射中寻找是否有这个语句的url-patter，若有，则根据这个语句寻找到其上一位的名字，根据这个名字到配置servlet语句的名字处，接着寻找到期下一行的具体的类的位置，然后执行这个类的方法，在控制台中输出语句

最后这是我们这个案例的web.xml的代码

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <!--修改默认主页-->
    <welcome-file-list>
        <welcome-file>/html/frame.html</welcome-file>
    </welcome-file-list>
    
    <!--servlet声明-->
    <servlet>
        <servlet-name>studentServlet</servlet-name>
        <servlet-class>com.itheima.servlet.StudentServlet</servlet-class>
    </servlet>
    
    <!--servlet映射-->
    <servlet-mapping>
        <servlet-name>studentServlet</servlet-name>
        <url-pattern>/studentServlet</url-pattern>
    </servlet-mapping>
</web-app>
```

这是我们这个案例的StudentServlet类的代码，注意这个类是放在com.itheima.servlet包下的

```
package com.itheima.servlet;

import javax.servlet.*;
import java.io.IOException;

/*
    Servlet入门程序
 */
public class StudentServlet implements Servlet {

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    /*
        所有的客户端请求都会通过这个方法
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("这是我的第一个servlet入门案例...");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

# Servlet

那么现在我们来正式学习Servlet，之前我们只是做过简单的学习，这一章节，我们要来正式学习Servlet，首先自然是对Servlet进行介绍。我们这里提供了JavaEE的帮助文档，我们直接翻阅帮助文档吧

## Servlet介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd5298f2cafee40403aa1a7c986185ecd.png)

可以看到都是英文哈，稍微有那么点痛苦，但无所谓，让痛重叠。

首先Servlet是一个接口，其是处于javax.servlet包下的，其下有两个子接口，分别是HttpJspPage和JspPage。我们要知道这里有两个主要实现类，分别是GenericServlet,HttpServlet类

接着分割线下面的内容是对于这个接口的说明，首先所有的Servlet类都必须实现这个接口。其次是Sevlet是运行在我们的Web服务器上的一个小程序，其核心作用是用于接受和响应我们的Web客户端的基于HTTP协议的请求

如果要实现Servlet功能，除了实现Servlet接口之外，我们还有其他两种方式，分别是继承javax.servlet.GenericServlet或者是继承javax.servlet.http.HttpServlet。总的来说，我们要实现Servlet功能有三种方式

在Servlet接口中我们定义了一些相关的功能，这里面包括一个核心的功能就是service方法，以及我们的初始化和销毁的方法。其实这些方法就介绍了我们的Servlet的一个生命周期，包括生成，调用以及销毁

在下面是一个优先级关系的一个说明，首先Servlet方法初始化时会调用init方法。其次是所有的请求都会通过service方法来进行处理。最后是当servlet从服务器中移除，也就是销毁时，会调用destroyed方法，最终会由垃圾回收器进行回收

最后这个接口还提供了getServletConfig方法，调用该方法可以返回该Servlet的配置信息。还有getServletInfo方法，该方法可以返回Servlet的基本信息，如作者和版权

以上就是本节的所有内容，最后我们来看看中英翻译版本的帮助文档

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf0694bca71ee1eb12b7a7d6b813d7e41.png)

最后我们可以做一个本章总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE33b8d596b72b6830edb0df74269103cc.png)

## Servlet快速入门

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7570451e0abee3dce323b0236f8a424e.png)

如同我们之前采用实现Servlet接口的方法来了解一样，这一次我们同样采用这种方式来进一步加深我们对Servlet的理解，只不过这一次换成继承GenericServlet的方式。在继承之前我们要先来看看GenericServlet的说明

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE939b2b55c70187437a4cc643b764823c.png)

可以看到其是一个抽象类，我们查看其方法会发现其方法特别多，不过其抽象方法只有一个，因此我们如果采用这种方式来实现Servlet，只需要重写其一个抽象方法就可以了，就像这样

```
package com.itheima.servlet;

import javax.servlet.*;
import java.io.IOException;

/*
    Servlet快速入门1
 */
public class ServletDemo01 extends GenericServlet {

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Servlet方法执行了...");
    }
}
```

我们可以看到这个重写的service里有两个参数，这两个参数分别是ServletRequest和ServletResponse，分别是请求和响应，我们以后要实现service里响应客户端的请求，就要通过这两个对象，也就是请求和响应这两个对象来实现

同样我们也要更改我们的web.xml文件，令其关联对应的映射，写入如下关联代码

```
    <!--Servlet快速入门1的配置-->
    <servlet>
        <servlet-name>servletDemo01</servlet-name>
        <servlet-class>com.itheima.servlet.ServletDemo01</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>servletDemo01</servlet-name>
        <url-pattern>/servletDemo01</url-pattern>
    </servlet-mapping>
</web-app>
```

最后我们经过测试我们会发现每次刷新，也就是客户端每次发送请求时，service方法都会执行一次，那么我们的实现就没问题了

## Servlet执行过程

之前我们也讲过Servlet的执行过程，但其实那只是一个非常简易的执行过程，其实其实际的执行过程要更为复杂。首先，在讲解之前我们要先知道我们的全地址为localhost:8080/demo1/servletDemo1

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe198f6055994518eaa966fa279316d8e.png)

其执行过程一共分为五部分，我们的客户端浏览器，接着是Tomcat服务器，然后是servlet_demo1，也就是我们自己创建的项目，接着是web.xml，也就是我们的项目的配置文件，最后是ServletDemo01，这是我们自己编写的功能类

其执行过程的第一步是由客户端浏览器向Tomcat浏览器发起请求，当Tomcat接收到这个请求之后，Tomcat会解析这个请求地址，也就是URL，其地址是我们在浏览器中输入的demo1的位置的地址。第三步Tomcat会根据其对应的地址来找到对应的应用。当初我们在Tomcat配置文件里写入的虚拟目录是/demo1并绑定了一个war包，那么当这个地址传入时其就会根据这个地址找到这个war包的对应内容并打开

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6ac719e7b51f0f9dcbb2b39929a3c694.png)

第四步是找到web.xml的项目的配置文件，找到之后会解析请求的资源地址（URI），什么是请求的资源地址呢？其实就是我们在浏览器里写入的servletDemo01，之后发生的过程就正如我们上一节叙述的那样，通过我们在配置文件里写好的映射找到其对应的实现类，然后执行service方法了，这个方法会相应给客户端浏览器。当然我们这里的客户端浏览器什么都不显示，但其实是响应了的，因为我们的控制台里输出了对应的语句，而且如果地址不对，那么浏览器也会报错，既然没有报错也就说明一切ok

## Servlet关系视图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7aed0dcf0e1335777baf3e9d65a15b61.png)

Servlet接口自己关联两个接口，这两个接口分别是ServletConfig和ServletContext。Servlet接口被GenericServlet抽象类部分实现，同时他们都实现了ServletRequest和ServletResponse接口，而HttpServlet抽象类则是继承GenericServlet抽象类的抽象类，其有两个接口，分别是HttpServletRequest和HttpServletResponse接口。最后这个三个类都是有service方法的，无非是传入的参数有所不同罢了

## 继承HttpServlet实现Servlet

Servlet实现的方式一共有三种，我们此前已经实现了前面两种，现在我们来实现第三种，继承HttpServlet抽象类的方式来实现Servlet

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc3f5966eae75450af049cf7f2467cf27.png)

我们先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE49d439d9c4a0b9b95c0c40248ba61c61.png)

这个其实和我们之前做过的事差不多了，我们就不多提了，我直接进行一个类的写。但是当我们写完类继承了HttpServlet之后发现他还就那个不需要重写任何方法，而且步骤里也只要求我们重写doGet和doPost方法，为啥就不用重写Servlet方法呢？不是说所有的请求响应都要经过Servlet方法吗？别急，我们来看看HttpServlet类的service方法

```
public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
    HttpServletRequest request;
    HttpServletResponse response;
    try {
        request = (HttpServletRequest)req;
        response = (HttpServletResponse)res;
    } catch (ClassCastException var6) {
        throw new ServletException("non-HTTP request or response");
    }

    this.service(request, response);
}
```

可以看到这个方法先将ServletResponse和ServletRequest两个对象进行强制类型转换，然后调用另外一个重载的service方法，我们继续看这个重载的service方法

```
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    String method = req.getMethod();
    long lastModified;
    if (method.equals("GET")) {
        lastModified = this.getLastModified(req);
        if (lastModified == -1L) {
            this.doGet(req, resp);
        } else {
            long ifModifiedSince;
            try {
                ifModifiedSince = req.getDateHeader("If-Modified-Since");
            } catch (IllegalArgumentException var9) {
                ifModifiedSince = -1L;
            }

            if (ifModifiedSince < lastModified / 1000L * 1000L) {
                this.maybeSetLastModified(resp, lastModified);
                this.doGet(req, resp);
            } else {
                resp.setStatus(304);
            }
        }
    } else if (method.equals("HEAD")) {
        lastModified = this.getLastModified(req);
        this.maybeSetLastModified(resp, lastModified);
        this.doHead(req, resp);
    } else if (method.equals("POST")) {
        this.doPost(req, resp);
    } else if (method.equals("PUT")) {
        this.doPut(req, resp);
    } else if (method.equals("DELETE")) {
        this.doDelete(req, resp);
    } else if (method.equals("OPTIONS")) {
        this.doOptions(req, resp);
    } else if (method.equals("TRACE")) {
        this.doTrace(req, resp);
    } else {
        String errMsg = lStrings.getString("http.method_not_implemented");
        Object[] errArgs = new Object[]{method};
        errMsg = MessageFormat.format(errMsg, errArgs);
        resp.sendError(501, errMsg);
    }

}
```

可以看到这个重载的方法先判断我们的的请求方式是什么方式，如果是GET的方式就调用doGet方法，如果是POST就调用doPost方法，还有其他很多的请求方式，但那些不是重点我们就不做讲解了。

从上我们可以知道其实自动调用service方法的，而service方法会调用doGet或doPost方法，因此我们只要重写这两个方法就可以了。那么我们可以创建一个ServletDemo02类继承HttpServlet并将方法重写如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    Servlet快速入门2
 */
public class ServletDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("方法执行了");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

然后配置相应的配置文件

```
<!--Servlet快速入门2的配置-->
<servlet>
    <servlet-name>servletDemo02</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo02</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo02</servlet-name>
    <url-pattern>/servletDemo02</url-pattern>
</servlet-mapping>
```

最后我们经过测试会发现的确没有什么问题，这说明我们用这个方法创建Servlet对象已经成功了

最后我们要记住，第三种方式，这就是这个方式，是我们实现Servlet的最常用的方式

## Servlet生命周期

Servlet存在生命周期，所谓生命周期则是对象创建到销毁的过程。当请求第一次到达Servlet时，对象就会创建出来并初始化成功，然后给Servlet对象就会放置于内存中。服务器提供服务的整个过程中，该对象一直存在并且每次都是执行service方法，而当服务停止或者服务器宕机时，该对象会调用销毁方法，调用该方法会使对象死亡。我们可以看到Servlet对象只会创建一次，销毁一次。每次只有一个实例，且是应用中的唯一存在，我们称这种模式为单例模式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE35f95f92d28ef2f538679130721832d5.png)

我们如果要测试这个生命周期也很简单，我们已知Servlet对象创建会调用init方法，死亡会调用destroy方法，那么我们可以创建一个ServletDemo03类继承HttpServlet并将方法重写如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    Servlet快速入门3
 */
public class ServletDemo03 extends HttpServlet {

    //对象出生的过程
    @Override
    public void init() throws ServletException {
        System.out.println("Servlet has born");
    }

    //对象服务的过程
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("jie shou dao le ke hu duan de qing qiu");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }

    //对象销毁的过程
    @Override
    public void destroy() {
        System.out.println("Servlet has destroy");
    }
}

```

接着配置相应的文件

```
<!--Servlet快速入门3的配置-->
<servlet>
    <servlet-name>servletDemo03</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo03</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo03</servlet-name>
    <url-pattern>/servletDemo03</url-pattern>
</servlet-mapping>
```

然后我们经过测试会发现的确也没有什么问题，说明的确其生命周期就与我们想得是一样的

## Servlet线程安全问题

前面我们说过Servlet是单例模式的，那么其实线程安全的吗？为了测试其是否是线程安全的，我们要来构造一个新的ServletDemo04并写入代码如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/*
    Servlet线程安全的问题
 */
public class ServletDemo04 extends HttpServlet {
    //1.定义一个用户名的成员变量
    private String username;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //2.获取用户名
        username = req.getParameter("username");

        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        //3.将用户名响应给浏览器
        PrintWriter pw = resp.getWriter();
        pw.println("welcome"+username);
        pw.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

上面的代码里我们写入了一个成员变量，然后获取其用户名，然后将该用户名打印在浏览器中。至于这个功能是怎么实现的，里面的方法到底是个什么原理，这个暂时不需要我们管

接着我们写入配置信息

```
<!--Servlet线程安全-->
<servlet>
    <servlet-name>servletDemo04</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo04</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo04</servlet-name>
    <url-pattern>/servletDemo04</url-pattern>
</servlet-mapping>
```

搞定了之后，启动tomcat，我们开启两个浏览器，分别填上http://localhost:8080/crm2/servletDemo04?username=aaa和http://localhost:8080/crm2/servletDemo04?username=bbb

我们期望的是其会在两个不同的浏览器上打印出aaa和bbb，但实际的结果，却是两个都是bbb。之所以会这样，是因为当我们的谷歌浏览器进入到我们的程序时，username的值被改为aaa，之后其进入休眠，然后火狐进入将其改为bbb，然后两个获取到的username就都是bbb了。从这个例子里我们也能够知道，tomcat是线程不安全的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5eb08a52a5d4365e7b7e2103996981bd.png)

那么我们要怎么解决这个线程不安全的问题呢？一个简单的想法当然就是加入线程同步关键字，但是我们还有其他效率更高的想法，比如说我们可以将变量定义为局部变量而非成员变量，又或者是我们判断进入的线程是否会修改值，如果不会那么我们就没必要执行线程同步，会的话我们再执行线程同步

先来说说第一种将变量定义为局部的变量的方法，这个方法的内容很简单，将username定义在方法内就可以了

而使用synchronized关键字实现线程安全的方法也很简单，我们可以将代码改造如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/*
    Servlet线程安全的问题
 */
public class ServletDemo04 extends HttpServlet {
    //1.定义一个用户名的成员变量
    private String username;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        synchronized (this){
            //2.获取用户名
            username = req.getParameter("username");

            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            //3.将用户名响应给浏览器
            PrintWriter pw = resp.getWriter();
            pw.println("welcome"+username);
            pw.close();
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

我们将所有可能修改内容的代码都加入线程同步中，然后锁对象就用这一个类的对象，其正好是唯一的

## Servlet不同映射方式

我们的Servlet其实一共有三种映射方式，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEff6b0048a11f5860972e1f95397ba2a0.png)

我们之前一直都在使用第一种映射方式，接下来我们来尝试下后两种映射方式，先来第一种吧

```
<servlet>
    <servlet-name>servletDemo04</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo04</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo04</servlet-name>
    <url-pattern>/servletDemo04</url-pattern>
</servlet-mapping>
```

第一种映射方式其访问资源的路径必须和映射配置完全相同，其最精确同时优先级也最高，这也意为着我们如果使用这种方式来访问我们对应的功能类，那么我们必须要输入完整的类名路径，比方说我们这里想要访问servletDemo04，那么我们就要输入http://localhost:8080/crm2/servletDemo04，其中http://localhost:8080/crm2的路径是我们事前设置的虚拟路径，无论是在何种映射方式下都是不可缺少不可省略的

接下来我们来讲讲第二种映射方式，请看代码

```
<servlet>
    <servlet-name>servletDemo04</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo04</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo04</servlet-name>
    <url-pattern>/servlet/*</url-pattern>
</servlet-mapping>
```

有进行修改的只有第七行，我们将后面的改为*，*代表通配符，而前面的servlet则是我们自己定义的开头。这种方式的精确性不高，但是普适性好，采用这种映射方式只要用户前面有输入http://localhost:8080/crm2/servlet/这个地址，后面的地址随便写什么都是可以运行的，甚至可以不写

接着我们来讲第三种映射方式，请看代码

```
<!--Servlet线程安全-->
<servlet>
    <servlet-name>servletDemo04</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo04</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo04</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

这种方式的普适性最高，精确性最差，同时优先级也最低，同时使用这种方式不需要在映射前加入/，还算比较方便

## Servlet多路径映射

那么我们刚刚学习的映射在具体场景上有什么用呢？我们本章就来解决这个问题，请看下图所需的需求

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4be32eee4855f4ab9e62f667a0075a70.png)

那么一个简单的想法就是使用第二种映射方式来实现这个需求，我们可以让服务器每次响应客户端的请求时判断最后截取的字符是vip还是vvip，通过这个来给商品进行对应的打折。那么我们首先可以构造该类的代码如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ServletDemo06 extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.定义一个商品金额
        int money = 1000;
        
        //2.获取访问资源路径
        String path = req.getRequestURI();
        path = path.substring(path.lastIndexOf("/"));
        
        //3.条件判断
        if("/vip".equals(path)){
            System.out.println("商品原价为："+money+"优惠后是："+(money*0.9));
        }else if("/vvip".equals(path)){
            System.out.println("商品原价为："+money+"优惠后是："+(money*0.5));
        }else {
            System.out.println("商品原价为："+money);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

设置配置文件代码如下

```
<!--Servlet多映射的配置-->
<servlet>
    <servlet-name>servletDemo06</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo06</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo06</servlet-name>
    <url-pattern>/itheima/*</url-pattern>
</servlet-mapping>
```

## Servlet的创建时机

Servlet的创建时机其实有两个，一个是我们的之前使用的当服务器接收到客户端的请求的时候创建，第二种方式是在服务器加载好的时候就创建。前者能够减少内存浪费提供服务器启动效率，其弊端是如果要在加载时做初始化动作，那么是无法完成的。而后者是提前创建好对象，可以提高首次执行效率，也可以完成一些应用加载时就需要做的初始化操作，而其弊端是对服务器的内存占用会比较多，而且会影响服务器的启动效率

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE02af79040c9f213e6812a89a76d36f44.png)

如果我们想要修改Servlet的创建时机，可以在配置文件的<servlet>标间中添加<load-on-startup>标签，这个标签中间只能写数字，数字越小则代表优先级越高，如果写入负整数或者不写则默认代表是第一次访问时被创建。我们之前是没有写的，所以其默认是第一次访问时被创建

这个时候就有一个问题，我们的Servlet对象不是只有一个的吗？为什么还有优先级的说法呢？这是因为后面我们可能会创建多个Servlet的，如果他们都要在启动时被创建出来，那么我们就应该给其分配优先级

## 默认Servlet

Tomcat的conf目录中的web.xml中配置了一个默认的Servlet，供给与用户使用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1e72c1513f2f287b9136331b27be4867.png)

其映射路径是/，简单来说就是当用户发送请求时，会优先寻找用户配置的Servlet，如果找不到，那么就去寻找默认的Tomcat，这个默认的Tomcat会将没有找到对应的网页的信息返回给用户。这个知识只做了解

# ServletConfig

## ServletConfig的介绍

首先ServletConfig是Servlet的配置参数对象，在Servlet的规范中，允许每一个Servlet都提供一些初始化的配置，因此每一个Servlet都有自己的一个ServletConfig。

其作用是在Servlet初始化时将一些配置信息传给Servlet，其传递的数据的方式是采用键值对的形式，而且其生命周期和Servlet相同

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE065ff536d8a55495677bb11e64da69b0.png)

## ServletConfig的配置方式

ServletConfig的配置方式是通过<init-param>标签类配置，其下有两个字标签，分别用于初始化参数的key和value。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE986bb383e2660cb99d3216f1793e6145.png)

假设我们创建一个ServletConfigDemo类并继承Http类，接着实现其doGet和dopost方法，那么接着我们想要配置ServletConfig的话就要到对应的配置文件中去配置，我们可以在配置文件中写入配置代码如下

```
<!--配置Servlet-->
<servlet>
    <servlet-name>servletConfigDemo</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo</servlet-class>
    <!--配置ServletConfig-->
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>desc</param-name>
        <param-value>This is ServletConfig</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>servletConfigDemo</servlet-name>
    <url-pattern>/servletConfigDemo</url-pattern>
</servlet-mapping>
```

这里我们配置了两个ServletConfig， 需要注意的是如果我们要配置多个ServletConfig，那么我们就要配置写入多个</init-param>标签，并在标签中配置对应信息，其中<param-name>是参数名称，<param-value>是参数的值。还有就是ServletConfig是给Servlet提供配置信息的，因此我们的配置信息必须要写在<servlet>标签中

## ServletConfig的常用方法

ServletConfig的作用与他内部的方法息息相关，因此本章我们来学习下ServletConfig的常用方法，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3940fbfd22397c1bd6b879c3341b7037.png)

上面这四个方法里，第一个getInitParameter是可以根据参数的名称来获得参数的值。第二个getInitParameterNames方法可以获取所有参数名称并以枚举类型返回，我们可以将这个方法与第一个方法结合来获得ServletConfig中的所有参数的名字和其值。而getServletName方法就是获取Servlet的名字，最后一个getServletContext可以获取ServletContext对象，但是这个内容我们还没学习到，因此我们这里只做简单的了解

我们都知道Servlet对象创建时必然会调用doget或者dopost方法，因此我们可以在这两个方法里做文章，来测试ServletConfig的常用方法，但是测试其常用方法之前，我们必须处理一个问题，那就是我们必须获得ServletConfig对象，我们知道Servlet方法是必然要调用init方法来初始化的，我们就可以通过这个方法来获得我们的init对象

```
package com.itheima.servlet;

import javax.servlet.ServletConfig;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;

/*
    ServletConfig的演示
 */
public class ServletDemo extends HttpServlet {

    //1.声明ServletConfig
    private ServletConfig config;

    //2.通过init方法，来对ServletConfig对象进行赋值
    @Override
    public void init(ServletConfig config) throws ServletException {
        this.config = config;
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //3.演示ServletConfig常用方法
        //根据key获取value
        String encodingValue = config.getInitParameter("encoding");
        System.out.println(encodingValue);

        //获取所有的key
        Enumeration<String> keys = config.getInitParameterNames();
        while (keys.hasMoreElements()){
            //获取每一个key
            String key = keys.nextElement();
            //根据key获取value
            String value = config.getInitParameter(key);

            System.out.println(key+","+value);
        }

        //获取Servlet的名称
        String servletName = config.getServletName();
        System.out.println(servletName);

        //获取ServletContext对象
        ServletContext servletContext = config.getServletContext();
        System.out.println(servletContext);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}

```

最后我们通过测试也能够在控制台上获得我们想要的结果

# 注解开发

我们以前都是基于配置文件进行开发的，我们一旦创建一个类，就要在配置文件上添加一个映射，就突出一个麻烦，实际上现在有更加好的替代方式了，就是基于注解开发，这个内容其实我们当初在实现图书馆管理系统的时候就已经有所了解了，但是现在我们就要正式进行学习了。更多的相关内容请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9080c5c92f85c6ace7a1cfd75944a6b5.png)

## 自动注解开发Servlet

注解开发也分两种，分别是自动注解开发和手动注解开发，后者只做了解，因为都有自动注解开发了谁几把还用手动注解开发啊。先来看看开发的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE176bae1dc850834195a735078346d6be.png)

我们先创建一个新的project项目，这样方便些，然后因为我们内置的jar包并不足够，比如HttpServlet类就不在我们的idea的内置jar包中，因此我们创建一个lib的文件夹将所有的jar包引入，然后选中所有的jar包，右键点击添加add as library...，然后我们就可以使用我们的所有jar包的内容了。这其实也是我们做项目的时候经常要做的一件事，最初我们做图书馆管理系统的时候其实也干过这事

然后我们创建一个JAVAEE项目，选择JAVAEE8，创建一个测试类，然后写入如下代码

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    基于注解方式开发Servlet
 */
@WebServlet("/servletDemo01")
public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("ServletDemo01执行了...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

然后我们点击run里的edit configuration，配置下tomcat然后启动，再地址栏上输入servletDemo01，然后我们能看到控制台上正确打印了doget方法里面的字符串，这说明我们已经关联成功了。这和我们之前使用配置文件来进行关联的效果是一样的，但是我们使用注解开发只需要及其简单的一行，这可方便太多了。

## WebServlet注解详解

我们以前就学习过，注解本身其实就是一个接口，我们也可以自定义注解，自定义注解比较麻烦，但是注解用起来就很方便。现在我们来看看WebServlet的注解的源码

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface WebServlet {
    String name() default "";//可以指定servlet的名称，等价于<servlet-name>

    String[] value() default {};//可以指定映射Servlet的路径，等效于<url-pattern>，同时可以指定多个路径，实现多映射

    String[] urlPatterns() default {};

    int loadOnStartup() default -1;//指定servlet的创建时机，默认是第一次访问时创建，等效于<load-on-startup>

    WebInitParam[] initParams() default {};//配置初始化参数，等效于<init-param>，当然也可以配置多个参数

    boolean asyncSupported() default false;//是否支持异步

    String smallIcon() default "";

    String largeIcon() default "";

    String description() default "";

    String displayName() default "";
}
```

这些里面的各种属性就相当于是我们当初配置文件里学习的各种配置，不同的是前者要比后者方便太多，具体怎么个对应法上面的代码都有所解释了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3807420ef335a2968c3b69d004e94c56.png)

## 手动创建Servlet容器

这个方法还就突出那个麻烦，我们只做了解就可以了，来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE36128d86d6030eaddeae16f923495120.png)

好了，本节就结束了，有兴趣的自己去看网课复习吧，我反正没兴趣

# 学生管理系统(上)

那么学习了上面这么多内容之后，我们现在先来一个小试牛刀，先做一个学生管理系统的一小部分，后续我们会将我们的系统的内容不断进行补充，这里面当然也会不断地学习新知识，当我们的学生管理系统做完了，我们的知识也就学完了

## 案例效果

那么先来看看我们的案例效果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbf82c5902e2e381aa98c21ba87534e6d.png)

可以看到我们要实现的功能很简单，首先是一个简单的数据输入页面，用户可以在页面里输入对应的数据，然后将数据传到java服务器，并通过IO流的形式传到硬盘中保存起来，并且，当我们的请求成功被响应的时候，会在我们的页面上返回一个成功保存的页面信息

## 具体实现

那么现在我们来正式实现我们的案例效果，我们先来看看我们的实现步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeac6b1e1383138a6980ed4c2d6a5441a.png)

首先我们要创造保存学生信息的html文件，那么我们可以创造其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>保存学生信息</title>
</head>
<body>
    <!--提前通过Run Edit设置其虚拟目录为stu，此处先写入虚拟目录，然后写入构造的类，就可以将数据传入到指定的类-->
    <form action="/stu/studentServlet" method="get" autocomplete="off">
        学生姓名：<input type="text" name="username"> <br/>
        学生年龄：<input type="number" name="age"> <br/>
        学生成绩：<input type="number" name="score"> <br/>
        <button type="submit">保存</button>
    </form>
</body>
</html>
```

接着我们配置web.xml下配置对应的信息

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--修改默认主页-->
    <welcome-file-list>
        <welcome-file>/addStudent.html</welcome-file>
    </welcome-file-list>

    <!--配置Servlet-->
    <servlet>
        <servlet-name>studentServlet</servlet-name>
        <servlet-class>com.itheima.servlet.StudentServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>studentServlet</servlet-name>
        <url-pattern>/studentServlet</url-pattern>
    </servlet-mapping>
</web-app>
```

最后我们在对应的类中写入代码，令其能够将数据写入到硬盘中并返回给我们的页面一个响应 ，我们可以构造代码如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

/*
    基于注解方式开发Servlet
 */
public class StudentServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取表单数据并保存到文件中，由于传入的数据其实就是一个请求，因此数据都保存在请求中，也就是req的对象中，我们只要调用其对应的方法就可以获得我们的所需要的数据了
        String username = req.getParameter("username");
        String age = req.getParameter("age");
        String score = req.getParameter("score");

        //采用字符输出流的方式将数据写入到文件中
        BufferedWriter bw = new BufferedWriter(new FileWriter("/d:\\stu.txt",true));
        bw.write(username+","+age+","+score);
        bw.newLine();
        bw.close();

        //响应客户端浏览器，如果我们采用自己创造一个字符输入流的的方式的话又需要额外创建一个新的文件，比较麻烦。这里直接通过响应方法获得一个打印流，直接在页面上打印我们想要打印的字符
        PrintWriter pw = resp.getWriter();
        pw.println("Save Success~");
        pw.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

具体实现的代码的解释，我们直接以注释的方式添加到代码里了。最后我们通过测试会发现，这个程序没有任何问题。那么我们学生管理系统的第一部分就算是实现完了

