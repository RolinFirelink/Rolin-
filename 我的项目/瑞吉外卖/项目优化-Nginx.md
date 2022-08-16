接着我们来学习Nginx，这个内容虽然我们之前学过了，但是我们不妨再学习一遍，我们先来看看Nginx的介绍

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaaf9c9e161993a9903b05a58919b44d9.png)

然后我们来看看其下载和安装的过程

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEadd69b785cdcf68df216c418e1d8b1c1.png)

安装和完成之后，我们的一个问题就是Nginx有什么用呢？我们的Nginx主要有三个作用，一是部署静态资源，二是反向代理，三是负载均衡，我们接下来我们一一讲解，不过在此之前，我们先来学习下Nginx的目录结构

第一个conf中最重点的内容在于nginx.conf，logs文件夹内部需要我们先运行了nginx之后才会有文件，其他请看图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE42e0e3f723fb50eb640b9e3fd5e00686.png)

这里我们值得一提的是，我们下面的命令都应该进入到我们的nginx中的sbin文件夹中才可以正确执行，否则其会报没有找到对应的文件

首先是查看版本的命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9a1007853698c0fcbf24556e8b7edab5.png)

然后是检查配置文件正确性的命令，我们启动Nginx服务之前都应该启动该命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE276c31a733e71aefe335d3ef40c8babc.png)

接着我们来启动Nginx，使用./nginx命令即可，但是由于Nginx默认使用的是80端口，而80端口在之前我们启动Tomcat的时候用过了，所以我们这里开启会显示端口被占用的问题

我们解决这个问题参考了这个文章https://blog.csdn.net/yufeng_lai/article/details/88819981，其中在该文章重启防火墙中又出现了Failed to start firewalld.service: Unit is masked.的报错，为了解决这个问题我们又参考了这篇文章https://blog.csdn.net/centose/article/details/96975849，最终我们成功开启了Nginx

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9c591c79784bb57e057619b9b0828848.png)

然后我们只要直接用我们的ip地址就可以访问到Nginx的首页了（80端口可以省略不写），注意我们要事先将我们的防火墙关闭，否则是无法访问的

logs文件夹内会存放各种日志文件，包括错误的日志文件和成功的日志文件。当我们的nginx启动时，其还会生成一个nginx.pid文件，内部记录了nginx的进程号

当我们修改了Nginx的配置文件后，我们需要用重新加载配置文件的命令才可以使得我们的配置文件生效

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE55d3ab385ef70d2a2db57c2496e65e96.png)

接着我们来学习Nginx的配置文件nginx.conf的整体结构，其总分为三大块，分别是全局块、events块、http块，其中最重要的就是http块

http块其下又分为两块，分别是http全局块与Server块，其中Server块又分为Server全局块和location块

这里最值得一提的是http块中可以配置多个Server块，每个Server块中可以配置多个location块

更加具体的内容可以看下图，红色框住了三大块，黑色框住了http块的两小块，黄色框住了Server块下的两小块

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4fcced59f07f7093db3214ea8c42410e.png)

那么接着我们就要到我们的具体应用了，终于讲到这里了，我们首先来讲解如何部署我们的静态资源，我们部署静态资源要将我们的静态资源放置到我们的html文件夹中，我们这里放入了一个hello文件，然后我们开启nginx服务，接着我们访问这个网址http://175.178.114.158/hello.html，我们会发现其能够正确访问到该网址，但是为什么这样做我们就可以访问到了呢？这就需要到我们的配置文件中去找答案了

我们看下面的内容，我们可以看到这里其默认监视的是80端口，因为这里设置了80不是，默认的地址就是我们本机的ip地址，也就是localhost，然后我们location则是用于处理我们的请求的块，其可以匹配客户端请求的url，而root，也就是根，根指向html文件夹，其用于指定我们的静态资源的根目录，也就是说，所有的我们的对我们的ip地址的请求都会从html该文件夹中去获取，这也是为什么我们可以访问到我们的静态资源

那为什么我们直接访问ip地址的时候仍然有页面呢？这是因为我们下面的index中配置了默认界面，默认界面可以配置多个，中间用空格隔开即可

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9122b0ae22624779156a96e8a2e3ee45.png)

最后我们来看看总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb55dc29e0ba054aa7e21413a3912413a.png)

接着我们来讲nginx的第二个作用，反向代理。再讲反向代理之前，我们先来理解一下正向代理的概念，所谓正向代理，I其意思就是在客户端和原始服务器之间设置一个代理服务器，用户需要请求代理服务器，然后由代理服务器完成对资源的请求和返回，这个代理服务器一般是在客户端上设置的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5a11e89bf3b8baccbc80d7895cd3953e.png)

而反向代理是位于用户和目标服务器之间的，客户端向反向代理服务器请求，然后反向代理服务器就会跟具体要请求的服务器进行交互，然后完成对用户请求的响应。这样单看起来似乎正向代理和反向代理大差不差，但其实他们是有区别的，最简单的区别是，正向代理一般而言用户是知道代理服务器的存在的，而是设置在客户端的，而反向代理则不需要设置在客户端，用户直接访问反向代理服务器即可，用户不需要知道目标服务器的地址，我们也不用需要在用户端做什么特别的设定

最简单粗暴理解可以是，正向代理是向用户服务，而反向代理是为服务器服务，前者关联在在客户端上，后者关联在服务器上

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3703a07c6779d771ebe2a2ddc8441e3a.png)

那么我们要如何在Nginx上做反向代理呢？其实很简单，我们只需要直接在对应的配置文件上设置上下面这行代码即可，最重要的标红的代码，这个代码是设置我们的反向代理的服务器的目标地址，设置好了之后我们只要访问反向代理服务器的对应请求地址，我们的反向代理服务器就会接受请求并返回对应的结果，而且在我们的真实的服务器上也会接收到对应的请求

当然，我们不要忘了我们的请求的路径应该是一样的，最多要变化的就是其端口号和ip地址

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5eeadbbd6faeb0f0113f3bbdd0d674dd.png)

最后我们来学习负载均衡，这也是Nginx的最后一个内容，关于负载均衡的内容直接看图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9082630dda6f18735b33f14f1b8cd170.png)

那么我们要如何实现负载均衡呢？其实很简单，我们到负载均衡的服务器上，然后设置如下的内容，我们这里请求的地址直接变成了网址，而这个网址我们上面有设置，通过upstream我们可以在下面定义一组服务器，令其实现访问多个服务器来处理请求的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4bdb757dcde12cc132720f5137ef457c.png)

最后我们的负载均衡同时也有多个策略，具体有啥策略可以自己看 

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE513dac80cedb81cac1872747a518bea5.png)

